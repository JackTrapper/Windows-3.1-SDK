**WINDOWS.MD**

SDK windows.h Enhancements
==========================

The 3.1 windows.h header file contains various features that make application
development faster and easier by helping you find problems as you compile your
code.  These improvements include:

- STRICT option provides stricter type checking, helping you find type mismatch errors quickly.

- windows.h has been completely reorganized so that related functions, types, structures, and constants are grouped together.

- New UINT type used for 32-bit Windows upward compatibility

- New unique typedefs for all handle types, such as HINSTANCE and HMODULE.

- Various constants and typedefs missing in the 3.0 windows.h have been added.

- Windows 3.0 compatibility: the 3.1 windows.h can be used to compile applications that run under 3.0.

- Proper use of "const" for API pointer parameters and structure fields where pointer is read-only.

- C++ compatibility: windows.h can now be included without modification for use with the C++ language.

- -W4 and ANSI compatibility: windows.h can be used with the highest degree of compiler warning (-W3 or -W4) without generating warnings.  
  windows.h is ANSI compatible.



Catching Compile-Time Errors: The windows.h STRICT option
---------------------------------------------------------

The new windows.h supports an option that allows you to enable the strictest
possible compiler error checking in order to help you find programming errors
when you compile your app, rather than at runtime.

The idea is that you #define STRICT before #including windows.h, which causes
the various types and function prototypes in windows.h to be declared in a
way that enforces very strict type checking.  For example, without STRICT, it
is possible to pass an HWND to a function that requires an HDC without any
kind of compiler warning: with STRICT #defined, this results in a compiler
error.

Specific features provided by the STRICT option include:

- Strict handle type checking (you can't pass an HWND where an HDC is
      declared).

- Correct and more consistent declaration of certain parameter and return
      value types (e.g., GlobalLock returns void FAR* instead of LPSTR).

- Fully prototyped typedefs for all callback function types
      (e.g., dialog procs, hook procs, and window procs)

- Windows 3.0 backward compatible: STRICT can be used with the 3.1
      windows.h for creating applications that will run under Windows 3.0.

- The COMM DCB and COMSTAT structures are now declared in an ANSI
      compatible way.


Why use STRICT?
---------------

The best way to think of STRICT is as a way for you to get the most out of
the error checking capabilities build into today's modern C and C++
compilers.  STRICT is of great benefit especially with code under development,
because it helps you catch bugs right away when you compile your code, rather
than having to track it down with a debugger.  By catching certain kinds of
bugs right away, it's less likely that you'll ship your applications with
bugs that weren't encountered in testing.

STRICT also makes it easier to migrate your code to the 32-bit Windows
platform later, because it will help you locate and deal with type
incompatibilities that will arise when migrating to 32 bits.

It's not very difficult to convert your application to use STRICT, and it can
be done in stages if needed.

In order to take advantage of the STRICT option, you will probably have to
make some simple changes to your source code (described in detail later).

We think you'll find that STRICT makes modifying, maintaining, and even
reading your code much easier, and well worth the effort to convert your
application.

Unless you #define STRICT, your 3.0 applications will compile just fine with
windows.h without any modifications.  The type declarations for many of the
Windows APIs and callback functions have changed, but unless you #define
STRICT the new declarations are 100% compatible with the old declarations.


Windows.h Enhancement details
-----------------------------

New typedefs, constants, and helper macros
------------------------------------------

The following new typedefs and constants have been added to windows.h.
All are 3.0 backward compatible:

-  **WINAPI** - Used in place of `FAR PASCAL` in API declarations.  If you
		  are writing a DLL with exported API entry points, you
		  can use this for your own APIs.

- **CALLBACK** - Used in place of `FAR PASCAL` in application callback
		  routines such as window procs and dialog procs

- **LPCSTR** - Same as LPSTR, except used for read-only string pointers.  
		  Typedefed as `const char FAR*`.

- **UINT** - Portable unsigned integer type whose size is determined
		  by host environment (16 bits for Win 3.1).  Synonym for
		  "`unsigned int`".  Used in place of `WORD` except in the rare
		  cases where a 16-bit unsigned quantity is desired even on
		  32-bit platforms.

- **LRESULT** - Type used for declaration of all 32-bit polymorphic return values.

