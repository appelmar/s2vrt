cmake_minimum_required(VERSION 3.5)
project(s2vrt LANGUAGES CXX VERSION 0.0.1)


string(TIMESTAMP build_date "%Y%m%d")
string(TIMESTAMP build_time "%H%M%S")

if (NOT CMAKE_BUILD_TYPE)
    message(STATUS "No build type specified, using Debug")
    set(CMAKE_BUILD_TYPE Debug)
endif (NOT CMAKE_BUILD_TYPE)


set(CMAKE_CXX_STANDARD 11)


## find GDAL
find_path(gdal_INCLUDEDIR "gdal.h" HINTS $ENV{GDAL_INCLUDEDIR} PATH_SUFFIXES gdal)
if (gdal_INCLUDEDIR-NOTFOUND)
    message(FATAL_ERROR "GDAL headers not found")
endif (gdal_INCLUDEDIR-NOTFOUND)

find_library(gdal_LIBRARY NAMES gdal gdal_i HINTS $ENV{GDAL_LIBDIR})
if (NOT gdal_LIBRARY)
    message(FATAL_ERROR "GDAL library not found!")
else (NOT gdal_LIBRARY)
    message(STATUS "GDAL library found at ${gdal_LIBRARY}")
endif (NOT gdal_LIBRARY)


##  find PROJ
find_path(proj_INCLUDEDIR "proj_api.h" HINTS $ENV{PROJ_INCLUDEDIR})
if (proj_INCLUDEDIR-NOTFOUND)
    message(FATAL_ERROR "PROJ headers not found")
endif (proj_INCLUDEDIR-NOTFOUND)

find_library(proj_LIBRARY NAMES proj proj_i HINTS $ENV{PROJ_LIBDIR} )
if (NOT proj_LIBRARY)
    message(FATAL_ERROR "PROJ library not found!")
else (NOT proj_LIBRARY)
    message(STATUS "PROJ library found at ${proj_LIBRARY}")
endif (NOT proj_LIBRARY)



## find boost
find_package(Boost COMPONENTS program_options filesystem date_time REQUIRED )
if (Boost_FOUND)
    message(STATUS "Boost headers found at ${Boost_INCLUDE_DIRS}")

else (Boost_FOUND)
    message(FATAL_ERROR "Boost headers not found")
endif (Boost_FOUND)






include_directories (${gdal_INCLUDEDIR} ${proj_INCLUDEDIR} ${Boost_INCLUDE_DIRS} ${geos_INCLUDEDIR})

set(SOURCE_FILES main.cpp)
add_executable(s2vrt ${SOURCE_FILES})
target_link_libraries (s2vrt ${proj_LIBRARY} ${gdal_LIBRARY} ${Boost_LIBRARIES})


