# https://bbs.archlinux.org/viewtopic.php?id=84568

pkgname=cnijfilter-ip2600series
pkgver=2.90
pkgrel=1
pkgdesc="Canon IJ Printer Driver for Pixma IP2600 series (with cnijfilter-common290)"
url="http://fr.software.canon-europe.com/software/0035256.asp"
arch=('i686' 'x86_64')
license=('custom')

#if [ "${CARCH}" = 'x86_64' ]; then
  #depends=('lib32-libcups' 'lib32-popt' 'lib32-libpng12' 'lib32-libtiff' 'lib32-gtk2' 'lib32-libxml2')
#elif [ "${CARCH}" = 'i686' ]; then
  #depends=('libcups' 'popt' 'libpng12' 'libtiff')
#fi
# deps from https://aur.archlinux.org/cgit/aur.git/tree/PKGBUILD?h=cnijfilter-common
if [ "${CARCH}" = 'x86_64' ]; then
    depends_x86_64=('lib32-libcups' 'lib32-popt' 'lib32-libusb' 'lib32-libxml2')
elif [ "${CARCH}" = 'i686' ]; then
    depends_i686=('libcups' 'popt' 'libusb' 'libxml2')
fi
makedepends=('rpmextract')

source=(iP2600_RPM_Drivers.tar)
md5sums=('b6ca33e0f8383acdb3639c53a82319b2')

package() {
  cd "${srcdir}"
  
  # Extract packages
  rpmextract.sh cnijfilter-common-${pkgver}-${pkgrel}.i386.rpm
  rpmextract.sh ${pkgname}-${pkgver}-${pkgrel}.i386.rpm
  
  # Move licenses
  mkdir -p usr/share/licenses/${pkgname}
  mv usr/share/doc/cnijfilter-common-${pkgver}/* usr/share/licenses/${pkgname}
  rm -rf usr/share/doc
  
  # Move stuff out of usr/local
  mkdir usr/bin
  mv usr/local/bin/* usr/bin
  mv usr/local/share/* usr/share
  rm -rf usr/local
  
  # Move the ppd file
  mkdir usr/share/ppd
  mv usr/share/cups/model/* usr/share/ppd
  rm -rf usr/share/cups
  
  # Check permissions
  chmod -R a+rX usr/
  
  # Instal into fakeroot
  mv usr ${pkgdir}/
}
