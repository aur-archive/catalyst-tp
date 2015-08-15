# Contributor: Giuseppe Calderaro <giuseppecalderaro@gmail.com>
# Contributor: Alessandro Sagratini <ale_sagra@hotmail.com>
# Contributor: Vincent Viaud <lsuntzu@gmail.com>

pkgname=catalyst-tp
pkgver=8.5
_kernel_version=2.6.25
pkgrel=1
pkgdesc="Proprietary AMD/ATI kernel drivers for Radeon brand cards. For kernel26tp.."
arch=('i686' 'x86_64')
url="http://www.ati.amd.com"
license=('custom')
depends=("catalyst-utils>=${pkgver}" "kernel26tp>=${_kernel_version}")
makedepends=()
replaces=('ati-fglrx' 'fglrx') # Yay rebranding
install=${pkgname}.install
source=(http://www2.ati.com/drivers/linux/ati-driver-installer-${pkgver/./-}-x86.x86_64.run)
md5sums=('f0df93149fc44a8aacabcd9fc58e6d3c')

_kernver=${_kernel_version}-tp

build() {
    cd $startdir/src

    [ "$CARCH" = "i686" ] && _arch="x86"
    [ "$CARCH" = "x86_64" ] && _arch="x86_64"

    /bin/sh ./ati-driver-installer-${pkgver/./-}-x86.x86_64.run --extract archive_files

    cp $startdir/src/archive_files/arch/${_arch}/* $startdir/src/ -r
    cp $startdir/src/archive_files/common/* $startdir/src/ -r

    if [ "$CARCH" == "x86_64" ]; then
      cp $startdir/src/archive_files/x710_64a/* $startdir/src/ -r
    else
      cp $startdir/src/archive_files/x710/* $startdir/src/ -r
    fi
    cd $startdir/src
    cd $startdir/src/lib/modules/fglrx/build_mod/

    # Build the kernel module
    cp 2.6.x/Makefile .
    make -C /lib/modules/${_kernver}/build SUBDIRS="`pwd`" modules || return 1

    # Install the kernel module
    install -m 644 -D $startdir/src/lib/modules/fglrx/build_mod/fglrx.ko \
        $startdir/pkg/lib/modules/${_kernver}/video/fglrx.ko

    sed -i -e "s/KERNEL_VERSION=.*/KERNEL_VERSION=${_kernver}/g" $startdir/$install

    # install licenses
    install -m 0644 -D $startdir/src/archive_files/ATI_LICENSE.TXT \
                     $startdir/pkg/usr/share/licenses/${pkgname}/AMD_ATI_LICENSE.TXT

}