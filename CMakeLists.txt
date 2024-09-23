# Define the project for ImGui
project(ImGui LANGUAGES CXX)

# Set output directories (going one more directory back)
set(OUTPUT_DIR ${CMAKE_BINARY_DIR}/../bin-int/NeoStormSolution)
set(OUTPUT_TYPE_DIR ${OUTPUT_DIR}/${CMAKE_BUILD_TYPE})

# Create the ImGui static library (sources are in the same directory)
add_library(ImGui STATIC
        imconfig.h
        imgui.h
        imgui.cpp
        imgui_draw.cpp
        imgui_internal.h
        imgui_tables.cpp
        imgui_widgets.cpp
        imstb_rectpack.h
        imstb_textedit.h
        imstb_truetype.h
        imgui_demo.cpp
)

# Specify include directories (include files are in the same place)
target_include_directories(ImGui PUBLIC
        ${CMAKE_CURRENT_SOURCE_DIR}/
)

# Define output directories for the library
set_target_properties(ImGui PROPERTIES
        RUNTIME_OUTPUT_DIRECTORY ${OUTPUT_TYPE_DIR}
        ARCHIVE_OUTPUT_DIRECTORY ${OUTPUT_TYPE_DIR}
        LIBRARY_OUTPUT_DIRECTORY ${OUTPUT_TYPE_DIR}
)

# Compiler-specific settings
if(WIN32)
    set(CMAKE_CXX_STANDARD_REQUIRED ON)
    set(CMAKE_CXX_EXTENSIONS OFF)  # Disable compiler-specific extensions
elseif(UNIX)
    set(CMAKE_POSITION_INDEPENDENT_CODE ON)  # Enable position-independent code for shared libraries
endif()

# Add specific compile options for different configurations
target_compile_definitions(ImGui PRIVATE
        $<$<CONFIG:Debug>:DEBUG>
        $<$<CONFIG:Release>:NDEBUG>
        $<$<CONFIG:Dist>:DIST_BUILD>
)

# Optional: Set compiler flags
target_compile_options(ImGui PRIVATE
        $<$<CONFIG:Debug>:-g>
        $<$<CONFIG:Release>:-O3>
)
