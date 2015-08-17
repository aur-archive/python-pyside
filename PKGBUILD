# Maintainer: lilydjwg <lilydjwg@gmail.com>
# Contributor: Matthias Maennich <arch@maennich.net>
# Contributor: Massimiliano Torromeo <massimiliano.torromeo@gmail.com>
# Contributor: Jan-Erik Meyer-Luetgens <nyan at meyer-luetgens dot de>
pkgbase=python-pyside
pkgname=(python-pyside-common python2-pyside python-pyside)
_pkgrealname=pyside
pkgver=1.2.2
pkgrel=3
pkgdesc="Provides LGPL Qt bindings for Python and related tools for binding generation. Will build for both Python 2 and 3 versions."
arch=('i686' 'x86_64')
license=('LGPL')
url="http://qt-project.org/wiki/PySide"
_qtver=4.8
depends=('python' 'python2' "qt4>=${_qtver}" "shiboken>=$pkgver")
# you can remove qtwebkit and python below if you don't need them
makedepends=('cmake' 'qtwebkit' 'phonon')
optdepends=('qtwebkit: for PySide.QtWebKit'
            'phonon: for PySide.phonon')
source=("http://download.qt-project.org/official_releases/pyside/${_pkgrealname}-qt${_qtver}+$pkgver.tar.bz2")

build(){
    cd $srcdir/${_pkgrealname}-qt${_qtver}+$pkgver
    mkdir -p build_py3 && cd build_py3
    _pyverflags=$(python -c 'import sysconfig; print(sysconfig.get_config_var("SOABI"))')
    cmake .. -DCMAKE_INSTALL_PREFIX=/usr -DCMAKE_BUILD_TYPE=Release -DBUILD_TESTS=OFF \
      -DQT_PHONON_INCLUDE_DIR=/usr/include/qt4/phonon -DQT_QMAKE_EXECUTABLE=qmake-qt4 \
      -DPYTHON_SUFFIX=.${_pyverflags}
    make

    cd $srcdir/${_pkgrealname}-qt${_qtver}+$pkgver
    mkdir -p build_py2 && cd build_py2
    cmake .. -DCMAKE_INSTALL_PREFIX=/usr -DCMAKE_BUILD_TYPE=Release -DBUILD_TESTS=OFF \
      -DQT_PHONON_INCLUDE_DIR=/usr/include/qt4/phonon -DQT_QMAKE_EXECUTABLE=qmake-qt4
    make
}

package_python-pyside-common(){
    depends=()
    pkgdesc="Provides LGPL Qt bindings for Python and related tools for binding generation. Common Files."
    optdepends=()
    # cmake will use Python 3 version by default
    cd $srcdir/${_pkgrealname}-qt${_qtver}+$pkgver/build_py3
    make DESTDIR=$pkgdir install

    rm -rf "$pkgdir"/usr/lib/python* "$pkgdir"/usr/lib/libpyside.* \
      "$pkgdir"/usr/lib/cmake/PySide-$pkgver/PySideConfig*python*.cmake \
      "$pkgdir"/usr/lib/pkgconfig/pyside.pc
}

package_python-pyside(){
    depends=('python' "qt4>=${_qtver}" "shiboken>=$pkgver" "python-pyside-common=$pkgver")
    pkgdesc="Provides LGPL Qt bindings for Python and related tools for binding generation. Python 3 version."

    cd $srcdir/${_pkgrealname}-qt${_qtver}+$pkgver/build_py3
    make DESTDIR=$pkgdir install
    mv "$pkgdir"/usr/lib/pkgconfig/pyside.pc \
      "$pkgdir"/usr/lib/pkgconfig/pyside-py3.pc
    rm -rf "$pkgdir"/usr/include \
      "$pkgdir"/usr/lib/cmake/PySide-$pkgver/PySideConfig.cmake \
      "$pkgdir"/usr/lib/cmake/PySide-$pkgver/PySideConfigVersion.cmake \
      "$pkgdir"/usr/share
}

package_python2-pyside(){
    depends=('python2' "qt4>=${_qtver}" "shiboken>=$pkgver" "python-pyside-common=$pkgver")
    pkgdesc="Provides LGPL Qt bindings for Python and related tools for binding generation. Python 2 version."

    cd $srcdir/${_pkgrealname}-qt${_qtver}+$pkgver/build_py2
    make DESTDIR=$pkgdir install
    mv "$pkgdir"/usr/lib/pkgconfig/pyside.pc \
      "$pkgdir"/usr/lib/pkgconfig/pyside-py2.pc
    rm -rf "$pkgdir"/usr/include \
      "$pkgdir"/usr/lib/cmake/PySide-$pkgver/PySideConfig.cmake \
      "$pkgdir"/usr/lib/cmake/PySide-$pkgver/PySideConfigVersion.cmake \
      "$pkgdir"/usr/share
}

md5sums=('1969c2ff90eefaa4b200d234059d2287')
