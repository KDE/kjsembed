project(kjsembed-kjscmd)

set(kjscmd_SRCS kjscmd.cpp )
qt_add_resources(kjscmd_SRCS kjscmd.qrc)

# This is a developer tool, not intended for normal user installs
add_executable(kjscmd5 ${kjscmd_SRCS})
target_link_libraries(kjscmd5
    Qt5::Widgets # QApplication::lastWindowClosed
    KF5::JsEmbed
)

install(TARGETS kjscmd5 ${KF5_INSTALL_TARGETS_DEFAULT_ARGS})
