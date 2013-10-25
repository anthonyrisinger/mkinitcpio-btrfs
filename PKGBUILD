# Contributor: C Anthony Risinger
# Contributor: Michael Göhler
pkgname='mkinitcpio-btrfs'
pkgver=0.4.1
pkgrel=1
pkgdesc='mkinitcpio hook containing advanced features for btrfs-based root devices'
url='https://github.com/xtfxme/mkinitcpio-btrfs'
arch=('any')
license=('BSD')
install="${pkgname}.install"
depends=('mkinitcpio>=0.9.0' 'btrfs-progs' 'kexec-tools')
source=('btrfs_hook' 'btrfs_install' 'btrfs_config')

package() {
    install -o root -g root -D ${srcdir}/btrfs_install ${pkgdir}/usr/lib/initcpio/install/btrfs_advanced
    install -o root -g root -D ${srcdir}/btrfs_hook    ${pkgdir}/usr/lib/initcpio/hooks/btrfs_advanced
    install -o root -g root -D ${srcdir}/btrfs_config  ${pkgdir}/etc/default/btrfs_advanced
}

md5sums=('0b28de2e0ba9ce54c8fa4956fd051978'
         '8198a307fe8a38195016e7265833473c'
         '15ba7c02542cccba2c1c0c1d7f343a9f')
