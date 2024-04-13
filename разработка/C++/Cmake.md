```
cmake -B build -G Ninja
```
запускает конфигурирование проекта 

```
cmake --build "./build"  
```
компилируем и билдим проект в exe

```
SET(CMAKE_C_COMPILER "C:/Program Files/mingw64/bin/gcc.exe")  
SET(CMAKE_CXX_COMPILER "C:/Program Files/mingw64/bin/g++.exe")
```
устанавливаем переменные

```
add_executable(Tutorial tutorial.cxx)
```
говорим где искать main 

```
target_include_directories(Tutorial PUBLIC weird)
```
указываем путь где искать заголовочные файлы библиотеки??