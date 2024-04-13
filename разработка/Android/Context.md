Самая загадочная и покрытая тайнами, как говорит молодежь, дичь в [[Android]]
Но если не ударяться в философию, то Context - это класс, который обеспечивает доступ из одной части приложения (кода) к другой (ресурсам)
Получив context, вы заполучаете методы для считывания картинок, строковых ресурсов и всего остального, запиханного в ваш apk
А еще кто-то сильно подумал и решил, что он же будет отвечать за навигацию (запуск активитей) и работу фоновых служб (сервисов и бродкастов), то есть за взаимодействие приложения с системой
Позже кто-то не расслышал про "ни при каких условиях не создавать God-классы" и назначил контекст ответственным за работу с SharedPreference. Ну и за коннект со внутренними и внешними файлами заодно

Но в целом есть короткий ответ:
context - прокладка между кодом приложения и остальной системой

![[context.png]]

**[[Application]] Context**: It is our global context which means its instance is created only once and is used across whole application
With application context we are able to:
- Accessing application resources like strings, colors etc.
- Starting a service
- Binding to a service
- Sending a broadcast
- Registering a broadcast

**[[Activity]] Context:** This context declared seperately for each activity and has a theme since it inherits from ContextThemeWrapper. Each activty context has its own lifecycle and because of that, it must stay in its borders
With activity context, we are able to do everthing we can do with application context, plus:
- Layout Inflation
- Navigate to another Activity
- Start an Activity
- Show a Dialog

**[[Service]] Context:** It’s a context for Service class in Android and used for downloading operations or listening a music which sometimes has a very very long lifetime than our application. I mean it lives as long as our service lives