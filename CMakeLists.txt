set(cmake_version 3.13.4)

cmake_minimum_required(VERSION ${cmake_version} FATAL_ERROR)

# set project name
set(ProjectName "binancetest")

project(${ProjectName}
		VERSION 0.1.1
		LANGUAGES CXX
        DESCRIPTION "binancetest in an example app that uses binance-cxx-api")

# Require out-of-source builds because it's a good practice to forbid polluting
# your project directory with garbage created during the build phase. Because later
# when you need to remove build files you can unintentionally remove your source files.
file(TO_CMAKE_PATH "${PROJECT_BINARY_DIR}/CMakeLists.txt" LOC_PATH)
if(EXISTS "${LOC_PATH}")
    message(FATAL_ERROR "You cannot build in a source directory (or any directory with a CMakeLists.txt file). Please make a build subdirectory. Feel free to remove CMakeCache.txt and CMakeFiles.")
endif()

set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_CXX_EXTENSIONS OFF)

# create CMAKE_GENERATOR_NAME_WITHOUT_SPACES variable to use it later in "deps/CMakeLists.txt" for creation path where dependencies will be downloaded
string(REPLACE " " "" CMAKE_GENERATOR_NAME_WITHOUT_SPACES "${CMAKE_GENERATOR}")

# To organize targets into groups when using IDE like XCode or VisualStudio, etc.
set_property(GLOBAL PROPERTY USE_FOLDERS ON)

# Create executable
add_executable(binancetest)

# Add dependencies
add_subdirectory(deps)

# Add sources, definitions, etc.
add_subdirectory(sources)

# Create groups in the IDE which mirrors directory structure on the hard disk
# It can be removed because currently we do not use any IDE(like XCode)
get_target_property(binancetest_sources binancetest SOURCES)
source_group(
  TREE   ${CMAKE_CURRENT_SOURCE_DIR}/sources/src
  FILES  ${binancetest_sources}
)