- **LPARAM** - Type used for declaration of all 32-bit polymorphic parameters.

- **WPARAM** - Type used for declaration of all 16-bit polymorphic parameters.

- **MAKELPARAM(low, high)** - Macro used for combining two 16-bit quantities into an LPARAM.

- **MAKELRESULT(low, high)** - Macro used for combining two 16-bit quantities into an LRESULT.

- **MAKELP(sel, off)**  - Macro used for combining a selector and an offset  into a FAR VOID* pointer.

- **SELECTOROF(lp)** - Macro used to extract the selector part of a far ptr.  
			  Returns a UINT.

- **OFFSETOF(lp)** - Macro used to extract the offset part of a far ptr.  
			  Returns a UINT.

- **FIELDOFFSET(type, field)** - Macro used for calculating the offset of
				  a field in a data structure.  The type parameter
				  is the type of structure, and field is the name
				  of the field whose offset is desired.

New handle types:

- **HINSTANCE**       - Instance handle type
- **HMODULE**         - Module handle type
- **HLOCAL**          - Local handle type
- **HGLOBAL**         - Global handle type
- **HTASK**           - Task handle type
- **HFILE**           - File handle type
- **HRSRC**           - Resource handle type
- **HGDIOBJ**         - Generic GDI object handle type (except HMETAFILE)
- **HMETAFILE**       - Metafile handle type
- **HDWP**            - DeferWindowPos() handle
- **HACCEL**          - Accelerator table handle
- **HDRVR**           - Driver handle (3.1 only)


COMSTAT structure change
------------------------

The 3.0 declaration of the **COMSTAT** structure was not ANSI compatible: ANSI
does not allow the use of BYTE-sized bitfield declarations.  In order to
allow `windows.h` to be used with ANSI compilers or with `-W4`, the **COMSTAT**
structure has changed.  The 7 bit fields below are now accessed as byte flags
of the single status field:

| old field name | Bit of status field |
|----------------|---------------------|
| fCtsHold       | CSTF_CTSHOLD        |
| fDsrHold       | CSTF_DSRHOLD        |
| fRlsdHold      | CSTF_RLSDHOLD       |
| fXoffHold      | CSTF_XOFFHOLD       |
| fXoffSent      | CSTF_XOFFSENT       |
| fEof           | CSTF_EOF            |
| fTxim          | CSTF_TXIM           |

No change is not required if you are compiling with `WINVER 0x0300` and are not
using `STRICT`.

If you have code that accesses any of these fields, here's how you have
to change your code:

| Old code                 | New code                                     |
|--------------------------|----------------------------------------------|
| if (comstat.fEof \|\| ...) | if ((comstat.status & CSTF_EOF) \|\| ...)  |
| comstat.fCtsHold = TRUE; | comstat.status \|= CSTF_CTSHOLD;             |
| comstat.fTxim = FALSE;   | comstat.status ~= CSTF_TXIM;                 |

Be careful to properly parenthesize "`&`" expressions.

See `windows.h` for more details.


Making Your Application STRICT Compliant
----------------------------------------

Using STRICT with your existing Windows application code is not very
difficult.  Here's what you need to do:

**1. Decide what you want be STRICT compliant.**

The first step is to decide what you want to be STRICT compliant.

STRICT is most valuable with newly developed code or code is being maintained
or changed regularly.  If you have a lot of stable code that has already been
written and tested, and is not changed or maintained very often, you may
decide that it's not worth the trouble to convert to STRICT.

If you are writing a C++ application, you don't have the option of applying
STRICT to only some of your source files.  Because of the way C++ "type safe
linking" works, you may get linking errors if you mix and match STRICT and
non-STRICT source files in your application.

**2. Enable strict compiler error checking**

First, turn on your compiler's highest level of error checking.  Do this
without turning on STRICT for now.

The best level of compiler error checking for Microsoft compilers is warning
level 3, or -W3.  It's also very useful to specify the -WX flag, which causes
the compiler to treat any warning as an error that fails the compilation.
This can be very helpful, because there are a number of compiler warnings
that are almost always fatal Windows programming errors, such as passing an
incorrect number of arguments to a function.

**3. Change your code to use new STRICT types**

First you need to go through your own source and header files and change type
declarations to use the new types defined in windows.h.  Below are the common
types that should be changed:

