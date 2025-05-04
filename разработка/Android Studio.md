Если запустить сборку из консоли и `Android Studio` то будет запущено два `Gradle` демона, и чтобы такого не было нужно использовать одинаковую `JDK` для `Android Studio` и в консоли

Также процессы запускаются при:
- также открытие нескольких проектов в `Android Studio` с разными версиями gradle
- запуск `Build `-> `Project clean`

Инструкция для `Windows`
1) Установить `Android Studio`
2) `File` -> `Settings` -> `Build...` -> `Build Tools` -> `Gradle` -> `Gradle JDK` -> `jbr-21`
3) Для конфигурирования вручную:
- Создайте файл `gradle.properties` в директории `.gradle` в домашней директории пользователя:
    
    - `~/.gradle для MacOS и Linux
    - `C:\Users\<USERNAME>\.gradle` для Windows пользователей
- Добавьте параметр `org.gradle.java.home` и в качестве значения укажите директорию из настроек Android Studio (`org.gradle.java.home=C:\Users\<user_name>\AppData\Local\Programs\Android Studio\jbr`)