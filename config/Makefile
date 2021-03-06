#
# SPDX-License-Identifier: GPL-2.0-only
#
# Copyright (C) 2015-2020 Micron Technology, Inc.  All rights reserved.
#
SUBDIRS := vm_fault_t vm_fault_func
SUBDIRS += bioset_init bioset_create bio_set_dev bio_set_op_attrs
SUBDIRS += iov_iter_init iov_iter_get_pages invalidatepage
SUBDIRS += mem_cgroup_count_vm_event count_memcg_event_mm
SUBDIRS += generate_random_guid blkdev_flush
SUBDIRS += sched_clock submit_bio mmap_lock
SUBDIRS += bdi_init bdi_alloc_node bdi_name backing_dev_info

.PHONY: all clean distclean maintainer-clean ${SUBDIRS}

.DELETE_ON_ERROR:

KDIR ?= /lib/modules/$(shell uname -r)/build
KREL ?= $(patsubst /lib/modules/%/build,%,${KDIR})

ifeq (${KREL},${KDIR})
  KREL := $(patsubst /usr/src/kernels/%,%,${KDIR})
endif

ifeq (${KREL},${KDIR})
  $(error "Unable to determine kernel release from KDIR.  Try setting it via the KREL= variable")
endif

CLEANFILES := */Kbuild */Makefile */config.h

CF := config.h-${KREL}

MAKEFLAGS += --no-print-directory

all: ${CF}

clean:
	rm -f ${CLEANFILES}

distclean: clean
	rm -f config.h-*

maintainer-clean: distclean
	@true

${CF}: ${SUBDIRS}
	echo "/* SPDX-License-Identifier: GPL-2.0-only */" > $@.tmp
	echo "/* *** Machine generated for ${KREL} on $(shell date) *** */" >> $@.tmp
	cat */config.h >> $@.tmp
	mv $@.tmp $@

vm_fault_func: vm_fault_t

${SUBDIRS}:
	@cp Makefile.test $@/Makefile
	@cp Kbuild.test $@/Kbuild
	${MAKE} -C $@ KDIR=${KDIR} KREL=${KREL} ${MAKECMDGOALS}

