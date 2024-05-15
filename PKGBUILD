# Maintainer: Laurent Laffont <laurent.laffont@gmail.com>
# Maintainer: Benoit Verhaeghe <badetitou@gmail.com>


pkgname=pharo-launcher
# Major version of this pkgbuild
epoch=1
# Version of Pharo Launcher
pkgver=3.1.1
# Revision of this pkgbuild
pkgrel=1
pkgdesc="Pharo Launcher helps you manage your Pharo images"
arch=(x86_64)
source=(PharoLauncher-linux-$pkgver.zip pharo.conf)
url="http://www.pharo-project.org/"
license=('MIT')
depends=('glibc' 'libgit2')
makedepends=('gendesk' 'libcurl-gnutls')

source=(
	"https://github.com/pharo-project/pharo-launcher/releases/download/$pkgver/PharoLauncher-linux-$pkgver-x64.zip"
	'local://pharo.conf'
	'local://launcher-patch.st'
)

md5sums=(
	'cedba3fa83d6a80418315df8c95473ad'
	'b2e30a17b522ee265fd57fe7c45592fc'
	'c74b8fc7c1e23c60b1b2030afaead26c'
)

prepare() {
	gendesk -n -f --pkgname "$pkgname" --pkgdesc "$pkgdesc" --categories "Development" --name "Pharo Launcher"
}


package() {
	cd $srcdir/

	mkdir -p $pkgdir/usr/share/pharo-launcher/
	mkdir -p $pkgdir/usr/share/pharo-vm/
	mkdir -p $pkgdir/usr/share/pixmaps/
	mkdir -p $pkgdir/usr/bin


	cp -fR $srcdir/pharolauncher/shared/* $pkgdir/usr/share/pharo-launcher/
	chmod -R 777 $pkgdir/usr/share/pharo-launcher/

	cp -fR $srcdir/pharolauncher/pharo-vm/* $pkgdir/usr/share/pharo-vm/
	chmod -R 777 $pkgdir/usr/share/pharo-vm/

	sed -i 's/ROOT=`dirname "$DIR"`/ROOT=\/usr\/share/' $srcdir/pharolauncher/pharo-launcher
	sed -i 's/LINUX="$ROOT\/bin"/LINUX="$ROOT\/pharo"/' $srcdir/pharolauncher/pharo-launcher
	sed -i 's/RESOURCES="$ROOT\/shared"/RESOURCES="\/usr\/share\/pharo-launcher"/' $srcdir/pharolauncher/pharo-launcher
	sed -i 's/ICONS="$ROOT\/icons"/ICONS="$ROOT\/pixmaps"/' $srcdir/pharolauncher/pharo-launcher

	cp -f $srcdir/pharolauncher/pharo-launcher $pkgdir/usr/bin/pharo-launcher
	chmod +x $pkgdir/usr/bin/pharo-launcher

	install -Dm644 $srcdir/pharolauncher/icons/pharo-launcher.png $pkgdir/usr/share/pixmaps/$pkgname.png
	install -Dm644 "$pkgname.desktop" "$pkgdir/usr/share/applications/$pkgname.desktop"

	# fix vm stuff
	mkdir -p $pkgdir/etc/security/limits.d/
	cp $srcdir/pharo.conf $pkgdir/etc/security/limits.d/pharo.conf

	$pkgdir/usr/share/pharo-vm/pharo --headless $pkgdir/usr/share/pharo-launcher/PharoLauncher.image st $srcdir/launcher-patch.st
}
