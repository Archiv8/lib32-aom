#!/bin/bash

# Created from the original package by Rodrigo Bezerra, <rodrigobezerra21 at gmail dot com>

# Disable various shellcheck rules that produce false positives in this file.
# Repository rules should be added to the .shellcheckrc file located in the
# repository root directory, see https://github.com/koalaman/shellcheck/wiki
# and https://archiv8.github.io for further information.
# shellcheck disable=SC2034,SC2154

_relname=aom
pkgname=lib32-aom
pkgver=3.3.0
pkgrel=1
pkgdesc="Alliance for Open Media video codec (32-bit)"
url="https://aomedia.org/"
arch=(x86_64)
license=(BSD custom:PATENTS)
depends=(
    "lib32-glibc" "aom"
)
makedepends=(
    "cmake"
    "ninja"
    "yasm"
)
source=(
    https://storage.googleapis.com/aom-releases/libaom-$pkgver.tar.gz{,.asc}
    )
sha512sums=(
    '9bd118bf46d777da4e85f348fed95510ce583d16d005d062d33e2899f16f24bdb8b120792a7c77ccb64b4e1ff5b3d934342fb1b356bb426693ef69220f138c5f'
    'SKIP'
)
validpgpkeys=(
    B002F08B74A148DAA01F7123A48E86DB0B830498
) # AOMedia release signing key <av1-discuss@aomedia.org>

prepare() {
    cd lib${_relname}-${pkgver}
}

build() {
    export CC='gcc -m32'
    export CXX='g++ -m32'
    export PKG_CONFIG_PATH='/usr/lib32/pkgconfig'

    cmake -S lib${_relname}-${pkgver} -B build -G Ninja \
        -DCMAKE_INSTALL_PREFIX=/usr \
        -DCMAKE_INSTALL_LIBDIR=lib32 \
        -DBUILD_SHARED_LIBS=1 \
        -DENABLE_TESTS=0 \
        -DENABLE_DOCS=0

    cmake --build build
}

package() {
    DESTDIR="${pkgdir}" cmake --install build

    install -Dt "$pkgdir/usr/share/licenses/$pkgname" -m644 libaom-$pkgver/{LICENSE,PATENTS}

    cd "$pkgdir/usr"

    rm -r bin include
}
