### 1.创建合约

contract a{

}

contract b{

 a A = new a();

}

### 2.可见性 external，internal，public，private

external：外部调用  internal：内部调用   public：内外都可  private：内部西游

### 3.访问函数 getter function

contract a{

uint public num =21;

}

contract b {

a A =new a();

function f()public constant returns(uint){

uint m =A.num();

}

}

#### 4.函数修改器 function  modifier 可以改变一个函数行为,多个函数修改器执行顺序

modifier 函数名(){

...... 

_;

}



多个按照先后执行顺序执行，有一个不满足，则不执行

### 5.状态常量

 使用constant修饰，无法改变值，不允许访问storage数据

### 6.  视图函数

view function  不能改变状态， 声明用view，constant的别名 

### 7.纯函数

pure function  不能修改也不能读取

### 8.回退函数

fallback function  没有名字，没有参数，一个合约只能有一个回退函数

### 9.  函数重载

function overloading ， 函数名相同 参数不用  类型不一样，但是外部接口ABI表现一样，也是无法重载的。 

### 10 事件

事件是EVM日志基础设施提供的一个接口

事件有监听事件触发事件等     