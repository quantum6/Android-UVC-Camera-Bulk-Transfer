# For more information about using CMake with Android Studio, read the
# documentation: https://d.android.com/studio/projects/add-native-code.html

# Sets the minimum version of CMake required to build the native library.

cmake_minimum_required(VERSION 3.10.2)

# Declares and names the project.

project("UVC_Preview")


# Creates and names a library, sets it as either STATIC
# or SHARED, and provides the relative paths to its source code.
# You can define multiple libraries, and CMake builds them for you.
# Gradle automatically packages shared libraries with your APK.

file(GLOB_RECURSE SOURCE_C ${YOUR_PROJECT_SRC_DIR}/ *.cpp)
#file(GLOB_RECURSE SOURCE_H ${YOUR_PROJECT_HEADER_DIR}/libusb/libusb/libusb.h)

add_library( # Sets the name of the library.
        uvc_preview

        # Sets the library as a shared library.
        SHARED

        # Provides a relative path to your source file(s).
        UVCPreview.cpp
        #            ${SOURCE_H}
        )


#target_include_directories(uvc_preview PUBLIC ../../c/libusb/libusb)
#target_include_directories(uvc_preview PUBLIC ../../c/libuvc/include/libuvc)
#target_include_directories(uvc_preview PUBLIC ../../c/libUvc_Support)
#target_include_directories(uvc_preview PUBLIC ../../c/libyuv/include)


# Searches for a specified prebuilt library and stores the path as a
# variable. Because CMake includes system libraries in the search path by
# default, you only need to specify the name of the public NDK library
# you want to add. CMake verifies that the library exists before
# completing its build.

find_library( # Sets the name of the path variable.
              log-lib

              # Specifies the name of the NDK library that
              # you want CMake to locate.
              log )


# Specifies libraries CMake should link to your target library. You
# can link multiple libraries, such as libraries you define in this
# build script, prebuilt third-party libraries, or system libraries.


#set (LIBUVC_DIR ../../libuvc)
#add_subdirectory (${LIBUVC_DIR} ./uvc)
#include_directories (${LIBUVC_DIR}/include/libuvc)
#target_link_libraries(uvc_preview uvc)

target_compile_options(uvc_preview PRIVATE -DANDROID_NDK -DLOG_NDEBUG)

target_link_libraries(
        # Specifies the target library.
        uvc_preview
        #-ljvm
        -ldl
        -ljnigraphics
        -landroid
        # Links the target library to the log library
        # included in the NDK.
        ${log-lib}

)

set (LIBUVC_DIR ../../libuvc)
add_subdirectory (${LIBUVC_DIR} ./uvc)
include_directories (${LIBUVC_DIR}/include/libuvc)
include_directories (${LIBUVC_DIR}/include)
target_link_libraries(uvc_preview uvc)




