**Readme.md**

This document covers the following topics:

* A guide to sources of information that complement the Windows 3.1 Software Development Kit (SDK) manuals.

* Instructions for setting your PATH, INCLUDE, and LIB environment variables, and other instructions for configuring your Windows development environment. (The SDK Install program appends this information to this README.SDK file.)


Other Sources Of Information
============================

The following list describes online documents that contain
important information about the Windows 3.1 SDK that is not
included in the on-line Windows Help or QuickHelp documentation.

| Document    | Contents                                                                                  |
|-------------|-------------------------------------------------------------------------------------------|
| README.WRI  | Additional information about the Windows 3.1 SDK. This file is in Microsoft Write format. |
| Windows.md  | Information about changes to WINDOWS.H, including STRICT.                                 |
| Robust.md   | Information on how to build robust applications.                                          |
| WindowsX.md | Information on using the message crackers and control functions in WINDOWSX.H.            |


Additional information is available on the following topics:

* Microsoft C Runtime Libraries

* Message Crackers and Control APIs

* Audio Documentation and Software

* New Directories

* Dialog Editor (DLGEDIT.EXE)

* Windows clipboard formats

* DOS Protected Mode Interface (DPMI) Specification

These sources are described in this section.


Microsoft C Runtime Libraries
-----------------------------
C runtime libraries for Windows are no longer included with the SDK. 
These files are now included with the C product you use for 
Windows Development.


Message Crackers and Control APIs
---------------------------------
The Message Crackers and Control APIs were moved from `WINDOWS.H` to `WINDOWSX.H`.  
Most known bugs and problems reported from pre-release 2 are fixed in this release of `WINDOWSX.H`. 
You can build generic Windows applications that use Message Crackers and Control APIs 
and which are `STRICT` compliant by using `WINDOWSX.H` with the generic sample `MAKEAPP` located in the `SAMPLES` subdirectory.


Audio Documentation and Software
--------------------------------
This release also includes `MMSYSTEM.H` and on-line documentation on how to use the new audio services in Windows 3.1.  
The on-line documentation is located in `WIN31MWH.HLP`, `MMPWKBK.HLP`, and `MCISTRWH.HLP` (WinHelp format) 
or `WIN31QH.HLP`, `MCISTRQH.HLP` (QuickHelp format).


New Directories
---------------   
   
- **`BIN`** contains the files previously installed in the root WINDEV directory
- **`GUIDE`** contains the Guide to Programming Samples 
- **`SAMPLES`** contains the new sample source code applications
- **`REDIST`** contains the redistributable file for the SDK
- **`HELP`** contains the SDK Quickhelp files


Windows Clipboard Formats
-------------------------

Specifications for various clipboard formats supported by
Microsoft and other vendors' applications are available from
sources described below.

- **Rich Text Format (RTF)** -- similar to the standard text
  format, except that formatting attributes such as font,
  style, and size are embedded.  The RTF specification is
  included in the *"Microsoft Word Technical Reference for
  Windows and OS/2 Presentation Manager,"*, which may be
  ordered through bookstores or directly from **Microsoft
  Press** at (800)888-3303.

- **Binary Interchange File Format (BIFF)** -- the native
  Microsoft Excel format. The BIFF specification is
  available from Microsoft Excel Telephone Sales at
  (800)227-6444; ask for department *"ET"*.

- **Tag Image File Format (TIFF)** -- a format that assists in
  incorporating line art, photographs, and other raster
  images into documents via desktop publishing application
  programs. The TIFF specification is available from Aldus
  Corporation at (206)628-2320; select the Production
  Information phone extension.  It is also available in the
  CompuServe "Aldus" Forum, Library 10.

- **Data Interchange Format (DIF), WKS, and WK1** -- formats
  maintained by Lotus Development Corporation,
  (617)253-9150.


DOS Protected Mode Interface (DPMI) Specification
-------------------------------------------------
This specification defines how DOS programs can access the
extended memory of PC architecture computers while
maintaining system protection.  Windows provides
applications access to extended memory through Windows'
implementation of DPMI. Most Windows applications do not
need to directly use DPMI services, since these services are
provided by the Windows application programming interface
(API). Special Windows applications and device drivers may
need to access DMPI services directly.

The DPMI specification is available from Intel Corporation
at (800)548-4725. (Request part number JP-26.) This DPMI
specification should be read in conjunction with the
*"Windows Support for DPMI"* note included in the Windows
Developer's Notes, described above.


Setting Your PATH, INCLUDE, and LIB Environment Variables
=========================================================

Based on the directories that you specified during the SDK
installation procedure, you should add the following
directories in your environment variables:

- Insert `C:\WINDEV\INCLUDE` as the first path in the INCLUDE variable.
- Insert `C:\WINDEV\LIB` as the first path in the LIB variable.
- Insert `C:\WINDEV\BIN` in the PATH variable.
- Insert `C:\WINDEV\HELP` in the HELPFILES variable.

**Editing `SYSTEM.INI`:**

If you plan on debugging with CVW, add the line

    DEVICE=WINDEBUG.386
	
to the `[386Enh]` section of this file.

**Editing `PROGMAN.INI`:**

If you want to use the `SDKTOOLS` program group, add the line

    Group{next available number}=C:\WINDEV\SDKTOOLS.GRP
	 
to this file.

Install was unable to copy your NoDebug GDI.EXE, USER.EXE, KERNEL.EXE,
KRNL286.EXE and KRNL386.EXE files into your NoDebug directory. Before
you use N2D.BAT or D2N.BAT, make sure you copy these files into your
NoDebug directory. These files are located on your Retail Windows
disks in compressed format. Use the SDK Expand utility to uncompress
them. In addition, you will need to add your Windows system 
directory to the top of the SWITCH.BAT file.
