IMG_SIZE ?= 128
BASE_BOOTLOADER_URL ?= https://github.com/beagleboard/u-boot-pocketbeagle2
BOOTLOADER_RELEASE_VERSION ?= latest

# Latest release links are different from some reason
ifeq (${BOOTLOADER_RELEASE_VERSION}, latest)
	DOWNLOAD_URL = ${BASE_BOOTLOADER_URL}/releases/latest/download
else
	DOWNLOAD_URL = ${BASE_BOOTLOADER_URL}/releases/download/${BOOTLOADER_RELEASE_VERSION}
endif

u-boot.img:
	$(info "Download U-Boot")
	wget ${DOWNLOAD_URL}/u-boot.img

tispl.bin:
	$(info "Download TI-SPL")
	wget ${DOWNLOAD_URL}/tispl.bin

tiboot3.bin:
	$(info "Download TI-Boot3")
	wget ${DOWNLOAD_URL}/tiboot3.bin

zephyr.img: u-boot.img tispl.bin tiboot3.bin
	$(info "Building image")
	truncate -s $$(( ${IMG_SIZE} * 1024 * 1024 )) zephyr.img
	mformat -i zephyr.img -F -v BOOT
	mcopy -i zephyr.img u-boot.img tispl.bin tiboot3.bin ::
	mmd -i zephyr.img extlinux
	mcopy -i zephyr.img assets/extlinux.conf ::extlinux/

zephyr.img.xz: zephyr.img
	$(info "Compressing release build")
	xz -z zephyr.img

.PHONY: clean
clean:
	rm -rf u-boot.img
	rm -rf tispl.bin
	rm -rf tiboot3.bin
	rm -rf zephyr.img
	rm -rf zephyr.img.xz
