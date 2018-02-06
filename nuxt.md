#### 安装
```
// 快速使用: starter 模板
$ vue init nuxt-community/starter-template <project-name>

// 从头开始新建项目
// package.json
{
  "scripts": {
    "dev": "nuxt"
  }
}
$ npm install --save nuxt
$ npm run dev
```

#### 目录结构
assets: 资源目录--用于组织未编译的静态资源如 LESS、SASS 或 JavaScript
components: 组件目录--用于组织应用的 Vue.js 组件。Nuxt.js 不会扩展增强该目录下 Vue.js 组件，即这些组件不会像页面组件那样有 asyncData 方法的特性
layouts: 布局目录--用于组织应用的布局组件(该目录名为Nuxt.js保留的，不可更改)
middleware: 中间件目录--用于存放应用的中间件
pages: 页面目录--用于组织应用的路由及视图。Nuxt.js 框架读取该目录下所有的 .vue 文件并自动生成对应的路由配置(该目录名为Nuxt.js保留的，不可更改)
plugins: 插件目录--用于组织那些需要在 根vue.js应用实例化之前运行的 Javascript 插件
static: 静态文件目录--用于存放应用的静态文件，此类文件不会被 Nuxt.js 调用 Webpack 进行构建编译处理。服务器启动的时候，该目录下的文件会映射至应用的根路径'/'下(该目录名为Nuxt.js保留的，不可更改)
store: 用于组织应用的 Vuex 状态树文件。Nuxt.js 框架集成了 Vuex 状态树的相关功能配置，在 store 目录下创建一个 index.js 文件可激活这些配置(该目录名为Nuxt.js保留的，不可更改)
nuxt.config.js: 用于组织Nuxt.js 应用的个性化配置，以便覆盖默认配置(该目录名为Nuxt.js保留的，不可更改)

#### 视图 layouts
默认模板: 在应用根目录下创建 app.html 的文件
默认布局: 添加 layouts/default.vue 文件来扩展应用的默认布局
错误页面: 通过编辑 layouts/error.vue 文件来定制化错误页面

#### 路由 pages
Nuxt.js 依据 pages 目录结构自动生成 vue-router 模块的路由配置
动态路由: 带参数的动态路由，需要创建对应的以下划线作为前缀的 Vue 文件/目录
嵌套路由: 创建内嵌子路由，你需要添加一个 Vue 文件，同时添加一个与该文件同名的目录用来存放子视图组件
过渡动效: Nuxt.js 使用 Vue.js 的< transition >组件来实现路由切换时的过渡动效,Nuxt.js 默认使用的过渡效果名称为 page
中间件: 中间件允许您定义一个自定义函数运行在一个页面或一组页面渲染之前
```
pages/
--| _category/
-----| _subCategory/
--------| _id.vue
--------| index.vue
-----| _subCategory.vue
-----| index.vue
--| _category.vue
--| index.vue

router: {
  routes: [
    {
      path: '/',
      component: 'pages/index.vue',
      name: 'index'
    },
    {
      path: '/:category',
      component: 'pages/_category.vue',
      children: [
        {
          path: '',
          component: 'pages/_category/index.vue',
          name: 'category'
        },
        {
          path: ':subCategory',
          component: 'pages/_category/_subCategory.vue',
          children: [
            {
              path: '',
              component: 'pages/_category/_subCategory/index.vue',
              name: 'category-subCategory'
            },
            {
              path: ':id',
              component: 'pages/_category/_subCategory/_id.vue',
              name: 'category-subCategory-id'
            }
          ]
        }
      ]
    }
  ]
}
```

#### 组件 components
```
// component.vue

<template>
  <div>
    // 在布局中显示页面组件（即非布局内容)
    <nuxt/>
    // 显示嵌套路由场景下的页面内容
    <nuxt-child/>
    // 在页面中添加链接至别的页面,同<router-link>
    <nuxt-link to="/about">关于</nuxt-link>
  </div>
</template>
```

#### Vuex 状态树
Nuxt.js 会尝试找到应用根目录下的 store 目录，如果该目录存在，它将做以下的事情：
1. 引用 vuex 模块
2. 将 vuex 模块 加到 vendors 构建配置中去
3. 设置 Vue 根实例的 store 配置项

Nuxt.js 支持两种使用 store 的方式，你可以择一使用：
- 普通方式： store/index.js 返回一个 Vuex.Store 实例
```
import Vue from 'vue'
import Vuex from 'vuex'

Vue.use(Vuex)

const store = () => new Vuex.Store({

  state: {
    counter: 0
  },
  mutations: {
    increment (state) {
      state.counter++
    }
  }
})

export default store
```

