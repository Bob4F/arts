## Algorithm:整数反转
给出一个 32 位的有符号整数，你需要将这个整数中每位上的数字进行反转。(请注意反转后32位整数溢出)
demo
```
输入 120
返回 21

输入 -321
返回 -123

输入 347
返回 743
```

解决代码:
```java
public int reverse(int x) {
    long temp = 0;// 防溢出     
     while(x!=0){
         // 反转
        temp = (temp*10) + x % 10;
        x=x/10;
    }
    // 溢出判断
    if(temp>Integer.MAX_VALUE|| temp<Integer.MIN_VALUE){
        return 0;
    }
    return (int)temp;
}
```


## Review : How TDD Changed the Way I approach software development

原文地址：https://medium.com/better-programming/how-tdd-changed-the-way-i-approach-software-development-38509263f9ec    

这篇文章让我对 TDD 有一个较为清晰的概念,TDD 适用于周期较长的项目，因为测试代码会增加前期的开发工作，但是对于长周期项目而言，多出来的测试开发时间相比较 TDD 带来的好处是可以接受的，TDD 能够控制**业务代码的平均实现时间和 新功能导致的Bug百分比**几乎保持不变
## Share : 分享blog
本周我分享的是上面我阅读的文章吗，并进行了翻译  
blog地址: https://juejin.im/post/5d6a3028e51d4561de20b626
## Tip: Vue 上遇到的问题 注意Vue下data的命名 
 
本周我第一次学习使用Vue,开发过程中遇见了Select标签无法更新选择的值,下面是我的代码
```html
<div>
	<label>机型</label>
    <select v-model="bianma"  v-on:change="checkjixing">
    	<option   v-for ="item in jixing" :value='item.bianma' v-model="item.bianma" >
				{{item.bianma}}
		</option>
	</select>
</div>
```
vue对象内方法
```javascript
// 获取数据的数据
getJixing : function(){
	var that = this;
	app.get('index.php?c=api&m=jixing',function(res){
		if(res.status=="1"){
            // 机型是data下的数组
			that.jixing = res.msg;
		}
	});
},
// 选择的时候 会将option的值放到select代表的data对象中
// 这一步其实是没必要的，后面我在事件中没有绑定该方法
checkjixing:function(data){
	var that = this;
	that.bianma = data.target.value;
	console.log("bianma"+that.bianma);
}
```
传过来的json数据
```json
{
    "msg":[
        {
            "id":"6",
            "bianma":"\u5c0f\u591a\u80541\u62d64"
        },
        {
            "id":"2",
            "bianma":""
        }
    ]
}
```
那么上面我们通过vue循环的option标签，html就会变成下面这样  
```html
<div>
	<label>机型</label>
    <select v-model="bianma"  v-on:change="checkjixing">
    		<option   value="\u5c0f\u591a\u80541\u62d64" v-model="bianma">
				\u5c0f\u591a\u80541\u62d64
			</option>
            <option  value="\u5c0f\u591a\u80541\u62d64" v-model="bianma" >
				BM001
			</option>
	</select>
</div>
```
这select和option输入绑定了一样的data，导致了我一直获取不到select的值。。是我一开始没看到，修改下select绑定的模型就好了,搞了我好久

```html
<div>
	<label>机型</label>
	<select v-model="bianmaname">
		<option   v-for ="item in jixing" :value='item.bianma' >
			{{item.bianma}}
		</option>
	</select>
</div>

```
