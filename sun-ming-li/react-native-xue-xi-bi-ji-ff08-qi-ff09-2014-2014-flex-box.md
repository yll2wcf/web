#### 一、容器的属性

1. ##### flex-direction决定主轴的方向
2. row  
   （默认值）：主轴为水平方向，起点在左端。

3. row-reverse
   ：主轴为水平方向，起点在右端。
4. column
   ：主轴为垂直方向，起点在上沿。
5. column-reverse
   ：主轴为垂直方向，起点在下沿。

##### 2. flex-wrap 如果一条轴线排不下，如何换行

* nowrap\(默认\)：不换行。
* wrap: 换行，第一行在上方。
* wrap-reverse：换行，第一行在下方。

##### 3. flex-flow属性是flex-direction属性和flex-wrap属性的简写形式，默认值为row nowrap。

##### 4. justify-content属性定义了项目在主轴上的对齐方式。

```
 具体对齐方式与轴的方向有关。下面假设主轴为从左到右。
```

* flex-start（默认值）：左对齐
* flex-end：右对齐
* center：居中
* space-between：两端对齐，项目之间的间隔都相等
* space-around：每个项目两侧的间隔相等。所以，项目之间的间隔比项目与边框的间隔大一倍。

##### 5. align-items属性定义在交叉轴上如何对齐。

```
   假设交叉轴从上到下
```

* flex-start: 交叉轴的起点对齐
* flex-end: 交叉轴的终点对齐
* center：交叉轴的中点对齐
* baseline：项目的第一行文字的基线对齐
* stretch（默认值）：如果项目未设置高度或者设为auto，将占满整个容器的高度。

##### 6. align-content属性定义了多根轴线的对齐方式。如果项目只有一根轴线，该属性不起作用。

* flex-start: 与交叉轴的起点对齐
* flex-end: 与交叉轴的终点对齐
* center：与交叉轴的中点对齐
* space-between: 与交叉轴两端对齐，轴线之间的间隔平均分布。
* spcace-arround: 每根轴线两侧的间隔都相等。所以，轴线之间的间隔比轴线与边框的间隔大一倍。
* stretch（默认值）：如果项目未设置高度或者设为auto，将占满整个容器的高度。

#### 二、项目的属性

##### 1. order属性定义项目的排列顺序。数值越小，排列越靠前，默认为0。

##### 2. flex-grow 属性定义项目的放大比例，默认为0，即如果存在剩余空间，也不放大。

如果所有项目的flex-grow属性都为1，则它们将等分剩余空间（如果有的话）。如果一个项目的flex-grow属性为2，其他项目都为1，则前者占据的剩余空间将比其他项多一倍。

##### 3. flex-shrink 属性定义了项目的缩小比例，默认为1，即如果空间不足，该项目将缩小。

如果所有项目的flex-shrink属性都为1，当空间不足时，都将等比例缩小。如果一个项目的

flex-shrink属性为0，其他项目都为1，则空间不足时，前者不缩小。负值对该属性无效。

##### 4. flex-basis

属性定义了在分配多余空间之前，项目占据的主轴空间（main size）。浏览器根据这个属性，计算主轴是否有多余空间。它的默认值为auto，即项目的本来大小。它可以设为跟width或height属性一样的值（比如350px），则项目将占据固定空间。

##### 5. flex属性是flex-grow, flex-shrink和 flex-basis的简写，

默认值为0 1 auto。后两个属性可选。该属性有两个快捷值：auto\(1 1 auto\) 和 none \(0 0 auto\)。

##### 建议优先使用这个属性，而不是单独写三个分离的属性，因为浏览器会推算相关值。

##### 6.algin-self 属性允许单个项目有与其他项目不一样的对齐方式，可覆盖align-items属性。默认值为auto，表示继承父元素的align-items属性，如果没有父元素，则等同于stretch。

