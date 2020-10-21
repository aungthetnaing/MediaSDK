set(CMAKE_MODULE_PATH ~/host_setup/paho.mqtt.cpp/cmake)
find_package(OpenCV 4.5.0 REQUIRED )
find_package(PahoMqttC REQUIRED)

include_directories (
  ${CMAKE_CURRENT_SOURCE_DIR}/../sample_common/include
  ${CMAKE_CURRENT_SOURCE_DIR}/../sample_misc/wayland/include
  ${CMAKE_CURRENT_SOURCE_DIR}/include
  ${CMAKE_CURRENT_SOURCE_DIR}/../sample_plugins/vpp_plugin/include
  ${CMAKE_CURRENT_SOURCE_DIR}/../sample_plugins/rotate_cpu/include
  ${CMAKE_CURRENT_SOURCE_DIR}/../../tutorials/common/
)

add_library(
   ffmpeg_utils
   ${CMAKE_CURRENT_SOURCE_DIR}/../../tutorials/common/common_utils_linux.cpp
   ${CMAKE_CURRENT_SOURCE_DIR}/../../tutorials/common/ffmpeg_utils.cpp
   ${CMAKE_CURRENT_SOURCE_DIR}/../../tutorials/common/cmd_options.cpp
)

set(HEADERS
   ${HEADERS}
   ${CMAKE_CURRENT_SOURCE_DIR}/../../tutorials/common/common_utils.h
   ${CMAKE_CURRENT_SOURCE_DIR}/../../tutorials/common/ffmpeg_utils.h
   ${CMAKE_CURRENT_SOURCE_DIR}/../../tutorials/common/common_vaapi.h
   ${CMAKE_CURRENT_SOURCE_DIR}/../../tutorials/common/bits/linux_defs.h
   ${CMAKE_CURRENT_SOURCE_DIR}/../../tutorials/common/cmd_options.h
)

list( APPEND LIBS_VARIANT sample_common )
list( APPEND LIBS_NOVARIANT vpp_plugin )

set(DEPENDENCIES itt libmfx dl pthread)

make_executable( shortname universal )

target_link_libraries(${target}  ${OpenCV_LIBS} paho-mqttpp3 paho-mqtt3a ffmpeg_utils -lavformat -lavcodec -lavutil -lswresample)

install( TARGETS ${target} RUNTIME DESTINATION ${MFX_SAMPLES_INSTALL_BIN_DIR} )
