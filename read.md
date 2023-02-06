1，pybind11简介
    pybind11是一个只有头文件的轻量级库，它向Python暴露了C++的类型同时也向C++暴露了Python类型，pybind11主要用于创建现有C++代码的Python绑定。

2，编译
1.1 gcc版本
GCC 4.8或更新版本

sudo apt-get install gcc-4.8 cmake
conda install pytest


1.2 编译pybind11

    git clone https://github.com/pybind/pybind11.git
    cd pybind11
    mkdir build
    cd build
    cmake ..
    cmake --build . --config Release --target check
    make check -j 4

    git clone https://github.com/pybind/pybind11.git
    cd pybind11
    mkdir build
    cd build
    cmake ..
    cmake --build . --config Release --target check
    make check -j 4


3，代码
1，example.cpp

    #include <pybind11/pybind11.h>
    #include <pybind11/stl.h>
    #include <vector>

    // ----------------
    // Regular C++ code
    // ----------------

    // multiply all entries by 2.0
    // input:  nested std::vector ([[...],[...]]) (read-only)
    // output: nested std::vector ([[...],[...]]) (new copy)
    std::vector<std::vector<double>> modify(const std::vector<std::vector<double>>& input)
    {
    std::vector<std::vector<double>> output;

    std::transform(
        input.begin(),
        input.end(),
        std::back_inserter(output),
        [](const std::vector<double> &iv) {
        std::vector<double> dv;
        std::transform(iv.begin(), iv.end(), std::back_inserter(dv), [](double x) -> double { return 2.*x; });
        return dv;
        }
    );

    return output;
    }

    // ----------------
    // Python interface
    // ----------------

    namespace py = pybind11;

    PYBIND11_MODULE(example,m)
    {
    m.doc() = "pybind11 example plugin";

    m.def("modify", &modify, "Multiply all entries of a nested list by 2.0");
    }

2，CMakeLists.txt

cmake_minimum_required(VERSION 2.8.12)
project(example)

add_subdirectory(pybind11) # 已经编译完的pybind11
pybind11_add_module(example example.cpp)


3，python调用

import example

A = [[1,2,3,4],[5,6]]

B = example.modify(A)

print(B)

4，运行

cmake .
make
python test.py
[[2.0, 4.0, 6.0, 8.0], [10.0, 12.0]]