# maintainer: Jordan jonhnston <triplesquarednine@gail.com>
# Contributor: Rener Paiva <ren2r@mail.com> 

pkgname=gtk2-dragmenubar
pkgver=2.24.24
pkgrel=1
pkgdesc="The GTK+ Toolkit (v2) patched to support drag window by empty space in menubar and toolbars."
arch=('i686' 'x86_64')
url="http://www.gtk.org/"
makedepends=('atk' 'pango' 'libxcursor' 'libxinerama' 'libxrandr' 'libxi' 'libxcomposite' 'libxdamage' 'krb5' 'gnutls'
             'shared-mime-info' 'cairo' 'libcups' 'gdk-pixbuf2' 'gobject-introspection')
depends=('atk' 'pango' 'libxcursor' 'libxinerama' 'libxrandr' 'libxi' 'libxcomposite' 'libxdamage' 'krb5' 'gnutls' 'shared-mime-info' 'cairo' 'libcups' 'gtk-update-icon-cache')
provides=("gtk2=${pkgver}")
conflicts=('gtk2')
options=('!libtool' '!docs')
license=('LGPL')
install=gtk2.install

source=(http://ftp.gnome.org/pub/gnome/sources/gtk+/2.24/gtk+-${pkgver}.tar.xz
        xid-collision-debug.patch
        drag_menubar_and_toolbar.patch)
sha256sums=('12ceb2e198c82bfb93eb36348b6e9293c8fdcd60786763d04cfec7ebe7ed3d6d'
            'd758bb93e59df15a4ea7732cf984d1c3c19dff67c94b957575efea132b8fe558'
            '86c0d048ebce3b9f8513725ab631c2b5fc188e5fb640fa5ec2f13927d8fada44')

build() {
    cd "${srcdir}/gtk+-${pkgver}"
    patch -Np1 -i "${srcdir}/xid-collision-debug.patch"
    patch -Np1 -i "${srcdir}/drag_menubar_and_toolbar.patch"

    ./configure --prefix=/usr \
        --sysconfdir=/etc \
        --localstatedir=/var \
        --with-xinput=yes
    make
}
package() {

    backup=(etc/gtk-2.0/gtkrc)

    cd "${srcdir}/gtk+-${pkgver}"

    make DESTDIR="${pkgdir}" install
    sed -i "s#env python#env python2#" $pkgdir/usr/bin/gtk-builder-convert
    echo 'gtk-fallback-icon-theme = "gnome"' > "${pkgdir}/etc/gtk-2.0/gtkrc"
    #split this out to use with gtk3 too
    rm ${pkgdir}/usr/bin/gtk-update-icon-cache
}
