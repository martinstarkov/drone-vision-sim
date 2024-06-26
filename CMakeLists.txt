cmake_minimum_required(VERSION 3.16)

project(simulation)

set(CMAKE_CXX_STANDARD 17)

find_package(OpenCV REQUIRED)

set(SOURCE_FILES src/main.cpp)
file(GLOB_RECURSE HEADER_FILES CONFIGURE_DEPENDS "src/*.h" "src/*.hpp")

include(FetchContent)

FetchContent_Declare(csvparser
  GIT_REPOSITORY https://github.com/AriaFallah/csv-parser.git
  GIT_TAG master
)

FetchContent_MakeAvailable(csvparser)

add_executable(simulation ${SOURCE_FILES} ${HEADER_FILES})

target_include_directories(simulation PRIVATE src)

target_link_libraries(simulation ${OpenCV_LIBS} AriaCsvParser)

set_property(DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR} PROPERTY VS_STARTUP_PROJECT simulation)

if (CMAKE_IMPORT_LIBRARY_SUFFIX)
  add_custom_command(
    TARGET simulation POST_BUILD
    COMMAND ${CMAKE_COMMAND} -E copy $<TARGET_RUNTIME_DLLS:simulation> $<TARGET_FILE_DIR:simulation>
    COMMAND_EXPAND_LISTS
  )
endif ()
