Если запустить сборку из консоли и `Android Studio` то будет запущено два `Gradle` демона, и чтобы такого не было нужно использовать одинаковую `JDK` для `Android Studio` и в консоли

Также процессы запускаются при:
- также открытие нескольких проектов в `Android Studio` с разными версиями gradle
- запуск `Build `-> `Project clean`

Инструкция для `Windows`
1) Установить `Android Studio`
2) `File` -> `Settings` -> `Build...` -> `Build Tools` -> `Gradle` -> `Gradle JDK` -> `Download` -> `Jet Brains Runtime 17 (with JCEF)`
3) добавляем переменную `JAVA_HOME` с путем где лежит `Java`, и добавляем переменную `%JAVA_HOME%\bin` в `PATH`