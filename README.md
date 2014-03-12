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

## Glyphicons 图标

### 可用的图标

包括200个来自Glyphicon Halflings的字体图标。

<link rel="stylesheet" href="src/icons.css">
<ul class="bs-glyphicons">
      <li>
        <span class="glyphicon glyphicon-adjust"></span>
        <span class="glyphicon-class">glyphicon glyphicon-adjust</span>
      </li>
      <li>
        <span class="glyphicon glyphicon-align-center"></span>
        <span class="glyphicon-class">glyphicon glyphicon-align-center</span>
      </li>
      <li>
        <span class="glyphicon glyphicon-align-justify"></span>
        <span class="glyphicon-class">glyphicon glyphicon-align-justify</span>
      </li>
      <li>
        <span class="glyphicon glyphicon-align-left"></span>
        <span class="glyphicon-class">glyphicon glyphicon-align-left</span>
      </li>
      <li>
        <span class="glyphicon glyphicon-align-right"></span>
        <span class="glyphicon-class">glyphicon glyphicon-align-right</span>
      </li>
      <li>
        <span class="glyphicon glyphicon-arrow-down"></span>
        <span class="glyphicon-class">glyphicon glyphicon-arrow-down</span>
      </li>
      <li>
        <span class="glyphicon glyphicon-arrow-left"></span>
        <span class="glyphicon-class">glyphicon glyphicon-arrow-left</span>
      </li>
      <li>
        <span class="glyphicon glyphicon-arrow-right"></span>
        <span class="glyphicon-class">glyphicon glyphicon-arrow-right</span>
      </li>
      <li>
        <span class="glyphicon glyphicon-arrow-up"></span>
        <span class="glyphicon-class">glyphicon glyphicon-arrow-up</span>
      </li>
      <li>
        <span class="glyphicon glyphicon-asterisk"></span>
        <span class="glyphicon-class">glyphicon glyphicon-asterisk</span>
      </li>
      <li>
        <span class="glyphicon glyphicon-backward"></span>
        <span class="glyphicon-class">glyphicon glyphicon-backward</span>
      </li>
      <li>
        <span class="glyphicon glyphicon-ban-circle"></span>
        <span class="glyphicon-class">glyphicon glyphicon-ban-circle</span>
      </li>
      <li>
        <span class="glyphicon glyphicon-barcode"></span>
        <span class="glyphicon-class">glyphicon glyphicon-barcode</span>
      </li>
      <li>
        <span class="glyphicon glyphicon-bell"></span>
        <span class="glyphicon-class">glyphicon glyphicon-bell</span>
      </li>
      <li>
        <span class="glyphicon glyphicon-bold"></span>
        <span class="glyphicon-class">glyphicon glyphicon-bold</span>
      </li>
      <li>
        <span class="glyphicon glyphicon-book"></span>
        <span class="glyphicon-class">glyphicon glyphicon-book</span>
      </li>
      <li>
        <span class="glyphicon glyphicon-bookmark"></span>
        <span class="glyphicon-class">glyphicon glyphicon-bookmark</span>
      </li>
      <li>
        <span class="glyphicon glyphicon-briefcase"></span>
        <span class="glyphicon-class">glyphicon glyphicon-briefcase</span>
      </li>
      <li>
        <span class="glyphicon glyphicon-bullhorn"></span>
        <span class="glyphicon-class">glyphicon glyphicon-bullhorn</span>
      </li>
      <li>
        <span class="glyphicon glyphicon-calendar"></span>
        <span class="glyphicon-class">glyphicon glyphicon-calendar</span>
      </li>
      <li>
        <span class="glyphicon glyphicon-camera"></span>
        <span class="glyphicon-class">glyphicon glyphicon-camera</span>
      </li>
      <li>
        <span class="glyphicon glyphicon-certificate"></span>
        <span class="glyphicon-class">glyphicon glyphicon-certificate</span>
      </li>
      <li>
        <span class="glyphicon glyphicon-check"></span>
        <span class="glyphicon-class">glyphicon glyphicon-check</span>
      </li>
      <li>
        <span class="glyphicon glyphicon-chevron-down"></span>
        <span class="glyphicon-class">glyphicon glyphicon-chevron-down</span>
      </li>
      <li>
        <span class="glyphicon glyphicon-chevron-left"></span>
        <span class="glyphicon-class">glyphicon glyphicon-chevron-left</span>
      </li>
      <li>
        <span class="glyphicon glyphicon-chevron-right"></span>
        <span class="glyphicon-class">glyphicon glyphicon-chevron-right</span>
      </li>
      <li>
        <span class="glyphicon glyphicon-chevron-up"></span>
        <span class="glyphicon-class">glyphicon glyphicon-chevron-up</span>
      </li>
      <li>
        <span class="glyphicon glyphicon-circle-arrow-down"></span>
        <span class="glyphicon-class">glyphicon glyphicon-circle-arrow-down</span>
      </li>
      <li>
        <span class="glyphicon glyphicon-circle-arrow-left"></span>
        <span class="glyphicon-class">glyphicon glyphicon-circle-arrow-left</span>
      </li>
      <li>
        <span class="glyphicon glyphicon-circle-arrow-right"></span>
        <span class="glyphicon-class">glyphicon glyphicon-circle-arrow-right</span>
      </li>
      <li>
        <span class="glyphicon glyphicon-circle-arrow-up"></span>
        <span class="glyphicon-class">glyphicon glyphicon-circle-arrow-up</span>
      </li>
      <li>
        <span class="glyphicon glyphicon-cloud"></span>
        <span class="glyphicon-class">glyphicon glyphicon-cloud</span>
      </li>
      <li>
        <span class="glyphicon glyphicon-cloud-download"></span>
        <span class="glyphicon-class">glyphicon glyphicon-cloud-download</span>
      </li>
      <li>
        <span class="glyphicon glyphicon-cloud-upload"></span>
        <span class="glyphicon-class">glyphicon glyphicon-cloud-upload</span>
      </li>
      <li>
        <span class="glyphicon glyphicon-cog"></span>
        <span class="glyphicon-class">glyphicon glyphicon-cog</span>
      </li>
      <li>
        <span class="glyphicon glyphicon-collapse-down"></span>
        <span class="glyphicon-class">glyphicon glyphicon-collapse-down</span>
      </li>
      <li>
        <span class="glyphicon glyphicon-collapse-up"></span>
        <span class="glyphicon-class">glyphicon glyphicon-collapse-up</span>
      </li>
      <li>
        <span class="glyphicon glyphicon-comment"></span>
        <span class="glyphicon-class">glyphicon glyphicon-comment</span>
      </li>
      <li>
        <span class="glyphicon glyphicon-compressed"></span>
        <span class="glyphicon-class">glyphicon glyphicon-compressed</span>
      </li>
      <li>
        <span class="glyphicon glyphicon-copyright-mark"></span>
        <span class="glyphicon-class">glyphicon glyphicon-copyright-mark</span>
      </li>
      <li>
        <span class="glyphicon glyphicon-credit-card"></span>
        <span class="glyphicon-class">glyphicon glyphicon-credit-card</span>
      </li>
      <li>
        <span class="glyphicon glyphicon-cutlery"></span>
        <span class="glyphicon-class">glyphicon glyphicon-cutlery</span>
      </li>
      <li>
        <span class="glyphicon glyphicon-dashboard"></span>
        <span class="glyphicon-class">glyphicon glyphicon-dashboard</span>
      </li>
      <li>
        <span class="glyphicon glyphicon-download"></span>
        <span class="glyphicon-class">glyphicon glyphicon-download</span>
      </li>
      <li>
        <span class="glyphicon glyphicon-download-alt"></span>
        <span class="glyphicon-class">glyphicon glyphicon-download-alt</span>
      </li>
      <li>
        <span class="glyphicon glyphicon-earphone"></span>
        <span class="glyphicon-class">glyphicon glyphicon-earphone</span>
      </li>
      <li>
        <span class="glyphicon glyphicon-edit"></span>
        <span class="glyphicon-class">glyphicon glyphicon-edit</span>
      </li>
      <li>
        <span class="glyphicon glyphicon-eject"></span>
        <span class="glyphicon-class">glyphicon glyphicon-eject</span>
      </li>
      <li>
        <span class="glyphicon glyphicon-envelope"></span>
        <span class="glyphicon-class">glyphicon glyphicon-envelope</span>
      </li>
      <li>
        <span class="glyphicon glyphicon-euro"></span>
        <span class="glyphicon-class">glyphicon glyphicon-euro</span>
      </li>
      <li>
        <span class="glyphicon glyphicon-exclamation-sign"></span>
        <span class="glyphicon-class">glyphicon glyphicon-exclamation-sign</span>
      </li>
      <li>
        <span class="glyphicon glyphicon-expand"></span>
        <span class="glyphicon-class">glyphicon glyphicon-expand</span>
      </li>
      <li>
        <span class="glyphicon glyphicon-export"></span>
        <span class="glyphicon-class">glyphicon glyphicon-export</span>
      </li>
      <li>
        <span class="glyphicon glyphicon-eye-close"></span>
        <span class="glyphicon-class">glyphicon glyphicon-eye-close</span>
      </li>
      <li>
        <span class="glyphicon glyphicon-eye-open"></span>
        <span class="glyphicon-class">glyphicon glyphicon-eye-open</span>
      </li>
      <li>
        <span class="glyphicon glyphicon-facetime-video"></span>
        <span class="glyphicon-class">glyphicon glyphicon-facetime-video</span>
      </li>
      <li>
        <span class="glyphicon glyphicon-fast-backward"></span>
        <span class="glyphicon-class">glyphicon glyphicon-fast-backward</span>
      </li>
      <li>
        <span class="glyphicon glyphicon-fast-forward"></span>
        <span class="glyphicon-class">glyphicon glyphicon-fast-forward</span>
      </li>
      <li>
        <span class="glyphicon glyphicon-file"></span>
        <span class="glyphicon-class">glyphicon glyphicon-file</span>
      </li>
      <li>
        <span class="glyphicon glyphicon-film"></span>
        <span class="glyphicon-class">glyphicon glyphicon-film</span>
      </li>
      <li>
        <span class="glyphicon glyphicon-filter"></span>
        <span class="glyphicon-class">glyphicon glyphicon-filter</span>
      </li>
      <li>
        <span class="glyphicon glyphicon-fire"></span>
        <span class="glyphicon-class">glyphicon glyphicon-fire</span>
      </li>
      <li>
        <span class="glyphicon glyphicon-flag"></span>
        <span class="glyphicon-class">glyphicon glyphicon-flag</span>
      </li>
      <li>
        <span class="glyphicon glyphicon-flash"></span>
        <span class="glyphicon-class">glyphicon glyphicon-flash</span>
      </li>
      <li>
        <span class="glyphicon glyphicon-floppy-disk"></span>
        <span class="glyphicon-class">glyphicon glyphicon-floppy-disk</span>
      </li>
      <li>
        <span class="glyphicon glyphicon-floppy-open"></span>
        <span class="glyphicon-class">glyphicon glyphicon-floppy-open</span>
      </li>
      <li>
        <span class="glyphicon glyphicon-floppy-remove"></span>
        <span class="glyphicon-class">glyphicon glyphicon-floppy-remove</span>
      </li>
      <li>
        <span class="glyphicon glyphicon-floppy-save"></span>
        <span class="glyphicon-class">glyphicon glyphicon-floppy-save</span>
      </li>
      <li>
        <span class="glyphicon glyphicon-floppy-saved"></span>
        <span class="glyphicon-class">glyphicon glyphicon-floppy-saved</span>
      </li>
      <li>
        <span class="glyphicon glyphicon-folder-close"></span>
        <span class="glyphicon-class">glyphicon glyphicon-folder-close</span>
      </li>
      <li>
        <span class="glyphicon glyphicon-folder-open"></span>
        <span class="glyphicon-class">glyphicon glyphicon-folder-open</span>
      </li>
      <li>
        <span class="glyphicon glyphicon-font"></span>
        <span class="glyphicon-class">glyphicon glyphicon-font</span>
      </li>
      <li>
        <span class="glyphicon glyphicon-forward"></span>
        <span class="glyphicon-class">glyphicon glyphicon-forward</span>
      </li>
      <li>
        <span class="glyphicon glyphicon-fullscreen"></span>
        <span class="glyphicon-class">glyphicon glyphicon-fullscreen</span>
      </li>
      <li>
        <span class="glyphicon glyphicon-gbp"></span>
        <span class="glyphicon-class">glyphicon glyphicon-gbp</span>
      </li>
      <li>
        <span class="glyphicon glyphicon-gift"></span>
        <span class="glyphicon-class">glyphicon glyphicon-gift</span>
      </li>
      <li>
        <span class="glyphicon glyphicon-glass"></span>
        <span class="glyphicon-class">glyphicon glyphicon-glass</span>
      </li>
      <li>
        <span class="glyphicon glyphicon-globe"></span>
        <span class="glyphicon-class">glyphicon glyphicon-globe</span>
      </li>
      <li>
        <span class="glyphicon glyphicon-hand-down"></span>
        <span class="glyphicon-class">glyphicon glyphicon-hand-down</span>
      </li>
      <li>
        <span class="glyphicon glyphicon-hand-left"></span>
        <span class="glyphicon-class">glyphicon glyphicon-hand-left</span>
      </li>
      <li>
        <span class="glyphicon glyphicon-hand-right"></span>
        <span class="glyphicon-class">glyphicon glyphicon-hand-right</span>
      </li>
      <li>
        <span class="glyphicon glyphicon-hand-up"></span>
        <span class="glyphicon-class">glyphicon glyphicon-hand-up</span>
      </li>
      <li>
        <span class="glyphicon glyphicon-hd-video"></span>
        <span class="glyphicon-class">glyphicon glyphicon-hd-video</span>
      </li>
      <li>
        <span class="glyphicon glyphicon-hdd"></span>
        <span class="glyphicon-class">glyphicon glyphicon-hdd</span>
      </li>
      <li>
        <span class="glyphicon glyphicon-header"></span>
        <span class="glyphicon-class">glyphicon glyphicon-header</span>
      </li>
      <li>
        <span class="glyphicon glyphicon-headphones"></span>
        <span class="glyphicon-class">glyphicon glyphicon-headphones</span>
      </li>
      <li>
        <span class="glyphicon glyphicon-heart"></span>
        <span class="glyphicon-class">glyphicon glyphicon-heart</span>
      </li>
      <li>
        <span class="glyphicon glyphicon-heart-empty"></span>
        <span class="glyphicon-class">glyphicon glyphicon-heart-empty</span>
      </li>
      <li>
        <span class="glyphicon glyphicon-home"></span>
        <span class="glyphicon-class">glyphicon glyphicon-home</span>
      </li>
      <li>
        <span class="glyphicon glyphicon-import"></span>
        <span class="glyphicon-class">glyphicon glyphicon-import</span>
      </li>
      <li>
        <span class="glyphicon glyphicon-inbox"></span>
        <span class="glyphicon-class">glyphicon glyphicon-inbox</span>
      </li>
      <li>
        <span class="glyphicon glyphicon-indent-left"></span>
        <span class="glyphicon-class">glyphicon glyphicon-indent-left</span>
      </li>
      <li>
        <span class="glyphicon glyphicon-indent-right"></span>
        <span class="glyphicon-class">glyphicon glyphicon-indent-right</span>
      </li>
      <li>
        <span class="glyphicon glyphicon-info-sign"></span>
        <span class="glyphicon-class">glyphicon glyphicon-info-sign</span>
      </li>
      <li>
        <span class="glyphicon glyphicon-italic"></span>
        <span class="glyphicon-class">glyphicon glyphicon-italic</span>
      </li>
      <li>
        <span class="glyphicon glyphicon-leaf"></span>
        <span class="glyphicon-class">glyphicon glyphicon-leaf</span>
      </li>
      <li>
        <span class="glyphicon glyphicon-link"></span>
        <span class="glyphicon-class">glyphicon glyphicon-link</span>
      </li>
      <li>
        <span class="glyphicon glyphicon-list"></span>
        <span class="glyphicon-class">glyphicon glyphicon-list</span>
      </li>
      <li>
        <span class="glyphicon glyphicon-list-alt"></span>
        <span class="glyphicon-class">glyphicon glyphicon-list-alt</span>
      </li>
      <li>
        <span class="glyphicon glyphicon-lock"></span>
        <span class="glyphicon-class">glyphicon glyphicon-lock</span>
      </li>
      <li>
        <span class="glyphicon glyphicon-log-in"></span>
        <span class="glyphicon-class">glyphicon glyphicon-log-in</span>
      </li>
      <li>
        <span class="glyphicon glyphicon-log-out"></span>
        <span class="glyphicon-class">glyphicon glyphicon-log-out</span>
      </li>
      <li>
        <span class="glyphicon glyphicon-magnet"></span>
        <span class="glyphicon-class">glyphicon glyphicon-magnet</span>
      </li>
      <li>
        <span class="glyphicon glyphicon-map-marker"></span>
        <span class="glyphicon-class">glyphicon glyphicon-map-marker</span>
      </li>
      <li>
        <span class="glyphicon glyphicon-minus"></span>
        <span class="glyphicon-class">glyphicon glyphicon-minus</span>
      </li>
      <li>
        <span class="glyphicon glyphicon-minus-sign"></span>
        <span class="glyphicon-class">glyphicon glyphicon-minus-sign</span>
      </li>
      <li>
        <span class="glyphicon glyphicon-move"></span>
        <span class="glyphicon-class">glyphicon glyphicon-move</span>
      </li>
      <li>
        <span class="glyphicon glyphicon-music"></span>
        <span class="glyphicon-class">glyphicon glyphicon-music</span>
      </li>
      <li>
        <span class="glyphicon glyphicon-new-window"></span>
        <span class="glyphicon-class">glyphicon glyphicon-new-window</span>
      </li>
      <li>
        <span class="glyphicon glyphicon-off"></span>
        <span class="glyphicon-class">glyphicon glyphicon-off</span>
      </li>
      <li>
        <span class="glyphicon glyphicon-ok"></span>
        <span class="glyphicon-class">glyphicon glyphicon-ok</span>
      </li>
      <li>
        <span class="glyphicon glyphicon-ok-circle"></span>
        <span class="glyphicon-class">glyphicon glyphicon-ok-circle</span>
      </li>
      <li>
        <span class="glyphicon glyphicon-ok-sign"></span>
        <span class="glyphicon-class">glyphicon glyphicon-ok-sign</span>
      </li>
      <li>
        <span class="glyphicon glyphicon-open"></span>
        <span class="glyphicon-class">glyphicon glyphicon-open</span>
      </li>
      <li>
        <span class="glyphicon glyphicon-paperclip"></span>
        <span class="glyphicon-class">glyphicon glyphicon-paperclip</span>
      </li>
      <li>
        <span class="glyphicon glyphicon-pause"></span>
        <span class="glyphicon-class">glyphicon glyphicon-pause</span>
      </li>
      <li>
        <span class="glyphicon glyphicon-pencil"></span>
        <span class="glyphicon-class">glyphicon glyphicon-pencil</span>
      </li>
      <li>
        <span class="glyphicon glyphicon-phone"></span>
        <span class="glyphicon-class">glyphicon glyphicon-phone</span>
      </li>
      <li>
        <span class="glyphicon glyphicon-phone-alt"></span>
        <span class="glyphicon-class">glyphicon glyphicon-phone-alt</span>
      </li>
      <li>
        <span class="glyphicon glyphicon-picture"></span>
        <span class="glyphicon-class">glyphicon glyphicon-picture</span>
      </li>
      <li>
        <span class="glyphicon glyphicon-plane"></span>
        <span class="glyphicon-class">glyphicon glyphicon-plane</span>
      </li>
      <li>
        <span class="glyphicon glyphicon-play"></span>
        <span class="glyphicon-class">glyphicon glyphicon-play</span>
      </li>
      <li>
        <span class="glyphicon glyphicon-play-circle"></span>
        <span class="glyphicon-class">glyphicon glyphicon-play-circle</span>
      </li>
      <li>
        <span class="glyphicon glyphicon-plus"></span>
        <span class="glyphicon-class">glyphicon glyphicon-plus</span>
      </li>
      <li>
        <span class="glyphicon glyphicon-plus-sign"></span>
        <span class="glyphicon-class">glyphicon glyphicon-plus-sign</span>
      </li>
      <li>
        <span class="glyphicon glyphicon-print"></span>
        <span class="glyphicon-class">glyphicon glyphicon-print</span>
      </li>
      <li>
        <span class="glyphicon glyphicon-pushpin"></span>
        <span class="glyphicon-class">glyphicon glyphicon-pushpin</span>
      </li>
      <li>
        <span class="glyphicon glyphicon-qrcode"></span>
        <span class="glyphicon-class">glyphicon glyphicon-qrcode</span>
      </li>
      <li>
        <span class="glyphicon glyphicon-question-sign"></span>
        <span class="glyphicon-class">glyphicon glyphicon-question-sign</span>
      </li>
      <li>
        <span class="glyphicon glyphicon-random"></span>
        <span class="glyphicon-class">glyphicon glyphicon-random</span>
      </li>
      <li>
        <span class="glyphicon glyphicon-record"></span>
        <span class="glyphicon-class">glyphicon glyphicon-record</span>
      </li>
      <li>
        <span class="glyphicon glyphicon-refresh"></span>
        <span class="glyphicon-class">glyphicon glyphicon-refresh</span>
      </li>
      <li>
        <span class="glyphicon glyphicon-registration-mark"></span>
        <span class="glyphicon-class">glyphicon glyphicon-registration-mark</span>
      </li>
      <li>
        <span class="glyphicon glyphicon-remove"></span>
        <span class="glyphicon-class">glyphicon glyphicon-remove</span>
      </li>
      <li>
        <span class="glyphicon glyphicon-remove-circle"></span>
        <span class="glyphicon-class">glyphicon glyphicon-remove-circle</span>
      </li>
      <li>
        <span class="glyphicon glyphicon-remove-sign"></span>
        <span class="glyphicon-class">glyphicon glyphicon-remove-sign</span>
      </li>
      <li>
        <span class="glyphicon glyphicon-repeat"></span>
        <span class="glyphicon-class">glyphicon glyphicon-repeat</span>
      </li>
      <li>
        <span class="glyphicon glyphicon-resize-full"></span>
        <span class="glyphicon-class">glyphicon glyphicon-resize-full</span>
      </li>
      <li>
        <span class="glyphicon glyphicon-resize-horizontal"></span>
        <span class="glyphicon-class">glyphicon glyphicon-resize-horizontal</span>
      </li>
      <li>
        <span class="glyphicon glyphicon-resize-small"></span>
        <span class="glyphicon-class">glyphicon glyphicon-resize-small</span>
      </li>
      <li>
        <span class="glyphicon glyphicon-resize-vertical"></span>
        <span class="glyphicon-class">glyphicon glyphicon-resize-vertical</span>
      </li>
      <li>
        <span class="glyphicon glyphicon-retweet"></span>
        <span class="glyphicon-class">glyphicon glyphicon-retweet</span>
      </li>
      <li>
        <span class="glyphicon glyphicon-road"></span>
        <span class="glyphicon-class">glyphicon glyphicon-road</span>
      </li>
      <li>
        <span class="glyphicon glyphicon-save"></span>
        <span class="glyphicon-class">glyphicon glyphicon-save</span>
      </li>
      <li>
        <span class="glyphicon glyphicon-saved"></span>
        <span class="glyphicon-class">glyphicon glyphicon-saved</span>
      </li>
      <li>
        <span class="glyphicon glyphicon-screenshot"></span>
        <span class="glyphicon-class">glyphicon glyphicon-screenshot</span>
      </li>
      <li>
        <span class="glyphicon glyphicon-sd-video"></span>
        <span class="glyphicon-class">glyphicon glyphicon-sd-video</span>
      </li>
      <li>
        <span class="glyphicon glyphicon-search"></span>
        <span class="glyphicon-class">glyphicon glyphicon-search</span>
      </li>
      <li>
        <span class="glyphicon glyphicon-send"></span>
        <span class="glyphicon-class">glyphicon glyphicon-send</span>
      </li>
      <li>
        <span class="glyphicon glyphicon-share"></span>
        <span class="glyphicon-class">glyphicon glyphicon-share</span>
      </li>
      <li>
        <span class="glyphicon glyphicon-share-alt"></span>
        <span class="glyphicon-class">glyphicon glyphicon-share-alt</span>
      </li>
      <li>
        <span class="glyphicon glyphicon-shopping-cart"></span>
        <span class="glyphicon-class">glyphicon glyphicon-shopping-cart</span>
      </li>
      <li>
        <span class="glyphicon glyphicon-signal"></span>
        <span class="glyphicon-class">glyphicon glyphicon-signal</span>
      </li>
      <li>
        <span class="glyphicon glyphicon-sort"></span>
        <span class="glyphicon-class">glyphicon glyphicon-sort</span>
      </li>
      <li>
        <span class="glyphicon glyphicon-sort-by-alphabet"></span>
        <span class="glyphicon-class">glyphicon glyphicon-sort-by-alphabet</span>
      </li>
      <li>
        <span class="glyphicon glyphicon-sort-by-alphabet-alt"></span>
        <span class="glyphicon-class">glyphicon glyphicon-sort-by-alphabet-alt</span>
      </li>
      <li>
        <span class="glyphicon glyphicon-sort-by-attributes"></span>
        <span class="glyphicon-class">glyphicon glyphicon-sort-by-attributes</span>
      </li>
      <li>
        <span class="glyphicon glyphicon-sort-by-attributes-alt"></span>
        <span class="glyphicon-class">glyphicon glyphicon-sort-by-attributes-alt</span>
      </li>
      <li>
        <span class="glyphicon glyphicon-sort-by-order"></span>
        <span class="glyphicon-class">glyphicon glyphicon-sort-by-order</span>
      </li>
      <li>
        <span class="glyphicon glyphicon-sort-by-order-alt"></span>
        <span class="glyphicon-class">glyphicon glyphicon-sort-by-order-alt</span>
      </li>
      <li>
        <span class="glyphicon glyphicon-sound-5-1"></span>
        <span class="glyphicon-class">glyphicon glyphicon-sound-5-1</span>
      </li>
      <li>
        <span class="glyphicon glyphicon-sound-6-1"></span>
        <span class="glyphicon-class">glyphicon glyphicon-sound-6-1</span>
      </li>
      <li>
        <span class="glyphicon glyphicon-sound-7-1"></span>
        <span class="glyphicon-class">glyphicon glyphicon-sound-7-1</span>
      </li>
      <li>
        <span class="glyphicon glyphicon-sound-dolby"></span>
        <span class="glyphicon-class">glyphicon glyphicon-sound-dolby</span>
      </li>
      <li>
        <span class="glyphicon glyphicon-sound-stereo"></span>
        <span class="glyphicon-class">glyphicon glyphicon-sound-stereo</span>
      </li>
      <li>
        <span class="glyphicon glyphicon-star"></span>
        <span class="glyphicon-class">glyphicon glyphicon-star</span>
      </li>
      <li>
        <span class="glyphicon glyphicon-star-empty"></span>
        <span class="glyphicon-class">glyphicon glyphicon-star-empty</span>
      </li>
      <li>
        <span class="glyphicon glyphicon-stats"></span>
        <span class="glyphicon-class">glyphicon glyphicon-stats</span>
      </li>
      <li>
        <span class="glyphicon glyphicon-step-backward"></span>
        <span class="glyphicon-class">glyphicon glyphicon-step-backward</span>
      </li>
      <li>
        <span class="glyphicon glyphicon-step-forward"></span>
        <span class="glyphicon-class">glyphicon glyphicon-step-forward</span>
      </li>
      <li>
        <span class="glyphicon glyphicon-stop"></span>
        <span class="glyphicon-class">glyphicon glyphicon-stop</span>
      </li>
      <li>
        <span class="glyphicon glyphicon-subtitles"></span>
        <span class="glyphicon-class">glyphicon glyphicon-subtitles</span>
      </li>
      <li>
        <span class="glyphicon glyphicon-tag"></span>
        <span class="glyphicon-class">glyphicon glyphicon-tag</span>
      </li>
      <li>
        <span class="glyphicon glyphicon-tags"></span>
        <span class="glyphicon-class">glyphicon glyphicon-tags</span>
      </li>
      <li>
        <span class="glyphicon glyphicon-tasks"></span>
        <span class="glyphicon-class">glyphicon glyphicon-tasks</span>
      </li>
      <li>
        <span class="glyphicon glyphicon-text-height"></span>
        <span class="glyphicon-class">glyphicon glyphicon-text-height</span>
      </li>
      <li>
        <span class="glyphicon glyphicon-text-width"></span>
        <span class="glyphicon-class">glyphicon glyphicon-text-width</span>
      </li>
      <li>
        <span class="glyphicon glyphicon-th"></span>
        <span class="glyphicon-class">glyphicon glyphicon-th</span>
      </li>
      <li>
        <span class="glyphicon glyphicon-th-large"></span>
        <span class="glyphicon-class">glyphicon glyphicon-th-large</span>
      </li>
      <li>
        <span class="glyphicon glyphicon-th-list"></span>
        <span class="glyphicon-class">glyphicon glyphicon-th-list</span>
      </li>
      <li>
        <span class="glyphicon glyphicon-thumbs-down"></span>
        <span class="glyphicon-class">glyphicon glyphicon-thumbs-down</span>
      </li>
      <li>
        <span class="glyphicon glyphicon-thumbs-up"></span>
        <span class="glyphicon-class">glyphicon glyphicon-thumbs-up</span>
      </li>
      <li>
        <span class="glyphicon glyphicon-time"></span>
        <span class="glyphicon-class">glyphicon glyphicon-time</span>
      </li>
      <li>
        <span class="glyphicon glyphicon-tint"></span>
        <span class="glyphicon-class">glyphicon glyphicon-tint</span>
      </li>
      <li>
        <span class="glyphicon glyphicon-tower"></span>
        <span class="glyphicon-class">glyphicon glyphicon-tower</span>
      </li>
      <li>
        <span class="glyphicon glyphicon-transfer"></span>
        <span class="glyphicon-class">glyphicon glyphicon-transfer</span>
      </li>
      <li>
        <span class="glyphicon glyphicon-trash"></span>
        <span class="glyphicon-class">glyphicon glyphicon-trash</span>
      </li>
      <li>
        <span class="glyphicon glyphicon-tree-conifer"></span>
        <span class="glyphicon-class">glyphicon glyphicon-tree-conifer</span>
      </li>
      <li>
        <span class="glyphicon glyphicon-tree-deciduous"></span>
        <span class="glyphicon-class">glyphicon glyphicon-tree-deciduous</span>
      </li>
      <li>
        <span class="glyphicon glyphicon-unchecked"></span>
        <span class="glyphicon-class">glyphicon glyphicon-unchecked</span>
      </li>
      <li>
        <span class="glyphicon glyphicon-upload"></span>
        <span class="glyphicon-class">glyphicon glyphicon-upload</span>
      </li>
      <li>
        <span class="glyphicon glyphicon-usd"></span>
        <span class="glyphicon-class">glyphicon glyphicon-usd</span>
      </li>
      <li>
        <span class="glyphicon glyphicon-user"></span>
        <span class="glyphicon-class">glyphicon glyphicon-user</span>
      </li>
      <li>
        <span class="glyphicon glyphicon-volume-down"></span>
        <span class="glyphicon-class">glyphicon glyphicon-volume-down</span>
      </li>
      <li>
        <span class="glyphicon glyphicon-volume-off"></span>
        <span class="glyphicon-class">glyphicon glyphicon-volume-off</span>
      </li>
      <li>
        <span class="glyphicon glyphicon-volume-up"></span>
        <span class="glyphicon-class">glyphicon glyphicon-volume-up</span>
      </li>
      <li>
        <span class="glyphicon glyphicon-warning-sign"></span>
        <span class="glyphicon-class">glyphicon glyphicon-warning-sign</span>
      </li>
      <li>
        <span class="glyphicon glyphicon-wrench"></span>
        <span class="glyphicon-class">glyphicon glyphicon-wrench</span>
      </li>
      <li>
        <span class="glyphicon glyphicon-zoom-in"></span>
        <span class="glyphicon-class">glyphicon glyphicon-zoom-in</span>
      </li>
      <li>
        <span class="glyphicon glyphicon-zoom-out"></span>
        <span class="glyphicon-class">glyphicon glyphicon-zoom-out</span>
      </li>
</ul>

### 如何使用

出于性能的考虑，所有图标都需要基类和单独的图标类。把下面的代码放在任何地方都能使用。为了留下正确的内补（padding），一定要在图标和文本之间加上一个空格。

> 不要和其它组件混合使用
> 图标 class 不能和其它元素联合使用，因为这些图标被设计为独立的元素、独立使用。

```html
<span class="glyphicon glyphicon-search"></span>
```