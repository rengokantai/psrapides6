#### psrapides6
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
