add_executable(kjsconsole)

target_sources(kjsconsole PRIVATE
   main.cpp
   console.cpp
   kjs_object_model.cpp
   numberedtextview.cpp

   console.qrc
)

ki18n_wrap_ui(kjsconsole jsconsole.ui)
target_link_libraries(kjsconsole KF5::JsEmbed Qt5::Widgets)

install(TARGETS kjsconsole DESTINATION ${KDE_INSTALL_BINDIR} )
