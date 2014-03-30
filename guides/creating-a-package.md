编写插件
---

插件系统是 Atom 的核心组成部分，几乎所有不属于编辑器内核的功能，在 Atom 中，我们都使用插件的方式来进行组织。这些我们默认提供的插件，包括文件树，状态栏，语法高亮等等模块。

插件可以包括很多种改变编辑器行为的资源，一个基本的插件结构应如下所示：

```
my-package/
  grammars/
  keymaps/
  lib/
  menus/
  spec/
  snippets/
  stylesheets/
  index.coffee
  package.json
```

当然，并不是所有的插件都包括（或者需要）这些目录和文件。

Atom 提供了一个 [简单的教程](#) 引导你创建自己的第一个插件。

这里也有一些其他的教程关于如何转换 TextMate 插件和主题到 Atom 平台。

### package.json

和 npm 模块相似， Atom 插件在根目录也包括一个主描述文件 `package.json`。这个文件定义了插件的元数据，比如主文件路径，模块依赖，或者插件中资源文件的加载顺序等等。

除了 npm 模块描述中定义的配置项之外，Atom 还提供这些配置项：

- main (必要): 插件的入口文件，即对外暴露接口的文件
- stylesheets (可选): 一个定义了样式表先后加载顺序的文件数组，如果没有填写这个配置项，Atom 会默认加载 stylesheets 文件夹中的样式表，并按照字母表排序。
- keymaps(可选): 一个定义了快捷键映射先后加载顺序的文件数组，如果没有填写这个配置项，Atom 会默认加载 keymaps 文件夹中的快捷键映射，并按照字母表排序。
- menus(可选): 一个定义了菜单映射先后加载顺序的文件数组，如果没有填写这个配置项，Atom 会默认加载 menus 文件夹中的菜单映射，并按照字母表排序。
- snippets (可选): 一个定义了代码片段先后加载顺序的文件数组，如果没有填写这个配置项，Atom 会默认加载 snippets 文件夹中的代码片段，并按照字母表排序。
- activationEvents (可选): 一个激活插件的事件数组。你可以在这里定义何时你的插件被激活或者触发。

### 插件源码

如果你想定义 Atom 的行为，插件的主入口文件必须包括并暴露一个顶级模块。注意，插件的主入口文件可能是 index.coffee，也可以是在 package.json 中定义的 main 文件，我们建议把插件的源码放置在插件的 lib 目录，并在主入口文件引用。

插件的顶级模块是一个单体对象 (singleton object)，这个对象管理插件的生命周期，也就是说，甚至你的插件生成了十个不同的视图，并将他们 append 到 DOM 的不用部分，这些试图也是由这个对象进行管理。

这个对象必须包括这些方法：

- activate(state): 这个必要的方法会在插件激活时被触发。Atom 会将上次编辑器初始化并执行 serialize() 后的状态传入这个函数。这个函数适合进行某些初始化操作，比如初始化 DOM 元素或者绑定插件中的事件。

- serialize(): 这个可选的方法在编辑器关闭时被执行，如果它返回一个 JSON 表述此时模块的状态，在编辑器下次打开时，这些数据会被传入该插件的 activate 函数中，使得插件重新返回当时的状态或视图变得可能。

- deactivate(): 这个可选的方法在编辑器窗口关闭时被执行。如果你的插件在处理某些外部资源或者监听文件的变动，应当在此时销毁这些函数或者监视器。
如果只是对当前编辑床可起作用，则无需进行处理，Atom 将自动回收

### 范例插件

范例插件的文件结构可能如下所示：

```
my-package/
  package.json
  index.coffee
  lib/
    my-package.coffee
```
index.coffee 可能像这样：

```
module.exports = require "./lib/my-package"
```
my-package/my-package.coffee 可能是这样：

```
module.exports =
  activate: (state) -> # ...
  deactivate: -> # ...
  serialize: -> # ...
```

除了插件的基本结构，插件代码都有使用 Atom API 的权限，开发者需要注意，Atom 的 API 还在早期开发阶段，这意味着 API 可能有计划中的更新，比如区分公共 API 和 私有 API 等等。如果你需要一个现在暂时没有的 API 用于开发，欢迎给 Atom 贡献代码。我们的目标是建立一套开发者真正需要的 API 的体系。

### 插件样式表

Stylesheets for your package should be placed in the stylesheets directory. Any stylesheets in this directory will be loaded and attached to the DOM when your package is activated. Stylesheets can be written as CSS or LESS.

Ideally, you won't need much in the way of styling. We've provided a standard set of components which define both the colors and UI elements for any package that fits into Atom seamlessly. You can view all of Atom's UI components by opening the styleguide: open the command palette (cmd-shift-P) and search for styleguide, or just type cmd-ctrl-G.

If you do need special styling, try to keep only structural styles in the package stylesheets. If you must specify colors and sizing, these should be taken from the active theme's ui-variables.less. For more information, see the theme variables docs. If you follow this guideline, your package will look good out of the box with any theme!

An optional stylesheets array in your package.json can list the stylesheets by name to specify a loading order; otherwise, stylesheets are loaded alphabetically.

### 插件快捷键

It's recommended that you provide key bindings for commonly used actions for your extension, especially if you're also adding a new command:

```
'.tree-view-scroller':
  'ctrl-V': 'changer:magic'
```
Keymaps are placed in the keymaps subdirectory. By default, all keymaps are loaded in alphabetical order. An optional keymaps array in your package.json can specify which keymaps to load and in what order.

Keybindings are executed by determining which element the keypress occurred on. In the example above, changer:magic command is executed when pressing ctrl-V on the .tree-view-scroller element.

See the main keymaps documentation for more detailed information on how keymaps work.

### 插件菜单

Menus are placed in the menus subdirectory. By default, all menus are loaded in alphabetical order. An optional menus array in your package.json can specify which menus to load and in what order.

#### 应用菜单

It's recommended that you create an application menu item for common actions with your package that aren't tied to a specific element:

```
'menu': [
  {
    'label': 'Packages'
    'submenu': [
      {
        'label': 'My Package'
        'submenu': [
          {
            'label': 'Toggle'
            'command': 'my-package:toggle'
          }
        ]
      }
    ]
  }
]
```

To add your own item to the application menu, simply create a top level menu key in any menu configuration file in menus. This can be a JSON or CSON file.

The menu templates you specify are merged with all other templates provided by other packages in the order which they were loaded.

### 上下文菜单

It's recommended to specify a context menu item for commands that are linked to specific parts of the interface, like adding a file in the tree-view:

```
'context-menu':
  '.tree-view':
    'Add file': 'tree-view:add-file'
  '.workspace':
    'Inspect Element': 'core:inspect'
```

To add your own item to the application menu simply create a top level context-menu key in any menu configuration file in menus. This can be a JSON or CSON file.

Context menus are created by determining which element was selected and then adding all of the menu items whose selectors match that element (in the order which they were loaded). The process is then repeated for the elements until reaching the top of the DOM tree.

In the example above, the Add file item will only appear when the focused item or one of its parents has the tree-view class applied to it.

### 代码片段

An extension can supply language snippets in the snippets directory which allows the user to enter repetitive text quickly:

```
".source.coffee .specs":
  "Expect":
    prefix: "ex"
    body: "expect($1).to$2"
  "Describe":
    prefix: "de"
    body: """
      describe "${1:description}", ->
        ${2:body}
    """
```

A snippets file contains scope selectors at its top level (.source.coffee .spec). Each scope selector contains a hash of snippets keyed by their name (Expect, Describe). Each snippet also specifies a prefix and a body key. The prefix represents the first few letters to type before hitting the tab key to autocomplete. The body defines the autofilled text. You can use placeholders like $1, $2, to indicate regions in the body the user can navigate to every time they hit tab.

All files in the directory are automatically loaded, unless the package.json supplies a snippets key. As with all scoped items, snippets loaded later take precedence over earlier snippets when two snippets match a scope with the same specificity.

### 语法分析

If you're developing a new language grammar, you'll want to place your file in the grammars directory. Each grammar is a pairing of two keys, match and captures. match is a regular expression identifying the pattern to highlight, while captures is an object representing what to do with each matching group.

For example:

```
{
  'match': '(?:^|\\s)(__[^_]+__)'
  'captures':
    '1': 'name': 'markup.bold.gfm'
}
```

This indicates that the first matching capture ((__[^_]+__)) should have the markup.bold.gfm token applied to it.

To capture a single group, simply use the name key instead:

```
{
  'match': '^#{1,6}\\s+.+$'
  'name': 'markup.heading.gfm'
}
```
This indicates that Markdown header lines (#, ##, ###) should be applied with the markup.heading.gfm token.

More information about the significance of these tokens can be found in section 12.4 of the TextMate Manual.

Your grammar should also include a filetypes array, which is a list of file extensions your grammar supports:

```
'fileTypes': [
  'markdown'
  'md'
  'mkd'
  'mkdown'
  'ron'
]
```
### 外部资源

It's common to ship external resources like images and fonts in the package, to make it easy to reference the resources in HTML or CSS, you can use the atom protocol URLs to load resources in the package.

The URLs should be in the format of atom://package-name/relative-path-to-package-of-resource, for example, the atom://image-view/images/transparent-background.gif would be equivalent to ~/.atom/packages/image-view/images/transparent-background.gif.

You can also use the atom protocol URLs in themes.

### 编写插件测试

Your package should have tests, and if they're placed in the spec directory, they can be run by Atom.

Under the hood, Jasmine executes your tests, so you can assume that any DSL available there is also available to your package.

### 运行插件测试

Once you've got your test suite written, you can run it by pressing cmd-alt-ctrl-p or via the Developer > Run Package Specs menu.

You can also use the apm test command to run them from the command line. It prints the test output and results to the console and returns the proper status code depending on whether the tests passed or failed.

### 发布插件

Atom bundles a command line utility called apm which can be used to publish Atom packages to the public registry.

Once your package is written and ready for distribution you can run the following to publish your package:

```
cd my-package
apm publish minor
```

This will update your package.json to have a new minor version, commit the change, create a new Git tag, and then upload the package to the registry.

Run apm help publish to see all the available options and apm help to see all the other available commands.
