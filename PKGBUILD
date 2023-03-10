# U-Boot: Quartz64 (Mainline with Rockchip blobs)
# Maintainer: Dan Johansen <strit@manjaro.org>

pkgname=uboot-soquartz-super6c
pkgver=2022.01.rc4
pkgrel=3
#_tfaver=2.5
pkgdesc="U-Boot for SoQuartz Super6c"
arch=('aarch64')
url='http://www.denx.de/wiki/U-Boot/WebHome'
license=('GPL')
makedepends=('dtc' 'bc')
provides=('uboot')
conflicts=('uboot')
install=${pkgname}.install
source=( #"ftp://ftp.denx.de/pub/u-boot/u-boot-${pkgver/rc/-rc}.tar.bz2" #for mainline release
        "https://gitlab.com/pgwipeout/u-boot-quartz64/-/archive/quartz64/u-boot-quartz64-quartz64.tar.gz" #Peter Geis' u-boot based on Mainline
        "rk3568_bl31_v1.28.elf::https://github.com/JeffyCN/rockchip_mirrors/blob/6186debcac95553f6b311cee10669e12c9c9963d/bin/rk35/rk3568_bl31_v1.28.elf?raw=true"
        "rk3568_ddr_1560MHz_v1.13.bin::https://github.com/JeffyCN/rockchip_mirrors/blob/ddf03c1d80b33dac72a33c4f732fc5849b47ff99/bin/rk35/rk3568_ddr_1056MHz_v1.13.bin?raw=true")
        #"https://git.trustedfirmware.org/TF-A/trusted-firmware-a.git/snapshot/trusted-firmware-a-$_tfaver.tar.gz") #for TF-A release
sha256sums=('2d1dcaa3dfdd7684e69402a9cd73440c5ab4dfddb15e0c05178da3a5b415647c'
            '67bf19566fb646e2f1f55b7fbf084f0d71b59b875a19a077e638b95adf1b254a'
            '6f165b37640eb876b5f41297bcce6451eb8a86fa56649633d4aca76047136a36')

prepare() {
  mv rk3568_bl31_v1.28.elf u-boot-quartz64-quartz64/bl31.elf
  mv rk3568_ddr_1560MHz_v1.13.bin u-boot-quartz64-quartz64/ram_init.bin
  #cd u-boot-${pkgver/rc/-rc}
}

build() {
  # Avoid build warnings by editing a .config option in place instead of
  # appending an option to .config, if an option is already present
  update_config() {
    if ! grep -q "^$1=$2$" .config; then
      if grep -q "^# $1 is not set$" .config; then
        sed -i -e "s/^# $1 is not set$/$1=$2/g" .config
      elif grep -q "^$1=" .config; then
        sed -i -e "s/^$1=.*/$1=$2/g" .config
      else
        echo "$1=$2" >> .config
      fi
    fi
  }

  unset CFLAGS CXXFLAGS CPPFLAGS LDFLAGS
  
  #echo -e "\nBuilding TF-A for Pine64 SoQuartz CM4...\n"
  #cd trusted-firmware-a-$_tfaver
  #make PLAT=rk356x
  #cp build/rk356x/release/bl31/bl31.elf ../u-boot-${pkgver/rc/-rc}/
  
  #cd ../u-boot-${pkgver/rc/-rc}
  cd u-boot-quartz64-quartz64

  echo -e "\nBuilding U-Boot for Pine64 SoQuartz CM4...\n"
  make quartz64-b-rk3566_defconfig

  update_config 'CONFIG_IDENT_STRING' '" Manjaro Linux ARM"'
  make EXTRAVERSION=-${pkgrel}
}

package() {
  #cd u-boot-${pkgver/rc/-rc}
  cd u-boot-quartz64-quartz64

  mkdir -p "${pkgdir}/boot/extlinux"

  install -D -m 0644 idbloader.img u-boot.itb -t "${pkgdir}/boot"
}
