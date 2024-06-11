# JS notes

Starting a new project

```javascirpt
yarn init
```

Adding a dependency

```javascirpt
yarn add [package]
yarn add [package]@[version]
yarn add [package]@[tag]
```

Adding a dependency to different categories of dependencies

Add to devDependencies, peerDependencies, and optionalDependencies respectively:

```javascirpt
yarn add [package] --dev
yarn add [package] --peer
yarn add [package] --optional
```

Upgrading a dependency

```javascirpt
yarn upgrade [package]
yarn upgrade [package]@[version]
yarn upgrade [package]@[tag]
```

Removing a dependency

```javascirpt
yarn remove [package]
```

Installing all the dependencies of project

```javascirpt
yarn
# or
yarn install
```

Use n module from npm to upgrade node:

``` javascirpt
sudo npm cache clean -f
sudo npm install -g n
sudo n stable # stable nodejs
sudo n latest # latest nodejs
```

To Fix PATH:

``` javascirpt
sudo apt-get install --reinstall nodejs-legacy     # fix /usr/bin/node
```

To undo:

``` javascirpt
sudo n rm 6.0.0     # replace number with version of Node that was installed
sudo npm uninstall -g n
```