- 模块方式： store 目录下的每个 .js 文件会被转换成为状态树指定命名的子模块 （当然，index 是根模块）
```
export const state = () => ({
  counter: 0
})

export const mutations = {
  increment (state) {
    state.counter++
  }
}
```

#### 页面
```
// page.vue

export default{

  data(){
    return{
      title: 'Hello World!'
    }
  },

  // 在渲染组件之前异步获取数据
  // 在方法无法通过 this 来引用组件的实例对象
  // Nuxt.js 会将 asyncData 返回的数据融合组件 data 方法返回的数据一并返回给当前组件
  asyncData({context,...},...){ // Function
    return {
            // 是否来自客户端渲染(客&服)
    context:isClient: Boolean
            // 是否来自服务端渲染(客&服)
            isServer: Boolean
            // 是否是开发(dev) 模式，在生产环境的数据缓存中用到(客&服)
            isDev: Boolean
            // vue-router 实例(客&服)
            route: vue-router
            // Vuex.Store 实例,只有vuex 数据流存在相关配置时可用(客&服)
            store: vuex
            // nuxt.config.js 中配置的环境变量(客&服)
            env: Object
            // route.params 的别名(客&服)
            params: Object
            // route.query 的别名(客&服)
            query: Object
            // Node 的 Request 对象,中间件:根据使用的框架而定。nuxt generate 不可用(服)
            req: http.Request
            // Node 的 Response 对象,中间件:根据使用的框架而定。nuxt generate 不可用(服)
            res: http.Response
            // 重定向用户请求到另一个路由,状态码在服务端被使用，默认 302(客&服)
            redirect([status,] path [, query]): Function
            // 展示错误页(客&服)
            error(params){ // Function
              params.statusCode(必)
              params.message(必)
            }
    }
  },

  // 在每次加载前（在服务端或切换至目标路由之前) 填充应用的状态树（store）数据
  fetch({context,...}){ // Function
    return {...}
  }

  // 使用 vue-meta 更新应用的头部标签(Head) 和 html 属性,可通过 this 获取组件的数据
  head(){ // Object | Function
    return {
      title: this.title,
      meta: [
        { hid: 'description', name: 'description', content: 'My custom description' }
      ]
    }
  },

  // 为页面指定使用哪一个布局文件
  layout ({context,...}) { // String | Function('default')
    return ...
  },

  // 中间件,定义一个自定义函数运行在一个页面或一组页面渲染之前
  // 可以在 nuxt.config.js 、 layouts 或者 pages 中使用
  middleware: String | Array,

  // 控制页面渲染前是否滚动至页面顶部
  scrollToTop: Boolean (默：false)

  //路由切换时的过渡动效
  transition: 'transition.name', // String | Object | Function
  transition: {
    // 所有路由过渡都会用到的过渡名称
    name: String(page)
    // 所有路由都用到的过渡模式
    mode: String(ut-in)
    // 是否给页面组件根元素添加 CSS 过渡类名。false:路由过渡时将触发页面组件事件注册的 Javascript 钩子方法
    css: Boolean(true)
    // 指定过滤动效事件的类型，用于判断过渡结束的时间点。值可以是transition/animation。默认情况下, Nuxt.js 会自动侦测动效事件的类型
    type: String
    // 目标路由动效开始时的类名
    enterClass: String
    // 目标路由动效结束时的类名
    enterToClass: String
    // 目标路由过渡过程中的类名
    enterActiveClass: String
    // 当前路由动效开始时的类名
    leaveClass: String
    // 当前路由动效结束时的类名
    leaveToClass: String
    // 当前路由动效过程中的类名
    leaveActiveClass: String
  },
  transition (to, from) {},

  // 配置一个校验方法用于校验动态路由参数的有效性
  validate ({context,...}) { // Function
    return ...
  }
}
```
- 注意：为了避免子组件中的meta标签不能正确覆盖父组件中相同的标签而产生重复的现象，建议利用 hid 键为meta标签配一个唯一的标识编号。
- Javascript 钩子方法:
    beforeEnter(el)
    enter(el, done)
    afterEnter(el)
    enterCancelled(el)
    beforeLeave(el)
    leave(el, done)
    afterLeave(el)
    leaveCancelled(el)


