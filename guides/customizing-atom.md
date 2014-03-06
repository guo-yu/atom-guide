编辑器配置
---

To change a setting, configure a theme, or install a package just open the
Settings view in the current window by pressing cmd-,.

### 切换主题

Atom comes with both light and dark UI themes as well as several syntax themes. You are also encouraged to create or fork your own theme.

To change the active theme just open the Settings view (cmd-,) and select the Themes section from the left hand side. You will see a drop-down menu to change the active Syntax and UI themes.

You can also install more themes from here by browsing the featured themes or searching for a specific theme.

### 安装插件

You can install non-bundled packages by going to the Packages section on left hand side of the Settings view (cmd-,). You will see several featured packages and you can also search for packages from here. The packages listed here have been published to atom.io which is the official registry for Atom packages.

You can also install packages from the command line using apm.

Check that you have apm installed by running the following command in your terminal:

```
apm help install
```

You should see a message print out with details about the apm install command.

If you do not, launch Atom and run the Atom > Install Shell Commmands menu to install the apm and atom commands.

You can also install packages by using the apm install command:

- apm install <package_name> to install the latest version.
- apm install <package_name>@<package_version> to install a specific version.

For example apm install emmet@0.1.5 installs the 0.1.5 release of the Emmet package into ~/.atom/packages.

You can also use apm to find new packages to install:

- apm search coffee to search for CoffeeScript packages.
- apm view emmet to see more information about a specific package.

### 定义快捷键

Atom keymaps work similarly to stylesheets. Just as stylesheets use selectors to apply styles to elements, Atom keymaps use selectors to associate keystrokes with events in specific contexts. Here's a small example, excerpted from Atom's built-in keymaps:

```
'.editor':
  'enter': 'editor:newline'

'.mini.editor input':
  'enter': 'core:confirm'
```

This keymap defines the meaning of enter in two different contexts. In a normal editor, pressing enter emits the editor:newline event, which causes the editor to insert a newline. But if the same keystroke occurs inside of a select list's mini-editor, it instead emits the core:confirm event based on the binding in the more-specific selector.

By default, ~/.atom/keymap.cson is loaded when Atom is started. It will always be loaded last, giving you the chance to override bindings that are defined by Atom's core keymaps or third-party packages.

You can open this file in an editor from the Atom > Open Your Keymap menu.

You'll want to know all the commands available to you. Open the Settings panel (cmd-,) and select the Keybindings tab. It will show you all the keybindings currently in use.

### 进阶配置

Atom loads configuration settings from the config.cson file in your ~/.atom directory, which contains CoffeeScript-style JSON (CSON):

```
'core':
  'excludeVcsIgnoredPaths': true
'editor':
  'fontSize': 18
```

The configuration itself is grouped by the package name or one of the two core namespaces: core and editor.

You can open this file in an editor from the Atom > Open Your Config menu.

### 配置项清单

- core
- editor
- fuzzyFinder
- whitespace
- wrap-guide

### 快速定义指引

#### init.coffee

When Atom finishes loading, it will evaluate init.coffee in your ~/.atom directory, giving you a chance to run arbitrary personal CoffeeScript code to make customizations. You have full access to Atom's API from code in this file. If customizations become extensive, consider creating a package.

You can open this file in an editor from the Atom > Open Your Init Script menu.

For example, if you have the Audio Beep configuration setting enabled, you could add the following code to your ~/.atom/init.coffee file to have Atom greet you with an audio beep every time it loads:

```
atom.beep()
```
This file can also be named init.js and contain JavaScript code.

#### styles.less

If you want to apply quick-and-dirty personal styling changes without creating an entire theme that you intend to publish, you can add styles to the styles.less file in your ~/.atom directory.

You can open this file in an editor from the Atom > Open Your Stylesheet menu.

For example, to change the color of the cursor, you could add the following rule to your ~/.atom/styles.less file:

```
.editor .cursor {
  border-color: pink;
}
```

Unfamiliar with LESS? Read more about it here.

This file can also be named styles.css and contain CSS.
