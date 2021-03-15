## plug-ins need to disable fortification to ensure syscalls are intercepted
#add_definitions(-D_GNU_SOURCE)
add_definitions(-U_FORTIFY_SOURCE)

add_cflags(-fno-inline)
add_cflags(-fno-strict-aliasing)

add_cflags(-Wno-unknown-attributes)
add_cflags(-Wno-unused-command-line-argument)
add_cflags(-Wno-unknown-warning-option)

message(STATUS "CMAKE_C_FLAGS = ${CMAKE_C_FLAGS}")

## check env to see if we should allow interposition ion EVP_Cipher
if(SHADOW_ENABLE_EVPCIPHER STREQUAL ON)
    add_definitions(-DSHADOW_ENABLE_EVPCIPHER)
    message(STATUS "Added definition SHADOW_ENABLE_EVPCIPHER")
endif(SHADOW_ENABLE_EVPCIPHER STREQUAL ON)

## flags
add_cflags(-std=gnu99)
add_cflags(-fPIC)

## preload library for intercepting functions
add_library(shadow-preload-tor SHARED shadowtor-preload.c)
install(TARGETS shadow-preload-tor DESTINATION lib)

if(SHADOW_ENABLE_TORFLOW STREQUAL ON)
    add_subdirectory(torflow)
endif()
