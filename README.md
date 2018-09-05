# lazyImages

图片懒加载的实现

## 什么是懒加载？

页面中资源，只要不需要，就不加载。

> 改善页面的呈现，更好地利用设备的资源和降低相关的消耗
> 懒加载可以应用在任何的资源上，像 JS 等

## 哪些图片是需要被懒加载的？

基本的观点是，任何资源都不是需要马上加载的。

可以通过 [Google Lighthouse audit tool](https://developers.google.com/web/tools/lighthouse/) ，找出候选的图片有哪些以及在初始化页面的时候你加载了多少字节。也可以使用 [ImageKit's website analyzer](https://imagekit.io/website-analyzer?utm_source=lazyblog&utm_medium=blog) 工具检测出你的网站是否使用的懒加载。

## 懒加载的方法

网页中图片记载的方式有两种：使用 `<img>` 标签和 CSS 的 `background` 属性。公用的做法是，先使用 `<img>` 标签然后使用 CSS 的 `background`。

### 采用 `<img>` 标签进行懒加载的一般的感念

`<img>` 标签，浏览器使用标签 `src` 属性去触发图片的加载。所以，为了加载图片，将图片的  `URL` 值放在另一个属性值里，而不是 `src` 属性中。我们定义一个 `data-src` 属性去保存 `URL` 的值。既然 `src` 的值为空，这个浏览器不会触发图片的加载。

```html
<img data-src="https://ik.imagekit.io/demo/default-image.jpg" />
```

### 使用 `javascript` 事件触发图片加载

我们的技术中，采用监听事件 `scroll`, `resize` 和 `orientationChange` 。`scroll` 用来检测用户是否滚动了页面。`resize` 用来检测浏览器的视窗大小发生改变。`orientationChange` 会在设备发生旋转是触发。

> 事件监听例子 `src/eventListener.html`

## 使用 `Intersection Observer API` 触发图片加载

`Intersection Observer Api` 是比较新的浏览器 API。它可以很简单地检测什么时候元素进去视窗中并且发出一个动作。相较之前的方式，我们必须绑定事件，计算是否元素在视窗中并记住呈现。通过 `Intersection Observer Api` 会简化这个过程，并避免了计算的问题和提供了好的表现。

> 事件监听例子 `src/intersectionObserver.html`

## 通过 CSS `background` 懒加载图片

CSS `background` 并不会像 `<img>` 那样直接。为了加载 CSS 的背景图片，浏览器需要算出 `DOM(Document Object Model)` 树和 `CSSOM(CSS Object Model)` 树去决定是否这个 CSS 样式应该应用到现在 DOM 节点中。

> 通过 CSS 背景图片懒加载 `src/css.html`

## 优化图片懒加载的用户体验

1. 使用正确的图片预览显示
a. 颜色占位图
使用通过原始的图片得到的占位颜色。通过下载 1X1 像素图片然后放大到占位的大小。使用 `ImageKit` 工具。
b. 低像素图片(LQIP)
图片的模糊预览。可以向用户提供一些图片的简单信息。

2. 为图片的像素增加缓冲的时间
考虑到用户滚动太快的话，那之前图片资源的加载会消耗时间，导致当前视图中用户需要查看的图片资源加载时间增多。
采用 `Intersection Observer API`，你可以使用 `root` 和 `rootMargin` 参数(类似 CSS Margin)，这可以增加有效的边界框，这被称为找到交叉点。
采用事件监听的方式，是去检测图片边界和视图边界的差值为 0。我们使用一个正值作为阀。

3. 应该注意的事情未记录
    


## 参考链接

1. [Lazy Loading Images  —  The Complete Guide](https://codeburst.io/lazy-loading-images-the-complete-guide-340d7d31d0bb)