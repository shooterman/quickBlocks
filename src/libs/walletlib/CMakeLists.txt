# minimum cmake version supported
cmake_minimum_required (VERSION 2.6)

# project
project (wallet)

# c++ compilation flags
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++11 -Wall -O2")


# The sources to be used
file(GLOB SOURCE_FILES "*.cpp")

# Output
set(CMAKE_ARCHIVE_OUTPUT_DIRECTORY "${CMAKE_SOURCE_DIR}/libs")

# Define the executable to be generated
add_library(${PROJECT_NAME} STATIC ${SOURCE_FILES})

# Headers required, it depends on utillib and abilib
target_include_directories(${PROJECT_NAME} PUBLIC "${CMAKE_SOURCE_DIR}/libs/utillib")
target_include_directories(${PROJECT_NAME} PUBLIC "${CMAKE_SOURCE_DIR}/libs/abilib")
target_include_directories(${PROJECT_NAME} PUBLIC "${CMAKE_SOURCE_DIR}/libs/etherlib")
target_include_directories(${PROJECT_NAME} PUBLIC "${CMAKE_SOURCE_DIR}/libs/tokenlib")
target_include_directories(${PROJECT_NAME} PUBLIC "${CMAKE_SOURCE_DIR}/libs/walletlib")

# Copy the library headers, make them available for the rest of modules
set(COMMON_INCLUDE_PATH "${CMAKE_SOURCE_DIR}/libs/include")
file(GLOB HEADER_FILES "*.h*")
file(COPY ${HEADER_FILES} DESTINATION ${COMMON_INCLUDE_PATH})

# plint target
add_custom_target(${LINT_TARGET}
        COMMAND ${PYLINT_CMD} ${SOURCE_FILES}
        WORKING_DIRECTORY ${CMAKE_CURRENT_LIST_DIR}
        COMMENT "Running pylint for ${TOOL_NAME}"
        VERBATIM)