## Homework  
Основная работа заключается в добавлении нужных тегов-параметров в Cmake  
Вот основной **CMakeLists.txt**
```bash
cmake_minimum_required(VERSION 3.22)

project(main)

set(PRINT_VERSION_PATCH 0)
set(PRINT_VERSION_MINOR 1)
set(PRINT_VERSION_MAJOR 0)
set(PRINT_VERSION_TWEAK 0)
set(PRINT_VERSION ${PRINT_VERSION_MAJOR}.${PRINT_VERSION_MINOR}.${PRINT_VERSION_PATCH}.${PRINT_VERSION_TWEAK})
set(PRINT_VERSION_STRING "v${PRINT_VERSION}")

add_subdirectory(${CMAKE_CURRENT_SOURCE_DIR}/formatter_ex_lib formatter_ex_new_name)

target_include_directories(formatter_ex PUBLIC formatter_ex_lib formatter_lib hello_world_application solver_lib)

add_library(solver ${CMAKE_CURRENT_SOURCE_DIR}/solver_lib/solver.cpp)

add_executable(main ${CMAKE_CURRENT_SOURCE_DIR}/solver_application/equation.cpp)

target_link_libraries(main formatter formatter_ex solver)

install(TARGETS main
RUNTIME DESTINATION bin
)

include(CPackConfig.cmake)
```
Это **CPackConfig.cmake**  
```bash
include(InstallRequiredSystemLibraries)

set(CPACK_PACKAGE_VERSION_MAJOR ${PRINT_VERSION_MAJOR})
set(CPACK_PACKAGE_VERSION_MINOR ${PRINT_VERSION_MINOR})
set(CPACK_PACKAGE_VERSION_PATCH ${PRINT_VERSION_PATCH})
set(CPACK_PACKAGE_VERSION_TWEAK ${PRINT_VERSION_TWEAK})
set(CPACK_PACKAGE_VERSION ${PRINT_VERSION})
set(CPACK_PACKAGE_DESCRIPTION_SUMMARY "Lab06_home")

set(CPACK_SOURCE_INSTALLED_DIRECTORIES "${CMAKE_SOURCE_DIR}; /")

set(CPACK_SOURCE_GENERATOR "TGZ;ZIP")

set(CPACK_DEBIAN_PACKAGE_NAME "lab06")
set(CPACK_DEBIAN_FILE_NAME "lab06_home-${PRINT_VERSION}.deb")
set(CPACK_DEBIAN_PACKAGE_ARCHITECTURE "all")
set(CPACK_DEBIAN_PACKAGE_MAINTAINER "FarToBblu232")
set(CPACK_DEBIAN_PACKAGE_DESCRIPTION "MB it works correctly")
set(CPACK_DEBIAN_PACKAGE_RELEASE 1)

set(CPACK_GENERATOR "DEB")

include(CPack)
```
Чтобы создать .deb файл я использовал следующее
```bash
mkdir _build && cd _build
cmake ..
cmake --build .
cpack
```
После чего в папке *_build* появился архив, который я перенёс в Arts и разпаковал  
Чтобы артефакты пофялялись ещё и дистанционно, я обновил yml файл  
```bash
name: Actions
on:
  push:
    branches: [main]
    tags:
       - "v*.*.*"
  pull_request:
    branches: [main]

jobs: 
 build_Linux:

  runs-on: ubuntu-latest

  steps:
  - uses: actions/checkout@v4

  - name: Configurate
    run: |
      mkdir ${{github.workspace}}/_build && cd ${{github.workspace}}/_build
      cmake ..
      cmake --build .

  - name: my_exe
    run: |
      echo -e "1\n2\n1" | ${{github.workspace}}/_build/main
 
  - name: package
    run: cmake --build ${{github.workspace}}/_build --target package
  
  - name: package_source
    run: cmake --build ${{github.workspace}}/_build --target package_source

  - name: Arts
    uses: actions/upload-artifact@v4
    with:
        name: main
        path: ${{github.workspace}}/_build/main

  - name: Make a release
    uses: ncipollo/release-action@v1
    with:
        artifacts: "${{github.workspace}}/_build/*.deb,${{github.workspace}}/_build/*.tar.gz,${{github.workspace}}/_build/*.zip"
        tag: 1.0.0
        token: ${{ secrets.GITHUB_TOKEN }}
        allowUpdates: true
```
Этот файл:
1. Создаёт артефакт из моего executable файла
2. Обновляет релиз 1.0.0
```
убрал папку _build из релиза
```
