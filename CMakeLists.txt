cmake_minimum_required(VERSION 3.10 FATAL_ERROR)
project(osgStarter VERSION 1.0.0 LANGUAGES CXX)

set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_CXX_EXTENSIONS OFF)

# === Zmodyfikowana sekcja dla OpenSceneGraph ===
# Definiujemy opcję do podania ścieżki do OpenSceneGraph
# Użytkownik może ją ustawić np. poprzez -DOSG_ROOT_DIR="/path/to/osg"
option(OSG_ROOT_DIR "Specify the root directory of your OpenSceneGraph installation" "")

if(NOT OSG_ROOT_DIR STREQUAL "")
    # Jeśli użytkownik podał ścieżkę przez OSG_ROOT_DIR, użyjemy jej
    message(STATUS "Using OSG_ROOT_DIR provided by user: ${OSG_ROOT_DIR}")
    set(OpenSceneGraph_DIR "${OSG_ROOT_DIR}/lib/cmake/OpenSceneGraph" CACHE PATH "Path to OpenSceneGraph's CMake configuration files")
    # Alternatywnie, jeśli struktura OSG jest inna, można ustawić inne zmienne
    # np. set(OSG_INCLUDE_DIR "${OSG_ROOT_DIR}/include")
    # set(OSG_LIBRARY_DIR "${OSG_ROOT_DIR}/lib")
endif()

# Znajdź pakiet OpenSceneGraph z wymaganymi komponentami
# COMPONENTS określa, które biblioteki OSG są potrzebne (dopasowane do tych używanych w target_link_libraries)
# Dodano osgDB, ponieważ błędy linkera wskazują na brakujące symbole z tej biblioteki (np. osgDB::Registry::instance)
find_package(OpenSceneGraph REQUIRED COMPONENTS osg osgDB osgGA osgUtil osgViewer)

# Sprawdź, czy OpenSceneGraph zostało znalezione i wyświetl informację
if(OPENSCENEGRAPH_FOUND)
    message(STATUS "OpenSceneGraph found. Version: ${OPENSCENEGRAPH_VERSION}")
    message(STATUS "OpenSceneGraph include directories: ${OPENSCENEGRAPH_INCLUDE_DIR}")
    message(STATUS "OpenSceneGraph libraries: ${OPENSCENEGRAPH_LIBRARIES}")
else()
    message(FATAL_ERROR "OpenSceneGraph not found. Please set OSG_ROOT_DIR or OSG_DIR.")
endif()
# === Koniec zmodyfikowanej sekcji ===

add_subdirectory(src)