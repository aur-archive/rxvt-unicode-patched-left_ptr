# Contributor: emmanuelux

pkgname=rxvt-unicode-patched-left_ptr
_pkgname=rxvt-unicode
pkgver=9.15
pkgrel=1
pkgdesc="Unicode enabled rxvt-clone terminal emulator (urxvt) with fixed font spacing (width) and with mouse pointer XC_left_ptr in place of XC_xterm"
arch=('i686' 'x86_64')
url="http://software.schmorp.de/pkg/rxvt-unicode.html"
license=('GPL')
depends=('gcc-libs' 'libxft' 'gdk-pixbuf2')
optdepends=('perl: lots of utilities' 'gtk2-perl: to use the urxvt-tabbed')
source=(http://dist.schmorp.de/rxvt-unicode/$_pkgname-$pkgver.tar.bz2
        $_pkgname.desktop
        font-width-fix.patch)
sha1sums=('e6fdf091860ecb458730dc68b0176f67f207a2f7'
          'a61b126b40f2452400a073bb69725e669371db1d'
          '01ee8f212add79a158dcd4ed78d0ea1324bdc59b')
provides=(rxvt-unicode)
conflicts=(rxvt-unicode)

build() {
  cd "$srcdir/$_pkgname-$pkgver"
  patch -p0 -i ../font-width-fix.patch
  sed -i 's/XC_xterm/XC_left_ptr/' src/init.C
  ./configure --prefix=/usr \
    --with-terminfo=/usr/share/terminfo \
    --enable-256-color \
    --enable-font-styles \
    --enable-xim \
    --enable-keepscrolling \
    --enable-selectionscrolling \
    --enable-smart-resize \
    --enable-pixbuf \
    --enable-transparency \
    --enable-utmp \
    --enable-wtmp \
    --enable-lastlog \
    --disable-frills
  make
}

package() {
  cd "$srcdir/$_pkgname-$pkgver"
  install -d "$pkgdir/usr/share/terminfo"
  export TERMINFO="$pkgdir/usr/share/terminfo"
  make DESTDIR="$pkgdir" install
  # install the tabbing wrapper ( requires gtk2-perl! )
  sed -i 's/\"rxvt\"/"urxvt"/' doc/rxvt-tabbed
  install -Dm 755 doc/rxvt-tabbed "$pkgdir/usr/bin/urxvt-tabbed"
  # install freedesktop menu
  install -Dm644 ../$_pkgname.desktop \
    "$pkgdir/usr/share/applications/$_pkgname.desktop"
}
