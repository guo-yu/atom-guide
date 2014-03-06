主题设计
---

Atom's interface is rendered using HTML, and it's styled via LESS. Don't worry if you haven't heard of LESS before; it's just like CSS, but with a few handy extensions.

Atom supports two types of themes: UI and syntax. UI themes style elements such as the tree view, the tabs, drop-down lists, and the status bar. Syntax themes style the code inside the editor.

Themes can be installed and changed from the settings view which you can open by selecting the Atom > Preferences... menu and navigating to the Themes section on the left hand side.

### 设计主题前的准备

Themes are pretty straightforward but it's still helpful to be familiar with a few things before starting:

- LESS is a superset of CSS, but it has some really handy features like variables. If you aren't familiar with its syntax, take a few minutes to familiarize yourself.
- You may also want to review the concept of a package.json, too. This file is used to help distribute your theme to Atom users.
- Your theme's package.json must contain a "theme" key with a value of "ui" or "syntax" for Atom to recognize and load it as a theme.
- You can find existing themes to install or fork on atom.io.

### 编写语法高亮主题

Let's create your first theme.

To get started, hit cmd-shift-P, and start typing "Generate Syntax Theme" to generate a new theme package. Select "Generate Syntax Theme," and you'll be asked for the path where your theme will be created. Let's call ours motif-syntax. Tip: syntax themes should end with -syntax.

Atom will pop open a new window, showing the motif-syntax theme, with a default set of folders and files created for us. If you open the settings view (cmd-,) and navigate to the Themes section on the left, you'll see the Motif theme listed in the Syntax Theme drop-down. Select it from the menu to activate it, now when you open an editor you should see that your new motif-syntax theme in action.

Open up stylesheets/colors.less to change the various colors variables which have been already been defined. For example, turn @red into #f4c2c1.

Then open stylesheets/base.less and modify the various selectors that have been already been defined. These selectors style different parts of code in the editor such as comments, strings and the line numbers in the gutter.

As an example, let's make the .gutter background-color into @red.

Reload Atom by pressing cmd-alt-option-L to see the changes you made reflected in your Atom window. Pretty neat!

Tip: You can avoid reloading to see changes you make by opening an atom window in dev mode. To open a Dev Mode Atom window run atom --dev . in the terminal, use cmd-shift-o or use the View > Developer > Open in Dev Mode menu. When you edit your theme, changes will instantly be reflected!


### 编写主题界面

Interface themes must provide a ui-variables.less file which contains all of the variables provided by the core themes.

To create an interface UI theme, do the following:

1. Fork one of the following repositories:
  - atom-dark-ui
  - atom-light-ui
2. Clone the forked repository to the local filesystem
3. Open a terminal in the forked theme's directory
4. Open your new theme in a Dev Mode Atom window run atom --dev . in the terminal or use the View > Developer > Open in Dev Mode menu
5. Change the name of the theme in the theme's package.json file
6. Name your theme end with a -ui. i.e. super-white-ui
7. Run apm link to symlink your repository to ~/.atom/packages
8. Reload Atom using cmd-alt-ctrl-L
9. Enable the theme via UI Theme drop-down in the Themes section of the settings view
10. Make changes! Since you opened the theme in a Dev Mode window, changes will be instantly reflected in the editor without having to reload

### 主题开发工作流

There are a few of tools to help make theme development faster and easier.

#### Live Reload

Reloading by hitting cmd-alt-ctrl-L after you make changes to your theme is less than ideal. Atom supports live updating of styles on Dev Mode Atom windows.

To enable a Dev Mode window:

1. Open your theme directory in a dev window by either going to the View > Developer > Open in Dev Mode menu or by hitting the cmd-shift-o shortcut

2. Make a change to your theme file and save it. Your change should be immediately applied!

If you'd like to reload all the styles at any time, you can use the shortcut cmd-ctrl-shift-r.

#### 开发者工具

Atom is based on the Chrome browser, and supports Chrome's Developer Tools. You can open them by selecting the View > Toggle Developer Tools menu, or by using the cmd-alt-i shortcut.

The dev tools allow you to inspect elements and take a look at their CSS properties.

![](https://f.cloud.github.com/assets/69169/1347391/2d51f91c-36af-11e3-806f-f7b334af43e9.png)

Check out Google's extensive tutorial for a short introduction.

#### 设计风格指导

If you are creating an interface theme, you'll want a way to see how your theme changes affect all the components in the system. The styleguide is a page that renders every component Atom supports.

To open the styleguide, open the command palette (cmd-shift-P) and search for styleguide, or use the shortcut cmd-ctrl-shift-g.

![](https://f.cloud.github.com/assets/69169/1347390/2d431d98-36af-11e3-8f8e-3f4ce1e67adb.png)
