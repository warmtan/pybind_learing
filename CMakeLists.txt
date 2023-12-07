cmake_minimum_required(VERSION 2.8.12)
project(example)
set(PYTHON_EXECUTABLE /home/robot/anaconda3/envs/mujoco-env/bin/python)
set(pybind11_DIR /home/robot/anaconda3/envs/mujoco-env/lib/python3.8/site-packages/pybind11/share/cmake/pybind11)

find_package(pybind11 REQUIRED) # 已经编译完的pybind11
pybind11_add_module(example src/example.cpp)