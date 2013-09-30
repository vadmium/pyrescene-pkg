# Maintainer: dryes <joswiseman@gmail>
pkgname='pyrescene-hg'
pkgver=235.530c172a8e68
pkgrel=1
pkgdesc='pyReScene is a port of ReScene .NET to the Python programming language.'
url='https://bitbucket.org/Gfy/pyrescene'
arch=('any')
license=('MIT')
depends=('python2' 'python2-numpy')
optdepends=(
  'chromaprint: Recreating MP3 and FLAC sample files'
  'unrar: Handling vobsub files'
)
makedepends=('mercurial' 'git')
conflicts=('awescript' 'rescene' 'resample')
provides=('awescript' 'rescene' 'resample')

_hgrepo='pyrescene'
_hgroot="${url%/*}"

_gitroot='git://github.com/dryes/rarlinux.git'
_gitname='rarlinux'

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
  
  cd "${srcdir}"
  
  msg 'Connecting to GIT server....'
  
  if [ -d "${_gitname}" ] ; then
    cd "${_gitname}" && git pull origin
    msg 'The local files are updated.'
  else
    git clone "${_gitroot}" "${_gitname}"
  fi
  
  msg 'GIT checkout done or server timeout'
}

package() {
  cd "${srcdir}/${_hgrepo}"
  
  cp 'rescene/srr.py' 'rescene/srr.py~'
  sed -i -r 's|(dest=\"rar_executable_dir\",)|\1 default=\"/opt/rarlinux\",|' 'rescene/srr.py'
  python2 'setup.py' install --root="${pkgdir}"
  mv 'rescene/srr.py~' 'rescene/srr.py'

  mkdir -p "${pkgdir}/opt/rarlinux"
  python2 'bin/preprardir.py' "${srcdir}/rarlinux" "${pkgdir}/opt/rarlinux"

  cp 'awescript/awescript.py' 'awescript/awescript.py~'
  sed -i -r 's|/usr/local/bin/sr([rs])|sr\1|ig' 'awescript/awescript.py'
  install -D -m755 "awescript/awescript.py" "${pkgdir}/usr/bin/awescript"
  mv 'awescript/awescript.py~' 'awescript/awescript.py'
}
