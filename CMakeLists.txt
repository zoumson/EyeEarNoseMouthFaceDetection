cmake_minimum_required (VERSION 3.0)

PROJECT(HumanBodyPartsDetection)

#Set the compiler
set(CMAKE_CXX_STANDARD 17)
set(CMAKE_BUILD_TYPE Release)
set( EXECUTABLE_OUTPUT_PATH ${PROJECT_SOURCE_DIR}/bin)
#set( LIBRARY_OUTPUT_PATH ${PROJECT_SOURCE_DIR}/lib)


#Set ccache (for speeding up)
find_program(CCACHE_FOUND ccache) 
if(CCACHE_FOUND) 
    set_property(GLOBAL PROPERTY RULE_LAUNCH_COMPILE ccache) 
    set_property(GLOBAL PROPERTY RULE_LAUNCH_LINK ccache) 
endif(CCACHE_FOUND) 
set (CMAKE_CXX_FLAGS "-DPCL_ONLY_CORE_POINT_TYPES=ON -DNO_EXPLICIT_INSTANTIATIONS") 


IF(EXISTS ${CMAKE_BINARY_DIR}/conanbuildinfo.cmake)
    include(${CMAKE_BINARY_DIR}/conanbuildinfo.cmake)
    conan_basic_setup()
ENDIF()

# Requires OpenCV
FIND_PACKAGE( OpenCV 4.0.0 REQUIRED )
MESSAGE("OpenCV version : ${OpenCV_VERSION}")

include_directories(${OpenCV_INCLUDE_DIRS})
link_directories(${OpenCV_LIB_DIR})

ADD_EXECUTABLE( earDetector ${PROJECT_SOURCE_DIR}/src/earDetector.cpp )
TARGET_LINK_LIBRARIES( earDetector ${OpenCV_LIBS} )

ADD_EXECUTABLE( overlayFacemask ${PROJECT_SOURCE_DIR}/src/overlayFacemask.cpp )
TARGET_LINK_LIBRARIES( overlayFacemask ${OpenCV_LIBS} )

ADD_EXECUTABLE( overlayMoustache ${PROJECT_SOURCE_DIR}/src/overlayMoustache.cpp )
TARGET_LINK_LIBRARIES( overlayMoustache ${OpenCV_LIBS} )

ADD_EXECUTABLE( overlayNose ${PROJECT_SOURCE_DIR}/src/overlayNose.cpp )
TARGET_LINK_LIBRARIES( overlayNose ${OpenCV_LIBS} )

ADD_EXECUTABLE( overlaySunglasses ${PROJECT_SOURCE_DIR}/src/overlaySunglasses.cpp )
TARGET_LINK_LIBRARIES( overlaySunglasses ${OpenCV_LIBS} )