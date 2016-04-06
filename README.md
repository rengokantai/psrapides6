#### psrapides6
#####New ES6 syntax
######let
let does not allow hoisting.
######arrow function
```
document.addEventListener('click',()=>console.log(this)); //window  
```
arrow function is not set to the element that getting the event,context that the code run.  

Ex2
```
var f={
a:10,
g:function(){
    console.log(this):
  }
};
f.g();  // Object{a:10}
```
Ex2 using arrow
```
var f={
a:10,
g:()=>console.log(this)
};
f.g();  // window{..}
```
You can not bind a arrow funciton to new obj.

######Defalt Function Parameters
```
var f = function(a,b=3){console.log(argument.length};  //1
f(1);
```
some quirky:
```
var x = function(a=b,b=1){};
x()  //error
var x = function(a=b,b=1){};
x(1)  //ok
```
using Function class
```
var f = new Function("a=10","return a");
console.log(f()); //10
```
######Rest and Spread
```
var f = function(a,...b){}
console.log(f.length) //1
```
```
var arr = Array(...[,,]);  //Or
var arr = [...[,,]];
console.log(arr);   //[undefined,undefined]
Math.max(..."123")  //3
["A",..."BC"]  //A B C
```
######Object Literal Extensions
Note function literal will point to locattion, not execution scope
```
var a=1;
var f ={
  a:2,
  g(){
    return this.a
  }
}
console.log(f.g()) //1 not2
```

dynamic field
```
var fi = 'dynamic';
var ni = 1;
var p = {[fi]:ni};
console.log(p);  //dynamic:1
```
######Template Literals
interpolation have priority.
```
function show(s){ let x ="100"; console.log(s)}
let x = "200";
show(`string is : ${x}`);  //200
```
This will be a array
```
function x(s){console.log(s);}
x `a`;
```
pass value
```
function multi(a,...b){
  console.log(a);     //["start: ", "End: ",""]
  console.log(b);     // [1,2]
}
let x=1;
let y=2;
multi `Start: ${x} End:  ${y}`;
```

######Destructing
```
let all=['a','b','c'];
let [d,e,f]=all;   //Or let [d,,f]  Or  let [d,...e] Or let[d,e,f=10]
console(e); //'b'
```
call func with array param
```
function q([x,y]){console.log(y);}
q([2,3function q([x,y]){console.log(y);}
q(2,3);
```
This is incorrect:
```
function q([x,y]){console.log(y);}
q(2,3);  //Error
```
Destructure objects:
```
let o ={a:'a',b:'b',c:'c'};
let {x,y,z}=o;
console.log(z);
```
assign alias, but the order is reversed
let{x:newx,y:newy,z:newz}=o;
console.log(newx);
```

#####ES6 Modules and Classes
######named exports in mudules
We cannot change var.  
file1.js
```
import{x}from 'file2.js'
x=3;  //illegal.
```
file2.js
```
export let x=1;
```
But we can change a property in objects;
file1.js
```
import{x}from 'file2.js'
x.y=3;  //legal.
```
file2.js
```
export let x={
  y:1
};
```
change property.
file1.js
```
import{x}from 'file2.js'
x.y=3;  //legal.
```
file2.js
```
export let x={
  y:1
};
```
######Class fundamentals
function
```
function x(){}
x===window.x  //true
class x{}
x===window.x //false
```
######extends and super
super can be used in object literals
```
let p = { g(){return 1;}};
let q ={ g(){return super.g()+1;}};
Object.setPrototypeOf(q,p);
console.log(q.g()); //2
```
######new.target
```
class X{
  constructor(){
    console.log(typeof new.target);    //function ->constructor
  }
}
var x = new X();
class Y extends X{
  constructor(){
    super()
  }
}
var x = new Y(); //  constructor(){super()}
```
#####New Types and Object
######Symbols
intro
```
let sy = Symbol('r');
console.log(typeof sy);  //Symbol
sy.toString()  //Symbol(r)
Symbol('x')===Symbol('x') //false
Symbol.for('x')===Symbol.for('x') //true
```
keyFor
```
let p = Symbol.for('r');
let dest = Symbol.keyFor(p);
console.log(dest);  //r
```
use as a key
```
let p={
  a:'s',
  [Symbol.for('x')]:'str'
}
let val = p[Symbol.for('x')];
console.log(val); //str
Object.getOwnPropertyNames(p)  //only a
Object.getOwnPropertySymbols(p) //[Symbol(x)]
```
######well known
Ex1 Symbol.toStringTag
```
let Parent = function(){}
let p =new Parent();
console.log(p.toString()); //object Object
Parent.prototype[Symbol.toStringTag]='parent';
console.log(p.toString()); //object parent
```
Ex2 Symbol.isConcatSpreadable
```
let arr =[1,2,3];
console.log([].concat(arr)) //[1,2,3]
arr[Symbol.isConcatSpreadable]=false;
console.log([].concat(arr)) //[[1,2,3]]
```
Ex3
```
let arr=[1,2,3]
let s = att+100
console.log(s)  //1,2,3100
arr[Symbol.toPrimitive]=function(h){
  return 10;
}
console.log(s)  //103
```
######Object.setPrototypeOf
```
var a ={x:1}
var b= {y:1}
Object.setPrototypeOf(a,b) //b is parent class
a.y  //1
```
######Object.assign
```
var a ={x:1}
var b= {y:1}
var c ={}
Object.assign(c,a,b);  
console.log(c)  {x:1,y:1}
```
but not applicable if is not enumerable
```
var a ={x:1}
var b= {y:1}
Object.defineProperty(b,'z',{
  value:1,
  enumberable:false
});
var c ={}
Object.assign(c,a,b);  
console.log(c)  {x:1,y:1}
```
And not walk prototype
```
var a ={x:1}
var b= {y:1}
var p ={z:1}
 Object.setPrototype(b,p);
