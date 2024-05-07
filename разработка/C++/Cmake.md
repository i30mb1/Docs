таргет - цель или компонент который необходимо собрать
проект это набор таких таргетов

```
cmake -B build -G Ninja
```
запускает конфигурирование проекта используя генератор Ninja

```
cmake --build "./build"  
```
компилируем и билдим проект в exe

```
SET(CMAKE_C_COMPILER "C:/Program Files/mingw64/bin/gcc.exe")  
SET(CMAKE_CXX_COMPILER "C:/Program Files/mingw64/bin/g++.exe")
```
устанавливаем переменные, кто будет компилировать наш код

```
add_executable(PROJECT_N7 tutorial.cxx)
```
говорим что нужно собрать .exe файл с таким именем из этих сорсов


```
add_subdirectory(libs/http)
```
добавляет поддиректорию в проект, в которой есть еще CmakeLists.txt (включает дополнительные части проекта в общую систему сборки)

```
target_link_libraries(
PROJECT_N7 - имя цели/проекта
PUBLIC 
httplib - имя библиотеки
)
```
для определения библиотек с которыми должна быть связана конкретная цель/проект
```
target_include_directories(PROJECT_N7 PUBLIC libs/http)
```
указываем путь где искать заголовочные файлы библиотеки 

```
add_subdirectory(libs/http)  
target_link_libraries(DemoCPP PUBLIC httplib)  
target_include_directories(DemoCPP PUBLIC libs/http)
```
в общем подключаем библиотеку которая представлена сорцами/другим проектом