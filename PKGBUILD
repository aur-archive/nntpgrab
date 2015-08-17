# Maintainer: BlackEagle < ike DOT devolder AT gmail DOT com >

pkgbase=nntpgrab
pkgname=nntpgrab
true && pkgname=('nntpgrab-cli' 'nntpgrab-gtk' 'nntpgrab-qt')
pkgver=0.7.2
pkgrel=3
pkgdesc="open source usenet download program"
arch=('i686' 'x86_64')
url="http://www.nntpgrab.nl/"
license="gpl2"
makedepends=('gtk3' 'qt4' 'networkmanager' 'dbus-glib' 'libsoup' 'gtk-doc' 'par2cmdline' 'intltool')
options=('!libtool')
source=("http://nntpgrab.nl/releases/$pkgbase-$pkgver.tar.bz2")

_gtktarget=${srcdir}/${_svnmod}-gtk-target

build()
{
	cd $pkgbase-$pkgver

	./configure\
		--enable-introspection=no \
		--enable-gtk-doc=no \
		--enable-gtk-doc-html=no \
		--enable-gtk-doc-pdf=no \
		--prefix=/usr
	make

	pushd client/gui_qt
		qmake-qt4 gui_qt.pro -o Makefile
		make
	popd

	pushd server_qt
		qmake-qt4 server_qt.pro -o Makefile
		make
	popd
}

package_nntpgrab-cli() {
	pkgdesc="NNTPGrab is an open source usenet download program cli version"
	depends=('networkmanager' 'gnutls')
	optdepends=('par2cmdline: for automatic repair'
		'unrar: for automatic extraction')
	cd $pkgbase-$pkgver
	make DESTDIR=${pkgdir} install

	mkdir ${_gtktarget}
	mkdir -p ${_gtktarget}/usr/bin
	mv ${pkgdir}/usr/bin/nntpgrab_gui ${_gtktarget}/usr/bin/nntpgrab_gui
	mv ${pkgdir}/usr/bin/nntpgrab_server_gtk ${_gtktarget}/usr/bin/nntpgrab_server_gtk
	mkdir -p ${_gtktarget}/usr/lib/nntpgrab/gtk
	mv ${pkgdir}/usr/lib/libnntpgrab_gui_base.so.0.0.0 ${_gtktarget}/usr/lib/libnntpgrab_gui_base.so.0.0.0
	rm ${pkgdir}/usr/lib/libnntpgrab_gui_base.so*
	#mv ${pkgdir}/usr/lib/nntpgrab/gtk/libnntpgrab_plugin_counter_gtk.so ${_gtktarget}/usr/lib/nntpgrab/gtk/libnntpgrab_plugin_counter_gtk.so
	#mv ${pkgdir}/usr/lib/nntpgrab/gtk/nntpgrab_plugin_counter_gtk.plugin ${_gtktarget}/usr/lib/nntpgrab/gtk/nntpgrab_plugin_counter_gtk.plugin
	mkdir -p ${_gtktarget}/usr/share/applications
	mv ${pkgdir}/usr/share/applications/nntpgrab.desktop ${_gtktarget}/usr/share/applications/nntpgrab.desktop
	mv ${pkgdir}/usr/share/applications/nntpgrab_server_gtk.desktop ${_gtktarget}/usr/share/applications/nntpgrab_server_gtk.desktop
	mkdir -p ${_gtktarget}/usr/share/nntpgrab/plugins
	mv ${pkgdir}/usr/share/nntpgrab/nntpgrab_gui.ui ${_gtktarget}/usr/share/nntpgrab/nntpgrab_gui.ui
	mv ${pkgdir}/usr/share/nntpgrab/nntpgrab_server.ui ${_gtktarget}/usr/share/nntpgrab/nntpgrab_server.ui
	#mv ${pkgdir}/usr/share/nntpgrab/plugins/counter_plugin_gtk.ui ${_gtktarget}/usr/share/nntpgrab/plugins/counter_plugin_gtk.ui
}

package_nntpgrab-gtk() {
	pkgdesc="NNTPGrab is an open source usenet download program gtk version"
	depends=('nntpgrab-cli' 'gtk3' 'dbus-glib' 'libsoup' 'desktop-file-utils')
	install='nntpgrab.install'

	cd ${_gtktarget}

	# install binaries
	install -Dm 0755 usr/bin/nntpgrab_gui ${pkgdir}/usr/bin/nntpgrab_gui
	install -Dm 0755 usr/bin/nntpgrab_server_gtk ${pkgdir}/usr/bin/nntpgrab_server_gtk

	# install libs
	install -Dm 0644 usr/lib/libnntpgrab_gui_base.so.0.0.0 ${pkgdir}/usr/lib/libnntpgrab_gui_base.so.0.0.0
	#install -Dm 0644 usr/lib/nntpgrab/gtk/libnntpgrab_plugin_counter_gtk.so ${pkgdir}/usr/lib/nntpgrab/gtk/libnntpgrab_plugin_counter_gtk.so
	#install -Dm 0644 usr/lib/nntpgrab/gtk/nntpgrab_plugin_counter_gtk.plugin ${pkgdir}/usr/lib/nntpgrab/gtk/nntpgrab_plugin_counter_gtk.plugin

	# install in desktop files
	install -Dm 0644 usr/share/applications/nntpgrab.desktop ${pkgdir}/usr/share/applications/nntpgrab.desktop
	install -Dm 0644 usr/share/applications/nntpgrab_server_gtk.desktop ${pkgdir}/usr/share/applications/nntpgrab_server_gtk.desktop

	# install gtk ui files
	install -Dm 0644 usr/share/nntpgrab/nntpgrab_gui.ui ${pkgdir}/usr/share/nntpgrab/nntpgrab_gui.ui
	install -Dm 0644 usr/share/nntpgrab/nntpgrab_server.ui ${pkgdir}/usr/share/nntpgrab/nntpgrab_server.ui
	#install -Dm 0644 usr/share/nntpgrab/plugins/counter_plugin_gtk.ui ${pkgdir}/usr/share/nntpgrab/plugins/counter_plugin_gtk.ui
}

package_nntpgrab-qt() {
	pkgdesc="NNTPGrab is an open source usenet download program qt version"
	depends=('nntpgrab-cli' 'qt4')
	install='nntpgrab.install'

	cd $pkgbase-$pkgver

	# install binaries
	install -Dm 0755 client/gui_qt/nntpgrab_gui_qt ${pkgdir}/usr/bin/nntpgrab_gui_qt
	install -Dm 0755 server_qt/nntpgrab_server_qt ${pkgdir}/usr/bin/nntpgrab_server_qt

	# install translations
	install -dm 0755 ${pkgdir}/usr/share/nntpgrab/translations
	for translation in client/gui_qt/translations/*.qm; do
		install -Dm 0644 ${translation} ${pkgdir}/usr/share/nntpgrab/translations
	done

	# install desktop files
	install -Dm 0644 client/gui_qt/nntpgrab_qt.desktop ${pkgdir}/usr/share/applications/nntpgrab_qt.desktop
	install -Dm 0644 server_qt/nntpgrab_server_qt.desktop ${pkgdir}/usr/share/applications/nntpgrab_server_qt.desktop
}
sha256sums=('fb6b5dee6db4a008b84a3ceb1121299778ac2d0c60bc422eaa2e0f6fd50d0ec4')
