компонент [[Android]]

один и тот же instance нельзя показать в одном `fragmentManager`, перед добавлением можно смотреть `Fragment.isAdded` и `isMainThread` чтобы не получить ошибку

### При вызове диалога из Fragment

перед вызовом диалога нужно проверить `Fragment.isStateSaved` для предотвращения ошибки `IllegalStateException (Can not perform this action after onSavedState)`
или вместо `commit()` использовать `commitWithStateLoss()`

### Жизненный Цикл

Before API 28
'''
`LifecycleEvent.ON_STOP` -> `onSaveInstanceState()` -> `onStop()`
'''
API 28+
'''
`LifecycleEvent.ON_STOP` -> `onStop()` ->` onSaveInstanceState()`
'''

Если у нас есть `Fragment`
```Kotlin
val fragment: Fragment = //
```

```Kotlin
val fragment: Class<out Fragment> = //
```

Для его отображения понадобится написать пару строк
```Kotlin
val transaction = supportFragmentManager.beginTransaction()  
transaction.add(container.id, fragment, null)  
transaction.commit()
```

Если нужно чтобы при нажатии кнопки `back` мы его скрывали, нам нужно положить его в `backStack` дописав `addToBackStack(null)`

```Kotlin
val transaction = supportFragmentManager.beginTransaction()  
transaction.add(container.id, fragment, null)  
transaction.addToBackStack(null)
transaction.commit()
```

При попытке добавить тот же instance Fragment в ту же FragmentManager
```Kotlin
val transaction = supportFragmentManager.beginTransaction()  
transaction.add(container.id, fragment)  
transaction.addToBackStack(null)  
transaction.commit()  
  
val transaction2 = supportFragmentManager.beginTransaction()  
transaction2.add(container.id, fragment)  
transaction2.addToBackStack(null)  
transaction2.commit()
```

будет ошибка `java.lang.IllegalStateException: Fragment already added: fragment`

Чтобы ее избежать можно воспользоваться проверкой `fragment.isAdded` и если инстанс этого фрагмента уже был добавлен то `Exception` не произойдет
```Kotlin
if (!fragment.isAdded) {  
    val transaction2 = supportFragmentManager.beginTransaction()  
    transaction2.add(container.id, fragment)  
    transaction2.addToBackStack(null)  
    transaction2.commit()  
}
```

Но из-за того что метод `commit` асинхронный то в момент нашей проверки `fragment.isAdded` фрамент не будет будет добавлен и она будет ложно отрицательная что приведет к `java.lang.IllegalStateException: Fragment already added: fragment`

Чтобы добавить фрагмент синхронного можем воспользоваться `commitNow` 
```Kotlin
val transaction = supportFragmentManager.beginTransaction()  
transaction.add(container.id, fragment)  
transaction.addToBackStack(null)  
transaction.commitNow()  
  
if (!fragment.isAdded) {  
    val transaction2 = supportFragmentManager.beginTransaction()  
    transaction2.add(container.id, fragment)  
    transaction2.addToBackStack(null)  
    transaction2.commit()  
}
```

На этот раз получим другую ошибку `IllegalStateException: This transaction is already being added to the back stack` которая говорит о том что при использовании `commitNow` нельзя `fragment` класть в `backstack` из-за возможности его сломать если другие транзакции в `FragmentManager` еще выполняются

Так что если `fragment` нам не нужно класть в `backstack` то удаляем эту строчку и получаем наконец рабочий код
```Kotlin
val transaction = supportFragmentManager.beginTransaction()  
transaction.add(container.id, fragment)  
transaction.commitNow()  
  
if (!fragment.isAdded) {  
    val transaction2 = supportFragmentManager.beginTransaction()  
    transaction2.add(container.id, fragment)  
    transaction2.addToBackStack(null)  
    transaction2.commit()  
}
```

Ну а если захотим чтобы работало добавление в `backstack` тригерим чтобы все запланированные транзакции были выполнены командой `supportFragmentManager.executePendingTransactions` (Можно вызывать только с `Main` потока)

```Kotlin
val transaction = supportFragmentManager.beginTransaction()  
transaction.add(container.id, fragment)  
transaction.addToBackStack(null)  
transaction.commit()  
  
supportFragmentManager.executePendingTransactions()  
if (!fragment.isAdded) {  
    val transaction2 = supportFragmentManager.beginTransaction()  
    transaction2.add(container.id, fragment)  
    transaction2.addToBackStack(null)  
    transaction2.commit()  
}
```
