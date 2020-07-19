# java转kotlin基础语法（2.22）

## 1.创建一个运行类

java：

```java
public static void main(String[] args) {
	System.out.println("Hello World!") //代码
}
```

kotlin：
```kotlin
 fun main(args: Array<String>) {    // 包级可见的函数，接受一个字符串数组作为参数  	 				println("Hello World!")         // 分号可以省略 代码 
 } 
```

## 2.函数定义

1.  函数定义使用关键字 fun，参数格式为：参数 : 类型   

   ```kotlin
   fun sum(a: Int, b: Int): Int {   // Int 参数，返回值 Int
       return a + b
   }
   ```

2.  表达式作为函数体，返回类型自动推断： 

   ```kotlin
   fun sum(a: Int, b: Int) = a + b
   
   public fun sum(a: Int, b: Int): Int = a + b   // public 方法则必须明确写出返回类型
   ```

3. 无返回值的函数(类似Java中的void)： (现在好像直接省略Unit也是ok的)

   ```kotlin
   fun printSum(a: Int, b: Int): Unit { 
       print(a + b)
   }
   
   
   // 如果是返回 Unit类型，则可以省略(对于public方法也是这样)：
   public fun printSum(a: Int, b: Int) { 
       print(a + b)
   }
   ```

## 3. 可变长参数函数

​    函数的变长参数可以用 **vararg** 关键字进行标识： （类似java的数组）

```kotlin
fun vars(vararg v:Int){
    for(vt in v){
        print(vt)
    }
}

// 测试
fun main(args: Array<String>) {
    vars(1,2,3,4,5)  // 输出12345
}
```

## 4.lambda(匿名函数)

lambda表达式使用实例： （这是真tm奇怪）

```kotlin
// 测试
fun main(args: Array<String>) {
    val sumLambda: (Int, Int) -> Int = {x,y -> x+y}
    println(sumLambda(1,2))  // 输出 3
}
```

## 5.常量

 可变变量定义：var 关键字 

```kotlin
var <标识符> : <类型> = <初始化值>
```

```kotlin
var a : Int = 1;
```

 不可变变量定义：val 关键字，只能赋值一次的变量(类似Java中final修饰的变量) :

```
val <标识符> : <类型> = <初始化值>
```

```kotlin
val b : Int = 1;
```

常量与变量都可以没有初始化值,但是在引用前必须初始化

编译器支持自动类型判断,即声明时可以不指定类型,由编译器判断。

```kotlin
val a: Int = 1
val b = 1       // 系统自动推断变量类型为Int
val c: Int      // 如果不在声明时初始化则必须提供变量类型
c = 1           // 明确赋值


var x = 5        // 系统自动推断变量类型为Int
x += 1           // 变量可修改
```

## NULL检查机制

Kotlin的空安全设计对于声明可为空的参数，在使用时要进行空判断处理，有两种处理方式，字段后加!!像Java一样抛出空异常，另一种字段后加?可不做处理返回值为 null或配合?:做空判断处理

```kotlin
//类型后面加?表示可为空
var age: String? = "23" 
//抛出空指针异常
val ages = age!!.toInt()
//不做处理返回 null
val ages1 = age?.toInt()
//age为空返回-1
val ages2 = age?.toInt() ?: -1
```

## 类型检测及自动类型转换

我们可以使用 is 运算符检测一个表达式是否某类型的一个实例(类似于Java中的instanceof关键字)。

使用is判断之后，回被自动转化为String类型

```kotlin
fun getStringLength(obj: Any): Int? {
  if (obj is String) {
    // 做过类型判断以后，obj会被系统自动转换为String类型
    return obj.length 
  }

  //在这里还有一种方法，与Java中instanceof不同，使用!is
  // if (obj !is String){
  //   // XXX
  // }

  // 这里的obj仍然是Any类型的引用
  return null
}
```

## 区间

区间表达式由具有操作符形式 **..** 的 rangeTo 函数辅以 in 和 !in 形成。

区间是为任何可比较类型定义的，但对于整型原生类型，它有一个优化的实现。以下是使用区间的一些示例:

```kotlin
for (i in 1..4) print(i) // 输出“1234”

for (i in 4..1) print(i) // 什么都不输出

if (i in 1..10) { // 等同于 1 <= i && i <= 10
    println(i)
}

// 使用 step 指定步长
for (i in 1..4 step 2) print(i) // 输出“13”

for (i in 4 downTo 1 step 2) print(i) // 输出“42”


// 使用 until 函数排除结束元素
for (i in 1 until 10) {   // i in [1, 10) 排除了 10
     println(i)
}
```