#### 配置
```
// nuxt.config.js

module.exports = {

  // 根据服务端需求，自定义 webpack 的构建配置
  build: {
      // webpack-bundle-analyzer: 分析 bundle 内容的插件及 CLI 工具，以便捷的、交互式、可缩放的树状图形式展现给用户
      analyze: { // Boolean(false) | Object
      // 分析器将启动HTTP服务器来显示软件包报告/生成带有报告的单个HTML文件/当`generateStatsFile=true`生成Webpack_Stats_JSON文件
      analyzerMode: server(默)/static/disabled
      // 启动HTTP服务器的主机名
      analyzerHost: String(127.0.0.1)
      // 启动HTTP服务器的端口号
      analyzerPort: Number(8888)
      // 当'analyzerMode=static"时,生成的报告文件的路径
      reportFilename: String
      // /统计大小,从Webpack的stats对象中获得/通过gzip压缩运行解析的bundle,modules的大小
      defaultSizes: parsed(默)/stat/gzip
      // 在默认浏览器中自动打开报告
      openAnalyzer: Boolean(true)
      // 当'generateStatsFile=true'时,在bundle输出目录中生成webpack_stats_JSON文件
      generateStatsFile: Boolean(false)
      // 当'generateStatsFile=true'时,webpack_stats_JSON文件的名称
      statsFilename: String(stats.json.Name)
      // /stats.toJson()方法的选项
      statsOptions: null(默)/Object
      // 用于控制插件输出的详细信息
      logLevel: info/warn/error/silent
    },

    // 为 JS 和 Vue 文件设定自定义的 babel 配置
    babel: { // Object
      presets: ['vue-app'](默)
    },

    // 客户端和服务端的构建配置进行手工的扩展处理,该扩展方法会被调用两次，一次在服务端打包构建时，另一次在客户端打包构建时
    extend(config, { isDev/isClient/isServer }) { // Function(Webpack 配置对象,构建环境对象) },

    // 自定义打包文件名
    filenames { // Object
      vendor: 'vendor.bundle.[hash].js',(默)
      app: 'nuxt.bundle.[chunkhash].js'(默)
    },

    // 自定义 webpack 加载器
    // 当 nuxt.config.js 里有自定义的 loaders 配置时，将会覆盖默认的配置
    loaders: [ // Array[Object,...]
      {(默)
        test: /\.(png|jpe?g|gif|svg)$/,
        loader: 'url-loader',
        query: {
          limit: 1000, // 1KO
          name: 'img/[name].[hash:7].[ext]'
        }
      },
      {(默)
        test: /\.(woff2?|eot|ttf|otf)(\?.*)?$/,
        loader: 'url-loader',
        query: {
          limit: 1000, // 1 KO
          name: 'fonts/[name].[hash:7].[ext]'
        }
      }
    ],

    // 配置 Webpack 插件
    plugins: [ // Array ],

    // 自定义 postcss 配置
    postcss: [ // Array
      require('autoprefixer')({(默)
        browsers: ['last 3 versions']
      })
    ],

    // 将待发布的文件直接上传至 CDN 以获得最佳访问性能
    publicPath: String('/_nuxt/'),

    // 把模块打包进 vendor bundle，以减少应用 bundle 的体积(防止重复打包)
    vendor: [ // Array[String,...]
      一些第三方模块 (比如 axios)
    ]
  },

  // 使用 lru-cache 提供组件缓存功能以获得更好的渲染性能
  cache: { // Boolean(false) | Object
    // 缓存组件的最大数目，当第 1001 个组件被添加至缓存中时，第一个被缓存的组件会从缓存中移除
    max: Number(1000)
    // 缓存时间，单位毫秒, 默: 15 分钟
    maxAge: Number(900000)
  },

  // 配置全局 CSS 文件、模块、库（每个页面都会被引入）
  css: [ // Array[String | Object]
    {
      // 文件路径
      src: String,
      // 所需的预处理器
      lang: String
    }
  ],

  // 配置 Nuxt.js 应用是开发模式还是生产模式,在编码中使用 nuxt.js 时才会用到该配置
  // dev 属性的值会被 nuxt 命令覆盖,当使用 nuxt 命令时'dev=true',当使用 nuxt build/nuxt start/uxt generate 命令时'dev=false'
  dev: Boolean(true),

  // 配置在客户端和服务端共享的环境变量
  env: { // Object },

  // 配置生成静态站点的具体方式
  // 当运行 nuxt generate 命令/在编码中调用 nuxt.generate() 时触发
  generate: { // Object
    // nuxt generate 生成的目录名称
    dir: Sring('dist')

    // Nuxt.js 在生成静态文件时使用 html-minifier 对 html 文件进行压缩，你可以修改以下默认配置来调整压缩的行为
    minify:{ // Object(默)
      collapseBooleanAttributes: true,
      collapseWhitespace: false,
      decodeEntities: true,
      minifyCSS: true,
      minifyJS: true,
      processConditionalComments: true,
      removeAttributeQuotes: false,
      removeComments: false,
      removeEmptyAttributes: true,
      removeOptionalTags: true,
      removeRedundantAttributes: true,
      removeScriptTypeAttributes: false,
      removeStyleLinkTypeAttributes: false,
      removeTagWhitespace: false,
      sortAttributes: true,
      sortClassName: false,
      trimCustomFragments: true,
      useShortDoctype: true
    }

    // Nuxt.js 执行 generate 命令时，动态路由 会被忽略,如果想让 Nuxt.js 为动态路由也生成静态文件，你需要指定动态路由参数的值，并配置到 routes 数组中去
    routes: // Array[String] | Promise | Callback
  },

  // 在 nuxt.config.js 中配置应用的 meta 信息
  head: { // Object
    title (String),
    titleTemplate (String | Function), // '%s'代替title_val
    htmlAttrs (Object),
    bodyAttrs (Object),
    base (Object),
    meta (Array[Object]),
    link (Array[Object]),
    style ([Object]),
    script ([Object]),
    noscript ([Object]),
    __dangerouslyDisableSanitizers ([String]),
    __dangerouslyDisableSanitizersByTagID ({[String]}),
    changed (Function)
  },

  // 定制加载组件的样式，禁用或者创建自己的加载组件
  loading: Boolean,
  loading:{ // Object
    // 进度条的颜色
    color: String(black),
    // 页面加载失败时的颜色（当 data 或 fetch 方法返回错误时）
    failedColor: String(red),
    // 进度条的高度 (在进度条元素的 style 属性上体现)
    height: String(2px),
    // 进度条的最大显示时长(毫秒),Nuxt.js 假设页面在该时长内加载完毕
    duration: Number(5000)	
  },
  loading: String(组件的路径)

  // 配置 Nuxt.js 的性能选项
  performance: { // Object
    // 在生产模式下, Nuxt.js 会使用 compression 模块来 gzip 应用的所有资源文件
    gzip: Boolean | Object
    // 在生产模式下，Nuxt.js 使用浏览器的预加载策略来预加载目标页面的脚本资源
    prefetch: Boolean(true)
  },
  
  // 为 Nuxt.js 配置使用 Vue.js 插件
  plugins: [ // Array[String | Object]
    {
      // 文件的路径
      src: String,
      // 如果值为 false，该文件只会在客户端被打包引入
      ssr: Boolean(true)
    }
  ],
  
  // 设置 Nuxt.js 应用的根目录
  // 该配置项一般是编码中使用 Nuxt.js 时才会被用到,并且会被 nuxt 命令行指定的路径参数覆盖
  // 应用的 node_modules 目录必须在 rootDir 目录内。应用的源码目录（srcDir）则无此限制
  rootDir: String(process.cwd()),
  
  // 个性化配置 Nuxt.js 应用的路由（vue-router）
  router: { // Object
    // 应用的根URL,设置后，Nuxt.js会自动将它添加至页面中：<base href="{{ router.base }}"/>
    // 该配置项的值会被直接传给 vue-router 的构造器
    base: String('/'),
    // 配置路由的模式，鉴于服务端渲染的特性，不建议修改该配置
    mode: String(history),
    // 全局配置 <nuxt-link> 组件默认的激活类名
    linkActiveClass: String(nuxt-link-active),
    // 个性化配置跳转至目标页面后的页面滚动位置。每次页面渲染后都会调用 scrollBehavior 配置的方法
    scrollBehavior: Function(to, from, savedPosition){(默)
      // savedPosition 只有在 popstate 导航（如按浏览器的返回按钮）时可以获取。
      if (savedPosition) {
        return savedPosition
      } else {
        let position = {}
        // 目标页面子组件少于两个
        if (to.matched.length < 2) {
          // 滚动至页面顶部
          position = { x: 0, y: 0 }
        }
        else if (to.matched.some((r) => r.components.default.options.scrollToTop)) {
          // 如果目标页面子组件中存在配置了scrollToTop为true
          position = { x: 0, y: 0 }
        }
        // 如果目标页面的url有锚点,  则滚动至锚点所在的位置
        if (to.hash) {
          position = { selector: to.hash }
        }
        return position
      }
    },
    // 为应用的每个页面设置默认的中间件
    middleware: String | Array[String],
    // 扩展 Nuxt.js 生成的路由配置
    extendRoutes: Function
  },
  
  // 设置 Nuxt.js 应用的源码目录
  srcDir: String(rootDir_value),

  // 设置页面切换过渡效果的默认属性值
  transition: { // String | Object
    name: 'page',
    mode: 'out-in'
  },

  // 覆盖 Nuxt.js 默认的 watchers 配置
  watchers: { // Object
    // Nodejs文件监控
    chokidar: Object,
    webpack: Object
  }
}
``` 
自定义组件需要实现以下接口方法:
// 路由更新（即浏览器地址变化）时调用, 请在该方法内显示组件(必)

