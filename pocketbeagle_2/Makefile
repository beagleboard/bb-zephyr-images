IMG_SIZE ?= 512

u-boot.img:
	$(info "Download U-Boot")
	wget https://pocketbeagle.beagleboard.io/u-boot-pocketbeagle2/u-boot.img

tispl.bin:
	$(info "Download TI-SPL")
	wget https://pocketbeagle.beagleboard.io/u-boot-pocketbeagle2/tispl.bin

tiboot3.bin:
	$(info "Download TI-Boot3")
	wget https://pocketbeagle.beagleboard.io/u-boot-pocketbeagle2/tiboot3.bin

zephyr.img: u-boot.img tispl.bin tiboot3.bin
	$(info "Building image")
	truncate -s $$(( ${IMG_SIZE} * 1024 * 1024 )) zephyr.img
	mformat -i zephyr.img -F -v BOOT
	mcopy -i zephyr.img u-boot.img tispl.bin tiboot3.bin ::
	mmd -i zephyr.img extlinux
	mcopy -i zephyr.img assets/extlinux.conf ::extlinux/

.PHONY: clean
clean:
	rm -rf u-boot.img
	rm -rf tispl.bin
	rm -rf tiboot3.bin
	rm -rf zephyr.img
