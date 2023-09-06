cmake_minimum_required(VERSION 3.20)

set(CMAKE_C_COMPILER_ID GNU)
set(CMAKE_CXX_COMPILER_ID GNU)
set(CMAKE_COMPILER_IS_GNUCC 1)
set(CMAKE_C_COMPILE_WORKS 1)

project(dynamoRIO_trace_reader)

add_executable(${PROJECT_NAME} main.c)

include(cmake/DynamoRIO.cmake)

if (UNIX)
  set_target_properties(${PROJECT_NAME}
            PROPERTIES
            RUNTIME_OUTPUT_DIRECTORY "${PROJECT_BINARY_DIR}"
            )
  set(DynamoRIO_USE_LIBC OFF)
endif (UNIX)
configure_DynamoRIO_standalone(${PROJECT_NAME})

if (WIN32)
  set_target_properties(drsyms_static PROPERTIES
            INTERFACE_LINK_LIBRARIES "libcpmt;libcmt;dynamorio;dbghelp;${DynamoRIO_ROOT}/lib${_DR_bits}/dbghelp_imports.lib;drcontainers;dwarf;elftc"
            )
  set_target_properties(binary_rts_resolver
            PROPERTIES
            RUNTIME_OUTPUT_DIRECTORY_DEBUG ${CMAKE_CURRENT_BINARY_DIR})
endif (WIN32)

# Find and copy dynamorio.dll and dbghelp.dll
DynamoRIO_get_full_path(DR_TARGET_LOCATION dynamorio "${location_suffix}")
file(COPY "${DR_TARGET_LOCATION}" DESTINATION ${PROJECT_BINARY_DIR})
if (WIN32)
    file(COPY "${DynamoRIO_ROOT}/ext/lib${_DR_bits}/${_DR_type}/dbghelp.dll" DESTINATION ${PROJECT_BINARY_DIR})
endif (WIN32)