var c ={}
Object.assign(c,a,b);  
console.log(c)  {x:1,y:1}
```
Object.is
```
Object.is(NaN,NaN) //true
0===-0
Object.is(0,-0) false
```
######String extension
startsWith endsWith includes  
```
"\u{12345}".length  //2 (emoji)
var str="\u{12345}\u{54321}";
console.log(Array.from(str).length  //3
```
codePointAt
```
var s = "aaaaaaa\u0301n";
console.log(s.normalize.codePointAt(7).toString(16));  //6e
string.fromCodePoint(0x12345);
String.raw`\u{12345}` //\u{12345}  not translate
```
repeat
```
console.log('x'.repeat(10))  //xxxxxxxxxx
```
######Number Extensions
```
Number.parseInt === parseInt  //true
isNaN('NaN') //true
Number.isNaN('NaN') //false
isFinite('1') //true
Number.isFinite('1') //false
Number.isInteger(NaN)
Number.isInteger(Infinity)
Number.isInteger(undefined)
Number.isInteger(12.3)   //all false
Number.isSafeIntefer(Math.pow(2,53)) //False max=2^53-1
```
######math
cosh acosh......  
log2 log10 imul log1p....  
sign
```
Math.sign(0)  //0
Math.sign(-0)  //-0
Math.sign(-20)  //-1
Math.sign(NaN)  //NaN
```
trunc
```
trunc(22.2)  //22
trunc(-22.2)  //22
```
######RegExp
new flag `u`
```
let a = /\u{12345}/
let b = /\u{12345}/u
a.test('emoji') //false
b.test('emoji') //true
b.flags //u     //order = gimuy
```
new flag `y`
[see here](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/RegExp/sticky)

######Function Extension
fn name
```
let x =function y(){}
console.log(x.name) //y
let x =function(){}
console.log(x.name) //x
let x =function(){}
let z = x;
console.log(z.name) //x
```
fnname is not writable but is configurable with Object.defineProperty

#####Iterators,Generators
######Iterators
```
let a=[1,2,3]
typeof a[Symbol.iterator] //function
```
use it.
```
let val = a[Symbol.iterator]();
val.next()   //{done:false,value:1}
```

infinite iterator
```
let p = { 
  [Symbol.iterator](){
    let next=1;
    return{
      next(){
        return {
          value:next++,
          done:false
        };
      }
    };
  }
};
p[Symbol.iterator]()   //
p.next.value() //1
p.next.value() //2
```
add a break
```
for(let i of p){if(i>3)break; console.log(i);}
```

Or, rewrite the above iterator
```
let p = { 
  [Symbol.iterator](){
    let next=1;
    return{
      next(){
        return {
          let value = next>2?undefined:next++;
          let done=!value;
          return{value,done};
        };
      }
    };
  }
};
```
splash
```
let a =[1,2,3]
function p(a,b,c){console.log(c)};
p(...a);  //3
```
######Generators
```
function *x(){
  yield 1;
  yield 2;
}
let y =x();
y.next();  //1
```
infinite yield
```
function *x(){
  let id =1;
  while(true)yield(id++);
}
let y =x();
y.next();  //1
```
add a break
```
for(let i of x()){
  if(i>3)break;
  console.log(i);
}
```
######Yielding in generators
assign yield
```
function *x(){
  let res = yield;
  console.log(`${res}`);
}
let y =x();
t.next();   //kill off generator.
t.next(100);  //100
consloe.log(t.next(100));    //done,true....   
```
yield second element
```
function *x(){
  let res = [yield,yield,yield];
  console.log(res[2]);
}
let y=x();
y.next();
y.next(1);
y.next(2);
y.next(3);  //this will print
```
yield has very low precedence
```
function *x(){
  let res = 4*(yield 100);
  console.log(res);
}
let y = x();
y.next();  //100 will be discarded
y.next(4); //return 16
```
yidle pri and array
```
function *x(){
  yield 1;
  yield [1,2,3];
}
let y = x();
y.next().value;  //return 1
y.next().value; //return [1,2,3]
```
splash array
yidle pri and array
```
function *x(){
  yield 1;
  yield* [1,2,3];   //iterator delegation
}
let y = x();
y.next().value;  //return 1
y.next().value; //return 1
y.next().value; //return 2
y.next().value; //return 3
```

######throw and return
Ex 1
```
function *x(){
  try{
    yield 1;
    yield 2;
  }
  catch(e){
  
  }
}
let y = x();
console.log(y.next().value);
console.log(y.throw('wrong'));  //generator completed. done:true
console.log(y.next());    //done:true
}
```
######More Promise
Promise.all
```
let a = new Promise();
let b = new Promise();
Promise.all([a,b]).then(function(value){console.log(value)},function(reason){console.log(reason)});
//assume a resolve 3 sec, b resolve 5 sec   //(after 5 sec)
//assume a resolve 1 sec, b reject 2 sec  //(rej after 2 sec)
//assume a reject 1 sec, b reject 2 sec //(rej after 1 sec) (reject fast)
```
Promise.race
```
Promise.race([a,b]).then(function(value){console.log(value)},function(reason){console.log(reason)});
//assume a resolve 3 sec, b resolve 5 sec   //(after 3 sec)
//assume a resolve 3 sec, b reject 2 sec  //(rej after 2 sec)
//assume a resolve 1 sec, b reject 2 sec  //(after 1 sec)
```
#####Arrays and Collections
######Array Extensions
```
let x = Array.of(10)   //length:1
let a = [10,20,30];
let b = Array.from(a,v=>v+10);
console.log(b); //20,30,40
```
Or another syntax
```
let b = Array.from(a,function(v){return v+this.amount},{amount:10});    //v=>v+this.amount will return NaN,NaN, WRong!!!
console.log(b) //20,30,40
```
fill and find
```
let c = a.fill(30) //30,30,30
let c = a.fill(30,1) //start at index 1  so  10,30,30
let c = a.fill(30,1,2) //start at index 1,end at index 2(excluded)  so  10,30,10
let c = a.fill(30,-1) //start at index -1  so  10,20,30
let c = a.find(v=>v>20)  //30
```
findIndex
```
let c = a.findIndex(function(val,idx,arr){return val=this},10); //0
```
copyWithin
```
let c=a.copyWithin(2,0)   //hard to memorize  params:(targetidx->end,resourceidx)     //10,20,10
let c=a.copyWithin(0,1)  //20,30,30
[1,2,3,4,5].cppyWitnin(3,0,2)=[1,2,3,1,2]
```
others
```
...a.entries() // [0,10],[1,20],[2,30]
...a.keys()   //0,1,2
...a.values()  //10,20,30
```
######ArrayBuffers and Typed arrays
```
let c =ArrayBuffer(1024);
c.byteLength  //1024
c[0]=0xff
c[0] //255
```
Alltypes:
Int8Array() Uint8Array() Uint8ClampedArray() Int32Array Uint32Array Int16Array Unit16Array  
Float32Array Float64Array
```
let a =new Uint8Array(c)
a[0]=0xff  //255
let a =new Int8Array(c)
a[0]=0xff  //-1
```
```
let a =new Uint8ClampedArray(c)
a[0]=-100 //0
a[0]=300  //255        //only 0-255 allowed
```
buffer share
```
var a = new ArrayBuffer(1024);
undefined
var b = new Uint16Array(a);
undefined
var c = new Uint8Array(a);
undefined
b[0]=1;
1
c[0]
1
c[1]=1;
1
b[0]
256
```
######big endian(most significant byte store first)
```
let dv = new DataView(a)
dv.byteLength   //1024
let dv = new DataView(a,0,10)
dv.byteLength   //10
```

```
let dv = new DataView(a)
dv.setUint8(0,1)   //set 1 at index 0
dv.getUnit16(0)  //256     //big endian.

