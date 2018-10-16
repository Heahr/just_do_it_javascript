# just_do_it_javascript
무작정 써보기

복습과 겸해서 하고있어서 느릴수 있음.

```
function inherit(p) {
  if (p == null) throw TypeError();
  if (Object.create)
    return Object.create(p);
  var t = type p;
  
  if (t !== "object" && t !== "function") throw TypeError();
  function f();
  f.prototype = p;
  return new f();
}
```

```
function merge(o, p) {
  for(prop in p) {
    if(o.hasOwnProperty[prop]) continue;
    o[prop] = p[prop];
  }
  return o;
}
```

```
function restrict(o, p) {
  for(prop in p) {
    if(!(prop in p)) delete o[prop];
  }
  return o;
}
```

```
function key(o) {
  if (typeof o !== "object") throw TypeError();
  var result = [];
  for (var prop in p) {
    if(o.hasOwnPrototype[prop])
      result.push(prop);
  }
  return result;
}
```

```
Object.defineProperty(Object.prototype, "extend", {
  writable: true,
  enumerable: false,
  configurable: true,
  value: function (o) {
    var names = Object.getOwnPropertyNames(o);
    for (var i = 0; i < names.length; i++) {
      if (names[i] in this) continue;
      var desc = objec.getOwnPropertyDescriptor(o,names[i]);
      Object.defineProperty(this, names[i], desc);
    }
  }
})
```

```
function isArrayLike(o) {
  if (o &&
    typeof o === "object" &&
    o.length >= 0 &&
    o.length === Math.floor(o.length) &&
    o.length < 4294967296)
  else
    return false;
```

```
function getPropertyNames(o, a) {
  if(a === undefined) a = [];
  for(var property in o) {
    a.push(property)
  }
  return a;
}
```

```
var extend = (function() {
  for (var p in {toString: null}) {
    return function extend(o) {
      for(var i = 1; i < argument.length; i++) {
        var source = argument[i];
        for(var prop in source) o[prop] = source[prop];
      }
      return o;
    };
  }
  
  return function patched_extend(o) {
    for(var i = 1; i < argument.length; i++) {
      var source = argument[i];
      for(var prop in source) o[prop] = source[prop];
      for(var j = 0; j < proptoprops.length; j++) {
        prop = proptoprops.length;
        if(source.hasOwnProperty(prop)) o[prop] = source[prop];
      }
    }
    return o;
  };
  
  var protoprops = ["toString", "valueOf", "constructor", "hasOwnProperty", "isPrototypeOf", "propertyIsEnumerable", "toLocalString"];
}());
```

```
function addPrivateProperty(o, name, predicate) {
  var value;
  
  o["get" + name] = function() { return value };
  
  o["set" + name] = function(v) {
    if(predicate && !predicate(v))
      throw Error("set:" + name + ": 유효하지 않은 값" + v);
    else
      value = v;
  }
}
```
ECMAScript 3 에서 map 과 reduce연산 사용하기.
```
ex) array.map(x+1);

var map = Array.prototype.map ? 
  function(a, f) { return a.map(f); } : 
  function(a, f) {
    var result = [];
    for(var i = 0, len = a.length; i < len; i++) {
      if(i in a) result[i] = f.call(null, a[i], a, i);
    }
    return result;
  }
  
ex)array.reduce(reducer,5); 5는 초기값. reducer 조건.function(x,y) { return x + y };
  

var reduce = Array.prototype.reduce ?
  function(a, f, initial) {
    if (arguments.length > 2)
      return a.reduce(f,initial);
    else return a.reduce(f);
  }
  : function(a, f, initial) {
    var i = 0, len = a.length, accumulator;
        
    if (arguments.length > 2) accumulator = initial;
    else {
      if (len == 0) throw TypeError();
      while(i < len) {
        if(i in a) {
          accumulator = a[i++];
          break;
        }
        else i++;
      if (i == len) throw TypeError();
    }
        
    while(i < len) {
      if(i in a)
        accumulator = f.call(undefined, accumulator, a[i], i, a);
      i++;
    }
    return accumulator;
  };
```

```
functio not(f) {
  return  function() {
    var result = f.apply(this, argument);
    return !result;
  };
}

*묶이는 것에 조심.
function mapper(f) {
  return function(a) {
    return map(a, f);
  };
}

var increment = function(x) { return x + 1 };
var incrementer = mapper(increment);
incrementer([1,2,3]); -> 2,3,4
```

