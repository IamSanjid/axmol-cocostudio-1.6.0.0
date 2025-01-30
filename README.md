# cocostudio 1.4\~1.6 or 0.1.4\~0.1.6
Cocos Studio Legacy versions 1.4.0.0\~1.6.0.0 or 0.1.4\~0.1.6 support for axmol

# How to use
Add this to your project's CMakeFile before it calls `ax_setup_app_props`
```zsh
set(AX_ENABLE_EXT_COCOSTUDIO OFF CACHE BOOL "Build extension cocostudio" FORCE)

# Clone legacy-cocostudio
include(FetchContent)
set(LEGACY_COCOSTUDIO "axmol-legacy-cocostudio")
FetchContent_Declare(
    cocostudio
    GIT_REPOSITORY https://github.com/IamSanjid/axmol-legacy-cocostudio.git
    GIT_SHALLOW ON
    SOURCE_DIR "${CMAKE_CURRENT_LIST_DIR}/${LEGACY_COCOSTUDIO}"
)
FetchContent_MakeAvailable(cocostudio)

# let this cocstudio use spine
if(AX_ENABLE_EXT_SPINE) # this check can be removed if you're using custom spine
    set(SPINE_DIR "${_AX_ROOT}/extensions/spine")
    target_compile_definitions(cocostudio PRIVATE AX_ENABLE_EXT_SPINE=1)
    if(BUILD_SHARED_LIBS)
        target_link_libraries(cocostudio spine particle3d)
    else()
        target_include_directories(cocostudio
            PUBLIC ${SPINE_DIR}/runtime/include
            PUBLIC ${SPINE_DIR}/src
            PUBLIC ${_AX_ROOT}/extensions/Particle3D/src
        )
    endif()
endif()

# let cocstudio use dragon bones
if(AX_ENABLE_EXT_DRAGONBONES)
    set(DRAGONBONES_DIR "${_AX_ROOT}/extensions/DragonBones")
    target_compile_definitions(cocostudio PRIVATE AX_ENABLE_EXT_DRAGONBONES=1)
    if(BUILD_SHARED_LIBS)
        target_link_libraries(cocostudio DragonBones)
    else()
        target_include_directories(cocostudio PUBLIC ${DRAGONBONES_DIR}/src)
    endif()
endif()

# link cocostudio
if(BUILD_SHARED_LIBS)
    target_link_libraries(${APP_NAME} cocostudio)
else()
    target_include_directories(${APP_NAME} PUBLIC ${LEGACY_COCOSTUDIO}/src)
endif()
```


# Credits
Discord: rh.dev for figuring out how 1.6.0.0/0.1.6.0 really works this way<br>
Cocos2dx-2<br>
Axmol
