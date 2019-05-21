# Maintainer: Markus Näther <naetherm@informatik.uni-freiburg.de>
pkgname=rocprim
pkgver=2.3.0
pkgrel=1
pkgdesc="ROCm Parallel Primitives"
arch=('x86_64')
url="https://github.com/ROCmSoftwarePlatform/rocPRIM"
license=('NCSAOSL')
depends=(hcc hip)
makedepends=(git cmake gcc make hcc python2 rocminfo)
srcver="2.3"
source=("https://github.com/ROCmSoftwarePlatform/rocPRIM/archive/$srcver.tar.gz")
sha256sums=("9dbc0c7714df00479cf94181ae5f120040377b1057aa7de89304d557fc66c8ff")

build() {
  mkdir -p "$srcdir/build"
  cd "$srcdir/build"

  # fix broken build with stack protection
  export CXXFLAGS=$(echo $CXXFLAGS | sed -e 's/-fstack-protector-strong//')
  export CFLAGS=$(echo $CFLAGS | sed -e 's/-fstack-protector-strong//')
  export CPPFLAGS=$(echo $CPPFLAGS | sed -e 's/-fstack-protector-strong//')

  # compile with HCC
  export CXX=/opt/rocm/hcc/bin/hcc

  # TODO: fix librocprim.so, it contains references to $srcdir
  cmake -DCMAKE_BUILD_TYPE=Release \
        -DCMAKE_INSTALL_PREFIX="$pkgdir/opt/rocm/rocprim" \
        -DBUILD_TEST=OFF \
        -G "Unix Makefiles" \
        "$srcdir/rocPRIM-$srcver"
  make 
}

package() {
  cd $srcdir/build
  make install

  mkdir -p $pkgdir/etc/ld.so.conf.d
  cat <<-EOF > $pkgdir/etc/ld.so.conf.d/rocprim.conf
    /opt/rocm/rocprim/lib/
		EOF
}
