include_directories(
    ${CMAKE_SOURCE_DIR}/src
    ${CMAKE_CURRENT_BINARY_DIR}
    ${Boost_INCLUDE_DIRS}
    ${COIN3D_INCLUDE_DIRS}
    ${PYTHON_INCLUDE_DIRS}
    ${ZLIB_INCLUDE_DIR}
)

link_directories(${OCC_LIBRARY_DIR})

set(InspectionGui_LIBS
    Inspection
    FreeCADGui
)

set(Dialogs_UIC_SRCS
    VisualInspection.ui
)

if(BUILD_QT5)
    qt5_add_resources(Inspection_QRC_SRCS Resources/Inspection.qrc)
    qt5_wrap_ui(Dialogs_UIC_HDRS ${Dialogs_UIC_SRCS})
else()
    qt4_add_resources(Inspection_QRC_SRCS Resources/Inspection.qrc)
    qt4_wrap_ui(Dialogs_UIC_HDRS ${Dialogs_UIC_SRCS})
endif()

SET(Dialogs_SRCS
    ${Dialogs_UIC_HDRS}
    VisualInspection.cpp
    VisualInspection.h
)
SOURCE_GROUP("Dialogs" FILES ${Dialogs_SRCS})

SET(InspectionGui_SRCS
    ${Inspection_QRC_SRCS}
    ${Dialogs_SRCS}
    AppInspectionGui.cpp
    Command.cpp
    PreCompiled.cpp
    PreCompiled.h
    ViewProviderInspection.cpp
    ViewProviderInspection.h
    Workbench.cpp
    Workbench.h
)

set(InspectionGui_Scripts
    ../InitGui.py
)

SET(InspectionGuiIcon_SVG
    Resources/icons/InspectionWorkbench.svg
)

add_library(InspectionGui SHARED ${InspectionGui_SRCS} ${InspectionGui_Scripts} ${InspectionGuiIcon_SVG})
target_link_libraries(InspectionGui ${InspectionGui_LIBS})


fc_target_copy_resource_flat(InspectionGui
    ${CMAKE_CURRENT_SOURCE_DIR}
    ${CMAKE_BINARY_DIR}/Mod/Inspection
    ${InspectionGui_Scripts}
)

SET_BIN_DIR(InspectionGui InspectionGui /Mod/Inspection)
SET_PYTHON_PREFIX_SUFFIX(InspectionGui)

fc_copy_sources(InspectionGui "${CMAKE_BINARY_DIR}/${CMAKE_INSTALL_DATADIR}/Mod/Inspection" ${InspectionGuiIcon_SVG})

INSTALL(TARGETS InspectionGui DESTINATION ${CMAKE_INSTALL_LIBDIR})
INSTALL(FILES ${InspectionGuiIcon_SVG} DESTINATION "${CMAKE_INSTALL_DATADIR}/Mod/Inspection/Resources/icons")
