#set(CMAKE_BUILD_TYPE Debug)

if(NOT ENABLE_GUI)
  return()
endif()

option(BUILD_AGX_BODYEXTENSION_PLUGIN "Building a AGXBodyExtensionPlugin" OFF)
if(NOT BUILD_AGX_BODYEXTENSION_PLUGIN)
  return()
endif()

set(target CnoidAGXBodyExtensionPlugin)

set(sources
  AGXBodyExtensionPlugin.cpp
  AGXBodyExtensionSample.cpp
  AGXBreakableJoint.cpp
  AGXMagneticJoint.cpp
  AGXWire.cpp
)

set(headers
)

make_gettext_mofiles(${target} mofiles)
add_cnoid_plugin(${target} SHARED ${sources} ${headers})

AGX_TARGET_LINK_LIBRARIES(${target} optimized ${AGX_LIBRARIES_RELEASE})
if(AGX_LIBRARIES_DEBUG)
  AGX_TARGET_LINK_LIBRARIES(${target} debug ${AGX_LIBRARIES_DEBUG} )
else()
  AGX_TARGET_LINK_LIBRARIES(${target} debug ${AGX_LIBRARIES_RELEASE} )
endif()

target_link_libraries(${target} CnoidAGXDynamicsPlugin)

get_target_property(compile_flags ${target} COMPILE_FLAGS)
if(NOT compile_flags)
  set(compile_flags "")
endif()
set_target_properties(${target} PROPERTIES COMPILE_FLAGS "${compile_flags} -msse3")

apply_common_setting_for_plugin(${target})

