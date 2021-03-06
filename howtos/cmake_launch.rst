.. _cmake_launch:

Launch conan install from cmake
============================================

It is possible to launch ``conan install`` from cmake, which can be convenient for end users,
package consumers, that are not creating packages themselves.

This is work under **testing**, please try it and give feedback or contribute. The ``cmake``
code to do this task is here: https://github.com/conan-io/cmake-conan

To be able to use it, you can directly download the code from your cmake script:

.. code-block:: cmake

    cmake_minimum_required(VERSION 2.8)
    project(myproject CXX)

    # Download automatically, you can also just copy the conan.cmake file
    if(NOT EXISTS "${CMAKE_BINARY_DIR}/conan.cmake")
       message(STATUS "Downloading conan.cmake from https://github.com/conan-io/cmake-conan")
       file(DOWNLOAD "https://raw.githubusercontent.com/conan-io/cmake-conan/master/conan.cmake"
                     "${CMAKE_BINARY_DIR}/conan.cmake")
    endif()

    include(${CMAKE_BINARY_DIR}/conan.cmake)

    conan_cmake_run(REQUIRES Hello/0.1@memsharded/testing
                    BASIC_SETUP
                    BUILD missing)

    add_executable(main main.cpp)
    target_link_libraries(main ${CONAN_LIBS})


If you want to use targets, you could do:

.. code-block:: cmake

    include(conan.cmake)
    conan_cmake_run(REQUIRES Hello/0.1@memsharded/testing
                    BASIC_SETUP CMAKE_TARGETS
                    BUILD missing)

    add_executable(main main.cpp)
    target_link_libraries(main CONAN_PKG::Hello)
