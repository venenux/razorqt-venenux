cmake_minimum_required(VERSION 2.6)

set(PLUGIN "helloworld")


set(HW_HEADERS
  helloworld.h
)

set(HW_SOURCES
  helloworld.cpp
)

set(HW_MOCS

)

#*******************************************
set(PROJECT "${PLUGIN}")

project(${PROJECT})

set(PROGRAM "razor-desktop")

set(PROG_SHARE_DIR ${CMAKE_INSTALL_PREFIX}/share/razor/${PROGRAM})
set(PLUGIN_SHARE_DIR ${PROG_SHARE_DIR}/${PLUGIN})

include_directories (
    ${CMAKE_CURRENT_SOURCE_DIR}/../../libraries
    ${CMAKE_CURRENT_SOURCE_DIR}/../
    ${CMAKE_CURRENT_SOURCE_DIR}
)

qt4_wrap_cpp(HW_MOC_SOURCES ${HW_MOCS})
qt4_add_resources(QRC_SOURCES ${RESOURCES})

# Translations **********************************
include(RazorTranslate)

razor_translate_ts(QM_FILES 
    SOURCES
        ${HW_HEADERS}
        ${HW_SOURCES} 
    INSTALLATION_DIR        
        ${PLUGIN_SHARE_DIR}/${NAME}
)

file (GLOB DESKTOP_FILES_IN resources/*.desktop.in)
razor_translate_desktop(DESKTOP_FILES
    SOURCES
        ${DESKTOP_FILES_IN}
)
#************************************************

add_library(${PROJECT} SHARED ${HW_HEADERS} ${HW_SOURCES} ${HW_MOC_SOURCES} ${QM_FILES} ${QRC_SOURCES} ${DESKTOP_FILES})
target_link_libraries(${PROJECT} ${QT_QTCORE_LIBRARY} ${QT_QTGUI_LIBRARY} desktop-razor)
add_dependencies(${PROJECT} razorqt desktop-razor)

install(TARGETS ${PROJECT} DESTINATION ${DESKTOP_PLUGIN_DIR})
install(FILES ${DESKTOP_FILES} DESTINATION share/razor/razor-desktop/)
