# ----------------------------------------------------------------------------------------
# This file belongs to the QuickBlocks project from QuickBlocks.io
# ----------------------------------------------------------------------------------------
# Minimum cmake version supported
cmake_minimum_required (VERSION 2.6)

# ----------------------------------------------------------------------------------------
# folder project
project (quickBlocks)

# ----------------------------------------------------------------------------------------
# Debug msg
# message(DEBUG  "Building src")

# ----------------------------------------------------------------------------------------
# Allow targets in cascade
set_property (GLOBAL PROPERTY ALLOW_DUPLICATE_CUSTOM_TARGETS 1)

# ----------------------------------------------------------------------------------------
# Mandatory python interpret, the directive PythonInterp sets the following variables
message (STATUS "======== LOOKING FOR PYTHON ========================")
# PYTHONINTERP_FOUND         - Was the Python executable found
# PYTHON_EXECUTABLE          - path to the Python interpreter
find_package (PythonInterp REQUIRED)
find_package (PythonLibs REQUIRED)

# ----------------------------------------------------------------------------------------
# We require the CURL library to be installed for most apps and tools
# if found add its include path to includes section and link with it
find_package (CURL REQUIRED)
if (CURL_FOUND)
    include_directories(${CURL_INCLUDE_DIRS})
endif (CURL_FOUND)

# ----------------------------------------------------------------------------------------
# Globally available C++ settings
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++11 -Werror -Wall -O2")

# ----------------------------------------------------------------------------------------
# Linkable libraries
set (BASE_LIBS acct abi ether util ${CURL_LIBRARIES})

# ----------------------------------------------------------------------------------------
# Folders for various purposes
set (SCRIPTS_DEST_PATH "${CMAKE_SOURCE_DIR}/../bin")

# ----------------------------------------------------------------------------------------
# Enable testing
enable_testing()

set (TEST_PATH   "${CMAKE_SOURCE_DIR}/../test/working")
set (GOLD_PATH   "${CMAKE_SOURCE_DIR}/../test/gold")
set (TEST_CHECK  "${SCRIPTS_DEST_PATH}/testCheck.py")
set (TAG_REPLACE "${SCRIPTS_DEST_PATH}/replaceTag.py")

# ----------------------------------------------------------------------------------------
# Builds the README.md file in the given folder
function(build_readme sourcePath toolName)
    set(README_FILE "${sourcePath}/README.md")
    set(TOOL_README_TEMPLATE "${sourcePath}/docs/README.tmpl.md")
    add_custom_command(OUTPUT help.txt COMMAND ${toolName} "-th" ">" help.txt "2>&1")
    add_custom_target(readme COMMAND "${TAG_REPLACE}" "${TOOL_README_TEMPLATE}" "${toolName}" help.txt "${README_FILE}")
endfunction(build_readme)

# ----------------------------------------------------------------------------------------
# Runs test and checks output against expected gold file
function(run_the_test testPath goldPath testName testExe) # Optional argv4 with command parameters
	set(TEST_FILE "${testPath}/${testName}.txt")
	set(GOLD_FILE "${goldPath}/${testName}.txt")
#message (STATUS " ")
#message (STATUS "== IN ====== run_the_test ${testName} ========================")
	if (NOT ${ARGV4} STREQUAL "" )
		# In ARGV4 we receive the parameters to be passed to testExe  (optional)
	    add_test(NAME ${testName} COMMAND ${TEST_CHECK} ${testExe} ${ARGN} ${TEST_FILE} ${GOLD_FILE})
#        message(STATUS ${testName} " " COMMAND " " ${TEST_CHECK} " " ${testExe} " " ${ARGN} " " ${TEST_FILE} " " ${GOLD_FILE})
        else()
	    add_test(NAME ${testName} COMMAND ${TEST_CHECK} ${testExe} ${TEST_FILE} ${GOLD_FILE})
#        message(STATUS ${testName} " " COMMAND " " ${TEST_CHECK} " " ${testExe} " " ${TEST_FILE} " " ${GOLD_FILE})
        endif()
#message (STATUS "=== OUT ===== run_the_test ${testName} ========================")
#message (STATUS "  ")
endfunction(run_the_test)

# ----------------------------------------------------------------------------------------
# Compilation order for the subfolders
add_subdirectory (libs)
add_subdirectory (test-libs)
add_subdirectory (apps)
add_subdirectory (tools)
add_subdirectory (samples)
#add_subdirectory (monitors)
#add_subdirectory (other)
