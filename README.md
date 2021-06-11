[![Build Status](https://travis-ci.com/qt-legion/lab05.svg?branch=main)](https://travis-ci.com/qt-legion/lab05)

Создайте CMakeList.txt для библиотеки banking.

Создайте модульные тесты на классы Transaction и Account.
    
Настройте сборочную процедуру на TravisCI.

## CMakeLists.txt
```
cmake_minimum_required(VERSION 3.2)
set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

project(bank)
SET(COVERAGE OFF CACHE BOOL "Coverage")
enable_testing()
add_subdirectory(third-party/gtest)
include_directories(${gtest_SOURCE_DIR}/include ${gtest_SOURCE_DIR})


add_library(lib1 Account.h Account.cpp)
add_library(lib2 Transaction.h Transaction.cpp)
add_executable(testst test.cpp)
target_compile_options(testst PRIVATE --coverage)
target_link_libraries(testst PRIVATE --coverage gtest gtest_main)
add_test( testst testst )
```
## .travis.yml
```
language: cpp


addons:
  apt:
    sources:
      - george-edison55-precise-backports
    packages:
      - cmake
      - cmake-data
      - mingw-w64


script:
- cmake .
- make
- ./testst

```
## test.cpp
```
#include "Account.cpp"
#include "Transaction.cpp"
#include "gtest/gtest.h"
TEST(Account, test1)
{
	Account A(1, 3);
	EXPECT_ANY_THROW(A.ChangeBalance(14));
}
TEST(Account, test2)
{
	Account A(1, 3);
	A.Lock();
	A.ChangeBalance(1);
	EXPECT_EQ(A.GetBalance(), 4);
}
TEST(Account, test3)
{
	Account A(1, 3);
	EXPECT_ANY_THROW(A.ChangeBalance(10));
}
TEST(Account, test4)
{
	Account A(1, 3);
	EXPECT_EQ(A.GetBalance(), 3);
}
TEST(Account, test5)
{
	Account A(1, 3);
	A.Lock();
	EXPECT_ANY_THROW(A.Lock());
}
TEST(Transaction, test1)
{
	Transaction B;
	EXPECT_EQ(B.fee(), 1);
}
TEST(Transaction, test2)
{
	Transaction B;
	B.set_fee(3);
	EXPECT_EQ(B.fee(), 3);
}
TEST(Transaction, test3)
{
	Transaction B;
	Account A1(1, 1000);
	Account A2(2, 500);
	B.set_fee(100);
	B.Make(A1, A2, 300);
	EXPECT_EQ(A1.GetBalance(), 600);
	EXPECT_EQ(A2.GetBalance(), 800);
}
TEST(Transaction, test4)
{
	Transaction B;
	Account A1(1, 1000);
	Account A2(2, 500);
	B.set_fee(137);
	EXPECT_FALSE(B.Make(A1, A2, 273));
}
TEST(Transaction, test5)
{
	Transaction B;
	Account A1(1, 1000);
	Account A2(2, 500);
	EXPECT_FALSE(B.Make(A1, A2, 2000));
}
TEST(Transaction, test6)
{
	Transaction B;
	Account A1(1, 1000);
	Account A2(2, 500);
	EXPECT_ANY_THROW(B.Make(A1, A2, -19));
}
TEST(Transaction, test7)
{
	Transaction B;
	Account A1(1, 1000);
	Account A2(2, 500);
	EXPECT_ANY_THROW(B.Make(A1, A2, -1));
	EXPECT_ANY_THROW(B.Make(A1, A2, 10));
}
TEST(Transaction, test8)
{
	Transaction B;
	Account A1(1, 1000);
```
