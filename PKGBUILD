# Contributor: Cyker Way <cykerway at gmail dot com>
# Modified: Tomas Lindquist Olsen <tomas.l.olsen@gmail.com>

pkgname=cnijfilter-ip2600series
pkgver=2.90
pkgrel=1
pkgdesc="Canon IJ Printer Driver for Pixma IP2600 series (with cnijfilter-common290)"
url="http://fr.software.canon-europe.com/software/0035256.asp"
arch=('i686' 'x86_64')
license=('custom')
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

    rpmextract.sh cnijfilter-common-${pkgver}-${pkgrel}.i386.rpm
    rpmextract.sh ${pkgname}-${pkgver}-${pkgrel}.i386.rpm

    mv usr ${pkgdir}/
}
