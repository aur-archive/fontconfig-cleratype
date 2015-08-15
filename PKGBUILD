# Maintainer: Tevin Zhang <mail2tevin {at} gmail {dot} com>
# Contributor: Andrea Fagiani <andfagiani {at} gmail {dot} com>
# Contributor: Biru Ionut <biru.ionut at gmail.com>
# Contributor: Paul Bredbury <brebs@sent.com>

# Installation order:  freetype2-ubuntu fontconfig-ubuntu libxft-ubuntu cairo-ubuntu
pkgname=fontconfig-cleratype
pkgver=2.10.1
_ubver=2.10.1-0ubuntu3.debian
pkgrel=3
pkgdesc="A library for configuring and customizing font access, with Ubuntu's LCD rendering patches."
arch=('i686' 'x86_64')
url="https://launchpad.net/ubuntu/precise/+source/fontconfig"
license=('custom')
depends=('expat>=2.0.1' 'freetype2-cleartype>=2.3.11')
conflicts=('fontconfig')
provides=("fontconfig=$pkgver")
options=('!libtool')
install=fontconfig.install
source=(http://archive.ubuntu.com/ubuntu/pool/main/f/fontconfig/fontconfig_$pkgver.orig.tar.bz2
        http://archive.ubuntu.com/ubuntu/pool/main/f/fontconfig/fontconfig_$_ubver.tar.gz)
        
md5sums=('e9b9142080b7cd6b43890344fec7fbd8'
         'fca38e76807462308a29aae57f2ec3f9')

build() {
  #patch -Np1 -i ../fontconfig_$_ubver.diff
  cd $srcdir/fontconfig-$pkgver

  for _f in $(cat ../debian/patches/series) ; do
     patch -Np1 -i "../debian/patches/$_f"
  done


  # make sure there's no rpath trouble and sane .so versioning - FC and Gentoo do this as well
  libtoolize -f
  autoreconf -f

  # Enable Position Independent Code for prelinking
  export CFLAGS="$CFLAGS -fPIC"
  ./configure --prefix=/usr \
    --sysconfdir=/etc \
    --with-templatedir=/etc/fonts/conf.avail \
    --with-xmldir=/etc/fonts \
    --localstatedir=/var \
    --disable-static \
    --with-default-fonts=/usr/share/fonts \
    --with-add-fonts=/usr/share/fonts
  make
  make DESTDIR=$pkgdir install

  #rm -f $pkgdir/etc/fonts/conf.d/*.conf

  # License
  install -Dm0644 COPYING $pkgdir/usr/share/licenses/$pkgname/COPYING

  # Docs
  install -Dm0644 $srcdir/debian/changelog $pkgdir/usr/share/doc/fontconfig/changelog

}

