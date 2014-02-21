# base

---

深入了解SeaUI底层结构的关键部分，包括我们让web开发变得更好、更快、更强壮的最佳实践。

---

## HTML5文档类型

SeaUI 使用到的某些HTML元素和CSS属性需要将页面设置为HTML5文档类型。在你项目中的每个页面都要参照下面的格式进行设置。

````html
<!DOCTYPE html>
<html lang="zh-cn">
  ...
</html>
````

## 移动设备优先

**SeaUI 是移动设备优先的**。针对移动设备的样式融合进了框架的每个角落，而不是一个单一的文件。

为了确保适当的绘制和触屏缩放，需要在`<head>`之中**添加viewport元数据标签**。

````html
<meta name="viewport" content="width=device-width, initial-scale=1.0">
````

在移动设备浏览器上，通过为viewport meta标签添加user-scalable=no可以禁用其缩放（zooming）功能。这样禁用缩放功能后，用户只能滚动屏幕，就能让你的网站看上去更像原生应用的感觉。注意，这种方式我们并不推荐所有网站使用，还是要看你自己的情况而定！

````html
<meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
````

## 响应式图片

通过添加`.img-responsive` class 可以让 SeaUI 中的图片对响应式布局的支持更友好。其实质是为图片赋予了`max-width: 100%;` 和`height: auto;`属性，可以让图片按比例缩放，不超过其父元素的尺寸。

## 排版和链接

SeaUI设置了全局样式，包括显示、排版和链接。尤其是，我们还：

* 为`body`标签设置了`background-color: #fff;`样式
* 设置了排版的基本属性`@font-family-base、@font-size-base`和`@line-height-base`
* 设置了全局链接颜色属性 `@link-color` 并在 `:hover` 状态时应用下划线样式

这些样式可以在`scaffolding.less`文件中找到。

## Normalize

为了增强跨浏览器表现的一致性，我们使用了[Normalize](http://necolas.github.io/normalize.css/)，这是由[Nicolas Gallagher](http://twitter.com/necolas)和 [Jonathan Neal](http://twitter.com/jon_neal)维护的一个reset库。

## Containers

用`.container`包裹页面上的内容即可实现居中对齐。在不同的媒体查询阈值范围内都为container设置了`width`，用以匹配栅格系统。

注意，由于设置了`padding` 和 固定宽度，`.container`不能嵌套。

````html
<div class="container">
  ...
</div>
````

# 栅格系统

---

SeaUI内置了一套响应式、移动设备优先的流式栅格系统，随着屏幕设备或视口（viewport）尺寸的增加，系统会自动分为最多12列。它包含了易于使用的预定义classe，还有强大的mixin用于生成更具语义的布局。

## 简介

栅格系统用于通过一系列的行（row）与列（column）的组合创建页面布局，你的内容就可以放入创建好的布局中。下面就介绍以下SeaUI栅格系统的工作原理：

*   行（row）”必须包含在`.container`中，以便为其赋予合适的排列（aligment）和内补（padding）。
*   使用“行（row）”在水平方向创建一组“列（column）”。
*   你的内容应当放置于“列（column）”内，而且，只有“列（column）”可以作为行（row）”的直接子元素。
*   类似 `.row` and `.col-xs-4` 这些预定义的栅格class可以用来快速创建栅格布局。SeaUI源码中定义的mixin也可以用来创建语义化的布局。
*   通过设置`padding`从而创建“列（column）”之间的间隔（gutter）。然后通过为第一和最后一样设置负值的`margin`从而抵消掉`padding`的影响。
*   栅格系统中的列是通过指定1到12的值来表示其跨越的范围。例如，三个等宽的列可以使用三个`.col-xs-4`来创建。

通过研究案例，可以将这些原理应用到你的代码中。

> #### 栅格与全宽布局
> 对于需要占据整个浏览器视口（viewport）的页面，需要将内容区域包裹在一个容器元素内，并且赋予`padding: 0 15px;`，为的是抵消掉为`.row`所设置的`margin: 0 -15px;`（如果不这样的话，你的页面会左右超出视口15px，页面底部出现横向滚动条）。

## 媒体查询

在栅格系统中，我们在LESS文件中使用以下媒体查询（media query）来创建关键的分界点阈值。

```css
/* Extra small devices (phones, less than 768px) */
/* No media query since this is the default in SeaUI */

/* Small devices (tablets, 768px and up) */
@media (min-width: @screen-sm-min) { ... }

/* Medium devices (desktops, 992px and up) */
@media (min-width: @screen-md-min) { ... }

/* Large devices (large desktops, 1200px and up) */
@media (min-width: @screen-lg-min) { ... }
```

我们偶尔会精确媒体查询包括`max-width`限制CSS范围更窄的设备。

```css
@media (max-width: @screen-xs-max) { ... }
@media (min-width: @screen-sm-min) and (max-width: @screen-sm-max) { ... }
@media (min-width: @screen-md-min) and (max-width: @screen-md-max) { ... }
@media (min-width: @screen-lg-min) { ... }
```

## 栅格选项

通过下表可以详细查看SeaUI的栅格系统如何在多种屏幕设备上工作的。

<div class="table-responsive">
  <table class="table table-bordered table-striped">
    <thead>
      <tr>
        <th></th>
        <th>
          超小屏幕设备
          <small>手机 (&lt;768px)</small>
        </th>
        <th>
          小屏幕设备
          <small>平板 (≥768px)</small>
        </th>
        <th>
          中等屏幕设备
          <small>桌面 (≥992px)</small>
        </th>
        <th>
          大屏幕设备
          <small>桌面 (≥1200px)</small>
        </th>
      </tr>
    </thead>
    <tbody>
      <tr>
        <th>栅格系统行为</th>
        <td>总是水平排列</td>
        <td colspan="3">开始是堆叠在一起的，超过这些阈值将变为水平排列</td>
      </tr>
      <tr>
        <th>最大<code>.container</code>宽度</th>
        <td>None (自动)</td>
        <td>750px</td>
        <td>970px</td>
        <td>1170px</td>
      </tr>
      <tr>
        <th>class前缀</th>
        <td><code>.col-xs-</code></td>
        <td><code>.col-sm-</code></td>
        <td><code>.col-md-</code></td>
        <td><code>.col-lg-</code></td>
      </tr>
      <tr>
        <th>列数</th>
        <td colspan="4">12</td>
      </tr>
      <tr>
        <th>最大列宽</th>
        <td class="text-muted">自动</td>
        <td>60px</td>
        <td>78px</td>
        <td>95px</td>
      </tr>
      <tr>
        <th>槽宽</th>
        <td colspan="4">30px (每列左右均有15px)</td>
      </tr>
      <tr>
        <th>可嵌套</th>
        <td colspan="4">Yes</td>
      </tr>
      <tr>
        <th>偏移（Offsets）</th>
        <td colspan="4">Yes</td>
      </tr>
      <tr>
        <th>列排序</th>
        <td colspan="4">Yes</td>
      </tr>
    </tbody>
  </table>
</div>

栅格class在屏幕宽度大于或等于阈值的设备上起作用，并且将针对小屏幕设备所设置的class覆盖掉。因此，对任何一个元素应用任何`.col-md-` class 将不仅作用于中等尺寸的屏幕，还将作用于大屏幕设备（如果没有设置`.col-lg-` class的话）。
