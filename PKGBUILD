_npmname=nativefier
_npmver=7.6.10
pkgname=nodejs-nativefier
pkgver=7.6.10
pkgrel=1
pkgdesc="Wrap web apps natively"
arch=(any)
url="https://github.com/jiahaog/nativefier#readme"
license=()
depends=('nodejs')
makedepends=('jq' 'npm')
optdepends=()
source=(http://registry.npmjs.org/$_npmname/-/$_npmname-$_npmver.tgz)
sha1sums=('777c6a61c3471ca5ec2f5737db2faa316ad516e0')

package() {
    # Thanks jeremejevs and je-vv for the pointers on these!
    npm install -g --user root --cache "${srcdir}/npm-cache" --prefix "$pkgdir/usr" "$srcdir/$_npmname-$pkgver.tgz"

    # Fix permissions
    find "$pkgdir"/usr -type d -exec chmod 755 {} +

    # Remove references to pkgdir
    find "$pkgdir" -type f -name package.json -print0 | xargs -0 sed -i "/_where/d"

    # Remove references to srcdir
    local tmppackage="$(mktemp)"
    local pkgjson="$pkgdir/usr/lib/node_modules/$_npmname/package.json"
    jq '.|=with_entries(select(.key|test("_.+")|not))' "$pkgjson" > "$tmppackage"
    mv "$tmppackage" "$pkgjson"
    chmod 644 "$pkgjson"
}

# vim:set ts=2 sw=2 et:
