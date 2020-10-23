* Vue基本语法

1. {{}} 插值表达式，插值表达式只能用于普通文本

2. 参数说明

```javascript
var app = new Vue({
        el:"#app",
        data:{
            message:"信息化台账"
        }
    });
```

​		el:挂载点

​		data:数据对象

3. vue的作用范围

   ​	Vue会话管理el选项命中的元素及其内部的后代元素

4. 是否可以设置使用其他选择器

   ​	id、class、标签等选择器均可，建议使用id选择器（id选择器唯一，不会造成语义不清）

5. 可以选择什么样的dom元素作为挂载点

   ​	除了body、html外的双标签元素，建议用div作为挂在点（因为其他元素会有默认样式）

* Vue指令

1. 内容绑定，事件绑定

   * v-text 设置标签的文本值

   ```javascript
   <h5 v-text="message">714</h5> //直接替换
   <h5>{{message}}714</h5> //插值
   ```

   * v-html
     1. 设置元素的innerHTML
     2. 会解析html标签
     3. v-text 只解析为文本
     
   * v-on

     1. 为元素绑定事件
     2. 事件名不需要写on
     3. 指令可以简写为@
     4. 绑带那个的方法定义再methods中
     
     ```javascript
     <div id="von">
          <input type="button" value="v-on" v-on:click="functionName"/>
          <input type="button" value="v-on简写" @click="functionName"/>
     </div>
     var von=new Vue({
         el:"#von",
         methods:{
             functionName(){
               alert("测试");
             }
         }
     })
     ```
     
   * v-show

     根据表达式的真假，切换元素的显示隐藏

     1. 原理：修改元素的display实现显示隐藏
     2. 指令后面的内容，最终解析为布尔表达式
     3. 值为true显示 false隐藏

     ```html
     <div id="vshow">
         <button @click="changeShow">显示图标</button>
         <image src="http://www.bluewave.com.cn/template/91/4752.png" v-show="isShow"></image>
     </div>
     ```

     ```javascript
     var vshow = new Vue({
         el:"#vshow",
         data:{
             isShow:false
         },
         methods:{
             changeShow:function(){
                 this.isShow=!this.isShow;
             }
         }
     })
     ```

   * v-if 

     根据表达式的真假切换元素的显示状态

     1. 原理是通过操纵dom元素切换显示状态
     2. 表达式为true元素存在dom树中，表达式false从dom树中删除
     3. 频繁切换v-show,反之使用v-if，前者消耗小

   * v-bind

     为元素绑定属性

     1. 完整写法v-bind:属性名
     2. 简写省略v-bind,只保留 :属性名
     3. 需要动态的增删class建议使用对象的方式

     

     

     

     

     

     

