### electron-vue
* electron-vue默认的静态文件都放在public文件夹中，可以通过这个vue.config.js配置调整form和to,来调整electron的静态文件
``` javascript
module.exports = {
    chainWebpack: (config) => {
        // 设置新的静态资源目录.调整electron静态文件资源
        config.plugin("copy").tap((args) => {
            args[0][0].from = `${__dirname}/src/static`;
            return args;
        });
    },
    }
```