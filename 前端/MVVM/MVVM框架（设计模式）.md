# 观察者模式

ViewModel：包含Data与View层

观察者模式：首先要有一个Observer，它用来监听Data发生的变化，它发生变化以后会通知所有的观察者列表（Dep）。怎么理解观察者列表？比如说Data中设置了一个值`a=1`，当a发生变化，Observer会监听到Data的变化（数据的变化会触发`object.defineProperty`下的`set`函数），`set`会执行一个对观察者列表的触发，比如刚才说的a改变了，然后要通知观察者列表，假如观察者列表里有a,b,c,d，就会触发观察者列表里的`update`这个函数，也就是这个Dep，要进行回调，这个回调就是观察者（Watcher）给的，然后它拿到这个更新了，就会更新View层。Dep是怎么知道有谁是观察者列表呢？是观察者（Watcher）给它的。