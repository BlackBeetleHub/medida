cmake_minimum_required (VERSION 3.8)

## Metadata

project (medida)
set (medida_VERSION_MAJOR 0)
set (medida_VERSION_MINOR 1)
set (medida_VERSION_PATCH 0)
set (medida_VERSION_TAG "")
set (medida_VERSION "${medida_VERSION_MAJOR}.${medida_VERSION_MINOR}.${medida_VERSION_PATCH}${medida_VERSION_TAG}")

## Configuration

if(NOT CMAKE_CONFIGURATION_TYPES AND NOT CMAKE_BUILD_TYPE)
    set(CMAKE_BUILD_TYPE "Release")
endif()

if((${CMAKE_COMPILER_IS_GNUCXX}) OR ("${CMAKE_CXX_COMPILER_ID}" MATCHES "Clang"))
    set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -g -fno-diagnostics-show-option -std=c++0x -Wall -Wextra -pedantic -Wno-long-long -Wno-deprecated -fno-strict-aliasing")
endif()

if(${CMAKE_SYSTEM_NAME} MATCHES "Linux")
    include(FindPkgConfig)
endif()

## Source

set(medida_SOURCES
        src/medida/stats/snapshot.cc
        src/medida/stats/ewma.cc
        src/medida/stats/uniform_sample.cc
        src/medida/stats/exp_decay_sample.cc
        src/medida/counter.cc
        src/medida/meter.cc
        src/medida/metric_name.cc
        src/medida/metric_processor.cc
        src/medida/metrics_registry.cc
        src/medida/timer.cc
        src/medida/timer_context.cc
        src/medida/reporting/abstract_polling_reporter.cc
        src/medida/reporting/console_reporter.cc
        src/medida/reporting/json_reporter.cc
        src/medida/reporting/util.cc
        src/medida/histogram.cc
        )

set(medida_HEADERS
        src/medida/medida.h
        src/medida/counter.h
        src/medida/histogram.h
        src/medida/meter.h
        src/medida/metered_interface.h
        src/medida/metric_interface.h
        src/medida/metric_name.h
        src/medida/metric_processor.h
        src/medida/metrics_registry.h
        src/medida/sampling_interface.h
        src/medida/summarizable_interface.h
        src/medida/timer.h
        src/medida/timer_context.h
        src/medida/reporting/abstract_polling_reporter.h
        src/medida/reporting/collectd_reporter.h
        src/medida/reporting/console_reporter.h
        src/medida/reporting/json_reporter.h
        src/medida/reporting/util.h
        src/medida/stats/ewma.h
        src/medida/stats/exp_decay_sample.h
        src/medida/stats/sample.h
        src/medida/stats/snapshot.h
        src/medida/stats/uniform_sample.h
        )

## Dependencies

find_package(Threads REQUIRED)

include_directories(
        ${medida_SOURCE_DIR}/src
        ${medida_BINARY_DIR}
)

add_library(medida STATIC
        ${medida_SOURCES}
        ${medida_PROTO_SRCS}
        )

target_link_libraries(medida
        ${CMAKE_THREAD_LIBS_INIT}
        )