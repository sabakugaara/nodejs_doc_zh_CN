# How to write a test for the Node.js project

## What is a test?

Most tests in Node.js core are JavaScript programs that exercise a functionality
provided by Node.js and check that it behaves as expected. Tests should exit
with code `0` on success. A test will fail if:

> node.js 中的大部分测试代码都是使用 Javascript 编写：若测试代码正常退出，则认为测试通过。以下两种情况被视作测试失败：

- It exits by setting `process.exitCode` to a non-zero number.
  - This is usually done by having an assertion throw an uncaught Error.
  - Occasionally, using `process.exit(code)` may be appropriate.
- It never exits. In this case, the test runner will terminate the test because
  it sets a maximum time limit.

> - 进程退出时，`process.exitCode` 的值非 0
>   - 一般情况是代码抛出异常，退出码会被自动修改为非 0 值
>   - 也可能会直接通过 `process.exit(code)` 修改退出码
> - 进程永不退出，测试进程会被强制退出，因为每个测试都有超时设置

Add tests when:

> 哪些情况需要添加测试用例？

- Adding new functionality.
- Fixing regressions and bugs.
- Expanding test coverage.

> - 添加新功能时
> - 修复 bug 时
> - 提升测试覆盖率

## Test structure

Let's analyze this basic test from the Node.js test suite:

> 下面是一个基本的 node.js 项目测试代码：

```javascript
'use strict';                                                          // 1
const common = require('../common');                                   // 2

// This test ensures that the http-parser can handle UTF-8 characters  // 4
// in the http header.                                                 // 5

const assert = require('assert');                                      // 7
const http = require('http');                                          // 8

const server = http.createServer(common.mustCall((req, res) => {       // 10
  res.end('ok');                                                       // 11
}));                                                                   // 12
server.listen(0, () => {                                               // 13
  http.get({                                                           // 14
    port: server.address().port,                                       // 15
    headers: { 'Test': 'Düsseldorf' }                                  // 16
  }, common.mustCall((res) => {                                        // 17
    assert.strictEqual(res.statusCode, 200);                           // 18
    server.close();                                                    // 19
  }));                                                                 // 20
});                                                                    // 21
```

### **Lines 1-2**

```javascript
'use strict';
const common = require('../common');
```

The first line enables strict mode. All tests should be in strict mode unless
the nature of the test requires that the test run without it.

> 第一行代码开启严格模式，所有的测试代码都必须在严格模式下运行，
> 除非被测试的特性本身需要在非严格模式下运行。

The second line loads the `common` module. The [`common` module][] is a helper
module that provides useful tools for the tests.

> 第二行加载 `commom` 模块，`test/common` 模块提供了很多有用的辅助函数，用来帮助编写测试

Even if a test uses no functions or other properties exported by `common`,
the test should still include the `common` module before any other modules. This
is because the `common` module includes code that will cause a test to fail if
the test leaks variables into the global space. In situations where a test uses
no functions or other properties exported by `common`, include it without
assigning it to an identifier:

> 即使一个测试用例没有使用到任何 `common` 模块中的内容，仍然需要在加载其他模块前加载该模块。
> 这是因为 `common` 模块中包含了检测代码：如果一个测试用例定义了一个全局变量，检测代码会使该测试执行失败。
> 当测试用例没有使用任何 `common` 模块中的内容时，直接 `require`，不赋值给任何变量即可：

```javascript
require('../common');
```

### **Lines 4-5**

```javascript
// This test ensures that the http-parser can handle UTF-8 characters
// in the http header.
```

A test should start with a comment containing a brief description of what it is
designed to test.

> 测试代码的开头应当包含一段简短备注，用来说明该测试的测试目标。

### **Lines 7-8**

```javascript
const assert = require('assert');
const http = require('http');
```

The test checks functionality in the `http` module.
> 示例代码是测试 `http` 模块的功能，所以第八行需要加载该模块

Most tests use the `assert` module to confirm expectations of the test.

> 第 7 行加载了 `assert` 断言库，大部分测试用例使用该库来检查测试结果

The require statements are sorted in
[ASCII][] order (digits, upper
case, `_`, lower case).