- `HANDLE` ??? `HINSTANCE`, `HMODULE`, `HGLOBAL`, `HLOCAL`, etc. as appropriate
- `WORD`   ???  `UINT` (EXCEPT where you really want a 16-bit value even on a 32-bit platform)
- `WORD`   ???  `WPARAM`
- `LONG`   ???  `LPARAM` or `LRESULT` as appropriate
- `FARPROC` ??? `WNDPROC`, `DLGPROC`, `HOOKPROC`, etc. as appropriate  
		(**MakeProcInstance** and **FreeProcInstance** calls require casting)

See **"Strict Conversion Notes"** below for particular things to watch out for
when changing your code.

The `UINT` type is important for 32-bit Windows migration.  On 16-bit windows,
`WORD` and `UINT` are identical: 16 bit unsigned quantities.  On a 32-bit
platform, a `UINT` will be 32 bits and `WORD` is 16 bits. This allows much more
efficient code to be generated on the 32-bit platform where `UINT`s are used.
You should use `WORD` in your code **only** in those places where you want a 16-bit value, even on a 32-bit platform.

You may also want to use `CALLBACK` instead of `FAR PASCAL` in the declaration of your various callback functions, though this isn't necessary.

You may be able to save yourself some work by not converting the body of your
window and dialog procs to use `WPARAM`, `LPARAM`, and `LRESULT` right away, since those types are compatible with `WORD` and `LONG`, even in `STRICT`.

**4. Make sure your functions are declared before use**

In order to compile with `-W3`, all of your application functions must be
properly declared before they are used.  It's best to have all your
declarations in an include file, rather than declaring them in your source
files as needed: it's much easier to maintain your code this way should you
need to change any of the declarations in the future.  Chances are you will
need to be making changes to the function declarations as you change over to
the new STRICT data types.

You can use the compiler `-Zg` option to create header files with proper type
declarations for all the functions declared within it.  If you DO want to use
this option, it's important that you `#define STRICT` **before** you do so. This is
because the parameter and return type declarations generated by `-Zg` aren't
the same as you'd use if you did it by hand: for example, `LPSTR` is
generated as "`char _far *`".  Without `STRICT` defined, the types generated by
`HWND`, `HDC`, and other handle types will all be the same: "`unsigned short`".
With `STRICT`, you'll get something like "`HWND__ _near *`", which you can
globally search-and-replace to the correct type.  You don't **have** to edit
your `-Zg` output in order to proceed with your STRICT conversion, but it will
make your header files easier to read and more useful later.

While it's not strictly necessary to do so, it's a good idea to provide
function parameter names in your function prototypes.  This makes header
files much easier to read, and provides a degree of self-documentation.

**5. Recompile without STRICT and fix resulting warnings**

Without #defining STRICT anywhere, recompile your application and fix any
warnings that result.  At this point, it's super handy if your editor has the
capability of automatically positioning the cursor on the next compiler
error, so you can quickly move from warning to warning and fix them.  Almost
all programming editors have this capability: if you haven't used this
feature before, now is a great time to learn about this.  Check your editor's
manual for more information.

Some common compiler warnings -- and how you should deal with them -- are
described later in this section.  Use the rules found there to make the
appropriate changes to your source.

**6. Run the app to make sure all is well.**

After you've gotten your application to compile cleanly with `-W3`, it's a good
idea to run your app and put it through it's paces to make sure all is well.

**7. #define STRICT**

After you've made a first pass and gotten things to compile cleanly without
`STRICT`, it's time to turn it on and make the next round of changes.

If you've decided that you want to make your entire app STRICT, then the best
and easiest thing to do is `#define STRICT` in your makefile, by passing the
`-DSTRICT` flag to the compiler. If you want to do it on a per-sourcefile
basis, then simply `#define STRICT` in the source file before you `#include
windows.h`.

**8. Recompile and clean up resulting errors**

Once you've made the changes to your window and dialog procs as outlines
above, you're ready to recompile everything.

After turning on STRICT you'll probably get new errors and warnings that
you'll need to go through and clean up.

The list of warnings and errors below will explain how to deal with most of
the problems that arise.


STRICT conversion notes
-----------------------

