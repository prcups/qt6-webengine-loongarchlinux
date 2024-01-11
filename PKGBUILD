# Maintainer: Antonio Rojas <arojas@archlinux.org>
# Maintainer: Felix Yan <felixonmars@archlinux.org>

pkgname=qt6-webengine
_qtver=6.7.0-beta1
pkgver=${_qtver/-/}
pkgrel=1
arch=(loong64)
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
source=(https://download.qt.io/development_releases/qt/${pkgver%.*}/$_qtver/submodules/$_pkgfn.tar.xz
        libxml-2.12.patch
        icu-74.patch
		chromium_loongarch_support.patch
		gn_loongarch_support.patch
		webengine_loongarch_support.patch
		https://krgm.moe/static/llvm-11.0.tar.gz)
sha256sums=('7de613158bbbb5477a83dfe6daac4241e2ac172b3179f79ac232fbac8690f030'
            'bfae9e773edfd0ddbc617777fdd4c0609cba2b048be7afe40f97768e4eb6117e'
            '547e092f6a20ebd15e486b31111145bc94b8709ec230da89c591963001378845'
			'b8fbba09cf10d3e16db1a913b7154d38513b24271b360f657cf816c6e596fd1f'
			'17c312fdd24b33f82442ec91504cf94a40735985a52123e4cf941665a235674d'
			'bebe55ec3b479929a8057ff7273e0b02ad27d8c888497487dc159aff43aae796'
			'8f545e4c4d41053b257cd3a583ac97fce415faf407d762012b520709ba6e23be')

prepare() {
  patch -d $_pkgfn/src/3rdparty/chromium -p1 < libxml-2.12.patch
  patch -d $_pkgfn/src/3rdparty/chromium -p1 < icu-74.patch # Fix build with ICU 74 - patch from Alpine
  patch -d $_pkgfn/ -p1 < chromium_loongarch_support.patch 
  patch -d $_pkgfn/ -p1 < gn_loongarch_support.patch
  patch -d $_pkgfn/ -p1 < webengine_loongarch_support.patch
  mv llvm-11.0 $_pkgfn/src/3rdparty/chromium/third_party/swiftshader/third_party/
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
