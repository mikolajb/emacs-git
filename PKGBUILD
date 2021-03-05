# Maintainer: Mikolaj Baranowski <mikolajb@gmail.com>

pkgname=emacs-git
pkgver=$(date +%Y%m%d)
pkgrel=1
pkgdesc="The extensible, customizable, self-documenting real-time display editor from its official Git repository"
arch=('x86_64')
url="http://www.gnu.org/software/emacs/"
license=('GPL3')
depends=('dbus' 'desktop-file-utils' 'libpng' 'libtiff' 'librsvg' 'giflib' 'gtk3' 'libxpm' 'libjpeg' 'hicolor-icon-theme' 'gpm' 'libotf' 'm17n-lib' 'alsa-lib' 'imagemagick' 'gnutls')


makedepends=('git' 'pkgconfig' 'texinfo')
provides=("emacs=$pkgver")
conflicts=('emacs' 'emacs-nox' 'emacs-otf' 'emacs-cvs' 'emacs-bzr')
install=$pkgname.install

source=("$pkgname"::'git+https://github.com/emacs-mirror/emacs.git')
md5sums=('SKIP')

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
    --with-sound \
    --with-xft \
    --with-x-toolkit=gtk3 \
    --with-cairo \
    --without-makeinfo \
    --with-wide-int \
    --with-modules \
    --with-harfbuzz

  make
}

package() {
  cd "$srcdir/$pkgname"

  make DESTDIR=${pkgdir} install

  msg "Cleaning up..."

  # remove conflict with ctags package
  mv $pkgdir/usr/bin/{ctags,ctags.emacs}
  mv $pkgdir/usr/share/man/man1/{ctags.1.gz,ctags.emacs.1}

  # This is mostly superfluous, and conflicts with texinfo
  rm $pkgdir/usr/share/info/info.info.gz
  rm $pkgdir/usr/share/info/dir

 # fix user/root permissions on usr/share files
  find $pkgdir/usr/share/emacs -type d -exec chmod 755 {} \;
  find $pkgdir/usr/share/emacs -exec chown root.root {} \;
}
