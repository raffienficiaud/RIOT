include ../Makefile.tests_common

USEMODULE += suit
USEMODULE += riotboot_hdr
USEMODULE += embunit

# Lots of structs on the stack and crypto verification
CFLAGS += -DTHREAD_STACKSIZE_MAIN=\(8*THREAD_STACKSIZE_DEFAULT\)

# Add a macro for the board name without quotes to use in the include file
# generator macro
CFLAGS += -DBOARD_NAME_UNQ=$(BOARD)

# BINDIR is not included until Makefile.include is parsed
MANIFEST_DIR ?= bin/$(BOARD)/manifests
BLOBS += $(MANIFEST_DIR)/manifest0.bin
BLOBS += $(MANIFEST_DIR)/manifest1.bin
BLOBS += $(MANIFEST_DIR)/manifest2.bin
BLOBS += $(MANIFEST_DIR)/manifest3.bin

USEMODULE += suit_transport_mock

# Use a version of 'native' that includes flash page support
ifeq (native, $(BOARD))
  BOARDSDIR = $(CURDIR)/native_flashpage
endif

FEATURES_REQUIRED += periph_flashpage

TEST_DATA = $(MANIFEST_DIR)/created
BUILDDEPS += $(TEST_DATA)

include $(RIOTBASE)/Makefile.include

$(call target-export-variables,all,SUIT_TOOL SUIT_SEC MANIFEST_DIR)

$(TEST_DATA): $(SUIT_SEC) $(SUIT_PUB_HDR)
	@mkdir -p $(MANIFEST_DIR)
	sh create_test_data.sh
	@touch $@
