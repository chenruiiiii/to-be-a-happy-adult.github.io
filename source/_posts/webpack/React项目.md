<h1 id="XCBZA">创建项目</h1>
> create-react-app 项目名称
>



<h2 id="lQ2RM">配置tsconfig.json</h2>
配置`tsconfig.json`可以帮助



<h2 id="QIFtw">使用craco更改webpack配置</h2>
`React`官方文档推荐使用`CRA ( create-react-app )`来创建项目，其中会隐藏起有关`webpack`的配置，可以通过`npm run eject`修改相关配置，但是这个操作时不可逆的❗️也就是说一旦执行了此操作，那么`webpack`相关的处理都需要自行进行处理，比如说配置、构建等。对于小白来说，还是不要轻易尝试的好。



如果想要**配置路径别名**或者**less**等操作是，可以通过`craco`进行配置。

> npm i @craco/craco
>

<h3 id="eIOky">配置less</h3>
> npm i croco-less -D
>

```typescript
const CracoLessPlugin = require('craco-less')

module.exports = {
  // ... 
  plugins: [
    {
      plugin: CracoLessPlugin,
      options: {
        lessLoaderOptions: { // 配置可以参照webpack的less-loader具体配置
          lessOptions: {
            javascriptEnabled: true // 允许less文件中使用js表达式
          }
        }
      }
    }
  ],
}
```

<h3 id="yNmk8">移动端自适应 postcss-pxtorem</h3>
> npm i postcss-pxtorem -D
>

+ 在入口文件`index.ts`中引入

> import 'lib-flexible';
>

+ 配置postcss-pxtorem

```typescript
const postcssPx2Rem = require('postcss-pxtorem')
module.exports = {
    //...
    style: {
        postcss: {
          mode: 'extends',
          loaderOptions: () => {
            return {
              postcssOptions: {
                ident: 'postcss',
                config: false,
                plugins: [
                  postcssPx2Rem({
                    rootValue: 37.5, // 设计稿尺寸/10
                    propList: ['*'], // 需要转换的样式属性，默认为 ['*']，即匹配所有属性
                    exclude: /node_modules/i // 排除掉node_modules中转换
                  })
                ]
              },
              sourceMap: false
            }
          }
        }
      },
  //...
}

```

<h3 id="EIFiG">代理</h3>
```typescript
    // craco.config.js文件
    module.exports = {
        //...
        devServer: {
            port: 8888,
            hot: true,
            client: {
              overlay: false
            },
            // 配置代理解决跨域
            proxy: {
              '/': {
                target: process.env.REACT_APP_URL, // https://xxx.com
                changeOrigin: true,
                pathRewrite: {
                  '^/': ''
                }
              }
            }
        }
    //...
    }

```

<h3 id="KgRMz">路径别名和打包大小分析</h3>
```typescript
 // craco.config.js文件
const { BundleAnalyzerPlugin } = require('webpack-bundle-analyzer')
module.exports = {
  // ...
  webpack: {
    // 配置别名
    alias: {
      // 约定：使用 @ 表示 src 文件所在路径
      '@': path.resolve(__dirname, 'src')
    },
    
    plugins: [
      // 打包分析插件，需要分析时解开注释
      // [new BundleAnalyzerPlugin()]
    ],
   // ...
}

```

<h3 id="Em1x3">拆包和打包路径</h3>
```typescript
// craco.config.js文件
module.exports = {
  // ...
  webpack: {
    configure: webpackConfig => {
      // 生产环境配置
      if (isProd) {
        // 去除map文件
        webpackConfig.devtool = false
        // 拆包
        webpackConfig.optimization = {
          splitChunks: {
            chunks: 'async',
            minSize: 40000, // bite
            maxAsyncRequests: 10, // 最大异步请求数
            maxInitialRequests: 10, // 页面初始化最大异步请求数
            automaticNameDelimiter: '~', // 解决命名冲突
            name: false,
            cacheGroups: {
              antd: {
                name: 'chunk-antd',
                chunks: 'all',
                test: /[\\/]node_modules[\\/](@ant-design|antd-mobile)[\\/]/,
                priority: -7
              },
              common: {
                name: 'chunk-common',
                chunks: 'all',
                test: /[\\/]node_modules[\\/](react|react-dom|react-router|react-redux|react-router-dom|redux-persist|react-fastclick)[\\/]/,
                priority: -9
              },
              vendor: {
                name: 'chunk-vendor',
                chunks: 'all',
                test: /[\\/]node_modules[\\/](axios|lodash|core-js|react-copy-to-clipboard|crypto-js|web-vitals)[\\/]/,
                priority: -10
              }
            }
          }
        }
        // 输出output
        webpackConfig.output = {
          ...webpackConfig.output,
          publicPath: './' // 打包资源引入路径--目前使用的是相对路径
        }
      }
      return webpackConfig
    }
  },
  // ...
 }

```

<h3 id="s9fUg">移除开发日志console.log</h3>
```typescript
module.exports = {
     babel: {
        plugins: [
          // 生产环境只留console.error、warn，去除console.log
          [
            'babel-plugin-transform-remove-console',
            { exclude: isProd ? ['error', 'warn'] : ['error', 'warn', 'log'] }
          ]
        ]
      },
}

```

<h3 id="s4tQR">修改项目运行命令 & 使用`dotenv-cli`加载开发、测试、生产不同环境的环境变量</h3>
> <font style="color:rgb(102, 102, 102);background-color:rgb(248, 248, 248);">npm i dotenv-cli -D</font>
>

`<font style="color:rgb(102, 102, 102);background-color:rgb(248, 248, 248);">package.json</font>`



```typescript
  "scripts": {
    "start": "craco start",
    "build": "craco build",
    "test": "craco test",
    "eject": "react-scripts eject",
    "prod": "dotenv -e .env.production craco start",
    "build:dev": "dotenv -e .env.development craco build",
    "build:test": "dotenv -e .env.test craco build",
    "build:prod": "dotenv -e .env.production craco build",
    "lint": "eslint -c .eslintrc.js src --ext .ts,.tsx,.js,.jsx --fix",
    "clean": "rimraf node_modules"
  },
```

<h2 id="jvR17"></h2>


