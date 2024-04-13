anonymous function that we can trat as values

```Kotlin
val calculateSquare: (Int) -> Int = { value -> value * value }
```

переменная имеющая тип фукнции это реализация интерфейса FunctionN

```Kotlin
val calculateSquare = object : Function1<Int, Int> {  
    override fun invoke(value: Int): Int = value * value
}
```

Ключевое слово `return` в лямбда-выражениях производить выход из функции в которой вызывается эта лямба, это называется [[non-local return]]

## Lambdas with Receiver

used to acces the property of the receiver without any additional line of code

```Kotlin
fun build(action: (StringBuilder).() -> Unit): String
```


# Lambda vs Method Reference

While Lambda, captures **variables** by reference, which means they hold a reference to the **variable**, not a copy of its value. This means that when the lambda is invoked, it accesses the current value of the captured variable in its enclosing scope, not the value at the time of declaration

```Kotlin
class SayHelloTo(  
private val name: String  
) {  
fun onClick() {  
print(name)  
}  
}  
  
fun main() {  
var listener = SayHelloTo("no1")  
  
val lamdba = { listener.onClick() }  
val reference = listener::onClick  
  
listener = SayHelloTo("no2")  
  
lamdba() // prints no2  
reference() //prints no1  
}

