编辑器配置
---

变更编辑器设定，更换主题，或者安装插件都可以通过快捷键 `cmd-,` 打开 Atom 配置页面。

### 切换主题

Atom 自带两套主题和语法主题。当然我们鼓励大家编写更适合自己的主题。

更换或者激活想要使用的主题也可以通过快捷键打开 Atom 配置页面，选择左侧的主题面板，即可看到相关操作说明。在这个面板，不仅可以管理现有的主题，
还可以在主题市场搜索可用的主题并一键安装到本地。

### 安装插件

配置页面左侧的插件面板也可以快速安装新插件。在这里显示的插件都是注册并发布到 Atom.io 插件市场的模块。
除了使用管理面板管理和安装插件，你也可以使用 `apm` 命令行工具，使用命令行工具之前，确认是否安装了 `apm`：

```
apm help install
```

如果你看到了一行关于 apm install 操作的帮助提示，说明你已经成功了安装了 apm。
如果你并没有看到有效输出，那么打开 Atom ，在顶部左侧的 Atom 菜单中寻找到 Install Shell Commmands 菜单，点击安装 apm

使用 apm 按照如下的方式安装插件：

- apm install <package_name> 安装某个插件的最新版本
- apm install <package_name>@<package_version> 安装某个插件的指定版本

比如，我们需要安装 emmet 插件的 0.1.5 版本，则需要运行命令 `apm install emmet@0.1.5`，插件会被安装到这个地址 `~/.atom/packages`。

还有其他的命令帮助你使用 apm 搜索符合某个关键词的插件或者查询某个插件的详细信息：

- apm search coffee 搜索与 coffee 关键词相关的插件
- apm view emmet 查询 emmet 插件的详细信息

### 定义快捷键

Atom 快捷键映射表看起来就像样式表。这意味着编写快捷键映射也需要有一个选择符，以匹配相应的事件、行为或者指定上下文。
这里有一个简单的快捷键映射例子：

```
'.editor':
  'enter': 'editor:newline'

'.mini.editor input':
  'enter': 'core:confirm'
```

这个快捷键 map 定义了 回车 (enter) 在两种不同场景下的行为。在普遍的模式下，回车触发一个 换行 (editor:newline) 事件，导致编辑器开启新的一行。但如果在已选择一个列表的时候触发回车，编辑器就会触发 core:confirm 事件。(But if the same keystroke occurs inside of a select list's mini-editor, it instead emits the core:confirm event based on the binding in the more-specific selector.)

默认的快捷键映射表是 `~/.atom/keymap.cson`，这个文件会在 Atom 启动的时候最晚被加载，以确保用户修改这个文件的映射会覆盖 Atom 默认的配置以及第三方插件的快捷键配置。

当然，如果你不想通过其他方式打开，也可以在顶部菜单 `Atom > Open Your Keymap menu` 中直接打开。

打开配置面板的快捷键映射面板，你可以看到所有可用的快捷键，以方便自定义属于自己的快捷键。

### 进阶配置

Atom 的主配置文件在 `~/.atom/config.cson`，CSON 是一种 CoffeeScript-style JSON，看起来像这样：

```
'core':
  'excludeVcsIgnoredPaths': true
'editor':
  'fontSize': 18
```
配置文件由包名称进行分组，当然，也包括两个缺省的类别，那就是 核心配置(core) 和 编辑器(editor) 配置。
也可以在顶部菜单中找到 Atom > Open Your Config 打开配置文件。

### 配置项清单

- core
- editor
- fuzzyFinder
- whitespace
- wrap-guide

### 快速定义指引

#### init.coffee

当 Atom 本身加载完毕时，它会首先运行 `~/.atom/init.coffee` 文件，以便于用户可以快速的进行某些配置项的自定义，而不用修改配置文件。
并且，这个文件有访问所有 Atom API 的权限。如果你在这个文件里编写了太多的配置，我们还是建议您为这些配置单独新建一个模块。

也可以通过访问菜单 Atom > Open Your Init Script 打开这个文件。

举个例子，如果我们希望在 Atom 加载完毕后「哔」一声提示用户，我们可以在 `~/.atom/init.coffee` 加入这段代码：

```
atom.beep()
```
另外，如果你不喜欢编写 CoffeeScript，也可以将这个配置文件命名为 init.js 并编写原生 JavaScript 代码。

#### styles.less

同样的，如果你想要编写一些简单而粗暴的自定义样式表，而非新建一整个主题，也可以通过编辑配置文件 `~/.atom/styles.less` 来完成。

也可以通过访问菜单 Atom > Open Your Stylesheet 打开这个文件

举个例子，如果需要改变光标的颜色，你可以在 ~/.atom/styles.less 文件中加入这行代码：

```
.editor .cursor {
  border-color: pink;
}
```

如果你不熟悉 LESS，可以在[这里](#)查阅到更多关于它的信息。

另外，如果你不喜欢编写 LESS，也可以将这个配置文件命名为 styles.css 并编写 css。
