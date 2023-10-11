add_executable(unlock_camera0 ./unlock_camera0.cc)
target_link_libraries(unlock_camera0 ${SDKLIBS})

add_executable(unlock_camera1 ./unlock_camera1.cc)
target_link_libraries(unlock_camera1 ${SDKLIBS})

add_executable(lock_camera0 ./lock_camera0.cc)
target_link_libraries(lock_camera0 ${SDKLIBS})

add_executable(lock_camera1 ./lock_camera1.cc)
target_link_libraries(lock_camera1 ${SDKLIBS})

set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++11 -pthread")
