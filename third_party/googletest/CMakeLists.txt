message(STATUS "Looking for GTest dependency")

find_package(PkgConfig REQUIRED)
pkg_check_modules(GTest IMPORTED_TARGET gtest>=1.10 gtest_main>=1.10 gmock>=1.10)

if (GTest_FOUND)
    message(STATUS "Looking for GTest dependency -- found")
    add_custom_target(googletest_dependency)
else ()
    message(STATUS "Looking for GTest dependency -- not found")

    include(ExternalProject)
    ExternalProject_Add(googletest_dependency
            URL
                https://github.com/google/googletest/archive/release-1.11.0.tar.gz
            URL_HASH
                MD5=e8a8df240b6938bb6384155d4c37d937
            DOWNLOAD_NO_PROGRESS
                1
            UPDATE_COMMAND
                ""
            LOG_CONFIGURE
                1
            LOG_BUILD
                1
            LOG_INSTALL
                1
            CMAKE_ARGS
                -DINSTALL_GTEST:BOOL=ON
                -DCMAKE_INSTALL_PREFIX:PATH=${DEPENDENCIES_INSTALL_PREFIX}/googletest_dependency
            )

    include(GNUInstallDirs)
    list(APPEND PREFIX_PATH "${DEPENDENCIES_INSTALL_PREFIX}/googletest_dependency")
    set(CMAKE_PREFIX_PATH ${CMAKE_PREFIX_PATH};${PREFIX_PATH}
            CACHE PATH "append googletest library into the search path"
            FORCE)
endif ()
