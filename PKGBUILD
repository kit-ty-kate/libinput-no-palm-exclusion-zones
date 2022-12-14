# Maintainer: Andreas Radke <andyrtr@archlinux.org>

pkgname=libinput
pkgver=1.22.0
pkgrel=1.2
pkgdesc="Input device management and event handling library"
url="https://gitlab.freedesktop.org/libinput"
arch=(aarch64)
license=(custom:X11)
depends=('mtdev' 'systemd' 'libevdev' 'libwacom')
# upstream doesn't recommend building docs
makedepends=('gtk4' 'meson' 'wayland-protocols' 'check') # 'doxygen' 'graphviz' 'python-sphinx' 'python-recommonmark'
optdepends=('gtk4: libinput debug-gui'
            'python-pyudev: libinput measure'
            'python-libevdev: libinput measure')
options=(debug)
source=(https://gitlab.freedesktop.org/libinput/libinput/-/archive/$pkgver/$pkgname-$pkgver.tar.bz2
	0001-quirks-Add-Apple-MTP-touchpad-quirk.patch
	no-palm-exclusion-zones.patch)
sha256sums=('b9204a860b4f6f9b2587c484f67a21fbdedd01a97db1f983b0b51cb7bdd428aa'
            'c021958e9fab4dab2572311b56eabdc14088af077978e9023a9ca9bbe8b2b90a'
            'd38aa046d680677e55a05f92d9f77b66152a4fed3e42bf10f38cd11447205d7b')
#validpgpkeys=('3C2C43D9447D5938EF4551EBE23B7E70B467F0BF') # Peter Hutterer (Who-T) <office@who-t.net>

build() {
  ( cd $pkgname-$pkgver && patch -p1 -i ../0001-quirks-Add-Apple-MTP-touchpad-quirk.patch )
  ( cd $pkgname-$pkgver && patch -p1 -i ../no-palm-exclusion-zones.patch )

  arch-meson $pkgname-$pkgver build \
    -D udev-dir=/usr/lib/udev \
    -D documentation=false

  # Print config
  meson configure build

  meson compile -C build
}

check() {
  meson test -C build --print-errorlogs
}

package() {
  meson install -C build --destdir "$pkgdir"

  install -Dvm644 $pkgname-$pkgver/COPYING \
    "$pkgdir/usr/share/licenses/$pkgname/LICENSE"
}
