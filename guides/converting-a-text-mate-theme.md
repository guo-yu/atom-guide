转换 TextMate 插件
---

This guide will show you how to convert a TextMate theme to an Atom theme.

### 与原生主题的区别

TextMate themes use plist files while Atom themes use CSS or LESS to style the UI and syntax in the editor.

The utility that converts the theme first parses the theme's plist file and then creates comparable CSS rules and properties that will style Atom similarly.

### 安装 apm

The apm command line utility that ships with Atom supports converting a TextMate theme to an Atom theme.

Check that you have apm installed by running the following command in your terminal:

```
apm help init
```

You should see a message print out with details about the apm init command.

If you do not, launch Atom and run the Atom > Install Shell Commmands menu to install the apm and atom commands.

You can now run apm help init to see all the options for initializing new packages and themes.

### 开始转换

Download the theme you wish to convert, you can browse existing TextMate themes here.

Now, let's say you've downloaded the theme to ~/Downloads/MyTheme.tmTheme, you can convert the theme with the following command:

```
apm init --theme ~/.atom/packages/my-theme \
         --convert ~/Downloads/MyTheme.tmTheme
```
You can browse to ~/.atom/packages/my-theme to see the converted theme.

### 激活主题

Now that your theme is installed to ~/.atom/packages you can enable it by launching Atom and selecting the Atom > Preferences... menu.

Select the Themes link on the left side and choose My Theme from the Syntax Theme dropdown menu to enable your new theme.

:tada: Your theme is now enabled, open an editor to see it in action!

### 延伸阅读

Check out Publishing a Package for more information on publishing the theme you just created to atom.io.
