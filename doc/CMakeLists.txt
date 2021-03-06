########################################
########################################
##
##  Copyright (C) 2014 Korovasoft, Inc.
##
##  File:
##      \file CMakeLists.txt
##
##  Description:
##      \brief Doc-level CMakeLists.txt
##
##  Author:
##      \author J. Caleb Wherry
##
########################################
########################################


#
# Compile doxygen documentation:
#
find_package(Doxygen)

if (DOXYGEN_FOUND)

  configure_file(${PROJECT_DOC_DIR}/Doxyfile.in ${PROJECT_BINARY_DIR}/Doxyfile @ONLY IMMEDIATE)
  add_custom_target( doc
    ${DOXYGEN_EXECUTABLE} ${PROJECT_BINARY_DIR}/Doxyfile
    WORKING_DIRECTORY ${PROJECT_BINARY_DIR}
    COMMENT "Generating API documentation with Doxygen" VERBATIM
  )
  install(DIRECTORY ${PROJECT_BINARY_DIR}/doc/html DESTINATION doc/garth)

else ()

  message(FATAL_ERROR "Doxygen comments could not be created because Doxygen was not found on this system; aborting!")

endif ()


#
# Add subdirectories:
#
add_subdirectory(papers)
