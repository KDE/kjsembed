add_library(KF5JsEmbed)
add_library(KF5::JsEmbed ALIAS KF5JsEmbed)

target_sources(KF5JsEmbed PRIVATE
   kjseglobal.cpp
   binding_support.cpp
   static_binding.cpp
   variant_binding.cpp
   object_binding.cpp
   builtins.cpp
   fileio.cpp
   jseventmapper.cpp
   eventproxy.cpp
   slotproxy.cpp
   jseventutils.cpp
   qobject_binding.cpp
   kjsembed.cpp
   value_binding.cpp
   iosupport.cpp
   qwidget_binding.cpp
   qaction_binding.cpp
   qlayout_binding.cpp
   qpainter_binding.cpp
   settings.cpp
   svg_binding.cpp
   filedialog_binding.cpp
   application.cpp
   color.cpp dom.cpp font.cpp image.cpp pen.cpp
   pixmap.cpp point.cpp rect.cpp size.cpp url.cpp
   brush.cpp
   QBrush_bind.cpp
   quiloader_binding.cpp
)

generate_export_header(KF5JsEmbed BASE_NAME KJsEmbed)
target_include_directories(KF5JsEmbed PUBLIC "$<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/..;${CMAKE_CURRENT_BINARY_DIR}/..>")
target_include_directories(KF5JsEmbed INTERFACE "$<INSTALL_INTERFACE:${KDE_INSTALL_INCLUDEDIR_KF5}/KJsEmbed>"
                                                "$<INSTALL_INTERFACE:${KDE_INSTALL_INCLUDEDIR_KF5}/KJsEmbed/kjsembed>")
ecm_generate_headers(KJsEmbed_CamelCase_HEADERS
  HEADER_NAMES
    KJsEmbed

    PREFIX KJsEmbed
    REQUIRED_HEADERS KJsEmbed_HEADERS
)

target_link_libraries(KF5JsEmbed
    PUBLIC  KF5::I18n KF5::JS
    PRIVATE
        #all these are required for its respective bindings
        Qt5::Widgets
        Qt5::UiTools
        Qt5::Xml
        Qt5::Svg
)

set_target_properties(KF5JsEmbed PROPERTIES VERSION   "${KJSEMBED_VERSION}"
                                            SOVERSION "${KJSEMBED_SOVERSION}"
                                            EXPORT_NAME JsEmbed
)

install(TARGETS KF5JsEmbed EXPORT KF5JsEmbedTargets ${KF5_INSTALL_TARGETS_DEFAULT_ARGS})

install(FILES
 ${CMAKE_CURRENT_BINARY_DIR}/kjsembed_export.h
 ${KJsEmbed_HEADERS}

 pointer.h
 kjseglobal.h
 static_binding.h
 binding_support.h
 variant_binding.h

 DESTINATION ${KDE_INSTALL_INCLUDEDIR_KF5}/KJsEmbed/kjsembed COMPONENT Devel)

install(FILES ${KJsEmbed_CamelCase_HEADERS} DESTINATION ${KDE_INSTALL_INCLUDEDIR_KF5}/KJsEmbed/KJsEmbed COMPONENT Devel)




include(ECMGeneratePriFile)
ecm_generate_pri_file(BASE_NAME KJsEmbed LIB_NAME KF5JsEmbed DEPS "KI18n KJS" FILENAME_VAR PRI_FILENAME INCLUDE_INSTALL_DIR ${KDE_INSTALL_INCLUDEDIR_KF5}/KJsEmbed)
install(FILES ${PRI_FILENAME} DESTINATION ${ECM_MKSPECS_INSTALL_DIR})
