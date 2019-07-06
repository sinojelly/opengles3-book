cmake_minimum_required(VERSION 3.6)

set ( common_src Source/esShader.c
                 Source/esShapes.c
                 Source/esTransform.c
                 Source/esUtil.c )

include_directories(Include)
find_library( OPENGLES3_LIBRARY GLESv2 "OpenGL ES v3.0 library")
find_library( EGL_LIBRARY EGL "EGL 1.4 library" )
				 
# Win32 Platform files
if(WIN32)
    set( common_platform_src Source/Win32/esUtil_win32.c )
    add_library( Common STATIC ${common_src} ${common_platform_src} )
    target_link_libraries( Common ${OPENGLES3_LIBRARY} ${EGL_LIBRARY} )
elseif(ANDROID)
    set( common_platform_src Source/Android/esUtil_Android.c )
	include_directories("D:/Tools/android_sdk/ndk-bundle/sources/android/native_app_glue")
	add_library( Common SHARED ${common_src} ${common_platform_src}
	     D:/Tools/android_sdk/ndk-bundle/sources/android/native_app_glue/android_native_app_glue.c
	     ../Chapter_2/Hello_Triangle/Hello_Triangle.c)
    target_link_libraries( Common ${OPENGLES3_LIBRARY} ${EGL_LIBRARY}
	     android
         log)
else()
    find_package(X11)
    find_library(M_LIB m)
    set( common_platform_src Source/LinuxX11/esUtil_X11.c )
    add_library( Common STATIC ${common_src} ${common_platform_src} )
    target_link_libraries( Common ${OPENGLES3_LIBRARY} ${EGL_LIBRARY} ${X11_LIBRARIES} ${M_LIB} )
endif()

             


