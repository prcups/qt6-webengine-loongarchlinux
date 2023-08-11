# Maintainer: Antonio Rojas <arojas@archlinux.org>
# Maintainer: Felix Yan <felixonmars@archlinux.org>

pkgname=qt6-webengine
_qtver=6.5.2
pkgver=${_qtver/-/}
pkgrel=2
arch=(x86_64)
url='https://www.qt.io'
license=(GPL3 LGPL3 FDL custom)
pkgdesc='Provides support for web applications using the Chromium browser project'
depends=(qt6-webchannel qt6-positioning libxcomposite libxrandr libxkbfile 
         snappy nss libxslt minizip ffmpeg libvpx libxtst ttf-font) # pciutils
makedepends=(cmake ninja python-html5lib gperf jsoncpp qt6-tools pipewire nodejs qt6-websockets libepoxy git)
optdepends=('pipewire: WebRTC desktop sharing under Wayland')
groups=(qt6)
_pkgfn=${pkgname/6-/}-everywhere-src-$_qtver
source=(https://download.qt.io/official_releases/qt/${pkgver%.*}/$_qtver/submodules/$_pkgfn.tar.xz)
sha256sums=('e7c9438b56f502b44b4e376b92ed80f1db7c2c3881d68d319b0677afd5701d9f')

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
  cmake --build build
}

package() {
  DESTDIR="$pkgdir" cmake --install build

  install -Dm644 "$srcdir"/${_pkgfn}/src/3rdparty/chromium/LICENSE "$pkgdir"/usr/share/licenses/${pkgname}/LICENSE.chromium
}
