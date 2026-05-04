# Maintainer: Luc Ritchie

_pkgbase=appletbdrm-prealloc
_kver=7.0.3
_archrel=1
_tag=v${_kver}-arch${_archrel}

pkgname=${_pkgbase}-dkms
pkgver=${_kver}
pkgrel=1
pkgdesc="Apple Touch Bar DRM driver (DKMS, with preallocation patches)"
arch=('x86_64')
url="https://github.com/archlinux/linux"
license=('GPL-2.0-only')
depends=('dkms')
_patches=(
    0001-pre-allocate-usb-transfer-buffers.patch
)

source=(
    "appletbdrm.c::https://raw.githubusercontent.com/archlinux/linux/${_tag}/drivers/gpu/drm/tiny/appletbdrm.c"
    "dkms.conf"
    "Makefile"
    "Kconfig"
    "${_patches[@]}"
)
sha256sums=('SKIP'
            'SKIP'
            'SKIP'
            'SKIP'
            'SKIP')

prepare() {
    # Replace symlink with a real copy so patch can modify it
    cp --remove-destination "$(readlink -f appletbdrm.c)" appletbdrm.c

    for p in "${_patches[@]}"; do
        echo "Applying ${p}..."
        patch -Np5 < "${p}"
    done
}

package() {
    local dest="${pkgdir}/usr/src/${_pkgbase}-${pkgver}"
    install -Dm644 appletbdrm.c "${dest}/appletbdrm.c"
    install -Dm644 Makefile "${dest}/Makefile"
    install -Dm644 Kconfig "${dest}/Kconfig"

    # Install dkms.conf with version substituted
    sed "s/@VERSION@/${pkgver}/" dkms.conf > "${dest}/dkms.conf"
    chmod 644 "${dest}/dkms.conf"
}
