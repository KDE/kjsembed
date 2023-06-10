cmake_minimum_required(VERSION 3.16)

set(KF_VERSION "5.108.0") # handled by release scripts
set(KF_DEP_VERSION "5.107.0") # handled by release scripts
project(KJsEmbed VERSION ${KF_VERSION})

include(FeatureSummary)
find_package(ECM 5.107.0  NO_MODULE)
set_package_properties(ECM PROPERTIES TYPE REQUIRED DESCRIPTION "Extra CMake Modules." URL "https://commits.kde.org/extra-cmake-modules")
feature_summary(WHAT REQUIRED_PACKAGES_NOT_FOUND FATAL_ON_MISSING_REQUIRED_PACKAGES)

set(CMAKE_MODULE_PATH ${ECM_MODULE_PATH})

include(KDEInstallDirs)
include(KDECMakeSettings)
include(KDEFrameworkCompilerSettings NO_POLICY_SCOPE)

include(ECMSetupVersion)
include(ECMGenerateHeaders)
include(ECMDeprecationSettings)
include(GenerateExportHeader)
include(CMakePackageConfigHelpers)
include(ECMMarkNonGuiExecutable)

set(REQUIRED_QT_VERSION 5.15.2)
find_package(Qt5 "${REQUIRED_QT_VERSION}" CONFIG REQUIRED UiTools Widgets Xml Svg)
ecm_setup_version(PROJECT VARIABLE_PREFIX KJSEMBED
   #VERSION_HEADER "${CMAKE_CURRENT_BINARY_DIR}/kjsembed_version.h"
                        PACKAGE_VERSION_FILE "${CMAKE_CURRENT_BINARY_DIR}/KF5JsEmbedConfigVersion.cmake"
                        SOVERSION 5)

find_package(KF5JS ${KF_DEP_VERSION} REQUIRED)
find_package(KF5I18n ${KF_DEP_VERSION} REQUIRED)
find_package(KF5DocTools ${KF_DEP_VERSION})

ecm_set_disabled_deprecation_versions(
    QT 4.99
    KF 5.95
)

# remove definitions set by KDEFrameworkCompilerSettings which we fail to meet
remove_definitions(-DQT_NO_KEYWORDS)
remove_definitions(-DQT_NO_FOREACH)
remove_definitions(-DQT_NO_CAST_FROM_ASCII)
remove_definitions(-DQT_NO_CAST_FROM_BYTEARRAY)
# ideally we would support QT_NO_KEYWORDS, but at least we can do this
add_definitions(-DQT_NO_SIGNALS_SLOTS_KEYWORDS)

add_definitions(-DTRANSLATION_DOMAIN=\"kjsembed5\")
ki18n_install(po)
if (KF5DocTools_FOUND)
    kdoctools_install(po)
endif()
if(KF5DocTools_FOUND)
    add_subdirectory(docs)
endif()
add_subdirectory(src)
add_subdirectory(examples/kjsconsole)

# create a Config.cmake and a ConfigVersion.cmake file and install them
set(CMAKECONFIG_INSTALL_DIR "${KDE_INSTALL_CMAKEPACKAGEDIR}/KF5JsEmbed")

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

feature_summary(WHAT ALL   FATAL_ON_MISSING_REQUIRED_PACKAGES)
