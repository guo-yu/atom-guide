转换 TextMate 插件
---

This guide will show you how to convert a TextMate bundle to an Atom package.

Converting a TextMate bundle will allow you to use its editor preferences, snippets, and colorization inside Atom.

### 安装 apm

The apm command line utility that ships with Atom supports converting a TextMate bundle to an Atom package.

Check that you have apm installed by running the following command in your terminal:

```
apm help init
```

You should see a message print out with details about the apm init command.

If you do not, launch Atom and run the Atom > Install Shell Commmands menu to install the apm and atom commands.

### 开始转换

Let's convert the TextMate bundle for the R programming language. You can find other existing TextMate bundles here.

You can convert the R bundle with the following command:

```
apm init --package ~/.atom/packages/language-r \
         --convert https://github.com/textmate/r.tmbundle
```
You can now browse to ~/.atom/packages/language-r to see the converted bundle.

:tada: Your new package is now ready to use, launch Atom and open a .r file in the editor to see it in action!

### 延伸阅读

Check out Publishing a Package for more information on publishing the package you just created to atom.io.
