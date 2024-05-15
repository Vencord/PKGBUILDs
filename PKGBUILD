# Maintainer: zt <zt@zt64.dev>
pkgname=vencord-desktop-git
pkgdesc="A standalone Electron app that loads Discord & Vencord"
pkgver=r320.a8d72fa
pkgrel=2

arch=("x86_64" "aarch64")
url="https://github.com/Vencord/Vesktop"
license=('GPL-3.0-only')

depends=(
  'alsa-lib'
  'cairo'
  'dbus'
  'gtk3'
  'glib2'
  'glibc'
  'libcups'
  'nss'
  'nspr'
  'pango'
)
makedepends=("nodejs>=18" "git")
optdepends=(
  'libnotify: Notifications'
  'xdg-utils: Open links, files, etc'
)

provides=("vencord")
conflicts=("vencord")

source=("$pkgname::git+$url.git" "vesktop.desktop")

sha256sums=('SKIP'
            '894ac515e31e2fe7e88ac771184cc783885706dced346470c5eb428302b7802c')

pkgver() {
  cd "$pkgname"
  printf "r%s.%s" "$(git rev-list --count HEAD)" "$(git rev-parse --short=7 HEAD)"
}

build() {
  cd "$pkgname"

  corepack pnpm i
  corepack pnpm build
  corepack pnpx electron-builder --dir
}

package() {
  cd "$srcdir"

  install -d "$pkgdir"/opt/vesktop
  cp -R "$pkgname/dist/linux-unpacked/." "$pkgdir/opt/vesktop"

  install -Dm 644 "vesktop.desktop" "$pkgdir/usr/share/applications/vesktop.desktop"
  install -Dm 644 "$pkgname/static/icon.png" "$pkgdir/usr/share/pixmaps/vesktop.png"
  install -Dm 644 "$pkgname/LICENSE" "$pkgdir/usr/share/licenses/$pkgname/LICENSE"

  install -d "$pkgdir"/usr/bin
  ln -s /opt/vesktop/vesktop "$pkgdir"/usr/bin/vesktop
}
