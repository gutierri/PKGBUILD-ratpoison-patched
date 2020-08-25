# Maintainer: Gutierri Barboza <me@gutierri.me>

_pkgname=ratpoison
pkgname=ratpoison-git
pkgver=1.4.10.beta
pkgrel=1
pkgdesc='Say good-bye to the rodent'
arch=('x86_64')
license=('GPL2')
url='https://www.nongnu.org/ratpoison/'
depends=('libxinerama' 'readline' 'bash' 'perl' 'libxtst' 'libxft' 'texinfo' 'libxrandr')
conflicts=('ratpoison')
source=("git+https://git.savannah.nongnu.org/git/ratpoison.git"
        "${_pkgname}.desktop")
md5sums=("SKIP"
         "1324efe7bebef62faf366031d93f1283")

prepare() {
  cd "${srcdir}/${_pkgname}"

  git apply ../../patchs/001-add-removeleft-up-right-down-commands.patch
  git apply ../../patchs/002-manpage-layout-fixes.patch
  git apply ../../patchs/003-ignorehints.patch
  git apply ../../patchs/004-EWMH.patch
  git apply ../../patchs/005-EWMH.patch
  git apply ../../patchs/006-EWMH.patch
  git apply ../../patchs/007-EWMH.patch

}

build() {
  cd "${srcdir}/${_pkgname}"

  # FS#38726, v1.4.6-2 
  sed -i 's|PRINT_ERROR (("XGetWMName|PRINT_DEBUG (("XGetWMName|' src/manage.c

  ./autogen.sh
  ./configure --prefix=/usr
  make CFLAGS="$CFLAGS -DHAVE_GETLINE"
}

package() {
  cd "${srcdir}/${_pkgname}"
  make DESTDIR="${pkgdir}" install

  # fix permissions
  chmod a+x "${pkgdir}/usr/share/ratpoison/"{allwindows.sh,clickframe.pl,rpshowall.sh,rpws,split.sh}

  # Not useful outside the source tree.
  rm "${pkgdir}/usr/share/ratpoison/genrpbindings"

  cd contrib
  ./genrpbindings
  install -dm755 "${pkgdir}/usr/share/ratpoison/bindings"
  install -m644 {Ratpoison.pm,ratpoison-cmd.el,ratpoison.rb,ratpoison.lisp,ratpoison.py} \
    "${pkgdir}/usr/share/ratpoison/bindings/"

  install -Dm644 "${srcdir}/${_pkgname}.desktop" \
    "${pkgdir}/etc/X11/sessions/${_pkgname}.desktop"
}
