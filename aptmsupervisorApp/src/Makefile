TOP=../..

include $(TOP)/configure/CONFIG
#----------------------------------------
#  ADD MACRO DEFINITIONS BELOW HERE

# xxxRecord.h will be created from xxxRecord.dbd
DBDINC += xxxRecord

# Install xxxSupport.dbd into <top>/dbd
DBD += xxxSupport.dbd

# Build an IOC support library
LIBRARY_IOC += aptmsupervisorSupport

# Compile and add the code to the support library
aptmsupervisorSupport_SRCS += xxxRecord.c
aptmsupervisorSupport_SRCS += devXxxSoft.c

# Link locally-provided code into the support library,
# rather than directly into the IOC application.
# This is required for Windows DLL builds.
aptmsupervisorSupport_SRCS += dbSubExample.c
aptmsupervisorSupport_SRCS += aptmsupervisorHello.c
aptmsupervisorSupport_SRCS += initTrace.c

aptmsupervisorSupport_LIBS += $(EPICS_BASE_IOC_LIBS)


# Build the IOC application
PROD_IOC = aptmsupervisor

# aptmsupervisor.dbd will be created and installed
DBD += aptmsupervisor.dbd

# aptmsupervisor.dbd will include these files:
aptmsupervisor_DBD += base.dbd
aptmsupervisor_DBD += xxxSupport.dbd
aptmsupervisor_DBD += dbSubExample.dbd
aptmsupervisor_DBD += aptmsupervisorHello.dbd
aptmsupervisor_DBD += initTrace.dbd

# aptmsupervisor_registerRecordDeviceDriver.cpp derives from aptmsupervisor.dbd
aptmsupervisor_SRCS += aptmsupervisor_registerRecordDeviceDriver.cpp

# Build the main IOC entry point where needed
aptmsupervisor_SRCS_DEFAULT += aptmsupervisorMain.cpp
aptmsupervisor_SRCS_vxWorks += -nil-

# Link in the code from our support library
aptmsupervisor_LIBS += aptmsupervisorSupport

# To build SNL programs, SNCSEQ must be defined
# in the <top>/configure/RELEASE file
ifneq ($(SNCSEQ),)
    # Build sncExample into aptmsupervisorSupport
    sncExample_SNCFLAGS += +r
    aptmsupervisor_DBD += sncExample.dbd
    # A .stt sequence program is *not* pre-processed:
    aptmsupervisorSupport_SRCS += sncExample.stt
    aptmsupervisorSupport_LIBS += seq pv
    aptmsupervisor_LIBS += seq pv

    # Build sncProgram as a standalone program
    PROD_HOST += sncProgram
    sncProgram_SNCFLAGS += +m
    # A .st sequence program *is* pre-processed:
    sncProgram_SRCS += sncProgram.st
    sncProgram_LIBS += seq pv
    sncProgram_LIBS += $(EPICS_BASE_HOST_LIBS)
endif

# Finally link IOC to the EPICS Base libraries
aptmsupervisor_LIBS += $(EPICS_BASE_IOC_LIBS)

include $(TOP)/configure/RULES
#----------------------------------------
#  ADD EXTRA GNUMAKE RULES BELOW HERE

