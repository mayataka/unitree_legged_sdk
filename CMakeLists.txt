cmake_minimum_required(VERSION 2.8.3)
project(unitree_legged_sdk CXX)
set(CMAKE_CXX_STANDARD 11)

find_package(lcm REQUIRED)

macro(add_example EXAMPLE_NAME)
    add_executable(${EXAMPLE_NAME} examples/${EXAMPLE_NAME}.cpp)
    target_link_libraries(
        ${EXAMPLE_NAME}
        PUBLIC 
        pthread 
        ${PROJECT_SOURCE_DIR}/lib/libunitree_legged_sdk_amd64.so 
        lcm::lcm
    )
    target_include_directories(
        ${EXAMPLE_NAME}
        PUBLIC 
        ${PROJECT_SOURCE_DIR}/include
        ${LCM_INCLUDE_DIR}
    )
endmacro()

# one pc one process
add_example(example_position)
add_example(example_velocity)
add_example(example_torque)
add_example(example_walk)
add_example(example_dance)
add_example(example_wirelessHandle)

# one pc multi process
add_example(lcm_server_high)
add_example(lcm_server_low)

# multi pc
add_example(multi_pc_udp_send)
add_example(multi_pc_udp_recv)

# lcm server
add_example(lcm_server)

add_subdirectory(pybind11)
pybind11_add_module(
    robot_interface_high_level 
    src/python_interface_high_cmd.cpp
)
target_link_libraries(
    robot_interface_high_level
    PUBLIC 
    pthread 
    ${PROJECT_SOURCE_DIR}/lib/libunitree_legged_sdk_amd64.so 
    lcm::lcm
)
target_include_directories(
    robot_interface_high_level
    PUBLIC 
    ${PROJECT_SOURCE_DIR}/include
    ${LCM_INCLUDE_DIR}
)