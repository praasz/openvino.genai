# Copyright (C) 2023-2024 Intel Corporation
# SPDX-License-Identifier: Apache-2.0

find_package(OpenVINOGenAI REQUIRED
    PATHS
        "${CMAKE_BINARY_DIR}"  # Reuse the package from the build.
        ${OpenVINO_DIR}  # GenAI may be installed alogside OpenVINO.
    NO_CMAKE_FIND_ROOT_PATH
)

file(DOWNLOAD https://raw.githubusercontent.com/nothings/stb/f75e8d1cad7d90d72ef7a4661f1b994ef78b4e31/stb_image.h ${CMAKE_BINARY_DIR}/stb_image.h
     EXPECTED_HASH MD5=27932e6fb3a2f26aee2fc33f2cb4e696)

# create main sample executable

add_executable(text2image text2image.cpp imwrite.cpp)

target_include_directories(text2image PRIVATE ${CMAKE_BINARY_DIR} "${CMAKE_CURRENT_SOURCE_DIR}")
target_link_libraries(text2image PRIVATE openvino::genai)

set_target_properties(text2image PROPERTIES
    COMPILE_PDB_NAME text2image
    # Ensure out of box LC_RPATH on macOS with SIP
    INSTALL_RPATH_USE_LINK_PATH ON)

install(TARGETS text2image
        RUNTIME DESTINATION samples_bin/
        COMPONENT samples_bin
        EXCLUDE_FROM_ALL)

# create LoRA sample executable

add_executable(lora_text2image lora_text2image.cpp imwrite.cpp)

target_include_directories(lora_text2image PRIVATE "${CMAKE_CURRENT_SOURCE_DIR}")
target_link_libraries(lora_text2image PRIVATE openvino::genai)

set_target_properties(lora_text2image PROPERTIES
    COMPILE_PDB_NAME lora_text2image
    # Ensure out of box LC_RPATH on macOS with SIP
    INSTALL_RPATH_USE_LINK_PATH ON)

install(TARGETS lora_text2image
        RUNTIME DESTINATION samples_bin/
        COMPONENT samples_bin
        EXCLUDE_FROM_ALL)

# create heterogeneous_stable_diffusion sample executable

add_executable(heterogeneous_stable_diffusion
    heterogeneous_stable_diffusion.cpp
    imwrite.cpp)

target_include_directories(heterogeneous_stable_diffusion PRIVATE "${CMAKE_CURRENT_SOURCE_DIR}")
target_link_libraries(heterogeneous_stable_diffusion PRIVATE openvino::genai)

set_target_properties(heterogeneous_stable_diffusion PROPERTIES
    COMPILE_PDB_NAME heterogeneous_stable_diffusion
    # Ensure out of box LC_RPATH on macOS with SIP
    INSTALL_RPATH_USE_LINK_PATH ON)

install(TARGETS heterogeneous_stable_diffusion
        RUNTIME DESTINATION samples_bin/
        COMPONENT samples_bin
        EXCLUDE_FROM_ALL)

# create image2image sample executable

add_executable(image2image image2image.cpp load_image.cpp imwrite.cpp)

target_include_directories(image2image PRIVATE "${CMAKE_CURRENT_SOURCE_DIR}" "${CMAKE_BINARY_DIR}")
target_link_libraries(image2image PRIVATE openvino::genai)

set_target_properties(image2image PROPERTIES
    COMPILE_PDB_NAME image2image
    # Ensure out of box LC_RPATH on macOS with SIP
    INSTALL_RPATH_USE_LINK_PATH ON)

install(TARGETS image2image
        RUNTIME DESTINATION samples_bin/
        COMPONENT samples_bin
        EXCLUDE_FROM_ALL)
        
# create LoRA sample executable

add_executable(inpainting inpainting.cpp load_image.cpp imwrite.cpp)

target_include_directories(inpainting PRIVATE "${CMAKE_CURRENT_SOURCE_DIR}" "${CMAKE_BINARY_DIR}")
target_link_libraries(inpainting PRIVATE openvino::genai)

set_target_properties(inpainting PROPERTIES
    COMPILE_PDB_NAME inpainting
    # Ensure out of box LC_RPATH on macOS with SIP
    INSTALL_RPATH_USE_LINK_PATH ON)

install(TARGETS inpainting
        RUNTIME DESTINATION samples_bin/
        COMPONENT samples_bin
        EXCLUDE_FROM_ALL)