1、是用echarts提供的构建脚本自定义构建

```
node node_modules/echarts/build/build.js --min -i echarts.custom.js -o lib/echarts.custom.min.js
```
在构建过程中会出现uglifyPlugin is not a function 的错误，由于echarts/build/config.js导出的uglifyPlugin是一个对象

```
const uglifyPlugin = require('rollup-plugin-uglify');
```
打印uglifyPlugin { uglify: [Function: uglify] }，所以正确的是应该uglifyPlugin.uglify 

```
min && plugins.push(uglifyPlugin.uglify({
        compress: {
            // Eliminate __DEV__ code.
            // Currently, in uglify:
            // `var vx; if(vx) {...}` can not be removed.
            // `if (__DEV__) {...}` can be removed if `__DEV__` is defined as `false` in `global_defs`.
            // 'global_defs': {
            //     __DEV__: false
            // },
            'dead_code': true
        },
        output: {
            preamble: preamble
        }
    }));
```
