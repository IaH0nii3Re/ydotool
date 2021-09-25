cmake_minimum_required(VERSION 3.14)
project(ydotool)

set(CMAKE_CXX_STANDARD 17)

set(SOURCE_FILES_LIBRARY
        CommonIncludes.hpp
        Library/Tool.hpp Library/Tool.cpp
        Library/Utils.cpp Library/Utils.hpp
        Tools/Tools.hpp
        Tools/Sleep/Sleep.hpp Tools/Sleep/Sleep.cpp
        Tools/MouseMove/MouseMove.hpp Tools/MouseMove/MouseMove.cpp
        Tools/Key/Key.hpp Tools/Key/Key.cpp
        Tools/Click/Click.hpp Tools/Click/Click.cpp
        Tools/Type/Type.hpp Tools/Type/Type.cpp
        Tools/Recorder/Recorder.cpp Tools/Recorder/Recorder.hpp
        )
#        Library/Transports/UnixSocket.cpp Library/Transports/UnixSocket.hpp )

set(SOURCE_FILES_DAEMON CommonIncludes.hpp Daemon/ydotoold.cpp Daemon/ydotoold.hpp Library/Utils.hpp Library/Utils.cpp)


set(SOURCE_FILES_CLIENT
        CommonIncludes.hpp
        Client/ydotool.cpp Client/ydotool.hpp)

add_executable(ydotoold ${SOURCE_FILES_DAEMON})
target_link_libraries(ydotoold PUBLIC uInputPlus evdevPlus)
install(TARGETS ydotoold DESTINATION ${CMAKE_INSTALL_BINDIR})

add_library(ydotool_library STATIC ${SOURCE_FILES_LIBRARY})
add_executable(ydotool ${SOURCE_FILES_CLIENT})
target_link_libraries(ydotool ydotool_library dl pthread uInputPlus evdevPlus)
install(TARGETS ydotool DESTINATION ${CMAKE_INSTALL_BINDIR})

add_subdirectory(manpage)
add_subdirectory(Daemon)
