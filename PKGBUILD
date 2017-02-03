# $Id: PKGBUILD 177964 2016-06-05 11:30:41Z bluewind $
# Maintainer: Sébastien Luttringer
# Contributor: Angel Velasquez <angvp@archlinux.org>
# Contributor: tobias <tobias@archlinux.org>
# Contributor: dibblethewrecker dibblethewrecker.at.jiwe.dot.org

_pkgname=rxvt-unicode
pkgname=${_pkgname}-copy-patch
pkgver=9.22
pkgrel=2
pkgdesc="rxvt-unicode patched to copy with C-c when text is selected"
arch=('i686' 'x86_64')
url='http://software.schmorp.de/pkg/rxvt-unicode.html'
license=('GPL')
provides=($_pkgname)
conflicts=($_pkgname)
depends=('rxvt-unicode-terminfo')
makedepends=('libxft' 'perl' 'startup-notification')
source=(
  "http://dist.schmorp.de/rxvt-unicode/rxvt-unicode-$pkgver.tar.bz2"
  'urxvt.desktop'
  'urxvtc.desktop'
  'urxvt-tabbed.desktop'
  'copy.patch'
)
md5sums=('93782dec27494eb079467dacf6e48185'
         'fec94dc986fa37ec380079d81de3e0b2'
         'fac55f0a8404c86dad3e702146762332'
         '8a5599197568c63720e282b9722a7990'
         '6e9f4963fa948efa3cecbfef7e89d31f')


prepare() {
  cd $_pkgname-$pkgver
  patch -Np1 -i "${srcdir}/copy.patch"
}

build() {
  cd $_pkgname-$pkgver
  # we disable smart-resize (FS#34807)
  # do not specify --with-terminfo (FS#46424)
  ./configure \
    --prefix=/usr \
    --enable-256-color \
    --enable-combining \
    --enable-fading \
    --enable-font-styles \
    --enable-iso14755 \
    --enable-keepscrolling \
    --enable-lastlog \
    --enable-mousewheel \
    --enable-next-scroll \
    --enable-perl \
    --enable-pointer-blank \
    --enable-rxvt-scroll \
    --enable-selectionscrolling \
    --enable-slipwheeling \
    --disable-smart-resize \
    --enable-startup-notification \
    --enable-transparency \
    --enable-unicode3 \
    --enable-utmp \
    --enable-wtmp \
    --enable-xft \
    --enable-xim \
    --enable-xterm-scroll \
    --disable-pixbuf \
    --disable-frills
  make
}

package_rxvt-unicode-copy-patch() {
  pkgdesc='A unicode enabled rxvt-clone terminal emulator (urxvt)'
  depends=('rxvt-unicode-terminfo' 'libxft' 'perl' 'startup-notification')
  optdepends=('gtk2-perl: to use the urxvt-tabbed')

  # install freedesktop menu
  for _f in urxvt urxvtc urxvt-tabbed; do
    install -Dm644 $_f.desktop "$pkgdir/usr/share/applications/$_f.desktop"
  done
  cd $_pkgname-$pkgver

  # workaround terminfo installation
  export TERMINFO="$srcdir/terminfo"
  install -d "$TERMINFO"
  make DESTDIR="$pkgdir" install
  # install the tabbing wrapper ( requires gtk2-perl! )
  sed -i 's/\"rxvt\"/"urxvt"/' doc/rxvt-tabbed
  install -Dm 755 doc/rxvt-tabbed "$pkgdir/usr/bin/urxvt-tabbed"
}

# vim:set ts=2 sw=2 et:
