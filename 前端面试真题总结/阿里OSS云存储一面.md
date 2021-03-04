## 阿里OSS云存储一面 3.2 

#### 1、笔试题：

> //编写一个函数计算 localStorage中的单个item存储的字符串的最大长度 ，
>
> Cookie的单个最大存储容量是4KB，localStrage和sessionStorage的单个最大存储容量是5M
>
> // localStorage.setItem('key', str), 即求字符串str的最大长度。

```javascript
var str = "1"
var low = 1;
var high = 6*1024*1024;//最大大约5M
var flag = 0;////标志是否超出
var testMax = fuction(){
  while(low<=high){
  	const mid = Math.floor((low+high)/2);
    var testStr = str.reapeat(mid); //将字符串重复mid次
    test();
    if(flag = 1){
    	high = mid-1;
    }else{
    	low = mid+1;
    }else{
    	return mid;
    }
  } 
}
  //是否超出
  var test = function(){
  	try{
    	window.localStorage.remove('key');
      	add(teststr)
      	window.localStorage.setItem('key',str);
        flag = 0;
    }catch(e){
    	alert("Max length:"+teststr.length);
      	clearInterval(test);
        flag =1;
    }
  };
var res;
(function(){
  setIterval(function(){
  	res = testMax();
  },1)
)();  
console.log("Max length:"+res);
  // //不断尝试
  // var test = setInterval(function(){
  // 	try{
  //   	window.localStorage.remove('key');
  //     	add(teststr)
  //     	window.localStorage.setItem('key',str);
  //       flag = 0;
  //   }catch(e){
  //   	alert("Max length:"+teststr.length);
  //     	clearInterval(test);
  //       flag =1;
  //   }
  // },1);
}
testMax();
```

#### 2、电话面试：

20min

1、自我介绍

2、两个项目分别介绍

3、第一个项目，构建项目？怎么构建的？技术选型和架构都是你做的吗？

4、我写了"主导项目的需求落地与进度把控，协同用户明确产品优化方向"，你是怎么与用户沟通的？

5、平常知道哪些ES6特性？用过哪些？



6、项目跨域用的是什么，回答了个CROS和JSONP，面试官无追问，问”你还知道哪些跨域方式"



7、babel-loader、bable-core和bable-preset-env有什么区别？



8、异步方案讲一下？Promise请求可以取消吗？ajax请求可以取消吗？？？



9、Vue3.0 了解吗？有哪些新特性知道吗？

​	

10、你的这个项目有pc端，还有小程序端，那你是怎么做自适应布局的？