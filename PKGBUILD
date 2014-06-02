# Contributor: C Anthony Risinger
# Contributor: Michael Goehler
pkgname='mkinitcpio-btrfs'
pkgver=0.4.1
pkgrel=7
pkgdesc='mkinitcpio hook containing advanced features for btrfs-based root devices'
url='https://github.com/xtfxme/mkinitcpio-btrfs'
arch=('any')
license=('BSD')
install="${pkgname}.install"
depends=('mkinitcpio>=0.9.0' 'btrfs-progs' 'kexec-tools')
backup=('etc/default/btrfs_advanced')
source=('btrfs_hook' 'btrfs_install' 'btrfs_config')

package() {
    install -o root -g root -D ${srcdir}/btrfs_install ${pkgdir}/usr/lib/initcpio/install/btrfs_advanced
    install -o root -g root -D ${srcdir}/btrfs_hook    ${pkgdir}/usr/lib/initcpio/hooks/btrfs_advanced
    install -o root -g root -D ${srcdir}/btrfs_config  ${pkgdir}/etc/default/btrfs_advanced
}

md5sums=('fbc4f54f2aa8cab72cec018a28842e2c'
         '8198a307fe8a38195016e7265833473c'
         '73806b834c9fa8a8bb39d0201c63f21a')