start()
// 路由更新完毕（即asyncData方法调用完成且页面加载完）时调用，请在该方法内隐藏组件(必)

finish()
// 路由更新失败时调用（如asyncData方法返回异常)(否)

fail()
// 页面加载过程中调用, num 是小于 100 的整数(否)
increase(num)


#### 资源文件
默认情况下 Nuxt 使用 vue-loader、file-loader 以及 url-loader 这几个 Webpack 加载器来处理文件的加载和引用。
对于不需要通过 Webpack 处理的静态资源文件，可以放置在 static 目录中

默认情况下, vue-loader自动使用 css-loader 和Vue模板编译器来编译处理vue文件中的样式和模板。在此编译过程中，所有的资源URL例如 < img src="...">、 background: url(...) 和 CSS中的 @import 均会被解析成模块通过 require 引用。

.png 并非 JavaScript 文件, 因此 Nuxt.js 通过配置Webpack使用file-loader 和 url-loader 这两个加载器来处理此类引用。

这样做的好处有：
- file-loader 能让你指定从什么地方拷贝资源文件以及发布后放到哪个目录去，并能让你使用版本哈希码来重命名发布后的文件来实现增量更新和更好的缓存策略。
- url-loader 能根据你指定的文件大小阈值，来判断一个文件是转换成内联的base-64码（如果该文件尺寸小于该阈值）还是使用file-loader来降级处理。小文件base-64化能有效减少HTTP请求数
即文件（图片或字体）的尺寸小于1K的时候，它将会被转换成 Base-64 data URL 来内联引用；否则它将被拷贝至指定的子目录（在 .nuxt 目录下），并被重命名（加上7位的哈希码作为版本标识）以实现更好的缓存策略

