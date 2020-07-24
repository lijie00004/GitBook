> `import StateMachine = require("./state-machine");`
>
> `state-machine.js`位于本目录下的res
>
> `https://github.com/jakesgordon/javascript-state-machine/tree/0d603577423244228cebcd62e60dbbfff27c6ea3`

#### States and Transitions

```javascript
var fsm = new StateMachine({
    init: 'solid',
    transitions: [
        { name: 'melt',     from: 'solid',  to: 'liquid' },
        { name: 'freeze',   from: 'liquid', to: 'solid'  },
        { name: 'vaporize', from: 'liquid', to: 'gas'    },
        { name: 'condense', from: 'gas',    to: 'liquid' }
    ],
    // 可不写 methods
    methods: {
        onMelt:     function(){ console.log('I melted')    },
        onFreeze:   function(){ console.log('I froze')     },
        onVaporize: function(){ console.log('I vaporized') },
        onCondense: function(){ console.log('I condensed') },

        onBeforeMelt: function(){ console.log('onBeforeMelt')    },
        onAfterMelt: function(){ console.log('onAfterMelt')    },

        onLeaveSolid: function(){ console.log('onLeaveSolid')    },
        onEnterLiquid: function(){ console.log('onEnterLiquid')    },
        // fired when any chance
        onBeforeTransition: function(){console.log('onBeforeTransition')},
        onLeaveState: function(){console.log('onLeaveState')},
        onTransition: function(){console.log('onTransition')},
        onEnterState: function(){console.log('onEnterState')},
        onAfterTransition: function(){console.log('onAfterTransition')},
    }
});
```

依次调用 `onBeforeMelt` -> `onLeaveSolid` -> `onEnterLiquid` -> `onAfterMelt` -> `onMelt`

* `fsm.melt();` // methods to transition to a different state:

```javascript
fsm.state; // 返回当前状态
fsm.is("liquid"); // 当前状态是否为"liquid"
fsm.can("condense"); // 当前状态是否能触发"condense"方法
fsm.cannot("condense"); // 与 can 相反
fsm.transitions(); // 返回当前状态的方法的数组
fsm.allTransitions(); // 返回所有方法的数组
fsm.allStates(); // 返回所有状态的数组
```

#### Multiple states for a transition

使用 `goto`.会调用生命周期方法

```javascript
var fsm = new StateMachine({
    init: 'A',
    transitions: [
      { name: 'step',  from: 'A',               to: 'B' },
      { name: 'step',  from: 'B',               to: 'C' },
      { name: 'step',  from: 'C',               to: 'D' },
      { name: 'reset', from: [ 'B', 'C', 'D' ], to: 'A' },
      { name: 'resetNew', from: '*', to: 'A' }
      { name: 'stepNew', from: '*', to: function(n) { return increaseCharacter(this.state, n || 1) } }
    	]
      { name: 'goto', from: '*', to: function(s) { return s } }
    ],
     methods: {
         onStep:     function(){ console.log('onStep' + fsm.state)    },
         onReset:     function(){ console.log('onStep' + fsm.state)    },
     }
})

cc.log(fsm.state);      // A
fsm.stepNew();
cc.log(fsm.state);      // B
fsm.stepNew(5);
cc.log(fsm.state);      // G

function increaseCharacter(str, n) {
	return String.fromCharCode(str.charCodeAt(0) + n);
}

fsm.goto('D');
```

#### State Machine Factory

使用相同配置构造多个实例

这些实例共用方法，其它是私有的

```javascript
var Matter = StateMachine.factory({     //  <-- the factory is constructed here
    init: 'solid',
    transitions: [
        { name: 'melt',     from: 'solid',  to: 'liquid' },
        { name: 'freeze',   from: 'liquid', to: 'solid'  },
        { name: 'vaporize', from: 'liquid', to: 'gas'    },
        { name: 'condense', from: 'gas',    to: 'liquid' }
    ]
});

var a = new Matter(),    //  <-- instances are constructed here
b = new Matter(),
c = new Matter();

b.melt();
c.melt();
c.vaporize();

cc.log(a.state);    // solid
cc.log(b.state);    // liquid
cc.log(c.state);    // gas
```

#### 应用于已经存在的对象

```javascript
var component = { /* ... */ };

StateMachine.apply(component, {
    init: 'A',
    transitions: {
    	{ name: 'step', from: 'A', to: 'B' }
    }
});
```

#### 应用到现有类，调用this._fsm()来负责初始化

