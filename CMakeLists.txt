# CMakeLists.txt for libopusenc
cmake_minimum_required(VERSION 3.15)
project(libopusenc LANGUAGES C)

# Set options to disable documentation and examples
option(OP_ENABLE_EXAMPLES "Enable examples" OFF)

# Define compile-time flags for compatibility
add_definitions(-DRANDOM_PREFIX=libopusenc -DOUTSIDE_SPEEX)

# Library source files
set(OPUSENC_SOURCES
    src/ogg_packer.c
    src/opus_header.c
    src/opusenc.c
    src/picture.c
    src/resample.c
    src/unicode_support.c
)

# Private headers
set(OPUSENC_HEADERS
    src/arch.h
    src/ogg_packer.h
    src/opus_header.h
    src/picture.h
    src/resample_sse.h
    src/speex_resampler.h
    src/unicode_support.h
)

# Create the opusenc library
add_library(opusenc STATIC ${OPUSENC_SOURCES})
target_include_directories(opusenc PRIVATE ${CMAKE_CURRENT_SOURCE_DIR}/include)

# Define PACKAGE_NAME and PACKAGE_VERSION with proper quoting after target is created
target_compile_definitions(opusenc PRIVATE PACKAGE_NAME=\"libopusenc\")
target_compile_definitions(opusenc PRIVATE PACKAGE_VERSION=\"0.2.1\")

# Link with required libraries (Ogg and Opus)
target_link_libraries(opusenc PRIVATE opus ogg)

# Optional examples
if(OP_ENABLE_EXAMPLES)
    add_executable(opusenc_example examples/opusenc_example.c)
    target_link_libraries(opusenc_example PRIVATE opusenc)
endif()

# Install target for headers
install(FILES include/opusenc.h DESTINATION ${CMAKE_INSTALL_INCLUDEDIR}/opus)
