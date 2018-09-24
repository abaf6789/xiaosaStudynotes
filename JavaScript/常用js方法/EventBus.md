# EventBus

```javascript
function Handler() {
  const fns = [];
  this.add = fn => {
    fns.push(fn);
  };
  this.invoke = () => {
    fns.forEach(fn => {
      try {
        fn();
      } catch (error) {
        console.log(error);
      }
    });
  };
  this.remove = fn => {
    const i = fns.indexOf(fn);
    if (i >= 0) {
      fns.splice(i, 1);
    }
  }
}

class EventBus {
  constructor() {
    this.handlers = {};
  }
  on(eventName, fn) {
    let handle = this.handlers[eventName];
    if (handle == null) {
      handle = new Handler();
      this.handlers[eventName] = handle;
    }
    handle.add(fn);
  }
  emit(eventName) {
    const handle = this.handlers[eventName];
    if (handle != null) {
      handle.invoke();
    }
  }
  off(eventName, fn) {
    if (eventName == null) {
      return;
    }
    const handle = this.handlers[eventName];
    if (handle != null) {
      if (fn == null) {
        delete this.handlers[eventName];
      } else {
        handle.remove(fn);
      }
    }
  }
  once(eventName, fn) {
    this.emit(eventName);
    this.off(eventName, fn);
  }
}

export default new EventBus();

```

