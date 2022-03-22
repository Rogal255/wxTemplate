cmake_minimum_required(VERSION 3.21)

project(wxWidgetsFetchContent)

#Source files list (cpp only)
set(SRC_FILES main.cpp)

#Set which libs should link to main target
set(LIBS wxbase wxcore)

#wxWidgets config
set(wxBUILD_SHARED ON CACHE BOOL "Build wx libraries as shared libs")
set(wxBUILD_PRECOMP OFF CACHE BOOL "Use precompiled headers")
set(wxBUILD_MONOLITHIC OFF CACHE BOOL "Build a single library")

include(FetchContent)
Set(FETCHCONTENT_QUIET FALSE)
FetchContent_Declare(
        wxWidgets
        GIT_REPOSITORY  https://github.com/wxWidgets/wxWidgets.git
        GIT_TAG         master
        GIT_PROGRESS    TRUE
        GIT_SHALLOW     TRUE
)
FetchContent_MakeAvailable(wxWidgets)

#Main target config
add_executable(${PROJECT_NAME} WIN32 ${SRC_FILES})
set_target_properties(${PROJECT_NAME} PROPERTIES CXX_STANDARD 20 CXX_STANDARD_REQUIRED ON)
add_dependencies(${PROJECT_NAME} wxbase wxcore wxhtml wxnet wxxml wxxrc)
if (wxBUILD_SHARED)
    target_compile_definitions(${PROJECT_NAME} PRIVATE WXUSINGDLL=1)
endif ()
target_link_libraries(${PROJECT_NAME} PRIVATE ${LIBS})
