# CMakeList.txt : CMake project for guiapp, include source and define
# project specific logic here.
#
cmake_minimum_required (VERSION 3.8)

set(project_name "libusb")
project(${project_name})

if (CMAKE_HOST_WIN32)
	message(STATUS "Platform=Windows")
	list(APPEND PLATFORM_DEFINE -DTARGET_HOST_MS_WINDOWS=1)
elseif (CMAKE_HOST_UNIX)
	message(STATUS "Platform=Linux")
	list(APPEND PLATFORM_DEFINE -DTARGET_HOST_POSIX_X11=1)
elseif (CMAKE_HOST_APPLE)
	message(STATUS "Platform=Macos")
endif ()


include_directories(
	${CMAKE_CURRENT_SOURCE_DIR}
	${CMAKE_CURRENT_SOURCE_DIR}/os
	${CMAKE_CURRENT_BINARY_DIR}
)
if(MSVC)
include_directories(
	${CMAKE_CURRENT_SOURCE_DIR}//msvc
)
else(MSVC)
include_directories(
	${CMAKE_CURRENT_SOURCE_DIR}/linux
)
endif(MSVC)
link_directories(
	${CMAKE_CURRENT_BINARY_DIR}
)

if(MSVC)
file(GLOB SRCS 
	"${CMAKE_CURRENT_SOURCE_DIR}/core.c"
	"${CMAKE_CURRENT_SOURCE_DIR}/descriptor.c"
	"${CMAKE_CURRENT_SOURCE_DIR}/hotplug.c"
	"${CMAKE_CURRENT_SOURCE_DIR}/io.c"
	"${CMAKE_CURRENT_SOURCE_DIR}/strerror.c"
	"${CMAKE_CURRENT_SOURCE_DIR}/sync.c"
	"${CMAKE_CURRENT_SOURCE_DIR}/os/events_windows.c"
	"${CMAKE_CURRENT_SOURCE_DIR}/os/threads_windows.c"
	"${CMAKE_CURRENT_SOURCE_DIR}/os/windows_common.c"
	"${CMAKE_CURRENT_SOURCE_DIR}/os/windows_usbdk.c"
	"${CMAKE_CURRENT_SOURCE_DIR}/os/windows_winusb.c"
)
else(MSVC)
file(GLOB SRCS 
	"${CMAKE_CURRENT_SOURCE_DIR}/core.c"
	"${CMAKE_CURRENT_SOURCE_DIR}/descriptor.c"
	"${CMAKE_CURRENT_SOURCE_DIR}/hotplug.c"
	"${CMAKE_CURRENT_SOURCE_DIR}/io.c"
	"${CMAKE_CURRENT_SOURCE_DIR}/strerror.c"
	"${CMAKE_CURRENT_SOURCE_DIR}/sync.c"
	"${CMAKE_CURRENT_SOURCE_DIR}/os/events_posix.c"
	"${CMAKE_CURRENT_SOURCE_DIR}/os/threads_posix.c"
	"${CMAKE_CURRENT_SOURCE_DIR}/os/linux_netlink.c"
	"${CMAKE_CURRENT_SOURCE_DIR}/os/linux_usbfs.c"
	"${CMAKE_CURRENT_SOURCE_DIR}/os/linux_udev.c"
)
endif(MSVC)

# Add source to this project's executable.

add_library (${project_name} STATIC ${SRCS})


# TODO: Add tests and install targets if needed.
