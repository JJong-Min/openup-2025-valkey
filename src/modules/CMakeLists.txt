# Build modules
list(APPEND MODULES_LIST "helloacl")
list(APPEND MODULES_LIST "helloblock")
list(APPEND MODULES_LIST "hellocluster")
list(APPEND MODULES_LIST "hellodict")
list(APPEND MODULES_LIST "hellohook")
list(APPEND MODULES_LIST "hellotimer")
list(APPEND MODULES_LIST "hellotype")
list(APPEND MODULES_LIST "helloworld")

foreach (MODULE_NAME ${MODULES_LIST})
    message(STATUS "Building module: ${MODULE_NAME}")
    add_library(${MODULE_NAME} SHARED "${CMAKE_CURRENT_LIST_DIR}/${MODULE_NAME}.c")
    target_include_directories(${MODULE_NAME} PRIVATE "${CMAKE_SOURCE_DIR}/src")
    set_target_properties(${MODULE_NAME} PROPERTIES PREFIX "")
    valkey_install_bin(${MODULE_NAME})
    if (APPLE)
        # Some symbols can only be resolved during runtime (they exist in the executable)
        target_link_options(${MODULE_NAME} PRIVATE -undefined dynamic_lookup)
    endif ()
endforeach ()
