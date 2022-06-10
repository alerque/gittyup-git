# Maintainer: Fabio 'Lolix' Loli <lolix@disroot.org>
# Maintainer: Caleb Maclennan <caleb@alerque.com>
# Contributor: Tim Schumacher <timschumi@gmx.de>
# Contributor: KillWolfVlad <github.com/KillWolfVlad>
# Contributor: WaveHack <email@wavehack.net>
# Contributor: Whovian9369 <Whovian9369@gmail.com>

pkgname=gittyup-git
_pkgname=${pkgname%-git}
pkgver=1.1.1.r6.g6bd6d8a
pkgrel=1
pkgdesc='Graphical Git client (GitAhead fork)'
url="https://github.com/Murmele/${_pkgname^}"
arch=(x86_64)
license=(MIT)
depends=(cmark
         libssh2
         libsecret
         lua
         qt5-base)
makedepends=(cmake
             git
             hunspell
             libgit2
             libgnome-keyring
             ninja
             openssl
             qt5-tools
             qt5-translations)
optdepends=('git-lfs: git-lfs support'
            'libgnome-keyring: for GNOME Keyring for auth credentials'
            'qt5-translations: translations')
provides=("$_pkgname")
conflicts=("$_pkgname")
source=("$_pkgname::git+$url.git#branch=ArchBuild"
        "$_pkgname-cmark::git+https://github.com/commonmark/cmark.git"
        "$_pkgname-git::git+https://github.com/git/git.git"
        "$_pkgname-hunspell::git+https://github.com/hunspell/hunspell.git"
        "$_pkgname-libgit2::git+https://github.com/stinb/libgit2.git" # a fork, not the official upstream!
        "$_pkgname-libssh2::git+https://github.com/libssh2/libssh2.git"
        "$_pkgname-openssl::git+https://github.com/openssl/openssl.git"
        "$_pkgname-zip::git+https://github.com/kuba--/zip.git"
        "$_pkgname.desktop")
sha256sums=('SKIP'
            'SKIP'
            'SKIP'
            'SKIP'
            'SKIP'
            'SKIP'
            'SKIP'
            'SKIP'
            'fb900e4d101c19a5b3e1dc4c7619780e1d38fd78d0b9d951e794fbf36eebf21a')

pkgver() {
	cd "$_pkgname"
	git describe --long --tags --exclude latest |
		sed 's/^gittyup_v//;s/\([^-]*-g\)/r\1/;s/-/./g'
}

prepare() {
	cd "$_pkgname"
	git config advice.detachedHead false
	git submodule init
	git config submodule.test/dep/zip.url "$srcdir/$_pkgname-zip"
	git config submodule.dep/libgit2/libgit2.url "$srcdir/$_pkgname-libgit2"
	git config submodule.dep/git/git.url "$srcdir/$_pkgname-git"
	git config submodule.dep/libssh2/libssh2.url "$srcdir/$_pkgname-libssh2"
	git config submodule.dep/hunspell/hunspell.url "$srcdir/$_pkgname-hunspell"
	git config submodule.dep/openssl/openssl.url "$srcdir/$_pkgname-openssl"
	git config submodule.dep/cmark/cmark.url "$srcdir/$_pkgname-cmark"
	git submodule update
	sed -i -e 's/cmark_exe/cmark/' src/app/CMakeLists.txt
}

build() {
	cmake \
		-G Ninja \
		-W no-dev \
		-D CMAKE_BUILD_TYPE=None \
		-D CMAKE_INSTALL_PREFIX=/usr \
		-D CMAKE_INSTALL_DATADIR=share/$_pkgname \
		-D ENABLE_REPRODUCIBLE_BUILDS=ON \
		-D BUILD_SHARED_LIBS=OFF \
		-D BUILD_TESTS=OFF \
		-B build \
		-S "$_pkgname"
	ninja -C build
}

package() {
	cd "$_pkgname"
	DESTDIR="$pkgdir" ninja -C ../build install
	# rm -rf "$pkgdir/usr/lib/gittyup/"*.so.*
	# local _bin="$pkgdir/usr/lib/$_pkgname/${_pkgname^}"
	# install -Dm0755 "$_bin" "$pkgdir/usr/bin/$_pkgname"
	# rm "$_bin"
	# mv "$pkgdir/usr/"{lib/$_pkgname/Resources,share/$_pkgname}
	# rm -rf "$pkgdir/usr/lib/$_pkgname/"{include,lib/cmake}
	# install -Dm0644 -t "$pkgdir/usr/share/licenses/$_pkgname/" LICENSE.md
	# install -Dm0644 -t "$pkgdir/usr/share/applications/" ../$_pkgname.desktop
	# install -Dm0644 rsrc/Gittyup.iconset/gittyup_logo.svg "$pkgdir/usr/share/icons/hicolor/scalable/apps/gittyup.svg"
	# for s in 16x16 32x32 64x64 128x128 256x256 512x512; do
	#     install -Dm0644 "rsrc/Gittyup.iconset/icon_$s.png" "$pkgdir/usr/share/icons/hicolor/$s/apps/$_pkgname.png"
	# done
	# rm -rf "$pkgdir/usr/share/man" # libssh2 man pages
}
