# JS、react、antd、css笔记

## js笔记

=========【原生js】

#### (1)跳转页面:

利用js在新窗口打开新页面window.open("https://www.baidu.com/","_blank"); 
不在新窗口window.location.href = "https://www.baidu.com/"



#### (2)创建超链接绑定点击事件触发点击

·情形1新建超链接

let eleLink = document.createElement('a');//制作超链接
eleLink.href = "javascript:;"
document.body.appendChild(eleLink);//添加到页面

eleLink.click();// 触发点击

·情形2

goto=()=>{
window.location.href = "https://www.baidu.com"
}
document.getElementById("haha").onclick = goto;//绑定点击方法的声明，注:所以这里方法名不加括号
document.getElementById("haha").click();// 触发点击

#### (3)判断数组

let a = [];
console.log(Array.isArray(a));
console.log(a instanceof Array);

#### (4)对对象数组进行[深拷贝]

先JSON.stringify()再JSON.parse()
即JSON.parse(JSON.stringify(目标数组))
补充：诸如 Map, Set, RegExp, Date, ArrayBuffer 和其他内置类型在进行序列化时会丢失

其他：对常规数组“深层遍历”（下面两例依然是浅拷贝）
let a = [[1,1,1],[2,2,2],[3,3,3]];let b = a;
let c = Object.assign([],a);
let d = [...a];
a[0]=2;
console.log(a[0]);console.log(b[0]);console.log(c[0])
console.log(d[0]);

```js
let a = [1,2,3,4]

let b =[...a];

console.log(a)

b[2]=0
console.log(a)
console.log(b)

let a1 = [{a:1},{a:2},{a:3}];//这种程度复杂度的也可以用...进行深层遍历
let b1 = [...a1];
b1[2] = {a:0};
console.log(JSON.stringify(a1))
console.log(JSON.stringify(b1))
```

#### (5.1）es6中的...

【使用1】作为“rest[剩余参数](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Functions/Rest_parameters)”

如果函数的最后一个命名参数以`...`为前缀，则它将成为一个由剩余参数组成的真数组，其中从`0下标`到`.length-1`的元素由传递给函数的实际参数提供。

——剩余参数和 `arguments`对象的区别主要有三个：

- 剩余参数只包含那些没有对应形参的实参，而 `arguments` 对象包含了传给函数的所有实参。
- `arguments`对象不是一个真正的数组，而剩余参数是真正的 `Array`实例，也就是说你能够在它上面直接使用所有的数组方法，比如 `sort`，`map`，`forEach`或`pop`。
- `arguments`对象还有一些附加的属性 。