1. Always declare function pointers with the proper function type, rather
   than `FARPROC`.  You'll need to cast function pointers to and from the
   proper function type when using MakeProcInstance, FreeProcInstance,
   and other functions that take or return a FARPROC:

       BOOL CALLBACK DlgProc(HWND hwnd, UINT msg, WPARAM wParam, LPARAM Param);
       DLGPROC lpfnDlg;
       
       lpfnDlg = (DLGPROC)MakeProcInstance(DlgProc, hinst);
       ...
       FreeProcInstance((FARPROC)lpfnDlg);

2. Take special care with `HMODULE`s and `HINSTANCE`s.  For the most part,
   the Kernel module management functions use `HINSTANCE`s, but there are a
   few APIs that return or accept only `HMODULE`s.

3. If you've copied any API function declarations from `windows.h`, they
   may have changed, and your local declaration may be out of date.  
   Remove your local declaration.

4. Properly cast the results of **LocalLock()** and **GlobalLock()** to the
   proper kind of data pointer.  Parameters to these and other memory
   management functions chould be cast to `LHANDLE` or `GHANDLE`, as
   appropriate.

5. Properly cast the result of **GetWindowWord()** and **GetWindowLong()**
       and the parameters to **SetWindowWord()** and **SetWindowLong()**.

6. When casting **SendMessage()**, **DefWindowProc()**, and **SendDlgItemMsg()** or
   any other function that returns an `LRESULT` or `LONG` to a handle of
   some kind, you must first cast the result to a UINT:

       HBRUSH hbr;

       hbr = (HBRUSH)(UINT)SendMessage(hwnd, WM_CTLCOLOR, ..., ...);

7. The **CreateWindow** and **CreateWindowEx** `hmenu` parameter is sometimes
   used to pass an integer control ID.  In this case you must cast
   this to an `hmenu`:

       HWND hwnd;
       int id;
       
       hwnd = CreateWindow("Button", "Ok", BS_PUSHBUTTON,
             x, y, cx, cy, hwndParent,
             (HMENU)id,      // Cast required here
             hinst,
             NULL);

8. Polymorphic data types (`WPARAM`, `LPARAM`, `LRESULT`, `void FAR*`) should
       be assigned to variables of a known type as soon as possible.  You
       should avoid using them in your own code when the type of the value is
       known.  This will minimize the number of potentially unsafe and
       non-32-bit-portable casting you will have to do in your code.  The
       macro APIs and message cracker mechanisms provided in `windowsx.h` will
       take care of almost all packing and unpacking of these data types, in
       a 32-bit portable way.

9. Become familiar with the common compiler warnings and errors that
       you're likely to encounter as you convert to STRICT.


Common Compiler Warnings and Errors
-----------------------------------

Here are some common compiler warnings and errors you might get when trying
to make your application compile cleanly under warning level 3 or 4, with or
without STRICT.  The error messages shown are for Microsoft C 6.0.

These are also the kinds of warnings and errors you'll recieve as you maintain
STRICT source code.

- `foo.c(24) : warning C4021: 'bar' : too few actual parameters`

    This warning simply means that you're not passing enough parameters to a
    function.  THIS IS ALWAYS A SERIOUS BUG in windows application
    programming even though the compiler treats it as a warning!

- `foo.c(28) : warning C4035: 'foo' : no return value`

   This warning means that a function declared to return a value does not
   return a value.  In older, non-ANSI C code, it was common to declare
   functions that did not return a value with no return type:

      foo(i)
      int i;
      {
         ...
      }

   Functions declared in this manner are treated by the C compiler as being
   declared to return an "int".  If the function does not return anything,
   it should be declared "void":

      void foo(int i)
      {
      }


- `foo.c(19) : warning C4047: no function prototype given`

   This means that a function was used before it was fully prototyped, or
   declared.  It can also arise when a function that takes no arguments
   is not prototyped with void:

      void bar();     /* Should be: bar(void) */
      
      main()
      {
         bar();
      }

- `foo.c(182) : warning C4047: '=' : different levels of indirection`
- `foo.c(249) : warning C4047: 'argument' : different levels of indirection`

    These warnings indicate that you are trying to assign or pass a
    non-pointer type when a pointer type is required.  With STRICT defined,
    all handle types as well as LRESULT, WPARAM, and LPARAM are internally
    declared as pointer types, so trying to pass an int, WORD, or LONG as a
    handle will result in this warning.

    These warnings should be fixed by properly declaring the non-pointer
    values you're assigning or passing.  In the case of special constants
    such as (HWND)1 to indicate "insert at bottom" to the window positioning
    functions, you should use the new #definition such as HWND_BOTTOM.

    Only in rare cases should you suppress a "different levels of
    indirection" warning with a cast: this can often generate incorrect code.

