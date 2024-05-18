# Maintainer: Laurent Laffont <laurent.laffont@gmail.com>
# Maintainer: Benoit Verhaeghe <benoit@badetitou.fr>

pkgname=pharo-launcher
# Major version of this pkgbuild
epoch=1
# Version of Pharo Launcher
pkgver=3.1.1
# Revision of this pkgbuild
pkgrel=3
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
	'https://files.pharo.org/media/logo/icon-opaque-512x512.png'
)

md5sums=(
	'cedba3fa83d6a80418315df8c95473ad'
	'b2e30a17b522ee265fd57fe7c45592fc'
	'c74b8fc7c1e23c60b1b2030afaead26c'
	'b8323c1d7421dce18db921ae83ff8409'
)

prepare() {
	# Icon for the launcher
	gendesk -n -f --icon "pharo-launcher" --pkgname "$pkgname" --pkgdesc "$pkgdesc" --categories "Development" --name "Pharo Launcher"
	# Icon for the vm
	gendesk -n -f --icon "pharo" --pkgname "pharo" --pkgdesc "A Pharo VM executed" --categories "Development" --name "Pharo" --custom="NoDisplay=true"
}


package() {
	cd $srcdir/

	mkdir -p $pkgdir/usr/share/pharo-launcher/
	mkdir -p $pkgdir/usr/share/pharo-vm/
	mkdir -p $pkgdir/usr/share/pixmaps/
	mkdir -p $pkgdir/usr/bin

	sed -i 's/ROOT=`dirname "$DIR"`/ROOT=\/usr\/share/' $srcdir/artifacts/PharoLauncher-linux-3.1.1-x64/pharo-launcher
	sed -i 's/LINUX="$ROOT\/bin"/LINUX="$ROOT\/pharo"/' $srcdir/artifacts/PharoLauncher-linux-3.1.1-x64/pharo-launcher
	sed -i 's/RESOURCES="$ROOT\/shared"/RESOURCES="\/usr\/share\/pharo-launcher"/' $srcdir/artifacts/PharoLauncher-linux-3.1.1-x64/pharo-launcher
	sed -i 's/ICONS="$ROOT\/icons"/ICONS="$ROOT\/pixmaps"/' $srcdir/artifacts/PharoLauncher-linux-3.1.1-x64/pharo-launcher
	# Rename executable of the vm for the icon
	sed -i 's/$LINUX\/pharo/$LINUX\/pharo-launcher/' $srcdir/artifacts/PharoLauncher-linux-3.1.1-x64/pharo-launcher
	sed -i 's/$BIN\/pharo/$BIN\/pharo-launcher/' $srcdir/artifacts/PharoLauncher-linux-3.1.1-x64/shared/pharo-vm/pharo
	sed -i 's/$BIN\/pharo/$BIN\/pharo-launcher/' $srcdir/artifacts/PharoLauncher-linux-3.1.1-x64/shared/pharo-vm/bin/pharo
	# Rename library of the vm for the icon
	cp -fR $srcdir/artifacts/PharoLauncher-linux-3.1.1-x64/shared/* $pkgdir/usr/share/pharo-launcher/
	cp -fR $srcdir/artifacts/PharoLauncher-linux-3.1.1-x64/shared/pharo-vm/* $pkgdir/usr/share/pharo-vm/
	mv $pkgdir/usr/share/pharo-vm/pharo $pkgdir/usr/share/pharo-vm/pharo-launcher
	mv $pkgdir/usr/share/pharo-vm/bin/pharo $pkgdir/usr/share/pharo-vm/bin/pharo-launcher
	mv $pkgdir/usr/share/pharo-vm/lib/pharo $pkgdir/usr/share/pharo-vm/lib/pharo-launcher
	chmod -R 777 $pkgdir/usr/share/pharo-vm/
	chmod -R 777 $pkgdir/usr/share/pharo-launcher/

	cp -f $srcdir/artifacts/PharoLauncher-linux-3.1.1-x64/pharo-launcher $pkgdir/usr/bin/pharo-launcher
	chmod +x $pkgdir/usr/bin/pharo-launcher

	install -Dm644 $srcdir/artifacts/PharoLauncher-linux-3.1.1-x64/pharo-launcher.png $pkgdir/usr/share/pixmaps/$pkgname.png
	install -Dm644 $srcdir/icon-opaque-512x512.png $pkgdir/usr/share/pixmaps/pharo.png
	install -Dm644 "$pkgname.desktop" "$pkgdir/usr/share/applications/$pkgname.desktop"
	install -Dm644 "pharo.desktop" "$pkgdir/usr/share/applications/pharo.desktop"

	# fix vm stuff
	mkdir -p $pkgdir/etc/security/limits.d/
	cp $srcdir/pharo.conf $pkgdir/etc/security/limits.d/pharo.conf

	$pkgdir/usr/share/pharo-vm/pharo-launcher --headless $pkgdir/usr/share/pharo-launcher/PharoLauncher.image st $srcdir/launcher-patch.st
}
