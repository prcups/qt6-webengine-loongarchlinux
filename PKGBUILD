# Maintainer: Antonio Rojas <arojas@archlinux.org>
# Maintainer: Felix Yan <felixonmars@archlinux.org>

pkgname=qt6-webengine
_qtver=6.6.1
pkgver=${_qtver/-/}
pkgrel=2
arch=(x86_64)
url='https://www.qt.io'
license=(GPL3 LGPL3 FDL custom)
pkgdesc='Provides support for web applications using the Chromium browser project'
depends=(alsa-lib
         dbus
         expat
         ffmpeg
         fontconfig
         freetype2
         gcc-libs
         glib2
         glibc
         harfbuzz
         icu
         lcms2
         libdrm
         libevent
         libjpeg-turbo
         libpng
         libtiff
         libwebp
         libx11
         libxcb
         libxcomposite
         libxfixes
         libxkbcommon
         libxkbfile
         libxdamage
         libxext
         libxml2
         libxrandr
         libxslt
         libxtst
         mesa
         minizip
         nspr
         nss
         openjpeg2
         opus
         qt6-base
         qt6-declarative
         qt6-positioning
         qt6-webchannel
         snappy
         ttf-font
         zlib)
       # system libvpx disabled since https://codereview.qt-project.org/c/qt/qtwebengine/+/454908
       # libvpx pciutils re2
makedepends=(cmake
             gperf
             jsoncpp
             libepoxy
             ninja
             nodejs
             perf
             pipewire
             python-html5lib
             qt6-tools
             qt6-websockets)
optdepends=('pipewire: WebRTC desktop sharing under Wayland')
groups=(qt6)
_pkgfn=${pkgname/6-/}-everywhere-src-$_qtver
source=(https://download.qt.io/official_releases/qt/${pkgver%.*}/$_qtver/submodules/$_pkgfn.tar.xz
        libxml-2.12.patch
        icu-74.patch)
sha256sums=('7a6ea228214bd66029ca90549b29021f30f7544abff997b7f831ceac2ce73691'
            'bfae9e773edfd0ddbc617777fdd4c0609cba2b048be7afe40f97768e4eb6117e'
            '547e092f6a20ebd15e486b31111145bc94b8709ec230da89c591963001378845')

prepare() {
  patch -d $_pkgfn/src/3rdparty/chromium -p1 < libxml-2.12.patch
  patch -d $_pkgfn/src/3rdparty/chromium -p1 < icu-74.patch # Fix build with ICU 74 - patch from Alpine
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
  cmake --build build
}

package() {
  DESTDIR="$pkgdir" cmake --install build

  install -Dm644 "$srcdir"/${_pkgfn}/src/3rdparty/chromium/LICENSE "$pkgdir"/usr/share/licenses/${pkgname}/LICENSE.chromium
}
