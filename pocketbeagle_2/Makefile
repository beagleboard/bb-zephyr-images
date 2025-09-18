IMG_SIZE ?= 128
BASE_BOOTLOADER_URL ?= https://github.com/beagleboard/u-boot-pocketbeagle2
BOOTLOADER_RELEASE_VERSION ?= latest

# Latest release links are different from some reason
ifeq (${BOOTLOADER_RELEASE_VERSION}, latest)
	DOWNLOAD_URL = ${BASE_BOOTLOADER_URL}/releases/latest/download
else
	DOWNLOAD_URL = ${BASE_BOOTLOADER_URL}/releases/download/${BOOTLOADER_RELEASE_VERSION}
endif

zephyr.img.xz: zephyr.img
	$(info "Compressing release build")
	xz -z zephyr.img

u-boot.img:
	$(info "Download U-Boot")
	wget ${DOWNLOAD_URL}/u-boot.img

tispl.bin:
	$(info "Download TI-SPL")
	wget ${DOWNLOAD_URL}/tispl.bin

tiboot3.bin:
	$(info "Download TI-Boot3")
	wget ${DOWNLOAD_URL}/tiboot3.bin

boot.scr: assets/boot.its assets/boot.txt
	mkimage -f assets/boot.its boot.scr

zephyr.img: u-boot.img tispl.bin tiboot3.bin boot.scr
	$(info "Building image")
	truncate -s $$(( ${IMG_SIZE} * 1024 * 1024 )) zephyr.img
	mformat -i zephyr.img -F -v BOOT
	mcopy -i zephyr.img u-boot.img tispl.bin tiboot3.bin boot.scr ::

.PHONY: clean
clean:
	rm -rf u-boot.img
	rm -rf tispl.bin
	rm -rf tiboot3.bin
	rm -rf zephyr.img
	rm -rf zephyr.img.xz
	rm -rf boot.scr
