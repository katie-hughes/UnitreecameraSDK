cmake_minimum_required( VERSION 3.8 )
project( UnitreeCameraSDK )

set(EXECUTABLE_OUTPUT_PATH ${PROJECT_SOURCE_DIR}/bins/)

# Set path for OpenCV if using it
if(UNITREE_CAMERA_USE_CUSTOM_OPENCV_DIR)
    set(OpenCV_DIR ${UNITREE_CAMERA_OPENCV_DIR}/lib/cmake/opencv4)
endif()

find_package(OpenCV 4 REQUIRED)
if(OpenCV_FOUND)
    include_directories(${OpenCV_INCLUDE_DIR})
    message(STATUS "OpenCV ${OpenCV_VERSION} FOUND")
    message(STATUS ${OpenCV_LIBS})
    if(${OpenCV_VERSION_MAJOR} EQUAL 4)
        add_definitions(-DOPENCV_VERSION_4)
    elseif(${OpenCV_VERSION_MAJOR} LESS 4)
        add_definitions(-DOPENCV_VERSION_3)
    endif()
else()
    message(FATAL_ERROR "OpenCV Library Not Found")
endif()

if(CMAKE_HOST_SYSTEM_PROCESSOR MATCHES "x86_64")
set(LIBRARY_OUTPUT_PATH ${PROJECT_SOURCE_DIR}/lib/amd64/)
link_directories(${PROJECT_SOURCE_DIR}/lib/amd64/)
else()
set(LIBRARY_OUTPUT_PATH ${PROJECT_SOURCE_DIR}/lib/arm64/)
link_directories(${PROJECT_SOURCE_DIR}/lib/arm64/)
endif()

set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++11 -pthread")
include_directories(${PROJECT_SOURCE_DIR}/include)

set(SDKLIBS unitree_camera tstc_V4L2_xu_camera udev systemlog ${OpenCV_LIBS})

add_subdirectory(${PROJECT_SOURCE_DIR}/examples)

# https://stackoverflow.com/questions/34234446/cmake-is-it-possible-to-build-an-executable-from-only-static-libraries-and-no-s
file(WRITE ${CMAKE_CURRENT_BINARY_DIR}/null.cpp "")

add_library(UnitreeCameraSDK ${CMAKE_CURRENT_BINARY_DIR}/null.cpp)
target_link_libraries(UnitreeCameraSDK 
    ${LIBRARY_OUTPUT_PATH}/libunitree_camera.a
    ${LIBRARY_OUTPUT_PATH}/libtstc_V4L2_xu_camera.a
    ${LIBRARY_OUTPUT_PATH}/libsystemlog.a
    udev
    ${OpenCV_LIBS}
)

# install include directories
install(DIRECTORY include/ DESTINATION include)

# install libs
install(DIRECTORY ${LIBRARY_OUTPUT_PATH} DESTINATION lib)

# install targets
install(TARGETS UnitreeCameraSDK
        EXPORT UnitreeCameraSDK-targets
        LIBRARY DESTINATION ${LIBRARY_OUTPUT_PATH}
        ARCHIVE DESTINATION ${LIBRARY_OUTPUT_PATH}
        INCLUDES DESTINATION include
)

# export targets
install(EXPORT UnitreeCameraSDK-targets
        FILE unitreecamerasdk-config.cmake
        NAMESPACE UnitreeCameraSDK::
        DESTINATION lib/cmake/${PROJECT_NAME}
)

