project(GridFx)

set(APP_MAJOR_VERSION 1)
set(APP_MINOR_VERSION 0)
set(APP_PATCH_VERSION 0)

configure_file(
	"${PROJECT_SOURCE_DIR}/app_config.h.in"
	"${PROJECT_SOURCE_DIR}/app_config.h"
)

add_definitions(-DSOCI_USE_BOOST)
add_definitions(-DAVHTTP_ENABLE_ZLIB)
add_definitions(-DAVHTTP_ENABLE_OPENSSL)
add_definitions(-DAVHTTP_DISABLE_THREAD)

set(ZLIB_USE_STATIC_LIBS TRUE)
set(Boost_USE_STATIC_LIBS TRUE)
set(OPENSSL_USE_STATIC_LIBS TRUE)
set(SQLITE3_USE_STATIC_LIBS TRUE)
set(SOCI_USE_STATIC_LIBS TRUE)
set(FREEIMAGE_USE_STATIC_LIBS TRUE)

find_package(zlib REQUIRED)
find_package(Boost COMPONENTS filesystem system REQUIRED)
find_package(openssl REQUIRED)
find_package(sqlite3 REQUIRED)
find_package(soci REQUIRED)
find_package(freeImage REQUIRED)
find_package(freeglut REQUIRED)
find_package(Threads REQUIRED)

# header only
find_package(glm REQUIRED)
find_package(nlohmann_json REQUIRED)

include_directories(${Boost_INCLUDE_DIR})

include_directories($ENV{OPENSSL_ROOT_DIR}/include)
link_directories($ENV{OPENSSL_ROOT_DIR}/lib)

include_directories(${SOCI_INCLUDE_DIRS})
include_directories(${FREEGLUT_INCLUDE_DIRS})

include_directories(${CMAKE_SOURCE_DIR}/third_party/imgui)
include_directories(${CMAKE_SOURCE_DIR}/third_party/Box2D)
include_directories(${CMAKE_SOURCE_DIR}/third_party/avhttp/include)
include_directories(${CMAKE_SOURCE_DIR}/third_party/spine-runtimes/spine-cpp/spine-cpp/include)

include_directories(${CMAKE_SOURCE_DIR}/src)
include_directories(${CMAKE_SOURCE_DIR}/src/common)
include_directories(${CMAKE_SOURCE_DIR}/src/example)

set(EXTRA_LIBS ${EXTRA_LIBS} ${ZLIB_LIBRARIES})
set(EXTRA_LIBS ${EXTRA_LIBS} ${Boost_LIBRARIES})
set(EXTRA_LIBS ${EXTRA_LIBS} ${OPENSSL_LIBRARIES})
set(EXTRA_LIBS ${EXTRA_LIBS} ${SQLITE3_LIBRARIES})
set(EXTRA_LIBS ${EXTRA_LIBS} ${SOCI_LIBRARIES})
set(EXTRA_LIBS ${EXTRA_LIBS} ${FREEIMAGE_LIBRARIES})
set(EXTRA_LIBS ${EXTRA_LIBS} ${FREEGLUT_LIBRARIES})
set(EXTRA_LIBS ${EXTRA_LIBS} ${CMAKE_THREAD_LIBS_INIT})

file(GLOB APP_CPP ${CMAKE_SOURCE_DIR}/src/*.cpp)
file(GLOB_RECURSE COMMON_CPP ${CMAKE_SOURCE_DIR}/src/common/*.cpp)

file(GLOB IMGUI_CPP ${CMAKE_SOURCE_DIR}/third_party/imgui/*.cpp)
file(GLOB_RECURSE BOX2D_CPP ${CMAKE_SOURCE_DIR}/third_party/Box2D/Box2D/*.cpp)
file(GLOB SPINE_CPP ${CMAKE_SOURCE_DIR}/third_party/spine-runtimes/spine-cpp/spine-cpp/src/spine/*.cpp)

file(GLOB_RECURSE FLUID_SIM_CPP ${CMAKE_SOURCE_DIR}/src/example/fluid_sim/*.cpp)

set(APP_CPP ${APP_CPP} ${COMMON_CPP} ${IMGUI_CPP})
set(LIB_CPP)
set(EXP_CPP ${FLUID_SIM_CPP})

set(ALL_CPP ${APP_CPP} ${LIB_CPP} ${EXP_CPP})

add_executable(${PROJECT_NAME} ${ALL_CPP})
target_link_libraries(${PROJECT_NAME} ${EXTRA_LIBS})