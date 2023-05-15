## boilerplate for C++ projects

This is a boilerplate for C++ projects. What you get:

- Sources, headers and mains separated in distinct folders
- Use of modern [CMake](https://cmake.org/) for much easier compiling
- Setup for tests using [doctest](https://github.com/onqtam/doctest)
- Code coverage reports, including automatic upload to [Coveralls.io](https://coveralls.io/) and/or [Codecov.io](https://codecov.io)
  - Code coverage for CMake `only support Compiler GNU gcc or Clang`
- Code documentation with [Doxygen](http://www.stack.nl/~dimitri/doxygen/)
  - Code coverage report use [lcov](https://github.com/linux-test-project/lcov)


### setup

- install `doxygen`

```bash
# macOS
$ brew install doxygen lcov

# debian
$ sudo apt install -y doxygen graphviz lcov

# windows please install visual studio and add msbuild to env:PATH
> scoop install main/doxygen main/graphviz

# check setup
$ doxygen -v
$ dot -V
$ lcov -v
```

### Using the GitHub template

Click the `Use this template` button to make a new repository from this template.

### Structure

``` text
.
├── CMakeLists.txt
├── app
│   └── main.cpp
├── include
│   ├── example.h
│   └── exampleConfig.h.in
├── src
│   └── example.cpp
└── tests
    ├── dummy.cpp
    └── main.cpp
```

Sources go in [src/](src/), header files in [include/](include/), main programs in [app/](app), and
tests go in [tests/](tests/) (compiled to `unit_tests` by default).

If you add a new executable, say `app/hello.cpp`, you only need to add the following two lines to [CMakeLists.txt](CMakeLists.txt):

``` cmake
add_executable(main app/main.cpp)   # Name of exec. and location of file.
target_link_libraries(main PRIVATE ${LIBRARY_NAME})  # Link the executable to lib built from src/*.cpp (if it uses it).
```

You can find the example source code that builds the `main` executable in [app/main.cpp](app/main.cpp) under the `Build` section in [CMakeLists.txt](CMakeLists.txt).
If the executable you made does not use the library in [src/](src), then only the first line is needed.

## Building

Build by making a build directory (i.e. `build/`), run `cmake` in that dir, and then use `make` to build the desired target.

Example:

``` bash
> mkdir build
> cd build
> cmake .. -DCMAKE_BUILD_TYPE=[Debug | Coverage | Release]

# or
> cmake -Bbuild -DCMAKE_BUILD_TYPE=Debug
> cmake -Bbuild -DCMAKE_BUILD_TYPE=Coverage

# into build folder
> cd build
> make
# then build pass
# check out
> ./main
> make test      # Makes and runs the tests.
> make coverage  # Generate a coverage report. only run in CMAKE_BUILD_TYPE=Coverage
> make doc       # Generate html documentation.
```

- windows with powershell use [msbuild](https://learn.microsoft.com/cpp/build/msbuild-visual-cpp)

```ps1
> mkdir build
> cd build
> cmake .. -DCMAKE_BUILD_TYPE=[Debug | Coverage | Release]

# remove build
> Remove-Item -Recurse -Force .\build

# build debug
> cmake -Bbuild -DCMAKE_BUILD_TYPE=Debug
# into build folder
> cd .\build\
# will new vs project with [ CPP_BOILERPLATE.sln ]
> msbuild .\CPP_BOILERPLATE.sln /p:Configuration=Debug /t:main -m
# output
> .\Debug\main.exe
# unit test
> msbuild .\CPP_BOILERPLATE.sln /p:Configuration=Debug /t:unit_tests -m
> .\Debug\unit_tests.exe

# or relase
> cmake -Bbuild -DCMAKE_BUILD_TYPE=Release
# into build folder
> cd .\build\
> msbuild .\CPP_BOILERPLATE.sln /p:Configuration=Release /t:main -m
# output
> .\Release\main.exe
> msbuild .\CPP_BOILERPLATE.sln /p:Configuration=Release /t:unit_tests -m
# unit test
> .\Release\unit_tests.exe
```