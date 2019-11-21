echarts4之后，可以使用dataset管理数据
一下是一个简单的例子：
```javascript
option = {
    legend: {},
    tooltip: {},
    dataset: {
        // 提供一份数据。
        source: [
            ['product', '2015', '2016', '2017'],
            ['Matcha Latte', 43.3, 85.8, 93.7],
            ['Milk Tea', 83.1, 73.4, 55.1],
            ['Cheese Cocoa', 86.4, 65.2, 82.5],
            ['Walnut Brownie', 72.4, 53.9, 39.1]
        ]
    },
    // 声明一个 X 轴，类目轴（category）。默认情况下，类目轴对应到 dataset 第一列。
    xAxis: {type: 'category'},
    // 声明一个 Y 轴，数值轴。
    yAxis: {},
    // 声明多个 bar 系列，默认情况下，每个系列会自动对应到 dataset 的每一列。
    series: [
        {type: 'bar'},
        {type: 'bar'},
        {type: 'bar'}
    ]
}
```
或者也可以使用常见的对象数组的格式：
```javascript
option = {
    legend: {},
    tooltip: {},
    dataset: {
        // 这里指定了维度名的顺序，从而可以利用默认的维度到坐标轴的映射。
        // 如果不指定 dimensions，也可以通过指定 series.encode 完成映射，参见后文。
        dimensions: ['product', '2015', '2016', '2017'],
        source: [
            {product: 'Matcha Latte', '2015': 43.3, '2016': 85.8, '2017': 93.7},
            {product: 'Milk Tea', '2015': 83.1, '2016': 73.4, '2017': 55.1},
            {product: 'Cheese Cocoa', '2015': 86.4, '2016': 65.2, '2017': 82.5},
            {product: 'Walnut Brownie', '2015': 72.4, '2016': 53.9, '2017': 39.1}
        ]
    },
    xAxis: {type: 'category'},
    yAxis: {},
    series: [
        {type: 'bar'},
        {type: 'bar'},
        {type: 'bar'}
    ]
};
```
数据到图形的映射
我们制作数据可视化图表的逻辑是这样的：基于数据，在配置项中指定如何映射到图形。
简单来说，可以进行这些映射：
1.指定dataset的列还是行映射为图形系列（series）。这件事可以使用series.seriesLayoutBy属性来配置。默认是按照列来映射。
2.指定维度映射的规则：如何从dataset的维度（一个维度的意思是一行/列）映射到坐标轴（如x、y轴）、提示框(tooltip)、标签(label)、图形元素大小颜色等(visualMap)。这件事可以使用series.encode属性，以及visualMap组件（如果需要映射颜色大小等视觉维度的话）来配置。上面的例子中，没有给出这种映射配置，那么echarts最常见的理解进行默认映射：X坐标轴声明为类目轴，默认情况下会自动对应到dataset.source中的第一列；三个柱图系列，意义对应到dataset.source中后面每一列。

按行还是按列做映射

有了数据表之后，使用者可以灵活的配置：数据如何对应到轴和图形系列。
用户可以使用seriesLayoutBy配置项，改变图表对于行列的理解。seriesLayoutBy可取值：
'column':默认值。系列被安放到dataset的列上面
'row':系列被安放到dataset的行上面

维度（dimension）
介绍encode之前，首先要介绍“维度”的概念
常用图表所描述的数据大部分是“二维表”结构，上述例子中，我们都使用二维数组来容纳二维表。现在，当我们把系列对应到“列”的时候，那么每一列就称为一个“维度”，而每一行称为数据项（item）。反之，如果我们把系列对应到表行，那么每一行就是“维度”，每一列就是数据项。

维度可以有单独的名字，便于在图表中显示。维度名可以在定义dataset的第一行（或者第一列）。例如上面的例子中，“score”、“amount”、“product”就是维度名。从第二行开始，才是正式的数据。dataset.source中的第一行（列）到底包含不包含维度名，echarts默认会自动探测。当然也可以设置dataset.sourceHeader:true显示第一行（列）就是维度，或者dataset.sourceHeader:false表示第一行（列）开始就是数据。