let dv = new DataView(a)
dv.setUint8(0,1)   //set 1 at index 0
dv.getUnit16(0,true)  //256     //little endian.   //browser default
```
######map and weakmap
```
var p ={c:1}
let ps = new Map();
ps.set(p,'x');
ps.get(p) //x
ps.size
ps.clear
```
using arrays
```
let arr =[[p,'x'],[q,'y']]
let ps =new Map(arr)
ps.size  //2
ps.has(p)
...ps.values()
...ps.entries()
```
weakmap
```
p=null;
ps.size //undefined
```

######set and weakset
```
let se = new Set()
let arr = new Set(['s','r'])
se.add('x')
se.has('x')
let obj=new Set([{i:1},{i:1}])  //size=2 different objects!
```
We cannot add pri types in weakset.
```
let arr = new WeakSet(['s','r'])  //error
let obj=new WeakSet([{i:1},{i:1}])  
obj.size //undefined
obj.has({i:1}) //true
```

######subclassing
```
class Parent extends Array{
  sum(){
    let t=0;
    this.map(v=>t+v;);
    return t;
  }
}
let a = Parent.from([1,2])
a instanceof Parent  //true
a.length //2
let r = a.reverse()
r instanceof Parent //true
r instanceof Array //true
a.sum() //3
```




#####The Reflect API
######Construction and Method
Ex0 construct
```
class Parent{
  constructor(){
    console.log(`${a}`);
  }
}
let r = Reflect.construct(Parent,['time']) //time
```
Ex1
```
class R{
  constructor(){
    this.t = 1;
  }
  show(){
    console.log(this.t);
  }
}

