# Skeleton Screen Loading

> A Vue.js project for skeleton screen
> 所谓Skeleton Screen Loading即表示在页面完全渲染完成之前，用户会看到一个样式简单，描绘了当前页面的大致框架，感知到页面正在逐步加载，加载完成后，最终骨架屏中各个占位部分将被真实的数据替换。

* 一图胜千言，来看看微信阅读的客户端，它就使用了Skeleton Screen Loading来提升它的加载体验，可以下载它的客户端实际感受下：
*
![图片描述](https://blog-10039692.file.myqcloud.com/1508382617834_4936_1508382642834.gif)


* 现在好多web和客户端都已经放弃了以前的那种菊花的加载体验，转而使用Skeleton Screen Loading，比如Facebook、medium以及slack等。国内的饿了么、掘金等也都使用Skeleton Screen Loading来提升它们的加载体验。


#### VUE实现思路
* 在本文中，我们将会使用vue组件 (Component) 功能来实现一个Skeleton Screen Loading，如下图所示：

![](https://blog-10039692.file.myqcloud.com/1508382785194_44_1508382808701.gif)

* 具体vue组件 (Component) 功能这里就不详讲了，可以去[官方的文档](https://cn.vuejs.org/v2/guide/components.html)看看详细的信息。

* 这里推荐一个模拟开发数据的开源服务[jsonplaceholder](https://jsonplaceholder.typicode.com/)，类似经常使用的图片占位服务一样，它提供了在web开发中一些常见数据类型的api服务，比如文章、评论、用户系统等，都提供了对应的接口，在开发调试阶段还是非常方便的。

* 比如我们做的这个例子要用到用户系统，直接使用这个[用户数据接口](https://jsonplaceholder.typicode.com/users)就行了。

* 首先，主要的工作是实现Skeleton Screen Loading加载动画，先使用常规的html和css来实现这个动画。

* 动画效果如下所示：

![](https://blog-10039692.file.myqcloud.com/1508382906163_7373_1508382929797.gif)

* 先定义好html结构：
```html
 <div class="timeline-item">
   <div class="animated-background">
     <div class="background-masker header-top"></div>
     <div class="background-masker header-left"></div>
     <div class="background-masker header-right"></div>
     <div class="background-masker header-bottom"></div>
     <div class="background-masker subheader-left"></div>
     <div class="background-masker subheader-right"></div>
     <div class="background-masker subheader-bottom"></div>
   </div>
</div>
```

* 下面来说说实现动画的主要思路：

* 要实现这样的效果，需要使用一点点小技巧。先在图层animated-background定义一个大的渐变背景，然后在不需要显示背景图的位置，定义几个占位的结构填充为白色就可以实现上图所示的UI展现形式。最后使用background-position来移动背景图片的位置，就可以实现图中的动画效果。

* 详细的代码可以去这里查看，[demo](https://codepen.io/janily/pen/rGqQgJ)。

* 主要的效果实现了，下面就可以正式开工来定义我们的Skeleton Screen Loading组件。

```javascript
Vue.component('user-item', {
  props: ['email', 'name'],
  template: `<div>
      <h2 v-text="name"></h2>
      <p v-text="email"></p>
    </div>`
})

Vue.component('loading-item', {
  template: `<div class="animated-background">
     <div class="background-masker header-top"></div>
     <div class="background-masker header-left"></div>
     <div class="background-masker header-right"></div>
     <div class="background-masker header-bottom"></div>
     <div class="background-masker subheader-left"></div>
     <div class="background-masker subheader-right"></div>
     <div class="background-masker subheader-bottom"></div>
   </div>`
})
```

* 上面定义了两个组件，一个是用来显示用户信息数据的组件user-item；一个在加载完之前来显示Skeleton Screen Loading效果的loading-item组件。

* 定义好组件后，然后在主文件定义好对应的结构来注册使用组件：

```html
<div id="app">
  <div v-for="user in users" class="items" v-if="loading">
    <user-item :name="user.name" :email="user.email"></user-item>
  </div>
  <div v-for="load in loades" v-if="!loading">
    <loading-item></loading-item>
  </div>
</div>
```

* 根据上面定义好的组件，上面的代码表示，当数据加载完后，显示用户数据。当还没有加载完用户数据，则显示预先定义好的加载组件即loading-item。

```javascript
var app = new Vue({
  el: '#app',
  data: {
    users: [],
    loading: false,
    loades: 10
  },
  methods: {
    getUserDetails: function() {
      fetch(`https://jsonplaceholder.typicode.com/users`)
        .then(result => result.json())
        .then(result => {
          this.users = result
          this.loading = true
        });
    }
  },
  created: function() {
    setTimeout(() => {
      this.getUserDetails()
    }, 1000);
  }
});
```

* 一个简单优雅的Skeleton Screen Loading就完成了。

* 通过上面简单的实例，可以明显感受到当使用Skeleton Screen Loading来代替传统的菊花图在体验上更加好一些。

* 使用Skeleton Screen Loading，可以利用一些视觉元素来将内容的轮廓更快显示在屏幕上，让用户在等待加载的过程中对将要加载的内容有一个更加清晰的预期，特别是在弱网络的场景下，Skeleton Screen Loading的体验无疑是更好的，用起来吧。

* 对于Skeleton Screen Loading，你有什么样的看法呢？欢迎在评论区留言一起分享你的看法。

## 参考资料：
[http://www.phpgang.com/facebook-style-timeline-content-loading-placeholder-with-css_4994.html](http://www.phpgang.com/facebook-style-timeline-content-loading-placeholder-with-css_4994.html)

[https://css-tricks.com/building-skeleton-screens-css-custom-properties/](https://css-tricks.com/building-skeleton-screens-css-custom-properties/)

## Build Setup

``` bash
# install dependencies
npm install

# serve with hot reload at localhost:8080
npm run dev

# build for production with minification
npm run build

# build for production and view the bundle analyzer report
npm run build --report
```

For a detailed explanation on how things work, check out the [guide](http://vuejs-templates.github.io/webpack/) and [docs for vue-loader](http://vuejs.github.io/vue-loader).
