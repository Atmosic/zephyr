# Copyright (c) 2022 Nordic Semiconductor
#
# SPDX-License-Identifier: Apache-2.0

# Include MCUboot if enabled.
if(SB_CONFIG_BOOTLOADER_MCUBOOT)
  set(image mcuboot)
  set(mcuboot_args
    APPLICATION ${image}
    SOURCE_DIR ${ZEPHYR_MCUBOOT_MODULE_DIR}/boot/zephyr/
    APP_TYPE BOOTLOADER
  )

  # Set target board to ${BOARD}@${BOARD_REVISION} if revision contains
  # "mcuboot" and qualifiers contain "ns".
  if(DEFINED BOARD_REVISION AND DEFINED BOARD_QUALIFIERS)
    string(REGEX MATCH "mcuboot" revision_has_mcuboot "${BOARD_REVISION}")
    string(REGEX MATCH "ns" qualifies_has_ns "${BOARD_QUALIFIERS}")
    if(revision_has_mcuboot AND qualifies_has_ns)
      set(board_str ${BOARD}@${BOARD_REVISION}_bl)
      list(APPEND mcuboot_args BOARD ${board_str})
    endif()
  endif()

  ExternalZephyrProject_Add(${mcuboot_args})
  # Place MCUBoot first in list to ensure it is configured and flashed before other images.
  sysbuild_add_dependencies(CONFIGURE ${DEFAULT_IMAGE} ${image})
  sysbuild_add_dependencies(FLASH ${DEFAULT_IMAGE} ${image})

  set_config_string(${image} CONFIG_BOOT_SIGNATURE_KEY_FILE "${SB_CONFIG_BOOT_SIGNATURE_KEY_FILE}")
  set_config_bool(${image} CONFIG_BOOT_ENCRYPT_IMAGE "${SB_CONFIG_BOOT_ENCRYPTION}")
  if(SB_CONFIG_BOOT_ENCRYPTION)
    set_config_string(${image} CONFIG_BOOT_ENCRYPTION_KEY_FILE "${SB_CONFIG_BOOT_ENCRYPTION_KEY_FILE}")
  endif()
endif()
