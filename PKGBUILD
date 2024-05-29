# https://gitlab.archlinux.org/archlinux/packaging/packages/telegram-desktop/-/blob/main/PKGBUILD
pkgname=telegram-desktop
pkgver=5.0.4
pkgrel=2
pkgdesc='Telegram Desktop client with some anti-features (sponsored messages, saving restrictions and other) disabled.'
arch=('x86_64')
url="https://github.com/Layerex/telegram-desktop-patches"
license=('GPL3')
depends=('hunspell' 'ffmpeg' 'hicolor-icon-theme' 'lz4' 'minizip' 'openal'
         'qt6-imageformats' 'qt6-svg' 'qt6-wayland' 'xxhash'
         'rnnoise' 'pipewire' 'libxtst' 'libxrandr' 'libxcomposite' 'libxdamage' 'abseil-cpp' 'libdispatch'
         'openssl' 'protobuf' 'glib2' 'libsigc++-3.0' 'kcoreaddons')
makedepends=('cmake' 'git' 'ninja' 'python' 'range-v3' 'tl-expected' 'microsoft-gsl' 'meson'
             'extra-cmake-modules' 'wayland-protocols' 'plasma-wayland-protocols' 'libtg_owt'
             'gobject-introspection' 'boost' 'fmt' 'mm-common' 'perl-xml-parser' 'python-packaging')
optdepends=('webkit2gtk: embedded browser features'
            'xdg-desktop-portal: desktop integration')
source=("https://github.com/telegramdesktop/tdesktop/releases/download/v${pkgver}/tdesktop-${pkgver}-full.tar.gz"
        "0001-Disable-sponsored-messages.patch"
        "0002-Disable-saving-restrictions.patch"
        "0003-Disable-invite-peeking-restrictions.patch"
        "0004-Disable-accounts-limit.patch")

sha512sums=('611bad9f174a35c27ffc55efe3544318f992af25a89364c3b64fa1afb4765ebf7aae7d4a83539d9e0038fe4ca8e637bdb5d453f058e64dffc2c45edf2f348dbb'
'643de210f33e1759bf31fc1b39d9681757419e05017d6c5ae3147d7c25f232d062737120ab694ac4dd34c3aa35a9110e74883c8b58d8aeee6a6b52721afb201a'
"4072e1c304fbb699d0d0cc42070480a3ac5580ba57bbc9b66debc030b04724fe5e7d02105c8ab270de2c4d3c58c1890040a03fe42cd329c934cfb087c1fab360"
"2c19b303ce77aa5b92dcbc46e61c0f45a5eb5fdb8810bd5f86a5d51acc4a79d6c41742d5197a0d72a6224e5f26855ab74ed35b5d085e8ba713cc9c87d8f54897"
"cba09b95960960f5657b5482389deb75abad8f4200f4809943e1ca873c19cf4caa99ef79f0ff32ecb17337e1b375523e310bc5e8843d13c8b3a5dff705ca9218")

prepare() {
    cd tdesktop-$pkgver-full
    patch --forward --strip=1 -i "${srcdir}/0001-Disable-sponsored-messages.patch"
    patch --forward --strip=1 -i "${srcdir}/0002-Disable-saving-restrictions.patch"
    patch --forward --strip=1 -i "${srcdir}/0003-Disable-invite-peeking-restrictions.patch"
    patch --forward --strip=1 -i "${srcdir}/0004-Disable-accounts-limit.patch"
}

build() {
    CXXFLAGS+=' -ffat-lto-objects'

    # Turns out we're allowed to use the official API key that telegram uses for their snap builds:
    # https://github.com/telegramdesktop/tdesktop/blob/8fab9167beb2407c1153930ed03a4badd0c2b59f/snap/snapcraft.yaml#L87-L88
    # Thanks @primeos!
    cmake -B build -S tdesktop-$pkgver-full -G Ninja \
        -DCMAKE_VERBOSE_MAKEFILE=ON \
        -DCMAKE_INSTALL_PREFIX="/usr" \
        -DCMAKE_BUILD_TYPE=Release \
        -DTDESKTOP_API_ID=611335 \
        -DTDESKTOP_API_HASH=d524b414d21f4d37f08684c1df41ac9c
    cmake --build build
}

package() {
    DESTDIR="$pkgdir" cmake --install build
}
