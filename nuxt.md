#### 页面
```
export default{
  data(){
    return{
      title: 'Hello World!'
    }
  },
  // 在渲染组件之前异步获取数据
  asyncData(context){
    return {
      project:'nuxt'
    }
  },
  // 在渲染页面前填充应用的状态树（store）数据
  async fetch({store,params}){
    let {data} = await axios.get('')
    store.commit('setStarts',data)
  },
  // 设置当前页面的头部标签
  head(){
    return {
      title: this.title,
      meta: [
        { hid: 'description', name: 'description', content: 'My custom description' }
      ]
    }
  },
  // 为页面指定使用哪一个布局文件
  layout: 'blog',
  layout (context) {
    return 'blog'
  },
  // 在应用中的特定页面设置中间件

  // 控制页面渲染前是否滚动至页面顶部
  scrollToTop: true,
  //路由切换时的过渡动效
  transition: 'name',
  transition: {
    name: 'test',
    mode: 'out-in'
  },
  transition (to, from) {
    if (!from) return 'slide-left'
    return +to.query.page < +from.query.page ? 'slide-right' : 'slide-left'
  },
  // 配置一个校验方法用于校验动态路由参数的有效性
  validate ({ params, store }) {
    return store.state.categories.some((category) => category.id === params.id)
  }
}
```
- 注意：为了避免子组件中的meta标签不能正确覆盖父组件中相同的标签而产生重复的现象，建议利用 hid 键为meta标签配一个唯一的标识编号。

#### 组件
```
<template>
  <div>
    // 在布局中显示页面组件（即非布局内容)
    <nuxt/>
    // 显示嵌套路由场景下的页面内容
    <nuxt-child/>
    // 在页面中添加链接至别的页面
    <nuxt-link to="/about">关于</nuxt-link>
  </div>
</template>
```

####
```
// nuxt.config.js

module.exports = {
  // 使用 lru-cache 提供组件缓存功能以获得更好的渲染性能
  cache: true,
  cache: {
    max: 1000,
    maxAge: 900000
  },
  // 配置全局 CSS 文件、模块、库（每个页面都会被引入）
  css: [
    // 加载一个 node.js 模块
    'hover.css/css/hover-min.css',
    // 同样加载一个 node.js 模块，不过我们定义所需的预处理器
    { src: 'bulma', lang: 'sass' },
    // 项目中的 CSS 文件
    '~assets/css/main.css',
    // 项目中的 Sass 文件
    { src: '~assets/css/main.scss', lang: 'scss' } // 指定 scss 而非 sass
  ],
  // 配置 Nuxt.js 应用是开发模式还是生产模式
  dev: (process.env.NODE_ENV !== 'production'),
  // 配置在客户端和服务端共享的环境变量
  env: {
    baseUrl: process.env.BASE_URL || 'http://localhost:3000'
  },
  // 配置生成静态站点的具体方式
  







}
``` 









#### 配置
```
// nuxt.config.js

module.exports = {
  // 自定义 webpack 的构建配置
  build: {
    // 用 webpack-bundle-analyzer 分析并可视化构建后的打包文件
    analyze: true
    analyze: {
      analyzerMode: 'static'
    },
    // 为 JS 和 Vue 文件设定自定义的 babel 配置
    babel: {
      presets: ['es2015', 'stage-0']
    },
    // 为客户端和服务端的构建配置进行手工的扩展处理
    extend (config, { isClient }) {
      if (isClient) {
        config.devtool = 'eval-source-map'
      }
    },
    // 自定义打包文件名
    filenames: {
      vendor: 'vendor.[hash].js',
      app: 'app.[chunkhash].js'
    },
    // 自定义 webpack 加载器
    loaders: [
      {
        test: /\.(png|jpe?g|gif|svg)$/,
        loader: 'url-loader',
        query: {
          limit: 10000, // 10KO
          name: 'img/[name].[hash].[ext]'
        }
      }
    ],
    // 配置 Webpack 插件
    plugins: [
      new webpack.DefinePlugin({
        'process.VERSION': require('./package.json').version
      })
    ],
    // 自定义 postcss 配置
    postcss: [
      require('postcss-nested')(),
      require('postcss-responsive-type')(),
      require('postcss-hexrgba')(),
      require('autoprefixer')({
        browsers: ['last 3 versions']
      })
    ],
    // 将待发布的文件直接上传至 CDN
    publicPath: 'CDN 地址',
    // 在自动生成的 vendor.bundle.js 文件中添加一些模块，以减少应用 bundle 的体积
    vendor: [
      'axios',
      '~plugins/my-lib.js'
    ]
  }
}
```