* flex-start: 交叉轴的起点对齐
* flex-end: 交叉轴的终点对齐
* center：交叉轴的中点对齐
* baseline：项目的第一行文字的基线对齐
* stretch：如果项目未设置高度，将占满整个容器的高度。
* auto：继承父元素的align-items属性

三、flex实践，实现了类似骰子的效果。

效果图：

![](/assets/1534839695980.jpg)

```
import React,{Component} from "react"
import {StyleSheet,View,Text} from "react-native"

export default class Dice_Flex extends Component<>{
    render(){
        return(
            <View style={styles.container}>
                <View style={styles.item}>
                    <Text style={styles.itemText}>1</Text>
                </View>
                <View style={[styles.item,{alignItems:'center'}]}>
                    <Text style={styles.itemText}>2</Text>
                </View>
                <View style={[styles.item,{alignItems:'flex-end'}]}>
                    <Text style={styles.itemText}>3</Text>
                </View>
                <View style={[styles.item,{justifyContent:'center'}]}>
                    <Text style={styles.itemText}>4</Text>
                </View>
                <View style={[styles.item,{justifyContent:'center',alignItems:'center'}]}>
                    <Text style={styles.itemText}>5</Text>
                </View>
                <View style={[styles.item,{justifyContent:'flex-end',alignItems:'center'}]}>
                    <Text style={styles.itemText}>6</Text>
                </View>
                <View style={[styles.item,{justifyContent:'flex-end',alignItems:'flex-end'}]}>
                    <Text style={styles.itemText}>7</Text>
                </View>
                <View style={[styles.item,{flexDirection:'row',justifyContent:'space-between'}]}>
                    <Text style={styles.itemText}>8</Text>
                    <Text style={styles.itemText}>8</Text>
                </View>
                <View style={[styles.item,{flexDirection:'column',justifyContent:'space-between'}]}>
                    <Text style={styles.itemText}>9</Text>
                    <Text style={styles.itemText}>9</Text>
                </View>
                <View style={[styles.item,{flexDirection:'column',justifyContent:'space-between',alignItems:'center'}]}>
                    <Text style={styles.itemText}>10</Text>
                    <Text style={styles.itemText}>10</Text>
                </View>
                <View style={[styles.item,{flexDirection:'column',justifyContent:'space-between',alignItems:'flex-end'}]}>
                    <Text style={styles.itemText}>11</Text>
                    <Text style={styles.itemText}>11</Text>
                </View>
                <View style={[styles.item,{flexDirection:'row'}]}>
                    <Text style={[styles.itemText,{flex:1}]}>12</Text>
                    <Text style={[styles.itemText, {alignSelf:'center',flex:1}]}>12</Text>
                    <View style={{flex:1}}/>
                </View>
                <View style={[styles.item,{flexDirection:'row',justifyContent:'space-between'}]}>
                    <Text style={styles.itemText}>13</Text>
                    <Text style={[styles.itemText, {alignSelf:'flex-end'}]}>13</Text>
                </View>
                <View style={[styles.item,{justifyContent:'space-between'}]}>
                    <Text style={styles.itemText}>14</Text>
                    <Text style={[styles.itemText, {alignSelf:'center'}]}>14</Text>
                    <Text style={[styles.itemText, {alignSelf:'flex-end'}]}>14</Text>
                </View>
                <View style={[styles.item,{flexWrap:'wrap',flexDirection:'row',alignContent:'space-between',justifyContent:'flex-end'}]}>
                    <Text style={[styles.itemText,{width:26,textAlign:'center'}]}>15</Text>
                    <Text style={[styles.itemText,{width:26,textAlign:'center'}]}>15</Text>
                    <Text style={[styles.itemText,{width:26,textAlign:'center'}]}>15</Text>
                    <Text style={[styles.itemText,{width:26,textAlign:'center'}]}>15</Text>
                </View>
                <View style={[styles.item,{flexWrap:'wrap',flexDirection:'row',alignContent:'space-between'}]}>
                    <View style={styles.itemRow}>
                        <Text style={styles.itemText}>16</Text>
                        <Text style={styles.itemText}>16</Text>
                    </View>
                    <View style={styles.itemRow}>
                        <Text style={styles.itemText}>16</Text>
                        <Text style={styles.itemText}>16</Text>
                    </View>
                </View>

                <View style={[styles.item,{flexWrap:'wrap',flexDirection:'row',alignContent:'space-between'}]}>
                    <View style={styles.itemRow}>
                        <Text style={styles.itemText}>17</Text>
                        <Text style={styles.itemText}>17</Text>
                        <Text style={styles.itemText}>17</Text>
                    </View>
                    <View style={styles.itemRow}>
                        <Text style={styles.itemText}>17</Text>
                        <Text style={styles.itemText}>17</Text>
                        <Text style={styles.itemText}>17</Text>
                    </View>
                </View>

                <View style={[styles.item,{flexWrap:'wrap',alignContent:'space-between'}]}>
                    <View style={styles.itemColumn}>
                        <Text style={styles.itemText}>18</Text>
                        <Text style={styles.itemText}>18</Text>
                        <Text style={styles.itemText}>18</Text>
                    </View>
                    <View style={styles.itemColumn}>
                        <Text style={styles.itemText}>18</Text>
                        <Text style={styles.itemText}>18</Text>
                        <Text style={styles.itemText}>18</Text>
                    </View>
                </View>

                <View style={[styles.item,{flexWrap:'wrap',flexDirection:'row',alignContent:'space-between'}]}>
                    <View style={styles.itemRow}>
                        <Text style={styles.itemText}>19</Text>
                        <Text style={styles.itemText}>19</Text>
                        <Text style={styles.itemText}>19</Text>
                    </View>
                    <View style={[styles.itemRow,{justifyContent:'center'}]}>
                        <Text style={styles.itemText}>19</Text>
                    </View>
                    <View style={styles.itemRow}>
                        <Text style={styles.itemText}>19</Text>
                        <Text style={styles.itemText}>19</Text>
                    </View>
                </View>

                <View style={[styles.item,{flexWrap:'wrap',flexDirection:'row',alignContent:'space-between'}]}>
                    <View style={styles.itemRow}>
                        <Text style={styles.itemText}>20</Text>
                        <Text style={styles.itemText}>20</Text>
                        <Text style={styles.itemText}>20</Text>
                    </View>
                    <View style={styles.itemRow}>
                        <Text style={styles.itemText}>20</Text>
                        <Text style={styles.itemText}>20</Text>
                        <Text style={styles.itemText}>20</Text>
                    </View>
                    <View style={styles.itemRow}>
                        <Text style={styles.itemText}>20</Text>
                        <Text style={styles.itemText}>20</Text>
                        <Text style={styles.itemText}>20</Text>
                    </View>
                </View>

            </View>
        )
    }
}

const styles = StyleSheet.create({
    container:{
        backgroundColor:'blue',
        flex:1,
        justifyContent:'space-between',
        flexWrap:'wrap',
        flexDirection:'row',
        marginVertical: 50
    },
    item:{
        backgroundColor:'#fff',
        height:80,
        width:80,
        margin:4
    },
    itemText:{
        color:'#000',
    },
    itemRow:{
        flexBasis:'100%',
        flexDirection:'row',
        justifyContent:'space-between'
    },
    itemColumn:{
        flexBasis:'100%',
        justifyContent:'space-between'
    }
})
```



参考链接：

[http://www.ruanyifeng.com/blog/2015/07/flex-grammar.html](http://www.ruanyifeng.com/blog/2015/07/flex-grammar.html)

[http://www.ruanyifeng.com/blog/2015/07/flex-examples.html](http://www.ruanyifeng.com/blog/2015/07/flex-examples.html)

