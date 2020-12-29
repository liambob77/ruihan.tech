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