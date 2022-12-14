**WINDOWSX.MD**

Macro APIs and Message Crackers
===============================

The macro APIs, message crackers and control APIs are defined in 
the file windowsx.h.  The new handle types, structures, and 
helper macros as well as the STRICT option defined in winstric.h 
are a part of the standard windows.h.


Macro APIs
----------

Windowsx.h contains a number of new APIs implemented as macros that call
other APIs.  Generally they make your code both easier to read and write, and
they can save you lots of typing.  These macros are all portable to 32-bit
Windows.

`void FAR* WINAPI GlobalAllocPtr(WORD flags, DWORD cb)`

- Same as GlobalAlloc(), except that it returns a far pointer directly.

`void FAR* WINAPI GlobalReAllocPtr(void FAR* lp, DWORD cbNew, WORD flags)`

- Same as GlobalReAlloc(), except that it takes and returns a far pointer.

`BOOL    WINAPI GlobalFreePtr(void FAR* lp)`

- Same as GlobalFree, except used with far pointer alloced (or realloced)
      with functions above.

`BOOL    WINAPI GlobalLockPtr(void FAR* lp)`

- Same as GlobalLock, except used with far pointer.

`BOOL    WINAPI GlobalUnlockPtr(void FAR* lp)`

- Same as GlobalUnlock, except used with far pointer.

`HMODULE WINAPI GetInstanceModule(HINSTANCE hInstance);`

- Maps an instance handle to a module handle.

`void    WINAPI UnlockResource(HGLOBAL hResource);`

- Unlocks a global resource handle locked with LockResource().

`BOOL    WINAPI DeletePen(HPEN hpen)`

- Deletes a pen (with proper typecasting)

`HPEN    WINAPI GetStockPen(int i);`

- Returns one of the stock pens indicated by i (properly cast to HPEN).

`HPEN    WINAPI SelectPen(HDC hdc, HPEN hpenSelect)`

- Selects a pen and returns previously selected pen. (with proper type casting)

`BOOL    WINAPI DeleteBrush(HBRUSH hbr)`
- Deletes a brush (with proper typecasting)

`HBRUSH  WINAPI GetStockBrush(int i);`
- Returns one of the stock brushes indicated by i (properly cast to HBRUSH).

`HBRUSH  WINAPI SelectBrush(HDC hdc, HBRUSH hbrSelect)`
- Selects a brush and returns previously selected brush (with proper type
      casting).

`BOOL    WINAPI DeleteFont(HFONT hfont)`
- Deletes a font (with proper typecasting)

`HFONT   WINAPI GetStockFont(int i);`
- Returns one of the stock fonts indicated by i (properly cast to HFONT)

`HFONT   WINAPI SelectFont(HDC hdc, HFONT hfontSelect)`
- Selects a font and returns previously selected font (with proper type casting).

`BOOL    WINAPI DeleteBitmap(HBITMAP hbm)`
- Deletes a bitmap (with proper typecasting)

`HBITMAP WINAPI SelectBitmap(HDC hdc, HBITMAP hbmSelect)`
- Selects a bitmap and returns previously selected bitmap (with proper
      type casting)

`BOOL    WINAPI DeleteRgn(HRGN hrgn)`
- Deletes a region (with proper typecasting)

`int     WINAPI CopyRgn(HRGN hrgnDst, HRGN hrgnSrc);`
- Copies hrgnSrc to hrgnDst.

`int     WINAPI IntersectRgn(HRGN hrgnResult, HRGN hrgnA, HRGN hrgnB);`
- Intersects hrgnA with hrgnB, setting hrgnResult to the result.

`int     WINAPI SubtractRgn(HRGN hrgnResult, HRGN hrgnA, HRGN hrgnB);`
- Subtracts hrgnB from hrgnA, setting hrgnResult to the result.

`int     WINAPI UnionRgn(HRGN hrgnResult, HRGN hrgnA, HRGN hrgnB);`
- Computes the union of hrgnA and hrgnB, setting hrgnResult to the result.

`int     WINAPI XorRgn(HRGN hrgnResult, HRGN hrgnA, HRGN hrgnB);`
- XORs hrgnA with hrgnB, setting hrgnResult to the result.

`void    WINAPI InsetRect(RECT FAR* lprc, int dx, int dy)`
- Insets the edges of a rectangle by dx and dy.

`HINSTANCE WINAPI GetWindowInstance(HWND hwnd)`
- Returns the instance handle associated with a window.

`DWORD   WINAPI GetWindowStyle(HWND hwnd)`
- Returns the window style of a window.

`DWORD   WINAPI GetWindowExStyle(HWND hwnd)`
- Returns the extended window style of a window

`int     WINAPI GetWindowID(HWND hwnd)`
- Returns the window ID of a window.

`void    WINAPI SetWindowRedraw(HWND hwnd, BOOL fRedraw)`
- Disables or enables drawing in a window, without hiding the window.

`WNDPROC WINAPI SubclassWindow(HWND hwnd, WNDPROC lpfnWndProc)`
- Subclasses a window by storing a new window proc address.  Returns
      previous window proc address.

`BOOL    WINAPI IsMinimized(HWND hwnd)`
- Returns TRUE if hwnd is minimized

`BOOL    WINAPI IsMaximized(HWND hwnd)`
- Returns TRUE if hwnd is maximized

`BOOL    WINAPI IsRestored(HWND hwnd)`
- Returns TRUE if hwnd is restored.

`BOOL    WINAPI IsLButtonDown(void)`
- Returns TRUE if the left mouse button is down.

`BOOL    WINAPI IsRButtonDown(void)`
- Returns TRUE if the right mouse button is down.

`BOOL    WINAPI IsMButtonDown(void)`
- Returns TRUE if the middle mouse button is down.

