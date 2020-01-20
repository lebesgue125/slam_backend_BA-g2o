cmake_minimum_required(VERSION 3.15)
project(backend)

set(CMAKE_CXX_STANDARD 14)
set(CMAKE_BUILD_TYPE "Release")
list(APPEND CMAKE_MODULE_PATH /home/lebesgue/Downloads/g2o/cmake_modules)
set(G2O_ROOT /usr/local/include/g2o)

find_package(Eigen3 3.1.0 REQUIRED)
find_package(Pangolin REQUIRED)
find_package(Sophus REQUIRED)
find_package(OpenCV REQUIRED)
find_package(G2O REQUIRED)
Find_Package(CSparse REQUIRED)

message(status "Pangolin_INCLUDE_DIRS-------" ${Pangolin_INCLUDE_DIRS})
message(status "EIGEN3_INCLUDE_DIR--------" ${EIGEN3_INCLUDE_DIR})
message(status "Sophus_INCLUDE_DIRS-------" ${Sophus_INCLUDE_DIRS})
message(status "g2o-------" ${G2O_INCLUDE_DIRS})

include_directories(
        ${EIGEN3_INCLUDE_DIR}
        ${Pangolin_INCLUDE_DIRS}
        ${Sophus_INCLUDE_DIRS}
        ${OpenCV_INCLUDE_DIRS}
        ${G2O_INCLUDE_DIRS}
        ${CSPARSE_INCLUDE_DIR}
        "/usr/local/boost/include"
)


set(CMAKE_RUNTIME_OUTPUT_DIRECTORY ${PROJECT_SOURCE_DIR})

#SET(G2O_LIBS g2o_csparse_extension g2o_stuff g2o_core cxsparse)
SET(G2O_LIBS g2o_cli g2o_ext_freeglut_minimal g2o_simulator g2o_solver_slam2d_linear
        g2o_types_icp g2o_types_slam2d g2o_types_sba g2o_types_slam3d g2o_core g2o_interface
        g2o_solver_csparse g2o_solver_structure_only g2o_csparse_extension g2o_opengl_helper g2o_solver_dense
        g2o_stuff g2o_types_sclam2d g2o_parser g2o_solver_pcg g2o_types_data g2o_types_sim3 cxsparse )


add_library(bal_common common.cpp)
add_executable(bundle_adjustment_g2o bundle_adjustment_g2o.cpp)

target_link_libraries(bundle_adjustment_g2o ${G2O_LIBS} bal_common)

add_executable(directBA directBA.cpp)

target_link_libraries(directBA
        ${G2O_LIBS}
        #${EIGEN3_LIBS}
        ${Pangolin_LIBRARIES}
        ${Sophus_LIBRARIES}
        ${OpenCV_LIBS}
        "/usr/local/boost/lib"
        )