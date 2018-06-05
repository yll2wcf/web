>对于使用Objective-C开发iOS的程序员来说，对象是否为nil在编写程序的过程中程序员不太关心，直接使用就可以，在一些需要判断的地方在判断，当转为Swift开发时，首先头疼的问题就是可选类型，到底何时使用？，何时使用！总是拿不太准，下面就详细的讲解一下

<div align=center>![](https://user-gold-cdn.xitu.io/2018/6/5/163cec66615fb934?w=600&h=336&f=jpeg&s=7352)

###一、什么是可选类型
可选类型伴随着Swift诞生，在原有的Objective-C语言中不存在，究其原因，是因为Swift是类型安全的语言，而OC则是弱类型语言，OC中 str字符串既可以是nil，也可以是字符串，而Swift中，这两种状态是不能同时存在的。
OC和Swift对于nil的解释也是不太一样的
> 1.Objective-C中的nil:表示缺少一个合法的对象，是指向不存在对象的指针，对结构体、枚举等类型不起作用(会返回NSNotFound)
2.Swift中的nil:表示任意类型的值缺失，是一个确定的值，要么是该类型的一个值要么什么都没有(即为nil)

在Swift中Optional(可选类型)是一个含有两种情况的枚举，None 和 Some(T)，用来表示可能有或可能没有值。任何类型都可以明确声明为（或者隐式转换）可选类型。当声明一个可选类型的时候，要确保用括号给 ? 操作符一个合适的范围。

#### 1.1可选类型的声明

```
var optionalStr: String? = "swift语言可选类型"//声明可选类型字符串，并赋初值
var opStu:Student? //声明可选opStu对象，赋初值nil
```
***注意:在类型和 ?之间没有空格***

#### 1.2 强制解析(拆包)
当你确定自定义的可选类型一定有值时，可以使用操作符(!)进行强制解析，拿到数据，叹号表示"我知道一定有值，请使用它",但是当你判断错误，可选值为nil时使用(!)进行强制解析，会有运行错误。

```
var myStr:String? = nil
myStr="强制解析，一定有值"
if myStr != nil {
    print(myStr!)//使用！进行强制解析
}else{
    print("字符串为nil")
}
```
运行结果
```
强制解析，一定有值
```
#### 1.3 自动解析
在最初的声明时使用？修饰，当你希望它自动解析是可以用！代替？来修饰，这样在变量使用时就不需要加！来强制拆包，它会自动解析。
```
var myStr:String! //使用！修饰
myStr="自动解析"
if myStr != nil {
    print(myStr)
}else{
    print("字符串为nil")
}
```
运行结果
```
自动解析
```
#### 1.4 可选绑定
A.使用可选绑定，摆脱了频繁的判断是否为nil在赋值，但是使用可选绑定（optional binding）来判断可选类型是否包含值，如果包含就把值赋给一个临时常量或者变量。可选绑定可以用在if和while语句中来对可选类型的值进行判断并把值赋给一个常量或者变量。
格式：
```
if let tempStr = someOptional{
    tempStr//如果someOptiona有值的话，就会赋值给tempStr，然后使用
}
```
```
var myStr:String?
myStr="可选绑定"
if  let tempStr = myStr {
    print(tempStr)
}else{
    print("字符串为nil")
}
```
运行结果
```
可选绑定
```
B.在可选类型判断中还有guard let的用法，guard名为守卫的意思。guard let和if let刚好相反，guard let守护一定有值。如果没有，直接返回
```
let name: String? = "张三"
let age: Int? = 10

guard let nameNew = name,
      let ageNew = age
else {
        print("姓名 或 年龄 为nil")
        return
}

print("姓名 或 年龄 为\(name),\(age)")//运行到这里是一定有值的
```

#### 1.5  as! 与 as? 的类型转换
在写Swift代码时总是拿不准到底使用哪个，有时会根据编辑器的提示去Fix，但是这样不能很好的解决问题。
1.as!使用场合
>向下转型（Downcasting）时使用。由于是强制类型转换，如果转换失败会报 runtime 运行错误。就是说强制从父类转换成子类
```
class Animal {}
class Cat: Animal {}
let animal :Animal  = Cat()
let cat = animal as! Cat
```
2.as?使用场合
>as? 和 as! 操作符的转换规则完全一样。但 as? 如果转换不成功的时候便会返回一个 nil 对象。成功的话返回可选类型值（optional），需要我们拆包使用。由于 as? 在转换失败的时候也不会出现错误，所以对于如果能确保100%会成功的转换则可使用 as!，否则使用 as?

```
let animal:Animal = Cat()

if let cat = animal as? Cat{
    print("cat is not nil")
} else {
    print("cat is nil")
}
```
###二、可选类型的使用场景

#### 2.1  函数或方法的返回类型为可选类型
在方法中返回值如Int?、String?、(Int, String)?、[Int]?、[Int: String]?等
```
func returnOptionValue(value: Bool) -> String? { // 返回类型为可选String类型
    if value {
        return "返回类型是可选类型值" // 如果为真，返回Int类型的值1
    } else {
        return nil //返回nil
    }
}

let optionValue = returnOptionValue(value: true) // 要用可选绑定判断再使用，因为returnOptionValue为String？可选类型
if let value = optionValue {
    print(value)
} else {
    print("none value")
}
```
运行结果
```
返回类型是可选类型值
```
返回类型为闭包可选
```
func returnOptionalFunc(value: Bool) -> (() -> (Void))? { // 返回类型为可选类型的闭包
    if value {
        return { () in
            print("返回类型是可选类型闭包")
        }
    } else {
        return nil
    }
}

let possibleFunc = returnOptionalFunc(value: true) // 要用可选绑定判断再使用，因为possibleFunc 为可选类型的闭包，类型为() -> (Void)
if let aFunc = possibleFunc {
    print(aFunc())  // 注意增加()调用闭包，因为没有参数则是空括号
} else {
    print("none func")
}
```
运行结果
```
返回类型是可选类型闭包
```
#### 2.2  可选类型在类或结构体中的运用
可选类型在类中的使用
```
class PossibleClass {
    var someValue: Int
    var possibleValue: String? // 可选存储属性，默认值为nil
    init(someValue: Int) { // 构造方法中可以不对possibleValue属性初始化
        self.someValue = someValue
    }
}

let someClass = PossibleClass(someValue: 4)
```
可选类型在结构体中的运用
```
struct PossibleStruct {
    var someValue: Int
    var possibleValue: String? // 可选存储属性，默认值为nil
    // 结构体中可以自定义一个init构造器对属性初始化，也可以不自定义
}

let someStruct = PossibleStruct(someValue: 4, possibleValue: nil)
```
#### 2.3  可选类型在构造器中使用
```
class PossibleStructInit {
    let someValue: String
    init?(someValue: String) { // 可失败构造器
        if someValue.isEmpty { return nil } // 如果实例化为空串，则返回nil(即实例化失败)
        self.someValue = someValue
    }
}

let oneStruct = PossibleStructInit(someValue: "构造器中使用") // abc不是空串，oneStruct为PossibleStructInit？可选类型
if let one = oneStruct { // 使用if let可选绑定判断
    print(one.someValue)
} else {
    print("none value")
}
//输入结果为：构造器中使用

let twoStruct = PossibleStructInit(someValue: "") // 传参为空串
if let two = twoStruct {
    print(two.someValue)
} else {
    print("none value")
}
//输入结果为：none value
```
#### 2.4  可选类型在错误处理中使用（try!与try?）
>1.try?会将错误转换为可选值，当调用try?＋函数或方法语句时候，如果函数或方法抛出错误，程序不会发崩溃，而返回一个nil，如果没有抛出错误则返回可选值。
2.使用try!可以打破错误传播链条。错误抛出后传播给它的调用者，这样就形成了一个传播链条，但有的时候确实不想让错误传播下去，可以使用try!语句

```
let photo = try! loadImage(atPath: "./Resources/John Appleseed.jpg")
// 上述语句中在执行loadImage方法时如果执行失败，使用try!来禁用错误传递，会有运行错误导致App崩溃
```
```
func someThrowingFunction() throws -> Int {
    // ...
}

let x = try? someThrowingFunction() // x可能正常返回一个Int类型的值也有可能抛出一个错误异常，使用时对x用if let可选绑定判断
```