```javascript
function Person(name) {
  this.name = name;
  this._fsm(); //  <-- IMPORTANT
}

Person.prototype = {
  speak: function() {
    console.log('my name is ' + this.name + ' and I am ' + this.state);
  }
}

StateMachine.factory(Person, {
    init: 'idle',
    transitions: [
        { name: 'sleep', from: 'idle',     to: 'sleeping' },
        { name: 'wake',  from: 'sleeping', to: 'idle'     }
    ]
});

var amy = new Person('amy'),
bob = new Person('bob');

bob.sleep();

cc.log(amy.state);   // 'idle'
cc.log(bob.state);   // 'sleeping'

amy.speak(); // 'my name is amy and I am idle'
bob.speak(); // 'my name is bob and I am sleeping'
```

#### 异步

```javascript
var fsm = new StateMachine({
    init: 'menu',
    transitions: [
      { name: 'play', from: 'menu', to: 'game' },
      { name: 'quit', from: 'game', to: 'menu' }
    ],
    methods: {
      onEnterMenu: function() {
        return new Promise(function(resolve, reject) {
          $('#menu').fadeIn('fast', resolve)
        })
      },

      onEnterGame: function() {
        return new Promise(function(resolve, reject) {
          $('#game').fadeIn('fast', resolve)
        })
      },

      onLeaveMenu: function() {
        return new Promise(function(resolve, reject) {
          $('#menu').fadeOut('fast', resolve)
        })
      },

      onLeaveGame: function() {
        return new Promise(function(resolve, reject) {
          $('#game').fadeOut('fast', resolve)
        })
      }
    }
})
```

#### 自定义 Data and Methods

```javascript
var fsm = new StateMachine({
    init: 'A',
    transitions: [
      { name: 'step', from: 'A', to: 'B' }
    ],
    data: {
      color: 'red'
    },
    methods: {
      describe: function() {
        console.log('I am ' + this.color);
      }
    }
  });

  fsm.state;      // 'A'
  fsm.color;      // 'red'
  fsm.describe(); // 'I am red'
```

```javascript
var FSM = StateMachine.factory({
    init: 'A',
    transitions: [
      { name: 'step', from: 'A', to: 'B' }
    ],
    //  <-- use a method that can be called for each instance
    data: function(color) {      
      return {
        color: color
      }
    },
    methods: {
      describe: function() {
        console.log('I am ' + this.color);
      }
    }
  });

  var a = new FSM('red'),
      b = new FSM('blue');

  a.state; // 'A'
  b.state; // 'A'

  a.color; // 'red'
  b.color; // 'blue'

  a.describe(); // 'I am red'
  b.describe(); // 'I am blue'
```

#### Error Handling

```javascript
// 当前状态不允许的转换 会调用 onInvalidTransition
var fsm = new StateMachine({
    init: 'A',
    transitions: [
        { name: 'step',  from: 'A', to: 'B' },
        { name: 'reset', from: 'B', to: 'A' }
    ],
    methods: {
        onInvalidTransition: function(transition, from, to) {
            cc.log("transition not allowed from that state");
        }
    }
});
    
fsm.state;        // 'A'
fsm.can('step');  // true
fsm.can('reset'); // false

fsm.reset();      //  <-- throws "transition not allowed from that state"
```

```javascript
// 在上一个转变未完成，又进行新的转变，会调用 onPendingTransition
var fsm = new StateMachine({
    init: 'A',
    transitions: [
    { name: 'step', from: 'A', to: 'B' },
    { name: 'step', from: 'B', to: 'C' }
    ],
    methods: {
        onLeaveA: function() {
        	this.step(); // 不建议在生命函数里这么使用
        },
        onPendingTransition: function(transition, from, to) {
        	cc.log("已经在进行的转变");
        }
    }
});

fsm.state;       // 'A'
fsm.can('step'), // true
fsm.step();      //  已经在进行的转变
```

#### Initialization

```javascript
var fsm = new StateMachine({
    transitions: [
      { name: 'init', from: 'none', to: 'A' },
      { name: 'step', from: 'A',    to: 'B' },
      { name: 'step', from: 'B',    to: 'C' }
    ]
  });
  fsm.state;    // 'none'
  fsm.init();   // 'init()' transition 被显式地触发
  fsm.state;    // 'A'

var fsm = new StateMachine({
    init: 'A',
    transitions: [
      { name: 'step', from: 'A', to: 'B' },
      { name: 'step', from: 'B', to: 'C' }
    ]
});           // 'init()' transition fires from 'none' to 'A' during 构造
  fsm.state;    // 'A'

var FSM = StateMachine.factory({
    init: 'A',
    transitions: [
      { name: 'step', from: 'A', to: 'B' },
      { name: 'step', from: 'B', to: 'C' }
    ]
});

  var fsm1 = new FSM(),   // 'init()' transition fires from 'none' to 'A' for fsm1
      fsm2 = new FSM();   // 'init()' transition fires from 'none' to 'A' for fsm2
```

