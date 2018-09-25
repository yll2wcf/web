在React native 开发中可以很方便的将通用的控件封装为子组件，本文主要介绍了父组件和子组件的交互，包含：  
（1）父组件传递属性到子组件  
（2）父组件调用子组件的方法  
（3）子组件调用组件中的方法

我们使用一个小Demo来说明这个问题，Demo的界面如下：

![](/assets/1537861664964.jpg)

子组件封装了一个FlatList，父组件中加载了子组件（图中灰色部分），父组件通过点击“调用子组件方法”按钮，调用子组件中的方法，修改列表中展示的内容，子组件选中列表中的某一项后，调用父组件的方法，展示到左上角的Text控件中

代码：  
Parent.js

```
import React,{Component} from 'react'
import{
    View,
    Text,
    StyleSheet,
    TouchableOpacity,
    SafeAreaView

}from 'react-native'

import Child from './Child'

export default class Parent extends Component{
    constructor(props){
        super(props)

        this.state={
            selectedText:'********'
        }
    }
    render(){
        return(
           <SafeAreaView
            style={styles.container}>

               <View style={{flexDirection:'row'}}>
                   <Text style={styles.text}>
                       {this.state.selectedText}
                   </Text>

                   <TouchableOpacity style={styles.btn} onPress={()=>{
                       this.childList.changeListData()
                   }}>
                       <Text>调用子组件的方法</Text>
                   </TouchableOpacity>
               </View>


               <Child  ref={(view)=>this.childList=view} listData={['早','中','晚']} _itemSelect={(item)=>{
                   this._showItemText(item)
               }}/>





           </SafeAreaView>
        )
    }

    _showItemText(item){
        console.log('((((((('+item)
        this.setState({
            selectedText:item
        })
    }
}

const styles = StyleSheet.create({
    container:{
        flex:1,
        alignItems:'center',
        justifyContent:'center'
    },
    text:{
        margin:80,
        marginTop:100,
        backgroundColor:'pink',
    },
    btn:{
        margin:80,
        marginTop:100,
        padding:10,
        backgroundColor:'red'
    }
})
```

Child.js

```
import React,{Component} from 'react'
import {
    View,
    FlatList,
    TouchableOpacity,
    StyleSheet,
    Text
}from 'react-native'

import PropTypes from 'prop-types'


export default class Child extends Component{

    constructor(props){
        super(props)

        this.state={
            listData:this.props.listData
        }
    }

    render(){
        return(
            <View>
                <FlatList data={this.state.listData}
                          renderItem={this._renderItem}
                          style={styles.flatList}
                          keyExtractor={(item, index) => 'time' + index}
                          getItemLayout={(data, index) => ({
                              length:40, offset: 40 * index, index
                          })}
                />
            </View>
        )
    }


    _renderItem=({item,index})=>{
        return(
            <TouchableOpacity style={styles.item}
                              onPress={()=>{ this.props._itemSelect(item)}}>
                <Text style={{fontSize:18, color: '#333333'}}>{item}</Text>
            </TouchableOpacity>
        )
    }

    changeListData(){
        this.setState({
            listData:['起床','工作','睡觉']
        })
    }
}



Child.propTypes = {
    _itemSelect:PropTypes.func,
    listData:PropTypes.array
};


Child.defaultProps = {
    _itemSelect:(index)=>{},
    listData:['早','中','晚']
};


const styles = StyleSheet.create({
    flatList:{
        backgroundColor:'#f0f0f0',
    },
    item:{
        paddingHorizontal:24,
        height: 40,
        justifyContent:'center'
    }
})
```

#### 1.父组件传递属性到子组件

```
listData={['早','中','晚']} _itemSelect={(item)=>{
                   this._showItemText(item)
```

子组件可以设定属性的类型和默认值

```
Child.propTypes = {
    _itemSelect:PropTypes.func,
    listData:PropTypes.array
};
Child.defaultProps = {
    _itemSelect:(index)=>{},
    listData:['早','中','晚']
};
```

#### 2.父组件调用子组件的方法

通过ref指向子组件，在需要的地方调用子组件中的方法。

ref是组件的一种特殊属性，可以理解为，组件被渲染后，指向组件的一个引用。我们可以通过ref属性来获取真实的组件。

```
ref={(view)=>this.childList=view}
```

```
 <TouchableOpacity style={styles.btn} onPress={()=>{
                       this.childList.changeListData()
                   }}>
                       <Text>调用子组件的方法</Text>
                   </TouchableOpacity>
```

#### 3.子组件调用组件中的方法

父组件将回调方法作为属性传递给子组件，子组件在需要的地方调用

```
listData={['早','中','晚']} _itemSelect={(item)=>{
                   this._showItemText(item)
```

```
 <TouchableOpacity style={styles.item}
                              onPress={()=>{ this.props._itemSelect(item)}}>
                <Text style={{fontSize:18, color: '#333333'}}>{item}</Text>
            </TouchableOpacity>
```



