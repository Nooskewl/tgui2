cmake_minimum_required(VERSION 2.8.12)
project(tgui2 CXX)

OPTION(WANT_WIDGETS "Want some default widgets" on)

# Set USER_INCLUDE_PATH as may be necessary
# e.g., -DUSER_INCLUDE_PATH=/home/trent/local/include

set(USER_INCLUDE_PATH "" CACHE FILEPATH "Any extra include directories you want")
set(USER_LIBRARY_PATH "" CACHE FILEPATH "Any extra library directories you want")

include_directories(${USER_INCLUDE_PATH})
link_directories(${USER_LIBRARY_PATH})

set(SOURCES tgui2.cpp)

if(WANT_WIDGETS)
	list(APPEND SOURCES tgui2_widgets.cpp)
endif()

add_library(tgui2-static STATIC ${SOURCES})

if(MINGW)
	set_target_properties(tgui2-static PROPERTIES COMPILE_FLAGS "-DTGUI2_BUILD -DTGUI2_STATIC")
elseif(WIN32)
	set_target_properties(tgui2-static PROPERTIES COMPILE_FLAGS "/DTGUI2_BUILD /DTGUI2_STATIC /MT /Zl")
else()
	set_target_properties(tgui2-static PROPERTIES COMPILE_FLAGS "-O3")
endif()
