# Maintainer: Gregory Eric Sanderson <gsanderson@avencall.com>
pkgname=xivo-client-qt-git
pkgver=20140325
pkgrel=1
pkgdesc="XIVO GUI client (qt)"
arch=(i386 x86_64)
url="https://github.com/xivo-pbx/xivo-client-qt"
license=('GPL')
depends=('qt5-base' 'desktop-file-utils')
makedepends=('git')
install=xivo-client-qt-git.install
md5sums=('a1572670b2cf63f652f77378202fd739') #generate with 'makepkg -g'

_gitroot=git://github.com/xivo-pbx/xivo-client-qt.git
_gitname=xivo-client-qt

build() {
  cd "$srcdir"
  msg "Connecting to GIT server...."

  if [[ -d "$_gitname" ]]; then
    cd "$_gitname" && git pull origin
    msg "The local files are updated."
  else
    git clone "$_gitroot" "$_gitname"
  fi

  msg "GIT checkout done or server timeout"
  msg "Starting build..."

  rm -rf "$srcdir/$_gitname-build"
  git clone "$srcdir/$_gitname" "$srcdir/$_gitname-build"
  cd "$srcdir/$_gitname-build"

  qmake-qt5
  make

}

package() {
  resources=${srcdir}/${_gitname}-build/packaging/resources

  mkdir -p $pkgdir/opt/xivoclient
  cp -r ${srcdir}/${_gitname}-build/bin/* $pkgdir/opt/xivoclient

  mkdir -p $pkgdir/usr/share/icons/
  cp ${resources}/xivoclient.png $pkgdir/usr/share/icons/

  mkdir -p $pkgdir/usr/share/applications/
  cp ${resources}/xivoclient.desktop $pkgdir/usr/share/applications/
  sed -i s/xivoicon/xivoclient/ $pkgdir/usr/share/applications/xivoclient.desktop

  mkdir -p $pkgdir/usr/bin
  install -m755 ${resources}/xivoclient.script $pkgdir/usr/bin/xivoclient
}

# vim:set ts=2 sw=2 et:
