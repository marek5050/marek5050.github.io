---
layout: post
title: "Configuring GTest on Mac"
subtitle:  "CS N378 Summer 2015"
date:   2015-06-14 21:50:51
categories: cs378 STL
status: publish
---

## Introduction

GoogleTest is a framework based on the xUnit architecture. This framework is to write test for C++ code and valid for the most common platforms. 


## Installation

To build and install the google test library, first, we must install python and cmake in our development environment. These are straight forward installations using DMG's. 

[Download CMake ][Cmake Download]  
[Download Python version 2.7][Python Download]

Now we can move onto installing GTest. [Download GTest 1.7.0] [GTest Download] from the Google GTest repository. Once downloaded configure GTest using. 

{% highlight bash %}
Download and Extract using: 
% gtest_version=1.7.0
% wget https://googletest.googlecode.com/files/gtest-$gtest_version.zip --no-check-certificate
% unzip gtest-$gtest_version.zip

For build the library type:
% cd gtest-$gtest_version
% mkdir mybuild
% cd mybuild
% cmake -G"Unix Makefiles" ..
% make

Install the compiled library:
% cp -r ../include/gtest /usr/local/include
% cp lib*.a /usr/local/lib
{% endhighlight %}

## Verify Functionality
We'll write up a simple function sum and test the different asserts GTest provides us. 
The basic functions of GTest can be found in the table: 

{: .table .table-striped}
|Fatal assertion	|Nonfatal assertion	|Verifies | 
| ----------------- | ----------------- | ------- |
| ASSERT_EQ(expected, actual); |	EXPECT_EQ(expected, actual); |	expected == actual |
| ASSERT_NE(val1, val2); |	EXPECT_NE(val1, val2); |	val1 != val2 |
| ASSERT_LT(val1, val2); |	EXPECT_LT(val1, val2); |	val1 < val2 |
| ASSERT_LE(val1, val2); |	EXPECT_LE(val1, val2); |	val1 <= val2 |
| ASSERT_GT(val1, val2); |	EXPECT_GT(val1, val2); |	val1 > val2 |
| ASSERT_GE(val1, val2); |	EXPECT_GE(val1, val2); |	val1 >= val2 |
|------------------------|-------------------------|---------------------|
|||[Source GTest Primer][Primer]|


Write up a test file called **test.cpp**
{% highlight c++ %}
#include <cassert>  // assert
#include <iostream> // cout, endl

#include <gtest/gtest.h>

int sum(int a, int b){
  return a+b;
}

TEST(MyUnitTests, test_1) {
    ASSERT_EQ(sum(1,1), 2);}

TEST(MyUnitTests, test_2) {
    ASSERT_EQ(sum(2,2), 4);}

TEST(MyUnitTests, test_3) {
    ASSERT_EQ(sum(10,-1), 9);}



GTEST_API_ int main(int argc, char **argv) {
  std::cout << "Running main() from testmain.cc\n";
 
  testing::InitGoogleTest(&argc, argv);
  return RUN_ALL_TESTS();
}
{% endhighlight %}



{% highlight bash %}
% g++ -pedantic -std=c++11 -Wall test.cpp -o UnitTest -lgtest -lgtest_main
Running main() from testmain.cc
[==========] Running 3 tests from 1 test case.
[----------] Global test environment set-up.
[----------] 3 tests from MyUnitTests
[ RUN      ] MyUnitTests.test_1
[       OK ] MyUnitTests.test_1 (0 ms)
[ RUN      ] MyUnitTests.test_2
[       OK ] MyUnitTests.test_2 (0 ms)
[ RUN      ] MyUnitTests.test_3
[       OK ] MyUnitTests.test_3 (0 ms)
[----------] 3 tests from MyUnitTests (0 ms total)

[----------] Global test environment tear-down
[==========] 3 tests from 1 test case ran. (0 ms total)
[  PASSED  ] 3 tests.
{% endhighlight %}




[Primer]: https://code.google.com/p/googletest/wiki/V1_7_Primer
[CMake Download]: http://www.cmake.org/download/
[Python Download]: https://www.python.org/downloads/
[GTest Download]: https://code.google.com/p/googletest/downloads/detail?name=gtest-1.7.0.zip&can=2&q=