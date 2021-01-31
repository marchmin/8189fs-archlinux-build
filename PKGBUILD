pkgname=8189fs-git
_pkgname=rtl8189ES_linux
pkgver=r64.c6493c3
pkgrel=1
pkgdesc="Kernel module for Realtek RTL8189FTV SDIO wireless devices."
arch=('aarch64')
url="https://github.com/jwrdegoede/rtl8189ES_linux/tree/rtl8189fs"
license=('GPL')
depends=('linux')
makedepends=('linux-headers' 'git')
source=("git://github.com/jwrdegoede/$_pkgname.git#branch=rtl8189fs"
	    "0001-Disable-debug-messages.patch")
sha256sums=('SKIP'
            '0ffeea04836aa157e87af921596f4c1b8c6b3cb4acf1dc2cd464ff95626bee7c')
install=depmod.install

_extramodules="$(basename $(readlink -f /lib/modules/$(uname -r)/extramodules/))"

pkgver() {
  cd "$_pkgname"
  printf "r%s.%s" "$(git rev-list --count HEAD)" "$(git rev-parse --short HEAD)"
}

prepare() {
  cd "$_pkgname"
  local i; for i in "${source[@]}"; do
    case $i in
      *.patch)
        msg2 "Applying patch ${i}"
        patch -p1 -i "${srcdir}/${i}"
    esac
  done
}

build() {
  cd "$_pkgname"
  make ARCH=arm64 KSRC="/lib/modules/$(uname -r)/build/"
  gzip -9 8189fs.ko
}

package() {
  install -d "$pkgdir/usr/lib/modules/${_extramodules}/"
  install -m644 "$srcdir/$_pkgname/8189fs.ko.gz" \
    "$pkgdir/usr/lib/modules/${_extramodules}/8189fs.ko.gz"
}
