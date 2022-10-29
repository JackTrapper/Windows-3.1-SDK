Additional Notes About the Microsoft Windows 3.1 Software Development Kit
=========================================================================


This online document contains information about the Microsoft Windows operating system, version 3.1, that is not available in the other documentation for the Microsoft Windows 3.1 Software Development Kit (SDK).

Using Windows Write to View This Document
-----------------------------------------

If you enlarge the Write window to its maximum size, this document will be easier to read. To do so, click the Maximize button in the upper-right corner of the window or open the System menu in the upper-left corner of the Write window (press ALT+SPACEBAR) and then choose the Maximize command. (The System menu is sometimes referred to as the Control menu.)

To move through the document, use the PAGE UP and PAGE DOWN keys or click the arrows at the top and bottom of the scroll bar along the right side of the Write window.

To print the document, choose the Print command from the File menu.

For Help on using Write, press F1.

To read other online documents, choose the Open command from the File menu and then select a file. 


### Contents


This document contains the following topics:

- **1.0  General Release Notes**
  - **1.1**  Network Installation 
  - **1.2**  Sample Source Code
  - **1.3**  Microsoft C 6.00 Run-time Libraries for Windows
  - **1.4**  Redistributable Components
  - **1.5**  Multimedia Components Added to the Windows 3.1 SDK

- **2.0  International Notes**
  - **2.1**  International Common Dialog Boxes
  - **2.2**  Translation Tables

- **3.0  Release Notes Specific to the Final SDK**
  - **3.1**  CodeView for Windows (CVW)
    - **3.1.1**  CVW Single-Screen Debugging
    - **3.1.2**  Using CVW 3.07 with .OBJ Files Linked with C 7.00
    - **3.1.3**  Using the Windows 3.1 Debugging Version with CVW 4.00
    - **3.1.4**  Comparing CVW 4.00 with CVW 3.07
    - **3.1.5**  Using CVW with Borland C++ Applications
  - **3.2**  COMPAQ Computers
  - **3.3**  80386 Debugger and Dr. Watson
  - **3.4**  Profiler
  - **3.5**  The GetVersion Function
  - **3.6**  Using the Windows 3.1 SDK to Develop Windows 3.0 Applications
  - **3.7**  Using a Serial Device on COM1 with the Debugging Version of Windows
  - **3.8**  DDESpy
  - **3.9**  Dialog Editor

- **4.0  Other Documents**

--------------------

1.0 General Release Notes
-------------------------

The Windows 3.1 SDK provides information about developing Windows applications but does not provide much information about environment-specific development tools. This information is provided instead by such other Windows development environments as Microsoft C 7.00, Microsoft QuickC for Windows, or other independent software vendor (ISV) development kits. 

### **1.1 Network Installation**

The SDK does not support installation from a network drive to a local Windows installation nor does it support installation from disks to a network installation of Windows 3.1. (The Windows 3.0 SDK also did not support this.)

### **1.2 Sample Source Code**

The SDK contains sample code that demonstrates new features in Windows or clarifies existing features. These samples can be found in the \SAMPLES subdirectory when you install the SDK.

**Note:** The sample code for the *Microsoft Windows Guide to Programming* is installed in the \GUIDE subdirectory. You can choose whether to install these samples when installing the SDK.

### **1.3 Microsoft C 6.00 Run-time Libraries for Windows**

The SDK installation program no longer installs the C 6.00 run-time libraries for Windows. The C 6.00 run-time libraries are now distributed on a separate disk with their own simple installation batch file (the alternate-math libraries are no longer included). The C run-time libraries for Windows were removed because most ISV Windows development environments include their own Windows C run-time libraries. Microsoft C 7.00 and Microsoft QuickC for Windows also contain C run-time libraries for Windows. If you already have C run-time libraries for Windows, it is not necessary for you to install these libraries. For the most part, only Microsoft C 6.00 users need to install these libraries.

### **1.4 Redistributable Components**

Many new Windows features are included in dynamic-link libraries (DLLs) and can be used in Windows 3.0 and Windows 3.1. If you use the new features in your Windows 3.0 applications, you will need to redistribute the Windows DLLs and related files that support these features. For a list of the redistributable libraries and files, see the separate SDK license agreement.

Some DLLs and device drivers are not redistributable. In particular, the Multimedia System library (MMSYSTEM.DLL) is not redistributable.

