# Maintainer: Bernhard Landauer <bernhard[at]manjaro[dot]org>
# Maintainer: Philip Müller <philm[at]manjaro[dot]org>
# Contributor: Gerd Röthig (DAC24)

# Archlinux credits:
# Maintainer : Thomas Baechler <thomas@archlinux.org>
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
pkgrel=69111
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
        'kernel-6.2.patch'
        'kernel-6.3.patch'
        'kernel-6.4.patch'
        'kernel-6.6.patch'
        'kernel-6.8.patch'
        'buildfix_kernel_6.5-garbage-collect-all-references-to-get_user.patch'
        'buildfix_kernel_6.5-handle-get_user_pages-vmas-argument-remova.patch'
        'conftest.patch')
sha256sums=('162317a49aa5a521eb888ec12119bfe5a45cec4e8653efc575a2d04fb05bf581'
            '3501f0bbd9603543da74873905448ff58f5095948a7375617bba74f122d575aa'
            '4284f95f808df4e43afc4632e3fc1f87da1a805f0f6f9af1f6b519c7cf7562b4'
            '6a73ba0760c278a835ec5dcc6f3a9e0f8f0787fde95e832c38e8038152242708'
            '070879076eb75801ecf42819ccf89f09556db421475cb883413b10876a306f87'
            'fa82f41f0d2d12d3dcf983996683c8576d9f41407b052244a57ad3778031ec0f'
            'b50bfc1f45c706fed3429bf2a876d127405a8a0c7617042ef088ca5493b0814f'
            'dbb15bb863a74513946738be1ae223f26eab0e631b8ecb05b0f9c52549fdb847'
            'd20d52d1d31e788822d3daf8f31dde4857c2932b17c1af8b4fce26ad2c22725a')

_pkg="NVIDIA-Linux-x86_64-${pkgver}-no-compat32"

prepare() {
    sh "${_pkg}.run" --extract-only

    cd "${_pkg}/kernel"
    local src
    for src in "${source[@]}"; do
        src="${src%%::*}"
        src="${src##*/}"
        [[ $src = *.patch ]] || continue
        msg2 "Applying patch: $src..."
        patch -Np1 < "../../$src"
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
