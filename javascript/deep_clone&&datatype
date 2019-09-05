深拷贝，适合所有类型
```javascript
function clone(target, map = new WeakMap()) {
    if (typeof target === 'object') {
        const isArray = Array.isArray(target);
        let cloneTarget = isArray ? [] : {};

        if (map.get(target)) {
            return map.get(target);
        }
        map.set(target, cloneTarget);

        const keys = isArray ? undefined : Object.keys(target);
        forEach(keys || target, (value, key) => {
            if (keys) {
                key = value;
            }
            cloneTarget[key] = clone2(target[key], map);
        });

        return cloneTarget;
    } else {
        return target;
    }
}
```

说明：
1.如果是原始类型，无需继续拷贝，直接返回
  如果是引用类型，创建一个新的对象，遍历需要克隆的对象，将需要克隆对象的属性执行深拷贝后依次添加到新对象上。
2.考虑到兼容数组.
3.解决循环引用问题，我们可以额外开辟一个存储空间，来存储当前对象和拷贝对象的对应关系，当需要拷贝当前对象时，先去存储空间中找，有没有拷贝过这个对象，如果有的话直接返回，如果没有的话继续拷贝，这样就巧妙化解的循环引用的问题。
  这个存储空间，需要可以存储key-value形式的数据，且key可以是一个引用类型，我们可以选择Map这种数据结构：

  检查map中有无克隆过的对象
  有 - 直接返回
  没有 - 将当前对象作为key，克隆对象作为value进行存储
  继续克隆
4.WeakMap 对象是一组键/值对的集合，其中的键是弱引用的。其键必须是对象，而值可以是任意的。
5.如果是WeakMap的话，target和obj存在的就是弱引用关系，当下一次垃圾回收机制执行时，这块内存就会被释放掉。
  设想一下，如果我们要拷贝的对象非常庞大时，使用Map会对内存造成非常大的额外消耗，而且我们需要手动清除Map的属性才能释放这块内存，而WeakMap会帮我们巧妙化解这个问题。
  我也经常在某些代码中看到有人使用WeakMap来解决循环引用问题，但是解释都是模棱两可的，当你不太了解WeakMap的真正作用时。我建议你也不要在面试中写这样的代码，结果只能是给自己挖坑，即使是准备面试，你写的每一行代码也都是需要经过深思熟虑并且非常明白的。
  能考虑到循环引用的问题，你已经向面试官展示了你考虑问题的全面性，如果还能用WeakMap解决问题，并很明确的向面试官解释这样做的目的，那么你的代码在面试官眼里应该算是合格了


6.当遍历数组时，直接使用forEach进行遍历，当遍历对象时，使用Object.keys取出所有的key进行遍历，然后在遍历时把forEach会调函数的value当作key使用：

###合理的判断引用类型
1.首先，判断是否为引用类型，我们还需要考虑function和null两种特殊的数据类型
```javascript
function isObject(target) {
    const type = typeof target;
    return target !== null && (type === 'object' || type === 'function');
}
```
###获取数据类型（toString()）

每一个引用类型都有toString方法，默认情况下，toString()方法被每个Object对象继承。如果此方法在自定义对象中未被覆盖，toString() 返回 "[object type]"，其中type是对象的类型。

注意，上面提到了如果此方法在自定义对象中未被覆盖，toString才会达到预想的效果，事实上，大部分引用类型比如Array、Date、RegExp等都重写了toString方法。
我们可以直接调用Object原型上未被覆盖的toString()方法，使用call来改变this指向来达到我们想要的效果。

```javascript
function getType(target) {
    return Object.prototype.toString.call(target);
}
```

###可继续遍历的类型
上面我们已经考虑的object、array都属于可以继续遍历的类型，因为它们内存都还可以存储其他数据类型的数据，另外还有Map，Set等都是可以继续遍历的类型，这里我们只考虑这四种，如果你有兴趣可以继续探索其他类型。
有序这几种类型还需要继续进行递归，我们首先需要获取它们的初始化数据，例如上面的[]和{}，我们可以通过拿到constructor的方式来通用的获取。
例如：const target = {}就是const target = new Object()的语法糖。另外这种方法还有一个好处：因为我们还使用了原对象的构造方法，所以它可以保留对象原型上的数据，如果直接使用普通的{}，那么原型必然是丢失了的。

```javascript
function getInit(target) {
    const Ctor = target.constructor;
    return new Ctor();
}
```
下面，我们改写clone函数，对可继续遍历的数据类型进行处理:
```javascript
function clone(target, map = new WeakMap()) {

    // 克隆原始类型
    if (!isObject(target)) {
        return target;
    }

    // 初始化
    const type = getType(target);
    let cloneTarget;
    if (deepTag.includes(type)) {
        cloneTarget = getInit(target, type);
    }

    // 防止循环引用
    if (map.get(target)) {
        return map.get(target);
    }
    map.set(target, cloneTarget);

    // 克隆set
    if (type === setTag) {
        target.forEach(value => {
            cloneTarget.add(clone(value,map));
        });
        return cloneTarget;
    }

    // 克隆map
    if (type === mapTag) {
        target.forEach((value, key) => {
            cloneTarget.set(key, clone(value,map));
        });
        return cloneTarget;
    }

    // 克隆对象和数组
    const keys = type === arrayTag ? undefined : Object.keys(target);
    forEach(keys || target, (value, key) => {
        if (keys) {
            key = value;
        }
        cloneTarget[key] = clone(target[key], map);
    });

    return cloneTarget;
}

```

###不可继续遍历类型
其他剩余的类型我们把它们统一归类成不可处理的数据类型，我们依次进行处理：

Bool、Number、String、String、Date、Error这几种类型我们都可以直接用构造函数和原始数据创建一个新对象
```javascript
function cloneOtherType(targe, type) {
    const Ctor = targe.constructor;
    switch (type) {
        case boolTag:
        case numberTag:
        case stringTag:
        case errorTag:
        case dateTag:
            return new Ctor(targe);
        case regexpTag:
            return cloneReg(targe);
        case symbolTag:
            return cloneSymbol(targe);
        default:
            return null;
    }
}
```

####克隆Symbol类型：
```javascript
function cloneSymbol(targe) {
    return Object(Symbol.prototype.valueOf.call(targe));
}

//克隆正则：

function cloneReg(targe) {
    const reFlags = /\w*$/;
    const result = new targe.constructor(targe.source, reFlags.exec(targe));
    result.lastIndex = targe.lastIndex;
    return result;
}
```

####克隆函数

```javascript
function cloneFunction(func) {
    const bodyReg = /(?<={)(.|\n)+(?=})/m;
    const paramReg = /(?<=\().+(?=\)\s+{)/;
    const funcString = func.toString();
    if (func.prototype) {
        console.log('普通函数');
        const param = paramReg.exec(funcString);
        const body = bodyReg.exec(funcString);
        if (body) {
            console.log('匹配到函数体：', body[0]);
            if (param) {
                const paramArr = param[0].split(',');
                console.log('匹配到参数：', paramArr);
                return new Function(...paramArr, body[0]);
            } else {
                return new Function(body[0]);
            }
        } else {
            return null;
        }
    } else {
        return eval(funcString);
    }
}

```
