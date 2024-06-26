cmake_minimum_required( VERSION 3.16 FATAL_ERROR )

fenix_project( TARGET hooks PROJECT HOOKS )



###########################################################################################
#                                                                                         #
#                             Compilation include directories                             #
#                                                                                         #
###########################################################################################
include_directories( ${PROJECT_SOURCE_DIR}/api )
include_directories( ${TRACING_SOURCE_DIR}/api )
include_directories( ${BASE_SOURCE_DIR}/api )
include_directories( ${OSW_SOURCE_DIR}/api )



###########################################################################################
#                                                                                         #
#                                Linkage include directories                              #
#                                                                                         #
###########################################################################################
link_directories( ${TRACING_BINARY_DIR} )
link_directories( ${BASE_BINARY_DIR} )
link_directories( ${OSW_BINARY_DIR} )



###########################################################################################
#                                                                                         #
#                                   Compile definitions                                   #
#                                                                                         #
###########################################################################################



###########################################################################################
#                                                                                         #
#                                   Build source file list                                #
#                                                                                         #
###########################################################################################
find_files_by_ext( RECURSE FILES PROJECT_SOURCE_FILES LOCATION ${PROJECT_SOURCE_DIR} EXTENTIONS ${EXTENTIONS_CPP_SRC} )



###########################################################################################
#                                                                                         #
#                                      Debug messages                                     #
#                                                                                         #
###########################################################################################
print_variables( )



add_library(
      ${PROJECT_TARGET_NAME} SHARED
      ${PROJECT_SOURCE_FILES}
   )

target_link_libraries( ${PROJECT_TARGET_NAME} PUBLIC ${TRACING_TARGET_NAME} )
target_link_libraries( ${PROJECT_TARGET_NAME} PUBLIC ${BASE_TARGET_NAME} )
target_link_libraries( ${PROJECT_TARGET_NAME} PUBLIC ${OSW_TARGET_NAME} )

add_custom_target( "${PROJECT_TARGET_NAME}_documentation" ALL
      COMMENT "cmake ${PROJECT_TARGET_NAME}_documentation"
      DEPENDS ${PROJECT_GEN_PLANTUML_FILES}
   )

add_dependencies( ${PROJECT_TARGET_NAME} "${PROJECT_TARGET_NAME}_documentation" )

install( TARGETS ${PROJECT_TARGET_NAME} DESTINATION lib )