Reflect.apply(R.prototype.show, {t:100});   //100
```
Ex2
```
class R{
  constructor(){
    this.t = 1;
  }
  show(args){
    console.log(this.t+" "+args);
  }
}

Reflect.apply(R.prototype.show, {t:100},["200"]);   //100 200
```

######Reflect and prorotype
Ex1
```
class Parent{
  constructor(){
    console.log('parent');
  }
}
class Child extends Parent{}
console.log(Reflect.getPrototypeOf(Child)); //constructor(){console.log('parent')}
```

Ex2
```
class Parent{}
let prop={getId(){return 1;}}
let i = new Parent();
Reflect.setPrototypeOf(i,prop);
console.log(i.getId());    //1
```
######Reflect and Properties
Ex1 get 
```
class Parent{
  constructor(){
    this.i=10;
  }
}
let p = new Parent();
console.log(Reflect.get(p,'i'));//10
```
Ex2 get
```
class Parent{
  constructor(){
    this._i=10;
  }
  get i(){
    return this._i;
  }
}
let p = new Parent();
console.log(Reflect.get(p,'i',{_i:1000})); //1000
```
Ex3 set 
```
class Parent{
  constructor(){
    this.i=10;
  }
}
let p = new Parent();
Reflect.set(p,'i',20)
console.log(p.id);//20
```
Ex4 set
```
class Parent{
  constructor(){
    this._i=10;
  }
  set i(value){
    this._i=value
  }
}
let p = new Parent();
let n = {i:20};
Reflect.set(p,'_i',20,n}
console.log(p._i);//10
console.log(n._i);//20
```

Ex6 has
```
class Parent{
  constructor(){
    this.c='p';
  }
}
class Child extends Parent{
  constructor(){
    super();
    this.d=10;
  }
}
let p = new Child();
console.log(Reflect.has(p,'c')); //true
console.log(Reflect.has(p,'d'));  //true
```
Ex7 ownKeys
```
class Parent{
  constructor(){
    this.c='p';
  }
}
class Child extends Parent{
  constructor(){
    super();
    this.d=10;
  }
}
let p = new Child();
console.log(Reflect.ownKeys(p)); //["c","d"]
```
Ex8 defineProperty
```
class Parent{
}
let p = new Parent();
Reflect.defineProperty(r,'c',{
  value:10,
  configurable:true,
  enumerable:true
});
console.log(p['c']); //10
```
Ex9 deleteProperty
```
let o = {c:10};
Reflect.deleteProperty(o,'c');
consolw.log(o.c);  //undefined
```
Ex10 getOwnPropertyDescriptor
```
let o = {c:10};
let d = Reflect.getOwnPropertyDescriptor(o,'c');
consolw.log(d);   //cong,enum.....
```
######Reflect and Property Extensions
Ex1 preventExtensions
```
let parent={
c:10}
Reflect.preentExtensions(parent);
parent.d=20;
console.log(parent.d); //undfined
```
Ex2 isExtensible
```
let parent={
c:10}
Reflect.preentExtensions(parent);
parent.d=20;
console.log(Reflect.isExtensible(parent)); //false
```
#####The Proxy API
######Get by Proxy
Ex1 
```
function Parent(){
  this.c ="str";
}
var p = new Parent();
var proxy = new Proxy(p,{
  get:function(target,prop,receiver){
    return "I want" +prop;
  }
});
console.log(proxy.name); //I want str
```
Ex2 Reflect.get
```
function Parent(){
  this.c ="str";
}
var p = new Parent();
var proxy = new Proxy(p,{
  get:function(target,prop,receiver){
    return Reflect.get(target,prop,receiver);
  }
});
console.log(proxy.c); //str
```
Ex3 denied
```
function Parent(){
  this.c ="str";
  this.d="s";
}
var p = new Parent();
var proxy = new Proxy(p,{
  get:function(target,prop,receiver){
    if(prop=='d')
      return 'wrong!';
    return Reflect.get(target,prop,receiver);
  }
});
console.log(proxy.d); //wrong
```
######calling functions by proxy
```
function c(){
  return 1;
}
var proxy = new Proxy(c,{
  apply:function(target,prop,arg){
    return Reflect.apply(target,prop,arg);
  }
});
console.log(proxy());  //1
```
######A Proxy as a prototype
```
var parent={c:10}
var proxy=new Proxy({},{get:function(target,prop,receiver){return prop+"not exist";}});
Object.setPrototypeOf(parent,proxy);
console.log(parent.c);  //10
console.log(parent.d);  //d not exist
```
######Revocable proxies
Ex1 revocable
```
var parent={c:10}
var {proxy,revoke}=Proxy.revocable(c,{get:function(target,prop,receiver){return Reflect.get(target,prop,receiver}+10}});
console.log(proxy.c);  //10
revoke();
console.log(proxy.c);   //not exist
```
