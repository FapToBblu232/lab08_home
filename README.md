## Homework  
Для работы с github **Actions** я создал папку ```.github/workflows/```  
Там создал **yml файл**.
```
name: Lab
on: push
jobs:
    build-project:
        runs-on: ubuntu-latest
        steps:
            - name: Checkout
              uses: actions/checkout@v4
            
            - name: formater_lib
              run: |
                cd ${{ github.workspace }}/formatter_lib/
                rm -rf build
                mkdir build && cd build
                cmake ..
                cmake --build .
            - name: formater_ex_lib
              run: |
                cd ${{ github.workspace }}/formatter_ex_lib/
                rm -rf build
                mkdir build && cd build
                cmake ..
                cmake --build .
            - name: hello_world
              run: |
                cd ${{ github.workspace }}/hello_world_application/
                rm -rf build
                mkdir build && cd build
                cmake ..
                cmake --build .
            - name: solver_lib
              run: |
                cd ${{ github.workspace }}/solver_lib/
                rm -rf build
                mkdir build && cd build
                cmake ..
                cmake --build .
            - name: solver_application
              run: |
                cd ${{ github.workspace }}/solver_application/
                rm -rf build
                mkdir build && cd build
                cmake ..
                cmake --build .
```
Что у меня написано в каждом run'е:  
Когда я пытался забилдить каждую библиотеку, у меня вылетала ошибка, что кэш в репозитории не сопоставим с кэшем при выполнении поманды  
```
cmake --build .
```
Поэтому пришлось дистанционно удалять папку *build*, после чего создавать её обратно.  
Пример вывода для solver_application  
```
Run cd /home/runner/work/lab04_home/lab04_home/solver_application/
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
-- Configuring done (0.2s)
-- Generating done (0.0s)
-- Build files have been written to: /home/runner/work/lab04_home/lab04_home/solver_application/build
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
