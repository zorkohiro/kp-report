MAJORV  ?= 0
MINORV  ?= 9
MICROV  ?= 5
BUILDV  ?= -5
RELEASE := $(MAJORV).$(MINORV).$(MICROV)${BUILDV}

BASE    := kp-report
PKGNAME := $(BASE)-$(RELEASE)
BINS    := spawn_ol SetSystemTime CheckOrthancDataMount

all:    dist

.PHONY: dist
dist:
	rm -rf ${BASE}-*
	for c in $(BINS); do install -D -m 755 $$c $(PKGNAME)/usr/bin/$$c; done
	install -D -m 640 orthanc_datamount_checker.service ${PKGNAME}/lib/systemd/system/orthanc_datamount_checker.service
	install -D -m 640 dbmount.conf ${PKGNAME}/lib/systemd/system/orthanc.service.d/dbmount.conf
	install -D -m 440 orthanc.sudo ${PKGNAME}/etc/sudoers.d/orthanc
	install -D -m 644 ../ReportPlugin/Build/libReport.so.1.0 ${PKGNAME}/usr/lib/kp-report/libReport.so.1.0
	install -D -m 644 ../OHIFPlugin/Build/libOrthancOHIF.so.1.4 ${PKGNAME}/usr/lib/kp-report/libOrthancOHIF.so.1.4
	install -D -m 555 SetSystemTime.desktop ${PKGNAME}/usr/lib/kp-report/SetSystemTime.desktop
	mkdir -p -m 755 $(PKGNAME)/DEBIAN
	sed -e "s,%VERSION%,$(RELEASE)," control > $(PKGNAME)/DEBIAN/control
	cp preinst postinst postrm ${PKGNAME}/DEBIAN
	dpkg-deb --build $(PKGNAME)

clean:
	@rm -rf $(BASE)-*

