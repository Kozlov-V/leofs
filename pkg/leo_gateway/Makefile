include ../../leofs.mk
VERSION=$(LEOFS_VERSION)
COMPONENT_INTERNAL=leo_gateway
COMPONENT=leo_gateway
COMPONENT_PATH=../../package/$(COMPONENT_INTERNAL)
DIR=../bootstrap/$(COMPONENT)

.PHONY: build_info package package_list all clean  bootstrap

all: package

bootstrap:
	-rm -r $(DIR)
	cp -r $(COMPONENT_PATH) $(DIR)
	mkdir $(DIR)/share
	cp files/$(COMPONENT) $(DIR)/bin/
	cp files/$(COMPONENT).xml $(DIR)/share
	cp files/start $(DIR)/erts-*/bin/start
	rm $(DIR)/etc/leo_gateway.conf
	cp files/leo_gateway.conf $(DIR)/etc/leo_gateway.conf.example
	cp files/leo_gateway_snmp.config.0 $(DIR)/snmp/snmpa_gateway_0/leo_gateway_snmp.config
	cp files/leo_gateway_snmp.config.1 $(DIR)/snmp/snmpa_gateway_1/leo_gateway_snmp.config

package_list: bootstrap
	cat pre_pkg > packlist
	(cd $(DIR); find * -type f | sort) >> packlist
	cat post_pkg >> packlist

build_info:
	pkg_info -X pkg_install | egrep '^(MACHINE_ARCH|OPSYS|OS_VERSION|PKGTOOLS_VERSION)' >build-info
	echo "CATEGORIES=leofs" >>build-info
	echo "HOMEPAGE=http://www.leofs.org/" >>build-info

clean:
	-rm *.tgz build-info packlist
	-rm -r $(DIR)

package: package_list build_info 
	pkg_create -i install.sh -k deinstall.sh -D displayfile -B build-info -c comment -d description -f packlist -I /opt/local/$(COMPONENT) -p $(DIR) -U $(COMPONENT)-$(VERSION)$(SUFFIX).tgz
