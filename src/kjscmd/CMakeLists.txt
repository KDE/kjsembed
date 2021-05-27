project(kjsembed-kjscmd)

# This is a developer tool, not intended for normal user installs
add_executable(kjscmd5)

target_sources(kjscmd5 PRIVATE
    kjscmd.cpp
    kjscmd.qrc
)

target_link_libraries(kjscmd5
    Qt5::Widgets # QApplication::lastWindowClosed
    KF5::JsEmbed
)

install(TARGETS kjscmd5 ${KF5_INSTALL_TARGETS_DEFAULT_ARGS})