维度的定义，也可以使用单独的dataset.dimensions或者series.dimensions来定义，这样可以同时指定维度名，和维度的类型
```javascript
var option1 = {
    dataset: {
        dimensions: [
            {name: 'score'},
            // 可以简写为 string，表示维度名。
            'amount',
            // 可以在 type 中指定维度类型。
            {name: 'product', type: 'ordinal'}
        ],
        source: [...]
    },
    ...
};

var option2 = {
    dataset: {
        source: [...]
    },
    series: {
        type: 'line',
        // 在系列中设置的 dimensions 会更优先采纳。
        dimensions: [
            null, // 可以设置为 null 表示不想设置维度名
            'amount',
            {name: 'product', type: 'ordinal'}
        ]
    },
    ...
};
```
大多数情况下，我们并不需要去设置维度类型，因为会自动判断。但是如果因为数据为空之类原因导致判断不足够准确时，可以手动设置维度类型。
维度类型可以取这些值：
1.'number'：默认，表示普通数据
2.'ordinal'：对于类目、文本这些string类型的数据，如果需要能在数轴上使用，须是'ordinal'类型。echarts默认会自动判断这个类型。但是自动判断也不是很完备的，所以使用者可以手动强制指定。
3.'time'：表示时间数据。设置成'time'则能支持自动解析数据成时间戳，比如该维度的数据是'2018-08-21'会自动被解析。如果这个维度被用在时间数轴上，那么会被自动设置为'time'类型
4.'float'：如果设置成'float'，在存储时候会使用typedArray，对性能优化有好处。
5.'int'：如果设置成'int'，在存储时候会使用TypedArray，对性能优化有好处

了解了维度的概念后，我们就可以使用encode来做映射，如下：
```javascript
var option = {
    dataset: {
        source: [
            ['score', 'amount', 'product'],
            [89.3, 58212, 'Matcha Latte'],
            [57.1, 78254, 'Milk Tea'],
            [74.4, 41032, 'Cheese Cocoa'],
            [50.1, 12755, 'Cheese Brownie'],
            [89.7, 20145, 'Matcha Cocoa'],
            [68.1, 79146, 'Tea'],
            [19.6, 91852, 'Orange Juice'],
            [10.6, 101852, 'Lemon Juice'],
            [32.7, 20112, 'Walnut Brownie']
        ]
    },
    xAxis: {},
    yAxis: {type: 'category'},
    series: [
        {
            type: 'bar',
            encode: {
                // 将 "amount" 列映射到 X 轴。
                x: 'amount',
                // 将 "product" 列映射到 Y 轴。
                y: 'product'
            }
        }
    ]
};
```
encode声明的基本结构如下，其中冒号左边是坐标系、标签等特定名称，如'x','y',等，冒号右边是数据中的维度名或者维度的序号，可以指定一个或多个维度。通常情况下，下面各种信息不需要所有的都写，按需要写即可。
下面是encode支持的属性：
```javascript
// 在任何坐标系和系列中，都支持：
encode: {
    // 使用 “名为 product 的维度” 和 “名为 score 的维度” 的值在 tooltip 中显示
    tooltip: ['product', 'score']
    // 使用 “维度 1” 和 “维度 3” 的维度名连起来作为系列名。（有时候名字比较长，这可以避免在 series.name 重复输入这些名字）
    seriesName: [1, 3],
    // 表示使用 “维度2” 中的值作为 id。这在使用 setOption 动态更新数据时有用处，可以使新老数据用 id 对应起来，从而能够产生合适的数据更新动画。
    itemId: 2,
    // 指定数据项的名称使用 “维度3” 在饼图等图表中有用，可以使这个名字显示在图例（legend）中。
    itemName: 3
}

// 直角坐标系（grid/cartesian）特有的属性：
encode: {
    // 把 “维度1”、“维度5”、“名为 score 的维度” 映射到 X 轴：
    x: [1, 5, 'score'],
    // 把“维度0”映射到 Y 轴。
    y: 0
}

// 单轴（singleAxis）特有的属性：
encode: {
    single: 3
}

// 极坐标系（polar）特有的属性：
encode: {
    radius: 3,
    angle: 2
}

// 地理坐标系（geo）特有的属性：
encode: {
    lng: 3,
    lat: 2
}

// 对于一些没有坐标系的图表，例如饼图、漏斗图等，可以是：
encode: {
    value: 3
}
```

默认的映射
值得一提的是，echarts针对最常见直角坐标系中的图表，给出了简单的默认的映射，从而不需要配置encode也可以出现图表。默认的映射规则不易做的复杂，基本规则大体是：
在坐标系中：如果有类目轴(axis.type为'category')，则将第一列（行）映射到这个轴上，后续每一列（行）对应一个系列；如果没有类目轴，假如坐标系有两个轴，则每两列对应一个系列，这两列分别映射到这两个轴上。
如果没有坐标系（如饼图），取第一列（行）为名字，第二列（行）为数值。







