# Maintainer: Mikolaj Baranowski <mikolajb@gmail.com>

pkgname=emacs-git
pkgver=$(date +%Y%m%d)
pkgrel=1
pkgdesc="The extensible, customizable, self-documenting real-time display editor from its official Git repository"
arch=('i686' 'x86_64')
url="http://www.gnu.org/software/emacs/"
license=('GPL3')
depends=('dbus' 'desktop-file-utils' 'libpng' 'libtiff' 'librsvg' 'giflib' 'gtk3' 'libxpm' 'libjpeg' 'hicolor-icon-theme' 'gpm' 'libotf' 'm17n-lib' 'gconf' 'alsa-lib' 'imagemagick' 'gnutls')


makedepends=('git' 'pkgconfig' 'texinfo')
provides=("emacs=$pkgver")
conflicts=('emacs' 'emacs-nox' 'emacs-otf' 'emacs-cvs' 'emacs-bzr')
install=$pkgname.install

source=("$pkgname"::'git://git.savannah.gnu.org/emacs.git')
md5sums=('SKIP')
_mandir=/usr/share/man

pkgver() {
  date +%Y%m%d
}

build() {
  cd "$srcdir/$pkgname"

  msg "Starting make..."
  ./autogen.sh && ./configure --prefix=/usr \
    --sysconfdir=/etc \
    --localstatedir=/var \
    --libexecdir=/usr/lib \
    --mandir=${_mandir} \
    --with-sound \
    --with-xft \
    --with-x-toolkit=gtk3 \
    --without-makeinfo

  make
}

package() {
  cd "$srcdir/$pkgname"

  make DESTDIR=${pkgdir} install

  msg "Cleaning up..."
  mv $pkgdir/usr/bin/{ctags,ctags.emacs}
  mv $pkgdir${_mandir}/man1/{etags.1,etags.emacs.1}.gz

  # This is mostly superfluous, and conflicts with texinfo
  rm $pkgdir/usr/share/info/info.info.gz
  rm $pkgdir/usr/share/info/dir

  find $pkgdir/usr/share/emacs -type d -exec chmod 755 {} \;
  find $pkgdir/usr/share/emacs -exec chown root.root {} \;
  chmod 775 $pkgdir/var/games
  chmod 775 $pkgdir/var/games/emacs
  chmod 664 $pkgdir/var/games/emacs/*
  chown -R root:50 $pkgdir/var/games
}
