В основе систетмы лежит ядро Linux
Каждое установленное на телефоне приложение это отдельный пользователь в Linux. У него есть свой user_id и group_id
Каждое приложение работает в своей изолированной песочнице, имеет доступ только к своим данным (лежат по пути /data/data/<идентификатор приложения>) и не может напрямую получить данные других приложений

buildType (release, debug) - define like app build, allow to specify build configuration, optimization settings, debugging options
productFlavor (free, paid) - allow you to create different versions of your app with different resources, source code, configuration

targetSdk - this is the version of the Android SDK that the app is designed to run on
compileSdk - which version of SDK should compile your **code** to **bytecode**
