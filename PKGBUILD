# Maintainer: lepokle <devel@leo.von-klenze.de>
pkgname='thunderbird-gnome-keyring'
pkgver=31.6.0
pkgrel=1
pkgdesc="Gnome-keyring integration for Thunderbird"
arch=('i686' 'x86_64')
url='http://github.com/lepokle/firefox-gnome-keyring'
license=('GPL')
depends=('thunderbird>=$pkgver' 'gnome-keyring')
#depends=('gnome-keyring')
makedepends=('zip' 'unzip' 'nspr>=4.10.8' 'libgnome-keyring' 'yasm' 'nss>=3.18')
provides=('thunderbird-gnome-keyring')
source=("${pkgname}-1.1.tar.gz::$url/tarball/v1.1"
        "ftp://ftp.mozilla.org/pub/mozilla.org/thunderbird/releases/$pkgver/source/thunderbird-$pkgver.source.tar.bz2"
        "mozconfig")

md5sums=('fb4aa263306b47d58937ddf80c8d1b67'
         '692e40f0070de37f3e84ccee4359e799'
         '0f72bb0f20f58ba0182ce060d3a7cef9')

_xulsrcdir="comm-esr31"
_xuldir="xulrunner"
_subdir="lepokle-firefox-gnome-keyring-31a2521"

prepare()
{
    cd "$srcdir/$_xulsrcdir"

    msg "Prepare XUL Runner..."
    cp "$srcdir/mozconfig" .mozconfig 
    echo "ac_add_options --prefix=\"$srcdir/$_xuldir\"" >> .mozconfig

    # configure script misdetects the preprocessor without an optimization level
    # https://bugs.archlinux.org/task/34644
    sed -i '/ac_cpp=/s/$CPPFLAGS/& -O2/' mozilla/configure
}


build()
{
    msg "Build XUL Runner..."
   
    export LDFLAGS="$LDFLAGS -Wl,-rpath,/usr/lib/xulrunner-$pkgver"
    export PYTHON="/usr/bin/python2"
        
    cd "$srcdir/$_xulsrcdir"
    make -j1 -f client.mk build MOZ_MAKE_FLAGS="$MAKEFLAGS"
    mkdir -p "$srcdir/$_xuldir"
    cd "$srcdir/$_xulsrcdir/obj-x86_64-unknown-linux-gnu/mozilla"
    make install
    
    msg "Building extension..."
    cd "${srcdir}/${_subdir}"
    export PKG_CONFIG_PATH="$srcdir/$_xuldir/lib/pkgconfig"
    make clean && make
}

package()
{
    cd "${srcdir}"

    mkdir -p ${pkgdir}/usr/lib/thunderbird/extensions
    unzip ${srcdir}/${_subdir}/mozilla-gnome-keyring-*.xpi -d ${pkgdir}/usr/lib/thunderbird/extensions/{6f9d85e0-794d-11dd-ad8b-0800200c9a66}
}