> require 语句应当按照 `ASCII` 码表排序：数字、大写字母、下划线、小写

### **Lines 10-21**

This is the body of the test. This test is simple, it just tests that an
HTTP server accepts `non-ASCII` characters in the headers of an incoming
request. Interesting things to notice:

> 10-21 行部分是测试用例的主要部分。代码很简单，只需要测试一个 http 服务器可以接收请求头中包含的非 `ASCII` 码表中的字符。这段代码中有以下几点需要注意：

- If the test doesn't depend on a specific port number, then always use 0
  instead of an arbitrary value, as it allows tests to run in parallel safely,
  as the operating system will assign a random port. If the test requires a
  specific port, for example if the test checks that assigning a specific port
  works as expected, then it is ok to assign a specific port number.
- The use of `common.mustCall` to check that some callbacks/listeners are
  called.
- The HTTP server closes once all the checks have run. This way, the test can
  exit gracefully. Remember that for a test to succeed, it must exit with a
  status code of 0.

> - 如果测试不依赖于特定的端口号，那么每次监听是应该使用随机端口号(`listen(0)`)。这样并行运行测试更加安全，不会端口冲突。除非测试代码就是为了测试需要监听一个指定的端口号的情况，那么可以在测试中可以指定监听的端口号
> - 使用了 `common.mustCall` 确保一些回调函数一定被调用
> - 一旦所有的测试断言运行通过，HTTP 服务器必须关闭退出。这样才能让测试进程优雅的自动退出，退出状态码也会被自动设置为 0，必须记住这一点。否则非 0 的退出码，会导致测试失败



## General recommendations

### Timers

