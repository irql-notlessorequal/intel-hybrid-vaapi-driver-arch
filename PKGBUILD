pkgname=libva-intel-driver-hybrid-irql
pkgver=2.4.3
pkgrel=1
pkgdesc='VA-API implementation for Intel G45 and HD Graphics family (IRQL fork)'
arch=(x86_64)
url=https://01.org/linuxmedia/vaapi
license=(MIT)
depends=(
  libva
  libdrm
)
makedepends=(
  git
  meson
  xorgproto
)
optdepends=(
  'intel-hybrid-codec-driver: Provides codecs with partial HW acceleration'
)

replaces=(libva-driver-intel libva-intel-driver libva-intel-driver-irql libva-intel-driver-hybrid libva-intel-driver-hybrid-git)
conflicts=(libva-intel-driver libva-intel-driver-hybrid libva-intel-driver-irql libva-intel-driver-hybrid-git)
provides=(libva-intel-driver libva-intel-driver-hybrid libva-intel-driver-irql libva-intel-driver-hybrid-git)
source=(git+https://github.com/irql-notlessorequal/intel-vaapi-driver.git)
sha256sums=('SKIP')

pkgver() {
  cd intel-vaapi-driver
  cat VERSION
}

prepare() {
  cd intel-vaapi-driver

  # Only relevant if intel-gpu-tools is installed,
  # since then the shaders will be recompiled
  sed -i '1s/python$/&2/' src/shaders/gpp.py
}

build() {
  CFLAGS+=' -fcommon' # https://wiki.gentoo.org/wiki/Gcc_10_porting_notes/fno_common
  arch-meson -Denable_hybrid_codec=true intel-vaapi-driver build
  ninja -C build
}

package() {
  DESTDIR="${pkgdir}" meson install -C build
  install -Dm 644 intel-vaapi-driver/COPYING -t "${pkgdir}"/usr/share/licenses/libva-intel-driver
}

# vim: ts=2 sw=2 et:
