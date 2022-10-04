# Maintainer: Antonio Rojas <arojas@archlinux.org>
# Maintainer: Felix Yan <felixonmars@archlinux.org>

pkgname=qt6-webengine
_qtver=6.4.0
pkgver=${_qtver/-/}
pkgrel=2
arch=(x86_64)
url='https://www.qt.io'
license=(GPL3 LGPL3 FDL custom)
pkgdesc='Provides support for web applications using the Chromium browser project'
depends=(qt6-webchannel qt6-positioning libxcomposite libxrandr libxkbfile 
         snappy nss libxslt minizip ffmpeg re2 libvpx libxtst ttf-font) # pciutils
makedepends=(cmake ninja python-html5lib gperf jsoncpp qt6-tools pipewire nodejs qt6-websockets libepoxy git)
optdepends=('pipewire: WebRTC desktop sharing under Wayland')
groups=(qt6)
#options=(debug) - hangs on stripping
_pkgfn=${pkgname/6-/}-everywhere-src-$_qtver
source=(https://download.qt.io/official_releases/qt/${pkgver%.*}/$_qtver/submodules/$_pkgfn.tar.xz
        qtbug-107144.patch::https://code.qt.io/cgit/qt/qtwebengine.git/patch/?id=64b7da9d)
sha256sums=('662ae9ec599b00e902e2c2751ae4977fa95bedf1b035033547ea1b1b69e75303'
            '8297f0375c8103828f3fd53bfd6005d009ae51cffa581c65cec93f098a6eb54e')

prepare() {
  patch -d $_pkgfn -p1 < qtbug-107144.patch
}

build() {
  cmake -B build -S $_pkgfn -G Ninja \
    -DCMAKE_MESSAGE_LOG_LEVEL=STATUS \
    -DCMAKE_TOOLCHAIN_FILE=/usr/lib/cmake/Qt6/qt.toolchain.cmake \
    -DQT_FEATURE_webengine_system_ffmpeg=ON \
    -DQT_FEATURE_webengine_system_icu=ON \
    -DQT_FEATURE_webengine_system_libevent=ON \
    -DQT_FEATURE_webengine_proprietary_codecs=ON \
    -DQT_FEATURE_webengine_kerberos=ON \
    -DQT_FEATURE_webengine_webrtc_pipewire=ON
#    -DQT_FEATURE_webengine_full_debug_info=ON
  cmake --build build
}

package() {
  DESTDIR="$pkgdir" cmake --install build

  install -Dm644 "$srcdir"/${_pkgfn}/src/3rdparty/chromium/LICENSE "$pkgdir"/usr/share/licenses/${pkgname}/LICENSE.chromium
}