Avoid timers unless the test is specifically testing timers. There are multiple
reasons for this. Mainly, they are a source of flakiness. For a thorough
explanation go [here](https://github.com/nodejs/testing/issues/27).

> 尽量避免使用定时器，除非要测试定时器。之所以这么建议，有很多原因。更多的讨论见：[nodejs/testing#27](https://github.com/nodejs/testing/issues/27)

In the event a test needs a timer, consider using the
`common.platformTimeout()` method. It allows setting specific timeouts
depending on the platform. For example:

```javascript
const timer = setTimeout(fail, common.platformTimeout(4000));
```

will create a 4-second timeout on most platforms but a longer timeout on slower
platforms.

### The *common* API

Make use of the helpers from the `common` module as much as possible.

One interesting case is `common.mustCall`. The use of `common.mustCall` may
avoid the use of extra variables and the corresponding assertions. Let's explain
this with a real test from the test suite.

```javascript
'use strict';
require('../common');
const assert = require('assert');
const http = require('http');

let request = 0;
let response = 0;
process.on('exit', function() {
  assert.equal(request, 1, 'http server "request" callback was not called');
  assert.equal(response, 1, 'http request "response" callback was not called');
});

const server = http.createServer(function(req, res) {
  request++;
  res.end();
}).listen(0, function() {
  const options = {
    agent: null,
    port: this.address().port
  };
  http.get(options, function(res) {
    response++;
    res.resume();
    server.close();
  });
});
```

This test could be greatly simplified by using `common.mustCall` like this:

```javascript
'use strict';
const common = require('../common');
const http = require('http');

const server = http.createServer(common.mustCall(function(req, res) {
  res.end();
})).listen(0, function() {
  const options = {
    agent: null,
    port: this.address().port
  };
  http.get(options, common.mustCall(function(res) {
    res.resume();
    server.close();
  }));
});

```

### Flags

Some tests will require running Node.js with specific command line flags set. To
accomplish this, add a `// Flags: ` comment in the preamble of the
test followed by the flags. For example, to allow a test to require some of the
`internal/*` modules, add the `--expose-internals` flag.
A test that would require `internal/freelist` could start like this:

```javascript
'use strict';

// Flags: --expose-internals

require('../common');
const assert = require('assert');
const freelist = require('internal/freelist');
```

### Assertions

When writing assertions, prefer the strict versions:

* `assert.strictEqual()` over `assert.equal()`
* `assert.deepStrictEqual()` over `assert.deepEqual()`

When using `assert.throws()`, if possible, provide the full error message:

```js
assert.throws(
  () => {
    throw new Error('Wrong value');
  },
  /^Error: Wrong value$/ // Instead of something like /Wrong value/
);
```

### ES.Next features

For performance considerations, we only use a selected subset of ES.Next
features in JavaScript code in the `lib` directory. However, when writing
tests, for the ease of backporting, it is encouraged to use those ES.Next
features that can be used directly without a flag in
[all maintained branches][]. [node.green][] lists available features
in each release.

For example:

* `let` and `const` over `var`
* Template literals over string concatenation
* Arrow functions when appropriate

## Naming Test Files

Test files are named using kebab casing. The first component of the name is
`test`. The second is the module or subsystem being tested. The third is usually
the method or event name being tested. Subsequent components of the name add
more information about what is being tested.

For example, a test for the `beforeExit` event on the `process` object might be
named `test-process-before-exit.js`. If the test specifically checked that arrow
functions worked correctly with the `beforeExit` event, then it might be named
`test-process-before-exit-arrow-functions.js`.

## Imported Tests

### Web Platform Tests

Some of the tests for the WHATWG URL implementation (named
`test-whatwg-url-*.js`) are imported from the [Web Platform Tests Project][].
These imported tests will be wrapped like this:

```js
/* The following tests are copied from WPT. Modifications to them should be
   upstreamed first. Refs:
   https://github.com/w3c/web-platform-tests/blob/8791bed/url/urlsearchparams-stringifier.html
   License: http://www.w3.org/Consortium/Legal/2008/04-testsuite-copyright.html
*/
/* eslint-disable */

// Test code

/* eslint-enable */
```

To improve tests that have been imported this way, please send
a PR to the upstream project first. When the proposed change is merged in
the upstream project, send another PR here to update Node.js accordingly.
Be sure to update the hash in the URL following `WPT Refs:`.

## C++ Unit test
C++ code can be tested using [Google Test][]. Most features in Node.js can be
tested using the methods described previously in this document. But there are
cases where these might not be enough, for example writing code for Node.js
that will only be called when Node.js is embedded.

### Adding a new test
The unit test should be placed in `test/cctest` and be named with the prefix
`test` followed by the name of unit being tested. For example, the code below
would be placed in `test/cctest/test_env.cc`:

```c++
#include "gtest/gtest.h"
#include "node_test_fixture.h"
#include "env.h"
#include "node.h"
#include "v8.h"

static bool called_cb = false;
static void at_exit_callback(void* arg);

class EnvTest : public NodeTestFixture { };

TEST_F(EnvTest, RunAtExit) {
  v8::HandleScope handle_scope(isolate_);
  v8::Local<v8::Context> context = v8::Context::New(isolate_);
  node::IsolateData* isolateData = node::CreateIsolateData(isolate_, uv_default_loop());
  Argv argv{"node", "-e", ";"};
  auto env = Environment:CreateEnvironment(isolateData, context, 1, *argv, 2, *argv);
  node::AtExit(at_exit_callback);
  node::RunAtExit(env);
  EXPECT_TRUE(called_cb);
}

static void at_exit_callback(void* arg) {
  called_cb = true;
}
```

Next add the test to the `sources` in the `cctest` target in node.gyp:
```console
'sources': [
  'test/cctest/test_env.cc',
  ...
],
```
The test can be executed by running the `cctest` target:
```console
$ make cctest
```

### Node test fixture
There is a [test fixture][] named `node_test_fixture.h` which can be included by
unit tests. The fixture takes care of setting up the Node.js environment
and tearing it down after the tests have finished.

It also contains a helper to create arguments to be passed into Node.js. It
will depend on what is being tested if this is required or not.

[ASCII]: http://man7.org/linux/man-pages/man7/ascii.7.html
[Google Test]: https://github.com/google/googletest
[Web Platform Tests Project]: https://github.com/w3c/web-platform-tests/tree/master/url
[`common` module]: https://github.com/nodejs/node/blob/master/test/common/README.md
[all maintained branches]: https://github.com/nodejs/lts
[node.green]: http://node.green/
[test fixture]: https://github.com/google/googletest/blob/master/googletest/docs/Primer.md#test-fixtures-using-the-same-data-configuration-for-multiple-tests
