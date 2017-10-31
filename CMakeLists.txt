cmake_minimum_required(VERSION 3.0)

set(KF5_VERSION "5.40.0") # handled by release scripts
set(KF5_DEP_VERSION "5.39.0") # handled by release scripts
project(KJsEmbed VERSION ${KF5_VERSION})

include(FeatureSummary)
find_package(ECM 5.39.0  NO_MODULE)
set_package_properties(ECM PROPERTIES TYPE REQUIRED DESCRIPTION "Extra CMake Modules." URL "https://projects.kde.org/projects/kdesupport/extra-cmake-modules")
feature_summary(WHAT REQUIRED_PACKAGES_NOT_FOUND FATAL_ON_MISSING_REQUIRED_PACKAGES)

set(CMAKE_MODULE_PATH ${ECM_MODULE_PATH} ${ECM_KDE_MODULE_DIR})

set(REQUIRED_QT_VERSION 5.7.0)
find_package(Qt5 "${REQUIRED_QT_VERSION}" CONFIG REQUIRED UiTools Widgets Xml Svg)
include(KDEInstallDirs)
include(KDEFrameworkCompilerSettings NO_POLICY_SCOPE)
include(KDECMakeSettings)

include(GenerateExportHeader)
include(CMakePackageConfigHelpers)
include(ECMSetupVersion)
include(ECMGenerateHeaders)

ecm_setup_version(PROJECT VARIABLE_PREFIX KJSEMBED
   #VERSION_HEADER "${CMAKE_CURRENT_BINARY_DIR}/kjsembed_version.h"
                        PACKAGE_VERSION_FILE "${CMAKE_CURRENT_BINARY_DIR}/KF5JsEmbedConfigVersion.cmake"
                        SOVERSION 5)

if("${CMAKE_BINARY_DIR}" STREQUAL "${CMAKE_CURRENT_BINARY_DIR}")
  find_package(KF5JS ${KF5_DEP_VERSION} REQUIRED)
  find_package(KF5I18n ${KF5_DEP_VERSION} REQUIRED)
  find_package(KF5DocTools ${KF5_DEP_VERSION} REQUIRED)
else()
    set(_kdeBootStrapping TRUE)
    include(${CMAKE_SOURCE_DIR}/tier2/kdoctools/KF5DocToolsMacros.cmake)
    set(KDOCTOOLS_CUSTOMIZATION_DIR "${KDocTools_BINARY_DIR}/src/customization/")
endif()

add_definitions(-DQT_DISABLE_DEPRECATED_BEFORE=0)
remove_definitions(-DQT_NO_CAST_FROM_ASCII)
remove_definitions(-DQT_NO_CAST_FROM_BYTEARRAY)

add_definitions(-DTRANSLATION_DOMAIN=\"kjsembed5\")
if (IS_DIRECTORY "${CMAKE_CURRENT_SOURCE_DIR}/po")
    ki18n_install(po)
    kdoctools_install(po)
endif()
add_subdirectory(docs)
add_subdirectory(src)
add_subdirectory(examples/kjsconsole)

# create a Config.cmake and a ConfigVersion.cmake file and install them
set(CMAKECONFIG_INSTALL_DIR "${KDE_INSTALL_CMAKEPACKAGEDIR}/KF5JsEmbed")

include(CMakePackageConfigHelpers)

configure_package_config_file(
  "${CMAKE_CURRENT_SOURCE_DIR}/KF5JsEmbedConfig.cmake.in"
  "${CMAKE_CURRENT_BINARY_DIR}/KF5JsEmbedConfig.cmake"
  INSTALL_DESTINATION  ${CMAKECONFIG_INSTALL_DIR}
)

install(FILES
  "${CMAKE_CURRENT_BINARY_DIR}/KF5JsEmbedConfig.cmake"
  "${CMAKE_CURRENT_BINARY_DIR}/KF5JsEmbedConfigVersion.cmake"
  DESTINATION "${CMAKECONFIG_INSTALL_DIR}"
  COMPONENT Devel
)

install(EXPORT KF5JsEmbedTargets DESTINATION "${CMAKECONFIG_INSTALL_DIR}" FILE KF5JsEmbedTargets.cmake NAMESPACE KF5:: )

#install(FILES
#${CMAKE_CURRENT_BINARY_DIR}/kjsembed_version.h
#DESTINATION ${KDE_INSTALL_INCLUDEDIR_KF5} COMPONENT Devel
#)

if ("${CMAKE_BINARY_DIR}" STREQUAL "${CMAKE_CURRENT_BINARY_DIR}")
  feature_summary(WHAT ALL   FATAL_ON_MISSING_REQUIRED_PACKAGES)
endif()
