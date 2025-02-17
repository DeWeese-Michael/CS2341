# How to Run Tests

You can write a main function that contains test code that throws `std::runtime_error` if a test fails. However, it
is much easier to use a unit testing framework.
We will use a simple header-based unit testing framework called
[Catch2](https://github.com/catchorg/Catch2). It can be [integrated with CMake/CTest](https://github.com/catchorg/Catch2/blob/devel/docs/cmake-integration.md), but we will use the stand alone version. Another popular framework is
[GoogleTest](https://google.github.io/googletest/).


Here is an [example with tests.](Chapter1_Programming/factorial)

Steps:

1. Add the directory with the single include version of Catch2 to your project (copy directory `catch2` from [this example.](Chapter1_Programming/factorial)). 
2. You need a test file like `test.cpp` in the [testing example](Chapter1_Programming/factorial/) to define test cases and required outputs. There are [more assertion macros](https://github.com/catchorg/Catch2/blob/v2.x/docs/assertions.md) available.
3. Add the test to `CMakeLists.txt`. The lines should look like (see [testing example](Chapter1_Programming/factorial/) for complete file):
   ```
   # testing with the single_include versions of Catch2 V2 (in directory catch2)
   include(CTest)
   add_executable(tests test.cpp <other.cpp you need for compiling the test>)
   add_test(NAME SimpleTest COMMAND tests)
   ```

  With some compilers you also need to specify the C++ standard with 
  `set(CMAKE_CXX_STANDARD17)`.

4. Build the project and then run the tests using the button on the status bar. The test program will print the test results to the screen.

**Note:** Compiling the test with Catch2 takes a while. You can change the build target in the status bar from `[all]` to `[main]` or whatever your executable is called to only compile the program and not the tests.  

Testing using GitHub Actions:

1. At the repository on the Github page, click on Actions and find `CMake based projects`.
2. If you want to use Catch2 for testing, then you need to install it for the test. [See example workflow file](.github/workflows/testing-example_cmake.yml).

## License

<img src="https://licensebuttons.net/l/by-sa/3.0/88x31.png" alt="CC BY-SA 4.0" align="left">

All code and documents in this repository are provided under [Creative Commons Attribution-ShareAlike 4.0 International (CC BY-SA 4.0) License.](https://creativecommons.org/licenses/by-sa/4.0/)
