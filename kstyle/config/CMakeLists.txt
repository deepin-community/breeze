### configuration plugin
add_definitions(-DTRANSLATION_DOMAIN=\"breeze_style_config\")
set(kstyle_breeze_config_PART_SRCS breezestyleconfig.cpp)

set(kstyle_breeze_config_PART_SRCS ${kstyle_breeze_config_PART_SRCS} breezestyleconfigmodule.cpp)

kconfig_add_kcfg_files(kstyle_breeze_config_PART_SRCS ../breezestyleconfigdata.kcfgc)
set(kstyle_breeze_config_PART_FORMS ui/breezestyleconfig.ui)
ki18n_wrap_ui(kstyle_breeze_config_PART_FORMS_HEADERS ${kstyle_breeze_config_PART_FORMS})

add_library(kstyle_breeze_config MODULE
    ${kstyle_breeze_config_PART_SRCS}
    ${kstyle_breeze_config_PART_FORMS_HEADERS}
)

target_link_libraries(kstyle_breeze_config Qt::Core Qt::Gui Qt::Widgets Qt::DBus)
target_link_libraries(kstyle_breeze_config KF5::CoreAddons KF5::ConfigCore KF5::ConfigWidgets KF5::I18n)
install(TARGETS kstyle_breeze_config DESTINATION ${KDE_INSTALL_QTPLUGINDIR})
install(FILES breezestyleconfig.desktop DESTINATION  ${KDE_INSTALL_KSERVICES5DIR})

########### next target ###############
find_package(KF5KCMUtils ${KF5_MIN_VERSION} CONFIG)
set_package_properties(KF5KCMUtils PROPERTIES
    DESCRIPTION "KF5 KCM utils Framework"
    URL "https://projects.kde.org/projects/frameworks/kcmutils"
    TYPE OPTIONAL
    PURPOSE "Required for breeze-settings5 application to build")

if( KF5KCMUtils_FOUND )
    set(breeze_settings_SOURCES main.cpp)
    add_executable(breeze-settings5 ${breeze_settings_SOURCES} )
    target_link_libraries(breeze-settings5 Qt::Core Qt::Gui Qt::Widgets )
    target_link_libraries(breeze-settings5 KF5::I18n KF5::KCMUtils)

    install(TARGETS breeze-settings5 ${KDE_INSTALL_TARGETS_DEFAULT_ARGS})

    # icon
    ecm_install_icons(ICONS sc-apps-breeze-settings.svgz
        DESTINATION ${KDE_INSTALL_ICONDIR}
        THEME hicolor )
endif()
