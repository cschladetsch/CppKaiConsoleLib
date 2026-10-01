cmake_minimum_required(VERSION 3.20)

# CppKaiConsoleLib - the KAI Console: the REPL engine (Pi, Rho and shell
# commands, history, optional ENet network console) over Executor and the
# Pi/Rho translators. Builds the ConsoleLib target.
#
# Downstream of CppKaiLanguage and CppKaiCore. Extracted from CppKaiCore, where
# Console.cpp sat in Executor's sources but had to be built separately because
# it depends on Pi and Rho. Builds standalone or via add_subdirectory from a
# parent project (CppKAI, CppKaiShell).

if(NOT DEFINED PROJECT_NAME)
    project(CppKaiConsoleLib CXX)
endif()

if(NOT DEFINED CMAKE_CXX_STANDARD)
    set(CMAKE_CXX_STANDARD 23)
    set(CMAKE_CXX_STANDARD_REQUIRED ON)
    set(CMAKE_CXX_EXTENSIONS OFF)
endif()

# PiLang/RhoLang come from CppKaiLanguage, which in turn finds CppKaiCore. When
# built as part of a parent those targets already exist; standalone, set
# KAI_LANGUAGE_DIR and KAI_CORE_DIR to checkouts.
if(NOT TARGET PiLang)
    if(NOT KAI_LANGUAGE_DIR)
        message(FATAL_ERROR
            "CppKaiConsoleLib requires CppKaiLanguage and CppKaiCore. "
            "Set -DKAI_LANGUAGE_DIR=/path/to/CppKaiLanguage -DKAI_CORE_DIR=/path/to/CppKaiCore")
    endif()
    if(NOT DEFINED KAI_HOME)
        set(KAI_HOME ${CMAKE_CURRENT_SOURCE_DIR})   # CppKaiCore writes its libraries to ${KAI_HOME}/Bin
    endif()
    add_subdirectory(${KAI_LANGUAGE_DIR} ${CMAKE_BINARY_DIR}/CppKaiLanguage)
endif()

add_library(ConsoleLib ${CMAKE_CURRENT_SOURCE_DIR}/Source/Console.cpp)
target_include_directories(ConsoleLib PUBLIC ${CMAKE_CURRENT_SOURCE_DIR}/Include)
target_link_libraries(ConsoleLib PUBLIC Executor PiLang RhoLang CommonLang Core)

# Honour a parent-provided output directory (CppKAI's BIN_HOME).
if(DEFINED BIN_HOME)
    set_target_properties(ConsoleLib PROPERTIES
        ARCHIVE_OUTPUT_DIRECTORY "${BIN_HOME}"
        LIBRARY_OUTPUT_DIRECTORY "${BIN_HOME}")
endif()