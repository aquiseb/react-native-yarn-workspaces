# RNW

This package offers an all-in-one solution to work with react-native toghether with yarn workspaces.

As symlinks don't work with react-native, rnw wraps [wml](https://github.com/wix/wml) and configures it in an easy way for you to use.

## How to use

Install the package

```
yarn add -D react-native-yarn-workspaces
```

Setup watchman by adding this to your *package.json*.
Your local packages should be specified in `dependencies` or `devDependencies`.
Never hoist this package.

```
"scripts": {
    "postinstall": "rnw link ../packages/path-to-my-local-packages",
    "watch": "rnw watch"
},
"workspaces": {
    "nohoist": [
      ...
      "react-native-yarn-workspaces",
      "react-native-yarn-workspaces/**"
    ]
  },
"dependencies": {
    "my-local-package": "1.0.0"
}
```


Link your local packages manually (or skip and install all with `yarn` or `lerna bootstrap`)

```
yarn postinstall
```

Watch for changes in your local packages

```
yarn watch
```

If you need to flush one day

```
watchman watch-del-all
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