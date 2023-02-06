cmake_minimum_required(VERSION 2.8.12)
project(example)

add_subdirectory(extern/pybind11) # 已经编译完的pybind11
pybind11_add_module(example example.cpp)