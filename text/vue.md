# Vue.js 
每个Vue应用都需要通过实例化Vue来实现。  
```javascript
var vm = new Vue({
  // 选项
})
```  
```javascript
<div id="vue_det">
    <h1>site : {{site}}</h1>
    <h1>url : {{url}}</h1>
    <h1>{{details()}}</h1>
</div>
<script type="text/javascript">
    var vm = new Vue({
        el: '#vue_det',
        data: {
            site: "菜鸟教程",
            url: "www.runoob.com",
            alexa: "10000"
        },
        methods: {
            details: function() {
                return  this.site + " - 学的不仅是技术，更是梦想！";
            }
        }
    })
</script>
```  
Vue构造器中有一个`el`参数,他是DOM元素中的`id`。  
**data**用于定义属性。  
**methods**用于定义的函数，可以通过return来返回函数值。  
`{{ }}`用来输出对象属性和函数返回值。  
```javascript
<div id="vue_det">
    <h1>site : {{site}}</h1>
    <h1>url : {{url}}</h1>
    <h1>{{details()}}</h1>
</div>
```  
当一个Vue实例创建时