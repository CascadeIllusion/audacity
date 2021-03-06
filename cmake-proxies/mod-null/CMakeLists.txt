
set( TARGET mod-null )
set( TARGET_ROOT "${libsrc}/${TARGET}" )

message( STATUS "========== Configuring ${TARGET} ==========" )

def_vars()

add_library( ${TARGET} MODULE EXCLUDE_FROM_ALL )

list( APPEND SOURCES
   PRIVATE
      ${TARGET_ROOT}/ModNullCallback.cpp
      ${TARGET_ROOT}/ModNullCallback.h
)

list( APPEND INCLUDES
   PUBLIC
      ${TARGET_ROOT}
)

list( APPEND DEFINES
   PRIVATE
      # This is needed until the transition to cmake is complete and
      # the Windows pragmas are removed from ModNullCallback.cpp.
      # Without it, the wxWidgets "debug.h" will define __WXDEBUG__
      # which then causes this module to emit library pragmas for the
      # debug versions of wxWidgets...even if the build is for Release.
      wxDEBUG_LEVEL=0
)

list( APPEND LIBRARIES
   PRIVATE
      Audacity
      wxWidgets
)

set_target_property_all( ${TARGET} LIBRARY_OUTPUT_DIRECTORY "${_DEST}/modules" )
set_target_properties( ${TARGET}
   PROPERTIES
      PREFIX ""
      FOLDER "lib-src"
)

organize_source( "${TARGET_ROOT}" "" "${SOURCES}" )
target_sources( ${TARGET} PRIVATE ${SOURCES} )
target_compile_definitions( ${TARGET} PRIVATE ${DEFINES} )
target_include_directories( ${TARGET} PRIVATE ${INCLUDES} )
target_link_libraries( ${TARGET} PRIVATE ${LIBRARIES} )

