# BackboneDirective 用Backbone构建可复用的弱指令

Backbone虽然是一个较为古老并且使用繁琐的MVC框架，但是使用Backbone依然可以写出复用性较高的弱指令。属性Angular的同学可能对指令的概念并不陌生。
简单说来指令就是对HTML语义的扩展。Angular的指令解析可以大致分为两步

* Compiler找出HTML中Angular定义的新的指令比如 ng-accordin
* 用自己定义的真实HTML片段去替换ng-accordin指令，将其解析为浏览器可以理解的语义片段。比如"\<div class="accordin"\>...\</div\>"
