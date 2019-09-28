# RNW

This package offers an all-in-one solution to work with react-native toghether with yarn workspaces.

As symlinks don't work with react-native, rnw wraps [wml](https://github.com/wix/wml) and configures it in an easy way for you to use.

## How to use

Install the package

```
yarn add -D react-native-yarn-workspaces
```

Setup watchman by adding this to your *package.json*.
Your local packages should be specified in `dependencies` or `devDependencies`

```
"scripts": {
    "link": "rnw link ../packages/path-to-my-local-packages",
    "watch": "rnw watch"
}
"dependencies": {
    "my-local-package": "1.0.0"
}
```

Link your local packages

```
yarn link
```

Watch for changes in your local packages

```
yarn watch
```

## Recommended

This bin creates a .watchmanconfig in each of your local packages. I recommend to also ignore `src` if you're building that package.

```
// .watchmanconfig
{
  "ignore_dirs": [
    "node_modules",
    "src"
  ]
}
```