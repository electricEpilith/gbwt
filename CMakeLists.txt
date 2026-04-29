cmake_minimum_required(VERSION 3.22)

project(gbwt
    LANGUAGES CXX
)

include(FetchContent)
include(GNUInstallDirs)

find_package(OpenMP REQUIRED)
find_package(Threads REQUIRED)
find_package(PkgConfig)
pkg_check_modules(ZSTD    REQUIRED IMPORTED_TARGET libzstd)

FetchContent_Declare(sdsl-lite
    GIT_REPOSITORY https://github.com/vgteam/sdsl-lite.git
    GIT_TAG        vg-cmake-hublabel
    EXCLUDE_FROM_ALL
)
FetchContent_MakeAvailable(sdsl-lite)

add_library(gbwt STATIC
    src/algorithms.cpp
    src/bwtmerge.cpp
    src/cached_gbwt.cpp
    src/dynamic_gbwt.cpp
    src/fast_locate.cpp
    src/files.cpp
    src/gbwt.cpp
    src/internal.cpp
    src/metadata.cpp
    src/support.cpp
    src/test.cpp
    src/utils.cpp
    src/variants.cpp
)
target_include_directories(gbwt PUBLIC include)
target_link_libraries(gbwt PRIVATE
    OpenMP::OpenMP_CXX Threads::Threads PkgConfig::ZSTD
    sdsl
)