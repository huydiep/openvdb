FILE(GLOB OPENVDB_UTIL_SOURCE_FILES ${CMAKE_CURRENT_SOURCE_DIR}/*.cc)

IF(MSVC)
    ADD_DEFINITIONS(-DNT_PLUGIN -DREQUIRE_IOSTREAM)
ENDIF()

FIND_PACKAGE(OpenGL)

FIND_PATH(MAYA_DIR REQUIRED)
SET(MAYA_INCLUDE_DIR "${MAYA_DIR}/include")
SET(MAYA_LIBRARY_DIR "${MAYA_DIR}/lib")
INCLUDE_DIRECTORIES(${MAYA_INCLUDE_DIR})
LINK_DIRECTORIES(${MAYA_LIBRARY_DIR})

FIND_PATH(GLEW_DIR REQUIRED)
SET(GLEW_INCLUDE_DIR "${GLEW_DIR}/include")
SET(GLEW_LIBRARY_DIR "${GLEW_DIR}/lib")
INCLUDE_DIRECTORIES(${GLEW_INCLUDE_DIR})
LINK_DIRECTORIES(${GLEW_LIBRARY_DIR})

FILE(GLOB OPENVDBMAYA_SOURCE_FILES ${CMAKE_CURRENT_SOURCE_DIR}/*.cc)
FILE(GLOB OPENVDBMAYA_HEADER_FILES ${CMAKE_CURRENT_SOURCE_DIR}/*.h)
FILE(GLOB OPENVDBMAYA_SCRIPT_FILES ${CMAKE_CURRENT_SOURCE_DIR}/*.mel)

ADD_LIBRARY(OpenVDBMaya SHARED ${OPENVDBMAYA_SOURCE_FILES} ${OPENVDBMAYA_HEADER_FILES} ${OPENVDBMAYA_SCRIPT_FILES})
TARGET_LINK_LIBRARIES(OpenVDBMaya openvdb Half tbb ${ZLIB_LIBRARIES} OpenMayaUI Foundation OpenMaya OpenMayaFX OpenMayaRender glew32 ${OPENGL_LIBRARIES})

INSTALL(TARGETS OpenVDBMaya RUNTIME DESTINATION "${CMAKE_INSTALL_PREFIX}/plugins/maya/plugins")
INSTALL(FILES ${OPENVDBMAYA_SCRIPT_FILES} DESTINATION "${CMAKE_INSTALL_PREFIX}/plugins/maya/scripts")
