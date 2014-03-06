发布插件
---

This guide will show you how to publish a package or theme to the atom.io package registry.

Publishing a package allows other people to install it and use it in Atom. It is a great way to share what you've made and get feedback and contributions from others.

This guide assumes your package's name is my-package and but you should pick a better name.

### 安装 apm

The apm command line utility that ships with Atom supports publishing packages to the atom.io registry.

Check that you have apm installed by running the following command in your terminal:

```
apm help publish
```
You should see a message print out with details about the apm publish command.

If you do not, launch Atom and run the Atom > Install Shell Commmands menu to install the apm and atom commands.

### 发布准备

If you've followed the steps in the your first package doc then you should be ready to publish and you can skip to the next step.

If not, there are a few things you should check before publishing:

- Your package.json file has name, description, and repository fields.
- Your package.json file has a version field with a value of "0.0.0".
- Your package.json file has an engines field that contains an entry for Atom such as: "engines": {"atom": ">=0.50.0"}.
- Your package has a README.md file at the root.
- Your package is in a Git repository that has been pushed to GitHub. Follow this guide if your package isn't already on GitHub.

### 发布插件

Before you publish a package it is a good idea to check ahead of time if a package with the same name has already been published to atom.io. You can do that by visiting http://atom.io/packages/my-package to see if the package already exists. If it does, update your package's name to something that is available before proceeding.

Now let's review what the apm publish command does:

- Registers the package name on atom.io if it is being published for the first time.
- Updates the version field in the package.json file and commits it.
- Creates a new Git tag for the version being published.
- Pushes the tag and current branch up to GitHub.
- Updates atom.io with the new version being published.

Now run the following commands to publish your package:

```
cd ~/github/my-package
apm publish minor
```

If this is the first package you are publishing, the apm publish command may prompt you for your GitHub username and password. This is required to publish and you only need to enter this information the first time you publish. The credentials are stored securely in your keychain once you login.

:tada: Your package is now published and available on atom.io. Head on over to http://atom.io/packages/my-package to see your package's page.

The minor option to the publish command tells apm to increment the second digit of the version before publishing so the published version will be 0.1.0 and the Git tag created will be v0.1.0.

In the future you can run apm publish major to publish the 1.0.0 version but since this was the first version being published it is a good idead to start with a minor release.

### 延伸阅读

Check out [semantic versioning](http://semver.org/) to learn more about versioning your package releases.
