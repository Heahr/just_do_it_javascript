# just_do_it_javascript
무작정 써보기
하다가 익숙해 진다면 (https://github.com/airbnb/javascript) 를 꼭 다시한번 공부하기.

***
ES3: 대중적, IE8까지, 함수단위 스코프, 호이스팅, 프로토타입, 모듈화 미지원

ES5: The "use strict" Directive, String.trim()

Array
```
isArray(), forEach(), map(), filter(), reduce(), reduceRight(), every(), some(), indexOf(), lastIndexOf()
```
JSON.parse(), JSON.stringify(), Date.now(), Property Getters and Setters, New Object Property Methods

ES6: let, const, JavaScript default parameter values, Array.find(), Array.findIndex()
***
복습과 겸해서 하고있어서 느릴수 있음.

```
function inherit(p) {
  if (p == null) throw TypeError();
  if (Object.create)
    return Object.create(p);
  var t = type p;
  
  if (t !== 'object' && t !== 'function') throw TypeError();
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
  if (typeof o !== 'object') throw TypeError();
  var result = [];
  for (var prop in p) {
    if(o.hasOwnPrototype[prop])
      result.push(prop);
  }
  return result;
}
```

```
Object.defineProperty(Object.prototype, 'extend', {
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
    typeof o === 'object' &&
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

//하나의 클래스로 여러개의 파일로 분할해서 보기 편하기 사용할 때 사용
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
function Range(from, to) {
  var props = {
    from: { value: from, enumerable: true, writable: false, configurable: false },
    to: { value: to, enumerable: true, writable: false, configurable: false }
  };
  
  if(this instansof Range)                        //생성자 호출시 this객체의 프로퍼티를 정의함.
    Object.defineProperties(this, props);
  else                                            //아니면 팩터리 함수로 정의하고 새로운 Range객체생성후, props를 설정후 반환
    return Object.create(Range.prototype, props);
}
Object.defineProperties(Range.prototype, {
  includes: {
    value: function(x) { return this.from <= x && x <= this.to; }
  },
  foreach: {
    value: function(x) {
      for(var x = Math.ceil(this.from); x <= this.to; x++) f(x);
    }
  },
  toString: {
    value: function() { return "(" + this.from + "..." + this.to + ")"; }
});
```

```
캡슐화
function Range(from, to) {
  if(from > to)  throw new Error("try from <= to");
  
  function getFrom() { return from; }
  function getTo() { return to; }
  function setFrom(f) {
    if(f <= to) from = f;
    else throw new Error("try from <= to");
  }
  function setTo(t) {
    if(t >= from) to = t;
    else throw new Error("try to >= from");
  }
  
  Object.defineProperties(this, {
    from: { get: getFrom, set: setFrom, enumerable: true, configurable: false},
    to: { get: getTo, set: setTo, enumerable: true, configurable: false}
  });
}

Range.prototype = hideProps({
  constructor: Range,
  incldues: function(x) { return this.from <= && x <= this.to; },
  ....
});
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
es5 subclass
function StringSet() {
  this.set = Object.create(null);       //프로토타입없는 객체 생성.
  this.n = 0;
  this.add.apply(this, arguments);
}

//AbstractWritableSet의 서브클래스 StringSet, 
//object.create를 사용하여 한번의 호출로 슈퍼클래스의 프로토타입을 상속하고, 읽기전용이라 서브클래스를 정의하기가 까다로워 질수 있다.
StringSet.prototype = Object.create(AbstractWritableSet.prototype, {
  constructor: { value: StringSet },
  contains: { value: function(x) { return x in this.set; } },
  size: { value: function(x) { return this.n; } },      //?
  foreach: { value: function(f, c) { Object.keys(this.set).foreach(f, c); } },
  add: {
    value: function() {
      for(var i = 0; i < arguments.length; i++) {
        if(!(arguments[i] in this.set)) {
          this.set[arguments[i]] = ture;
          this.n++;
        }
      }
      return this;
    }
  },
  remove: {
    value: function() {
      for(var i = 0; i < arguments.length; i++) {
        if(arguments[i] in this.set) {
          delete this.set[arguments[i]];
          this.n--;
        }
      }
      return this;
    }
  }
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

```
private 네임스페이스와 함수 유효범위
var Set = (functino invocation() {
  function Set() {
    this.value = {};
    this.n = 0;
    this.add.apply(this, arguments);
  }
  
  Set.prototype.contains = function(value) {
    return this.values.hasOwnProperty(v2s(value));
  };
  
  Set.prototype.size = function() { return this.n };
  ...
  functino v2s(val) { ... 전과동일 }
  function objectId(o) { ... 전과동일 }
  var nextId = 1;
  
  return Set;
}());

기본적 모듈 코드는 유효범위 내에 봉인되어 있어서 밖으로 꺼내야함.
var collections;
if (!collections) collections = {};
//sets 모듈 정의
collections.sets = (function namespace() {
  //지역번수와 지역함수 여기에 설정, 여러종류의 set클래스 이곳에 정의. (코드생략)
  
  //네임스페이스 객체를 반환함으로써 API를 외부로 내보냄.
  return {
    AbstractSet: AbstractSet,
    NotSet: NotSet,
    ...
  };
}());

//new를 사용하여 모듈 함수를 생성자처럼 호출한 다음 생성자 내에서 this에 API들을 할당함으로써 API 외부로 내보낼수 있음.
var collections;
if (!collections) collections = {};
collections.sets = (new function namespace() {
  ...
  //API를 this객체에 할당함으로써 외부로 내보냄.
  this.AbstractSet = AbstractSet;
  this.NotSet = NotSet;
  //아무값도 반환하지 않는다.
}());

다른 방법도 가능. -> 네임스페이스가 이미 정해져 있다면 해당 객체에 직접적으로 프로퍼티 설정 가능(어떤값도 반환필요없음)
var collections;
if (!collections) collections = {};
collections.sets = {};
(function namespace() {
  ...
  
  collections.sets.AbstractSet = AbstractSet;
  collections.sets.NotSet = NotSet;
  ...
  
  //앞에서 완료했기 떄문에 return 불필요.
}());
```

18/10/18
크로스코드랑는 게임을 보고나서 들은 소식은 오직 HTML5로만 만들었다는 사실.
뭐... 여기저기서 들은거이긴 하지만 재능보다는 많이 해보면서 느끼는게 중요하다 라고는 하지만.. 그 정도의 재능정도를 가지고 있어야 활용하고 만들고 무언가를 만들어 낼수 있지 않을까 싶었다 이것저것 해보면서 스펙트럼 늘리는것도 당연하게 중요한거고..
어중간하게 개발을 하고있지는 않은가.. 싶었다 재능도없으면서, 일단 노력만 죽어라 하는게 과연 그런사람들과 같이 일할수있을까..
우선 재미는 있지만 노력으로 충분하게 하고 있는가 싶었다. 공부는 죽어라해도 뭔가 자꾸 앞이 막힌 느낌이든다.


```
var url = /(\w+):\/\/([\w.]+)\/(\S*)/;
var text = "Visit my blog at http://www.example.com/~david";
var result = text.match(url);
if(!result != null){
  var fullurl = result[0];    -> http://www.example.com/~david 저장
  var protocol = result[1];   -> http
  var host = result[2];       -> www.example.com
  var path = result[3];       -> ~david
}
```

```
var pattern = /Java/g;
var text = "JavaScript is more fun than Java"
var result;
while((result = patern.exec(text)) != null) {
  alert("Matched " + result[0] + "'" + "at position " + result.index + "; next search begins at " + pattern.lastIndex);
}
```

```
E4X(ECMAScript for XML) for each
let o = {one: 1, two: 2, three: 3}
for (let p in o) console.log (p);       -> "one", "two", "three" 출력
for each (let v in o) console.log(v);   -> 1, 2, 3 출력
```

```
using nodejs ex)

