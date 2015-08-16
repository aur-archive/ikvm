pkgname=ikvm
pkgver=7.2.4630.5
pkgrel=3
pkgdesc="An implementation of Java for Mono."
arch=(i686 x86_64)
license=("custom:GPLv2")
depends=(mono java-environment)
makedepends=(nant dos2unix)
conflicts=(ikvm-cvs)
url="http://www.ikvm.net"
source=("http://downloads.sourceforge.net/ikvm/openjdk-7u6-b24-stripped.zip"
"http://downloads.sourceforge.net/ikvm/ikvmsrc-$pkgver.zip"
"ikvm-key"
"ikvmstub.sh"
"ikvmc.sh"
"ikvm.sh"
"ikvm.pc.in"
"ikvm.dll.config")
md5sums=('7f0c1d8b744dd59f114f46977d016e54'
         'd4b66fd2fcd53fa856f5bc1c0a0c39d1'
         '542db636108b996fd386cecc68c94d0e'
         '1f5a3a26f2daa098e594684fa10330f7'
         '5fe32856d71d9e804ffbceba05e1a670'
         'b3f145a87c124f1d19447be5eab52ea5'
         '8d9fc2977ac79b09c29ba402dcda6471'
         'b25dcece089a9fe6e7e516265835e89a')

build() {
	cd $srcdir
	find . -type f -exec dos2unix {} \;
	cd "$srcdir/ikvm-$pkgver"
	cp "$srcdir/ikvm-key" .
	ln -s "/usr/lib/mono/2.0/ICSharpCode.SharpZipLib.dll" "bin/ICSharpCode.SharpZipLib.dll"
	nant signed
	nant native
}

package() {
	cd "$srcdir/ikvm-$pkgver/bin"
	rm "ICSharpCode.SharpZipLib.dll"
	find . -name '*.dll' -o -name '*.exe' -o -name '*.mdb' -o -name '*.so' -o -name '*.config' |
		xargs -rtl1 -I {} install -Dm644 {} "$pkgdir/usr/lib/ikvm/"{}
	find "$srcdir/ikvm-$pkgver/openjdk" -name '*.jar' -o -name '*.zip' |
		xargs -rtl1 -I {} install -m644 {} "$pkgdir/usr/lib/ikvm/"
	install -m644 "$srcdir/ikvm-$pkgver/openjdk/assembly.class" "$pkgdir/usr/lib/ikvm/"
	install -m644 "../lib/ikvm-api.jar" "$pkgdir/usr/lib/ikvm/"
	install -m644 "../runtime/IKVM.Runtime.jar" "$pkgdir/usr/lib/ikvm/"
	install -m644 "$srcdir/ikvm.dll.config" "$pkgdir/usr/lib/ikvm/"
	install -Dm755 "$srcdir/ikvm.sh" "$pkgdir/usr/bin/ikvm"
	install -m755 "$srcdir/ikvmc.sh" "$pkgdir/usr/bin/ikvmc"
	install -m755 "$srcdir/ikvmstub.sh" "$pkgdir/usr/bin/ikvmstub"
	install -Dm644 "$srcdir/ikvm.pc.in" "$pkgdir/usr/lib/pkgconfig/ikvm.pc"
	sed -i "s|@VERSION@|$pkgver|" "$pkgdir/usr/lib/pkgconfig/ikvm.pc"
	install -Dm644 "../LICENSE" "$pkgdir/usr/share/licenses/ikvm/LICENSE"
	install -m644 "../THIRD_PARTY_README" "$pkgdir/usr/share/licenses/ikvm/"
	install -m644 "../TRADEMARK" "$pkgdir/usr/share/licenses/ikvm/"
	find "$pkgdir/usr/lib/ikvm/" -name '*.dll' | xargs -rtl1 -I {} gacutil -i {} -root "$pkgdir/usr/lib"
	#find "$pkgdir/usr/lib/ikvm/" -name '*.dll' -o -name '*.exe' | xargs -rtl1 mono --aot
	ln -s "/usr/lib/mono/2.0/ICSharpCode.SharpZipLib.dll" "ICSharpCode.SharpZipLib.dll"
}
