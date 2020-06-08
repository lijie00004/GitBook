---
description: This is a short description of my page
---

# 安装和设置

* 安装好 `Node.js` 和 `NPM`
* `npm install gitbook-cli -g`

* `gitbook init`

* `gitbook init ./directory`指定目录创建一本书

* `gitbook serve`
* `gitbook build`
* `gitbook fetch 4.0.0-alpha.1` 安装不同版本

* `gitbook ls-remote` 列出可安装的远程版本

* `gitbook build ./ --log=debug --debug` 调试
* `gitbook serve ./ --log=debug --debug` 调试

# 语法

* `[title](folder/readme.md#writing)` `#` 号能跳到指定标题

* `---` 分割线

* `#` 标题

* `*斜体*`

* `**粗体**`

* `~~删除线~~`

* `[链接](http://www.gibook.site/)没有标题属性`

* `这是[一个示例][id]参考样式链接`

* `[id]:http://www.gibook.site/ "可选标题这里"`

* `图片:![这是图片](../images/logo_200.png)`

* ```markdown
  Kanye West说：-- 引用块
  > 我们生活在未来
  > 现在是我们的过去。
  ```

* ```markdown
  |第一标题|第二标题|
  | ----- | ----- |
  |内容单元|内容单元|
  ```


# book.json

| 变量          | 说明                                                         |
| ------------- | ------------------------------------------------------------ |
| `root`        | 包含所有图书文件的根文件夹的路径，除了`book.json`            |
| `structure`   | 指定自述，摘要，词汇表等的路径。请参见[结构段落](http://gitbook.hushuang.me/＃结构)。 |
| `title`       | 书的标题，默认值从README中提取。在GitBook.com上，此字段已预填。 |
| `description` | 您的图书说明，默认值从自述文件中提取。在GitBook.com上，此字段已预填。 |
| `author`      | 作者姓名。在GitBook.com上，此字段已预填。                    |
| `isbn`        | 书的国际码ISBN                                               |
| `language`    | [语言ISO规范](https://en.wikipedia.org/wiki/List_of_ISO_639-1_codes)的书的语言，默认值是`en` |
| `direction`   | 文本的方向。可以是`rtl`或`ltr`，默认值取决于`language` 的值  |
| `gitbook`     | GitBook的版本。使用[SemVer](http://semver.org/)规范并接受诸如`“> = 3.0.0”`的条件 |
| `plugins`       | 要加载的插件列表 |
| `pluginsConfig` | 插件配置         |
| `structure.readme`    | 自述文件名(默认为“README.md”)     |
| `structure.summary`   | 摘要文件名(默认为“SUMMARY.md”)    |
| `structure.glossary`  | 词汇表文件名(默认为“GLOSSARY.md”) |
| `structure.languages` | 语言文件名(默认为`LANGS.md`)      |
| `pdf.pageNumbers`   | 将页码添加到每个页面的底部(默认为“true”)                     |
| `pdf.fontSize`      | 基本字体大小(默认为`12`)                                     |
| `pdf.fontFamily`    | 基本字体系列(默认为`Arial`)                                  |
| `pdf.paperSize`     | 纸张大小，选项为“a0”，“a1”，“a2”，“a3”，“a4”，“a5”，“a6”，“b0”，“b1”，“b2”，“b3” 'b4'，'b5'，'b6'，'legal'，'letter''(默认为'a4') |
| `pdf.margin.top`    | 顶部边距(默认为`56`)                                         |
| `pdf.margin.bottom` | 底边距(默认为`56`)                                           |
| `pdf.margin.right`  | 右边距(默认为“62”)                                           |
| `pdf.margin.left`   | 左边距(默认为“62”)                                           |

# 插件

* `"page-treeview"`
  * `D:\Git\GitBook\node_modules\gitbook-plugin-page-treeview\lib\index.js`
  * 删除 `copyRight +`
  * `	return renderContent ? `<div class="treeview__container">${copyRight + renderContent}</div>` : '';`

* 

脚注参考前的文本。[^2]

[^2]:评论要包括在脚注中。