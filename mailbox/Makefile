# Host tests and cartridge firmware builds for the mailbox carts.
#
#   make host-tests      every host_test Makefile, no SDK or console toolchain
#   make <cart>          that cart's firmware from its baked client (needs PICO_SDK_PATH)
#   make firmware        every cart in BAKED

BAKED = template studio2 arcadia astrocade channelf sms atari-7800

HOST_TESTS = common/host_test $(wildcard */firmware/host_test)

host-tests:
	@set -e; for dir in $(HOST_TESTS); do \
	    [ -f $$dir/Makefile ] || continue; \
	    echo "== $$dir"; $(MAKE) -C $$dir; \
	done

$(BAKED):
	cd $@ && ./build-cart.sh --baked

firmware: $(BAKED)

.PHONY: host-tests firmware $(BAKED)
