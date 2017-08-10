SRC_NAME=inadyn-2.2

CFLAGS  += -ffunction-sections -fdata-sections
CXXFLAGS += -ffunction-sections -fdata-sections
LDFLAGS += -Wl,--gc-sections

THISDIR = $(shell pwd)

all: config_test
	$(MAKE) -C $(SRC_NAME)

config_test:
	( if [ -f ./config_done ]; then \
		echo "the same configuration"; \
	else \
		make configure && touch config_done; \
	fi )

configure:
	( cd $(SRC_NAME) ; \
	./configure \
		--prefix=/usr \
		$(if $(filter $(CONFIG_FIRMWARE_INCLUDE_DDNS_SSL),y),--enable-openssl,--disable-ssl) \
		confuse_CFLAGS="-I$(STAGEDIR)/include" \
		confuse_LIBS="-L$(STAGEDIR)/lib -lconfuse" \
		OPENSSL_CFLAGS="-I$(STAGEDIR)/include" \
		OPENSSL_LIBS="-L$(STAGEDIR)/lib -lssl -lcrypto" \
		--host=$(HOST_TARGET) \
		--build=$(HOST_BUILD) ; \
	)

clean:
	if [ -f $(SRC_NAME)/Makefile ] ; then \
		$(MAKE) -C $(SRC_NAME) distclean ; \
	fi ; \
	rm -f config_done
	rm -rf install

romfs:
	$(ROMFSINST) $(THISDIR)/$(SRC_NAME)/src/inadyn /bin/inadyn
