# Maintainer: Jesus Alvarez <jeezusjr at gmail dot com>
#
# This PKGBUILD was generated by the archzfs build scripts located at
#
# http://github.com/archzfs/archzfs
#
#
pkgname="zfs-utils-common-git"

pkgver=2018.03.28.r3390.13a2ff272
pkgrel=1
pkgdesc="Kernel module support files for the Zettabyte File System."
depends=("")
makedepends=("git")
arch=("x86_64")
url="http://zfsonlinux.org/"
source=("git+https://github.com/zfsonlinux/zfs.git#commit=13a2ff2727dd5fb46b08570279689373a7d5b26a"
        "zfs-utils.bash-completion-r1"
        "zfs-utils.initcpio.install"
        "zfs-utils.initcpio.hook")
sha256sums=("SKIP"
            "b60214f70ffffb62ffe489cbfabd2e069d14ed2a391fac0e36f914238394b540"
            "e33adabbe3f2f4866802c9d63c7810c7a42b4df2288d0cdd23376519b15b36e4"
            "3eb874cf2cbb6c6a0e1c11a98af54f682d6225667af944b43435aeabafa0112f")
license=("CDDL")
groups=("archzfs-linux-git")
provides=("zfs-utils")
install=zfs-utils.install
conflicts=('zfs-utils-common' 'zfs-utils-linux-git' 'zfs-utils-linux' 'zfs-utils-linux-lts' 'zfs-utils-linux-lts-git')
replaces=("zfs-utils-linux", "zfs-utils-linux-lts")
backup=('etc/zfs/zed.d/zed.rc' 'etc/default/zfs')

build() {
    cd "${srcdir}/zfs"
    ./autogen.sh
    ./configure --prefix=/usr --sysconfdir=/etc --sbindir=/usr/bin --with-mounthelperdir=/usr/bin \
                --libdir=/usr/lib --datadir=/usr/share --includedir=/usr/include \
                --with-udevdir=/lib/udev --libexecdir=/usr/lib/zfs-0.7.7 \
                --with-config=user --enable-systemd
    make
}

package() {
    cd "${srcdir}/zfs"
    make DESTDIR="${pkgdir}" install
    # Remove uneeded files
    rm -r "${pkgdir}"/etc/init.d
    rm -r "${pkgdir}"/usr/lib/dracut
    # move module tree /lib -> /usr/lib
    cp -r "${pkgdir}"/{lib,usr}
    rm -r "${pkgdir}"/lib
    # Autoload the zfs module at boot
    mkdir -p "${pkgdir}/etc/modules-load.d"
    printf "%s\n" "zfs" > "${pkgdir}/etc/modules-load.d/zfs.conf"
    # fix permissions
    chmod 750 ${pkgdir}/etc/sudoers.d
    # Install the support files
    install -D -m644 "${srcdir}"/zfs-utils.initcpio.hook "${pkgdir}"/usr/lib/initcpio/hooks/zfs
    install -D -m644 "${srcdir}"/zfs-utils.initcpio.install "${pkgdir}"/usr/lib/initcpio/install/zfs
    install -D -m644 "${srcdir}"/zfs-utils.bash-completion-r1 "${pkgdir}"/usr/share/bash-completion/completions/zfs
}