【使用2】"spread扩展运算符"（[阮一峰](https://es6.ruanyifeng.com/#docs/array#%E6%89%A9%E5%B1%95%E8%BF%90%E7%AE%97%E7%AC%A6)），也叫"展开语法"（[mdn](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Operators/Spread_syntax)）

**提示:** 实际上, 展开语法和 [`Object.assign()`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Object/assign) 行为一致, 执行的都是浅拷贝(新/旧数组、对象改变影响旧/新数组、对象)

https://blog.csdn.net/qq_35306736/article/details/109174394 中的结论“**扩展运算符、concat、slice 均仅对第一层的基本数据实现深拷贝**”



#### (5.2)“apply拼接数组”、"sort与filter--处理数组"

【part.1】

[push()方法](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array/push)将一个或多个元素添加到数组的末尾，并返回该数组的新长度。

(如果是数组仅仅会作为一项，要么手动输入里面的每个元素作为多个参数传入，要么用解构)

```js
var arr = ['tom', 'jerry'];
var arr2 = [1, 2];

arr.push.apply(arr, arr2);
console.log(arr)
// ["tom", "jerry", 1, 2]
```

【part.2】

初次使用到js里面数组的sort方法，和filter方法，
sort对json数组进行排序,filter对json数组进行过滤(判断逻辑来决定是否返回)并返回新数组

```js
errMSG_arr = errMSG_arr.sort((arr1,arr2)=>parseInt(arr1.line)-parseInt(arr2.line));

arr1 = arr1.filter(function (val) { return arr2.indexOf(val) > -1 })
```



·【但如果——想从数组里返回部分键来改造json，就这样写】

```js
let data = [[{"name":"毕慧国","id":"000004","editing":{}},{"name":"胡志刚","id":"000009","editing":{}},{"name":"陈鸿宇","id":"000005","editing":{}},{"name":"刘磊","id":"000020","editing":{}},{"name":"汪洋","id":"000016","editing":{}}],[{"name":"张佳宇","id":"000015","editing":{}},{"name":"刘强","id":"000003","editing":{}},{"name":"鹏鹏","id":"000008","editing":{}},{"name":"汪洋","id":"000016","editing":{}},{"name":"翁军年","id":"000018","editing":{}},{"name":"陈亮","id":"000002","editing":{}},{"name":"焦第","id":"000019","editing":{}},{"name":"方庆华","id":"000010","editing":{}},{"name":"郑怀奇","id":"000017","editing":{}},{"name":"谭建本","id":"000007","editing":{}}]]
let newdata = []
data.map((item,index)=>{
    item = item.map((itemitem,itemindex)=>{//留下部分键并改造json
        return {"员工编号":itemitem.id,"姓名":itemitem.name}
    })
    newdata.push(item)
})

console.log(JSON.stringify(newdata));


let a = [1,2,3,4]

console.log(a.filter((val)=>val>2));//通过返回布尔值选择留下的元素
```



****

#### (6)token用法设计——自总结

【猜测】
前端：输入用户名和密码->后端：根据用户名和密码请求数据，根据用户名和密码生成token返回给前端。

前端：获取token，保存在localstorage如果得到新的token就替换旧的，
进行其他需要通过登录验证的操作并发送token->后端接收token，解析token，根据用户名和密码进行用户效验，
然后进行具体操作。

【疑问，一个用户一个密钥吗】然后失效的话，就是把密钥定期更新，导致前端的旧版token无法被后端解析，
无法解析的话就重新提示用户名和密码来用新密钥生成新token

#### (7)关于isNaN

当我们向isNaN传递一个参数，它的本意是通过Number()方法尝试转换参数的类型为Number，
如果可以转换为Number则返回false，否则转返回true，它只是判断这个参数能否转成数字而已，并不是判断是否严格等于NaN。

所以当你要判断某个值是否严格等于NaN时无法使用isNaN()方法，毕竟你传递任意字符串它都会返回true。
ES6中提供了一个Number.isNaN()方法用于判断一个值是否严格等于NaN：
Number.isNaN(NaN)//true

#### (8)Math.floor与parseInt在取整时的差别

Math.floor(value)和parseInt(value)在value都为正数时，都是直接去小数位取整。
如果value===-1.6,Math.floor(-1.6)===-2,parseInt(-1.6)===-1

#### (9)将数组变成指定字符串join、字符串变指定数组split、对字符串做正则

searchkey = searchkey.replace(/\s*/g,"");//去除所有空格

[在线测试正则](https://regex101.com/)

其中\s用于匹配空白字符，

\s*：匹配0个或多个空格，会尽可能多的匹配
\s*?：匹配最小数量的空格，也就是0个空格



let a = a.join("、")	//将数组用顿号分割输出成字符串

let b = b.split("#")		//按照#号分割成数组并返回分割后的数组
let c = c.replace(/-/g,"");			//拿掉所有的“-”
let d= d.replace(/(.*)、/, '$1');	//拿掉最后一处顿号
let e = e.replace('and ', '');	//这里是拿掉第1个的 and+空格，因为只匹配第一次

去除首尾逗号的两个写法(第一个是错的！)：
写法一:自己抄的两次replace  【不行！！！如果最后一位没有逗号会强行找个逗号删】
let a = ",1,2,3,"
a = a.replace(/(.*),/, '$1');
a = a.replace(/,(.*)/, '$1');
写法二：韩彬提供的一次replace
let a = ",1,2,3,"
a = a.replace(/(^\,*)|(\,*$)/g, "")

#### (10) js两种遍历对象的键方法-以及判断obj是否有某个键

let obj = {a:"1",b:"2",c:"3"}
for (let key in obj){
	console.log(key);
	console.log(obj[key]) }
Object.keys(obj).forEach((value,index)=>{
    console.log(value)
    console.log(obj[value])  })

【用hasOwnProperty判断是否有某个键】

let smsTemplateInfo = JSON.parse(sessionStorage.getItem('smsTemplateInfo'));

​    if(smsTemplateInfo && smsTemplateInfo.hasOwnProperty("smstemplateId") && smsTemplateInfo.hasOwnProperty("mode")){

​      this.setState({smsTemplateInfo})

​    }

#### (11)对json数组中某个字符串的值进行中文首字母排序

解释一下，sort()里面定义了一个函数来指定排序的规则，localeCompare()方法返回一个数字，
指示引用字符串是在排序顺序之前还是之后，或者与排序顺序中的给定字符串相同，
zh-Hans-CN是简体中文的排序规则，sensotivity 是灵敏度，包括 base、accent、case、variant这几种灵敏度。
参考连接：https://www.cnblogs.com/zxhyJack/p/10045786.html
MDN介绍：https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/localeCompare

#### (12)moment生成给人看的时间，与moment对象生成时间戳以及时间戳变回时间
```
let time1 = moment(new Date()).format('YYYY-MM-DD HH:mm:ss');//生成给人看的时间
let time2 = moment(new Date(), 'YYYY-MM-DD HH:mm:ss');//生成moment时间对象
let timeNumber = moment(new Date(), 'YYYY-MM-DD HH:mm:ss').valueOf();//先生成moment时间对象再生成时间戳
let time3 = moment(new Date(Number(item.stopTime))).format('YYYY-MM-DD HH:mm:ss')//时间戳转Date转moment

let time3 = moment(Number(1573455094000)).format('YYYY-MM-DD HH:mm:ss')//直接时间戳不加new Date也行

直接转换 moment ( timeString,'YYYY-MM-DD HH:mm:ss')
```

#### (12.5)原生js的Date生成给人看的时间以及生成时间戳和时间戳变回时间

时间转时间戳：

let timedate = new Date()

- 1、var timestamp1 = timedate.valueOf();
- 2、var timestamp2 = timedate.getTime();
- 3、var timetamp3 = Number(timedate) ;

let a = new Date(timestamp1);//时间戳变为date对象

```javascript
let str =  handleDatetime(new Date()); //自定义处理方法handleDatetime对date对象格式化

let handleDatetime = (datetime)=>{//传入时间对象返回格式化时间

    let year = datetime.getFullYear();
    let month = datetime.getMonth() + 1;
    let day = datetime.getDate();

    function timeAdd0(str) {
     	if(str.length<=1){str='0'+str;}
    	return str;
    }
   	let Hours = ""+datetime.getHours();Hours = timeAdd0(Hours);
   	let Minutes = ""+datetime.getMinutes();Minutes = timeAdd0(Minutes);
   	let Seconds = ""+datetime.getSeconds();Seconds = timeAdd0(Seconds);
   	let str = `${year}年${month}月${day}日 ${Hours}:${Minutes}:${Seconds}`;
   return str;
  }
```



#### (13)url编码(urlencode直译为百分号编码)

js全局对象里的	encodeURIComponent

encodeURIComponent() 函数可把字符串作为 URI 组件进行编码。(转了汉字、特殊符号等)

比如 let str = 'a&b'

console.log(encodeURIComponent(str)) // a%26b

而**decodeURIComponent** 用于解码

#### (14)js判断字符串是否全是中文

let str="es123"

var reg = /^[\u4E00-\u9FA5]+$/; 

if(!reg.test(str)){ 

​    console.log("no"); //不全是中文

}else{

​    console.log("yes"); //全是中文

}

#### (15)js中key键名相关-以及-Symbol

·是否有某个键：``

————————————————

·遍历键名Object.keys() 、Reflect.ownKeys() 以及 for in获得所有键名

其中

·**for...in**可以遍历可枚举的对象，包括不是它本身但存在于原型链上的属性。for in更适合遍历对象，不要使用for in遍历数组。

for in遍历的是数组的索引（即键名），而**for of**遍历的是数组元素值

1. **for in**遍历出除属性名为`d`以外的所有可枚举属性，包括其原型链上的属性
2. **Object.keys**方法会返回一个由对象的自身可枚举属性名(key)组成的数组，其原型链上的属性没有被包含
3. **Object.getOwnPropertyNames**方法会返回一个由对象的自身所有属性名(key)组成的数组，包括可枚举和不可枚举的属性
4. **Object.values**方法会返回一个由对象的自身可枚举属性的值(value)组成的数组
5. **Object.entries**方法会返回一个由对象的自身可枚举属性的键值对(key和value)组成的数组

for in会循环所有可枚举的属性，包括对象原型链上的属性，循环会输出循环对象的key，如果循环的是一个数组则会输出下标索引(index)。

```js
  let myObj = { name: 'pujie', age: 18 }
  let tempArr = Object.keys(myObj)//可以用Object.keys来获取键名，获取的结果是一个数组。【方法一】
  console.log(tempArr)

  for (let key in myObj) {//for in遍历的是数组的索引（即键名），而for of遍历的是数组元素值
    console.log(key)//使用for in则可以循环遍历得到键名【方法二】
    console.log(myObj[key])
  }


Reflect.ownKeys(myObj)//
```

————————————————

·用Symbol当键名

【注1:】symbol是不可枚举的，也就是说，for....of、for..in、Object.keys()是不能遍历到symbol类型的值的，要得到symbol的值有两种办法，一种是使用Object.getOwnPropertySymbols,返回一个数组包括这个对象中所有的symbol类型的键名，另一种是使用Reflect.ownKeys,可以同时返回常规类型的键名和symbol类型的键名。

【注2:】symbol创建的时候也可以传入参数，但这仅仅是他们的标识符而已，即使标识符相同这两个symbol也并不是相同的。如果需要读取这个标识符，可以使用改Symbol变量来调description

```js
// 定义
        let str1 = Symbol();
        let str2 = Symbol();
        console.log(str1 === str2);//false
        console.log(typeof str1);//symbol
        // 描述
        let str3 = Symbol('name');
        let str4 = Symbol('name');
        console.log(str3 === str4);//false
        // 对象的属性名
        const obj = {};
        // obj.name = 'a111';
        // obj.name = 'b222';
        // console.log(obj); // b222
        obj[Symbol('name')] = 'a111';
        obj[Symbol('name')] = 'b222';
        obj["test1"] = 'c333';
		obj["test2"] = Symbol('_(:з」∠)_');
        console.log(obj); //{Symbol(name): "a111", Symbol(name): "b222"}
 
console.log(Object.getOwnPropertySymbols(obj))//返回一个数组包括这个对象中所有的symbol类型的键
console.log(Reflect.ownKeys(obj))//返回一个对象中所有键，无论是不是symbol类型

//【推测】实际使用中 是“声明就用”  而不是 “想法子取”
console.log("\n====\n")
let s = Symbol("desc");
console.log(s);
console.log(Symbol('desc').toString());
console.log(Symbol('desc').description);//这里期望的值是desc，而此编译器出了问题
console.log("-=-=-=-=-=-=-=-=-=-");
//就这个例子是成功设了Symbol类型的键名
let a = {}; a[s]="a1";
console.log(a);console.log(a.s);console.log(a[s]);console.log(a["s"]);

console.log("\n====\n")

let b = {}; b.s="b1";
console.log(b);console.log(b.s);console.log(b[s]);console.log(b["s"]);

console.log("\n====\n")

let c = {}; c["s"]="c1";
console.log(c);console.log(c.s);console.log(c[s]);console.log(c["s"]);

console.log("\n====\n")

let d = {s:"d1"};
console.log(d);console.log(d.s);console.log(d[s]);console.log(d["s"]);

console.log("\n====\n")

```

#### (16)js中为false的6个值，以及佐证if(a)等效于if(!!a),以及**[仅仅null == undefined 是true]**

```js
let a = [NaN,null,undefined,0,"",false]
//注意，6个布尔转换为false的值之间仅仅null与undefined之间不完全等于，
console.log(null == undefined)//true
console.log(typeof null)//"object"
console.log(typeof undefined)//undefined

a.map((item,index)=>{

    console.log(`第${index+1}次===`)

    if(!!item){

        console.log(`双引号执行了`)

    }

    if(item){

        console.log(`无引号执行了`)

    }

    console.log(`\n`)

})
```

#### (17)可以用toString和valueOf输出Boolean对象

let createboolean: boolean = true;

console.log(createboolean);//true

let createBoolean: Boolean = new Boolean(0);//通过new使用构造函数 `Boolean` 创造的对象**不是**布尔值而是布尔对象



console.log(createBoolean.toString())//"false"

console.log(createBoolean.valueOf())//false

#### (18)[深入|重温]apply与call，以及bind还有阮一峰ES6中提到的对数组的操作

```js
function foo(){}

foo.call(this, arg1,arg2,arg3) == foo.apply(this, arguments) == this.foo(arg1, arg2, arg3);

let a = [1,2,3]
let b = [];
let c = [];

b.push.apply(b,a)
c.push.call(c,a,b)

console.log(b)//1，2，3
console.log(c)//1，2，3，1,2,3

//===============================
function active(fn) {
  fn(); // 真实调用者，为独立调用
}

var a = 20;
var obj = {
  a: 10,
  getA: foo
}
function foo() {
  console.log(this.a)
}

active(obj.getA);//20
//虽然看起来是obj调用的getA，但实际上是外面还有一层active
```

关于多个bind的问题

参考https://segmentfault.com/q/1010000010235712

和https://segmentfault.com/q/1010000010258226

衍生出一则讨论this的简书https://www.jianshu.com/p/d647aa6d1ae6

#### (19)【待·深入】js数组去重

参考链接https://segmentfault.com/a/1190000018705199?utm_source=tag-newest

#### (20)Obj对象去重

方法1

*Object.assign*

```js
let a ={t1:"1",t2:"2"}
let b ={t2:"2-2",t3:"3"}

console.log(Object.assign(a,b))
```

#### (21)批量修改jsonarray的键名key

```js
var array = [{id:1,name:"小明"},{id:2,name:"小红"}];
//改成 [{value:1,label:"小明"},{value:2,label:"小红"}]
var result = array.map(o=>{return{value:o.id, label:o.name}});
console.log(result);
```

#### (22)moment拓展知识(比如隔了多少个月/天)

获取当天0点0时0分的数据————加个.startOf('day')，如果是23:59:59则是*endOf*

获取月初的数据————加个.startOf('month')

```js
const t = '2019-10-17';
const dateFormat = 'YYYY-MM-DD';

使用moment.js获取当前月份1号

const m = moment().startOf('month').format("YYYY-MM-DD");  // "2019-10-01"

获取当前月份上一个月的1号

const afterM = moment(t, dateFormat).subtract(1, 'months').startOf('month').format('YYYY-MM-DD');
console.log(afterM)
获取当前月份下一个月的1号
const beforeM = moment(t, dateFormat).add(1, 'months').startOf('month').format('YYYY-MM-DD');
console.log(beforeM)

获取当前时间的下一个小时 （当前时间 9:22）
moment(new Date()).minute(0).add(1, 'hour').format('HH:mm'); // "10:00"
moment(new Date()).minute(0).add(2, 'hour').format('HH:mm'); // "11:00"

实际上前推3个月的代码还要加上一天
<DatePicker format={"YYYY-MM-DD"}
                            //模式3时间选择框
                            style={{width:300}}
                            disabledDate={(current)=>current&&current<moment().subtract(3, 'months').subtract(1, 'days')}
                            disabled={this.state.allDisabled===true||record.recordSign==="空"?true:false}
                            value={(text instanceof Array)?moment(text[0],"YYYY-MM-DD"):Boolean(text)===false?null:moment(text,"YYYY-MM-DD")}
                            onChange={(date,dateString)=>this.recordValueChange("3",record.recordId,dateString)}
                            />
正如上面的current<moment().subtract(3, 'months').subtract(1, 'days')

换成3个月前且月初就current<moment().subtract(3, 'months').startOf('month')
```

#### (23)三个常记混的数组操作函数splice、slice、split

slice->翻译为切开，用处：截取。结果：不改变原数组，返回修改后的新数组

```js
/*
	slice(start,end)
	注意： slice() 方法不会改变原始数组
参数:
start:开始位置的索引,end:结束位置的索引(但不包含该索引位置的元素)
*/
let arr= ['a','b','c','d'];
let newArr = arr.slice(0,3)//['a','b','d']从下标为0截取到下标为3但不包括下标3
let newArr = arr.slice(0)//['a','b','c','d']没有第二个参数，则截取到最后一个元素
```

splice->翻译为拼接，用处:删除、插入。结果：改变原数组，返回由被删除的元素组成的一个数组:如果只删除了一个元素，则返回只包含一个元素的数组。如果没有删除元素，则返回空数组。

```js
/*删除的功能
	splice(index,count)
参数:
index:开始位置的索引,count:要删除元素的个数,
返回:返回的是包含被删除元素的数组对象
*/
let arr= ['a','b','c','d'];
let newArr = arr.splice(1,2)//从下标1开始包括下标1删除2个元素即c、d
console.log(newArr)//['b','c']返回的是包含被删除元素的数组
console.log(arr)//['a','d']修改了原数组

/*删除的功能
	splice(index,howmany,item1,.....,itemX)
参数:
·index
必需。规定从何处添加/删除元素。
该参数是开始插入和（或）删除的数组元素的下标，必须是数字。
·howmany
可选。规定应该删除多少元素。必须是数字，但可以是 "0"。
如果未规定此参数，则删除从 index 开始到原数组结尾的所有元素。
·item1, ..., itemX
可选。要添加到数组的新元素
*/
var fruits = ["Banana", "Orange", "Apple", "Mango"];//移除数组的第三个元素，并在数组第三个位置添加新元素
fruits.splice(2,1,"Lemon","Kiwi");//Banana,Orange,Lemon,Kiwi,Mango

let arr= ['a','b','c','d'];
let newArr = arr.splice(2,0,'张三');//第二个参数为0则只插不删
console.log(arr)//['a','b','张三','c','d']
```

split->翻译为分割，用处:把字符串分割成字符串数组。结果：不改变原数组，返回被指定字符创分割后的数组字符串

```js
/*
split() 方法用于把一个字符串分割成字符串数组。
提示： 如果把空字符串 ("") 用作 separator，那么 stringObject 中的每个字符之间都会被分割。
注意： split() 方法不改变原始字符串

string.split(separator,limit)
参数：
·separato
可选。字符串或正则表达式，从该参数指定的地方分割 string Object。
·limit
可选。该参数可指定返回的数组的最大长度。如果设置了该参数，返回的子串不会多于这个参数指定的数组。如果没有设置该参数，整个字符串都会被分割，不考虑它的长度。

*/
var str="How are you doing today?";
var n=str.split(" ",3);//How,are,you
```



#### (24)关于es6的类

实例的属性除非 显示 的定义在其本身（即定义在**this对象**上），

​							否则都视为定义在**原型prototype上（即class**上）

### (25)可选链	?.	与	空值合并运算符	??

见自己的博客https://blog.csdn.net/qq_35306736/article/details/105659284

两个的优先级都比!要低，

所以可以用成这样

```typescript
if (!res.data?.data)//是对res.data?.data的结果的布尔化取反
```

其他使用技巧收集：

```javascript
//可选链作为布尔判断时?.的判断结果影响整个判断，导致整个返回undefined即a?.b的打印出来是undefined
let a = {
    b:1
}

if(a?.b){
    console.log("进来了1")
}else{
    console.log("进来了2")
}
//上述打印了true分支，a?.b值为a.b即1

a = 2;

if(a?.b){
    console.log("进来了1")
}else{
    console.log("进来了2")
}
//上述打印了false分支，a?.b值为undefined
```



### (26) some函数检测是否至少一个元素通过测试

https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array/some

有一个目标数组`["apple","banana","orange"]` ，我想检查其他数组是否包含任何目标数组元素。

```javascript
const array = [1, 2, 3, 4, 5];

// checks whether an element is even
const even = (element) => element % 2 === 0;

console.log(array.some(even));
// expected output: true
```



### (27)中文首字母排序

```typescript
typeSelect.sort((a, b) => a.label.localeCompare(b.label, 'zh-Hans-CN', { sensitivity: 'accent' }));
//条件类型根据汉字首字母排序

```

### (28)数组的reduce方法与reducer函数

https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array/reduce



reduce()方法对数组中的每个元素执行所提供的reducer函数(升序执行)，将结果汇总为单个返回值。

```javascript
/*=========================reducer函数接受4个参数:
Accumulator(简称acc，累计器)、
Current Value(简称cur，当前值)、
Current Index(简称idx，当前索引)、
Source Array(简称src,源数组)
*/

//示例1：
const array1 = [1, 2, 3, 4];
const reducer = (accumulator, currentValue) => accumulator + currentValue;

// 1 + 2 + 3 + 4
console.log(array1.reduce(reducer));
// expected output: 10

// 5 + 1 + 2 + 3 + 4
console.log(array1.reduce(reducer, 5));
// expected output: 15


/*====================数组的reduce方法:
arr.reduce(callback(accumulator, currentValue[, index[, array]])[, initialValue])
·关于initalValue:
注意：如果没有提供initialValue，reduce 会从索引1的地方开始执行 callback 方法，跳过第一个索引。如果提供initialValue，从索引0开始。
如果数组为空且没有提供initialValue，会抛出TypeError 。如果数组仅有一个元素（无论位置如何）并且没有提供initialValue， 或者有提供initialValue但是数组为空，那么此唯一值将被返回并且callback不会被执行。

·关于callback：执行数组中每个值 (如果没有提供 initialValue则第一个值除外)的函数，包含四个参数：
=====accumulator
	累计器累计回调的返回值; 它是上一次调用回调时返回的累积值，或initialValue（见于下方）。
=====currentValue
	数组中正在处理的元素。
=====index 
	|可选：数组中正在处理的当前元素的索引。 如果提供了initialValue，则起始索引号为0，否则从索引1起始。
=====array	
	|可选：调用reduce()的数组
·关于initialValue：
|可选：作为第一次调用 callback函数时的第一个参数的值。 如果没有提供初始值，则将使用数组中的第一个元素。 在没有初始值的空数组上调用 reduce 将报错

*/
```

### (29) for...of和for...in区别

for...of遍历的是可迭代对象iterable,无法遍历对象，特别的，可以用for...in去遍历对象

如上，获取键名可以Object.keys( )或者for...in

### (30) js中的const

```javascript
class Book {
    constructor(title, pages, isbn) {
        this.title = title;
        this.pages = pages;
        this.isbn = isbn;
    }
}

let book1 = new Book("测试书名", 99, true);
const PI = book1.title;//这个时候常量PI的值为"测试书名"
console.log(book1.title);
console.log(PI);
console.log(">>>>>>>>>>>>>>>>>>>>>>>>>>>>");
book1.title = '新的测试书名';
console.log(book1.title)//"新的测试书名"
console.log(PI);//"测试书名"
//虽然book1.title的值变成了"新的测试书名",但常量PI的值仍然为"测试书名"
console.log(">>>>>>>>>>>>>>>>>>>>>>>>>>>>");
const Demo2 = {
    a: "a"
};
Demo2.b = "b";//给声明时的对象里面添加属性是可以的
console.log(Demo2);
Demo2.a = "1";//给声明时的对象里面修改属性的值也是可以的
console.log(Demo2);
Demo2 = {c:"c"};//【这里会出错，不能指定新的对象引用】
```

### (31）es6的属性存取器setter和getter



```javascript
class Person {
  constructor(name1) {
    this.name1 = name1;
    this._name= name1;
  }

  get name() {
    return this._name;
  }

  set name(value) {
    this._name = value;
  }

  get name2() {
//setter与getter命名不能与变量同名，否则会无限递归，此处用了_name2
    return this._name2;
  }

  set name2(val) {
    this._name2 = val;
  }

}

let p1 =new Person('我是name1也是name');
console.log(p1.name);//打印_name默认为name1
p1.name = "我_name变了";
console.log(p1.name);//打印新的_name;
p1.name2 = "我是name2的初值，记住setter是用=来复制的不是方法调用";//name2没有初值需要先"setter"才能"getter"
console.log(p1.name2);
```

### (32)push()、shift()与pop()、unshift()对数组首尾进行操作 + 任意位置使用splice

一、**push()方法**

作用:将一个或多个元素添加到数组的**末尾**，并返回该数组的新长度。

返回:当调用该方法时，新的length属性值将被返回。

二、**shift()方法**

作用:从数组中删除**第一个**元素，并返回该元素的值。此方法更改数组的长度。

返回:从数组中删除的元素; 如果数组为空则返回undefined。

三、**pop()方法**

作用:从数组中删除**最后一个元素**，并返回该元素的值。此方法更改数组的长度。

返回:从数组中删除的元素(当数组为空时返回undefined)。

四、**unshift()方法**

作用:将一个或多个元素添加到数组的开头，并返回该数组的新长度(该方法修改原有数组)。

返回:当一个对象调用该方法时，返回其 length 属性值。

五、补充:

**splice()方法**

作用:在任意位置添加或删除元素

返回:

语法:

```javascript
array.splice(start[, deleteCount[, item1[, item2[, ...]]]])
```

参数:由被删除的元素组成的一个数组。如果只删除了一个元素，则返回只包含一个元素的数组。如果没有删除元素，则返回空数组


`start`


指定修改的开始位置（从0计数）。如果超出了数组的长度，则从数组末尾开始添加内容；如果是负值，则表示从数组末位开始的第几位（从-1计数，这意味着-n是倒数第n个元素并且等价于`array.length-n`）；如果负数的绝对值大于数组的长度，则表示开始位置为第0位。

`deleteCount` 可选

整数，表示要移除的数组元素的个数。

如果 `deleteCount` 大于 `start` 之后的元素的总数，则从 `start` 后面的元素都将被删除（含第 `start` 位）。

如果 `deleteCount` 被省略了，或者它的值大于等于`array.length - start`(也就是说，如果它大于或者等于`start`之后的所有元素的数量)，那么`start`之后数组的所有元素都会被删除。

如果 `deleteCount` 是 0 或者负数，则不移除元素。这种情况下，至少应添加一个新元素。

`item1, item2, *...*` 可选

要添加进数组的元素,从`start` 位置开始。如果不指定，则 `splice()` 将只删除数组元素。

### (33)js常用[数组方法]汇总参考

|     方法      |                             描述                             |
| :-----------: | :----------------------------------------------------------: |
|   `concat`    |                连接2个或更多数组，并返回结果                 |
|    `every`    | 对数组中的每个元素运行给定函数，如果该函数对每个元素都返回`true`，则返回`true` |
|   `filter`    | 对数组中的每个元素运行给定函数，返回该函数会返回`true`的元素组成的数组 |
|   `forEach`   |      对数组中的每个元素运行给定函数。这个方法没有返回值      |
|    `join`     |               将所有的数组元素连接成一个字符串               |
|   `indexOf`   | 返回第一个与给定参数相等的数组元素的索引，没有找到则返回`-1` |
| `lastIndexOf` |   返回在数组中搜索到的与给定参数相等的元素的索引里最大的值   |
|     `map`     | 对数组中的每个元素运行给定函数，返回每次函数调用的结果组成的数组 |
|   `reverse`   | 颠倒数组中元素的顺序，原先第一个元素现在变成最后一个，同样原先的最后一个元素变成了现在的第一个 |
|    `slice`    |    传入索引值，将数组里对应索引范围内的元素作为新数组返回    |
|    `some`     | 对数组中的每个元素运行给定函数，如果任一元素返回`true`，则返回`true` |
|    `sort`     |  按照字母顺序对数组排序，支持传入指定排序方法的函数作为参数  |
|  `toString`   |                     将数组作为字符串返回                     |
|   `valueOf`   |            和`toString`类似，将数组作为字符串返回            |

========================

关于**reduce**参照（28）的介绍

====================

关于[Array.prototype.entries()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array/entries)

注：此处区分于[Object.entries()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Object/entries)

```javascript
let a = [
"1","2","3","4"
];

let aEntries = a.entries();
console.log(aEntries);//[object Array Iterator]
console.log(JSON.stringify(aEntries));{}
console.log(aEntries.next().value);//[0,'1']位置0的值为1
console.log(aEntries.next().value);//[1,'2']
console.log(aEntries.next().value);//[2,'3']
console.log("====");
let index3 = aEntries.next();

console.log(JSON.stringify(index3));// {"value":[3,"4"],"done":false}
console.log("位置为"+index3.value[0]);//位置为3
console.log("值为"+index3.value[1]);//值为4
let index4 = aEntries.next();
console.log(JSON.stringify(index3));//{"value":[3,"4"],"done":false}
console.log(JSON.stringify(index4));//{"done":true}
let index5 = aEntries.next();
let str="";
for(let key in index5){
str+=(key+" ")
}
console.log(str)//"value done"
console.log(str.value===undefined)//true

```

关于**keys**和**values**

```javascript
let a = [
"1","2","3","4"
];
const aKeys = a.keys();
console.log(aKeys)// [object Array Iterator]
//如果done属性的值为false，就意味着还有可迭代的值。
console.log(JSON.stringify(aKeys.next()))//{value:0,done:false}
console.log(JSON.stringify(aKeys.next()))//{value:1,done:false}
console.log(JSON.stringify(aKeys.next()))//{value:2,done:false}
console.log(JSON.stringify(aKeys.next()))//{value:3,done:false}
console.log(JSON.stringify(aKeys.next()))//{value:undefined,done:false}

const aValues = a.values();
console.log(aValues)// [object Array Iterator]
console.log(JSON.stringify(aValues.next()))//{value:"1",done:false}
console.log(JSON.stringify(aValues.next()))//{value:"2",done:false}
console.log(JSON.stringify(aValues.next()))//{value:"3",done:false}
console.log(JSON.stringify(aValues.next()))//{value:"4",done:false}
console.log(JSON.stringify(aValues.next()))//{value:undefined,done:false}
```

关于**Array.from**和**Array.of**

```javascript
let a = [
"1","2","3","4"
];

let a2 = Array.from(a);
a[0]="5"
//至少可以用来深拷贝一个简单数组
console.log(a)// 5,2,3,4
console.log(a2)// 1,2,3,4

//也可以传来一个用来过滤的函数
let events = Array.from(a,item=>(item%2==0))
console.log(events)// false,true,false,true
let like_filter = Array.from(a,item=>{
    if(item%2==0)return item
})
console.log(JSON.stringify(like_filter))//[null,"2",null,4]

//Array.of方法根据传入的参数创建一个新数组
let numbers3 = Array.of(1,2,3);//等同于下面
let numbers4 = [1,2,3];
console.log(numbers3)// 1,2,3
console.log(numbers4)// 1,2,3
console.log(numbers3.toString() == numbers4.toString())//true

//同样可以用Array.of结合"展开运算符..."来对简单数组进行深拷贝
let a3 = Array.of(...a2);
a2[a2.length-1]="5"
console.log(a2)// 1,2,3,5
```

关于**fill**和**copyWithin**

【使用fill进行数组初始化/格式化】

```javascript
//介绍
//使用【fill方法】用静态值进行填充（设置的头尾参数包头不包尾,包尾得不设尾或者设-1）;
//使用【copyWithin方法】复制数组内部元素到同一个数组指定位置（设置的头尾参数包头不包尾,包尾得不设尾或者设-1）

let numbrsCopy = Array.of(1,2,3,4,5,6)
console.log(numbrsCopy)
numbrsCopy.fill(0)//数组所有值填充为0
console.log(numbrsCopy)
numbrsCopy.fill(1,0,1);//填充1从位置零到位置一，不包括位置一
console.log(numbrsCopy);
numbrsCopy.fill(2,1,3);//填充2从位置一到位置三，不包括位置三
console.log(numbrsCopy);
let ones = Array(6).fill(8);//创建一个长度为6值都为8的数组

console.log("=======")
let copyArray = [1,2,3,4,5,6];
//下面相当于copyArray.copyWithin(0,3,-1);
copyArray.copyWithin(0,3);//将位置三到结尾的元素(4,5,6)复制到位置零
console.log(copyArray);// 4,5,6,4,5,6
let copyArray2 = [1,2,3,4,5,6,7];
copyArray2.copyWithin(1,4,6);//从位置四到位置六的元素(5,6)复制到位置一，头尾不包尾
console.log(copyArray2);// 1,5,6,4,5,6,7

```

排序方法:

**reverse**反序、**sort**自定义排序；
搜索方法:
**indexOf**、**lastIndexOf**、**find**、**findIndex**、**includes**;

其他方法:
**toString**输出为字符串、**join**用指定分隔符输出为字符串

```javascript
let a = [1,2,3,10,11,12];
a.reverse();
console.log(a);//正常反序
a.sort();
console.log(a);//非正常升序排序，因为sort默认把元素认成字符串比较
a.sort((a,b)=>a-b);//正常升序排序
console.log(a);
//上面排序的完整版
a.sort((a,b)=>{
    //源码是三个判断
    if(a<b)return -1;
    if(a>b)return 1;
    if(a=b)return 0;

    /**手写简化
     * if(a>b)return true
     * else return flase
     */
})
console.log(a)
console.log("============")
//【搜索】

//  indexOf方法返回与参数匹配的第一个元素的索引，没有则会返回-1
//  lastIndexOf方法返回与参数匹配的最后一个元素的索引，没有则会返回-1

//find和findIndex方法接收一个回调函数，搜索一个满足回调函数条件的值
//  find方法返回第一个满足条件的值，没有则返回undefined
//  findIndex方法返回该值在数组的索引，没有则返回-1
console.log("第一个是3倍数的值为"+a.find((item,index,array)=>{
    if(item%3===0)return true;else return false;
}))
console.log("第一个是3倍数的值的索引为"+a.findIndex((item,index,array)=>{
    if(item%3===0)return true;else return false;
}))

//如果数组里存在指定元素，includes方法返回true，否则返回false
console.log(a.includes(12))
console.log(a.includes(15))
//如果给includes方法传入一个起始索引，将从索引指定位置开始(不包头)
console.log(a.includes(1))
console.log(a.includes(0,1))//第一个参数配置的起始索引不被包括在搜索反复内

console.log(a.toString())
console.log(a.join("~"))//输出为指定分隔符分隔的字符串
```




### (34) 关于es6中的symbol

参考mdn链接:https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Symbol

symbol 是ES6新增的一种基本数据类型 

**（7种基本数据类型:Number、String、Boolean、Object、null、undefined、Symbol）**。

Symbol()函数会返回symbol类型的值，该类型具有静态属性和静态方法。它的静态属性会暴露几个内建的成员对象；它的静态方法会暴露全局的symbol注册，且类似于内建对象类，但作为构造函数来说它并不完整，**因为它不支持语法：`new Symbol()`。**

每个从`Symbol()`返回的symbol值都是唯一的。**一个symbol值能作为对象属性的标识符；这是该数据类型仅有的目的**。



```javascript
//Symbol 作为对象属性名时不能用.运算符，要用方括号。因为.运算符后面是字符串，所以取到的是字符串 sy 属性，而不是 Symbol 值 sy 属性。

let sy = Symbol("key1");
let syObject = {};
syObject[sy] = "kk";
 
syObject[sy];  // "kk"
syObject.sy;   // undefined
```

[`Symbol.for(key)`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Symbol/for)

使用给定的key搜索现有的symbol，如果找到则返回该symbol。否则将使用给定的key在全局symbol注册表中创建一个新的symbol。

[`Symbol.keyFor(sym)`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Symbol/keyFor)

从全局symbol注册表中，为给定的symbol检索一个共享的?symbol key。

=============================================================

**es6中内建好的symbol可供使用：**

除了自己创建的symbol，JavaScript还内建了一些在ECMAScript 5 之前没有暴露给开发者的symbol，它们代表了内部语言行为。它们可以使用以下属性访问：

【1】迭代相关的

[`Symbol.iterator`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Symbol/iterator)一个返回一个对象默认迭代器的方法。被 [for...of](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Statements/for...of) 使用。

Symbol.asyncIterator 
一个返回对象默认的异步迭代器的方法。被 for await of 使用。
正则表达式 symbols
Symbol.match
一个用于对字符串进行匹配的方法，也用于确定一个对象是否可以作为正则表达式使用。被 String.prototype.match() 使用。
Symbol.replace
一个替换匹配字符串的子串的方法. 被 String.prototype.replace() 使用。
Symbol.search
一个返回一个字符串中与正则表达式相匹配的索引的方法。被String.prototype.search() 使用。
Symbol.split
一个在匹配正则表达式的索引处拆分一个字符串的方法.。被 String.prototype.split() 使用。

【2】其他

Symbol.hasInstance
一个确定一个构造器对象识别的对象是否为它的实例的方法。被 instanceof 使用。
Symbol.isConcatSpreadable
一个布尔值，表明一个对象是否应该flattened为它的数组元素。被 Array.prototype.concat() 使用。
Symbol.unscopables
拥有和继承属性名的一个对象的值被排除在与环境绑定的相关对象外。
Symbol.species
一个用于创建派生对象的构造器函数。
Symbol.toPrimitive
一个将对象转化为基本数据类型的方法。
Symbol.toStringTag
用于对象的默认描述的字符串值。被 Object.prototype.toString() 使用。



### (35)数组简易技巧

```javascript
//普通对象的去重
let a1 = {key1:"key1",key2:"key2",key2:"key2",key3:"key3",key3:"key3"};
let b1 = Object.assign({},a1)
console.log(JSON.stringify(b1));



//普通数组的去重

let a2 = [1,2,2,3,3,4,4,5];
let b2 = Array.from(new Set(a2));
console.log(b2)
let c2 = [...new Set(a2)];
console.log(c2)
//Array.from的.map式用法
let a3 = [
    {key:"1",name:"甲"},
    {key:"2",name:"乙"},
    {key:"3",name:"丙"},
    {key:"4",name:"丁"},
    ];

let b3 = Array.from(a3,({name})=>name);//简写
let c3 = Array.from(a3,(item)=>item.name);//展开
console.log(b3)
console.log(c3)

//快捷清空数组
a3.length = 0;

//快捷将数组转为类数组对象
a2 = {...a2};
console.log(JSON.stringify(a2));


//展开运算符可以替代concat拼接数组
let a4=[1,2];
let b4=[3,4];
console.log([...a4,...b4])

//求两个数组的交集--先去重再遍历其中一个，遍历时判断每个元素看是否存在于另一个
let a5 = [0,2,4,6,8,8];
let b5 = [1,2,3,4,5,6];
//
let c5 = [...new Set(a5)].filter((item)=>b5.includes(item));
console.log(c5);

//简易从数组中删除虚值
let a6 = [0,'blue','',NaN,9,true,undefined,false];
let b6 = a6.filter((item)=>item)
    //相当于(item)=>{if(Boolean(item))return true;else return false;}
console.log(b6)

//从数组中获取随机值--基于数组长度取随机值下标索引即可
let c6 = a5[(Math.floor(Math.random()*(a5.length)))];
console.log(c6)

//对数组所有值进行求和——结合reduce
let sum5 = a5.reduce((x,y)=>x+y);
console.log(sum5)

```



### (36)js中“函数的四种方法“，以及“类的三种方法“

https://blog.csdn.net/qq_35306736/article/details/109180252

#### 【函数】中四种方法：

##### **1.内部方法（类似私有方法，因为我很少见到这种叫法，所以此处用类似）:**

定义在函数内部的方法，内部方法只能被内部的方法调用。

##### **2.实例方法（也叫对象方法）:**

在构造函数中this指向的是他的实例对象，函数定义时，定义函数中最外层this上方法便是实例方法，只有实例才能调用。

##### **3.原型方法:**

在prototype上添加的方法，既可以通过构造函数的原型链去调用，

也可以实例去调用

##### **4.静态方法（也叫类方法）:**

是直接在构造函数上定义的方法，不能被实例调用，构造函数可以调用

```javascript
//js中函数的四种方法
function Father(){
    console.log("创建了一个实例")
 
    function neibu(){
        console.log("~~我是Father函数的【内部方法】") 
    }
 
 
    this.shili2 = function(){         
        console.log("~~我是Father函数的【实例方法/对象方法】(写法2)");
    }
 
    console.log("===下面打印函数中的this对象：");
    console.log(this); //this上有实例方法
    console.log("------------");
	neibu(); 
    
}
 
 
Father.prototype.yuanxing2 = function(){
    console.log("~~我是Father函数的【原型方法】(写法2)");
}
 
 
Father.jingtai2 = function(){
    console.log("~~我是Father函数的【静态方法/类方法】(写法2)");
}
 
 
let fa = new Father();
console.log("下面打印原型链："); 
console.log(Father.prototype);//原型链上有原型方法
console.log("------------");
fa.yuanxing2(); //[用法1]用一个实例去调用
Father.prototype.yuanxing2()//[用法2]通过原型链调用
Father.jingtai2();
fa.shili2();
```

#### 【类】中三种方法：

##### 1.**实例方法（也叫对象方法）**

定义在构造函数constructor中的this对象上

##### **2.原型方法**

直接写在类中的方法，与constructor方法同级

##### **3.静态方法（也叫类方法）**

通过static关键字定义。

```javascript
class Demo{
    //类中，构造函数里的this指向的是他的实例对象
    constructor(){
        this.shili1=()=>{//需要实例对象来调用
        console.log("~~类中的【实例方法/对象方法】(写法1)");
        }
    }
    static jingtai1(){
        console.log("~~类中的【静态方法/类方法】(写法1)");
    } 
 
    yuanxing1(){//【特别注意】这儿的是原型方法
        console.log("~~类中的【原型方法】(写法1)") 
    }
 
}
 
let demo = new Demo();
Demo.jingtai1();
demo.shili1()
demo.yuanxing1()//通过实例调用
Demo.prototype.yuanxing1()//通过原型调用
```

### (37)立即执行的匿名async函数  （读作：啊sing课）

(async ()=>{//写点语句

})()



### (38)识别htmlstring中的`<br/> <br/>` ,以及快捷去[全局空格]和[首尾空格]`<p></p>`

添加样式white-space:"pre-line" ，并将`<br/> <br/>`替换为\n

```
<p style={{whiteSpace:"pre-line"}}>八六回答:{item[2].replace(/<br\S?>/g, "\n")}</p>
```

```
.replace(/\s+/g, "").replace(/^(<p>)?/g, "").replace(/(<\/p>)?$/g, "")
//先去空格.replace(/\s+/g, "")，然后如果有就去除开头的<p>,如果有就去除尾部的</p>
```

### (39)js自定义方法判断是否是数组

```js
            function isArray(obj) {
                return Object.prototype.toString.call(obj) === '[object Array]';
            }
```



### (40）axios如何链式调用

```js
function test(){
	axios.post(GLOBAL_URL.addTag, { 参数 })
                .then(res => {
                    res = res.data;
                    if (res.code === "200") {
                        //注意下面的这句return
                        return axios.post(GLOBAL_URL.updateTarcileMidTag, { 参数})
                    } else {

                    }
                })
                .then(res => {
                    if (res) {//这里要加个if(res)
                        res = res.data;
                        if (res.code === "200") {

                        } else {
                            
                        }
                    }


                })
}

```

### (41)一些生成对照数组、傀儡数组、映射对象的个人技巧

【1】比如拼接一二级评论，二级评论有个`second_Parent`对应一级标题的id，

那就遍历一级评论时生成id为key，一级评论的index位置为value的映射对象

```js
let first = xxx.map((item, index) => {
                        let newitem = {
                            ……
                            child: []//这个用来拼二级数组
                        }
                        //下一步是生成映射对象
                        firstId_index[item.first_id] = index;
                        return newitem;
                    });
 let second = xxx;
second.map(item=>{
    first[firstId_index[item.second_Parent]].child.push({
        ……
    })
})
```

【2】用新的标记去控制源数据，但是又不想直接动源数据的数组--使用傀儡数组/对照数组

```js
let dataSource = xxx;
//这里是填充了一个false，也可以填充别的标记
let dataShow = new Array(dataSource.length).fill(false)
```

### (42)不该变原数组给数组添加元素

```
let articleTags = [xxxx]
articleTags.slice().push([moment().valueOf(), value]);
其中articleTags.slice()复制原数组
但然如果articleTags不是数组就JSON.parse(JSON.stringify(articleTags))
```

### (43)字符串拼接、模板语法都不会同步其中变量的变化，除非再次赋值

图示![image-20201214140110068](https://github.com/yctjb1/front_notebook_by_devwolf/blob/master/img/image-20201214140110068.png)

### (44)简易版的解决js中计算失精

```
//确定为四位小数，放大100倍
let b= 0.0045; b = b *10000/100;//这样才不会失精

//缩小100倍，并去尾保留四位小数
Math.floor((Number(value) / 100) * 10000) / 10000;
```



### (45)JS检查是否是小数

```js
const checkIsDecimal = (a) => {
                if (a) {
                    let str = String(a);

                    let dot_index = str.indexOf(".");//获取小数点所在下表
                    let not_int_num = str.length - (dot_index + 1);//获取小数点后面的数字个数
                    if (dot_index > -1 && not_int_num > 0) {//如果有小数点并且小数点后面的数字个数不为0

                        return `${not_int_num}位小数`
                    } else {
                        return false;
                    }


                }
            };
```

### (46)moment或者date对象即使输入值也能生成对象，此时如何判断输入值非法（即如何判断是否生成了Invalid Date）

```js
//如果是Date对象，直接看他getTime()方法，看返回值是否isNaN为true
if(isNaN(moment("啊啊啊").valueOf()===true)){}
```

(47)

(48)

(49)





### react笔记

#### 【1】复习一下项目的快捷引入配置（eject暴露版）

修改快捷引用路径在config/webpack.config.js中resolve的alias里面

#### 【2】按需引入antd（未暴露脚手架配置时的react-app-rewired结合customize-cra版）

yarn add react-app-rewired customize-cra
react-app-rewired （一个对 create-react-app 进行自定义配置的社区解决方案）,
由于新的 react-app-rewired@2.x 版本的关系，你还需要安装 customize-cra。
以及需要修改 package.json 中的启动配置,等等，
详见https://ant.design/docs/react/use-with-create-react-app-cn
以及简书https://www.jianshu.com/p/1caa5b6d15c5

【其他可配置化】《ts模板下craco化配置项的antd4项目》https://blog.csdn.net/qq_35306736/article/details/107854029

#### 【3】axios写get请求时，如果不想手动把?参数=值拼到链接上

参照如下模板(注意:里面的{params:参数列表}是固定写法)
axios.get("链接",{
	params:{
	参数:值
	}
})

#### 【4】antd的文件上传，条件未知，存在无法获得文件对象的type的情形

--->解决方法:同时做一次截取文件后缀来判断文件类型的验证，给后缀一致的文件放行
let arrName = file.name.split(".")
const endNameType = (arrName[arrName.length-1] === "xls" || arrName[arrName.length-1] === "xlsx")
【补充】而antd的accept是——在弹出选择文件框时，出现一个自定义文件类型，而不是只有一个“所有文件类型”

#### 【5】element对象以及window对象的几个“窗口宽度”

scrollWidth：对象的实际内容的宽度，不包边线宽度，会随对象中内容超过可视区后而变大。 
clientWidth：对象内容的可视区的宽度，不包滚动条等边线，会随对象显示大小的变化而改变。 
offsetWidth：包括滚动条等边线，会随对象显示大小的变化而改变。【常用】
document.getElementById('root').offsetWidth
innerWidth：不加上工具条与滚动条窗口【常用】window.innerWidth

#### 【6】未知情况下的state的undefined现象 （触发条件未知）

constructor会比componentWillCount优先设置state，
“遇到了一些情况”，可能“如果是页面直接使用来判断是否显示组件的这种state，
得必须在constructor中[预定义]一下，不然会是undefined”

#### 【7】请求时禁用缓存

axios.get(url,{headers: {'Cache-Control': 'no-cache'}})
axios.get(url,{
    params: {ID: 12345}
  },{headers: {'Cache-Control': 'no-cache'}})
axios.post(url,data,{headers: {'Cache-Control': 'no-cache'}})

#### 【8】根据props重新渲染数据componentWillReceiveProps

这里特别的用的match.params.id这个，而且需要手动调用this.queryBase()这个
在componentWillMount就被调用的请求初始数据的方法。 
componentWillReceiveProps(nextProps) {
        if(this.props.match.params.id!==nextProps.match.params.id)
        this.setState({
            aim_Id:nextProps.match.params.id
        },()=> this.queryBase());
    }

#### 【9】react生命周期

constructor->componentWillMount->render->componentDidMount->componentWillUnmount
参考https://blog.csdn.net/xutongbao/article/details/82983511

#### 【10】react写计时器

sendEmail=()=>{
            this.setState({btnSendEmail: true,count:60},()=>{
                this.setTimer()//单独测一下计时器
})
setTimer = () => {
    /*此处正是定时器运用的巧妙之处，以及对定时器返回值的理解程度体现
        定时器必须在一个函数中赋值给一个属性，在state中赋值就不行，定时器会自执行,
        因此必须在一个不会自动执行的函数中把定时器ID赋值给一个变量保存
        此ID可以作为clearInterval()的参数，用于清除定时器*/
        this.timer = setInterval(()=>{
            let count = this.state.count;
            if ( count === 1) {
                this.clearTimer();
                this.setState( { btnSendEmail: false });
            } else {
                this.setState( { count: count - 1});
            }
        }, 1000)
    }
clearTimer= () =>{
        clearInterval(this.timer)
}
//如果想一旦验证成功不可再修改可以在disabled中加个前置的三目运算符判断
<Button onClick={()=>this.sendEmail()} disabled={this.state.btnSendEmail}>
            {this.state.btnSendEmail===true? `${this.state.count}秒重发`:'获取验证码'}
</Button>

#### 【11】axios:当公共的request被公共的interceptors拦截后如何单独放行某个接口

【概念】：这个封装了的公共request是一个axios实例，自己照着模板捏另一个axios实例，并在这个实例调用前，添加自定义的interceptors。

比如放行超时504就自定义10000ms(10秒)

```js
export const sendSMS = (data) => {//特化处理，放行504
    let special_axios = axios.create({timeout:10000});//自定义axios实例--超时10s
    special_axios.interceptors.response.use(function (response) {
        // 对响应数据做点什么

        return response;
      }, function (error) {
        // 对响应错误做点什么
        console.log("进入了后面")
        console.log(JSON.stringify(error))
        if(error.code=='ECONNABORTED'){   // 判断请求异常信息中是否

            return {
                data:{code:"200",data:"ok"}
                };
        }
        else{
            if(error.response){
                return error.response;
            }
            else{
                return Promise.reject(error);
            }
            
        }

        
      });
    let instance = special_axios({
        method: "post",
        url: axiosURL.sendSMS,
        data: data,
    })
    // .catch(function (err) {

    //     console.log("没有进isCancel")
    //     console.log(JSON.stringify(err))
        
    // });

    

    return instance


    // return request(axiosURL.sendSMS, {
    //     method: "post",
    //     data: data
    // });
}
```

### 【12*】文件下载，当接口返回blob数据时需要对axios做的配置以及如何触发下载

```typescript
	axios({
            method: 'post',
            data: params,
            url: url,
            headers: {
                'Content-Type': 'application/json'
            },
            responseType: 'blob'//设置响应的数据类型为一个包含二进制数据的 Blob 对象，必须设置
        }).then(res => {
            const blob = new Blob([res.data], { type: 'application/vnd.ms-excel' });
            const fileName = '导出信息.xlsx';
            if ('download' in document.createElement('a')) {// 非IE下载
                const elink = document.createElement('a');
                elink.download = fileName;//a标签的download属性规定下载文件的名称
                elink.style.display = 'none';
                elink.href = URL.createObjectURL(blob); //生成一个Blob URL
                document.body.appendChild(elink);
                elink.click();//模拟在按钮上的一次鼠标单击
                URL.revokeObjectURL(elink.href); // 释放URL 对象
                document.body.removeChild(elink);
            } else { // IE10+下载
                navigator.msSaveBlob(blob, fileName);
            }
        }).catch(error => {
            message.warning("导出异常：" + error)
        })
```

### 【13】react中的新特性——context

首先需要

```react
export const CreateTabContext = React.createContext({//这儿是defaultValue
    addNewPage:{},
});
//CreateTabContext可以写在utils文件夹之类的下面然后提供给引入
```

然后在顶级组件(传进值的地方)开启使用（比如此处传入一个方法）

```react
<CreateTabContext.Provider value={{addNewPage:this.addNewPage}}>
 </CreateTabContext.Provider>

```

最后在要使用的地方将那个组件设一下contextType来指定创建的context

```react
HomePage.contextType = CreateTabContext;//这句常看见写在export default HomePage;上面一行
//CreateTabContext需要import来从创建的地方引过来
```

至此，就可以this.context.addNewPage()来在CreateTabContext.Provider包裹的所有层级使用了。

### 【14】react16旧版生命周期

![react16旧版生命周期](E:\办公\统一笔记\react16旧版生命周期.jpg)

注：this.forceUpdate( )强制更新

### 【15】react-router中的withRouter无法结合react新特性中的context

### 【16】不全局安装脚手架的创建react项目（待核实）

```
npx create-react-app my-app
//不过，好像是下面这句
yarn create react-app react-antd-lottery --template typescript
```



### 【17】setState完整写法

https://www.cnblogs.com/libin-1/p/6725774.html

```
       this.setState((prevState, props) => ({
            count_copy: prevState.count_copy.pop()
          }),()=>{
              
          })

        
```



```react
  componentWillReceiveProps(newProps: Props) {
    if (newProps.count !== this.state.count_copy) {
      this.setState((prevState: Readonly<States>, props: Readonly<Props>) => ({
        count_copy: newProps.count
      }), () => {
        // console.log('componentWillReceiveProps--检测到props改变')
      })
    } else {
      // console.log('componentWillReceiveProps--未检测到props改变')
    }


  }
```

### 【18】创建自定义组件

官网 https://react.docschina.org/docs/jsx-in-depth.html#user-defined-components-must-be-capitalized 使用`React.createElement()`创建组件，小写字符串作为参数形如`React.createElement('div')`生成原生html组件，如果是自定义组件需要使用变量`React.createElement(Foo)`

antd4的每个Icon都是一个组件

`import { StarOutlined, StarFilled} from '@ant-design/icons';`

这里变量定义好了，可以直接`{React.createElement(StarOutlined)}`去生成所要的Icon组件

如果是未定义的变量，

“js动态生成变量”https://www.cnblogs.com/lguow/p/10212402.html

```javascript
let a;console.log(a);
console.log(window['StarOutlined']='123')//通过字符串定义一个变量（赋初值）
console.log(window['StarFilled'])//通过字符串声明一个变量未赋值
```



# antd笔记

#### {1}antd中的popver的content中不能用嵌套map

```
<Popover placement="bottom" trigger="click" content={<span>{this.a.map(item=>console.log("总之我只能用一层map"))}</span>}

	<Button type="link" size="small">查看</Button>

</Popover>
```



#### {2}不少框架的api虽然写了除了promise也接受别的返回，但出现问题还是改用promise

```
return new Promise((resolve,reject) => {

	if(xxx){resolve(); }else{reject(); }

})
```



# mirror.js笔记(mirrorx)

#### ·1、异步调用接口并存在mirrorx的Model

```js
let sms_send_info;
            let _thisprops = this.props;
            this.setState({sendBtn:true});
            let count = 0;
            const queryAfter = (item,resolve,reject) =>{
                count++;
                sms_send_info = _thisprops.sms_send_info;//试着把每次结果存在mirrorx里
                let params = {
                    backUrl:item.backUrl,
                    billId:item.billId,
                    createUser:item.createUser,
                    SignNameJson:item.templateSign,
                    TemplateCode:item.templateCode,
                    TemplateParamJson:"code"
                }
                actions.SmsPlatformModel.sendPhoneExcel(params)
                .then(res=>{

                    if(res.code==="200"){ 
                        sms_send_info.push({rowIndex:count,billId:item.billId,fileName:item.fileName,templateCode:item.templateCode,result:"发送成功"});
                        actions.SmsPlatformModel.updateState({sms_send_info});             
                            if(count===sendSelectedRows.length){resolve()}else{queryAfter(sendSelectedRows[count],resolve,reject)}
                    }else{
                        sms_send_info.push({rowIndex:count,billId:item.billId,fileName:item.fileName,templateCode:item.templateCode,result:"发送失败"}) 
                        actions.SmsPlatformModel.updateState({sms_send_info});
                            if(count===sendSelectedRows.length){resolve()}else{queryAfter(sendSelectedRows[count],resolve,reject)} 
                    }
                                  
                    
                })
            }

async function YiBu() {//异步+递归  
    await new Promise((resolve,reject) => {
        queryAfter(sendSelectedRows[0],resolve,reject)
    })
}

YiBu().then(()=>{
    message.success("全部执行完毕");
    Modal.info({
            width:'70%',
            title: '本次提交',
            content:'',
      });
    actions.SmsPlatformModel.updateState({sms_send_info:[]}); 
    this.setState({
        activePage:1, pageSize:10,sendBtn:false,
        selectedRowKeys:[],sendSelectedRows:[]
    },()=>{
        this.onSearch()
    })
})
```

#### ·2、新版mirrorx解决 dispatch之类的报错





# css笔记

## 1.点击事件如何穿透上层元素

在上层元素中设置pointer-events: none

参考链接https://www.cnblogs.com/shouke/p/12018403.html

## 2.css伪类中状态顺序link，visited，focus，hover，active

css中关于超链接的五个属性一般正常顺序为：link，visited，focus，hover，active

## 3.css实现一行文字居中，多行文字左对齐

参考https://www.cnblogs.com/weboey/p/6021793.html

思路:p设为行内块，行内块宽度会随着p内容动态变化，不足以换行时整个行内块会受到父级的text-align作用而居中造成视觉上的文字居中（实际是文字所在盒子居中，此时左对齐已经生效了，仅仅因为盒子宽度和文字一致而无法看出来）。
如果足以换行，左对齐的效果就会展现（盒子总宽被蛮行的宽所撑开）

```
//html
<div class="content">
	<p class="center_warpleft">
        我不够长居中
    </p>
</div>
<div class="content">
	<p class="center_warpleft">
        我足够长了，我要左对齐
    </p>
</div>


//css

.content {  width: 150px;
 border: 1px solid #ee2415;
 text-align: center  ;padding: 2px 5px}

/*display: inline-block使得宽度根据文字的宽度伸缩 */
.content .center_warpleft{   display: inline-block   ;text-align: left;}
```

## 4.css选择器权重:

参考https://www.cnblogs.com/zyjzz/p/10380729.html

一、选择器类型

　　**1、ID　　#id**

　　**2、class　　.class**

　　***\**\*\*\*\*\*\*\*\*\*\*\*3、标签　　p\*\*\*\*\*\*\*\*\*\*\*\*\****

　　**4、通用　　\***

　　**5、属性　　[type="text"]**

　　**6、伪类　　：hover**

　　**7、伪元素　　::first-line**

　　**8、子选择器、相邻选择器**

二、权重计算规则

1. **第一等：代表内联样式，如: style=””，权值为1000。**
2. **第二等：代表ID选择器，如：#content，权值为0100。**
3. **第三等：代表类，伪类和属性选择器，如.content，权值为0010。**
4. **第四等：代表元素选择器和伪元素选择器，如div p，权值为0001。**
5. **通配符、子选择器、相邻选择器等的。如\*、>、+,权值为0000。**
6. **继承的样式没有权值。**

## 5.常忘，也可能常用的一些css以及对应mdn链接

一、【transform】

**`transform`**属性允许你旋转，缩放，倾斜或平移给定元素。这是通过修改CSS视觉格式化模型的坐标空间来实现的。

mnd地址:https://developer.mozilla.org/zh-CN/docs/Web/CSS/transform

二、【pointer-events】

**`pointer-events`** CSS 属性指定在什么情况下 (如果有) 某个特定的图形元素可以成为鼠标事件的 [target](https://developer.mozilla.org/zh-CN/docs/Web/API/event.target)。

`值`**`none`**表示鼠标事件“穿透”该元素并且指定该元素“下面”的任何东西。

mdn链接：https://developer.mozilla.org/zh-CN/docs/Web/CSS/pointer-events

三、css选择器

mdn链接：https://developer.mozilla.org/zh-CN/docs/Learn/CSS/Building_blocks/Selectors

包括:

属性选择器——mdn参考链接https://developer.mozilla.org/zh-CN/docs/Web/CSS/Attribute_selectors

伪类选择器(提倡使用:单冒号)——mdn参考链接https://developer.mozilla.org/zh-CN/docs/Web/CSS/Pseudo-classes

伪元素选择器(提倡使用::双冒号)——mdn参考链接https://developer.mozilla.org/zh-CN/docs/Web/CSS/Pseudo-elements

四、Array.from()

**Array.from()** 方法从一个类似数组或可迭代对象创建一个新的，浅拷贝的数组实例。

mdn参考链接:https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array/from0

## 6.弧度与角度

两者转换https://www.cnblogs.com/xiejn/p/11901019.html

https://techbrood.com/h5b2a?p=html-canvas-shapes

　　弧度 = 角度*PI/180 
　　角度 = 弧度*180/PI

CanvasRenderingContext2D.rotate(angle)	这里的参数angle使用的是弧度

https://developer.mozilla.org/zh-CN/docs/Web/API/CanvasRenderingContext2D/rotate

## 7.子绝父相，父相子绝

## 7.5(题外话) 正斜杠 即是分号 比如1/2 ,反斜杠用于转义，比如\n

【路径方面】

1、”/“ 正斜杠、分号：”/“一般用于Unix系统和Windows系统中。”/“还可以用于浏览器地址栏网址访问。

2、“\“ 反斜杠：“\“一般用于DOS系统和Windows系统中。“\“不可以用于浏览器地址栏网址访问。也用于转义，比如\n

## 8.如何保留textarea中的换行和空格

`white-space: pre;`

[关于white-space的mdn](https://developer.mozilla.org/zh-CN/docs/Web/CSS/white-space)





# js数据结构与算法



# webpack笔记

#### 1.	添加对.less文件的支持

修改config/webpack.config.js

**参考https://www.jianshu.com/p/0389abafb0cb**

#### 2.	pnpm

经测试尚未支持mirrorx，所以最终没有使用pnpm来install生成node_modules,
所以还是用npm/cnpm/yarn来启动

#### 3.	windows下删除node_modules

`cnpm install rimraf -g//全局安装rimraf
rimraf node_modules`

#### 4.	react-transition-group设置组件加载和卸载时动画->还没会用

可以参考css动画的地方https://advence-liz.github.io/animate.css/

#### 5.	修改主题相关(upbuyfont项目2019年时候)

less 3+版本应用@import '~antd/dist/antd.less'可能会报错
报错参考https://github.com/ant-design/ant-design/issues/7927

在webpack.config.js中给less的配置出改成
{loader ： ' less-loader '，选项： {javascriptEnabled ： true }}修改于129行
但是less:"2.7.2"可以过不用开启上述配置

@import "~@ant-design/aliyun-theme/index.less";在App.css里
参考于https://github.com/ant-design/ant-design-aliyun-theme
注：别忘了加分号

#### 6.	sideEffects: true

是webpack4中和副作用相关的一个配置参数

#### 7.	react-addons-perf

如果你已经确定你的React应用存在性能上面的问题，可以使用这个工具来进行检测，它会给出一些帮助性的警告，让你能够了解到一些东西。
然而，Perf工具只能用来处理React应用，同时它只会显示性能最差的部分。

#### 8.	按需引入antd

`yarn add react-app-rewired customize-cra`

**react-app-rewired （一个对 create-react-app 进行自定义配置的社区解决方案）**,由于新的 react-app-rewired@2.x 版本的关系，你还需要安装 customize-cra。
 以及需要修改 package.json 中的启动配置,等等

详见https://ant.design/docs/react/use-with-create-react-app-cn
以及简书https://www.jianshu.com/p/1caa5b6d15c5

#### 9.	修改快捷引用路径[复习一下]

在config/webpack.config.js中resolve的alias里面



如果用antd推荐的那套用了create-react-app但没有执行不可逆还原配置的操作的话，如下在config.override.js配置引用路径https://blog.csdn.net/HamsterKnight/article/details/89221639

```js
const { override, fixBabelImports, addWebpackAlias } = require('customize-cra')
const path = require('path')
function resolve(dir) {
  return path.join(__dirname, '.', dir)
}
module.exports = override(
  // 配置路径别名
  addWebpackAlias({
    layout: path.resolve(__dirname, 'src/layout'),
    templates: path.resolve(__dirname, 'src/modules/templates'),
    router: path.resolve(__dirname, 'src/router'),
    utils: path.resolve(__dirname, 'src/utils'),
  }),
  // antd按需加载
  fixBabelImports('import', {
    libraryName: 'antd',
    libraryDirectory: 'es',
    style: 'css'
  })
)

```



#### 10."homepage": "./"

-------------2020/04/21
config-override.js中打包出错时(未eject暴露的create-react-app项目)，在package.json中添加
"homepage": "./"



#### 11.两套config-overrides.js代码

**因为是customize-cra在react-app-rewired基础上所以可以有customize-cra的写法**

react-app-rewired的官方文档https://github.com/timarney/react-app-rewired/blob/master/README_zh.md

customize-cra官方文档https://github.com/arackaf/customize-cra#readme

其一：react-app-rewired + customize-cra

```js
const { override, fixBabelImports, addWebpackAlias ,addLessLoader} = require('customize-cra')
const path = require('path')
const paths = require('react-scripts/config/paths');
paths.appBuild = path.join(path.dirname(paths.appBuild), 'dist');
function resolve(dir) {
  return path.join(__dirname, '.', dir)
}
module.exports = override(
  // 配置路径别名
  addWebpackAlias({
    layout: path.resolve(__dirname, 'src/layout/'),
    modules: path.resolve(__dirname, 'src/modules/'),
    templates: path.resolve(__dirname, 'src/modules/templates/'),
    mainrouter: path.resolve(__dirname, 'src/mainrouter/'),
    utils: path.resolve(__dirname, 'src/utils/'),
  }),
  // antd按需加载：根据import来打包 (使用babel-plugin-import)
  fixBabelImports('import', {
    libraryName: 'antd',
    libraryDirectory: 'es',
    style: 'css'
  }),
  //// 使用less-loader对源码重的less的变量进行重新制定，设置antd自定义主题
  //yarn add less less-loader --save-dev
  addLessLoader({
    javascriptEnabled: true,
    modifyVars:{'@primary-color':'#1DA57A'},
  })
  // ,(config)=>{ //暴露webpack的配置


      
  //   return config
  // }
)

```

其二：react-app-rewired 而没有customize-cra

```js
const { injectBabelPlugin } = require('react-app-rewired');
const rewireLess = require('react-app-rewire-less');

  module.exports = function override(config, env) {
  config = injectBabelPlugin(['import', { libraryName: 'antd', style: 'css' }], config);
  config = injectBabelPlugin(['import', { libraryName: 'antd', style: true }], config);
  config = rewireLess.withLoaderOptions({
     modifyVars: { "@primary-color": "#1DA57A" },
   })(config, env);
   // alias
config.resolve.alias = {
    ...config.resolve.alias,
    '@': resolve('src')
};
   return config;
 };
```



参考博客(以下标题仅代表个人收获):

未经核实的修改打包方法+不使用customize-crahttp://www.manongjc.com/article/77258.html

creat-react-app在配置config-overrides.js后如何修改打包路径https://blog.csdn.net/qq_38998250/article/details/103470055

不使用customize-cra的基础上配置快捷引用https://www.onlyling.com/archives/321

不使用customize-cra的基础上使用react-app-rewire-less来配置https://www.cnblogs.com/lanshu123/p/10660705.html

如何customize-cra中暴露confighttps://www.cnblogs.com/crazycode2/p/12584669.html

掘金的一则create-react-app综合问题https://juejin.im/post/5ca5bd0ee51d4564221c4cf3

## js历史

1998年，国际标准化组织（ISO）和国际电工委员会（IEC）也将ECMAScript采纳为标准（ISO/IEC-16262）。自此以后，各家浏览器均以ECMAScript作为自己JavaScript实现的依据，虽然具体实现各有不同。

**JavaScript**实现了**ECMAScript规范**，**ECMAScript规范**由**ECMA-262**定义。

（ECMAScript只是对实现这个规范描述的所有方面的一门语言的称呼。JavaScript实现了ECMAScript，而Adobe ActionScript同样也实现了ECMAScript。）
ECMA-262第4版是废弃案

es1对应ECMA-262第1版 -> es2对应ECMA-262第2版  ->es3 对应ECMA-262第3版

 ->es3.1对应ECMA-262第5版,意思是es3.1就是es5

 ->ECMA-262第6版即ES6、ES2015，发布于2015年6月

 ->ECMA-262第7版即ES7、ES2016，发布于2016年6月

 ->ECMA-262第8版即ES8、ES2017，发布于2017年6月 

->ECMA-262第9版即ES9、ES2018，发布于2018年6月 

->ECMA-262第10版即ES10、ES2019，发布于2019年6月

【补充：2008年7月ECMAScript 4.0版本废弃，发布为ECMAScript 3.1，后改名为ECMAScript 5，所以各类文章所说的ECMAScript 3.1等同与ECMAScript 5，2011年6月，ECMAscript 5.1版发布】

为了保持Web跨平台的本性，必须要做点什么。人们担心如果无法控制网景和微软各行其是，那么Web就会发生分裂，导致人们面向浏览器开发网页。就在这时，万维网联盟（W3C，World Wide Web Consortium）开始了制定**DOM标准的进程**。DHTML 不是由[万维网联盟](https://baike.baidu.com/item/万维网联盟)（W3C）规定的标准(**但是html是**)，DHTML 是一个营销术语。

WHATWG 和 W3C 从 2004 年开始合作。2012年7月，HTML5标准制定组织*WHATWG与W3C*因为理念上的差异*分裂*。这意味着以后将会有两个版本的 HTML5:即”标准版”*和*”living”版(活标准)

2019 年 5 月 28 日，W3C 官网发文称，W3C 和 WHATWG 签署协议，将合作开发 HTML 和 DOM 单一规范。

WHATWG 是由苹果、Google、微软和Mozilla四大浏览器厂商组成的浏览器厂商联盟。

将很快重新组建的 HTML 工作组，帮助 W3C 社区提出问题，并为 HTML 和 DOM 规范提出解决方案，并推荐 WHATWG 审阅草案。

W3C 和 WHATWG 都认为，搞两个不同的 HTML 和 DOM 规范对社区有害！

它们达成如下协议：

1、在 WHATWG 的 repo 基础上，合作制定一份 HTML 和 DOM Living Standard 与 Recommendation/Review Draft-snapshots ；

2、WHATWG 负责维护 HTML 和 DOM Living Standards；

3、W3C 搞社区建设（连接社区、开发用例、归档问题、编写测试、协调问题解决等）

4、W3C 停止独立发布与 HTML 和 DOM 相关的指定规范，而是将 WHATWG Review Draft 提交给 W3C Recommendations；

W3C 仍然致力于确保 HTML 开发继续考虑全球社区的需求，并在可访问性、国际化和隐私等方面继续改进，同时提供更好的互操作性、性能和安全性。

