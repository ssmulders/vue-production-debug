## What is the Vue Production Debug fork?

This fork is a quick implementation of the issue / feature described here:
https://github.com/vuejs/vue/issues/11666

To allow for debug / warning messages on a production build, edit your build command to include a `WARNING_LEVEL` like so:

```
"build": "cross-env NODE_ENV=production WARNING_LEVEL=handler node_modules/webpack/bin/webpack.js --progress --hide-modules --config=node_modules/laravel-mix/setup/webpack.config.js",
```

Any value other than `none` will work at this time.

You then need to defined custom `warnHandler` and `errorHandler`, as described here: https://vuejs.org/v2/api/

You can then trigger your Sentry or other external bug tracking tools, while simultaneously suppressing any console output. 

## Installation

You need two forks to make this work, and currently they run on Vue 2.6.12:

```
npm i ssmulders/vue-production-debug
npm i ssmulders/component-compiler-utils
```

Then lastly, you need to symlink the component-compiler-utils into the vue-loader directory with a `postinstall` hook:

```
"postinstall": "rm -rf ./node_modules/vue-loader/node_modules/@vue/component-compiler-utils && ln -s ../../../../node_modules/@vue/component-compiler-utils node_modules/vue-loader/node_modules/@vue/component-compiler-utils && echo 'Finished symlinking ssmulders/component-compiler-utils to vue-loader @vue/component-compiler-utils'",
```

After that, run `npm run build`, edit your code to trigger a warning / error and watch those handlers work!