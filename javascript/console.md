### console的多种用法 ###

#### console.assert ####
使用场景：条件性输出。有些信息你可能只想在某些条件不满足的时候才进行输出，这个时候可以用这种方法而不是多加个判断
用法：console.assert(condition,...data)
注意只有condition是false的时候data才会被输出

#### console.table ####
使用场景：以表格的形式输出数据。这个方法最适用的场景是对象的数组，因为可以一目了然的看到数组内对象各个属性的值
用法：console.table(arr);

每个group都需要手动的调用groupEnd来退出

#### console.trace ####
使用场景：追踪函数的执行栈。当你想知道一个函数具体是怎样被调用时，可以使用console.trace这个函数去追踪它的执行栈
用法：函数内调用


#### console.count ####
使用场景：统计代码的执行次数

#### console.time ####
使用场景：统计代码执行的耗时
