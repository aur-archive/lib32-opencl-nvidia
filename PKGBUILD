# $Id$
# Contributor: Patryk Kowalczyk <patryk at kowalczyk dot ws>

_pkgbasename=opencl-nvidia
pkgname=lib32-$_pkgbasename
pkgver=319.17
pkgrel=1
pkgdesc="NVIDIA OpenCL drivers utilities and libraries (32-bit)" 
arch=('x86_64')
url="http://www.nvidia.com/"
depends=('libcl' 'zlib' 'opencl-nvidia' 'lib32-nvidia-utils')
optdepends=('opencl-headers: headers necessary for OpenCL development')
conflicts=('') 
provides=('lib32-opencl-nvidia')
license=('custom')
options=('!strip')

_arch='x86'
_pkg="NVIDIA-Linux-${_arch}-${pkgver}"
source=("ftp://download.nvidia.com/XFree86/Linux-${_arch}/${pkgver}/${_pkg}.run")

build() {
    cd "${srcdir}"
    rm -rf "${srcdir}/${_pkg}"
    sh ${_pkg}.run --extract-only
}

package() {
    cd "${srcdir}/${_pkg}"

    # OpenCL
    install -D -m755 libnvidia-opencl.so.${pkgver} "${pkgdir}/usr/lib32/libnvidia-opencl.so.${pkgver}"
    # create soname links
    for _lib in $(find "${pkgdir}" -name '*.so*'); do
        _soname="$(dirname ${_lib})/$(readelf -d "$_lib" | sed -nr 's/.*Library soname: \[(.*)\].*/\1/p')"
        if [ ! -e "${_soname}" ]; then
            ln -s "$(basename ${_lib})" "${_soname}"
            ln -s "$(basename ${_soname})" "${_soname/.[0-9]*/}"
        fi
    done

    rm -rf "${pkgdir}"/usr/{include,share,bin}
}
md5sums=('993eee683aea53b7965a853ccde3d740')