3.1-only macro APIs
-------------------

`void    WINAPI MapWindowPoints(HWND hwndFrom, HWND hwndTo, POINT FAR* lppt, WORD cpt);`
- Maps `cpt` points at `*lppt` from the coordinate system of `hwndFrom` to that of `hwndTo`.

`void    WINAPI MapWindowRect(HWND hwndFrom, HWND hwndTo, RECT FAR* lprc)`
- Maps a rectangle from the coordinate system of `hwndFrom` to that of `hwndTo`.


Control Message APIs
--------------------

New APIs have been added for use in dealing with the various controls. These
APIs are implemented as macros that call SendMessage(), and they take care of
packing the various parameters into wParam and lParam and casting the return
value as needed.

These macros are fully portable to 32-bit windows: the 32-bit versions will
transparently take into account any differences in parameter packing on the
32-bit platform.

These macros make your source code smaller and more readable.  They're
especially valuable with STRICT in order to prevent type errors and incorrect
message parameter passing.

There is a 1-to-1 correspondence between a control API and a window message
or window manager API.  In the interests of brevity, the control APIs are
simply listed below: for more information, you can check out the macro
definitions in windowsx.h and the documentation for the corresponding window
message.

Some of the new control APIs are usable with Windows 3.1 only, and are not
available if you #define WINVER 0x0300.


Control API Examples
--------------------

Here is an example showing how these new APIs are used.  First, here is
some code that uses old-style SendMessage calls to print all the lines
in an edit control:

	void PrintLines(HWND hwndEdit)
	{
	    int line;
	    int lineLast = (int)SendMessage(hwndEdit, EM_GETLINECOUNT, 0, 0L);

	    for (line = 0; line < lineLast; line++)
	    {
		int cch;
		char ach[80];

		*((LPINT)ach) = sizeof(ach);
		cch = (int)SendMessage(hwndEdit, EM_GETLINE,
			line, (LONG)(LPSTR)ach);

		printf(ach);
		... or whatever ...
	    }
	}

Using control APIs, this code would be simplified as follows:

	void PrintLines(HWND hwndEdit)
	{
	    int line;
	    int lineLast = Edit_GetLineCount(hwndEdit);

	    for (line = 0; line < lineLast; line++)
	    {
		int cch;
		char ach[80];

		cch = Edit_GetLine(hwndEdit, line, ach, sizeof(ach));

		printf(ach);
		... or whatever ...
	    }
	}

The new style code is much easier to read (and write), doesn't generate
compiler warnings, and doesn't have any non-portable casts.

