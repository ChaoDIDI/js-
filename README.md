# Js 原型链讲解

```javascript
/****
  *创建Duck对象 构造函数 ***/
  function Duck(name,color){
  this.name =name|| '某某';
  this.color =color||'黄';
  }
  
  Duck.prototype = {
  //吃
  eat:function(){
  console.log(this.color+'颜色的鸭子'+this.name+'再吃虫子');
  },
  //走
  walk:function(){
  console.log(this.color+'颜色的鸭子'+this.name+'走起来');
  },
  toString:function(){
  console.log('这是一只'+this.color+'鸭子,它叫'+this.name);
  }
  }
  var jack =new Duck('Jack','黄');
  jack.walk();
  jack.toString();
  //定义一个fatDuck对象 继承Duck中的一些属性和方法
  function FateDuck(weight){
  //自由属性的weight||5;
  
  //通过apply的方法把duck对象的属性引入
  /**
  * 参数一 this 代表当前的FatDuck
  * 参数二 使用apply方法时[]数组,包含里duck对象所需要的参数 使用call方法时,后面跟所有的参数用','分割**/
  }
  //Duck.apply(this,['Jarry','蓝色']);
    Duck.call(this,'Jarry','蓝色');
    
    /////不能如此使用 待查证
    //this = Duck('Jarry','蓝色');
    
    //this.prototype = new Duck();
    // this.prototype = new Duck();
    // this.prototype.showWeight = function(){
    //     console.log('这是肥鸭的体重为'+this.weight+'斤');
    // }
    
    /////在此处重新定义一个showWeightFun方法
    ////用此方法定义的showWeightFun在Duck原型上不会出现
    this.showWeightFun = function(){
        console.log('这是肥鸭的体重为'+this.weight+'斤');
    }
    
}
////把Duck.prototype赋值给FatDuck.prototype
////属于引用赋值
FatDuck.prototype = Duck.prototype;
/////此处我修改了FatDuck原型上的方法，
/////同时Duck原型上的方法也被我改变了
/////使用此方法容易形成 猴子补丁
FatDuck.prototype.showWeight = function(){
    console.log('这是肥鸭的体重为'+this.weight+'斤');
}
// FatDuck.prototype = {
//     showWeight:function(){
//         console.log('这是肥鸭的体重为'+this.weight+'斤');
//     }
// }
var littleFatDuck = new FatDuck();
//littleFatDuck.showWeight();

/*****
 * 新创建的测试属性鸭子
 */
function ThinDuck(){
    Duck.apply(this,['Lily','蓝色']);
}
// ThinDuck.prototype = Duck.prototype;
// ThinDuck.prototype.showWeight = function(){
//     console.log('你好瘦啊！');
// }
/****
 * 顶一个空白的临时对象 用于对象赋值中间传递使用
 */
function EmptyDuck(){    
}
/////把Duck的原型赋值为EmptyDuck
EmptyDuck.prototype = Duck.prototype;
var emptyDuck = new EmptyDuck(); ///实例化了一个EmptyDuck对象
////实例化的空对象的原型赋值给我的ThinDuck
ThinDuck.prototype = emptyDuck; 
ThinDuck.prototype.laugh = function(){
    console.log('嘎嘎叫...');
}
```
