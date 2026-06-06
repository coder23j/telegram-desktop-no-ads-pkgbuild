# Based on https://gitlab.archlinux.org/archlinux/packaging/packages/telegram-desktop
pkgname=telegram-desktop-no-ads
pkgver=6.8.2
_td_commit=49b3bcbb6bfebf2ed44dd9f25102d2e1a94a58c4
pkgrel=1
pkgdesc='Patched Telegram Desktop client without ads, with premium features unlocked'
arch=('x86_64')
url="https://desktop.telegram.org/"
license=('GPL-3.0-or-later WITH OpenSSL-exception')
depends=(
  'abseil-cpp'
  'ada'
  'ffmpeg'
  'glib2'
  'glibc'
  'hicolor-icon-theme'
  'hunspell'
  'kcoreaddons'
  'libavif'
  'libdispatch'
  'libgcc'
  'libheif'
  'libjpeg-turbo'
  'libjxl'
  'libpipewire'
  'libstdc++'
  'libxcb'
  'libxcomposite'
  'libxdamage'
  'libxext'
  'libxfixes'
  'libxkbcommon'
  'libxrandr'
  'libxtst'
  'lz4'
  'minizip'
  'openal'
  'openh264'
  'openssl'
  'pipewire'
  'protobuf'
  'qt6-base'
  'qt6-imageformats'
  'qt6-svg'
  'qt6-wayland'
  'rnnoise'
  'xxhash'
  'zlib'
)
makedepends=(
  'boost'
  'boost-libs'
  'cmake'
  'git'
  'glib2-devel'
  'gobject-introspection'
  'gperf'
  'libtg_owt'
  'microsoft-gsl'
  'ninja'
  'python'
  'range-v3'
  'tl-expected'
)
optdepends=(
  'geoclue: geoinformation support'
  'crow-translate: translation provider'
  'webkit2gtk-4.1: embedded browser features provided by webkit2gtk-4.1'
  'webkitgtk-6.0: embedded browser features provided by webkitgtk-6.0 (Wayland only)'
  'xdg-desktop-portal: desktop integration'
)
provides=('telegram-desktop')
conflicts=('telegram-desktop')
source=(
  "https://github.com/telegramdesktop/tdesktop/releases/download/v${pkgver}/tdesktop-${pkgver}-full.tar.gz"
  "git+https://github.com/tdlib/td.git#tag=${_td_commit}"
  'tdesktop-fix-minizip-includes.patch'
  'remove-ads.patch'
  'local-default-premium.patch'
)
sha512sums=('a733992a12268ee4d429ed383f63182c12e3a5d61d78e0f31cbfc705a5a36cb872a2f2dfb6c76d50a22ed46d141b9c13f80da4ab94286fe35b339ca685d954e3'
           'f8f98b02b1c7d1ca9162c4867461605fa7a5ab449ac53701877f49ba393ff4a495a58984538fe3960c7090ab5b3749666b4d169058f5e40f8d35ea4c15aea8d5'
           'd9765588e92f154d83b95dc2840207bf22b26b6ca37b4d5cdfdb5e27a00c9e1ebcc9cd475a96bbcc5b02c24f6892320e009f843aa6b172a1820814b952a772eb'
           '79aa7f4749e32430957bbc2ffd61e9cb54602431872466a75c10041dd6b780b0657d2d2162157c6c929b7d2cc3f68a8674e331bb8e0365603d7f68c5441a4d0d'
           '91dc3fdab7052034cb5f9c25a36b32ffeea01f8dbbf4184a792f9ffb7b0360a711cb68be10d9d9d7b5b25b053f2e445d07260f17413c15bbf387b75342acff16')

prepare() {
  patch -Np1 -d "tdesktop-${pkgver}-full" -i "$srcdir/remove-ads.patch"
  patch -Np1 -d "tdesktop-${pkgver}-full" -i "$srcdir/local-default-premium.patch"
  patch -Np1 -d "tdesktop-${pkgver}-full/Telegram/lib_base" -i "$srcdir/tdesktop-fix-minizip-includes.patch"
}

build() {
  cmake -S td -B td/build \
    -DCMAKE_BUILD_TYPE=None \
    -DCMAKE_INSTALL_PREFIX="$PWD/td/install" \
    -Wno-dev \
    -DTD_E2E_ONLY=ON
  cmake --build td/build
  cmake --install td/build

  cmake -B build -S "tdesktop-${pkgver}-full" -G Ninja \
    -DCMAKE_VERBOSE_MAKEFILE=ON \
    -DCMAKE_INSTALL_PREFIX="/usr" \
    -Dtde2e_DIR="$PWD/td/install/lib/cmake/tde2e" \
    -DCMAKE_BUILD_TYPE=Release \
    -DTDESKTOP_API_ID=611335 \
    -DTDESKTOP_API_HASH=d524b414d21f4d37f08684c1df41ac9c
  cmake --build build
}

package() {
  DESTDIR="$pkgdir" cmake --install build
}
