PYTHON=python
TMPDIR=tmp
WEBDIR=web
SCRIPT=driver.py
TSPREP=timestamp_prep.log
TSUPLOAD=timestamp_upload.log
TSSANDBOX=timestamp_sandbox.log
TSPRODUCT=timestamp_production.log
DEPSCRIPT=

ifeq ($(CHKSCRIPT), 1)
	DEPSCRIPT=$(SCRIPT)
endif

all: prep upload
	@echo 'Finished all preliminary steps (or everything is up to date).'
	@echo 'Type "make sandbox" to publish to the sandbox.'
	@echo 'Type "make production" to publish to the actual MTurk worker space.'
	@echo 'Type "make bonus" to grant bonuses.'
	@echo 'Otherwise, type "make clean" to clean up the local workspace.'
$(TMPDIR)/$(TSPREP): $(DEPSCRIPT) $(WEBDIR)/compensate.html
	DRIVER_TMPDIR=$(TMPDIR) DRIVER_TSTAMP=$(TSPREP) $(PYTHON) $(SCRIPT) prep
prep: $(TMPDIR)/$(TSPREP)
	
$(TMPDIR)/$(TSUPLOAD): $(DEPSCRIPT) $(TMPDIR)/$(TSPREP)
	DRIVER_TMPDIR=$(TMPDIR) DRIVER_TSTAMP=$(TSUPLOAD) $(PYTHON) $(SCRIPT) upload
upload: $(TMPDIR)/$(TSUPLOAD)
	
$(TMPDIR)/$(TSSANDBOX): $(DEPSCRIPT) $(TMPDIR)/$(TSUPLOAD)
	DRIVER_TMPDIR=$(TMPDIR) DRIVER_TSTAMP=$(TSSANDBOX) $(PYTHON) $(SCRIPT) sandbox
sandbox: $(TMPDIR)/$(TSSANDBOX)
	
$(TMPDIR)/$(TSPRODUCT): $(DEPSCRIPT) $(TMPDIR)/$(TSUPLOAD)
	DRIVER_TMPDIR=$(TMPDIR) DRIVER_TSTAMP=$(TSPRODUCT) $(PYTHON) $(SCRIPT) production
production: $(TMPDIR)/$(TSPRODUCT)
	
bonus-sandbox:
	DRIVER_TMPDIR=$(TMPDIR) $(PYTHON) $(SCRIPT) bonus-sandbox
bonus:
	DRIVER_TMPDIR=$(TMPDIR) $(PYTHON) $(SCRIPT) bonus
clean:
	rm -rf tmp/
