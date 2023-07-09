include(FetchContent)

# Setup directory to which dependencies will be downloaded
# i usually do this to avoid downloading dependecies every time after my build directory is cleaned up
cmake_path(APPEND FETCHCONTENT_BASE_DIR "${CMAKE_SOURCE_DIR}/../" "deps_content" "${CMAKE_GENERATOR_NAME_WITHOUT_SPACES}")

# If dependencies are not available then download it and make available
FetchContent_Declare(
	binance-cxx-api # project name
	GIT_REPOSITORY https://github.com/dmikushin/binance-cxx-api.git # reference to the git repo
	GIT_TAG        d880a88c5a54a03c8cc171150314f8f23ea5eaca # commit from which the binance-cxx-api mush be compiled
	                                                        # (at the time of writing this comment it was the last commit on master branch)
)

# If necessary it's possible to use binance-cxx-api from different location instead of downloaded from GitHub
# string(TOUPPER binance-cxx-api UP_binance-cxx-api)
# set(FETCHCONTENT_SOURCE_DIR_${UP_binance-cxx-api} ${CMAKE_SOURCE_DIR}/../binance-cxx-api)

FetchContent_GetProperties(binance-cxx-api)
if(NOT binance-cxx-api_POPULATED)
    FetchContent_Populate(binance-cxx-api)
    add_subdirectory(${binance-cxx-api_SOURCE_DIR} ${binance-cxx-api_BINARY_DIR})
endif()

# this is necessary to be able to write in our application lines like: #include "binance.h" or #include "binance_logger.h"
# but usually should be done in binance-cxx-api like this:
# target_include_directories(binance-cxx-api INTERFACE
#   $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include>
#   $<INSTALL_INTERFACE:include>
# )
target_include_directories(binance-cxx-api PUBLIC
  $<BUILD_INTERFACE:${binance-cxx-api_SOURCE_DIR}/include>
  $<INSTALL_INTERFACE:include>
)

# link binance-cxx-api
target_link_libraries(binancetest PRIVATE binance-cxx-api)

# If use IDE add targets into deps source group(currently we do not use nor XCode nor VisualStudio so this line can be deleted)
set_target_properties(binance-cxx-api PROPERTIES FOLDER deps)
