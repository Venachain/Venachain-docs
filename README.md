# 使用说明

## 1. mkdocs 安装

安装链接<https://www.mkdocs.org/#installation>

## 2. 主题选择

编辑mkdocs.yml中theme字段可以更换主题

`mkdocs`是自带的主题，但是不是很好看

这里推荐主题，下载地址：

1. `windmill`主题<https://github.com/gristlabs/mkdocs-windmill>
2. `mkdocs-material`主题<https://github.com/squidfunk/mkdocs-material>

## 3. 文档渲染显示问题

相关代码已经写入mkdocs.yml，这里只需要下载相应插件即可。

### 3.1. 代码显示问题

手动修复：[stackoverflow上相同问题](<https://stackoverflow.com/questions/18088955/markdown-continue-numbered-list>)

* 在列表中嵌入代码块，需要多缩进4个空格。

例：假如正常情况只需要缩进一次，那么为了在mkdocs中正常显示，则需要缩进两次。

插件修复：[详见python-markdown docs](<https://python-markdown.github.io/extensions/fenced_code_blocks/#syntax>)

* **安装pymdown extensions**:
    1. 确保mkdocs, pymdown这些python的插件都是用pip安装（而不是apt），否则可能出现mkdocs无法加载插件的问题
    2. 安装指令：`pip install pymdown-extensions==<version>`，7.1版本可能无法正常安装，这里推荐使用6.0

### 3.2. Latex数学公示显示问题

通过添加插件和JS文件解决：

* 解决方案:<https://github.com/mkdocs/mkdocs/issues/253>
* 插件下载：`pip install python-markdown-math`

说明：也可以使用`pymdownx.arithmatex`插件解决该问题，相关指南请参考<https://squidfunk.github.io/mkdocs-material/extensions/pymdown/>   

### 3.3 其他第三方插件下载

<https://github.com/Python-Markdown/markdown/wiki/Third-Party-Extensions#math--latex>

## 4. 部署文档到GitHub

具体部署操作指南链接：<https://www.mkdocs.org/user-guide/deploying-your-docs/>

* mkdocs全量文档位置：<https://git-c.i.wxblockchain.com/PlatONE/doc/platone-docs>
* 部署在GitHub上对应的分支：<https://github.com/PlatONEnterprise/platonenterprise.github.io>
* PlatONE技术文档URL：<https://platonenterprise.github.io/#>
