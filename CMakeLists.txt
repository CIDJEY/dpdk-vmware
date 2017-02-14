cmake_minimum_required(VERSION 2.8)

include(ExternalProject)
include(ProcessorCount)

if(DEBUG)
	set(CFLAGS "-O0 -g")
else()
	set(CFLAGS "-O3 -g")
endif()

ProcessorCount(N)

ExternalProject_Add(
	dpdk
	PREFIX ${CMAKE_CURRENT_BINARY_DIR}
	GIT_REPOSITORY git://dpdk.org/dpdk
	GIT_TAG v16.11
	PATCH_COMMAND git apply --ignore-whitespace ${CMAKE_CURRENT_SOURCE_DIR}/vmware.patch
	BUILD_IN_SOURCE 1
	CONFIGURE_COMMAND echo
	BUILD_COMMAND DESTDIR=${CMAKE_BINARY_DIR}/out/usr/local make -j ${N} install T=x86_64-native-linuxapp-gcc EXTRA_CFLAGS=${CFLAGS}
	INSTALL_COMMAND echo
)