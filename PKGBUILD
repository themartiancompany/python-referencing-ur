# SPDX-License-Identifier: AGPL-3.0
#
# Maintainer: Truocolo <truocolo@aol.com>
# Maintainer: Pellegrino Prevete (tallero) <pellegrinoprevete@gmail.com>
# Maintainer: George Rawlinson <grawlinson@archlinux.org>

_py="python"
_pyver="$( \
  "${_py}" \
    -V | \
    awk \
      '{print $2}')"
_pymajver="${_pyver%.*}"
_pyminver="${_pymajver#*.}"
_pynextver="${_pymajver%.*}.$(( \
  ${_pyminver} + 1))"
_pkg=referencing
pkgname="${_py}-${_pkg}"
pkgver=0.35.1
pkgrel=1
pkgdesc=(
  'An implementation-agnostic'
  'implementation of JSON reference resolution'
)
pkgdesc="${_pkgdesc[*]}"
arch=(
  'any'
)
url="https://${_pkg}.readthedocs.io"
license=(
  'MIT'
)
depends=(
  "${_py}"
  "${_py}-attrs"
  "${_py}-rpds-py"
)
makedepends=(
  'git'
  "${_py}-build"
  "${_py}-installer"
  "${_py}-hatchling"
  "${_py}-hatch-vcs"
)
checkdepends=(
  "${_py}-pytest"
  "${_py}-pytest-subtests"
  "${_py}-jsonschema"
)
_http="https://github.com"
_ns="${_py}-jsonschema"
_url="${_http}/${_ns}/${_pkg}"
source=(
  "${pkgname}::git+${_url}#tag=v${pkgver}"
  "github.com-${_ns}-${_pkg}-suite::git+${_http}/${_ns}/${_pkg}-suite"
)
sha512sums=(
  'c62e162e5e8f880f2dfbe944e17e664ceb4a266c2fccfcdc6cc236f6da604a97b7a74940e383aac41f4acf091fcb775264e6df4a208bba88c0ed3b7f07c1d5e2'
  'SKIP'
)
b2sums=(
  'a626c35b9963f0771e615047c5da5edfe6bf4af9c395a6cb919c374ce446ff81f75ec7783d567371dcbc47c1f8d1bf7deeece3c83267c3160c9899806c0db41f'
  'SKIP'
)

prepare() {
  cd \
    "${pkgname}"
  # prepare git submodules
  git \
    submodule \
    init
  git \
    config \
    submodule.suite.url \
    "${srcdir}/github.com-${_ns}-${_pkg}-suite"
  git \
    -c \
      protocol.file.allow=always \
    submodule \
      update
}

build() {
  cd \
    "${pkgname}"
  python \
    -m \
      build \
    --wheel \
    --no-isolation
}

check() {
  cd \
    "${pkgname}"
  pytest \
    -v
}

package() {
  cd \
    "${pkgname}"
  python \
    -m \
      installer \
    --destdir="${pkgdir}" \
    dist/*.whl

  # symlink license file
  local \
    site_packages=$( \
      python \
        -c \
	"import site; print(site.getsitepackages()[0])")
  install \
    -d \
    "${pkgdir}/usr/share/licenses/${pkgname}"
  ln \
    -s \
    "${site_packages}/${_pkg}-${pkgver}.dist-info/licenses/COPYING" \
    "${pkgdir}/usr/share/licenses/${pkgname}/COPYING"
}

