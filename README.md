## Homework  
Для работы с github Actions я создал папку .github/workflows/  
Там создал yml файл.
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
