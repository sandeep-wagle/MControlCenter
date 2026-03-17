cmake_minimum_required(VERSION 3.25)

project(MControlCenter VERSION 0.5.1 LANGUAGES CXX)

set(CMAKE_INCLUDE_CURRENT_DIR ON)

set(CMAKE_AUTOUIC ON)
set(CMAKE_AUTOMOC ON)
set(CMAKE_AUTORCC ON)

set(CMAKE_CXX_STANDARD 20)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

add_compile_options(-fPIC)

find_package(Qt6 6.2 REQUIRED COMPONENTS Widgets LinguistTools DBus)

set(PROJECT_SOURCES
    src/main.cpp
    src/mainwindow.ui
    src/mainwindow.cpp src/mainwindow.h
    src/operate.cpp src/operate.h
    src/helper.cpp src/helper.h
    src/msi-ec_helper.cpp src/msi-ec_helper.h
    src/settings.cpp src/settings.h
    src/resources.qrc
)

set(TS_FILES
    MControlCenter_de_DE.ts
    MControlCenter_en.ts
    MControlCenter_es.ts
    MControlCenter_eu.ts
    MControlCenter_fi_FI.ts
    MControlCenter_fr.ts
    MControlCenter_hu.ts
    MControlCenter_it.ts
    MControlCenter_nb.ts
    MControlCenter_nl_NL.ts
    MControlCenter_pt_BR.ts
    MControlCenter_pt_PT.ts
    MControlCenter_ru.ts
    MControlCenter_tr.ts
    MControlCenter_vi.ts
    MControlCenter_zh_CN.ts
)

message(STATUS "Skipping runtime translation generation during configure")
if(Qt6_FOUND)
    qt_add_executable(mcontrolcenter
        MANUAL_FINALIZATION
        ${PROJECT_SOURCES}
    )
# Define target properties for Android with Qt 6 as:
#    set_property(TARGET mcontrolcenter APPEND PROPERTY QT_ANDROID_PACKAGE_SOURCE_DIR
#                 ${CMAKE_CURRENT_SOURCE_DIR}/android)
# For more information, see https://doc.qt.io/qt-6/qt-add-executable.html#target-creation
else()
    if(ANDROID)
        add_library(mcontrolcenter SHARED
            ${PROJECT_SOURCES}
        )
# Define properties for Android with Qt 5 after find_package() calls as:
#    set(ANDROID_PACKAGE_SOURCE_DIR "${CMAKE_CURRENT_SOURCE_DIR}/android")
    else()
        add_executable(mcontrolcenter
            ${PROJECT_SOURCES}
        )
    endif()
endif()

target_link_libraries(mcontrolcenter PRIVATE Qt${QT_VERSION_MAJOR}::Widgets Qt${QT_VERSION_MAJOR}::DBus)

install(TARGETS mcontrolcenter
    BUNDLE DESTINATION .
    LIBRARY DESTINATION ${CMAKE_INSTALL_LIBDIR})

add_subdirectory(src/helper ${CMAKE_BINARY_DIR}/helper)
add_definitions(-DMControlCenter_VERSION="${PROJECT_VERSION}")
