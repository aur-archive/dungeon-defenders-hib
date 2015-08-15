# Maintainer: Ainola <opp310@alh.rqh> (ROT13)
# Contributor: Claudio Kozicky <claudiokozicky@gmail.com>
# Contributor: Ben R <thebenj88 *AT* gmail *DOT* com>
# Contributor: Brandon D <draygera *AT* gmail *DOT* com>

pkgname=dungeon-defenders-hib
_pkgname=dungeondefenders
pkgver=7.48
_pkgver=03052013
pkgrel=2
epoch=1
pkgdesc="A co-operative 3D tower defense game with medieval theming."
url="http://dungeondefenders.com/"
license=(custom)
arch=('i686' 'x86_64')
[ $CARCH = i686 ] \
    && depends=(libgl sdl2 openal)
[ $CARCH = x86_64 ] \
    && depends=(lib32-libgl lib32-sdl2 lib32-openal)
makedepends=('unzip')
options=(!strip)
source=("hib://dundef-linux-$_pkgver.mojo.run"
        "dundef.install"
        "$pkgname.desktop")
md5sums=('31c59c04366405c5d57665bcac219669'
         'cfead43e7e8d45eff0d15dabd8f9725f'
         '135565aadfa384458938d7c9047ab22b')
DLAGENTS+=('hib::/usr/bin/echo Could not find %u. Manually download it to \"$(pwd)\", or set up a hib:// DLAGENT in /etc/makepkg.conf.; exit 1')
install=('dundef.install')
PKGEXT=.pkg.tar

prepare()
{
    cd "$srcdir"

    # unzip returns 1 because *.mojo.run is not a valid ZIP file
    # if unzip returns a value different from 0, save it to $_return
    unzip -uo dundef-linux-$_pkgver.mojo.run || _return=$?
    # if $_return is set and is not equal to "1", exit
    [ $_return ] && [ ! $_return -eq "1" ] && exit 1 || true

    # use system libraries
    rm "$srcdir"/data/UDKGame/Binaries/libopenal.so.1
    rm "$srcdir"/data/UDKGame/Binaries/libSDL2-2.0.so.0

    # fix https://bugzilla.icculus.org/show_bug.cgi?id=5894
    sed -e 's/DefaultGameplayLevel=LobbyLevel_Valentines2013.udk/LobbyLevel.udk/' \
        -e 's/DefaultGameplayLevelRanked=LobbyLevel_Valentines2013.udk/LobbyLevel.udk/' \
        -i data/UDKGame/Config/DefaultDunDef.ini
}

package()
{
    cd "$srcdir"

    # data
    cd data
    install -d "$pkgdir"/opt/$_pkgname
    cp -lR . "$pkgdir"/opt/$_pkgname
    chmod +x "$pkgdir"/opt/$_pkgname/{DungeonDefenders,UDKGame/Binaries/{DungeonDefenders-x86,xdg-open}}
    cd ..

    # launcher
    install -d "$pkgdir"/usr/bin
    ln -s /opt/$_pkgname/DungeonDefenders "$pkgdir"/usr/bin/$_pkgname

    # doc
    install -d "$pkgdir"/usr/share/doc/$pkgname
    ln -s /opt/$_pkgname/README-linux.txt "$pkgdir"/usr/share/doc/$pkgname/README

    # desktop integration
    install -d "$pkgdir"/usr/share/pixmaps
    ln -s /opt/$_pkgname/DunDefIcon.png "$pkgdir"/usr/share/pixmaps/$_pkgname.png
    install -Dm644 $pkgname.desktop "$pkgdir"/usr/share/applications/$_pkgname.desktop
}

# vim:et:sw=4:sts=4