- `foo.c(335) : warning C4049: 'argument' : indirection to different types`

    These warnings indicate that you are passing or assigning a pointer of
    the wrong type.   This is the warning you get if you pass the wrong type
    of handle to a function.  This is because under STRICT all handle types
    are defined as pointers to unique structures.

    To suppress these warnings, fix the type mismatch error in your code.
    Once again, it's dangerous to suppress these warnings with a cast, since
    there may be an underlying type error in your app.

- `foo.c(345) : warning C4024: 'SendMessage' : different types : parameter 3`

    These warnings usually occur with an "indirection to different types" or
    "different levels of indirection" warning to indicate which function parameter
    is incorrect.  Fixing the type warning will fix this warning as well.

- `foo.c(15) : error C2086: 'lParam' : redefinition`

    This error will result if you have inconsistent declarations of
    a variable, parameter, or function in your source code.

- `foo.c(352) : warning C4028: parameter 4 declaration different`
- `foo.c(352) : warning C4113: function parameter lists differed`

    These warnings occur when a function definition doesn't match its
    declaration.  Usually this means that you need to edit a window or dialog
    proc, or a callback function.

    These warnings can also result when you're passing a function to a
    function, and your function is improperly prototyped.  This can occur
    with MakeProcInstance and FreeProcInstance, in which case you must do the
    proper casting to and from FARPROC.


- `foo.c(191) : warning C4135: conversion between different integral types`

    This warning results when a value is converted by the compiler into a
    smaller value type, such as from LONG to int, or int to char.  You're
    being warned because you may lose information from this cast: for
    example, if you cast the int value 256 to a char, the resulting value is
    0, because only the lower 8 bits of the value are preserved.

    If you're sure there are no information-loss problems, you can suppress
    this warning with the appropriate explicit cast to the smaller type.

      char* pch;
      int i;
      
      i = (int)*pch;

- `foo.c(64) : warning C4069: conversion of near pointer to long integer`

   This error results when you cast a near pointer or a handle to a
   32-bit value such as LRESULT, LPARAM, LONG or DWORD.  This warning
   almost always represents a bug, because the hi-order 16 bits of the
   value will contain a non-zero value.  The C compiler first converts
   the 16-bit near pointer to a 32-bit far pointer by placing the current
   data segment value in the high 16 bits, then converts this far pointer
   to the 32-bit value.

   To avoid this warning and ensure that a 0 is placed in the hi 16 bits,
   you must first cast the handle to a UINT:

      HWND hwnd;
      LRESULT result = (LRESULT)(UINT)hwnd;

   In cases where you DO want the 32-bit value to contain a far pointer,
   you can avoid the warning with an explicit cast to a far pointer:

     char near* pch;
     LPARAM lParam = (LPARAM)(LPSTR)pch;

- `foo.c(32) : warning C4305: truncation during conversion of integral type
	    to pointer`

    This error occurs when you cast a 32-bit value such as an LPARAM or
    LRESULT to a 16-bit pointer.  Under STRICT, handles are declared as
    16-bit pointers, so you'll get this error, for example, when you cast the
    result of a SendMessage() to a window handle.

    To avoid this warning, you must first cast the 32-bit value to a UINT
    before casting to the 16-bit pointer or handle:

      HBRUSH hbr = (HBRUSH)(UINT)SendMessage(hwnd, WM_CTLCOLOR, ..., ...);

