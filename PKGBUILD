# Contributor: Corrado Primier <bardo@aur.archlinux.org>
# Maintainer: Laszlo Papp <djszapi2 at gmail com>
# Adapted for modern Linux distributions: <iuga . marian at gmail com>
pkgname=uisp_bbpg
pkgnameorig=uisp
pkgver=20050207
pkgrel=5
pkgdesc="A tool for AVR with added FTFI BitBang programmer for avrusb500 and avrusb500v2 self programming."
arch=('i686' 'x86_64' 'armv6h' 'armv7h')
url="http://savannah.nongnu.org/projects/uisp/"
license=('GPL')
depends=('gcc-libs' 'wget' 'libusb' 'libusb-compat')

source=(http://ftp.de.debian.org/debian/pool/main/u/${pkgnameorig}/${pkgnameorig}_${pkgver}.orig.tar.gz )
md5sums=('b1e499d5a1011489635c1a0e482b1627')

build() {
  cd "${srcdir}"
  rm -rf extractpatches
  wget -c -P ${srcdir}/.. http://http.debian.net/debian/pool/main/u/uisp/uisp_20050207-4.2.diff.gz
  wget -c -P ${srcdir}/.. http://www.tuxgraphics.org/common/src2/article07052/uisp-20050207-usb-bbpg-patch.txt
  mkdir ${srcdir}/extractpatches
  cp ../uisp_20050207.orig.tar.gz ${srcdir}/extractpatches
  cp ../uisp_20050207-4.2.diff.gz ${srcdir}/extractpatches
  cp ../uisp-20050207-usb-bbpg-patch.txt ${srcdir}/extractpatches
  cd "${srcdir}/extractpatches"
  tar xfvz uisp_20050207.orig.tar.gz
  gunzip uisp_20050207-4.2.diff.gz
  cd "${srcdir}/extractpatches/${pkgnameorig}-${pkgver}"
  patch -p1 -i ../uisp_20050207-4.2.diff
  cd "${srcdir}/${pkgnameorig}-${pkgver}"
  patch -p1 -i ${srcdir}/extractpatches/${pkgnameorig}-${pkgver}/debian/patches/10_const_char.dpatch || return 1
  patch -p1 -i ${srcdir}/extractpatches/${pkgnameorig}-${pkgver}/debian/patches/20_fix_manpage.dpatch || return 1
  patch -p1 -i ${srcdir}/extractpatches/${pkgnameorig}-${pkgver}/debian/patches/30_fix_g++_4.3.dpatch || return 1
  patch -p1 -i ${srcdir}/extractpatches/${pkgnameorig}-${pkgver}/debian/patches/40_fix_g++_4.6.dpatch || return 1
  patch -p1 -i ${srcdir}/extractpatches/uisp-20050207-usb-bbpg-patch.txt || return 1
  sed -i 's/busses = usb_get_busses();/busses = usb_get_busses();\n  for (busses = usb_busses; busses; busses = busses->next)\n{\n}\n  busses = usb_get_busses();/g' ${srcdir}/${pkgnameorig}-${pkgver}/src/ftdibb.c
  export CXXFLAGS="-g -Wall -O2 -Wno-narrowing -Wno-unused-result"
  ./configure --prefix=/usr --mandir=/usr/share/man --program-transform-name='s/uisp/uisp_bbpg/g'
  make || return 1
  cd "${srcdir}/.."
  wget -c -P ${srcdir}/.. http://tuxgraphics.org/common/src2/article07052/avrusb500v2-1.5.tar.gz
  tar xfvz avrusb500v2-1.5.tar.gz
}

package() {
  cd "${srcdir}/${pkgnameorig}-${pkgver}"
  echo "${pkgdir}"
  make DESTDIR="${pkgdir}" install
  mv ${pkgdir}/usr/share/doc/uisp-20050207 ${pkgdir}/usr/share/doc/uisp_bbpg-20050207
}
