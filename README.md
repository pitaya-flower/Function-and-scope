# Function-and-scope
函数与作用域
# 函数声明与函数表达式的区别
```
函数声明使用function来进行，由于存在函数声明提升，即可以在函数声明之前调用该函数，所以不论执行语句的位置在哪都可以调用该函数。

函数表达式通过赋值语句来声明函数，声明完毕后要加上分号表示该句结束，在表达式执行完毕后，才生成函数并可被调用。函数表达式只会将变量先提升，此时变量的类型是undefined，所以不能在函数表达式前调用函数。
```
#变量的声明前置与函数的声明前置

```
//变量的声明前置
console.log(a) //undefined
var a = 1;
console.log(a); //1
//相当于将var声明的变量提升到作用域顶部
var a;
console.log(a);
a = 1;
console.log(a);
```
```
//函数的声明前置
fn(); //3 因为function声明的函数会提升到作用域顶部，所以不会报错
function fn() {
	console.log(3);
}
```
#arguments
```
argument是类数组对象，它包含了函数的所有实参,每个函数中都存在argument对象，argument并不是一个真正的数组，所以不具备除length属性之外的属性，这个对象维护这所有传入该函数的参数列表。
```
```
function foo(a,b) {
 console.log(a);
console.log(b);
console.log(arguments);
}
foo(1,2);
//1
//2
//[1, 2]
```
通过以下语句可将arguments转化为数组对象
```
var args=Array.prototype.slice.call(arguments)
```
#函数重载
```
重载是函数具有相同的名字，但是由于传入的参数不同，执行不同操作。在js中没有类似其他语言的重载，因为同名函数会被覆盖。但是js可以通过在函数内部对传入参数进行判断来达到重载的目的。
```
```
//通过对参数进行条件判断进行函数重载
function fn(name,age,sex) {

if (name) {
 console.log(name);
}
if (age) {
 console.log(age);
}
if (sex){
 console.log(sex);
}
}
fn('a');
fn('a',18);
fn('a',18,'female');
```
```
//通过arguments进行函数重载
function fn(){
var sum=0;
for(var a=0;a<arguments.length;a++){
  sum=sum+[arguments[a];
}
return sum;
}
console.log(fn(1,2,3));
```
#立即执行函数表达式(IIFE)概念和作用
立即执行函数表达式就是在函数声明后立即执行，这样可以做到隔离作用域，避免变量污染全局。
```
(function(1,2,3){
var sum=0;
for(var a=0;a<arguments.length;a++){
sum=sum+arguments[a];
}
return sum;
})();
//也可以是这样的
(function(){/*code*/}());
~function(){/*code*/}();
!function(){/*code*/}();
表达式前的计算符号皆可，来避免function被首先解析，导致语法错误，总体而言，使用括号更加安全。其实只要将函数变为表达式就可以达到这样的效果。
```
#求n!，用递归来实现
```
function recursion(n) {
	if (n<0) {
		return console.log('非法数据！请正确填写0或任意正整数！');
	}
      else if(n === 0 || n === 1){
       return 1;
  }
return n * recursion(n-1); 	
}
```
#如下代码输出结果
```
	function getInfo(name, age, sex){
		console.log('name:',name);
		console.log('age:', age);
		console.log('sex:', sex);
		console.log(arguments);
		arguments[0] = 'valley';
		console.log('name', name);
	}
getInfo('饥人谷', 2, '男');   //name:饥人谷 age:2 sex:男 ['饥人谷',2,'sex'] name valley 
getInfo('小谷', 3);   //name:小谷、age:3、undefined、['小谷', 3]、name vally;
getInfo('男');   //name:男、undefined、undefined、['男']、name vally;
```
#写一个函数返回参数的平方和
```
function sumOfSquare() {
	var result = 0;
	for(var i=0; i<arguments.length; i++){
	  result = result + arguments[i]*arguments[i];
	}
	return result;
}
```
#如下代码的输出结果及原因
```
console.log(a); //声明提前，但是未赋值所以为undefined
var a = 1; 
console.log(b); //会报错，b is not defined
```
# 如下代码的输出结果及原因
```
sayName('world'); //hello world,因为函数声明前置
sayAge(10); //sayAge is not a function函数表达式只将函数表达式里的sayAge前置，此时还未赋值，所以以函数的方式调用会报错
function sayName(name){
	console.log('hello ', name);
}
var sayAge = function(age){
	console.log(age);
};
```
# 如下代码输出结果及作用域链查找过程伪代码
```
var x = 10
bar() 
function foo() {
  console.log(x)  
}
function bar(){
  var x = 30
  foo()
}
```  
#作用域查找伪代码
```
globalContext = {
	AO:{
		x:10
		foo:function
		bar:function
	},
	Scope:null
}
//foo()声明时
foo.[[scope]] = globalContext.AO
//bar声明时
bar.[[scope]] = globalContext.AO
//调用bar时,bar的执行上下文
barContext = {
	AO:{
	 x:30
	 foo:function
	}
	scope:bar.[[scope]] = globalContext.AO
}
//调用foo时，先从bar的AO中，找不到再从scope中找，这里在barConetext中能够找到
，就立即调用。
//调用时进入foo的执行上下文
fooContext = {
	AO:{	
	}
	scope:foo.[[scope]] = globalContext.AO
}
//所以从scope中即globalContext.AO中可以找到x:10。
```
#如下代码输出结果及作用域链的查找过程伪代码
```
var x = 10;
bar() 
function bar(){
  var x = 30;
  function foo(){
    console.log(x) 
  }
  foo();
}
```
#作用域查找伪代码
```
globalContext = {
	AO:{
		x:10
		bar:function
	}
	scope:null
}
bar.[[scope]] = globalContext.AO 
barContext = {
	AO:{
	 x:30
	 foo:function
	}
	bar.[[scope]] = globalContext.AO 
}
foo.[[scope]] = barContext.AO
fooContext = {
	AO:{
	}
	foo.[[scope]] = barContext.AO
}
//所以调用foo时会在barContext.AO中找到x:30。
```
#如下代码输出结果及作用域链查找过程伪代码
```
var x = 10;
bar() 
function bar(){
  var x = 30;
  (function (){
    console.log(x)
  })()
}
```
#作用域查找伪代码
```
globalContext = {
  AO:{
    x:10,
    bar:function,
  },
  scope:none;
}
bar['scope'] = globalContext.AO;
barContext = {
  AO:{
    x:30,
    foo:function,
  },
  scope:golbalContext.AO
}
foo['scope'] = barContext.AO;
fooContext = {
  AO:{},
  scope:barContext.AO
}
```
#如下代码输结果及作用域链查找过程伪代码
```
var a = 1;

function fn(){
  console.log(a)
  var a = 5
  console.log(a)
  a++
  var a
  fn3()
  fn2()
  console.log(a)

  function fn2(){
    console.log(a)
    a = 20
  }
}
function fn3(){
  console.log(a)
  a = 200
}
fn()
console.log(a)
```
#作用域查找伪代码
```
globalContext = {
	AO:{
		a:200
		fn:function
		fn3:function
	}
}
fn.[[scope]] = globalContext.AO
fn3.[[scope]] = globalContext.AO
fnContext = {
	AO:{
		a:20
		fn2:function
	}
	scope:fn.[[scope]] = globalContext.AO
}
fn2.[[scope]] = fnContext.AO
//第一次undefined 第二次5 
//执行fn3时，进入fn3的执行上下文
fn3Context = {
	AO:{
	}
	fn3.[[scope]] = globalContext.AO
} 
//第三次a在globalContext.AO中查找 a=1，并在globalContext.AO中的a赋值为200
fn2Context = {
	AO:{
	}
	fn2.[[scope]] = fnContext.AO
}
//第四次a在fnContext.AO中找到a=6，并赋值a=20
//第五次在fnContext.中查找 a=20
//第六次在globalContext中查找 a=200
```