var http = require('http');
var fs = require('fs');

var server = new http.Server();
server.listen(8000);

server.on("request", function (request, response) {
  var url = require('url').parse(request.url);
  
  if (url.pathname == "/test/delay") {
    var delay = parseInt(url.query) || 2000;
    response.writeHead(200, {"Content-Type": "text/plain; charset=UTF-8"});
    response.write("Sleeping for " + delay + " milliseconds..");
    setTimeout(function() {
      response.write("done");
      response.end();
    }, delay);
  }
  
  else if(url.pathname === "/test/mirror") {
    response.writeHead(200, {"Content-Type": "text/plain; charset=UTF-8"});
    response.write(request.method + " " + request.url + " HTTP/" + request.httpVersion + "\r\n");
    for(var h in request.headers) {
      response.write(h + ": " + request.headers[h] + "\r\n");
    }
    response.write("\r\n");
    request.on("data", function(chunk) { response.write(chunk); });
    request.on("end", function(chunk) { response.end(); });
  }
  
  else {
    var filename = url.pathname.substring(1);
    var type;
    switch(filename.substring(filename.lastIndexOf(".")+1)) {
      case "html":
      case "htm":       type = "text/html; charset=UTF-8"; break;
      case "js":        type = "application/javascript; charset=UTF-8"; break;
      case "css":       type = "text/css; charset=UTF-8"; break; break;
      case "txt":       type = "text/plain; charset=UTF-8"; break;
      case "manifest":  type = "text/cache-manifest; charset=UTF-8"; break;
      default:          type = "application/octet-stream"; break;
    }
    
    fs.readFile(Filename, function(err, content) {
      response.writeHead(404, {"Content-Type": "text/plain; charset=UTF-8"});
      response.write(err.message);
      response.end();
    }
    else {
      response.writeHead(200, {"Content-Type": type});
      response.write(content);
      response.end();
    }
  });
});
```

```
...
<style>
.reveal * { display: none; }
.reveal *.handle { display: block; }
</style>
<script>
window.onload = function() {
  var elements = document.getElementsByClassName("reveal");
  for(var i = 0; i < elements.length; i++) {
    var elt = elements[i];
    var title = elt.getElementsByClassName("handle")[0];
    title.onclick = function() {
      if(elt.className == "reveal") elt.className = "revealed";
      else if(elt.className == "revealed") elt.className = "reveal";
    }
  }
}
</script>
...
<body>
<div class="reveal">
<h1 class="handle"> ??? </h1>
</div>
</body>
```

```
...
<script>
function displayTime() {
  var elt = document.getElementById("clock");
  var now = new Date();
  elt.innerHTML = now.toLocaleTimeString();
  setTimeout(displayTime, 1000);
}
window.onload = displayTime;
</script>
...
<body>
<h1>clock</h1>
<span id="clock"></span>
</body>
...
```

```
<a href="javascript: var e = "", r = "";
  do {
    e = prompt("Expression: " + e + "\n" + r + "\n", e);
    try {
      r = "Result: " + eval(e);
    } catch {
      r = ex;
    } while(e);
    void 0;
'>
Javascript Evaluator
</a>
```

```
function loadasync(url) {
  var head = document.getElementsByTagName("head")[0];
  var s = document.createElement("script");
  s.src = url;
  head.appendChild(s);
}
```

```
function onLoad(f) {
  if (onLoad.loaded)
    window.setTimeout(f, 0);
  else if (window.addEventListener)
    window.addEventListener("load", f, false);
  else if (window.attachEvent)
    window.attachEvent("onload", f);
}

onload.loaded = false;                       -> 문서를 모두 불러오지 못했을때 나타내는 플래그 지정해서 시작.
onLoad(function() { onLoad.loaded = ture }); -> 문서를 불러올때 플래그를 지정할 함수 등록.
```

```
http://jquery.com

css클래스 clicktohide의 클릭 반응.
$(".clicktohide").click(function() { $(this).slideUp("slow"); });

var img = $("<img/>", { src: url, css: { borderWidth: 5 }, click: handleClick });

$(":header").map(function() { return this.id; }).toArray().sort();
$("div").each(function() { if ($(this).is(":hidden")) return; //나머지 작업 }); -> 숨겨진요소 무시

$("form").attr("action");                           -> 첫 form 요소 action 속성 조회
$("#icon").attr({src:"banner.gif", width: 720});    -> 한번에 여러번 속성지정.
$("a").attr("target", "_ blank");                   -> 모든링크 새창 열기
$("a").attr({target: function() {...});             -> 함수 넘기기 가능

$("h1").css({ backgroundColor: "black", textColor: "white" });
$("h1").css("font-size", function(i, curval) {
  return Math.round(1.25 * parseInt(curval));
});

$("h1").addClass("hihlte");                         -> 모든 h1 태그에 클래스 하나 추가.
$("h1").toggleClass(function(n) { return "section" + n });    -> 계산된 클래스를 토글한다(있으면 삭제, 없으면 추가)

$("input:redio[name=ship]:checked").val()           -> 체크된 라디오 버튼의 값을 가져온다.
$("#email").val("Invalid email address")            -> 텍스트 필드에 값을 지정한다.
$("input:text").val(function() { return this.defaultValue; })   -> 모든 텍스트 필드를 기본 값으로 초기화한다.

var title = $("head title").text()                  -> 문서 제목을 가져온다.
var headline = $("h1").html()                       -> 첫 번째 h1 태그의 html을 가져온다.
$("h1").text(function(n, currnet) {                 -> 각 헤딩 태그 앞에 절 번호를 넣는다.
  return "$" + (n+1) + ":" + current });
  
var elt = $("#sprite");                             -> 움직이려는 요소 선택
var position = elt.offset();                        -> 현재위치를 가져온다.
position.top += 100;                                -> y좌표를 바꾼다.
elt.offset(position);                               -> 새 위치를 지정한다.
//모든 h1 태그를 문서 내 원래 위치를 기준으로 오른쪽으로 이동시킨다.
$("h1").offset(function(index, curpos) {
  return { left: curpos.left + 25*index, top: curpos.top }; });

function page(n) {
  var w = $(window);                                -> window를 jQuery 객체로 감싼다.
  var pagesize = w.height();                        -> 페이지 크기를 가져온다.
  var current = w.scrollTop();                      -> 현재 스크롤바 위치를 가져온다.
  w.scrollTop(current + n*pagesize);                -> 새 스크롤바 위치를 지정한다.
}

$("div").data("x", 1);                              -> 데이터를 지정한다.
$("div.nodata").removeData("x");                    -> 이 데이터를 제거한다.
var x = $("#mydiv").data("x");                      -> 데이터를 조회한다.

befor, after 가능
$("hr").replaceWith("<br/>");                       -> <hr/> 태그를 <br/>로 교체.
$("h2").each(function() {                           -> h2 내용유지하면서 h1으로 교체.
  var h2 = $(this);
  h2.replaceWith("<h1>" + h2.html() + "</h1>");
});
//h1의 내용앞에 절 구분자를 넣는 다른방법.
$("h1").map(function() { return this.firstChild; }).before("$");

$("<br/>+massage").appendTo("#log");                -> id가 log인 요소에 html을 덧 붙인다.
$(document.createTextNode("$")).prependTo("h1");    -> 모든 h1요소에 텍스트 노드를 덧붙인다.
$("<hr/>").insertBefore("h1");                      -> 모든 h1 앞쪽에 구분선 추가
$("<hr/>").insertAfter("h1");                       -> 모든 h1 뒤쪽에 구분선 추가.
$("<br/>").replaceAll("hr");                        -> hr/을 br/로 모두 바꾼다.

//문서 끝에 id 가 linklist인 신규 div 추가.
$(document.body).append("<div id='linklist'><h1>List Links</h1></div>");
$("a").clone().appendTo("#linklist");               -> 문서의 모든 링크를 복사해서 새로운 div 에 넣는다.
$("#linklist > a").after("<br/>");                  -> 각 링크 뒤쪽에 br태그를 삽입해서 줄바꿈을 한다.

$("h1").wrap(document.createElement("i"));          -> i 태그로 h1 태그를 감싼다.
$("h1").wrapInner("<i/>");                          -> 모든 h1 태그의 내용을 감싼다. 문자열 전달인자 사용이 더 쉽다.
//첫 번째 문단은 a와 div 로 감싸고
$("body>p:first").wrap("<a name='lead'><div class='first'></div></a>");
//나머지 문단은 별도의 div 로 감싼다.
$("body>p:not(:first)").wrapAll("<div class='rest'></div>");

//jquery로 이벤트 다루기
$("p").click(functino() { $(this).css("background-color", "gray"); });
$("<img/>", { src: image_url, alt: image_description, className: "translucent_image", click: function() { $(this).css("opacity", "50%"); } });
//이벤트 등록할때에 bind함수를 사용할 수 있음.
$('p').bind('click', f);
$('a').bind('mouseenter mouseleave', f);              -> hover 형태
$('a').bind('mouseover.myMod', f);
$('a').bind('mouseout.myMod.yourMod', f);             -> myMod 와 yourMod에 함수 f 할당.
//마우스 이벤트 핸들러 등록 해제
$('a').unbind("mouseover.myMod mouseout.myMod");
$('a').unbind(".myMod");                              -> myMod 네임스페이스의 어떤 이벤트의 핸들러는 상관없이 해제한다.
$('a').unbind("click.ns1.ns2");                       -> ns1과 ns2 네임스페이스 둘 모두에서 clikc핸들러는 해제한다.
$('#mybutton').unbind('click', myClickHandler);
//이벤트 일으키기
$('#my_form').submit();                               -> $('#my_form').trigger('submit');
$('button').trigger('click!');                        -> 네임스페이스가 없는 click 이벤트의 핸들러를 실행한다.
$('#button1').click(function(e) { $('#button2').trigger(e); });       -> button1의 click 이벤트 핸들러가 button2의 click도 일으킨다.
$('#button1').trigger('click', true);                 -> 한 개의 확장 전달인자 넘기기
$('#button1').trigger('click, [x, y, z]);             -> 세 개의 확장 전달인자 넘기기
//사용자 정의 이벤트
$('#logoff').click(function() {
  $.event.trigger('logoff');
  window.location = 'logoff.php';
});
//라이브 이벤트
$(document).delegate('a', 'mouseover', linkHandler);  -> 원래 존재하던 a요소와 새로 추가된 a요소 모두에서 mouseover 이벤트 관리
->bind()를 쓰고 동적으로 변하는 부분에는 delegate()를 사용하는 방법도 있다.
$('a').bind('mouseover', linkHandler);                -> 정적 링크의 정적 이벤트 핸들러
$('.dynamic').delegate('a', 'mouseover', linkHandler);-> 동적으로 갱신된 문서 영역의 라이브 이벤트 핸들러.
-> jQuery에는 라이브 이벤트 등록에 사용될 수 있는 live()란 메서드가 있다.
-> bind()와 동일한 두개 혹은 세개의 전달인자를 갖고 있으며, 실제로 더 많이 사용된다.
-> 앞의 delegate()의 사용법을 보여준 두번째 예재는 live()를 사용하여 다음처럼 작성할 수도 있다.
$('a').live('mouseover', linkHandler);
$('a', $('.dynamic')).live('mouseover', linkHandler);
-> jQuery 객체는 context와 selector 프로퍼티를 통해서 이 값에 접근할 수 있게 만든다.
-> 보통은 전달인자 하나만으로 $()를 호출하며, 이러면 컨텍스트는 자동으로 현재 문서가 된다.
-> 따라서 x라는 jQuery 객체에 대한 다음 두 코드는 같다.
x.live(type, handler);
$(x.context).delegate(x.selector, type, handler);
-> 라이브 이벤트 핸들러를 등록 해제할 때는 die()나 undelegate()를 사용한다.
$('a').die('mouseover');                              -> mouseover 이벤트의 모든 라이브 이벤트 핸들러를 제거한다.
$('a').die('mouseover', linkHandler);                 -> 지정한 라이브 이벤트 핸들러 하나만 제거한다.
->다음으로 대체가능.
$(document).undelegate('a');                          -> a태그의 모든 라이브 이벤트 핸들러를 제거한다.
$(document).undelegate('a', 'mouseover', linkHandler);-> linkHandler 핸들러만 mouseover 라이브 이벤트 핸들러를 제거한다.

//애니메이션 효과 주기
-jQuery.fx.speeds에 새로운 문자열/숫자 매핑 값을 추가하면 새로운 지속 시간명을 정의할 수 있다.
jQuery.fx.speeds["medium-fast"] = 300;
jQuery.fx.speeds["medium-slow"] = 500;

$("#message").fadeIn();                               -> 요소가 400밀리초 속도로 나타난다.
$("#message").fadeOut().("fast");

-> 요소를 빠르게 노출하고, 노출되면 그 안에 텍스트를 출력한다.
$("message").fadeIn("fast", function() { $(this).text("Hello world"); });

$("#blinker").fadeIn(100).fadeOut(100).fadeIn(100).fadeOut(100).fadeIn();

-> 전달인자가 아닌 객체 프로퍼티로 지속 시간과 콜백을 전달한다.
$("#message").fadeIn({
  duration: "fast",
  complete: function() { $(this).text("Hello World"); }
});

//간단한 시각 효과들
***
fadeIn(), fadeOut(), fadeTo(), show(), hide(), toggle(), slideDown(), slideUp(), slideToggle()
***
-> 모든 이미지를 숨겼다가 노출하고, 위로 쓸어 올렸다가 쓸어내린다.
$("img").fadeOut(),show(300).slideUp().slideToggle();

->사용자 정의 애니메이션
$("img").animate({ height: 0 });                        -> 높이가 0으로 줄어든다.

$("sprite.animate({
  opacity: .25,                                         -> 불투명도 0.25로 애니메이션
  font-size: 10                                         -> 폰트 크기를 10픽셀로 애니메이션
},{
  duration: 500,                                        -> 0.5초 동안 애니메이션
  complete: function() {                                -> 애니메이션 후 호출할 콜백 함수
    this.text("goodbye");                               -> 요소의 텍스트 변경
  }
});

-> 애니메이션 프로퍼티 객체
$("p").animate({
  "margin-left": "+=.5in",                              -> 문단 들여쓰기를 늘린다.
  opacity: "-=.1"                                       -> 그리고 불투명도는 줄인다.
});

$("img").animate({                                      -> 오른쪽으로 쓸어내리는 효과.
  width: "hide",
  borderLeft: "hide",
  borderRight: "hide",
  paddingLeft: "hide",
  paddingRight: "hide"
});

-> 사용자 정의 easing 함수를 지정할 수 있는 specialEasing 옵션을 사용한다.
$("img").animate({ width: "hide", height: "hide", opacity: "hide" },
  {specialEasing: { width: "linear", height: "liner" }});
-> animate()의 첫 전달인자에서 값을 [목표 값, easing 함수] 형태의 배열로 넘긴다.
$("img").animate({
  width: ["hide", "linear"], height: ["hide", "linear"], opacity: "hide" });
  
//시각 효과의 취소, 지연, 대기열 처리
-> 마우스를 이미지에 올리면 불투명하게 된다. 하지만 마우스 이벤트로 애니메이션 대기열을 만들진 않는다.
$("img").bind({
  mouseover: function() { $(this).stop().fadeTo(300, 1.0); },
  mouseout: function() { $(this).stop().fadeTo(300, 0.5); }
});
-> 재빨리 반만 흐리게 만들고, 기다렸다가, 쓸어 올린다.(slide up).
$("img").fadeTo(100, 0.5).delay(200).slideUp();
-> 이미지 위를 마우스로 한번에 빨리 지나가는 경우에는 애니메이션 실행되지 않는다.
$("img").bind({
  mouseover: functino() { $(this).stop(true).delay(100).fadeTo(300, 1.0); },
  mouseout: function() { $(this).stop(true).fadeTo(300, 0.5); }
});
-> 문서 요소를 나타나게 하고, 기다렸다가 그 안에 텍스트를 뿌리고 테두리를 두껍게 한다.
$("#message").fadeIn().delay(200).queue(function(next) {
  $(this).text("Hello world");                          -> 문자열 출력
  next();                                               -> 실행 대기열의 다음 작업 실행
}).animate({ borderWidth: "+=10px;" });                 -> 테두리를 두껍게 한다.

-> 요소 e의 애니메이션 실행 대기열에 함수 f를 추가할 경우, 유틸리티 함수나 메서드의 형태로 작성하기
$(e).queue(f);
-> 요소 e를 감싼 jQuery 객체를 생성해서 queue() 메서드를 호출한다.
jQuery.queue(e, f);                                     -> 유틸리티 함수인 jQuery.queue()를 호출한다.

//jQuery를 이용한 Ajax
-> 60초마다 최신 상태 값 결과를 불러와서 출력한다.
setInterval(function() { $("#stats").load("status_report.html"); }, 60000};

-> 날씨 정보에서 온도 섹션만 불러와 출력한다.
$('#temp').load("weather_report.html #temperature");

-> 지정한 우편번호의 날씨 정보를 불러온다.
$('#temp').load("us_weather_report.html", zipcode=02134");
-> 문자열 대신 객체를 사용해서 온도를 화씨(F)로 지정한다.
$('#temp').load("us_weather_report.html", {zipcode:02134, units:'F' });

//jQuery.getScript()
-> 어떤 서버에서 동적으로 script를 불러온다.
jQuery.getScript("http://example.com/js/widget.js");
-> 라이브러리를 불러오고 완료되면 사용한다.
jQuery.getScript("js/jquery.my_plugin.js", function() {
  $('div').my_plugin();                               -> 불러온 라이브러리를 사용한다.
}

//jQuery.getJSON()
-> data.json이 '{"x":1, "y":2}' 텍스트를 갖고 있다고 가정한다.
jQuery.getJSON("data.json", function(data) {
  // 여기서 data는 {x:1, y:2} 객체다.
});

//jQuery.get()과 jQuery.post()

$(document).ready(function() {
  $("button").click(function() {
    $get("demo_text.asp", function(data, status) {
      alert("data:" + data + "\n status:" + status);
    });
  });
});

-> 서버에 텍스트를 요청해서 alert 대화상자에 출력한다.
jQuery.get("debug.txt", alert);

//jQuery.ajax() 함수
jQuery.ajax({
  type: "GET",                                      -> HTTP 요청방법
  url: url,                                         -> 가져올 데이터 URL
  data: null,                                       -> URL에 데이터를 추가하지 않는다.
  dataType: "script",                               -> 가져와서 스크립트로 응답 값을 실행한다.
  success: callback                                 -> ajax가 잘 실행되면 이 함수를 호출한다.
});

jQuery.ajaxSetup({
  timeout: 2000,                                    -> Ajax 요청 후 2초가 지나면 중단한다.
  cache: false                                      -> URL에 타임스탬프를 붙여서 브라우저 캐시를 무효화한다.
});

공통옵션
type  url data  dataType  contentType timeout cache ifModified  global
콜백  context beforeSend  success error complete  

기타 옵션과 후크
async dataFilter  jsonp jsonpCallback processData scriptCharset traditional
username.password xhr 

//Ajax 이벤트
$("#loading_animation").bind({
  ajaxStart: function() { $(this).show(); },
  ajaxStop: function() { $(this).hide(); }
});

//jQuery 선택자와 선택관련 메서드
#id .class  :animated :button :checkbox :checked  :file :disabled :header :nth-child  :submit 등등...

결합 선택자
"blockquote i"                                    -> <blockqute> 안에있는 i를 선택
"ol > li"                                         -> <ol> 바로 아래 자식 요소 중 <li>를 선택
"#output + *"                                     -> id = "output"인 요소 바로 뒤의 형제 요소
"div.note > h1 + p"                               -> <div class="note"> 바로 아래 자식요소중 <h1> 뒤에 있는 <p>

선택자 그룹
"h1, h2, h3"                                      -> h1, h2, h3 모두 선택
"#p1, #p2, #p3"                                   -> ID가 p1, p2, p3 요소 선택
"div.note, p.note"                                -> class='note'인 <div>, <p> 선택
"body>p, div.ote>p"                               -> <body>와 <div class="note">의 자식 요소 <p> 선택

선택 관련 메서드
var paras = $("p");
paras.first()                                     -> 첫 번째 <p>만 선택한다.
paras.last()                                      -> 마지막 <p>만 선택한다.
paras.eq(1)                                       -> 두번째 <p>를 선택한다
paras.eq(-2)                                      -> 뒤에서 두 번째 <p>를 선택한다.
paras[1]                                          -> 자신의 두번째 <p>. jQuery 객체가 아니다.
$("p").slice(2, 5)                                -> 세 번째, 네 번째, 다섯 번째 <p>를 선택한다.
$("div").slice(-3)                                -> 끝에서 <div> 세 개를 선택한다.
$("div").filter(".note")                          -> $("div.note")와 같다.
$("div").filter($(".note"))                       -> $("div.note")와 같다.
$("div").filter(functino(idx) { return idx%2 == 0 })  -> $("div.even")와 같다.
$("div").not("#header, #footer")                  -> 특정 요소 두 개를 제외한 모든 <div> 요소
$("p").has("a[href]")                             -> 링크를 포함한 <p>
->모든 div와 p를 선택하는 방법들
$("div, p")                                       -> 선택자 그룹을 사용한다
$("div").add("p")                                 -> add에 선택자를 넘긴다.
$("div").add($("p"))                              -> add에 jQuery 객체를 넘긴다,
var paras = document.getElementsByTagName("p")    -> paras는 유사 배열 객체
$("div").add(paras)                               -> add에 요소 배열을 넘긴다.

컨텍스트로 선택물 사용하기
$("div").find("p")                                -> div 안에 있는 p 요소를 찾는다. $("div p")와 같다.
-> id가 header나 footer인 요소의 자식 요소에서 span을 모두 찾는다.
-> $("header>span, #footer>span")와 같다.
$(#header, #footer").children("span")
$("h1").next("p")
$("h1").prev()
#("footer").nextAll("p")                          -> #footer 요소 뒤쪽의 모든 <a> 형제 요소
$("footer").prevAll()                             -> #footer 요소 앞쪽의 모든 형제 요소
$("li").parent()                                  -> ul과 ol같은 목록의 부모 요소를 선택한다.
$("a[href]").parents("p")                         -> 링크를 가진 p 요소들
$("a[href]").closest("div")                       -> 링크를 가진 가장 가까운 div
$("a[href]").parentsUntil(":not(div)")            -> a를 직접 감싼 모든 div

18.11.23 알고리즘 공부 다시시작
현제 하고있는 곳에서도 그렇고 인턴이던 스타트업이던 기본적으로 계속해서 알고리즘 시험을 보고싶어하고 보고있다.
기본적으로 어느정도 되는지 알기위해 거름망을 설치하는 느낌이랄까.. 아무튼 그것을 위해서 조금 늦은감이 없이 않아있지만..
ICPC했던 기억을 좀 떠올려서 취업전까지 지속적으로 해야할것같다.

// 사전 선택물의 반전
-> 모든 div를 찾는다 그리고 그 안에서 p태그를 찾는다.
-> p에 highlight CSS 클래스를 적용하고 div에는 테두리를 준다.
var divs - $('div');
var paras = divs.find('p');
paras.addClass('highlight');
divs.css('border', 'solid black 1px');
// 여기서는 메서드 체인으로 처리한다.
$('div').find('p').addClass('highlight').end().css('border', 'solid black 1px');
// 또는 처리 순서를 변경해서 end() 호출을 피할 수 있다.
$('div').css('border', 'solid black 1px').find('p').addClass('highlight');

var sel = $('div'); 
sel.pushStack(document.getElementByTagName('p')); -> 선택물을 p 태그로 변경
sel.end();                                        -> div 태그로 복구한다.

$('div').find('p').andSelf().                     -> div 안에서 p 를 찾고 둘이 합친다.
  addClass('highlight').                          -> highlight CSS클래스 적용
  end().end().                                    -> $('div')로 돌리기 위해 스택을 두 번 빼온다.
  css('border', 'solid'black 1px');               -> div에 테두리를 준다.
```
18.11.24 첫눈
css는 kakaotalk관련 글에 정리해뒀으며, 개인적인 공부이기 때문에 따로 정리하지는 않겠습니다. 

실행환경이 정의 되지 않은 자바스크립트는 코드 조각일 뿐이다. 

//클라이언트 측 자바스크립트
Window 객체는 클라이언트 측 자바스크립트의 기능과 API에서 핵심적인 진입점 이며, 객체는 웹브라우저의 창이나 프레임을 나타내고, 따라서 특정 창을 Window 객체로 참조할 수 있다.
Window 객체는 location과 같은 프로퍼티들을 제공하는데, location프로퍼티는 Location 객체를 가리킨다. 이 객체는 창에 현재 보이는 URL을 나타내고 또한 새로 주어진 URL을 불러올 수 있게 해준다.
```
window.location = "http://www.oreilly.com/";
setTimeout(function() { alert("hello world"); }, 2000);
```
Window 객체가 전역 객체임에 주의한다. Window객체는 유효범위 체인의 최상위에 있으며, Window객체의 프로퍼티와 메서드가 사실 전역 변수와 전역함수 라는 의미다.

Window 객체의 가장 중요한 프로퍼티 중 하나인 document는 창에 표시된 내용에 해당하는 Document 객체를 참조한다. 
Document 객체에는, 주어진 id 속성값을 가진 문서 요소 하나를 반환하는 getElementById()와 같은 중요한 메서드들이 있다. 여기서 문서요소란 HTML 태그 쌍과 그 안의 내용 전부를 담고 있는 HTML요소다.
```
var timestamp = document.getElementById("timestamp");
```

문서의 내용을 조회하고, 탐색하고 수정하는 기법도 가능하다.
```
timestamp.style.backgroundColor = "yellow";
timestamp.className = "highlight";
```
 style과 className 프로퍼티, CSS를 구성하는 다른 기법들도 가능하다.
 이벤트 핸들러 프로퍼티도 Window, Document, Element 객체의 중요한 프로퍼티인데, 어떤 이벤트가 발생할 때 비동기로 호출할 함수를 지정할 수 있다. 즉, 문서를 꾸미기 위해 window, document, element의 동작을 수정하는 자바스크립트 코드를 넣을 수 있으며, 해당 프로퍼티의 이름은 접두사 'on' 으로 시작한다.
```
timestamp.onclick = function() { this.innerHTML = new Date().toString(); }
```

웹 애플리케이션은 웹 문서의 자바스크립트 DHTML 기능을 모두 사용할 뿐만 아니라, 이를 넘어서서 웹 브라우저 환경이 제공하는 내용과 표현, 행위를 조작하는 API 등등의 핵심 부가 서비스를 제공한다.
웹 애플리케이션을 확실히 이해하려면, 웹브라우저가 문서를 보여주는 도구를 넘어서서, 간단한 운영체제로 탈바꿈 했음을 깨우쳐야한다. 브라우저가 제공하는 (네트워킹, 그래픽, 데이터 스토리지 같은) 확장된 서비스를 이용하기 위해 자바스크립트를 사용한 웹 페이지를 웹 애플리케이션이라고 정의할 수 있다. 확장 서비스중 가장 유명한 것은 XMlHttpRequest 객체다.

HTML안에 자바스크립트 포함하는 4가지
```
<script> 태그와 </script> 태그 안에 직접 작성.
<script> 태그의 src 속성에 별도 파일로 정의.
onclick이나 onmouseover 같은 HTML 이벤트 핸들러 속성 안에 작성.
javascript:라는 특별한 프로토콜을 사용해서 URL안에 작성.
```

//스크립트를 비동기로불러오고 실행하기.
```
function loadasync(url) {
  var head = document.getElementByTagName("head")[0];   -> 문서에서 <head>를 찾는다.
  var s = document.createElement("script");             -> <script> 태그 생성.
  s.src = url;                                          -> src 속성을 지정.
  head.appendChild(s);                                  -> head에 <script>를 추가.
}
```
//이벤트가 주도하는 자바스크립트
이벤트의 일반적인 형태들은 'click', 'change', 'load', 'mouseover', 'keypress', 'readystatechange' 같은 이름이 붙는다.

이벤트 핸들러 등록을 위해 HTML 속성을 사용할 수 있으나 이러면 HTML 내용과 자바스크립트 코드가 뒤섞여서 좋지 않다. 대신, 대른 간단한 방법은 대상 객체의 속성에 자바스크립트 함수를 지정하는 것이다.
```
window.onload = function() { ... };
document.getElementById("button1").onClick = function() { ... };
function headleResponse() { ... }
request.onreadystatechange = headleResponse;
```
이벤트 핸들러 속성에는 'on' 뒤에 이벤트 종류를 나타내는 이름을 쓴다. 코드는 이벤트 핸들러 속성에 함수를 지정만 했지 호출은 하지 않는다. 이벤트가 일어나면 브라우저가 함수를 호출할 것이다. 이벤트를 통한 비동기 프로그래밍에서는 종종 중첩 함수를 사용하게 된다. 그러니 함수 안에 있는 함수 안에 또 다시 함수를 정의하는 일도 드물지 않다.
대부분의 브라우저는 거의 모든 이벤트에 대해 이벤트 핸들러에 이벤트 객체를 전달인자로 넘기고, 이벤트 객체의 프로퍼티로 이벤트에 대한 상세 정보를 제공한다.
예를들어 클릭 이벤트가 넘긴 객체는 마우스 버튼의 클릭 위치를 명시한 속성을 가지고 있다. 이벤트 핸들러의 반환값은 함수 가 이벤트를 적절히 처리했는지를 확인하거나 다른 기본 동작을 막기 위해 사용한다. 
이벤트 타깃이 문서 요소일 때는 이벤트가 문서 계층 구조를 따라 상위로 올라가는데 이를, **버블링** 과정이라고도 한다.

하나의 이벤트에 여러 개의 이벤트 핸들러 함수를 등록하거나, 동일 객체의 동일 이벤트에 서로 다른 모듈이 이벤트 핸들러를 여러 개 등록하는 데 문제가 없으려면, 다른 이벤트 핸들러 등록 방법을 사용해야 한다.
```
window.addEventListener('load', function() { ... }, false);
request.addEventListener("readystatechange", function(), false);
```

-> 문서를 모두 불러왔을 때 실행할 함수 f를 등록하고 문서를 이미 불러왔다면 비동기로 최대한 빨리 실행.
```
functino onLoad(f) {
  if (onLoad.loaded)
    window.setTimeout(f, 0);
  else if (window.addEventListener)
    window.addEventListener("load", f, false);
  else if (window.attachEvent)
    window.attachEvent("onload", f);
}
//문서를 모두 불러오진 못했음을 나타내는 플래그를 지정해서 시작한다.
onLoad.loaded = false
// 그리고 문서를 불러올 때 플래그를 지정할 함수를 등록한다.
onLoad(function() { onLoad.loaded = ture; });
```

여담, 상호 호환성도 중요하고 현제로서 여러 웹 사이트들이 있지만.. 막연하게 브라우저 통합이라는 단어는 생각나지만 안될것같다 라는 생각밖에 들지않는다.
또한 여러사람들을 위한 접근성 개선들도 계속해서 일어났으면 좋겠다. 하고싶다.

//자바스크립트가 할 수 없는 것
자바스크립트는 클라이언트 컴퓨터에서 임의로 디렉터리와 파일을 만들거나 지울 수 없다. 이런 기능 제약으로 인해 자바스크립트 프로그램은 클라이언트 컴퓨터의 데이터를 삭제하거나 바이러스 침투시킬 수 없다.
같은이유로 클라이언트 측 자바스크립트는 범용 네트워킹 기능이 전혀없다. 물론 클라이언트 스크립트로 HTTP 프로토콜을 구현할 수 있다. 뿐만 아니라 웹 소켓으로 알려진 HTML5 관련 표준안은 소켓과 유사한 방식으로 전용 서버와 통신할 수 있는 API를 제공한다.
-> 자바스크립트는 브라우저에서 새 창을 열수 있찌만 남용을 막기위해 대부분 브라우저는 새 창 열기를 막아두고 마우스 클립같은 행동으로 사용자가 직접 새 창을 열게한다.
-> 자바스크립트는 스스로 연 새창은 다시 닫을 수 있지만, 다른 창을 다응려면 사용자가 확인 창에서 승인을 해주어야 한다.
-> HTML FileUpload요소 의 value 속성은 값을 불러올 수는 있지만 저장할 수는 없다. (이것이 가능하면 자바스크립트로 원하는 파일을 value 속성에 지정할 수 있으므로, 서버로 특정한 파일의 내용(계정 암호 등등..)을 업로드 하는 폼을 만들 수 있기 때문이다.
-> 스크립트는 자신이 실행된 문서의 서버와 다른 서버에서 불러온 문서의 내용은 읽을 수 없다. 비슷한 의미로 스크립트는 다른 서버에서 불러온 문서에 이벤트 리스너를 등록할 수 없다.

//동일 출처 정책
동일 출처 정책은 자바스크립트 코드가 웹 콘텐츠와 상호 작용을 할 수 있다는 사실전반에 걸친 폭 넓은 제약 사항이다. 이 정책은 일반적으로 <iframe> 요소를 포함한 웹 페이지나 새로운 브라우저 창이 열릴 때 작동하는데, 한 창이나 프레임의 코드는 동일 출처 정책의 감독하에 다른 창이나 프레임과 상호작용 한다. 즉 같은 문서의 Window, Document객체의 속성만을 사용할 수 있다.
//동일 출처 정책 해제
1. 두 개의 창이나 프레임에서 domain속성이 같은 값으로 지정된 스크립트를 가지고 있다면, 두 창에서는 동일 출처 정책이 해제되고, 상호작용도 가능해진다.
ex) orders.example.com과 catalog.example.com 에서 불러온 문서끼리 협업하는 스크립트 두 문서의 document.domain 속성을 "example.com"로 지정하면, 출처가 같아지므로 서로의 속성을 불러 올 수 있다.
2. 현재 만들어지고 있는 출처 간 자원 공유(CORS)란 이름의 표준안이다. 이 표준 초안은 요청 헤더와 Access-Control-Allow-Origin이라는 응답 헤더로 HTTP의 출처를 확장한다. 그래서 파일 요청이 들어올 명시적인 출처 목록의 헤더나 와일드카드를 서버에 사용할 수 있고, 그러면 어느 사이트에서든 요청할 수 있는 파일 제공이 가능하다.
3. 교차 출처 문서 간 메시지 교환인데, 스크립트의 출처가 달라도 이 문서에서 저 문서로 텍스트 메시지를 전달할 수 있다.
  
//문서 스크립팅
클라이언트 측 자바스크립트는 정적인 HTML 문서를 상호작용이 가능한 웹 애플리케이션으로 바꾸는 역할을 하며, 이렇게 웹 페이지의 내용을 제어하는 게 자바스크립트의 주된 작업이다. 다음은 문서 내용 제어에 대한 간단한 설명이다.
모든 Window 객체에는 Document 객체를 참조하는 document 프로퍼티가 존재하고, Document 객체는 해당 창의 내용을 나타낸다.
Document는 DOM으로 알려진 문서 객체 모델의 거대한 API의 일부이며 그중 문서 내용을 나타내고 조작하는 역할을 하는 중요한 객체다.
HTML이나 XML 문서의 요소 집합이 DOM에서는 객체의 계층구조로 표현된다는 사실을 알아야 한다. 이렇게 HTML 문서를 계층적으로 표현한 DOM은 <body>와 <p> 같은 HTML 태그나 요소를 나타내는 노드, 텍스트의 문자열을 나타내는 노드를 포함하고 있다.
```
<html>
  <head>
    <title>Sample Document</title>
  </head>
  <body>
    <h1>HTML Document</h1>
    <p> yap <i> yapp </i> yappp </p>
  </body>
</html>
```
다음은 위 문서를 계층 구조로 표현한 DOM 형태다.
```
Document -> <html> -> <head> -> <title> -> "Sample Document"
                   -> <body> -> <h1> -> "HTML Document"
                             -> <p> -> "yap"
                                    -> <i> -> "yapp"
                                    -> "yappp"
```
문서 전체를 나타내는 Document 노드가 있고, 아래쪽으로는 HTML 요소를 나타내는 element 노드와 텍스트를 나타내는 Text 노드가 있다.
다음은 Node들의 하위 타입과 계층구조를 보여주며, Document 타입은 HTML 또는 XML 문서를 나타내며, Element 클래스는 그 문서의 요소를 나타낸다. 반면 제네릭 타입의 서브클래스인 HTMLDocument와 HTMLElement는 좀 더 구체적으로 HTML의 문서와 요소만을 말한다.
```
Node -> Document -> HTMLDocument
     -> CharacterData -> Text
                      -> Comment
     -> Attr
     -> Element -> HTML Element -> HTMLHeadElement
                                -> HTMLBodyElement
                                -> HTMLTitleElement
                                -> HTMLParagraphElement
                                -> HTMLInputElement
                                -> HTMLTableElement
                                -> 등등...
```
HTMLElement의 서브타입은 HTML 단일 요소나 요소 집합의 속성에 해당하는 자바스크립트 프로퍼티를 정의한다. 이 중 몇 가지 특정 요소는 단순히 HTML 구문을 나타내는 데 그치지 않고 추가 프로퍼티와 메서드를 정의한다.
Comment 노드는 HTML이나 XML의 주석을 나타낸다. 주석은 기본적으로 텍스트 문자열이므로, 문서의 text 출력을 나타내는 Text 노드와 많이 비슷하다.
Attr 노드는 XML이나 HTML의 속성을 나타내지만 거의 사용하지 않는다. 속성은 문서의 노드보다는 이름과 값의 쌍으로 된 형태가 다루기 편한데, 이런 속성을 제어하는 메서드들이 Element클래스에 정의되어 있기 때문이다.
  
//문서 요소 선택
Document 객체를 참조할 땐 전역변수 document를 사용하면 되지만, 문서의 각 요소를 수정하려면 해당 요소를 가리키는 Element 객체를 선택할 방법이 있어야한다.
1. id 속성을 지정해서 선택하기
2. name 속성을 지정해서 선택하기
3. HTML 태그 이름을 지정해서 선택하기
4. CSS class 이름(들)을 지정해서 선택하기
5. 지정한 CSS 선택자와 일치 여부로 선택하기

// ID로 요소 선택하기
```
function getElement(/*ids...*/) {
  var element = {};
  for (var i = 0; i < argument.length; i++) {
    var id = argument[i];
    var elt = document.getElementById(id);
    if (elt == null) {
      throw new Error("No element with id : " + id);
    element[id] = elt;
  }
  return elements;
}
```

//Name으로 요소 선택하기
Document 객체의 getElementByName() 메서드를 사용하면 name속성의 값을 이용해서 HTML요소를 선택할 수 있다.
```
var radiobuttons = document.getElementByName("favorite_color");
```
다른 여러 요소들의 name들에도 속성설정이 가능하기때문에 이름에 따라 가져와 사용할 수 있다.(form, img, iframe, applet, embed, object)
-><from name="shippinh_address"> 요소를 의미하는 Element 객체를 가져온다.
```
var form = document.shipping_address;
```

//Type으로 요소 선택하기
문서 안에 있는 모든 <span>요소에 해당하는 Element 객체를 얻기 위해 다음처럼 작성할 수 있는데, 이 Element 객체를 담고 있는 객체 읽기 전용 배열과 유사한 객체다.
```
var spans = document.getElementByTagName("span");
```
getElementsByName()과 getElementByTagName() 같은 메서드는 NodeList객체를 반환한다. 반환된 NodeList의 요소는 문서 내 순서를 따른다. 그래서 문서상 첫번째 <p>요소를 선택하려면 다음처럼 하면 된다.
```
var firstpara = document.getElementByTagName("p")[0];
```
문서의 첫 번째 <p> 요소 안에 있는 모든 <span> 요소를 찾으려면 다음처럼 작성하면 된다.
```
var firstpara = document.getElementByTagName("p")[0];
var firstParaSpans = firstpara.getElementByTagName("span");
```

getElementByName()과 getElementByTagName()은 NodeList 객체를 반환하며, document.images와 document.forms는 HTMLCollectio객체를 반환한다.
또한 진짜 배열처럼 접근할수 있고 다음처럼 자바스크립트의 일반적인 반복문으로 NodeList나 HTMLCollection의 내용을 순회할 수 있다.
```
for (var i = 0; i < document.images.length; i++) {      -> 모든 이미지에 대한 반복문
  document.images[i].style.display = "none";            -> 이미지를 숨긴다.
}
```
NodeList와 HTMLCollection에서 배열의 메서드를 직접 호출할 수는 없고 간접적으로는 가능하다.
```
var content = Array.prototype.map.call(document.getElementByTagName("p"),
  function(e) { return e.innerHTML; });
```
HTMLCollection객체는 추가로 name 속성이 있는 프로퍼티를 가질 수 있고, 숫자뿐만 아니라 문자열로도 접근할 수 있다.

//CSS Class로 요소 선택하기
동일한 식별자를 class 속성에 포함하고 있는 모든 요소는 하나의 집합으로 취급되는데, 이번 항목에서는 이를 이용해서 관련된 문서 요소 집합을 정의하는 방법이다.
HTML5에서 정의된 getElementByClassName()은 class 속성의 식별자를 기반으로 문서 요소의 집합을 선택할 수 있다.
getElementByClassName()의 전달인자는 문자열 하나지만, 이 문자열에 공백으로 구분된 여러 개의 식별자를 쓸 수도 있다. 따라서 getElementByClassName() 메서드를 사용하면, 문서 요소의 class속성에 메서드 인자로 지정한 식별자들 전부가 있는 요소를 찾는다. (class속성과 getElementByClassName() 메서드에서는 class 식별자의 구분자를 쉼표가아닌 공백을 이용해 구분한다)
```
-> class 속성에 waring이 있는 모든 요소를 찾는다.
var warnings = document.getElementByClassName("warning");

-> 이름이 "log"인 요소의 자손 중에서, class에 "error"와 "fatal"이 있는 요소를 모두 찾는다.
var log = document.getElementById("log");
var fatal = log.getElementByClassName("fatal error");
```

***
현재의 웹브라우저는 문서 시작 부분의 <!DOCTYPE> 선언 방법에 따라서 쿼크모드 나 표준모드 로 HTML문서를 출력한다.
쿼크모드는 하위 호환성을 위해 존재하며, class속성과 CSS스타일시트의 class식별자에서 대소문자를 구분하지 않는 약간 특이한 특징이 있다. 그래서 getElementByClassName()은 스타일시트에 사용된 매칭 알고리즘을 따르는데, 문서가 쿼크모드로 렌더링 되면 이 메서드는 대소문자 구분없이 class식별자로 문자열 비교를 수행한다. 쿼크모드가 아니면 대소문자를 구분하여 비교한다.

//CSS선택자로 요소 선택하기
문서의 요소는 다음처럼 ID와 HTML 태그명, class 속성명으로 선택할 수 있다.
```
#nav                                                  -> id 속성이 nav인 요소
div                                                   -> <div> 요소
.warning                                              -> class 속성의 값이 warning인 요소

p[lang="fr"]                                          -> 프랑스어 단락인 <p>태그. 즉 <p lang="fr">
*[name="x"]                                           -> name="x"송성을 가진 어떤 요소

span.fatal.error                                      -> class속성 값에 fatal 과 error 가 있는 span
span[lang="fr"].warning                               -> class송성 값이 warning인 프랑스어 spna 태그

#log span                                             -> id가 log인 요소의 자손들 중에서 span
#log>span                                             -> id가 log인 요소의 한 레벨 아래의 span
body>h1:first-child                                   -> body 한 레벨 아래의 h1 중에서 첫 번째 요소

div, #log                                             -> id가 log 인 요소와 모든 div요소
```