#### Lifecycle Events

  * `onBeforeTransition` - fired before any transition
  * `onLeaveState`       - fired when leaving any state
  * `onTransition`       - fired during any transition
  * `onEnterState`       - fired when entering any state
  * `onAfterTransition`  - fired after any transition

> 特定状态转换和名称的时间

  * `onBefore<TRANSITION>` - fired before a specific TRANSITION begins
  * `onLeave<STATE>`       - fired when leaving a specific STATE
  * `onEnter<STATE>`       - fired when entering a specific STATE
  * `onAfter<TRANSITION>`  - fired after a specific TRANSITION completes

> 最简单的事件

  * `on<TRANSITION>` - convenience shorthand for `onAfter<TRANSITION>`
  * `on<STATE>`      - convenience shorthand for `onEnter<STATE>`

> 观察生命周期事件

```javascript
fsm.observe('onStep', function() {
    console.log('stepped');
});

fsm.observe({
    onStep: function() { console.log('stepped');         }
    onA:    function() { console.log('entered state A'); }
    onB:    function() { console.log('entered state B'); }
});

var fsm = new StateMachine({
    init: 'A',
    transitions: [
      { name: 'step', from: 'A', to: 'B' }
    ],
    methods: {
      onStep: function() { console.log('stepped');         }
      onA:    function() { console.log('entered state A'); }
      onB:    function() { console.log('entered state B'); }
    }
});
```

> 生命周期事件参数

  * **transition** - the transition name
  * **from**       - the previous state
  * **to**         - the next state

```
var fsm = new StateMachine({
    transitions: [
      { name: 'step', from: 'A', to: 'B' }
    ],
    methods: {
      onTransition: function(lifecycle, arg1, arg2) {
        console.log(lifecycle.transition); // 'step'
        console.log(lifecycle.from);       // 'A'
        console.log(lifecycle.to);         // 'B'
        console.log(arg1);                 // 42
        console.log(arg2);                 // 'hello'
      }
    }
});

fsm.step(42, 'hello'); // 自定义参数
```

> 生命周期事件名称

```
 var fsm = new StateMachine({
    transitions: [
      { name: 'do-with-dash',       from: 'has-dash',        to: 'has_underscore'   },
      { name: 'do_with_underscore', from: 'has_underscore',  to: 'alreadyCamelized' },
      { name: 'doAlreadyCamelized', from: 'alreadyCamelize', to: 'has-dash'         }
    ],
    methods: {
      onBeforeDoWithDash:         function() { /* ... */ },
      onBeforeDoWithUnderscore:   function() { /* ... */ },
      onBeforeDoAlreadyCamelized: function() { /* ... */ },
      onLeaveHasDash:             function() { /* ... */ },
      onLeaveHasUnderscore:       function() { /* ... */ },
      onLeaveAlreadyCamelized:    function() { /* ... */ },
      onEnterHasDash:             function() { /* ... */ },
      onEnterHasUnderscore:       function() { /* ... */ },
      onEnterAlreadyCamelized:    function() { /* ... */ },
      onAfterDoWithDash:          function() { /* ... */ },
      onAfterDoWithUnderscore:    function() { /* ... */ },
      onAfterDoAlreadyCamelized:  function() { /* ... */ }
    }
  });
```

> 生命周期事件顺序

  * `onBeforeTransition`   - fired before any transition
  * `onBefore<TRANSITION>` - fired before a specific TRANSITION
  * `onLeaveState`         - fired when leaving any state
  * `onLeave<STATE>`       - fired when leaving a specific STATE
  * `onTransition`         - fired during any transition
  * `onEnterState`         - fired when entering any state
  * `onEnter<STATE>`       - fired when entering a specific STATE
  * `on<STATE>`            - convenience shorthand for `onEnter<STATE>`
  * `onAfterTransition`    - fired after any transition
  * `onAfter<TRANSITION>`  - fired after a specific TRANSITION
  * `on<TRANSITION>`       - convenience shorthand for `onAfter<TRANSITION>`

> 取消过渡

通过返回“false”来取消转换生命周期事件

  * `onBeforeTransition`
  * `onBefore<TRANSITION>`
  * `onLeaveState`
  * `onLeave<STATE>`
  * `onTransition`