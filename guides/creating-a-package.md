编写插件
---

Packages are at the core of Atom. Nearly everything outside of the main editor is handled by a package. That includes "core" pieces like the file tree, status bar, syntax highlighting, and more.

A package can contain a variety of different resource types to change Atom's behavior. The basic package layout is as follows:

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
Not every package will have (or need) all of these directories.

We have a tutorial on creating your first package.

There are also guides for converting TextMate bundles and TextMate themes so they work in Atom.

### package.json

Similar to npm packages, Atom packages contain a package.json file in their top-level directory. This file contains metadata about the package, such as the path to its "main" module, library dependencies, and manifests specifying the order in which its resources should be loaded.

In addition to the regular npm package.json keys available, Atom package.json files have their own additions.

- main (Required): the path to the CoffeeScript file that's the entry point to your package
- stylesheets (Optional): an Array of Strings identifying the order of the stylesheets your package needs to load. If not specified, stylesheets in the stylesheets directory are added alphabetically.
- keymaps(Optional): an Array of Strings identifying the order of the key mappings your package needs to load. If not specified, mappings in the keymaps directory are added alphabetically.
- menus(Optional): an Array of Strings identifying the order of the menu mappings your package needs to load. If not specified, mappings in the menus directory are added alphabetically.
- snippets (Optional): an Array of Strings identifying the order of the snippets your package needs to load. If not specified, snippets in the snippets directory are added alphabetically.
- activationEvents (Optional): an Array of Strings identifying events that trigger your package's activation. You can delay the loading of your package until one of these events is triggered.

### 插件源码

If you want to extend Atom's behavior, your package should contain a single top-level module, which you export from index.coffee (or whichever file is indicated by the main key in your package.json file). The remainder of your code should be placed in the lib directory, and required from your top-level file.

Your package's top-level module is a singleton object that manages the lifecycle of your extensions to Atom. Even if your package creates ten different views and appends them to different parts of the DOM, it's all managed from your top-level object.

Your package's top-level module should implement the following methods:

- activate(state): This required method is called when your package is activated. It is passed the state data from the last time the window was serialized if your module implements the serialize() method. Use this to do initialization work when your package is started (like setting up DOM elements or binding events).

- serialize(): This optional method is called when the window is shutting down, allowing you to return JSON to represent the state of your component. When the window is later restored, the data you returned is passed to your module's activate method so you can restore your view to where the user left off.

- deactivate(): This optional method is called when the window is shutting down. If your package is watching any files or holding external resources in any other way, release them here. If you're just subscribing to things on window, you don't need to worry because that's getting torn down anyway.

### 范例插件

Your directory would look like this:

```
my-package/
  package.json
  index.coffee
  lib/
    my-package.coffee
```
index.coffee might be:

```
module.exports = require "./lib/my-package"
```
my-package/my-package.coffee might start:

```
module.exports =
  activate: (state) -> # ...
  deactivate: -> # ...
  serialize: -> # ...
```
Beyond this simple contract, your package has access to Atom's API. Be aware that since we are early in development, APIs are subject to change and we have not yet established clear boundaries between what is public and what is private. Also, please collaborate with us if you need an API that doesn't exist. Our goal is to build out Atom's API organically based on the needs of package authors like you.

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