### 1.5 Multimedia Components Added to the Windows 3.1 SDK

The Windows 3.1 SDK contains Multimedia development tools that make it possible for you to take advantage of the Multimedia capabilities in the retail version of Windows 3.1.

| File                      | Description                                                 |
|---------------------------|-------------------------------------------------------------|
| MMSYSTEM.DLL, .H and .LIB | Multimedia System library                                   |
| MCICDA.DRV                | Media Control Interface (MCI) CD-ROM driver                 |
| MCIPIONR.DRV              | MCI Pioneer Laserdisc driver                                |
| WIN31MWH.HLP              | Online Multimedia Windows Reference                         |
| MCISTRWH.HLP              | MCI Strings interface reference                             |
| MMPWKBK.HLP               | Microsoft Windows Multimedia Programmer's Reference         |
| MARKMIDI.EXE              | Tool that marks MIDI files as Multimedia Windows MIDI files |
| LOWPASS, MCITEST, MIDIMON, and REVERSE files | Sample source code                       |

2.0 International Notes
-----------------------

This section describes enhancements to the SDK that will help you localize your application.

### 2.1 International Common Dialog Boxes

The SDK contains localized versions of the common dialog box library, COMMDLG.DLL, in ten languages, as shown in the following table.  You can redistribute these files with your localized applications.

| Language   | Library name |
|------------|--------------|
| Danish     | COMMDLG.DAN  |
| Dutch      | COMMDLG.DUT  |
| Finnish    | COMMDLG.FIN  |
| French     | COMMDLG.FRA  |
| German     | COMMDLG.GER  |
| Italian    | COMMDLG.ITN  |
| Norwegian  | COMMDLG.NOR  |
| Portuguese | COMMDLG.POR  |
| Spanish    | COMMDLG.SPA  |
| Swedish    | COMMDLG.SWE  |

To use one of these files, rename it to COMMDLG.DLL.

### 2.2 Translation Tables

Translation tables are used by the **AnsiToOem** and **OemToAnsi** functions for all character-set conversions. These tables and the default keyboard driver table have undergone extensive modifications for the SDK; the default keyboard driver table is the table for code page 437 (United States). The names of the translation tables are in the form XLAT###.BIN, in which ### stands for 850, 860, 861, 863, or 865.

It is very important that you verify your application's filename conversions between Microsoft 
MS-DOS and Windows.

3.0 Release Notes Specific to the Final SDK
-------------------------------------------

This section outlines information that is specific to the final release of the Windows 3.1 SDK.

### 3.1 CodeView for Windows (CVW)

The executable file for the version of Microsoft CodeView for Windows that is distributed with the SDK has been renamed from CVW.EXE to CVW3.EXE. This distinguishes it from CVW4.EXE, which is distributed with Microsoft C 7.00.

**3.1.1 CVW Single-Screen Debugging**

If you experience problems with CVW and single-screen debugging, please use a secondary monochrome monitor as your debugging terminal. Debugging with two screens is much faster than single-screen debugging because CVW does not have to switch between character mode and graphics mode.

CVW is known to have problems with single-screen debugging mode on the following video adapters:

- ATI VGA
- Cirrus Logic 6410
- IBM P70
- Olivetti OVGA
- Orchid Pro Designer
- Paradise VGA
- Sigma Legend
- Speedstar VGA
- Trident 8900
- Vega Video 7 VGA

**3.1.2 Using CVW 3.07 with .OBJ Files Linked with C 7.00**

CVW 3.07 will cause a GP fault when you try to load a Windows application that was linked with .OBJ files from either C 6.00 or C 7.00. You can use files linked with C 6.00 by packing them, using the C 7.00 CVPACK utility.

**3.1.3 Using the Windows 3.1 Debugging Version with CVW 4.00**

An error in the following form will occur when you use the Windows 3.1 debugging version files (KERNEL, GDI, and USER) with CVW 4.00:

> Warning: relink 'C:\WIN31\SYSTEM\USER.EXE' with the current linker.

To prevent this problem, run the C 7.00 CVPACK utility on KERNEL, GDI, and USER.

Users of CVW 3.07 who also use the DBWIN debugging application should turn off warning messages before running CVW. CVW 3.07 cannot process warning output messages.

