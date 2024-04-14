## Homework

Представьте, что вы стажер в компании "Formatter Inc.".
### Задание 1
Вам поручили перейти на систему автоматизированной сборки **CMake**.
Исходные файлы находятся в директории formatter_lib.
В этой директории находятся файлы для статической библиотеки *formatter*.
Создайте `CMakeList.txt` в директории formatter_lib,
с помощью которого можно будет собирать статическую библиотеку *formatter*.


Скачиваем репозиторий в нужную папку и меняем origin

```
git clone https://github.com/tp-labs/lab03  
cd lab03
git remote remove origin
git remote add origin https://github.com/FapToBblu232/lab03_home
git branch wp/lab
git switch wp/lab
git push origin wp/lab
```

создаём Cmake файл  

```
cd formatter_lib
touch CMakeLists.txt
```
через nano редактируем  
Текст Cmake файла  
```
cmake_minimum_required(VERSION 3.22)
project(formatter_lib)

set(SOURCE_LIB formatter.cpp formatter.h)

add_library(formatter_lib STATIC ${SOURCE_LIB})
```
Теперь поработаем с ним  
```
mkdir build
cd build
cmake ..
cmake --build .
```
Вывод 1:  

```
-- The C compiler identification is GNU 11.4.0
-- The CXX compiler identification is GNU 11.4.0
-- Detecting C compiler ABI info
-- Detecting C compiler ABI info - done
-- Check for working C compiler: /usr/bin/cc - skipped
-- Detecting C compile features
-- Detecting C compile features - done
-- Detecting CXX compiler ABI info
-- Detecting CXX compiler ABI info - done
-- Check for working CXX compiler: /usr/bin/c++ - skipped
-- Detecting CXX compile features
-- Detecting CXX compile features - done
-- Configuring done
-- Generating done
-- Build files have been written to: /home/fartovii/workspace/projects/lab03/formatter_lib/build
```
Вывод 2:  

```
[ 50%] Building CXX object CMakeFiles/formatter_lib.dir/formatter.cpp.o
[100%] Linking CXX static library libformatter_lib.a
[100%] Built target formatter_lib
```

### Задание 2
У компании "Formatter Inc." есть перспективная библиотека,
которая является расширением предыдущей библиотеки. Т.к. вы уже овладели
навыком созданием `CMakeList.txt` для статической библиотеки *formatter*, ваш 
руководитель поручает заняться созданием `CMakeList.txt` для библиотеки 
*formatter_ex*, которая в свою очередь использует библиотеку *formatter*.  

```
cd ../../formatter_ex_lib
touch CMakeLists.txt
```

Через nano редактируем  
```
cmake_minimum_required(VERSION 3.22)

project(formatter_ex_lib)

add_library(formatter_ex_lib STATIC formatter_ex.cpp)

add_subdirectory("../formatter_lib" formatter_lib)

target_link_libraries(formatter_ex_lib PUBLIC formatter_lib)
target_include_directories(formatter_ex_lib PUBLIC
				"../formatter_ex_lib"
				"../formatter_lib")
```

Теперь билдим тут  

```
mkdir build
cd build
cmake ..
cmake --build .
```

```
-- The C compiler identification is GNU 11.4.0
-- The CXX compiler identification is GNU 11.4.0
-- Detecting C compiler ABI info
-- Detecting C compiler ABI info - done
-- Check for working C compiler: /usr/bin/cc - skipped
-- Detecting C compile features
-- Detecting C compile features - done
-- Detecting CXX compiler ABI info
-- Detecting CXX compiler ABI info - done
-- Check for working CXX compiler: /usr/bin/c++ - skipped
-- Detecting CXX compile features
-- Detecting CXX compile features - done
-- Configuring done
-- Generating done
-- Build files have been written to: /home/fartovii/workspace/projects/lab03/formatter_ex_lib/build
```
```
[ 25%] Building CXX object formatter_lib/CMakeFiles/formatter_lib.dir/formatter.cpp.o
[ 50%] Linking CXX static library libformatter_lib.a
[ 50%] Built target formatter_lib
[ 75%] Building CXX object CMakeFiles/formatter_ex_lib.dir/formatter_ex.cpp.o
[100%] Linking CXX static library libformatter_ex_lib.a
[100%] Built target formatter_ex_lib
```

### Задание 3
Конечно же ваша компания предоставляет примеры использования своих библиотек.
Чтобы продемонстрировать как работать с библиотекой *formatter_ex*,
вам необходимо создать два `CMakeList.txt` для двух простых приложений:
* *hello_world*, которое использует библиотеку *formatter_ex*;
* *solver*, приложение которое испольует статические библиотеки *formatter_ex* и *solver_lib*.

```
cd ../../hello_world_application

```

```
cmake_minimum_required(VERSION 3.22)

project(hello_world)

include_directories("../formatter_ex_lib/")

add_executable(hello_world hello_world.cpp)

add_subdirectory("../formatter_ex_lib/" formatter_ex_lib)

target_link_libraries(hello_world formatter_ex_lib)
```