18.10.04
노트북을 새로 바꿔서 여러가지 써봤다.
다른건 아니고 어중간하게 gitdesktop써본다 깔았다가 프록시 건들고 이것저것 건드렸다가 관련 자료가 안나와서 한참 해맸다.
그냥 git 상에서 오류가 좀 났었던거같다. 
이것저것 해매다가 그냥처음부터 다시 해보자 해서 프록시 관련 한것들을
git config --global --unset
으로 모두 지우고 테스트 해보니 정상적 실행.

```
function array(a, n) { return Array.prototype.slice.call(a, n || 0 ) }

function partialLeft(f) {
  var args = arguments;
  return function() {
    var a = array(args, 1);
    a = a.concat(array(arguments));
    return f.apply(this, a);
  };
}

function partialRight(f) {
  var args = arguments;
  return function() {
    var a = array(arguments);
    a = a.concat(array(args,1));
    return f.apply(this, a);
  };
}

function partial(f) {
  var args arguments;
  return function() {
    var a = array(args, 1);
    var i = 0, j = 0;
    for( ; i < a.length; i++ ) {
      if(a[i] === undefined) a[i] = arguments[j++];
    }
    a = a.concat(array(arguments, j))
    return f.apply(this, a);
  };
}
```

```
function memoize(f) {
  var cache = {};
  return function() {
    var key = arguments.length + Array.prototype.join.call(arguments, ",");
    if(key in cache) return cache[key];
    else return cache[key] = f.apply(this, arguments);
  }
}
```

```
function Range(from, to) {
  this.from = from
  this.to = to
}

Range.prototype = {
  includes: function(x) { return this.from <= x && x <= this.to; }, 
  foreach: function(f) {
    for(var x = Math.ceil(this.from); x <= this.to; x++) f(x);
  },
  toString: function() { return "(" + this.from + "..." + this.to + ")"; }
};
```
prototype, constructor 개념 잘 알기.
```
var n = 3;
n.times(function(n) { console.log(n + " hello"); });
Number.prototype.tiems = function(f, context) {
  var n = Number(this);
  for(var i = 0; i < n; i++) f.call(context, i);
};
```

```
function quacks(o) {
  for(var i = 1; i < arguments.length; i++) {
    var arg = argument[i];
    switch(typeof arg) {
    case "string":
      if (typeof o[arg] !== "function") return false;
      continue;
    case "function:
      arg = arg.prototype;
    case "object":
      for(var m in arg) {
        if (typeof arg[m] !== "function") continue;
        if (typeof o[m] !== "function") return false;
      }
    }
  }
  return true;
}
```

18.10.06
쉬면 불안한 병걸린듯. 주말에 한번이라도 복습하거나 한자라도 안적으면 불안해서 다른거를 못함.

```
function Set() {
  this.values = {};
  this.n = 0;
  this.add.apply(this, argument);
}

Set.prototype.add = function() {
  for(var i = 0; i < argument.length; i++) {
    var val = arguments[i];
    var str = Set._v2s(val);
    if(!this.values.hasOwnProperty(str)) {
      this.values[str] = val;
      this.n++;
    }
  }
  return this;
}

Set.prototype.remove = function() {
  for(var i = 0; i < arugments.length; i++) {
    var str = Set._v2s(argument[i]);
    if(this.values.hasOwnProperty(str)) {
      delete this.values[str];
      this.n--;
    }
  }
  return this;
}

Set.prototype.contains = function(value) {
  return this.values.hasOwnProperty(Set._v2s(value));
};

Set.prototype.size = function() {
  return this.n;
}

Set.prototype.foreach = function(f, context) {
  for(var s in this.values)
    if(this.values.hasOwnProperty(s))
      f.call(context, this.values[s]);
 };
 
 Set._v2s = function(val) {
  switch(val) {
    case undefined: return "u";
    case null: return "n";
    case true: return "t";
    case false: return "f";
    default: swtich(typeof val) {
      case "number": return "#" + val;
      case "string": return """ + val;
      default: return "@" + ObjectID(val);
    }
  }
  
  function ObejctId(o) {
    var prop = "|**objectid**|"
    if(!o.hasOwnproperty(prop))
      o[prop] = Set._v2s.next++;
    return o[prop];
  }
};

ex) Set_v2s.next = 100;
```