如果你的静态资源文件需要 Webpack 做构建编译处理，可以放到 assets 目录，否则可以放到 static 目录中去。
Nuxt 服务器启动的时候，该目录下的文件会映射至应用的根路径'/'下，像 robots.txt 或 sitemap.xml 这种类型的文件就很适合放到 static 目录中。
你可以在代码中使用根路径'/'结合资源相对路径来引用静态资源


#### NUXT 模块
```
// 以编程形式使用 Nuxt.js
const Nuxt = require('nuxt')

// 配置
Nuxt(options)

// 调试信息,在应用入口文件的头部加入以下设置
process.env.DEBUG = 'nuxt:*'

// 通过 nuxt.render函数，把 Nuxt.js 变成你 Node.js 服务端的中间件
// 建议把 nuxt.render 放到中间件列表的最后面，因为它不会再调用 next() 方法，而是直接处理你 web 应用的页面渲染
nuxt.render(req, res){ // Function{return Promise} }

// 使用指定的上下文对象渲染指定的路由路径
// 和 nuxt.renderAndGetWindow 类似，该方法只用于测试目的
nuxt.renderRoute(route, {context}){ // Function
  // 带渲染的路由路径
  route: String,
  // 指定的上下文对象
  context(可): { // Object
    req | res 
  }

  return Promise{
    html: String
    error: null 或 Object
    redirected: false | Object
  }
}

// 渲染指定url并获取对应的window对象
// 要使用这个方法，需要先安装 jsdom, 此方法只用于 测试目的
nuxt.renderAndGetWindow(url, options = {}){ // Function
  // 待渲染的 URL 路径
  url: String,
  // 
  options(可): { // Object
    virtualConsole： Boolean(true)
  }

  return Promise(window对象)
}

```

#### 发布部署
1. 服务端渲染应用部署
```
$ nuxt build
$ nuxt start

// 推荐的 package.json 配置
{
  "name": "my-app",
  "dependencies": {
    "nuxt": "latest"
  },
  "scripts": {
    "dev": "nuxt",
    "build": "nuxt build",
    "start": "nuxt start"
  }
}
```
> 建议将 .nuxt 加入 .npmignore 和 .gitignore 文件中

2. 静态应用部署
```
npm run generate
```
该命令会创建一个 dist 文件夹，所有静态化后的资源文件均在其中

