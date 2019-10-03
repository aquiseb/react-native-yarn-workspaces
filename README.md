# RNW

This package offers an all-in-one solution to work with react-native toghether with yarn workspaces.

As symlinks don't work with react-native, rnw wraps [wml](https://github.com/wix/wml) and configures it in an easy way for you to use.

## Prerequisite (skip if your workspaces are already setup)

Let's assume that you have a project structure similar to this. But of course yours can be different. The only thing that's important is just to have separate packages handled by yarn workspaces.

```
- myproject/
-- package.json (defines workspaces and could use lerna)

-- app-mobile/
--- package.json

-- app-web/
--- package.json

-- packages/
--- core/
---- package.json

```

#### myproject/package.json

This will define your yarn workspaces. You can include your apps in the workspaces like so.

```json
"name": "myproject",
"private": true,
"workspaces": {
    "packages": [
        "packages/*",
        "app-*"
    ]
},
"scripts": {
    "bootstrap": "lerna bootstrap"
}
```

#### app-mobile/package.json

This is your react-native app. Never hoist react-native-yarn-workspaces.

```json
{
  "name": "app-mobile",
  "private": true,
  "scripts": {
    "postinstall": "rnw link ../packages",
    "watch": "rnw watch",
    "flush": "watchman watch-del-all",
    "start": "expo start"
  },
  "workspaces": {
    "nohoist": [
      "expo",
      "expo/**",
      "react",
      "react/**",
      "react-native",
      "react-native/**",
      "react-native-yarn-workspaces",
      "react-native-yarn-workspaces/**"
    ]
  },
  "dependencies": {
    "@mypackages/core": "1.0.0",
    "expo": "^35.0.0",
    "react": "16.8.3",
    "react-dom": "16.8.3",
    "react-native": "https://github.com/expo/react-native/archive/sdk-35.0.0.tar.gz"
  },
  "devDependencies": {
    "react-native-yarn-workspaces": "1.1.0",
    "babel-preset-expo": "^7.0.0"
  }
}

```

#### app-web/package.json

```json
{
  "name": "app-web",
  "dependencies": {
    "@mypackages/core": "1.0.0"
  }
}
```

#### packages/core/package.json

These are the modules that *app-web* and *app-mobile* share.
There's nothing particular to mention here. Just that react, react-dom and react-native are peerDependencies.


## How to use

**In your mobile app:** Add react-native-yarn-workspaces to your mobile app dependencies.

```shell
yarn add -D react-native-yarn-workspaces
```

Your local packages should be specified in `dependencies` or `devDependencies`.
Never hoist this package.

```json
"name": "app-mobile",
"private": true,
"scripts": {
    "postinstall": "rnw link ../packages",
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
    "@mypackages/core": "1.0.0"
}
```


**In your mobile app:** Link your local packages manually
Or skip and install all your deps with `yarn` or `lerna bootstrap` from one level up.

```shell
yarn postinstall
```

**In your mobile app:** Watch for the changes that happen in *packages/core* by running this in your mobile app.

```shell
yarn watch
```

Of course, in order to have changes actually happening, you may be "watch building" in your packages as well, for instance with `babel src -d dist --watch` in *packages/core*.

If you need to flush one day

```shell
watchman watch-del-all
```

# Recommended

This bin creates a .watchmanconfig in each of your local packages, for instance *packages/core/.watchmanconfig*.
I recommend to also ignore `src` if you're building that package wit babel/webpack/rollup.

```json
// .watchmanconfig
{
  "ignore_dirs": [
    "node_modules",
    "src"
  ]
}
```
