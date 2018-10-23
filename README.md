# just_do_it_javascript
무작정 써보기
***
ES3: 대중적, IE8까지, 함수단위 스코프, 호이스팅, 프로토타입, 모듈화 미지원
ES5: The "use strict" Directive, String.trim(), 
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
