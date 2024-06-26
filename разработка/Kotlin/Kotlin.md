Все передается по значению

```Kotlin
val number1: Int = 3
val number2: Int = number1
```

`const val` is used to declare a compile-time constant value
`val` is used to declare a read-only immutable variable

Теневые поля /Backing Fields - field cannot be declared directly in Kotlin classes. However, when a property need a backing field, Kotlin
provides it automatically. Может быть доступа через `field` в `get()/set()`

```Kotlin
import str.last as lastChar - переименовываем импорты
```

Вложенный клас без модификаторов аналог static inner class from [[Java]], чтобы превратить во внутренний со ссылкой на внешний нужно
добавить `inner` модификатор

Модификатор `sealed` разрешает наследоваться только вложенным классам или классам из одного пакета
Модификатор `reified` помогает получить класс дженерика для `inline` функций
Модификатор `lateinit` - поздняя инициализация без null типа, используется для [[DI]]
`as` - приведение к типу, если не получится то `Exception`
`as?`- безопасное приведение к типу, если не получится то `null`
`is`- проверка на тип
""" """ - тройные ковычки избавляют от необходимости экранирования символов и включать переносы

Оператор `==` сравнивает обьекты через `equals`
Оператор `===` сравнивает по ссылке
Оператор `?:` принимает 2 значения и возвращает 1 если не null или 2 если `null`
Оператор `*` - распаковывает массив в `vararg` (для передачи в методы с `vararg`)

Платформенный тип `String!` - означает что компилятор не знает будет ли значение `null`

Два равнозначных кода

```Kotlin
if(s!=null) s.toUpper() else null

s?.toUpper()
```
