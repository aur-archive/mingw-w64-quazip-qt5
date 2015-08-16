pkgname=mingw-w64-quazip-qt5
pkgver=0.7.1
pkgrel=1
pkgdesc="C++ wrapper for the Gilles Vollant's ZIP/UNZIP C package (Qt5 library) (mingw-w64)"
url="http://sourceforge.net/projects/quazip"
arch=(any)
depends=(mingw-w64-crt mingw-w64-qt5-base)
makedepends=(mingw-w64-gcc)
source=("http://downloads.sourceforge.net/project/quazip/quazip/$pkgver/quazip-$pkgver.tar.gz")
options=(!strip !buildflags staticlibs)
license=("LGPL")
md5sums=('3b99effb2a9417707d463e6f19cf2629')

_architectures="i686-w64-mingw32 x86_64-w64-mingw32"

build() {
  unset LDFLAGS
  
  for _arch in ${_architectures}; do
    for opt in static dll; do
      mkdir "${srcdir}/${_arch}-${opt}"
      cd "${srcdir}/${_arch}-${opt}"
      ${_arch}-qmake-qt5 ../quazip-$pkgver/quazip/quazip.pro \
        PREFIX="${pkgdir}/usr/${_arch}" \
        CONFIG+=${opt} \
        LIBS+=-lz
      make
    done
  done
}

package() {
  for _arch in ${_architectures}; do
    mkdir -p "${pkgdir}/usr/${_arch}/"{bin,lib,include}
    for opt in static dll; do
      cd "${srcdir}/${_arch}-${opt}"
      make install
    done
    find "${pkgdir}/usr/${_arch}" -name "*.exe" -o -name '*.prl' | xargs -rtl1 rm
		find "${pkgdir}/usr/${_arch}" -name "*.dll" -exec ${_arch}-strip --strip-unneeded {} \;
		find "${pkgdir}/usr/${_arch}" -name "*.a" -o -name "*.dll" | xargs -rtl1 ${_arch}-strip -g
    mv "${pkgdir}/usr/${_arch}/lib/"*.dll "${pkgdir}/usr/${_arch}/bin/"
  done
}
