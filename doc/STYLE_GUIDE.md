# 风格指南
> # Style Guide

* 文档使用 markdown 文件编写使用小写加中划线分割的方式： `lowercase-with-dashes.md`。
  * 仅当文件要描述的主题包含下划线时，文件名可以使用下划线，例如 `child_process`
  * 一些顶级目录下的 markdown 文件也可能例外

> * Documentation is written in markdown files with names formatted as
>   `lowercase-with-dashes.md`.
>   * Underscores in filenames are allowed only when they are present in the
>     topic the document will describe (e.g., `child_process`).
>   * Some files, such as top-level markdown files, are exceptions.

* 文档应当不超过 80 个字符
* 优先使用 `.editorconfig` 中描述的格式
  * 可以使用一些编辑器[插件][http://editorconfig.org/#download]自动支持这些格式规则

>* Documents should be word-wrapped at 80 characters.
>* The formatting described in `.editorconfig` is preferred.
>  * A [plugin][] is available for some editors to automatically apply these
    rules.

* 诸如拼写和语法问题，应当提前被工具检测出来。如果没有，也应该被其他 review 的人发现
> * Mechanical issues, like spelling and grammar, should be identified by tools,
  insofar as is possible. If not caught by a tool, they should be pointed out by
  human reviewers.

* 优先使用美式英语拼写
> * American English spelling is preferred. "Capitalize" vs. "Capitalise",
  "color" vs. "colour", etc.

* 尽管存在争议，但是 [Oxford comma][] 
* Though controversial, the [Oxford comma][] is preferred for clarity's sake.

* 尽量避免使用人称代词，如我、你、我们
  * 代词可以使用在大部分口语式的文档中，例如入门指南
  * 可以使用中性人称代词和常识性短语
> * Generally avoid personal pronouns in reference documentation ("I", "you",
>   "we").
>   * Pronouns are acceptable in more colloquial documentation, like guides.
>   * Use gender-neutral pronouns and mass nouns. Non-comprehensive
>     examples:
>     * OK: "they", "their", "them", "folks", "people", "developers", "cats"
>     * NOT OK: "his", "hers", "him", "her", "guys", "dudes"

* When combining wrapping elements (parentheses and quotes), terminal
  punctuation should be placed:
  * Inside the wrapping element if the wrapping element contains a complete
    clause — a subject, verb, and an object.
  * Outside of the wrapping element if the wrapping element contains only a
    fragment of a clause.
* Place end-of-sentence punctuation inside wrapping elements — periods go
  inside parentheses and quotes, not after.
* Documents must start with a level-one heading. An example document will be
  linked here eventually.

* 优先使用内联链接`[a link][]` 替代 `[a link][http://example.com]`
> * Prefer affixing links to inlining links — prefer `[a link][]` to
  `[a link](http://example.com)`.

* When documenting APIs, note the version the API was introduced in at
  the end of the section. If an API has been deprecated, also note the first
  version that the API appeared deprecated in.
* When using dashes, use [Em dashes][] ("—" or `Option+Shift+"-"` on macOS)
  surrounded by spaces, as per [The New York Times Manual of Style and Usage][].

* Including assets:
  * If you wish to add an illustration or full program, add it to the
    appropriate sub-directory in the `assets/` dir.
  * Link to it like so: `[Asset](/assets/{subdir}/{filename})` for file-based
    assets, and `![Asset](/assets/{subdir}/{filename})` for image-based assets.
  * For illustrations, prefer SVG to other assets. When SVG is not feasible,
    please keep a close eye on the filesize of the asset you're introducing.

* 代码块
  * 标明语言："```js"
  * 代码不需要完整可执行，只要能够说明你的意图即可，如果需要一个完整的可执行代码，可以将其放在 `assets/code-examples` 中
> * For code blocks:
>   * Use language aware fences. ("```js")
>   * Code need not be complete — treat code blocks as an illustration or aid to
>     your point, not as complete running programs. If a complete running program
>     is necessary, include it as an asset in `assets/code-examples` and link to
>     it.

* When using underscores, asterisks, and backticks, please use proper escaping
  (`\_`, `\*` and ``\` `` instead of `_`, `*` and `` ` ``).

* 引用构造函数使用大写开头的驼峰命名
* 引用实例使用小写开头的驼峰命名
* 引用方法必须包含括号：`socket.end()`

> * References to constructor functions should use PascalCase.
> * References to constructor instances should use camelCase.
> * References to methods should be used with parentheses: for example,
>   `socket.end()` instead of `socket.end`.

* To draw special attention to a note, adhere to the following guidelines:
  * Make the "Note:" label italic, i.e. `*Note*:`.
  * Use a capital letter after the "Note:" label.
  * Preferably, make the note a new paragraph for better visual distinction.
* Function arguments or object properties should use the following format:
  * <code>* \`name\` {type|type2} Optional description. \*\*Default:\*\* \`defaultValue\`</code>
  * E.g. <code>* `byteOffset` {integer} Index of first byte to expose. **Default:** `0`</code>
  * The `type` should refer to a Node.js type or a [JavaScript type][]
* Function returns should use the following format:
  * <code>* Returns: {type|type2} Optional description.</code>
  * E.g. <code>* Returns: {AsyncHook} A reference to `asyncHook`.</code>

[Em dashes]: https://en.wikipedia.org/wiki/Dash#Em_dash
[Javascript type]: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Grammar_and_types#Data_structures_and_types
[Oxford comma]: https://en.wikipedia.org/wiki/Serial_comma
[The New York Times Manual of Style and Usage]: https://en.wikipedia.org/wiki/The_New_York_Times_Manual_of_Style_and_Usage
[plugin]: http://editorconfig.org/#download
