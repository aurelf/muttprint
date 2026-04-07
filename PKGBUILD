# Maintainer: jason ryan <jasonwryan@gmail.com>
# Contributor: Andreas Wagner <Andreas dot Wagner at em dot uni-frankfurt dot de>
# Contributor: Giorgio Lando <patroclo7@gmail.com>

pkgname=muttprint
pkgver=0.73_4
_mainver=0.73
pkgrel=8
arch=("i686" "x86_64")
pkgdesc="An app to print email from CLI mail clients, mutt in particular"
license=("GPL")
arch=("i686" "x86_64")
depends=('automake' 'texlive-latexextra' 'perl-timedate' 'perl-file-which' 
         'perl-text-iconv' 'psutils' 'dialog')
optdepends=('texlive-fontsextra:         Adds the CMBright font')
makedepends=('imagemagick' 'docbook-sgml' 'docbook-utils')
url="http://muttprint.sf.net"
backup=('etc/Muttprintrc')
source=(https://downloads.sf.net/$pkgname/$pkgname-$_mainver.tar.gz
        'muttprint_0.73-4.diff' 'regex.patch' 'two_edge.patch' 'filespeck.patch'
        'bool.patch' 'magick.patch' 'docparallel.patch' 'docsgml.patch')
sha256sums=('7cabe6a0aa59849f84914a2da33320611a2fcf5896b94ff957cfade8a325deb6'
            'f88d333722893bd932e15030c680bff6e35f6db4e65e0ca2f39b03902db761c3'
            'caec7cd37488c862e0db894826d87f92917e3fa3bb487e87845678c879671da2'
            'af56789584deb59a73295deb30c5de6d5d0829938920c63a6db2f54550ad5ce9'
            '207aed7ea63e79ccf5c49dd83cd957cbbd0558433f826ec505040df9421f39e8'
            'cfdefdab25eccf4e1edb8220103b79549787d26dc5b29f8a7728121c0515a406'
            '25a5ca0ba456ce208c605a73920e921ed34b2cbe4a9f6a0e40c7712156db84c1'
            '5a6aaa8053733cc5971da61768eb45ff25bacdf88a506ecfb324205430afaedf'
            '9aeb335c1682ca46f08bf30699868b19b9a35e3093fef564b0d6da159ddcce65')

prepare(){
   cd $pkgname-$_mainver
   patch -p1 < ../muttprint_0.73-4.diff
   patch -p1 < ../regex.patch
   patch -p1 < ../two_edge.patch
   patch -p1 < ../filespeck.patch
   patch -p1 < ../bool.patch
   patch -p1 < ../magick.patch
   patch -p1 < ../docparallel.patch
   patch -p1 < ../docsgml.patch
   # fix sample configs
   find . -type f -name 'sample*' -exec sed -i 's/-P$PRINTER/-p$PRINTER/' {} \;
   # convert images (and make pics/ build work)
   cd pics/ && \
     magick BabyTuX.eps -flop BabyTuX.eps
     for i in BabyTuX_color.eps BabyTuX.eps Debian_color.eps Debian.eps \
       Gentoo.eps Gentoo_color.eps ; do \
       magick $i $(basename $i .eps).png; \
     done && \
     magick penguin.eps penguin.jpg
}

build() {
   cd $pkgname-$_mainver

   aclocal
   automake --add-missing --copy
   autoconf
   ./configure --prefix=/usr
   make PREFIX=/usr
}

package() {
   cd $pkgname-$_mainver
   make PREFIX=/usr DESTDIR=$pkgdir install
   for i in README* CREDITS ChangeLog CHANGES AUTHORS ; do \
     install -m644 $i $pkgdir/usr/share/doc/muttprint/$i ; \
   done

   mkdir -p $pkgdir/etc
   install -m644 sample-muttprintrc-en 	 $pkgdir/etc/Muttprintrc
}

# vim:set ts=2 sw=2 et:
