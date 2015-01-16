DIRS = led tester

.PHONY: subdirs $(CLEANDIRS)
.PHONY: subdirs $(SUBDIRS)

CLEANDIRS = $(DIRS:%=clean-%)
BUILDDIRS = $(DIRS:%=build-%)

all: $(BUILDDIRS)
$(DIRS): $(BUILDDIRS)
$(BUILDDIRS):
	$(MAKE) -C $(@:build-%=%)

clean: $(CLEANDIRS)
$(CLEANDIRS): 
	$(MAKE) -C $(@:clean-%=%) clean

.PHONY: all clean
