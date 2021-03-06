project(lxqt-config-monitor)

find_package(Qt5Svg REQUIRED)

set(H_FILES
    main.h
    monitorsettingsdialog.h
    monitor.h
    monitorwidget.h
    xrandr.h
    monitorpicture.h
    quickoptions.h
    savesettings.h
)

set(CPP_FILES
    main.cpp
    monitorsettingsdialog.cpp
    xrandr.cpp
    monitor.cpp
    monitorwidget.cpp
    timeoutdialog.cpp
    monitorpicture.cpp
    quickoptions.cpp
    savesettings.cpp
)

set(UI_FILES
    mainwindow.ui
    monitorwidget.ui
    timeoutdialog.ui
    monitorpicture.ui
    quickoptions.ui
    savesettings.ui
)

set(ICON_FILES
	icons/monitor.svg
	icons/extended.svg
	icons/monitor1onmonitor2ff.svg
	icons/unified.svg
	icons/monitor1offmonitor2on.svg
)

# Config file
configure_file ( configure.in configure.h )

# Translations **********************************
lxqt_translate_ts(QM_FILES
    UPDATE_TRANSLATIONS
        ${UPDATE_TRANSLATIONS}
    SOURCES
        ${H_FILES}
        ${CPP_FILES}
        ${UI_FILES}
    INSTALL_DIR
        "${LXQT_TRANSLATIONS_DIR}/${PROJECT_NAME}"
)

lxqt_app_translation_loader(QM_LOADER ${PROJECT_NAME})
lxqt_translate_desktop(DESKTOP_FILES SOURCES ${PROJECT_NAME}.desktop.in)

#************************************************

add_executable(${PROJECT_NAME}
    ${CPP_FILES}
    ${RESOURCES}
    ${QRC_SOURCES}
    ${QM_FILES}
    ${DESKTOP_FILES}
    ${QM_LOADER}
)

target_link_libraries(${PROJECT_NAME}
    Qt5::Widgets
    Qt5::X11Extras
    Qt5::Svg
    lxqt
)

install(TARGETS
    ${PROJECT_NAME}
    RUNTIME DESTINATION "${CMAKE_INSTALL_BINDIR}"
    COMPONENT Runtime
)
install(FILES
    ${DESKTOP_FILES}
    DESTINATION "${CMAKE_INSTALL_DATAROOTDIR}/applications"
    COMPONENT Runtime
)
install(FILES
    ${ICON_FILES}
    DESTINATION "${CMAKE_INSTALL_DATAROOTDIR}/lxqt/icons"
    COMPONENT Runtime
)
