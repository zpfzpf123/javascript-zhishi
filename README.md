# 1 闭包
闭包 让私有变量变成全局变量 一般写法都是a函数里面return一个b函数 从而让a函数里面的变量变为全局变量 也就是每次调用a函数 里面的变量就不会初始化 而是继续保留上次的值 原理是一个函数只要在调用 那么它所在的环境不会被销毁 闭包正是因为返回一个b函数 当这个函数被调用 那么它所在的a函数里面的环境就不会被销毁 从而得就让私有变量变成全局变量
# 2 this
一般函数中，this指向当前函数调用的对象，箭头函数指向外层作用域，也就是箭头函数的上下文。
# 3 js同步和异步执行机制
首先判断js代码是同步还是异步,同步就进入主进程,异步就进入event table
异步任务在event table中注册函数,当满足触发条件后,被推入event queue
同步任务进入主线程后一直执行,直到主线程空闲时,才会去event queue中查看是否有可执行的异步任务,如果有就推入主进程中
以上三步循环执行,这就是event loop

既然js是单线程，那就像只有一个窗口的食堂，学生需要排队一个一个打饭，同理js任务也要一个一个顺序执行。这种模式执行简单，但随着日后的需求，事务，请求增多，这种单线程模式执行效率必定低下。只要有一个任务执行消耗了很长时间，在这个时间里后面的任务无法执行。

常见的有新闻包含的超清图片加载很慢，难道我们的网页要一直卡着直到图片完全显示出来？为了解决这个问题，**JavaScript语言将任务执行模式分成同步和异步：**

- **同步模式：** 就是上面所说的一种执行模式，后一个任务等待前一个任务结束，然后再执行，程序的执行顺序与任务的排列顺序是一致的、同步的。
- **异步模式：** 就是每一个任务有一个或多个回调函数（`callback`），前一个任务结束后，不是执行后一个任务，而是执行回调函数，后一个任务则是不等前一个任务结束就执行，所以程序的执行顺序与任务的排列顺序是不一致的、异步的。

https://img.jbzj.com/file_images/article/202109/2021924162932253.png?2021824162952

**导图要表达的内容用文字来表述的话：**

- 

- 同步和异步任务分别进入不同的执行"场所"，同步的进入主线程，异步的进入`Event Table`并注册函数。
- 当指定的事情完成时，`Event Table`会将这个函数移入`Event Queue`。
- 主线程内的任务执行完毕为空，会去`Event Queue`读取对应的函数，进入主线程执行。
- 上述过程会不断重复，也就是常说的`Event Loop`(事件循环)。

 **再配上代码表达：**

```
let data = [];``$.ajax({``  ``url:blog.csdn.net,``  ``data:data,``  ``success:() => {``    ``console.log(``'发送成功!'``);``  ``}``})``console.log(``'代码执行结束'``);
```

**上面是一段简易的ajax请求代码：**

- `ajax`进入`Event Table`，注册回调函数`success`。
- 执行`console.log`('代码执行结束')。
- ajax事件完成，回调函数`success`进入`Event Queue`。
- 主线程从`Event Queue`读取回调函数`success`并执行。




```javascript
setTimeout(()=>{

  console.log(111);

  },0)

  new Promise((*resolve*, *reject*) => {

  console.log(222);

  setTimeout(() => {

    console.log(333);

  }, 0);

  resolve(444);

  }).then(*res*=>{

  console.log(res);

  })

  console.log(555);
```

  /* js执行顺序是同步>微任务队列>宏任务队列，首先同步任务先执行222和555，

  然后promise是微任务队列,这个时候js主线程任务已经执行完毕,开始把微任务队列中的

  任务添加到主线程继续执行，所以输出555，最后定时器是宏任务队列，所以

  最后执行111和333

  js运行，代码刚开始就是定时器，我们首先把定时器加入event table中，注册好里面的回调函数，同时被推入入event queue(任务队列)，等待主线程执行完后，此时，就回去任务队列查找是否有可执行的异步任务，有就推到主线程执行，这个过程的循环执行，就是event loop[事件循环

  */
# 4 js原型和原型链
所有对象和函数数组等都有__proto__属性，他指向了其构造函数的protoType对象属性,如对象的构造函数就是new Object(),一般我们会把公共方法存入原型之中，这样方便在实例化对象使用方法时指向同一个内存地址，不要要每实例化一个对象就新开辟一个内存空间用来存放方法，同时__proto__和protoType都有constructor属性，他就是指向构造函数本身，故而，点开constructor属性，里面也会有protoType属性和__proto__等。注意，Object已经是最高层级的构造函数了，原型链也在这为起点。
