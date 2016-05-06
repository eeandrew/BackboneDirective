# BackboneDirective 用Backbone构建可复用的弱指令(weak directive)

Backbone虽然是一个较为古老并且使用繁琐的MVC框架，但是使用Backbone依然可以写出复用性较高的弱指令。熟悉Angular的同学可能对指令的概念并不陌生。
简单说来指令就是对HTML语义的扩展。Angular的指令解析可以大致分为两步

* Compiler找出HTML中Angular定义的新的指令比如 ng-accordin
* 用自己定义的真实HTML片段去替换ng-accordin指令，将其解析为浏览器可以理解的语义片段。比如"\<div class="accordin"\>...\</div\>"

那么什么是弱指令呢？弱指令是我这里暂时给出的一个定义。主要是指通过Backbone的View来统一可复用的UI代码。那为什么不直接叫做View而叫做Directive呢？因为Backbone没有提供一个Compiler，所以基于Backbone的View不能实现像Angular那样的HTML解析，只能靠自己手动解析和替换HTML代码。

那为什么我们将Backbone和Angular比较，而不是和React或者Vue比较？概况说来就是Backbone实在是太弱了。如果你的项目基于历史原因只能构建在Backbone之上，那么借鉴Angular1.X才是你的出路。React或者Vue已经和传统MVC框架相差太多。

下面通过一个accordin控件来介绍如何使用Backbone来构建弱指令。

这个accordin控件的样子

我们先来看看一种经常被使用的错误的构建方式：
在我们的页面中有一个accordin，所以我们直接就把accordin的代码放在这个页面中
这是HTML模板business.tpl
```
<...其他和accrodin无关的html片段...>
 <div id="accordin">
    <div class="row_box J-row-box">
        <span class="name font2 important-color-2">Accordin1</span>
        <span name="triangle" class="down_triangle ">&#xe6d9;</span>
        <span class="val font2 normal-color-1">小标题</span>
    </div>
    <div name="detail_box" class="detail_list font6 normal-color-1 border-top">
        <div>这是内容</div>        
    </div>
  </div>
  <...其他和accrodin无关的html片段...>
```
这是view文件
```
define('js/business',[
 'text!templates/business.tpl'
],function(TPL){
  var view = Backbone.View.extend({
    initialize: function(options,config) {
      this.render();
    },
    render: function() {
      self.$el.html(tpl(TOP)(data));
    },
    events: {
       'click .J-row-box': 'toggleDetailBox',
    },
    toggleDetailBox : function(e) {
       var $target = $(e.currentTarget).find('span[name="triangle"]').eq(0);
       var $detailBox = $($($target).parent()).parent().find('div[name="detail_box"]');
       if ($target.length < 1) {
            return;
       }

       if ($target.hasClass('down_triangle')) {
           // 显示detail_box
          $target.removeClass('down_triangle').addClass('up_triangle');
          $detailBox.show();
        } else {
         // 隐藏detail_box
         $target.removeClass('up_triangle').addClass('down_triangle');
         $detailBox.hide();
       }
    },
  });
  return view;
})
```
如果我们这个页面有两个accordin呢？按照上面的思路，我们很容易就写出这样的代码
```
<...其他和accrodin无关的html片段...>
 <div id="accordin">
    <div class="row_box J-row-box">
        <span class="name font2 important-color-2">标题1</span>
        <span name="triangle" class="down_triangle ">&#xe6d9;</span>
        <span class="val font2 normal-color-1">小标题1</span>
    </div>
    <div name="detail_box" class="detail_list font6 normal-color-1 border-top">
        <div>这是内容</div>        
    </div>
  </div>
  <...其他和accrodin无关的html片段...>
  <div id="accordin">
    <div class="row_box J-row-box">
        <span class="name font2 important-color-2">标题2</span>
        <span name="triangle" class="down_triangle ">&#xe6d9;</span>
        <span class="val font2 normal-color-1">小标题2</span>
    </div>
    <div name="detail_box" class="detail_list font6 normal-color-1 border-top">
        <div>这是内容</div>        
    </div>
  </div>
```

如果有10个accordin呢？我们就..............玩完 :(

这种思路最大的问题就在于不懂得代码复用。可能有同学要说，可是我复用了啊！你看我把accrodin相关的HTML代码复用了10次！

复用和复制？
复用：用一段代码处理相似的逻辑
复制：用多段相似的代码处理相似的逻辑





