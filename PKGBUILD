# Maintainer: Patrick Northon <northon_patrick3@yahoo.ca>

pkgname=mingw-w64-fmt
pkgver=7.1.3
pkgrel=3
pkgdesc="{fmt} is an open-source formatting library for C++. It can be used as a safe and fast alternative to (s)printf and iostreams. (mingw-w64)"
url="https://fmt.dev/"
license=("MIT")
depends=("mingw-w64-crt")
makedepends=("mingw-w64-cmake")
arch=("any")
options=(!strip !buildflags staticlibs)
optdepends=()
sha256sums=(
	"5cae7072042b3043e12d53d50ef404bbb76949dad1de368d7f993a15c8c05ecc"
)
source=(
	"https://github.com/fmtlib/fmt/archive/${pkgver}.tar.gz"
)

_architectures="x86_64-w64-mingw32"
_flags=( -Wno-dev -DCMAKE_BUILD_TYPE=Release -DCMAKE_CXX_FLAGS_RELEASE="-O2 -DNDEBUG" -DFMT_DOC=OFF )

prepare() {
	cd "fmt-${pkgver}"
	sed -i 's/class ostream final/class FMT_API ostream final/' 'include/fmt/os.h'
}

build() {
	for _arch in ${_architectures}; do
		${_arch}-cmake -S "fmt-${pkgver}" -B "build-${_arch}" "${_flags[@]}" -DFMT_TEST=OFF
		make -C "build-${_arch}"
	done
}

check() {
	for _arch in ${_architectures}; do
		${_arch}-cmake -S "fmt-${pkgver}" -B "build-${_arch}" "${_flags[@]}" -DFMT_TEST=ON
		make -C "build-${_arch}"
		make -C "build-${_arch}" test
	done
}

package() {
	for _arch in ${_architectures}; do
		make DESTDIR="${pkgdir}" -C "build-${_arch}" install
		${_arch}-strip --strip-unneeded "$pkgdir"/usr/${_arch}/bin/*.dll
	done
}