- `foo.c(31) : warning C4059: segment lost in conversion`

    This warning results when you cast a far pointer directly into a 16-bit
    quantity such as a near pointer, handle, WORD, UINT, or BOOL.

    The compiler is warning you that the segment part of the address is being
    lost in the conversion to a near pointer.  To suppress these warnings
    (after you've determined that you really want to perform the cast), cast
    the far pointer to a DWORD before casting before casting to the 16-bit
    type.  For example:

      void FAR* lp;
      HANDLE h;
      BOOL f;
      
      f = (BOOL)(DWORD)lp;
      h = (HANDLE)(DWORD)lp;

- `foo.c(190) : warning C4018: '<' : signed/unsigned mismatch`

    Signed/unsigned mismatch errors occur when you mix signed and unsigned
    values in an expression.  Often this means that a parameter, variable or
    field is incorrectly declared.  To fix these problems, you first have to
    determine which is the correct type that should be used in the
    expression. Then, change the declaration of the offending expression term.

    Don't use casts (unless you have to) to suppress these warnings, because
    that will often hide an underlying problem.  It's very important that you
    correctly determine the correct type for an expression, because it can
    lead to very subtle bugs like the following:

      int i;
      
      if ((WORD)i) < 0)
      {
         // This code optimized out by compiler
         // (since no unsigned value can be less than 0)
         //
         i = 0;
      }

- `foo.c(25) : error C2147: unknown size`

    This error results from trying to change the value of a void pointer
    with + or +=.  These typically result from the fact that certain Windows
    functions that return pointers to arbitrary types (such as GlobalLock()
    and LocalLock()) are defined to return void FAR* rather than LPSTR.

    To solve these problems, you should assign the void* value to a properly-
    declared variable (with the appropriate cast):

      BYTE FAR* lpb = (BYTE FAR*)GlobalLock(h);
      
      lpb += sizeof(DWORD);


- `foo.c(27) : error C2100: illegal indirection`

    This error typically results from trying to dereference a void pointer.
    This usually results from directly using the return value of GlobalLock()
    or LocalLock() as a pointer.  To solve this problem, assign the return
    value to a variable of the appropriate type (with the appropriate cast)
    before using the pointer:

      BYTE FAR* lpb = (BYTE FAR*)GlobalLock(h);
      
      *lpb = 0;

- `foo.c(231) : warning c4054 : function pointer cast to a data pointer`

    This warning results when casting a function pointer into a data pointer
    of some kind.  If you're sure this is really what you want to do, the
    solution is to first cast the function pointer to a DWORD, then to
    the data pointer:

      WNDPROC lpfnWndProc;
      LPSTR lpch;
      
      lpch = (LPSTR)(DWORD)lpfnWndProc;

- `foo.c(200) : warning c4100 : 'lParam' : unreferenced formal parameter`

    This message can result in callback functions when your code does not use
    certain parameters.  The following construct in your code will suppress
    this warning without generating any code:

      void Foo(UINT param1)
      {
         (void)param1;
      }

   For improved readability, you can also #define a macro: 

     #define UNUSED(p) (void)(p)

   and use it as follows:

      void Foo(UINT param1)
      {
         UNUSED(param1);
      }

- `foo.c(46) : warning C4204: 'AsmFunc' : in-line assembler precludes global optimizations`

    This warning results if you're using any of the global optimization
    flags: -Ogle.  To suppress this warning you must temporarily turn off the
    global optimizations, and do register allocation and loop optimizations
    by hand. It's usually best to split the assembly code out into a separate
    function so that the rest of your C code can benefit from the global
    optimizations.

      #pragma optimize("gle", off)
      
      void AsmFunc(void)
      {
         _asm
         {
            ...
        }
      }

	#pragma optimize("", on)    // Use default optimizations

- `foo.c(934) : warning C4203: 'BigFunc' : function too large for global optimizations`

    This warning results with really big functions when you use any of the
    global optimizations flags -Ogle.  You have two options: either break the
    function up into smaller pieces, or turn off the global optimizations in
    the function and do register allocation and loop optimizations by hand:

      #pragma optimize("gle", off)
      
      void BigFunc(void)
      {
         ...
      }

      #pragma optimize("", on)    // Use default optimizations

   It's best to break the function up, if you can, since that will allow the
    compiler to optimize your code as much as possible.

- `foo.c(193) : warning C4090: different 'const/volatile' qualifiers`

    This message results if you are passing a const pointer to a function
    expecting a non-const pointer, or if you are assigning a const pointer to
    a non-const pointer.  The MAKEINTRESOURCE and MAKEINTATOM macros are now
    declared to return a const pointer.  Errors can result if you assign
    either of these macros to a LPSTR.  To fix this kind of problem with
    these macros, reedeclare the assigned variable as an LPCSTR.
