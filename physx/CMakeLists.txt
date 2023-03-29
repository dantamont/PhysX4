# This is not an intended PhysX use-case, but the road has been paved. 
# See: https://github.com/NVIDIAGameWorks/PhysX/issues/249
#set(PHYSX_VS_FOLDER_NAME ${DEPENDENCY_FOLDER}/PhysX) 
set(PHYSX_ROOT_DIR ${CMAKE_CURRENT_SOURCE_DIR})  #This is needed for physx/compiler/public/CMakeLists.txt
set(PHYSX_INCLUDE_DIRS ${PHYSX_ROOT_DIR}/include/ ${PHYSX_ROOT_DIR}/../pxshared/include/ )

# Has to match the TARGET_BUILD_PLATFORM in PHYSX_ROOT_DIR/physx/buildtools/cmake_generate_projects.py
if(WIN32)
    set(TARGET_BUILD_PLATFORM "windows" CACHE INTERNAL "") 
elseif(UNIX AND NOT APPLE)
    set(TARGET_BUILD_PLATFORM "linux" CACHE INTERNAL "")
elseif(ANDROID)
    set(TARGET_BUILD_PLATFORM "android" CACHE INTERNAL "")
else()
    if(IOS)
        set(TARGET_BUILD_PLATFORM "ios" CACHE INTERNAL "")
    elseif(APPLE)
        set(TARGET_BUILD_PLATFORM "mac" CACHE INTERNAL "")
    else()
        message( FATAL_ERROR "Unsupported platform ${CMAKE_SYSTEM_NAME} for PhysX." )
    endif()
endif()

set(PX_BUILDSNIPPETS OFF CACHE BOOL "Do not generate the snippets")
set(PX_BUILDPUBLICSAMPLES OFF CACHE BOOL "Do not generate the samples projects")
set(PX_GENERATE_STATIC_LIBRARIES ON CACHE BOOL "Generate static libraries")
set(PX_FLOAT_POINT_PRECISE_MATH OFF CACHE BOOL "Float point precise math. If off, prefers speed")
set(NV_USE_STATIC_WINCRT OFF CACHE BOOL "Use the statically linked windows CRT")
set(NV_USE_DEBUG_WINCRT ON CACHE BOOL "Use the debug version of the CRT")
set(PXSHARED_PATH ${PHYSX_ROOT_DIR}/../pxshared)
set(PXSHARED_INSTALL_PREFIX ${CMAKE_INSTALL_PREFIX})
set(CMAKEMODULES_VERSION "1.27")
set(CMAKEMODULES_PATH ${PHYSX_ROOT_DIR}/../externals/cmakemodules)
set(PX_OUTPUT_LIB_DIR ${CMAKE_CURRENT_BINARY_DIR}/physx CACHE INTERNAL "")
set(PX_OUTPUT_BIN_DIR ${CMAKE_CURRENT_BINARY_DIR}/physx CACHE INTERNAL "")

# Actually add the PhysX CMakeLists.txt
#add_subdirectory(${PHYSX_ROOT_DIR}/compiler/public extern/physx)
add_subdirectory(${PHYSX_ROOT_DIR}/compiler/public)
