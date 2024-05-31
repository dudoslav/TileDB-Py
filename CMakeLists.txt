cmake_minimum_required(VERSION 3.15...3.26)

project(${SKBUILD_PROJECT_NAME})

set(PYBIND11_NEWPYTHON ON)

find_package(
  Python
  COMPONENTS Interpreter Development.Module NumPy
  REQUIRE
)

find_package(pybind11 CONFIG REQUIRED)

# In the future we should use vcpkg
set(TileDB_DIR $ENV{TILEDB_PATH})
find_package(TileDB QUIET)
if (NOT TileDB_FOUND)
    message(STATUS "TileDB library not found, downloading...")
    include(cmake/DownloadTileDB.cmake)
    fetch_prebuilt_tiledb(
            VERSION 2.24.0-rc0
            RELLIST_HASH SHA256=83fb0549fb58b92232b5a546bd926faae487bc7d0a365fa560bfb60e0d59ff5d
    )
    find_package(TileDB REQUIRED)
    set(TILEDB_DOWNLOADED ON)
endif()

add_subdirectory(tiledb)