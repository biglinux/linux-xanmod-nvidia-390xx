# Maintainer: Bernhard Landauer <bernhard[at]manjaro[dot]org>
# Maintainer: Philip Müller <philm[at]manjaro[dot]org>
# Contributor: Gerd Röthig (DAC24)
# Contributer : Thomas Baechler <thomas@archlinux.org>
# Contributor: Alonso Rodriguez <alonsorodi20 (at) gmail (dot) com>
# Contributor: Sven-Hendrik Haase <sh@lutzhaase.com>
# Contributor: Felix Yan <felixonmars@archlinux.org>
# Contributor: loqs
# Contributor: Dede Dindin Qudsy <xtrymind+gmail+com>
# Contributor: Ike Devolder <ike.devolder+gmail+com>

_linuxprefix=linux-xanmod

pkgname="${_linuxprefix}-nvidia-390xx"
pkgdesc="NVIDIA drivers for linux"
pkgver=390.157
pkgrel=610111
arch=('x86_64')
url="http://www.nvidia.com/"
license=('custom')
groups=("${_linuxprefix}-extramodules")
depends=("${_linuxprefix}" "nvidia-utils=${pkgver}")
makedepends=("${_linuxprefix}-headers")
provides=("nvidia=${pkgver}" 'NVIDIA-MODULE')
options=(!strip)
_durl="https://us.download.nvidia.com/XFree86/Linux-x86"
source=("${_durl}_64/${pkgver}/NVIDIA-Linux-x86_64-${pkgver}-no-compat32.run"
        'kernel-4.16+-memory-encryption.patch'
        'kernel-6.2.patch'
        'kernel-6.3.patch'
        'kernel-6.4.patch'
        'kernel-6.5.patch'
        'kernel-6.6.patch'
        'kernel-6.8.patch'
        'kernel-6.10.patch'
        'gcc14.patch')
sha256sums=('162317a49aa5a521eb888ec12119bfe5a45cec4e8653efc575a2d04fb05bf581'
            '6c5f5b11dbb43f40f4e2c6a2b5417f44b50cf29d16bbd091420b7e737acb6ccd'
            'a94d34cda96d443d02d992ee7962ce7c9949134b899e366fc3dafaf48bc19ebe'
            'd380ee05adc4cf2aeb673b72327fe6a4b3a43f7d1bb1823084228129e31e6c59'
            '92f3cb65ee1b5d07b0a28d02424cd7ae1ad5705d407b5aa7d635da680b4c8568'
            '10e50e41aca33d26f02df2ba53512cab7d3fbc0f49c161952e67cde619104b45'
            '11917658c2f4bb1d8c1a4603b9e3844cc9be10171fb6df0e9b482c07a3a3b6aa'
            '4add71eff4d4c7970a518faa4c6fbf83879c6237b082a37eb6427de4f1b95bfe'
            'efc5e88c082d405d53c0a5b22891cd295620ebf02ef6b488ab752df772d5b4ba'
            'af840e7e03aa9cf311c0d1e32469596e5e728a0206cbe06f99bbc22e3de25a12')

_pkg="NVIDIA-Linux-x86_64-${pkgver}-no-compat32"

prepare() {
    sh "${_pkg}.run" --extract-only

    cd "${_pkg}"
    local src
    for src in "${source[@]}"; do
        src="${src%%::*}"
        src="${src##*/}"
        [[ $src = *.patch ]] || continue
        msg2 "Applying patch: $src..."
        patch -Np1 < "../$src"
    done
}

build() {
  _kernver="$(cat /usr/src/${_linuxprefix}/version)"
    _kernver="$(cat /usr/src/${_linuxprefix}/version)"

    cd "${_pkg}"
    make -C kernel SYSSRC=/usr/lib/modules/"${_kernver}/build" module
}

package() {
    _kernver="$(cat /usr/src/${_linuxprefix}/version)"

    cd "${_pkg}"
    install -Dm 644 kernel/*.ko -t "${pkgdir}/usr/lib/modules/${_kernver}/extramodules/"

    # compress each module individually
    find "${pkgdir}" -name '*.ko' -exec xz -T1 {} +

    install -Dm 644 LICENSE -t "${pkgdir}/usr/share/licenses/${pkgname}/"
}
