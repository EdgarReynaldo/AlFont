
cmake_minimum_required(VERSION 3.0)

project(ALFONT)

set(DEPDIR "../../distro" CACHE PATH "Where the dependencies are installed")

set(FREETYPE2_INCDIR "${DEPDIR}/include/freetype2" CACHE PATH "Where to find headers for Freetype2")

set(FREETYPE2_LIBDIR "${DEPDIR}/lib" CACHE PATH "Where to find Freetype2 library.")

set(ALLEGRO4_LIBDIR "${DEPDIR}/lib" CACHE PATH "Where to find Allegro 4 libraries")

set(ALLEGRO4_INCDIR "${DEPDIR}/include" CACHE PATH "Where to find Allegro 4 headers")

if(CMAKE_BUILD_TYPE STREQUAL "Debug")

   add_library(AlFontSharedDebug SHARED src/alfont.c)

   add_library(AlFontDebug STATIC src/alfont.c)
   target_compile_definitions(AlFontDebug PUBLIC "-DALLEGRO_STATICLINK")

else()

   add_library(AlFontShared SHARED src/alfont.c)

   add_library(AlFont STATIC src/alfont.c)
   target_compile_definitions(AlFont PUBLIC "-DALLEGRO_STATICLINK")

endif(CMAKE_BUILD_TYPE STREQUAL "Debug")




find_library(ALLEGRO4_DEBUG liballeg44-debug.dll.a ${ALLEGRO4_LIBDIR})

find_library(ALLEGRO4_RELEASE liballeg44.dll.a ${ALLEGRO4_LIBDIR})

find_library(FREETYPE2_LIB freetype PATHS "${DEPDIR}/lib" NO_DEFAULT_PATH)

find_library(LIBPNG png16 PATHS "${DEPDIR}/lib" NO_DEFAULT_PATH)

find_library(ZLIB zlib PATHS "${DEPDIR}/lib" NO_DEFAULT_PATH)



if(CMAKE_BUILD_TYPE STREQUAL "Debug")

   target_include_directories(AlFontSharedDebug PUBLIC ${ALLEGRO4_INCDIR})
   target_include_directories(AlFontSharedDebug PUBLIC ${FREETYPE2_INCDIR})
   target_include_directories(AlFontSharedDebug PUBLIC include)

   target_include_directories(AlFontDebug PUBLIC ${ALLEGRO4_INCDIR})
   target_include_directories(AlFontDebug PUBLIC ${FREETYPE2_INCDIR})
   target_include_directories(AlFontDebug PUBLIC include)

   target_link_libraries(AlFontSharedDebug PUBLIC "${ALLEGRO4_DEBUG}")
   target_link_libraries(AlFontSharedDebug PUBLIC "${FREETYPE2_LIB}")
   target_link_libraries(AlFontSharedDebug PUBLIC "${LIBPNG}")
   target_link_libraries(AlFontSharedDebug PUBLIC "${ZLIB}")

   add_executable(Example1 examples/example1.c)

   target_link_libraries(Example1 PUBLIC AlFontSharedDebug)
   
   install(TARGETS AlFontSharedDebug DESTINATION "${CMAKE_INSTALL_PREFIX}/lib")
   install(TARGETS AlFontDebug DESTINATION "${CMAKE_INSTALL_PREFIX}/lib")

   
else()

   target_include_directories(AlFontShared PUBLIC ${ALLEGRO4_INCDIR})
   target_include_directories(AlFontShared PUBLIC ${FREETYPE2_INCDIR})
   target_include_directories(AlFontShared PUBLIC include)

   target_include_directories(AlFont PUBLIC ${ALLEGRO4_INCDIR})
   target_include_directories(AlFont PUBLIC ${FREETYPE2_INCDIR})
   target_include_directories(AlFont PUBLIC include)

   target_link_libraries(AlFontShared PUBLIC "${ALLEGRO4_RELEASE}")
   target_link_libraries(AlFontShared PUBLIC "${FREETYPE2_LIB}")
   target_link_libraries(AlFontShared PUBLIC "${LIBPNG}")
   target_link_libraries(AlFontShared PUBLIC "${ZLIB}")

   add_executable(Example1 examples/example1.c)

   target_link_libraries(Example1 PUBLIC AlFontShared)
   
   install(TARGETS AlFontShared DESTINATION "${CMAKE_INSTALL_PREFIX}/lib")
   install(TARGETS AlFont DESTINATION "${CMAKE_INSTALL_PREFIX}/lib")

endif(CMAKE_BUILD_TYPE STREQUAL "Debug")

install(DIRECTORY "include" DESTINATION ${CMAKE_INSTALL_PREFIX})

