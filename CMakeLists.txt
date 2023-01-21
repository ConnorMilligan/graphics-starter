cmake_minimum_required(VERSION 3.0)
project(graphics-starter)

# Set normal compiler flags
set(CMAKE_CXX_STANDARD 14)
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -Wno-deprecated -Werror=return-type")

# Checks to see if git is present
find_package(Git REQUIRED)

# Initialize the submodule if not already done so
if(NOT EXISTS lib/freeglut/CMakeLists.txt)
    execute_process(COMMAND ${GIT_EXECUTABLE} submodule update --init --recursive -- ${dir}
            WORKING_DIRECTORY ${PROJECT_SOURCE_DIR}
            COMMAND_ERROR_IS_FATAL ANY)
endif()

# Do not build the demos or static libs for freeGLUT
option(FREEGLUT_BUILD_DEMOS OFF)
option(FREEGLUT_BUILD_STATIC_LIBS OFF)

# Add freeGLUT to the project
include_directories("lib/freeglut/include")
add_subdirectory("lib/freeglut")

# Set the project files
file(GLOB PROJECT_HEADERS *.h)
file(GLOB PROJECT_SOURCES *.cpp)
file(GLOB PROJECT_CONFIGS CMakeLists.txt
                          Readme.md
                         .gitattributes
                         .gitignore
                         .gitmodules)


source_group("Headers" FILES ${PROJECT_HEADERS})
source_group("Sources" FILES ${PROJECT_SOURCES})

# Build the source files
add_executable(${PROJECT_NAME} ${PROJECT_SOURCES} ${PROJECT_HEADERS} ${PROJECT_CONFIGS})

# Link freeGLUT to the executable
target_link_libraries(${PROJECT_NAME} freeglut)
