# Contributor: Corrado Primier <bardo@aur.archlinux.org>
# Maintainer: Laszlo Papp <djszapi2 at gmail com>
pkgname=uisp
pkgnamewantedbin=uisp-bbpg
pkgver=20050207
pkgrel=5
pkgdesc="A tool for AVR which can interface to many hardware in-system programmers"
arch=('i686' 'x86_64' 'armv6h' 'armv7h')
url="http://savannah.nongnu.org/projects/uisp/"
license=('GPL')
depends=('gcc-libs' 'wget')

source=(http://ftp.de.debian.org/debian/pool/main/u/${pkgname}/${pkgname}_${pkgver}.orig.tar.gz )
md5sums=('b1e499d5a1011489635c1a0e482b1627')

build() {
  cd ${srcdir}
  rm -rf extractpatches
  wget -c -P ${srcdir}/.. http://http.debian.net/debian/pool/main/u/uisp/uisp_20050207-4.2.diff.gz
  wget -c -P ${srcdir}/.. http://www.tuxgraphics.org/common/src2/article07052/uisp-20050207-usb-bbpg-patch.txt
  mkdir ${srcdir}/extractpatches
  cp ../uisp_20050207.orig.tar.gz ${srcdir}/extractpatches
  cp ../uisp_20050207-4.2.diff.gz ${srcdir}/extractpatches
  cp ../uisp-20050207-usb-bbpg-patch.txt ${srcdir}/extractpatches
  cd ${srcdir}/extractpatches
  tar xfvz uisp_20050207.orig.tar.gz
  gunzip uisp_20050207-4.2.diff.gz
  cd ${srcdir}/extractpatches/${pkgname}-${pkgver}
  patch -p1 -i ../uisp_20050207-4.2.diff
# The 10 Debian patch is  different from the others, maybe needs to be the same
#  sed -i 's/--- uisp\/src\/Avr.h/--- uisp-20050207~\/src\/Avr.h/g' ${srcdir}/extractpatches/${pkgname}-${pkgver}/debian/patches/10_const_char.dpatch
#  sed -i 's/+++ uisp\/src\/Avr.h.new/+++ uisp-20050207\/src\/Avr.h/g' ${srcdir}/extractpatches/${pkgname}-${pkgver}/debian/patches/10_const_char.dpatch
  cd ${srcdir}/${pkgname}-${pkgver}
  patch -p1 -i ${srcdir}/extractpatches/${pkgname}-${pkgver}/debian/patches/10_const_char.dpatch || return 1
  patch -p1 -i ${srcdir}/extractpatches/${pkgname}-${pkgver}/debian/patches/20_fix_manpage.dpatch || return 1
  patch -p1 -i ${srcdir}/extractpatches/${pkgname}-${pkgver}/debian/patches/30_fix_g++_4.3.dpatch || return 1
  patch -p1 -i ${srcdir}/extractpatches/${pkgname}-${pkgver}/debian/patches/40_fix_g++_4.6.dpatch || return 1
  patch -p1 -i ${srcdir}/extractpatches/uisp-20050207-usb-bbpg-patch.txt || return 1
# Avoid busses not used error by using it for no reason
  sed -i 's/busses = usb_get_busses();/busses = usb_get_busses();\n  for (busses = usb_busses; busses; busses = busses->next)\n{\n}\n  busses = usb_get_busses();/g' ${srcdir}/${pkgname}-${pkgver}/src/ftdibb.c
  export CXXFLAGS="-g -Wall -O2 -Wno-narrowing -Wno-unused-result"
  ./configure --prefix=/usr --mandir=/usr/share/man
  make install || return 1
  cp ${srcdir}/${pkgname}-${pkgver}/src/uisp ${srcdir}/${pkgname}-${pkgver}
  mv ${srcdir}/${pkgname}-${pkgver}/src/uisp /usr/bin/${pkgnamewantedbin}
  cp ${srcdir}/${pkgname}-${pkgver}/uisp ${srcdir}/${pkgname}-${pkgver}/src
}

package() {
  cd ${srcdir}/${pkgname}-${pkgver}
  make install DEST_DIR=${pkgdir}
#  cp ${srcdir}/${pkgname}-${pkgver}/src/uisp ${srcdir}/${pkgname}-${pkgver}
#  mv ${srcdir}/${pkgname}-${pkgver}/src/uisp /usr/bin/${pkgnamewantedbin}
#  cp ${srcdir}/${pkgname}-${pkgver}/uisp ${srcdir}/${pkgname}-${pkgver}/src
  echo ""
  echo "Installed as /usr/bin/${pkgnamewantedbin}. Do not install the package."
  echo ""
  echo "Installed as /usr/bin/${pkgnamewantedbin}. Do not install the package."

  echo ""
  echo "Installed as /usr/bin/${pkgnamewantedbin}. Do not install the package."
  echo ""
  echo "Installed as /usr/bin/${pkgnamewantedbin}. Do not install the package."
  echo ""
}
