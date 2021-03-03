**三、Web API**

JS基础知识，是用来规定语法的（ECMA262标准），JS Web API是网页操作的API（W3C标准），前者是后者的基础，两者结合才能真正的实际应用。

**1、DOM**

DOM的本质：DOM的本质是从HTML文件解析出的一颗树。

**（1）获取DOM节点**

document.getElementById('div1'); //获取元素 document.getElementsByTagName('div') //获取元素集合 document.getElementsByClassName('.container') //根据classname来获取集合 document.querySelectorAll('p') //通过css选择器来获取集合

**（2）attribute**

// attribute const pList = document.querySelectorAll('p') const p1 = pList[0] p1.setAttribute('data-name', 'imooc') p1.setAttribute('style', 'font-size: 50px;')

**（3）property**

是使用js属性操作的一种形式，通过以js对象属性的形式来操作它里边的一些像style、className、nodeName之类。

// property 形式 const pList = document.querySelectorAll('p') const p1 = pList[0] p1.style.width = '100px' p1.className = 'red'

**（4）总结**

- property：修改对象属性，不会体现到html结构中
- attribute：修改html属性，会改变html结构
- 两者都有可能引起DOM重新渲染

**（5）结构操作**

新增/插入节点

const div1 =document.getElementById('div1'); const p1 = document.createElement('p'); p1.innerHTML = 'this is p1'; const p2 = document.getElementById('p2'); div1.appendChild(p2);

获取子元素列表，获取父元素

const div1 = document.getElementById('div1'); const child = div1.childNodes const div1 = document.getElementById('div1'); const parent = div1.parentNodes

删除元素

const div1 = document.getElement('div1'); const child = div1.childNodes div1.removeChild(child[0])

**（6）DOM性能**

DOM的操作非常”昂贵“，为避免频繁的DOM操作，对DOM查询做缓存将频繁操作改为一次性操作。

const list = document.getElementById('list') // 创建一个文档片段，此时还没有插入到 DOM 结构中 const frag = document.createDocumentFragment() for (let i = 0; i < 20; i++) {   const li = document.createElement('li');   li.innerHTML = `List item ${i}`;   // 先插入文档片段中   frag.appendChild(li) } // 都完成之后，再统一插入到 DOM 结构中 list.appendChild(frag) console.log(list)

**2、BOM**

**（1）navigator/screen**

//navigator const ua = navigator.userAgent; const isChrome = ua.indexOf('Chrome') console.log(isChrome) //screen console.log(screen.width); console.log(screen.height);

**（2）location/history**

//location console.log(location.href); console.log(location.protocol);//'http:''https:' console.log(location.pathname);//'/learn/199' console.log(loaction.search) console.log(loaction.hash) //history history.back(); history.forward();

**3、Ajax** 

**（1）手写一个Ajax请求**

//get请求 const x = new XMLHttpRquest(); //创建对象 x.open('get','url') //初始化函数 x.send()  //发送数据 x.onreadystatechange = function(){    if(x.readyState === 4){        if(x.status>= 200 && x.status<= 300){            console.log(x.response)        }    } }

**（2）状态值**

状态值是存储的XMLHttpRequest的状态，从0到4变化。

0：（为初始化）还没有调用send方法；

1：（载入）已经调用send方法，正在发送请求；

2：（载入完成）send方法执行完成，已经接受到全部响应内容；

3：（交互）正在解析响应内容；

4：（完成）响应内容解析完成，可以在客户端调用；

**（3）同源策略**

同源策略：最早由网景公司提出，是浏览器的一种安全策略，同源是指：同协议、同域名、同端口号，违背同源策略就是跨域。

**（4）跨域**

ajax请求时，浏览器要求当前网页和server端必须同源，即必须同协议、同域名、同端口，三者必须一致。违背同源策略就是跨域，所有的跨域都必须经过server端允许和配合，未经允许就实现跨域，说明浏览器有漏洞是危险信号。跨域的主要方法有以下两种：

1、JSONP（前端解决跨域问题）：

原理：在网页中有一些标签天生具有跨域的能力，比如img、link、iframe、script。JSONP利用了script标签可绕过跨域限制，且由于服务器端可任意的动态拼接数据返回的性质，使用script标签来获得跨域数据。

$.ajax({    url:'http://localhost:8800/x.json',    dataType:'jsonp',    jsonpCallback:'callback',    success:function(data){        console.log(data);    } })

2、CORS（服务器端解决跨域问题）：

原理：CORS是通过设置一个响应头来告诉浏览器，该请求允许跨域，浏览器收到该响应以后就会对响应放行。

response.setHeader("Access-Control-Allow-Origin","http://localhost:8011"); response.setHeader("Access-Control-Allow-Origin","X-Requested-With"); response.setHeader("Access-Control-Allow-Origin","PUT,POST,GET,DELETE,OPTIONS"); //接收跨域cookie response.setHeader("Access-Control-Allow-Origin","http://localhost:8011");

**4、事件绑定**

//通用的绑定函数 function bindEvent(elem,type,fn){    elem.addEventListener(type,fn); } const a= document.getElementById('link1'); bindEvent(a,'click',e=>{    e.preventDefault() //阻止默认行为    alert('clicked'); })

**5、事件冒泡**

<body>     <div id="div1">         <p id="p1">激活</p>         <p id="p2">取消</p>                 <p id="p3">取消</p>         <p id="p4">取消</p>     </div>     <div id="div2">         <p id="p5">取消</p>         <p id="p6">取消</p>     </div>     <script>         const p1 = document.getElementById('p1');         const body = document.body         bindEvent(p1,'click',e=>{             e.stopPropagation();             alert("激活");         })         bindEvent(body,'click',e=>{             alert('取消');         })     </script> </body>

**6、事件代理**

我们需要把事件绑定到一些元素上，但由于该类元素数量太多或者结果比较复杂，不好挨个绑定事件时，我们就把它绑定到它的父元素上，在父元素上获取它的触发元素并判断然后做出相应的处理，对于该类操作我们称为事件代理。事件代理的优点是可以使代码简洁、减少浏览器的内存占用。

<div id="div1">     <a href='#'>a1</a>     <a href='#'>a2</a>     <a href='#'>a3</a>     <a href='#'>a4</a> </div> <button>点击增加一个a标签</button> const div1=document.getElementById('div1'); div1.addEventListener('click',e=>{     const target = e.target     if(e.nodeName === 'A'){         alert(target.innerHTML);     } })

**7、存储**

**（1）cookie**

本身是用于浏览器和server通讯的，被“借用”到本地存储来使用，可通过document.cookie = '....'来修改。重复设置后同样的key的cookies值会被覆盖，不同的key值不会被覆盖,浏览器会将赋值的值存起来，除非清除Cookie，不然会一直存在。

缺点：

- 存储大小，最大存4KB；
- http请求时需要发送到服务端，增加请求数据量；
- 只能用document.cookie = '{KEY}={Value}'来修改，太过简陋

**（2）localStorage和sessionStorage**

HTML5专门为存储而设计的，最大可存储5M；API简单易用setItem、getItem；不会随着http请求被发送出去。

localStorage.setItem(key,value); //将value存储到key字段。 var b1 = localStorage.getItem(key); //获取指定key本地存储的值。 sessionStorage.setItem("sb","1");  var b2 = sessionStorage.getItem("sb");  

区别：

- localStorage数据会永久存储，除非代码或手动删除；
- sessionStorage数据只存储于当前会话，浏览器关闭则清空
- 一般用localStorage会更多一些