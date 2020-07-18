## Example

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

