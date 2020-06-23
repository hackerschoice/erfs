
VERSION=1.3
BIN_NAME=erfs
PKG_NAME=${BIN_NAME}-client-${VERSION}

dist:
	mkdir -p ${PKG_NAME}
	cp -a ../license ../man ../${BIN_NAME} ./${PKG_NAME}/
	tar cfz ${PKG_NAME}.tar.gz ${PKG_NAME}
	rm -rf ./${PKG_NAME}/
	ls -al ./${PKG_NAME}.tar.gz


debuild-setup: dist
	rm -rf build
	mkdir build
	cp ${PKG_NAME}.tar.gz build && \
	cd build && \
	tar xfz ${PKG_NAME}.tar.gz
	-cd build/${PKG_NAME} && \
	dh_make -sy -f ../${PKG_NAME}.tar.gz && \
	rm -f debian/*.EX debian/*.ex debian/*.docs debian/README.* && \
	cp -a ../../debian/* debian/ && \
	uscan . 

debian-debuild: debuild-setup
	cd build/${PKG_NAME} && \
	debuild -S
	lintian --pedantic -IE build/${BIN_NAME}-client_*.dsc

debian: debian-debuild
	echo Done.