```
cmake ..
cmake --build .
```

Вывод 1:

```
-- The C compiler identification is GNU 11.4.0
-- The CXX compiler identification is GNU 11.4.0
-- Detecting C compiler ABI info
-- Detecting C compiler ABI info - done
-- Check for working C compiler: /usr/bin/cc - skipped
-- Detecting C compile features
-- Detecting C compile features - done
-- Detecting CXX compiler ABI info
-- Detecting CXX compiler ABI info - done
-- Check for working CXX compiler: /usr/bin/c++ - skipped
-- Detecting CXX compile features
-- Detecting CXX compile features - done
-- Configuring done
-- Generating done
-- Build files have been written to: /home/fartovii/workspace/projects/lab03/hello_world_application/build

```
Вывод 2:

```
[ 16%] Building CXX object formatter_ex_lib/formatter_lib/CMakeFiles/formatter_lib.dir/formatter.cpp.o
[ 33%] Linking CXX static library libformatter_lib.a
[ 33%] Built target formatter_lib
[ 50%] Building CXX object formatter_ex_lib/CMakeFiles/formatter_ex_lib.dir/formatter_ex.cpp.o
[ 66%] Linking CXX static library libformatter_ex_lib.a
[ 66%] Built target formatter_ex_lib
[ 83%] Building CXX object CMakeFiles/hello_world.dir/hello_world.cpp.o
[100%] Linking CXX executable hello_world
[100%] Built target hello_world
```

*solver_lib*  
```
cd ../../solver_lib
touch CMakeLists.txt
mkdir build && cd build
```

Cmake-file
```
cmake_minimum_required(VERSION 3.22)
project(solver_lib)

set(SOURCE_LIB solver.cpp solver.h)

add_library(solver_lib STATIC ${SOURCE_LIB})
```

```
cmake ..
cmake --build .
```

1st output:
```
-- The C compiler identification is GNU 11.4.0
-- The CXX compiler identification is GNU 11.4.0
-- Detecting C compiler ABI info
-- Detecting C compiler ABI info - done
-- Check for working C compiler: /usr/bin/cc - skipped
-- Detecting C compile features
-- Detecting C compile features - done
-- Detecting CXX compiler ABI info
-- Detecting CXX compiler ABI info - done
-- Check for working CXX compiler: /usr/bin/c++ - skipped
-- Detecting CXX compile features
-- Detecting CXX compile features - done
-- Configuring done
-- Generating done
-- Build files have been written to: /home/fartovii/workspace/projects/lab03/solver_lib/build
```
2nd:
```
[ 50%] Building CXX object CMakeFiles/solver_lib.dir/solver.cpp.o
[100%] Linking CXX static library libsolver_lib.a
[100%] Built target solver_lib
```
Пришлось немного поменять cpp файл и подключить cmath

*solver_application*
```
cd ../../solver_application && touch CMakeLists.txt && mkdir build && cd build
```
Cmake file  
```
cmake_minimum_required(VERSION 3.22)

project(equation)

include_directories("../formatter_ex_lib/")
include_directories("../solver_lib/")

add_executable(equation equation.cpp)

add_subdirectory("../formatter_ex_lib/" formatter_ex_lib)
add_subdirectory("../solver_lib/" solver_lib)

target_link_libraries(equation formatter_ex_lib)
target_link_libraries(equation solver_lib)
```
```
cmake ..
cmake --build .
```

1st output:
```
-- The C compiler identification is GNU 11.4.0
-- The CXX compiler identification is GNU 11.4.0
-- Detecting C compiler ABI info
-- Detecting C compiler ABI info - done
-- Check for working C compiler: /usr/bin/cc - skipped
-- Detecting C compile features
-- Detecting C compile features - done
-- Detecting CXX compiler ABI info
-- Detecting CXX compiler ABI info - done
-- Check for working CXX compiler: /usr/bin/c++ - skipped
-- Detecting CXX compile features
-- Detecting CXX compile features - done
-- Configuring done
-- Generating done
-- Build files have been written to: /home/fartovii/workspace/projects/lab03/solver_application/build

```
2nd:
```
[ 12%] Building CXX object solver_lib/CMakeFiles/solver_lib.dir/solver.cpp.o
[ 25%] Linking CXX static library libsolver_lib.a
[ 25%] Built target solver_lib
[ 37%] Building CXX object formatter_ex_lib/formatter_lib/CMakeFiles/formatter_lib.dir/formatter.cpp.o
[ 50%] Linking CXX static library libformatter_lib.a
[ 50%] Built target formatter_lib
[ 62%] Building CXX object formatter_ex_lib/CMakeFiles/formatter_ex_lib.dir/formatter_ex.cpp.o
[ 75%] Linking CXX static library libformatter_ex_lib.a
[ 75%] Built target formatter_ex_lib
[ 87%] Building CXX object CMakeFiles/equation.dir/equation.cpp.o
[100%] Linking CXX executable equation
[100%] Built target equation
```