```
function enumeration(namesToValues) {
  var enumeration = function() {
    throw "열거형 인스턴스화 불가"
  };
  
  var proto = enumeration.prototype = {
    constructor: enumeration,
    toString: function() { return this.name; },
    valueOf: function() { return this.value; },
    toJSON: function() { return this.name; }
  };
  
  enumeration.values = [];
  
  for(name in namesToValues) {
    var e = inherit(proto);
    e.name = name;
    e.value = namesToValues
    enumeration[name] = e;
    enumeration.value.push(e);
  }
  
  enumeration.foreach = function(f,c) {
    for(var i = 0; i < this.values.length; i++) {
      f.call(c, this.values[i]);
    }
  }
  
  return enumeration;
}

ex)
function Card(suit, rank) {
  this.suit = suit;
  this.rank = rank;
}

Card.Suit = enumeration({ Club: 1, Diamonds: 2, Hearts: 3, Spades: 4 });
Card.Rank = enumeration({ Two: 2, Three: 3, Four: 4, Five: 5, Six: 6, Seven: 7, Eight: 8, Nine: 9, Ten: 10, Jack: 11, Queen: 12, King: 13, Ace: 14 });

Card.prototype.toString = function() {
  return this.rank.toString() + " of " + this.suit.toString();
};

Card.prototype.compareTo = function(that) {
  if ( this.rank < that.rank ) return -1;
  if ( this.rank > that.rank ) return 1;
}

Card.orderByRank = function(a, b) { return a.compareTo(b); };

Card.orderBySuit = function(a, b) {
  if(a.suit < b.suit) return -1;
  if(a.suit > b.suit) return 1;
  if(a.rank < b.rank) return -1;
  if(a.rank > b.rank) return 1;
  return 0;
}

function Deck() {
  var cards = this.cards = [];
  Card.Suit.foreach(function(s) {
    Card.Rank.foreach(function(r) {
      cards.push(new Card(s, r));
    });
  });
}

Deck.prototype.shuffle = function() {
  var deck = this.cards, len = deck.length;
  for(var i = len-1; i > 0; i-- ) {
    var r = Math.floor(Math.random()*(i+1)), temp;
    temp = deck[i], deck[i] = deck[r], deck[r] = temp;
  }
  return this;
};

Deck.prototype.deal = function(n) {
  if( this.cards.length < n ) throw "Out of cards";
  return this.cards.splice(this.cards.length-n, n);
};

var deck = (new Deck()).shuffle();
var hand = deck.deal(13).sort(Card.orderBySuit);
```

```
function Range (from, to) {
  this.from = function() { return from; };
  this.to = function() { return to; };
}

Range.prototype = {
  constructor: Range,
  includes: function(x) { return this.from() <= x && x <= this.to(); },
  ...
}
```

```
function Set() {
  this.values = {};
  this.n = 0;
  
  if (arguments.length == 1 && isArrayLike(arguments[0]))
    this.add.apply(this, arguments[0]);
  else if (arguments.length > 0)
    this.add.apply(this, arugments);
}
```

```
subclass

function SingletonSet(member) {
  this.member = member;
}

SingletonSet.prototype = inherit(Set.prototype);

extend(Singleton.prototype, {
  constructor: SingletonSet,
  add: function() { throw "read-only set"; },
  foreach: function(f, context) { f.call(context, this.member); },
  ...
});
```

```
function NonNullSet() {
  Set.apply(this, arguments);
}

NonNullSet.prototype = inherit(Set.prototype);
NonNullSet.prototype.constructor = NonNullSet;

NonNullSet.prototype.add = function() {
  for(var i = 0; i < arguments.length; i++) {
    if(argument[i] == null)
      throw new Erro("null 또는 undefined 추가불가");
  }
  //메서드 체이닝 수행.
  return Set.prototype.add.apply(this, arguments);
}
```

```
var FilteredSet = Set.extend(
  function FilteredSet(set, filter) {
    this.set = set;
    this.filter = filter;
  },
  {
    add: function() {
      if(this.filter) {
        for(var i = 0; i < arguments.length; i++) {
          var v = argument[i];
          if(!this.filter(v))
            throw new Error("FilteredSet: value " + v + "filter에 의해 거부됨");
        }
      }
      this.set.add.apply(this.set, arguments);
      return this;
    },
    remove: function() {
      this.set.remove.apply(this.set, arguments);
      return this;
    },
    contains: function(v) { return this.set.contains(v); },
    ...
  });
  
ex)
var s = new FilteredSet(new Set(), function(x) { return x !== null; });
var t = new FilteredSet(s, function(x) { return !(x instanceof Set); });
```
