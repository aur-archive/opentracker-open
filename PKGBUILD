# Contributor: Pierre Schmitz <pierre@archlinux.de>

pkgname=opentracker-open
pkgver=20100418
pkgrel=1
pkgdesc='An open and free bittorrent tracker'
arch=('i686' 'x86_64')
url='http://erdgeist.org/arts/software/opentracker/'
license=('custom' 'GPL')
makedepends=('libowfat' 'cvs')
depends=('bash' 'zlib')
backup=('etc/opentracker/config')
source=('opentracker.rc.d' 'license.txt')
md5sums=('390bde9d010e79ab1309c02dcf9972e8'
         '865211941e882c0aff5d36a22ac9efaa')

_cvsroot=':pserver:anoncvs@cvs.erdgeist.org:/home/cvsroot'
_cvsmod='opentracker'

build() {
	cd "$srcdir"
	msg 'Connecting to CVS server....'
	if [ -d $_cvsmod/CVS ]; then
		cd $_cvsmod
		cvs -z3 update -d
	else
		cvs -z3 -d $_cvsroot co -D $pkgver -f $_cvsmod
		cd $_cvsmod
	fi

	msg 'CVS checkout done or server timeout'
	msg 'Starting make...'

	rm -rf "$srcdir/$_cvsmod-build"
	cp -r "$srcdir/$_cvsmod" "$srcdir/$_cvsmod-build"
	cd "$srcdir/$_cvsmod-build"

	sed 	-e 's#PREFIX?=..#PREFIX?=/usr#' \
		-e 's#LIBOWFAT_HEADERS=$(PREFIX)/libowfat#LIBOWFAT_HEADERS=$(PREFIX)/include#' \
		-e 's#LIBOWFAT_LIBRARY=$(PREFIX)/libowfat#LIBOWFAT_LIBRARY=$(PREFIX)/lib#' \
		-e 's/#FEATURES+=-DWANT_RESTRICT_STATS/FEATURES+=-DWANT_RESTRICT_STATS/' \
		-e 's/FEATURES+=-DWANT_FULLSCRAPE/#FEATURES+=-DWANT_FULLSCRAPE/' \
		-i Makefile
	sed 	-e 's|# tracker.rootdir /usr/local/etc/opentracker|tracker.rootdir /etc/opentracker|' \
		-i opentracker.conf.sample

	make || return 1
	install -D -m 755 opentracker $pkgdir/usr/bin/opentracker
	install -D -m 755 $srcdir/opentracker.rc.d $pkgdir/etc/rc.d/opentracker
	install -D -m 644 opentracker.conf.sample $pkgdir/etc/opentracker/config
	install -D -m 644 $srcdir/license.txt $pkgdir/usr/share/licenses/opentracker/license.txt
}
