# Maintainer: Severen Redwood <sev@severen.dev>
# Maintainer: Kevin Stolp <kevinstolp@gmail.com>
# Contributor: Federico Squartini <federico.squartini at gmail dot com>

pkgname=exercism
pkgver=3.5.5
pkgrel=1
pkgdesc="Command line tool for exercism.io"
arch=("x86_64")
url="https://github.com/exercism/cli"
license=("MIT")
depends=("glibc")
makedepends=("go")
source=("https://github.com/exercism/cli/archive/v${pkgver}.tar.gz")
sha256sums=('7e01a838877d6fc9faea5a6ad3834351b8e485acc9053bbdf59ce3ab3ce0d3b8')

prepare() {
  export GOPATH="$srcdir/.gopath"

  mkdir -p "$GOPATH/src/github.com/exercism"
  ln -sf "$srcdir/cli-$pkgver" "$GOPATH/src/github.com/exercism/cli"
}

build() {
  export GOPATH="$srcdir/.gopath"

  cd "$GOPATH/src/github.com/exercism/cli"

  go build \
    -trimpath \
    -buildmode pie \
    -ldflags "-linkmode external -extldflags \"${LDFLAGS}\"" \
    -mod=readonly \
    -modcacherw \
    -o out/exercism \
    exercism/main.go
}

package() {
  cd $srcdir/cli-${pkgver}

  install -D out/exercism "$pkgdir/usr/bin/exercism"

  install -D -m644 shell/exercism_completion.bash "$pkgdir/usr/share/bash-completion/completions/${pkgname}"
  install -D -m644 shell/exercism_completion.zsh "$pkgdir/usr/share/zsh/site-functions/_${pkgname}"
  install -D -m644 shell/exercism.fish "$pkgdir/usr/share/fish/vendor_completions.d/exercism.fish"

  install -D -m644 LICENSE "$pkgdir/usr/share/licenses/$pkgname/LICENSE"
}
