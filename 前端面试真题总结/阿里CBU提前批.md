## 一、笔试题

### 1、对象扁平化

说明：请实现 flatten(input) 函数，input 为一个 javascript 对象（Object 或者 Array），返回值为扁平化后的结果。
示例：

```javascript
  var input = {
      a: 1,
      b: [ 1, 2, { c: true }, [ 3 ] ],
      d: { e: 2, f: 3 },
      g: null,
    }
    var output = flatten(input);
  output如下
    {
      "a": 1,
      "b[0]": 1,
      "b[1]": 2,
      "b[2].c": true,
      "b[3][0]": 3,
      "d.e": 2,
      "d.f": 3,
      // "g": null,  值为null或者undefined，丢弃
   }
  function flatten(input) {
    /** 代码实现 */
  }  
```

代码实现：

```javascript
function flattern(input){
    var res = {};
    function recurse(cur,prop){
        //判断的时候要特别注意，一定要先判断是否为数组，再判断是否为对象，否者得到的结果会不一样，因为在js里面数组也是Object
         if(Array.isArray(cur)){
             var l = cur.length;
             if(l===0){
                 res[prop] = [];
             }
             for(var i=0;i<l;i++){
                 recurse(cur[i],`${prop}[${i}]`)
             } 
         }else if(cur instanceof Object){
            for(p in cur){
                recurse(cur[p],prop?`${prop}.${p}`:p);
            }
         }else{
             res[prop] = cur; 
         }
    }
    recurse(input,"");
    return res;
}
```

### 2、原子的数量（简化版）

> 根据表达式计算字母数。
> 说明：
> 		给定一个描述字母数量的表达式，计算表达式里的每个字母实际数量
> 表达式格式：
> 		字母紧跟表示次数的数字，如 A2B3
>     	括号可将表达式局部分组后跟上数字，(A2)2B
>     	数字为1时可缺省，如 AB3。
> 示例：
>   	countOfLetters('A2B3'); // { A: 2, B: 3 }
>   	countOfLetters('A(A3B)2'); // { A: 7, B: 2 }
>  	 countOfLetters('C4(A(A3B)2)2'); // { A: 14, B: 4, C: 4 }
>
> function countOfLetters(letters, res) {
>   		/** 代码实现 */
> }

解题思路：

采用栈的思想，每遇到一个左括号就把一个空数组压入栈，之后遇到右括号之前，所有新元素都放入这个空数组，遇到右括号就弹栈，并把被弹出栈的数组里面的元素放入倒数第二个数组里。

```javascript
function countOfLetters(letter) {
    var ifUpper = a => a >= 'A' && a <= 'Z';
    var ifNum = a => a >= '0' && a <= '9';
    var getAtoms = index => {
        let atom = formula[index];
        index ++;
        while (ifLower(formula[index])) {
            atom += formula[index];
            index ++;
        }
        return atom;
    };
    var getNums = index => {
        let num = '';
        while (ifNum(formula[index])) {
            num += formula[index];
            index ++;
        }
        return num;
    };
    var stack = [];
    stack.push([]);
    var map = new Map();
    let i = 0;
    while (i < formula.length) {
        if (ifUpper(formula[i])) {
            let atom = getAtoms(i);
            i += atom.length;
            let obj = {};
            obj['name'] = atom;
            if (ifNum(formula[i])) {
                let num = getNums(i);
                i += num.length;
                obj['value'] = Number(num);
            } else {
                obj['value'] = 1;
            }
            stack[stack.length-1].push(obj);
        }
        else if (formula[i] === '(') {
            stack.push([]);
            i ++;
        }
        else if (formula[i] === ')') {
            i ++;
            let multi = getNums(i);
           
            i += multi.length;
            let left = stack[stack.length-2];
            let right = stack[stack.length-1];
            for (let j = 0; j < right.length; ++j) {
                let obj = {};
                obj['value'] = multi * right[j].value;
                obj['name'] = right[j].name;
                left.push(obj);
            }
            stack.pop();
        }
    }
    let obj = {};
    for (let i = 0; i < stack[0].length; ++i) {
        obj[stack[0][i].name] = 0;
    }
    for (let i = 0; i < stack[0].length; ++i) {
        obj[stack[0][i].name] += stack[0][i].value;
    }
    return obj;
};
```

原题连接：https://leetcode-cn.com/problems/number-of-atoms/ 726. 原子的数量

### 3、

> 实现一个`Foo`方法，接受函数`func`和时间`wait`，返回一个新函数，新函数即时连续多次执行，但也只限制在`wait`的时间执行一次。
> function Foo(func, wait) {
>   /* 代码实现 */
> }

```javascript
 function Foo(func,wait){
  	let timer = null;
    return function(){
        if(timer) return;
        timer = setTimeout(()=>{
            func.apply(this,arguments);
            timer = null;
        },wait)
    }
}
```

## 二、电话面试

先是1h笔试。五分钟后面试官打来电话

1. 先说刚才三个题的思路

2. 开始聊天

3. 你是怎么学习前端的？

4. 有没有参与开源作品的经历？热爱分享？平常怎么分享？分享哪些内容?

5. 我看你项目是小组长？你是怎么做这个小组长的？

   （我答了技术上和非技术上的）

6. 现在的项目有几个人？为什么是你当小组长（我说因为我研二可以带领研一+我沟通能力好）？

7. 遇到了哪些问题？如何解决的？

   （我答了webpack，git，babel，less-loader简答的）

8. 我看你项目写的是Vue，有没有了解过这个框架的深层原理和怎么设计的？

   我：跟着官网看过文档，了解一些MVVM，响应式的原理，源码还没有看过

9. 有没有用过react？

10. 在了解Vue原理的时候有没有想过他的哪些架构更适合于你业务上哪些具体的问题？

11. 有没有了解过loader？（具体问题没听清

12. 除了阿里应该还投了其他的吧？

