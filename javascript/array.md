### 数组的方法 ###

1. arr.push():向数组末尾追加项，返回数组的新长度

	push原理：
	```javascript
	let  ary = [1, 3, 5, 7];
	
	Array.prototype.myPush = function () {
	  // this 指向数组的实例
	  for (let i = 0; i < arguments.length; i++) {
	    this[this.length] = arguments[i];
	  }
	  return this.length;
	};
	
	let r2 = ary.myPush(11);
	console.log(ary); //[1,3,5,7,11]
	console.log(r2); //5
	```
2. arr.pop():删除并返回数组的最后一个元素

	pop原理：
	```javascript
	Array.prototype.myPop = function () {
	  // this就是数组实例
	  let last = this[this.length - 1];
	  this.length--;
	  return last;
	};
	
	let r3 = ary.myPop();
	console.log(r3);
	```
3. arr.shift():从前面删除元素，只能删除一个，返回值是删除的元素

	```javscript
	let arr = [1,2,3,4,5]
	console.log(arr.shift())  // 1
	console.log(arr)   // [2,3,4,5]
	```
4. arr.unshift():从前面添加元素，返回值是添加完后的数组

	```javascript
	let arr = [1,2,3,4,5]
	console.log(arr.unshift(2))    // 6
	console.log(arr)  //[2,1,2,3,4,5]
	```
5. arr.splice(i,n):删除从i开始之后的那个元素，返回值是删除的元素

	```javascript
	let arr = [1,2,3,4,5]
	console.log(arr.splice(2,2))     //[3,4]
	console.log(arr)    // [1,2,5]
	```
6. arr.concat():连接两个数组，返回值为连接后的新数组
	
	```javascript
	let arr = [1,2,3,4,5]
	console.log(arr.concat([1,2]))  // [1,2,3,4,5,1,2]
	console.log(arr)   // [1,2,3,4,5]
	```
7. arr.sort():将数组进行排序，返回值是排好到的数组，默认是按照最左边的数字进行排序，不是按照数字大小进行排序
	
	```javascript
	let arr = [2,10,6,1,4,22,3]
	console.log(arr.sort())   // [1, 10, 2, 22, 3, 4, 6]
	let arr1 = arr.sort((a, b) =>a - b)  
	console.log(arr1)   // [1, 2, 3, 4, 6, 10, 22]
	let arr2 = arr.sort((a, b) =>b-a)  
	console.log(arr2)  // [22, 10, 6, 4, 3, 2, 1]
	```
8. arr.reverse():将数组反转，返回值是反转后的数组
	
	```javascript
	let arr = [1,2,3,4,5]
	console.log(arr.reverse())    // [5,4,3,2,1]
	console.log(arr)    // [5,4,3,2,1]
	```
9. arr.slice(start,end)：切去索引值start到索引值end的数组，不包含end索引的值，返回值是切出来的数组
	
	```javascript
	let arr = [1,2,3,4,5]
	console.log(arr.slice(1,3))   // [2,3]
	console.log(arr)    //  [1,2,3,4,5]
	```

10. arr.forEach(value,index,array)：遍历数组，无return
11. arr.every():根据判断条件，数组的元素是否全满足，若满足则返回true

	```javascript
	let arr = [1,2,3,4,5]
	let arr1 = arr.every( (i, v) => i < 3)
	console.log(arr1)    // false
	let arr2 = arr.every( (i, v) => i < 10)
	console.log(arr2)    // true
	```
12. arr.some():根据判断条件，数组的元素是否有一个满足，若有一个则返回true

	```javascript
	let arr = [1,2,3,4,5]
	let arr1 = arr.some( (i, v) => i < 3)
	console.log(arr1)    // true
	let arr2 = arr.some( (i, v) => i > 10)
	console.log(arr2)    // false
	```
13. arr.filter():过滤数组，返回一个满足要求的数组
	```javascript
	let arr = [1,2,3,4,5]
	let arr1 = arr.filter( (i, v) => i < 3)
	console.log(arr1)    // [1, 2]
	```

11. arr.map(value,index,array):映射数组，有return 返回一个新数组

	arr.forEach()和arr.map()区别：
	- forEach()返回值是undefined，不可以链式调用
	- map()返回一个新数组，原数组不会改变
	- 没有办法终止或者跳出forEach()循环，除非抛出异常，所以想执行一个数组是否满足什么条件，返回布尔值，可以用一般的for循环实现，或者用Array.some()或者Array.every() 


### 数组应用 ###

#### sort应用 ####

```javascript
var ary = [12, 1, 3, 5, 9, 2, 7];
var ary1 = [
  {
    name: 'zhangsan',
    age: 15
  },
  {
    name: 'lisi',
    age: 18
  },
  {
    name: 'wangwu',
    age: 14
  },
  {
    name: 'liuliu',
    age: 19
  }
];
var ary3 = [
  [12],
  [6],
  [3],
  [24]
];
```

#### sort原理 ####
让相邻两项进行比较，如果return的数字大于0，那么会让这两项交换位置，小于0则不交换
- 回调函数的执行次数和数组成员个数，还跟数组的成员大小有关

```javascript
ary.sort(function (a, b) {
  // console.log(a, b);
  // console.log(ary);
  return a - b;
});

console.log(ary);
```
- 如果数组项是对象，sort排序时交换的是数组项，而不是数组项的某一个具体的值
```javascript
ary1.sort(function (a, b) {
  return a.age - b.age;
});
console.log(ary1);
```

#### 数组的复制 ####

- 一个数组中的数组项是一个基本数据类型的值时，这个数组项存储的就是这个值本身
- 如果数组项是一个引用数据类型的值时，这个十足实际存储的是这个引用数据类型值的堆内存地址
- 所以在复制数组是，如果数组项是基本数据类型，那么复制出来的新数组中的数组项和原数组中的项没有关系
- 如果复制的项是引用数据类型，再复制这一项时，其实是复制堆内存的地址，所以在操作新数组的这一项时，原数组也会受影响，所以这种复制称为浅拷贝

```javascript
let ary = [1, 2, 3, 4, 5];
let ary2 = [
  {
    name: 1
  },
  {
    name: 2
  },
  {
    name: 3
  }
];

// 复制数组ary和ary2；

let a1 = ary.slice();
a1[1] = 100;
console.log(a1); // [1, 100, 3, 4, 5]
console.log(ary); // [1, 2, 3, 4, 5]

let a2 = ary2.slice();
a2[1].name = 100;
console.log(a2); // [{name: 1}, {name: 100}, {name: 3}]
console.log(ary2); // [{name: 1}, {name: 100}, {name: 3}]
```

### 浅拷贝和深拷贝 ###


