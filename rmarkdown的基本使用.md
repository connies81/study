#rmarkdown的基本使用
交互式文档是一种创建Shiny apps的新途径。交互式文档是一种包含Shiny控件与输出的 R Markdown文件， 你可以再 markdown中写报告，并且作为app来启动它。


本文主要阐述如何使用R Markdown写报告。

##R Markdown

R Markdown是通过R语言制作动态文档的文件格式。R Markdown文档再markdown中完成，其中包含嵌入的R代码，如下图：

 
\---<br>
 title: R Markdown <br>
 output: html\_document<br>
\---<br>
This is an R Markdown document. Markdown is a simple formatting syntax which allows you to author HTML, PDF, and MS Word documents. For more details on how to use R Markdown, see <http://rmarkdown.rstudio.com>.<br>
When you click the **Knit** button a document will be generated that includes both content as well as the output of any embedded R code chunks within the document. You can embed an R code chunk like this:<br>
 \`\`\`
{r} <br>
summary(cars)<br>
\`\`\` <br>
You can also embed plots:<br>
 \`\`\`{r, echo=FALSE}<br>
plot(cars)<br>
\```<br>
Note that the `echo = FALSE` parameter was added to the code chunk to prevent printing of the R code that generated the plot.
R Markdown文档编辑需要 rmarkdown包，rmarkdown安装需要RStudio编辑器环境，但是你可以以github途径来下载rmarkdown，并安装。<br>
devtools:install_github("rmarkdown", "rstudio")<br>
R Markdown是资源代码丰富并高可用的文件，你可以将通过一下两种方式改变R Markdown文件格式。

1、knit -  knit 文件. rmarkdown包调用knitr包， knitr 将运行所有的R代码，并将得到的结果追加到代码之后，这种工作方式非常节省实践并且报告也可复用。

传统的，作者制作包含图形的报告。作者需要制作图形，保存为文件，然后copy并粘贴到最终的报告中，这个工作严重依赖人力，如果数据有变更，那么作者需要重复整个过程来更新图形。

现在，再R Markdown的模板中，每个报告都包含制作图形，表格，数字所需的代码，作者可通过knit来自动完成更新。

2、convert - convert 文件。rmarkdown包借助pandoc来将文件转变成新的格式，例如，你可以转变Rmd文件成HTML, PDF, 或Microsoft Word文件，你甚至可以转变成 HTML5 或 PDF 幻灯片，rmarkdown保持文本，代码结果与Rmd文件中的结构一致。

在实际应用中，作者经常同时用knit 和 convert文档。在本文，将用render 命令来对R markdwon文件执行 knitting 和 converting过程。

你可以认为的用render作用到R Markdown 文件， 如： rmarkdown::render(). 上面的代码渲染成HTML文件格式。

rmarkdown::render()，R markdown与RStudio深度合作 integrated into the RStudio IDE，因为，你可以通过按键来完成以上命令。
##基本参数

为了创建 R Markdown报告，打开text文件，并将它保存为.Rmd 文件。
R Markdown 报告由一下3部分组成：<br>

 * text文本<br>
 * knitr 处理 R code<br>
 * YAML的渲染参数<br>
 
本文将逐一介绍：

###处理文本

.Rmd 文件包含text，Markdown是一种处理普通格式文本的公约，包括一下特征： 

加粗和斜体文本<br>
列表<br>
title<br>
超链接<br>
更多<br>
这个协议虽然很朴素，但是，制作的文本非常易读.

#### Say Hello to markdown

Markdown is an **easy to use** format for writing reports. It resembles what you naturally write every time you compose an email. In fact, you may have already used markdown *without realizing it*. These websites all rely on markdown formatting

* [Github](www.github.com)
* [StackOverflow](www.stackoverflow.com)
* [Reddit](www.reddit.com)

编写过程中展示了如何使用 markdown:<br>
**headers** - 使用一个或者多个 # 在文本的开始阶段，例如： #### Say Hello to markdown. 单个#意味着文本是一级标题，两个#代表二级标题，以此类推.

**斜体和加粗字体** - 文本两侧加一个星号得到斜体字体，例如上文中：*without realizing it* . 用双星号包围文本得到加粗字体, 例如：**easy to use**.

**lists** - 每个要点之前用星号，正文与要点之间留空行<br>
  This is a list

  * item 1
  * item 2
  * item 3
  
This is a list

item 1<br>
item 2<br>
item 3<br>
**hyperlinks** - 1.用中括号包围网站名称，2.用括号包围具体链接，然后连接在一起使用，例如：

[Github](Build software better, together).

你可以查看更多的Mardown 操作指导：<br>
Markdown Quick Reference guide<br>
1.open a .md or .Rmd file in RStudio. <br>
2.打开"帮助"<br>
3.选择 “Markdown Quick Reference”<br>
4.在帮助面板即可查看<br>

###渲染

为了将markdown文件转化为HTML, PDF, 或 Word document，单机编辑面板的工具“Knit” 控件，出现下拉菜单，选择你要的转化文件类型。当你选定格式后， rmarkdown 将把你的文本转化成新格式文件。rmarkdown能够采用markdown语法的文件变更格式。 一旦文件被渲染，RStudio将预览目标格式结果，并保存在工作目录中。<br>
Note: RStudio不能直接转化PDF和word，需要装其他软件。

###knitr嵌入R代码

knitr包 能够兼容markdown语法，尤其包含执行R代码的能力。

渲染报告的过程中， knitr 将执行代码并将输出的结果展示。可以选择性的展示：之展示代码，只展示结果，代码与结果同时展示。

想嵌入R代码在报告中，用两行\`\`\` 将代码包围。在第一个\`\`\` {r}, 用于通知knitr下面的将是R代码，具体模板如下图：

Here's some code<br>
\`\`\` {r}<br>
dim(iris)<br>
\`\`\` <br>
在渲染文档的时候， knitr将运行代码并将结果追加在代码之后，knitr提供格式和语法高亮展示R代码和代码运行结果。该结果显示所有的代码和结果。

如果不想将结果results 追加到报告中，可以将 **eval = FALSE** 参数加入大括号中，这样做的结果就是**只把代码放入报告中，而不执行**。

**只将结果放入报告中，参数echo = FALSE 而不显示代码**：

参数echo 和 eval不仅仅用于自定义code， 你可以通过 rmarkdown 和 knitr 进行学习。

###行间代码

嵌入R代码到文本当中，在代码的两侧用点’来包围，如下图：


Two plus two equals\`r 2 + 2\`.
knitr 将用代码结果代替Ｒ代码，显示**Two plus two equals 4**。

###YAML 渲染参数

 YAML header将决定如何展现你的 .Rmd file.文件，用两个 --- 包围，如下：

\---
title: "Untitled"<br>
author: "Garrett"<br>
date: "July 10, 2014"<br>
output: html_document<br>
\---

Some inline R code, \`r 2 + 2\`.<br>
output: 决定最后的文件类型。output: 选择其中一种类型的文件类型:<br>
html_document, <br>
pdf_document,<br>
word_document,<br>
 RStudio IDE knit更加方便的进行设置。

####幻灯片：

可以将文档转换为幻灯片：

参数设定output: ioslides\_presentation 创建ioslides (HTML5)幻灯片<br>
参数设定output: beamer\_presentation 创建 a beamer (PDF) 幻灯片<br>
Note: 默认情况下RStudio编辑器中knitr没有默认选项，先在命令中修改输出类型，RStudio会输出类型加入默认选项菜单。

具体参数详见rmakdown.rstudio.com<br>
[R markdown 简介](https://zhuanlan.zhihu.com/p/24884324)
