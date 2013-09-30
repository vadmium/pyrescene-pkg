# Maintainer: dryes <joswiseman@gmail>
pkgname='pyrescene-hg'
pkgver=235.530c172a8e68
pkgrel=1
pkgdesc='Tools for backing up and restoring metadata from Rar files'
url='https://bitbucket.org/Gfy/pyrescene'
arch=('any')
license=('MIT')
depends=('python2' 'python2-numpy')
optdepends=(
  'chromaprint: Recreating MP3 and FLAC sample files'
  'unrar: Handling vobsub files'
)
makedepends=('mercurial')
conflicts=('awescript' 'rescene' 'resample')
provides=('awescript' 'rescene' 'resample')

_hgrepo='pyrescene'
_hgroot="${url%/*}"

pkgver() {
  cd "${srcdir}/${_hgrepo}"
  echo $(hg identify -n).$(hg identify -i) | sed 's|\+||g'
}

build() {
  cd "${srcdir}"
  
  msg 'Connecting to hg server...'
  
  if [ -d "${_hgrepo}/.hg" ]; then
    cd "${_hgrepo}" && hg pull -u
  else
    hg clone "${_hgroot}/${_hgrepo}"
  fi
}

package() {
  cd "${srcdir}/${_hgrepo}"
  
  python2 'setup.py' install --root="${pkgdir}"

  cp 'awescript/awescript.py' 'awescript/awescript.py~'
  sed -i -r 's|/usr/local/bin/sr([rs])|sr\1|ig' 'awescript/awescript.py'
  install -D -m755 "awescript/awescript.py" "${pkgdir}/usr/bin/awescript"
  mv 'awescript/awescript.py~' 'awescript/awescript.py'
  
  install -D -m644 COPYING "${pkgdir}/usr/share/licenses/${pkgname}/COPYING"
}
