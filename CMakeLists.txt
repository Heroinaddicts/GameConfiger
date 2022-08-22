cmake_minimum_required(VERSION 3.5)

project(GameConfiger LANGUAGES CXX)

set(CMAKE_INCLUDE_CURRENT_DIR ON)

set(CMAKE_AUTOUIC ON)
set(CMAKE_AUTOMOC ON)
set(CMAKE_AUTORCC ON)

set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

# QtCreator supports the following variables for Android, which are identical to qmake Android variables.
# Check http://doc.qt.io/qt-5/deployment-android.html for more information.
# They need to be set before the find_package(Qt5 ...) call.

#if(ANDROID)
#    set(ANDROID_PACKAGE_SOURCE_DIR "${CMAKE_CURRENT_SOURCE_DIR}/android")
#    if (ANDROID_ABI STREQUAL "armeabi-v7a")
#        set(ANDROID_EXTRA_LIBS
#            ${CMAKE_CURRENT_SOURCE_DIR}/path/to/libcrypto.so
#            ${CMAKE_CURRENT_SOURCE_DIR}/path/to/libssl.so)
#    endif()
#endif()

find_package(Qt5 COMPONENTS Widgets LinguistTools REQUIRED)

INCLUDE_DIRECTORIES(
    ${PROJECT_SOURCE_DIR}/src
)

set(code_ui_MainWindow
    ${PROJECT_SOURCE_DIR}/src/ui/MainWindow.ui
    ${PROJECT_SOURCE_DIR}/src/ui/MainWindow.h
    ${PROJECT_SOURCE_DIR}/src/ui/MainWindow.cpp
)

set(code_main
    ${PROJECT_SOURCE_DIR}/src/main.cpp
)

set(language_ts
    ${PROJECT_SOURCE_DIR}/assets/language/GameConfiger_zh_CN.ts
)

SOURCE_GROUP(/ FILES ${code_main})
SOURCE_GROUP(ui/MainWindow FILES ${code_ui_MainWindow})
SOURCE_GROUP(language FILES ${language_ts})

if(ANDROID)
  add_library(GameConfiger SHARED
      ${code_ui_MainWindow}
      ${code_main}
      ${language_ts}
  )
else()
  add_executable(GameConfiger
      ${code_ui_MainWindow}
      ${code_main}
      ${language_ts}
  )
endif()

target_link_libraries(GameConfiger PRIVATE Qt5::Widgets)

qt5_create_translation(QM_FILES ${CMAKE_SOURCE_DIR} ${TS_FILES})
