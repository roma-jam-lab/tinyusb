idf_build_get_property(target IDF_TARGET)

# MCU family selection
if(${target} MATCHES "esp32(s[23]|h4)")
    set(tusb_family "esp32sx")
else()
    set(tusb_family "esp32px")
endif()

# MCU mapping for each target
set(esp32s3_MCU "OPT_MCU_ESP32S3")
set(esp32s2_MCU "OPT_MCU_ESP32S2")
set(esp32p4_MCU "OPT_MCU_ESP32P4")
set(esp32h4_MCU "OPT_MCU_ESP32H4")
set(tusb_mcu "${${target}_MCU}")

string(CONCAT tinyusb_status
    "TinyUSB compile parameters: \n"
    "-------------------------------\n"
    "Family: ${tusb_family}\n"
    "MCU OPT: ${tusb_mcu}\n"
    "-------------------------------")
message(STATUS "${tinyusb_status}")

set(compile_options
    "-DCFG_TUSB_MCU=${tusb_mcu}"
    )

idf_component_get_property(freertos_include freertos ORIG_INCLUDE_PATH)

set(includes_private
    "src/"
    "src/device"
    "src/host"
    )

set(includes_public
    "src/"
    # The FreeRTOS API include convention in tinyusb is different from esp-idf
    "${freertos_include}"
    )

# tinyusb
set(srcs
    "src/portable/synopsys/dwc2/dwc2_common.c"
    "src/common/tusb_fifo.c"
    "src/tusb.c"
)

# TinyUSB device
# list(APPEND srcs
        # "src/portable/synopsys/dwc2/dcd_dwc2.c"
        # "src/device/usbd_control.c"
        # "src/device/usbd.c"
    # )

# TinyUSB host
list (APPEND srcs
        "src/portable/synopsys/dwc2/hcd_dwc2.c"
        "src/host/usbh.c"
        "src/host/hub.c"
        "src/class/hid/hid_host.c"
    )

    # "src/class/cdc/cdc_device.c"
    # "src/class/hid/hid_device.c"
    # "src/class/midi/midi_device.c"
    # "src/class/msc/msc_device.c"
    # "src/class/vendor/vendor_device.c"
    # "src/class/audio/audio_device.c"
    # "src/class/video/video_device.c"
    # "src/class/bth/bth_device.c"
    # "src/class/usbtmc/usbtmc_device.c"
    # NET class
    # "src/class/net/ecm_rndis_device.c"
    # "lib/networking/rndis_reports.c"
    # "src/class/net/ncm_device.c"
    # DFU
    # "src/class/dfu/dfu_device.c"
    # "src/class/dfu/dfu_rt_device.c"
    # Common, device-mode related

# set(requirements_private
    # esp_netif   # required by rndis_reports.c: #include "netif/ethernet.h"
    # )

# if(${target} STREQUAL "esp32p4")
    # list(APPEND requirements_private
                # esp_mm # required by dcd_dwc2.c: #include "esp_cache.h"
        # )
# endif()

idf_component_register(SRCS ${srcs}
                       INCLUDE_DIRS ${includes_public}
                       PRIV_INCLUDE_DIRS ${includes_private}
                       PRIV_REQUIRES  ${requirements_private}
                       )

target_compile_options(${COMPONENT_LIB} PUBLIC ${compile_options})

# when no builtin class driver is enabled, an uint8_t data compared with `BUILTIN_DRIVER_COUNT` will always be false
# set_source_files_properties("src/device/usbd.c" PROPERTIES COMPILE_FLAGS "-Wno-type-limits")
