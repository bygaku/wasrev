cmake_minimum_required(VERSION 3.22)

#project setting.
set(EXE_NAME "WasReverb")
set(PROJECT_NAME "Reverb")

project(${PROJECT_NAME} VERSION 1.0.0 LANGUAGES CXX)
set(CMAKE_CXX_STANDARD 20)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_CXX_EXTENSIONS OFF)
set(CMAKE_EXPORT_COMPILE_COMMANDS ON)


if(MSVC)
    add_compile_options(/utf-8 /TP)
endif()

#subdirectory.
add_subdirectory(./lib)

#make target.
juce_add_plugin(${EXE_NAME}
        COMPANY_NAME "Gaku-Fujii"
        COMPANY_WEBSITE "https://intwist.carrd.co"
        COMPANY_EMAIL "intmainwist@gmail.com"
        BUNDLE_ID "dev.wist.${EXE_NAME}"
        PLUGIN_MANUFACTURER_CODE "gaku"
        PLUGIN_CODE "warv"
        FORMATS Standalone VST3 AU
        COPY_PLUGIN_AFTER_BUILD TRUE
        PLUGIN_NAME "RealTime-Reverb"
)

target_compile_features(${EXE_NAME}
        PUBLIC
        cxx_std_20
)

target_compile_definitions(${EXE_NAME}
        PUBLIC
        JUCE_WEB_BROWSER=0
        JUCE_USE_CURL=0
        JUCE_VST3_CAN_REPLACE_VST2=0
)

#add sources to target.
target_sources(${EXE_NAME}
        PRIVATE
        ./src/PluginEditor.cpp
        ./src/PluginProcessor.cpp
)

target_link_libraries(${EXE_NAME}
        PRIVATE
        juce::juce_audio_utils
        PUBLIC
        juce::juce_recommended_config_flags
        juce::juce_recommended_lto_flags
        juce::juce_recommended_warning_flags)

target_include_directories(${EXE_NAME}
    PUBLIC
        ./include/
)