**3.1.4 Comparing CVW 4.00 with CVW 3.07**

If you have C 7.00 (which includes CVW 4.00), the only reason to use CVW 3.07 (which is included in the Windows 3.1 SDK) is to debug Basic or FORTRAN code in the Windows environment. CVW4 does not currently possess an expression evaluator for either of these languages.  CVW3 supports development in Basic and FORTRAN with the use command in the Command window.  (No recent version of CVW has supported Pascal.)

**3.1.5 Using CVW with Borland C++ applications**

Neither CVW3 nor CVW4 can load Borland C++ applications. 

### 3.2  COMPAQ Computers

Compaq Computer Corporation ships HIMEM.EXE and CEMM.EXE with their release of 
MS-DOS version 5.0 instead of HIMEM.SYS and EMM386.EXE.

### 3.3  80386 Debugger and Dr. Watson

If you run Microsoft 80386 Debugger (WDEB386.EXE) and Microsoft Dr. Watson (DRWATSON.EXE) in standard mode and press CTRL+ALT+SYS RQ, you will encounter a breakpoint that has been set by the Tool Helper library. Choosing Go at this point will cause a GP fault. To avoid this, either use the GZ command at the first int 3 instruction, or use CTRL+C instead of CTRL+ALT+SYS RQ.

### 3.4  Profiler

Microsoft Windows Profiler analyzes applications running with Windows in 386 enhanced mode. You cannot call the **ProfSetup** function once you have called the **ProfStart** function. Even if you call **ProfStop**, you cannot call **ProfSetup**. This is because **ProfStart** allocates the block of memory for the data, and if that block has been allocated the setup code exits during the setup call. The only way to release this memory and be able to resize it is to call the **ProfFinish** function.

### 3.5 The GetVersion Function

The GetVersion function returns the Windows version in the low-order word of the return value.
The minor version is in the high-order byte and the major version is in the low-order byte. For more information, see the reference material for this function in the **Microsoft Windows Programmer's Reference, Volume 2**.

### 3.6  Using the Windows 3.1 SDK to Develop Windows 3.0 Applications

The SDK allows you to create applications for either Windows 3.0 or 3.1. If you write your application carefully, you can create a single application that is compatible with Windows 3.0 but also takes advantage of newer features when the application is run with Windows 3.1. For a comprehensive discussion of this topic, see Chapter 3, "Creating Windows Applications," in *Microsoft Windows Getting Started*.

### 3.7  Using a Serial Device on COM1 with the Debugging Version of Windows

The debugging version of Windows writes information to the COM1 port as the default destination. If you want to use a serial device on COM1, you must redirect the debugging output to a file by adding a line of the following form to the [debug] section of the \SYSTEM.INI file:

    OutputTo=C:\ABC.TXT

You can replace C:\ABC.TXT with any valid filename or device (COM2, NUL, or LPT1) to which output is redirected. Using NUL prevents the debugging version from writing any debugging information to COM1. If you use NUL, you can use DBWIN to view debugging information.

### 3.8  DDESpy

If you use Microsoft DDESpy (DDESPY.EXE) with the debugging version of Windows and the application you are investigating sends a WM_DDE_UNADVISE message with the aItem atom set to NULL, DDESpy will cause a GP fault.

### 3.9 Dialog Editor

You should not use Microsoft Dialog Editor (DLGEDIT.EXE) with a 256-color video driver from Windows version 3.0. Instead, you should use a 256-color video driver from Windows 3.1 or a 
16-color driver. If you must use a 256-color driver from Windows 3.0, you can prevent problems with Dialog Editor by turning off the Grid option in the Arrange Settings dialog box.

4.0 Other Documents
------------------

The following list describes other online documents that contain important information about the Windows 3.1 SDK that is not included in the online Microsoft Windows Help or Microsoft QuickHelp documentation.

| Document     | Contents                                                                          |
|--------------|-----------------------------------------------------------------------------------|
| WINDOWS.MD   | Information about changes to WINDOWS.H, including STRICT enhancements.            |
| ROBUST.MD    | Information about how to build robust applications.                               |
| WINDOWSX.MD  | Information about using the message crackers and control functions in WINDOWSX.H. |
| README.SDK   | Other sources of information that complement the Windows SDK, and instructions for setting your PATH, INCLUDE, and LIB environment variables.                                           |