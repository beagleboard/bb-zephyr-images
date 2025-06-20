IMG_SIZE ?= 128
BASE_BOOTLOADER_URL ?= https://github.com/beagleboard/u-boot-pocketbeagle2/releases/download
BOOTLOADER_RELEASE_VERSION ?= v2025.07-rc4-am6232-pocketbeagle2-11.01.01

u-boot.img:
	$(info "Download U-Boot")
	wget ${BASE_BOOTLOADER_URL}/${BOOTLOADER_RELEASE_VERSION}/u-boot.img

tispl.bin:
	$(info "Download TI-SPL")
	wget ${BASE_BOOTLOADER_URL}/${BOOTLOADER_RELEASE_VERSION}/tispl.bin

tiboot3.bin:
	$(info "Download TI-Boot3")
	wget ${BASE_BOOTLOADER_URL}/${BOOTLOADER_RELEASE_VERSION}/tiboot3.bin

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