Here is a complete list of the control APIs.  See windowsx.h for more info.

    Static_Enable(hwnd, fEnable)
    Static_GetText(hwnd, lpch, cchMax)
    Static_GetTextLength(hwnd)
    Static_SetText(hwnd, lpsz)
    Static_SetIcon(hwnd, hIcon)
    Static_GetIcon(hwnd, hIcon)

    Button_Enable(hwnd, fEnable)
    Button_GetText(hwnd, lpch, cchMa
    Button_GetTextLength(hwnd)
    Button_SetText(hwnd, lpsz)
    Button_GetCheck(hwnd)
    Button_SetCheck(hwnd, check)
    Button_GetState(hwnd)
    Button_SetState(hwnd, state)
    Button_SetStyle(hwnd, style, fRedraw)

    Edit_Enable(hwnd, fEnable)
    Edit_GetText(hwnd, lpch, cchMax)
    Edit_GetTextLength(hwnd)
    Edit_SetText(hwnd, lpsz)
    Edit_LimitText(hwnd, cchMax)
    Edit_GetLineCount(hwnd)
    Edit_GetLine(hwnd, line, lpch, cchMax)
    Edit_GetRect(hwnd, lprc)
    Edit_SetRect(hwnd, lprc)
    Edit_SetRectNoPaint(hwnd, lprc)
    Edit_GetSel(hwnd)
    Edit_SetSel(hwnd, ichStart, ichEnd)
    Edit_ReplaceSel(hwnd, lpszReplace)
    Edit_GetModify(hwnd)
    Edit_SetModify(hwnd, fModified)
    Edit_LineFromChar(hwnd, ich)
    Edit_LineIndex(hwnd, line)
    Edit_LineLength(hwnd, line)
    Edit_Scroll(hwnd, dv, dh)
    Edit_CanUndo(hwnd)
    Edit_Undo(hwnd)
    Edit_EmptyUndoBuffer(hwnd)
    Edit_SetPasswordChar(hwnd, ch)
    Edit_SetTabStops(hwnd, cTabs, lpTabs)
    Edit_SetWordBreak(hwnd, lpfnWordBreak)
    Edit_FmtLines(hwnd, fAddEOL)
    Edit_GetHandle(hwnd)
    Edit_SetHandle(hwnd, h)
    Edit_GetFirstVisible(hwnd)

    ScrollBar_Enable(hwnd, flags)
    ScrollBar_Show(hwnd, fShow)
    ScrollBar_SetPos(hwnd, pos, fRedraw)
    ScrollBar_GetPos(hwnd)
    ScrollBar_SetRange(hwnd, posMin, posMax, fRedraw)
    ScrollBar_GetRange(hwnd, lpposMin, lpposMax)

    ListBox_Enable(hwnd, fEnable)
    ListBox_GetCount(hwnd)
    ListBox_ResetContent(hwnd)
    ListBox_AddString(hwnd, lpsz)
    ListBox_InsertString(hwnd, lpsz, index)
    ListBox_AddItemData(hwnd, data)
    ListBox_InsertItemData(hwnd, lpsz, index)
    ListBox_DeleteString(hwnd, index)
    ListBox_GetTextLen(hwnd, index)
    ListBox_GetText(hwnd, index, lpszBuffer)
    ListBox_GetItemData(hwnd, index)
    ListBox_SetItemData(hwnd, index, data)
    ListBox_FindString(hwnd, indexStart, lpszFind)
    ListBox_FindItemData(hwnd, indexStart, data)
    ListBox_SetSel(hwnd, fSelect, index)
    ListBox_SelItemRange(hwnd, fSelect, first, last)
    ListBox_GetCurSel(hwnd)
    ListBox_SetCurSel(hwnd, index)
    ListBox_SelectString(hwnd, indexStart, lpszFind)
    ListBox_SelectItemData(hwnd, indexStart, data)
    ListBox_GetSel(hwnd, index)
    ListBox_GetSelCount(hwnd)
    ListBox_GetTopIndex(hwnd)
    ListBox_GetSelItems(hwnd, cItems, lpIndices)
    ListBox_SetTopIndex(hwnd, indexTop)
    ListBox_SetColumnWidth(hwnd, cxColumn)
    ListBox_GetHorizontalExtent(hwnd)
    ListBox_SetHorizontalExtent(hwnd, cxExtent)
    ListBox_SetTabStops(hwnd, cTabs, lpTabs)
    ListBox_GetItemRect(hwnd, index, lprc)
    ListBox_SetCaretIndex(hwnd, index)
    ListBox_GetCaretIndex(hwnd)
    ListBox_SetAnchorIndex(hwnd, index)
    ListBox_GetAnchorIndex(hwnd)
    ListBox_Dir(hwnd, attrs, lpszFileSpec)
    ListBox_AddFile(hwnd, lpszFilename)

    ListBox_SetItemHeight(hwnd, index, cy)  (3.1 only)
    ListBox_GetItemHeight(hwnd, index)      (3.1 only)

    ComboBox_Enable(hwnd, fEnable)
    ComboBox_GetText(hwnd, lpch, cchMax)
    ComboBox_GetTextLength(hwnd)
    ComboBox_SetText(hwnd, lpsz)
    ComboBox_LimitText(hwnd, cchLimit)
    ComboBox_GetEditSel(hwnd)
    ComboBox_SetEditSel(hwnd, ichStart, ichEnd)
    ComboBox_GetCount(hwnd)
    ComboBox_ResetContent(hwnd)
    ComboBox_AddString(hwnd, lpsz)
    ComboBox_InsertString(hwnd, index, lpsz)
    ComboBox_AddItemData(hwnd, data)
    ComboBox_InsertItemData(hwnd, index, data)
    ComboBox_DeleteString(hwnd, index)
    ComboBox_GetLBTextLen(hwnd, index)
    ComboBox_GetLBText(hwnd, index, lpszBuffer)
    ComboBox_GetItemData(hwnd, index)
    ComboBox_SetItemData(hwnd, index, data)
    ComboBox_FindString(hwnd, indexStart, lpszFind)
    ComboBox_FindItemData(hwnd, indexStart, data)
    ComboBox_GetCurSel(hwnd)
    ComboBox_SetCurSel(hwnd, index)
    ComboBox_SelectString(hwnd, indexStart, lpszSelect)
    ComboBox_SelectItemData(hwnd, indexStart, data)
    ComboBox_Dir(hwnd, attrs, lpszFileSpec)
    ComboBox_ShowDropdown(hwnd, fShow)

    ComboBox_GetDroppedState(hwnd)              (3.1 only)
    ComboBox_GetDroppedControlRect(hwnd, lprc)  (3.1 only)
    ComboBox_GetItemHeight(hwnd)                (3.1 only)
    ComboBox_SetItemHeight(hwnd, cyItem)        (3.1 only)
    ComboBox_GetExtendedUI(hwnd)                (3.1 only)
    ComboBox_SetExtendedUI(hwnd, flags)         (3.1 only)


Message Cracker Macros
----------------------

The message cracker macros provide a convenient, portable, and type-safe
mechanism for dealing with window messages, their parameters, and their
return values.

The basic idea is that instead of having to pick apart message parameters with
casts and HIWORD/LOWORD and such, you simply declare and implement a function
that has the properly typed parameters and return value.  The message
crackers efficiently pick apart the message parameters, call your function,
and return the appropriate value from the window message.

Message forwarder macros allow you to forward a message via DefWindowProc(),
SendMessage(), or CallWindowProc(). The macros do the work of packing
explicitly typed arguments into wParam and lParam and calling the appropriate
function.

With these macros, you don't have to worry about what parameters go where and
what kind of casting you need to do.  They are also portable to 32-bit
windows (where some of the message parameters have changed).


Using Message Crackers and Forwarders:
--------------------------------------

For each window message, there are two macros: a cracker and a forwarder. To
see how these macros work, let's use the WM_CREATE message as an example.
Here is a code fragment showing how a window proc could use message crackers
to handle the WM_CREATE message.  For now, our WM_CREATE message processing
will simply call DefWindowProc().

**Note**: The following examples use STRICT-style declarations, but you can use message crackers without STRICT.

    // Message handler function prototype (declared in a .h file)
    //
    BOOL MyCls_OnCreate(HWND hwnd, CREATESTRUCT FAR* lpCreateStruct);

    // Window proc for class "MyCls" (defined in a .c file)
    //
    LRESULT _export CALLBACK MyCls_WndProc(HWND hwnd, UINT msg, WPARAM wParam, LPARAM lParam)
    {
	switch (msg)
	{
	case WM_CREATE:
	    return HANDLE_WM_CREATE(hwnd, wParam, lParam, MyCls_OnCreate);

	default:
	    return DefWindowProc(hwnd, msg, wParam, lParam);
	}
    }

    // WM_CREATE message handler function.
    // For now, just calls DefWindowProc.
    //
    BOOL MyCls_OnCreate(HWND hwnd, CREATESTRUCT FAR* lpCreateStruct)
    {
	return FORWARD_WM_CREATE(hwnd, lpCreateStruct, DefWindowProc);
    }

Some important points:

* You must declare and implement a function to handle the message, which
      must have a particular "signature" (the order and type of the
      parameters, and the type of the return value, if any).

* You pass this message handler function as the last parameter
      to the HANDLE_WM_XXX() function.  This function must be declared and
      fully prototyped before being used with a message cracker.

* You must always return the value returned by the HANDLE_WM_XXX()
      function, even if the message handler function is declared void.

* The FORWARD_WM_XXX function always has the same signature (parameters
      and return type) as its corresponding message handler function, with
      the addition of the last parameter, which is the API or function to be
      used to forward the message.

* You don't need to use message crackers to handle all your messages.
      Old-style message handling code can be mixed with message crackers
      in the same window proc.

* By convention, message handler functions are named "Class_OnXXX", where
      Class is the window class name and XXX is the name of the corresponding
      message, minus the "WM_" and using mixed case instead of all caps.
      This is just a convention: you can use any name you like for the
      function (although you can't alter its signature).


Saving time and improving readability with HANDLE_MSG
-----------------------------------------------------

The **HANDLE_MSG()** macro can be used to reduce the amount of "noise" in your
window procs and save yourself some typing.  The **HANDLE_MSG** macro replaces
the "`case WM_XXX:`", the **HANDLE_WM_XXX** call, and the return.  So, instead of

    case WM_CREATE:
    return HANDLE_WM_CREATE(hwnd, wParam, lParam, MyCls_OnCreate);

you can just type:

    HANDLE_MSG(hwnd, WM_CREATE, MyCls_OnCreate);

**HANDLE_MSG** is optional.  Some people prefer to "spell out" the goings-on in their window procedures, and others prefer the convenience and brevity of the **HANDLE_MSG** form.

**HANDLE_MSG** requires that you name your window proc message parameters `wParam` and `lParam`. The examples in this document use **HANDLE_MSG**.


How Message Crackers Work
-------------------------

To see how message crackers work, we'll take a look at the definition of the
message cracker macros as found in windowsx.h (these definitions are somewhat
simplified for the sake of clarity):


    /* BOOL Cls_OnCreate(HWND hwnd, CREATESTRUCT FAR* lpCreateStruct) */

    #define HANDLE_WM_CREATE(hwnd, wParam, lParam, fn) \
	((fn)(hwnd, (CREATESTRUCT FAR*)lParam) ? 0L : (LRESULT)-1L)

    #define FORWARD_WM_CREATE(hwnd, lpCreateStruct, fn) \
	(BOOL)(DWORD)(fn)(hwnd, WM_CREATE, 0, (LPARAM)lpCreateStruct)

The comment shows the signature of the message handler function that you must
declare and implement.

Essentially all these macros do is convert the wParam and lParam message
parameters to and from specific, explicitly typed parameters and invoke a
function.

The **HANDLE_WM_CREATE** macro calls the handler function with the appropriate
parameters, obtained by casting `wParam` and `lParam` appropriately.  The return
value of the handler value is mapped to the proper `LRESULT` return value, in
this case `0L` or `-1L`.  If the handler function returns no value, then `0L` is returned.

The **FORWARD_WM_CREATE** macro calls the supplied message function (which must
have the same signature as DefWindowProc) with the proper `hwnd`, `wParam`, and
`lParam` parameters calculated from the parameters supplied to the macro.


The **HANDLE_MSG()** macro is quite simple too:

    #define HANDLE_MSG(hwnd, message, fn)    \
	case message: return HANDLE_##message(fn, hwnd, wParam, lParam)

It simply does the "case" for you, and returns the result of the proper
**HANDLE_WM_XXX** function.  The message parameter names `wParam` and `lParam` are
hard-wired into this macro.


Message Cracker Examples
------------------------

Here is a more detailed example of a window procedure for the "Template"
class that uses message crackers and message forwarders:

    // Excerpt from header file for class Template

    // Window procedure prototype

    LRESULT _export CALLBACK Template_WndProc(HWND hwnd, WORD msg, WPARAM wParam, LPARAM lParam)

    // Default message handler

    #define Template_DefProc    DefWindowProc

    // Template class message handler functions, declared in a .h file:
    //
    void Template_OnMouseMove(HWND hwnd, int x, int y, UINT keyFlags);
    void Template_OnLButtonDown(HWND hwnd, BOOL fDoubleClick, int x, int y, UINT keyFlags);
    void Template_OnLButtonUp(HWND hwnd, int x, int y, UINT keyFlags);
    HBRUSH Template_OnCtlColor(HWND hwnd, HDC hdc, HWND hwndChild, int type);


    // Exerpt from c source file for class Template

    // Template window procedure implementation.
    //
    LRESULT _export CALLBACK Template_WndProc(HWND hwnd, WORD msg, WPARAM wParam, LPARAM lParam)
    {
	switch (msg)
	{
	    HANDLE_MSG(hwnd, WM_MOUSEMOVE,Template_OnMouseMove);
	    HANDLE_MSG(hwnd, WM_LBUTTONDOWN, Template_OnLButtonDown);
	    HANDLE_MSG(hwnd, WM_LBUTTONDBLCLK, Template_OnLButtonDown);
	    HANDLE_MSG(hwnd, WM_LBUTTONUP, Template_OnLButtonUp);
	default:
	    return Template_DefProc(hwnd, msg, wParam, lParam);
	}
    }

    // Message handler function implementations:
    //
    void Template_OnMouseMove(HWND hwnd, int x, int y, UINT keyFlags)
    {
	...
    }

    void Template_OnLButtonDown(HWND hwnd, BOOL fDoubleClick, int x, int y, UINT keyFlags)
    {
	...
    }

    void Template_OnLButtonUp(HWND hwnd, int x, int y, UINT keyFlags)
    {
	...
    }

    HBRUSH Template_OnCtlColor(HWND hwnd, HDC hdc, HWND hwndChild, int type)
    {
	switch (type)
	{
	case CTLCOLOR_BTN:
	    // Pass the WM_CTLCOLOR message on to the parent, and use the edit control
	    // colors.
	    //
	    return FORWARD_WM_CTLCOLOR(GetParent(hwnd),
		     hdc, hwndChild, CTLCOLOR_EDIT, SendMessage);
	    break;

	default:
	    // Perform default processing of the message.
	    //
	    return FORWARD_WM_CTLCOLOR(hwnd, hdc, hwndChild, type, Template_DefProc);
	}
    }



Message Handler Function Signatures
-----------------------------------

For the most part, the **OnXXX()** message handler function parameters have the
same name and type as those shown in the documentation of the corresponding
window message. To find out exactly what those messages are, look at the
commented function prototype in windowsx.h before the message cracker for the
message you're interested in.

There are a few cases where the **OnXXX()** functions work a bit differently than
the corresponding window message:

 * **OnCreate** (and **OnNCCreate()**) must return `TRUE` if all is well, or 
      the window will not be created and **CreateWindow()** will return `NULL`.

* The signatures for **OnKey** and **OnButtonDown()** functions are a little
      different from their corresponding messages.  **OnKey()** handles both key
      up and key down messages with the `fDown` parameter. The **OnButtonDown**
      functions handle double click messages too, with the `fDoubleClick`
      parameter (though you must be sure to register your window class with
      `CS_DBLCLKS` if you want to handle double clicks).

* The **OnChar** function is not passed the virtual key or key flags
      information, as this information is not usable in a `WM_CHAR` handling.
      This is because different virtual keys can generate the same `WM_CHAR`
      messages, and certain key macro processors will generate `WM_CHAR`
      messages with no virtual key or flags.


Improving reusability: Template_DefProc
---------------------------------------

For every window class, there is a function that must be called to perform
the default processing for that window.  Normally, this call is
DefWindowProc, but if you're subclassing a window, it's **CallWindowProc**, or if
you're implementing an MDI child window, it's **DefMDIChildProc**, etc.

A very common programming mistake is to copy code from another window proc
without making the appropriate change to the default message handler
function. This can lead to subtle, hard to track down bugs.

This is the purpose of the **Template_DefProc** macro defined and used in the
example above.  Every class should have an appropriate **XXX_DefProc macro** (or
function) defined which will perform default message processing.

In the example above, the default message handler is DefWindowProc, so
**Template_DefProc** is defined as follows:

    #define Template_DefProc   DefWindowProc.

For an MDI child window, it might be:

    #define MdiWnd_DefProc     DefMDIChildProc

The advantage of this scheme is that to steal code from another window proc,
you need only change the class name prefix, and the proper default handling
will be taken care of automatically.


Private and Registered Window Messages
--------------------------------------

Message crackers and forwarders work well with new window messages that you
define.  You must write a message cracker and forwarder macro for the new
message -- the easiest way to do this is to copy and modify existing macros
from windowsx.h.

If your new message value is a constant (e.g., `WM_USER`+100), then you can use
**HANDLE_MSG()** to handle the message in your window procedure.  However, if
your new message is registered with **RegisterWindowMessage()**, **HANDLE_MSG()**
can't be used, because variables cannot be used as switch statement case
values: only constants can.  In this case, you can handle it as follows:

    // In Template class initialization code:
    //
    UINT WM_NEWMESSAGE = 0;

    WM_NEWMESSAGE = RegisterWindowMessage("WM_NEWMESSAGE");

    ...

    // In Template_WndProc(): window procedure:
    //
    LRESULT _export CALLBACK Template_WndProc(HWND hwnd, WORD msg, WPARAM wParam, LPARAM lParam)
    {
	if (msg == WM_NEWMESSAGE)
	    HANDLE_WM_NEWMESSAGE(hwnd, wParam, lParam, Template_OnNewMessage);

	switch (msg)
	{
	    HANDLE_MSG(hwnd, WM_MOUSEMOVE, Template_OnMouseMove);
	...
    }


Message Crackers and Window Instance Data
-----------------------------------------

It's very common for a window to have some additional "instance data"
associated with it that is kept in a separate data structure allocated by the
application.  This separate data structure is associated with its
corresponding window by storing a pointer to the structure in a 
specially-named window property or in a window word (allocated by setting 
the `cbWndExtra` field of the `WNDCLASS` structure when the class is registered).

The message crackers fully support this style of programming by allowing you
to pass a pointer to the instance data as the first parameter to the message
handlers instead of a window handle.  The following example should make this
clear:

```
// Window instance data structure.  Must include window handle field.
//
typedef struct _FOO
{
    HWND hwnd;
    int otherStuff;
} FOO;

// "Foo" window class was registered with cbWndExtra = sizeof(FOO*), so we
// can use a window word to store back pointer.  Window properties can also
// be used.
//
// These macros get and set the hwnd -> FOO* backpointer.  Use GetWindowWord
// or GetWindowLong as appropriate based on the default size of data pointers.
//
#if (defined(M_I86SM) | defined(M_I86MM))
#define Foo_GetPtr(hwnd)        (FOO*)GetWindowWord((hwnd), 0)
#define Foo_SetPtr(hwnd, pfoo)  (FOO*)SetWindowWord((hwnd), 0, (WORD)(pfoo))
#else
#define Foo_GetPtr(hwnd)        (FOO*)GetWindowLong((hwnd), 0)
#define Foo_SetPtr(hwnd, pfoo)  (FOO*)SetWindowLong((hwnd), 0, (LONG)(pfoo))
#endif

// Default message handler

#define Foo_DefProc DefWindowProc

// Message handler functions, declared with a FOO* as their first argument,
// rather than an HWND.  Other than that, their signature is identical to
// that shown in windowsx.h.
//
BOOL Foo_OnCreate(FOO* pfoo, CREATESTRUCT FAR* lpcs);
void Foo_OnPaint(FOO* pfoo);

//
// Code to register the Foo window class:
//
BOOL Foo_Init(HINSTANCE hinst)
{
    WNDCLASS cls;

    cls.hCursor         = ...;
    cls.hIcon           = ...;
    cls.lpszMenuName    = ...;
    cls.hInstance       = hinst;
    cls.lpszClassName   = "Foo";
    cls.hbrBackground   = ...;
    cls.lpfnWndProc     = Foo_WndProc;
    cls.style           = CS_DBLCLKS;
    cls.cbWndExtra      = sizeof(FOO*);  // room for instance data ptr
    cls.cbClsExtra      = 0;

    return RegisterClass(&cls);
}

// The window proc for class "Foo".  This demonstrates how instance data is
// attached to a window and passed to the message handler functions.  It's
// fully STRICT enabled, and Win 3.0 compatible.
//
LRESULT CALLBACK _export Foo_WndProc(HWND hwnd, UINT msg, WPARAM wParam, LPARAM lParam)
{
    FOO* pfoo = Foo_GetPtr(hwnd);

    if (pfoo == NULL)
    {
	// If we're creating the window, then try to allocate it.
	//
	if (msg == WM_NCCREATE)
	{
	    // Create the instance data structure, set up the hwnd backpointer
	    // field, and associate it with the window.
	    //
	    pfoo = (FOO*)LocalAlloc(LMEM_FIXED | LMEM_ZEROINIT, sizeof(FOO));

	    // If an error occured, return 0L to fail the CreateWindow call.
	    // This will cause CreateWindow() to return NULL.
	    //
	    if (pfoo == NULL)
		return 0L;

	    pfoo->hwnd = hwnd;
	    Foo_SetPtr(hwnd, pfoo);

	    // NOTE: the rest of the FOO structure should be initialized
	    // inside of Template_OnCreate() (or Template_OnNCCreate()).  Further
	    // creation data may be accessed through the CREATESTRUCT FAR* parameter.
	    //
	}
	else
	{
	    // It turns out WM_NCCREATE is NOT necessarily the first message
	    // recieved by a top-level window (WM_GETMINMAXINFO is).
	    // Pass messages that precede WM_NCCREATE on through to
	    // Foo_DefProc
	    //
	    return Foo_DefProc(hwnd, msg, wParam, lParam);
	}
    }

    if (msg == WM_NCDESTROY)
    {
	// The window is being destroyed: free up the FOO structure.
	//
	// NOTE: If you want to handle WM_NCDESTROY with a message
	// cracker (NOT RECOMMENDED), you can uncomment the lines
	// below.
	//
	//LRESULT result = HANDLE_MSG(hwnd, WM_NCDESTROY, Client_OnNCDestroy);
	//
	// Deallocation of any fields of the FOO structure should
	// be done inside the OnNCDestroy() function.
	//
	LocalFree((HLOCAL)pfoo);

	pfoo = NULL;
	Foo_SetPtr(hwnd, NULL);

	//return result;
    }

    switch (msg)
    {
	HANDLE_MSG(pfoo, WM_CREATE, Foo_OnCreate);
	HANDLE_MSG(pfoo, WM_PAINT, Foo_OnPaint);
	...

    default:
	return Foo_DefProc(hwnd, msg, wParam, lParam);
    }
}
```

Message Crackers and Dialog Procedures
--------------------------------------

Dialog procedures are different from window procedures in that they return a
`BOOL` indicating whether the message was processed rather than an `LRESULT`. For
this reason, you can't use **HANDLE_MSG()**: you must invoke the message cracker
macro explicitly.

Here's an example that shows how you'd use message crackers in a dialog proc:

    BOOL MyDlg_OnInitDialog(HWND hwndDlg, HWND hwndFocus, LPARAM lParam);
    void MyDlg_OnCommand(HWND hwnd, int id, HWND hwndCtl, UINT codeNotify);

    BOOL _export CALLBACK MyDlg_DlgProc(HWND hwndDlg, UINT msg, WPARAM wParam, LPARAM lParam)
    {
	switch (msg)
	{
	//
	// Since HANDLE_WM_INITDIALOG returns an LRESULT,
	// we must cast it to a BOOL before returning.
	//
	case WM_INITDIALOG:
	    return (BOOL)HANDLE_WM_INITDIALOG(hwndDlg, wParam, lParam, MyDlg_OnInitDialog);

	case WM_COMMAND:
	    HANDLE_WM_COMMAND(hwndDlg, wParam, lParam, MyDlg_OnCommand);
	    return TRUE;
	    break;

	default:
	    return FALSE;
	}
    }

If you'd like to process messages that return values such as `WM_ERASEBKGND` in
your dialog proc, or would like to make it easier to share code between your
window procs and your dialog procs, you may want to make use of the
techniques shown later in *"Dialog Procs: A Better Way"*.


Message Crackers and Window Subclassing
---------------------------------------

Message crackers can also be used to simplify window subclassing code. With
message crackers, unprocessed messages must be forwarded using the
appropriate **FORWARD_WM_\*** macro.  When you are subclassing a window, the
proper way to forward unprocessed messages is by calling **CallWindowProc()**,
passing it the previous window proc address, along with the four standard
window message parameters.

The **FORWARD_WM_\*** macros can't be used directly with **CallWindowProc()**, because
they can only invoke functions having the standard window proc signature:
**CallWindowProc** has an extra `WNDPROC` parameter.

This problem is handled easily by simply declaring **XXX_DefProc** as a function
instead of a macro.  Your function must call **CallWindowPro**c instead of
calling **DefWindowProc**:

Here's an example showing how this works:

    // Global variable that holds the previous window proc address of
    // the subclassed window:
    //
    WNDPROC Foo_lpfnwpDefProc = NULL;

    // Code fragment to subclass a window and store previous wndproc value:
    //
    void SubclassFoo(HWND hwndFoo)
    {
	extern HINSTANCE g_hinstFoo;    // Global application instance handle
	...

	// SubclassWindow() is a macro API that calls SetWindowLong()
	// as appropriate to change the window proc of hwndFoo.
	//
	Foo_lpfnwpDefProc = SubclassWindow(hwndFoo,
		(WNDPROC)MakeProcInstance( (FARPROC)Foo_WndProc, g_hinstFoo));
	...
    }

    // Default message handler function
    //
    // This function invokes the superclasses' window procedure.  It
    // must be declared with the same signature as any window proc,
    // so it can be used with the FORWARD_WM_* macros.
    //
    LRESULT Foo_DefProc( HWND hwnd, UINT msg, WPARAM wParam, LPARAM lParam)
    {
	return CallWindowProc(Foo_lpfnwpDefProc, hwnd, msg, wParam, lParam);
    }

    // Foo window procedure.  Everything here is the same as in the
    // normal non-subclassed case: the differences are encapsulated in
    // Foo_DefProc.
    //
    LRESULT CALLBACK Foo_WndProc(HWND hwnd, UINT msg, WPARAM wParam, LPARAM lParam)
    {
	switch (msg)
	{
	    HANDLE_MSG(hwnd, WM_CHAR, Foo_OnChar);
	    ...
	default:
	    //
	    // Be sure to call Foo_DefProc(), NOT DefWindowProc()!
	    //
	    return Foo_DefProc(hwnd, msg, wParam, lParam);
	}
    }

    // Message handlers
    //
    void Foo_OnChar(HWND hwnd, UINT ch, int cRepeat)
    {
	if (ch == ... || whatever)
	{
	    // handle it here
	}
	else
	{
	    // Forward the message on to Foo_DefProc
	    //
	    FORWARD_WM_CHAR(hwnd, ch, cRepeat, Foo_DefProc);
	}
    }

Dialog Procs: A Better Way
--------------------------

There are two longstanding sources of confusion and bugs in Windows dialog
procs: 

a. there is no way to return a value from a message handled by a
dialog proc, and 
b. it's not possible to execute the default dialog behavior
for a message before executing your own code in your dialog proc.

Here is a simple solution to both of these problems that is compatible with
both Windows 3.0 and 3.1.  It unifies the way window procedures and dialog
procedures are coded, and it works very nicely with message crackers.

**Note**: These techniques, like message crackers, are completely optional.  You
can use these techniques with or without message crackers.

You can code a dialog proc just as if it were a window proc: you have the
same freedom to return values and execute the default dialog processing
messages as you do with window procs.  It's also easier to copy or share code
between dialog and window procs.


Executing default dialog procedure functionality
------------------------------------------------

Although Windows provides the **DefDlgProc()** API, it can't be used as is for
our purposes because its implementation calls the dialog proc again.  If we
called it from our dialog proc, the dialog proc would be called again,
recursively, until we run out of stack space and crash.  To prevent this
infinite recursion, we need only detect that we're being called recursively
and return `FALSE`, which will cause the default processing to be performed.


Returning message results from dialog procedures
------------------------------------------------

Windows 3.0 and 3.1 both support a general mechanism for returning values
from messages handled in dialog procedures.  Essentially, you store the
return value with **SetWindowLong()**, which will get returned from the message
when your dialog proc returns `TRUE`.

There are some screwy special cases you have to worry about: in some cases,
the return value must be returned in place of the BOOL return value.


How It Works
------------

Here is some code that shows how all this comes together:

    // function prototypes in header file..

    BOOL CALLBACK _export MyDlg_OldProc(HWND hwndDlg, UINT msg, WPARAM wParam, LPARAM lParam);

    LRESULT MyDlg_DlgProc(HWND hwndDlg, UINT msg, WPARAM wParam, LPARAM lParam);


    // implementation in .c file..

    // static (or global) variable for preventing infinite recursion
    //
    static BOOL fMyDlgRecurse = FALSE;

    BOOL CALLBACK _export MyDlg_OldProc(HWND hwndDlg, UINT msg, WPARAM wParam, LPARAM lParam)
    {
	LRESULT result;

	// Check for possible recursion.  If so, just return FALSE
	// after clearing the recursion flag, to ensure that the default
	// processing is executed.
	//
	if (fMyDlgRecurse)
	{
	    fMyDlgRecurse = FALSE;
	    return FALSE;
	}

	result = MyDlg_DlgProc(hwndDlg, msg, wParam, lParam);

	// Here if we handled the message, and want to return result.
	//
	switch (msg)
	{
	// The following messages are special-cased by the dialog manager,
	// and assumed to be returned as a BOOL from the dialog proc:
	//
	case WM_INITDIALOG:
	case WM_CTLCOLOR:
	case WM_COMPAREITEM:
	case WM_VKEYTOITEM:
	case WM_CHARTOITEM:
	case WM_QUERYDRAGICON:
	    return (BOOL)LOWORD(result);

	default:
	//
	// All other messages use the DWL_MSGRESULT window words:
	//
	    SetWindowLong(hwndDlg, DWL_MSGRESULT, (LPARAM)result);
	    return TRUE;
	}
    }

    LRESULT MyDlg_DlgProc(hwndDlg, msg, wParam, lParam)
    {
	switch (msg)
	{
	HANDLE_MSG(hwndDlg, WM_INITDIALOG, MyDlg_OnInitDialog);
	HANDLE_MSG(hwndDlg, WM_COMMAND, MyDlg_OnCommand);

	default:
	    // Call DefDlgProc to invoke default dialog processing
	    // for messages we don't handle ourself.  Set recursion
	    // flag before we go, so MyDlg_OldDlgProc() knows to
	    // return FALSE.
	    //
	    fMyDlgRecurse = TRUE;
	    return DefDlgProc(hwndDlg, msg, wParam, lParam);
	}
    }

You must declare a static (or global) `BOOL` variable that is initialized to
`FALSE`.  It's safe to use the same global variable for all your dialogs, even
if one dialog procedure brings up another dialog box.

What's important is that the same boolean variable be used in the
"OldDlgProc" and before the call to **DefDlgProc**: a local `BOOL` variable must
**not** be used, or infinite recursion will result.


A Simplified Example Using Predefined Macro APIs
------------------------------------------------

Three macro APIs are defined in `windowsx.h` that drastically simplify the code
shown above.  They are **SetDlgMsgResult**, **DefDlgProcEx**, and
**CheckDefDlgRecursion**.

Here's the same dialog code, this time using these macro APIs:

    // prototypes..

    BOOL CALLBACK _export MyDlg_OldProc(HWND hwndDlg, UINT msg, WPARAM wParam, LPARAM lParam);

    LRESULT MyDlg_DlgProc(HWND hwndDlg, UINT msg, WPARAM wParam, LPARAM lParam);


    // implementation..

    static BOOL fDefDlgEx = FALSE;

    BOOL CALLBACK _export MyDlg_OldProc(HWND hwndDlg, UINT msg, WPARAM wParam, LPARAM lParam)
    {
	CheckDefDlgRecursion(&fDefDlgEx);
	return SetDlgMsgResult(hwndDlg, msg, MyDlg_DlgProc(hwndDlg, msg, wParam, lParam));
    }

    LRESULT MyDlg_DefProc(HWND hwndDlg, UINT msg, WPARAM wParam, LPARAM lParam)
    {
	return DefDlgProcEx(hwnd, msg, wParam, lParam, &fDefDlgEx);
    }

    LRESULT MyDlg_DlgProc(HWND hwndDlg, UINT msg, WPARAM wParam, LPARAM lParam)
    {
	switch (msg)
	{
	HANDLE_MSG(hwndDlg, WM_INITDIALOG, MyDlg_OnInitDialog);
	HANDLE_MSG(hwndDlg, WM_COMMAND, MyDlg_OnCommand);

	default:
	    return MyDlg_DefProc(hwnd, msg, wParam, lParam);
	}
    }

As mentioned earlier, it's safe to use the same boolean `fDefDlgEx` variable
for all your dialog procs, or you can define one for each of your dialogs.
What's important is that the same boolean variable be used for the
**CheckDefDlgRecursion** call **and** the **DefDlgProcEx()** call in a given dialog proc:
a local `BOOL` variable must **not** be used, or infinite recursion will result.

You can also use the same `_DefProc` function declaration for all of your
dialog procs that use the same `fDefDlgEx` variable: for example, you could
implement the following function:


    LRESULT CommonDlg_DefProc(HWND hwndDlg, UINT msg, WPARAM wParam, LPARAM lParam)
    {
	return DefDlgProcEx(hwnd, msg, wParam, lParam, &fDefDlgEx);
    }

then, for each dialog class, #define something like:

    #define MyDlg_DefProc  CommonDlg_DefProc



Converting Existing Code to use Message Crackers
------------------------------------------------

Converting existing code over to use message crackers is not particularly
difficult.  Here are some suggestions that should help:

* Have a look at the sample app MAKEAPP for some more detailed
      examples of the use of message crackers and message forwarders.

* It's a good idea (though not necessary) to first convert your code
      to use STRICT.  With STRICT enabled, the compiler can help you find
      parameter type mismatch and other errors much easier.

* It's best to declare all your message handler functions in an include
      file, rather than declaring them directly in the .c file that uses
      them.  You don't have to use the ClassName_OnXXX() naming convention
      for your function, but we've found that it's quite a helpful way to
      organize the code for a window class.

* When declaring or implementing a message forwarder function, just
      use your editor to search for and copy the message handler function
      prototype comment from windowsx.h and paste it into your source code.
      This way you don't have to type it from scratch.

* Use the **FORWARD_WM_\*** macros to send or forward non-control messages
      to other windows by passing SendMessage as the first parameter.

* If you have defined your own private window messages, you should define
      a message cracker and forwarder for each.  This is pretty simple to do:
      just copy an existing cracker and forwarder from windowsx.h and edit
      it. Be sure to fully parenthesize your use of macro parameters, and be
      careful with the casts you use.

Converting existing window and dialog procs to message crackers can be a fair
amount of work, especially if the window and dialog procs are large.  You can
mix and match old-style message handlers with message crackers, so you may
want to use message crackers for new message handlers, or for existing code
you plan on modifying extensively.

Converting dialog procs over to the new-style LRESULT-returning dialog procs
is also something that isn't required for all your dialog procs.  You can
convert those that you plan to modify, or that contain code that you may want
to reuse in other dialog procs.
