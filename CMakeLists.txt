cmake_minimum_required(VERSION 3.19)


project(lilv)

set(CMAKE_C_STANDARD 99)
set(CMAKE_C_STANDARD_REQUIRED ON)

set(lilv_sources
	src/collections.c
	src/instance.c
	src/lib.c
	src/node.c
	src/plugin.c
	src/pluginclass.c
	src/port.c
	src/query.c
	src/scalepoint.c
	src/state.c
	src/ui.c
	src/util.c
	src/world.c
)

set(sord_sources
	sord/src/sord.c
	sord/src/syntax.c
)

set(serd_sources	
	serd/src/base64.c
	serd/src/byte_source.c
	serd/src/env.c
	serd/src/n3.c
	serd/src/node.c
	serd/src/reader.c
	serd/src/string.c
	serd/src/system.c
	serd/src/uri.c
	serd/src/writer.c
)

set(zix_sources
	zix/src/allocator.c
	zix/src/btree.c
	zix/src/bump_allocator.c
	zix/src/digest.c
	zix/src/errno_status.c
	zix/src/filesystem.c
	zix/src/hash.c
	zix/src/path.c
	zix/src/ring.c
	zix/src/status.c
	zix/src/string_view.c
	zix/src/system.c
	zix/src/tree.c
)

if(MSVC OR MSYS OR MINGW)
	message("DETECTED WINDOWS!")
	set(zix_sources
		${zix_sources}
		zix/src/win32/filesystem_win32.c
		zix/src/win32/sem_win32.c
		zix/src/win32/system_win32.c
		zix/src/win32/thread_win32.c
	)
endif()

if(UNIX AND NOT APPLE)
	message("DETECTED LINUX!")
	set(zix_sources
		${zix_sources}
		zix/src/posix/filesystem_posix.c
		zix/src/posix/sem_posix.c
		zix/src/posix/system_posix.c
		zix/src/posix/thread_posix.c
	)
endif()

if(APPLE)
	message("DETECTED APPLE!")
	set(zix_sources
		${zix_sources}
		zix/src/darwin/sem_darwin.c
	)
endif()
set(zix_headers
	zix/include/zix/allocator.h
	zix/include/zix/attributes.h
	zix/include/zix/btree.h
	zix/include/zix/bump_allocator.h
	zix/include/zix/digest.h
	zix/include/zix/filesystem.h
	zix/include/zix/hash.h
	zix/include/zix/path.h
	zix/include/zix/ring.h
	zix/include/zix/sem.h
	zix/include/zix/status.h
	zix/include/zix/string_view.h
	zix/include/zix/thread.h
	zix/include/zix/tree.h
	zix/include/zix/zix.h
)

set(lilv_headers
	include/lilv/lilv.h
	src/lilv_config.h
	src/lilv_internal.h
	sord/include/sord/sord.h
	sord/src/sord_config.h
	sord/src/sord_internal.h
	serd/include/serd/serd.h
	serd/src/serd_config.h
	serd/src/serd_internal.h
	sratom/include/sratom/sratom.h
)

set(sratom_sources
	sratom/src/sratom.c
)

add_library(lilv SHARED ${lilv_sources} ${sord_sources} ${serd_sources} ${zix_sources} ${sratom_sources} ${lilv_headers} ${zix_headers})


target_include_directories(lilv PRIVATE include)
target_include_directories(lilv PRIVATE sord/include)
target_include_directories(lilv PRIVATE serd/include)
target_include_directories(lilv PRIVATE zix/include)
target_include_directories(lilv PRIVATE sratom/include)

target_link_libraries(lilv PRIVATE m)

#set(CMAKE_VERBOSE_MAKEFILE ON)

