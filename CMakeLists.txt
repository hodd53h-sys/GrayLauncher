cmake_minimum_required(VERSION 3.20)
project(GrayLauncher VERSION 1.0.0 LANGUAGES CXX)

set(CMAKE_CXX_STANDARD 20)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_AUTOMOC ON)
set(CMAKE_AUTORCC ON)
set(CMAKE_AUTOUIC ON)

# Qt6
find_package(Qt6 REQUIRED COMPONENTS
    Core
    Widgets
    Network
    Concurrent
    WebEngineWidgets
)

# Sources
set(SOURCES
    src/main.cpp
    src/MainWindow.cpp
    src/Launcher.cpp
    src/MetaManager.cpp
    src/DownloadManager.cpp
    src/JavaManager.cpp
    src/AuthManager.cpp
    src/LogManager.cpp
    src/VersionManager.cpp
    src/ProfileManager.cpp
    src/ui/SideBar.cpp
    src/ui/PlayPage.cpp
    src/ui/SettingsPage.cpp
    src/ui/ConsolePage.cpp
    src/ui/AccountPage.cpp
    src/ui/ProgressOverlay.cpp
    src/ui/StyleHelper.cpp
)

set(HEADERS
    include/MainWindow.h
    include/Launcher.h
    include/MetaManager.h
    include/DownloadManager.h
    include/JavaManager.h
    include/AuthManager.h
    include/LogManager.h
    include/VersionManager.h
    include/ProfileManager.h
    include/ui/SideBar.h
    include/ui/PlayPage.h
    include/ui/SettingsPage.h
    include/ui/ConsolePage.h
    include/ui/AccountPage.h
    include/ui/ProgressOverlay.h
    include/ui/StyleHelper.h
    include/Constants.h
)

qt_add_executable(GrayLauncher
    ${SOURCES}
    ${HEADERS}
    resources/resources.qrc
)

target_include_directories(GrayLauncher PRIVATE include)

target_link_libraries(GrayLauncher PRIVATE
    Qt6::Core
    Qt6::Widgets
    Qt6::Network
    Qt6::Concurrent
    Qt6::WebEngineWidgets
)

# Windows: no console window in release
if(WIN32)
    set_target_properties(GrayLauncher PROPERTIES
        WIN32_EXECUTABLE TRUE
    )
endif()

# Install
install(TARGETS GrayLauncher
    BUNDLE DESTINATION .
    LIBRARY DESTINATION ${CMAKE_INSTALL_LIBDIR}
    RUNTIME DESTINATION ${CMAKE_INSTALL_BINDIR}
)
