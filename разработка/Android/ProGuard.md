инструмент для сокращения, оптимизации, обфускации, верификации [[Java]] byte-кода

- shrinker - removes unused classes, fields, and methods from codebase
- optimizer - optimizes the bytecode of application
- obfuscator - renames classes, methods, and fields to obscure original names

enable ProGuard in Gradle

```groovy
proguardFiles getDefaultProguardFile("proguard-android-optimize.txt"), "proguard-rules.pro"
```

ensures that `myMethod` will not be obfuscated or removed

```Java
-keep

class com.example.MyClass{
public void method();
        }
```

disable inlining optimizations which can sometimes negatively impact

```Java
-optimizations!method/inlining/
```

do not rename classes that extends MyClass

```Java
-keepnames class extends*com.example.MyClass{*;}
```

https://medium.com/@attilaptkai/proguard-everything-you-need-to-know-bb5ff9c04bcd

# Удаление неиспользуемых ресурсов  
  
## Strict Mode  
  
По умолчанию R8 resource shrinker сохраняет ресурс, если есть строковая константа которая содержится в его имени.   
  
```  
Marking string:common_thursday:2132089269 used because it matches string pool constant co  
Marking string:common_time_hour:2132089270 used because it matches string pool constant co  
```  
  
То есть в обычном режиме ресурсы практически не удаляются, так как может существовать какая-нибудь константа `a`, которая встречается в большом количестве имен файлов ресурсов.  
  
Можно включить strict mode, который выключает эту проверку.   
  
```xml  
<?xml version="1.0" encoding="utf-8"?>  
<resources xmlns:tools="http://schemas.android.com/tools"  
tools:shrinkMode="strict" />  
```  
  
Сохраняются только те ресурсы, идентификаторы которых напрямую используются из кода или в других ресурсах, например ссылка на цвет в xml файле.  
  
```kotlin  
context.getColorCompat(R.color.background_engaging)  
```  
  
Ресурсы, для которых индентификатор формируется в коде динамически, будут удяляться в процессе работы R8. Например:  
  
```kotlin  
context.resources.getIdentifier(String name, String defType, String defPackage)  
```  
  
Подробнее в [документации Android](https://developer.android.com/build/shrink-code#strict-reference-checks)  

  
## Правила удаления и сохранения конфигов  
  
Можно удалять все ресурсы из `raw`, имена которых попадают под паттерн `*_dev_*`или `*_dev`.  
```xml  
<?xml version="1.0" encoding="utf-8"?>  
<resources xmlns:tools="http://schemas.android.com/tools"  
    tools:discard="@raw/*_dev_*,@raw/*_dev" />  
```  
  
Можно сохранять, например *.xml файлы из `res/xml`.  
```xml  
<?xml version="1.0" encoding="utf-8"?>  
<resources xmlns:tools="http://schemas.android.com/tools"  
    tools:keep="@xml/*" />  
```  
  
## Защита остальных ресурсов от удаления  
  
:::danger Важно!  
Стоит обращать внимание на имена создаваемых ресурсов. Из-за правил для удаления файлов конфигов для разных сборок,   
ресурсы имена которых попадают под эти паттерны могут быть удалены для определенных сборок.  
  
Например, `raw/something_prod` будет удален в dev сборке, несмотря на добавление keep правила для этого файла.   
Такое может происходить, так как [документация R8](https://developer.android.com/build/shrink-code#strict-reference-checks) никак не регламентирует порядок применение правил.  
:::  
  
Если вы столкнулись с такой проблемой, то необходимо создать файл `res/raw/<resourcePrefix>_shrink_config.xml` в модуле, где находится ресурс, который должен сохраниться в процессе удаления неиспользуемых ресурсов. При создании файла обязательно использовать префикс `<resourcePrefix>`, из gradle.build файла этого модуля. Если `resourcePrefix` не указан, то его следует добавить:  
```  
android {  
    resourcePrefix = "some_prefix_" // лучше использовать название модуля}  
```  
Иначе возникнет проблема файлов с одинаковыми именами и при мерже ресурсов такие файлы перезапишутся и созданные правила потеряются.  
В этом файле через запятую добавляем имена ресурсов, которые хотим сохранить.   
```xml  
<?xml version="1.0" encoding="utf-8"?>  
<resources xmlns:tools="http://schemas.android.com/tools"  
   tools:keep="@drawable/some_res"/>  
```  
  
Можно использовать маски для файлов, но делать это надо очень аккуратно. Маска `"tools:keep=@layout/*"` сохранит все файлы верстки по всему проекту, а не только в модуле, где она объявлена. Так происходит потому что правила для resource shrinker применяются уже после мержа ресурсов и распространяются на весь проект.