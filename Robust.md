**ROBUST.TXT**

Building Robust Applications With the Windows 3.1 SDK
=====================================================

This document is a brief guide to using some of the features of the 3.1 SDK
to build robust windows applications.


Use the Debug System Binaries!
------------------------------

Testing your application with the debug system binaries is probably the best
thing you can do to make sure your applications are robust and reliable.  The
debugging versions of the system executables perform all sorts of useful
error checking for you, informing you of any problems that arise with debug
output messages.

The best way to use the debug system is with two machines: a machine for
testing and debugging that has the debug system installed, and a machine for
development.  If you put a monochrome adapter card in your debug machine,
CodeView and debug system output can be directed to the monochrome monitor.

But, not everyone can afford to buy a second machine just for debugging. One
drawback of installing the debug system on your development machine is that
it runs somewhat slower than the standard Windows system, so it slows down
your compiler, editor, and other debugging tools.

If you aren't running with a debugger or a debug terminal, you should run the
DBWIN application so you can see the error and warning messages produced by
the debug system.



Controlling Debug Messages
--------------------------

The DBWIN application allows you to interactively set various system
debugging options.  This application also allows you to view debug messages
from the debug system even when you don't have a debug terminal or debugger
hooked up.

This application replaces the old win.ini flags documented in the Beta 2 SDK,
such as [User] ILoveBear and [Kernel] DebugOptions.

The Dr. Watson WIN.INI options have not changed from the Beta II SDK (see the
Beta II release notes for more information).



## Interpreting Invalid Parameter Messages


The system checks all handles, pointers, structures, indexes, and bitflags
that are passed to system calls.  In most cases, passing an invalid parameter
to the system will fail the system call, but in some cases (e.g., setting
unused bits in bit flags parameters) the system call is executed as usual,
with an appropriate warning message.

When you get an invalid parameter error, you'll see a message similar to the
following:

    USER:CreateWindow+10: Invalid HWND: 0x0000

The displayed address should be at or near the function that was passed the
bogus parameter.  The 0x000 is the value of the invalid parameter.

If you don't recognize the displayed address, it may be because of an invalid
window message parameter.  APIs that take emssages such as SendMessage(),
DispatchMessage() and DefWindowProc() will show an address within the message
validation code: the symbol you'll see for an invalid WM_COMMAND message, for
example, will be PV_WM_COMMAND.

### Invalid Handles

1. NULL or -1 passed when it isn't allowed
2. Passing the wrong type of handle to a function (e.g., HDC instead of HWND)
3. Reuse of a destroyed or deleted handle
4. Use of an unitialized local variable
5. Use of an HDC created by CreateIC() or CreateMetaFile() in a call where that type of DC is not allowed.

### Invalid Pointers

1. NULL pointer when it's not allowed
2. Valid selector, but offset part is past segment limit
3. Valid pointer, but buffer size extends past end of segment
4. Function pointer not properly exported or MakeProcInstance'd.
5. Use of unitialized local variable
6. Passing a read-only pointer when a read-write pointer is required
7. A field in a structure is invalid.  Unfortunately the error message doesn't indicate which field is invalid.

Take special note of #7: if you call CreateBitmapIndirect() with an invalid pointer in the bmBits field, the parameter validation messag will tell you that the lpBitmap parameter to CreateBitmapIndirect() is invalid.

### Invalid flags or values

1. Passing in meaningless bitfields (VERY common with GlobalReAlloc!)
2. Index out of range
3. Value otherwise illegal



Developing 3.0-Compatible Applications with the 3.1 SDK
-------------------------------------------------------

All of the features of the 3.1 SDK can be used to develop applications that
will run under Windows 3.0.  There are two things you must do:

1. #define WINVER 0x0300 before including windows.h

    This ensures that only 3.0 compatible functions, structures, and
    definitions are available for use.  You can do this in your makefile
    with -DWINVER=0x0300 in the compiler command line.

2. Use the -30 switch to RC.

    This marks your executable as a 3.0 application, so that Windows 3.0
    won't prevent it from running with a "This application requires a later
    version of Windows" message.  You will typically run RC twice in your
    makefile: once to produce the .res file (with the -r switch), and a
    second time to combine your linked .exe and .res files into the final
    application.  The -30 flag must be used with the second invocation of RC.



Some Common Programming Errors
------------------------------

Here are some common programming errors that appear more frequently than they
should in shipped Windows applications.  Many of these problems can cause
random system UAEs and other problems under Windows 3.0.  The debug system 
binaries will help you track down these kinds of problems very quickly:

* Passing invalid parameters of all shapes and sizes

* Accessing nonexistent window words.  In 3.0, a SetWindowWord or SetWindowLong call past the end of the allocated window words (as defined with RegisterClass) would trash internal window manager data structures!

* Using handles after they've been deleted or destroyed

* Using a DC after it has been released

* Deleting GDI objects before they're deselected

* Forgetting to delete GDI bitmaps, brushes, pens, or other GDI or USER objects when an application terminates

* Writing past the end of an allocated memory block

* Reading or writing through a memory pointer after it has been freed

* Forgetting to export window procedures and other callbacks

* Forgetting to use MakeProcInstance with dialog procs and other callbacks

* Shipping an app without running it with the debugging KERNEL, USER, and GDI.
