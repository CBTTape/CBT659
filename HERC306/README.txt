------------------------------------------------------------------------

                         FishLib DLL

          Fish's Handy Dandy Support Functions Library

                        Version 2.7.0

------------------------------------------------------------------------

                     OVERVIEW / DESCRIPTION


Many of the programs I write end up doing the same things all over again
as are already being done in yet others programs I've already written.

Rather than re-invent the wheel each time (and/or copy & paste the same
code into each new program I write), I've instead gathered most of the
more common functions into one place: this library (dll).

That way if I need to fix a bug in a particular function and/or add
support for new features/functionality to an already existing function
or class, I only need to do it in one place and not all over again in
each program individually.

I could have accomplished the same thing by making this set of common
functions a static library rather than a DLL, but then if I had to make
a change to the library I'd then have to relink all of my programs with
the new library and then ship that new version to all my customers.

With the library being a DLL however, all I have to do is build a new
version of the DLL instead and ship that to everyone, WITHOUT having to
re-link and re-ship each given product individually.

This makes for easier maintenance on my part as well as for my customers
since all they have to do is download/install the new version of the DLL
and instantly *all* of their  COMPANY  products are magically updated
with the fix and/or new functionality.


------------------------------------------------------------------------

                          INSTALLATION


To install FishLib, simply unzip the distribution file and either:


  a) copy the DLLs in the 'bin' directory to someplace where Windows
     can find them (e.g. your %SystemRoot% directory for example),

                             -OR-

  b) add FishLib's 'bin' directory to your Windows search "PATH"
     environment variable (recommended).


The supplied 'include', 'lib' and 'pdb' directories are needed only if
you plan to use FishLib in your program development. They are NOT needed
if you only plan to *use* FishLib with one of my products that needs it.
Only the DLLs are actually needed to *use* FishLib.

The 'include', 'lib' and 'pdb' directories contain header files, linker
import libraries and debugging symbols respectively.


Q: "Why are there extra copes, each ending with 'D', 'U' and 'UD'? Do I
    need all of them in order to use FishLib?"

A: No, you don't need all of them if you only plan to *use* FishLib and
   not do any program development. You only need the plain 'FishLib.dll'
   (or possibly the 'FishLibU.dll').

   The two 'D' varieties (FishLibD and FishLibUD) are debugging versions
   used only for program development. The plain version (FishLib) is the
   normal Release mode version and is the only one you normally need.

   The FishLibU version is a UNICODE version of the same thing and is
   needed only if you use any of the UNICODE versions of my products.

   At the moment the UNICODE version is identical to the normal Release
   mode version (except that it's a UNICODE build obviously) but in the
   future it *might* be different in that it may be customized to your
   local language. For now though I only support English however, but
   that MAY change in the future. (I can't guarantee that of course)


Q: "Whenever I try running one of you programs that needs FishLib, I keep
    getting a Window error dialog saying "The application configuration
    is incorrect. Reinstalling this application may fix this problem."
    What is going on? How do I fix this?"

A: Chances are good you're simply missing the new VC++ 8.0 runtime DLLs,
   MSVCR80.dll and MFC80.dll. You should obtain the DLLs directly from
   Microsoft via the following URLs (depending on whether you're running
   the 32-bit "Windows XP" version of Windows or the 64-bit "Windows XP
   X64 Edition" of Windows:

   Visual C++ 2005 x86 Redistributables:  http://preview.tinyurl.com/odssp
   Visual C++ 2005 x64 Redistributables:  http://preview.tinyurl.com/kqwta

   (Note: the above tinyurl URLs should redirect to Microsoft)


------------------------------------------------------------------------

                      FISHLIB DEVELOPMENT


The directory containing both the FishLib link libaries as well as the
FishLib header files should be entered into Visual C++'s global settings
(Tools | Options | Directories) such that in order to use FishLib all you
should then need to do is #include "fishlib.h" somewhere in your project
and then call/use it's classes or functions as normal.

The "fishlib.h" header also uses #pragma comment(lib) statements to
cause your project to auto-link with the proper import libraries too,
so using FishLib is easy: just #include "fishlib.h" somewhere in your
project and then call/use any of it's classes / functions as needed.


                      KNOW ISSUES / CAVEATS


When you link a 'Release' (i.e. non-DEBUG) build of a program using FishLib,
you MAY encounter a link warning:

LINK: warning LNK4089: all references to "SHELL32.dll" discarded by /OPT:REF

which is benign. It's caused by the fact that FishLib's "BrowseForDirectory"
(and related) function(s) call certain shell functions (e.g. SHGetMalloc,
SHBrowseForFolder, etc), and if your program doesn't call or otherwise need
FishLib's BrowseForDirectory function, then the linker correctly determines
that your program doesn't really need SHELL32.dll at all (since the functions
within FishLib using it are never referenced). That's all the warning means
and thus it's nothing to worry about.

#define USE_FISHLIB_AFXTRACE is the default (#define NO_USE_FISHLIB_AFXTRACE
to disable) and will work around a MFC TRACE O/P bug and MFC TRACE length
limitation bug. (Sometimes trace o/p would get dropped if the trace data
(OutputDebugString) "came in too fast", and MFC's default AfxTrace function
only supports a maximum o/p length of 512 characters (which is dumb IMHO).)

It works fine when capturing TRACE o/p yourself oneself via, for example, 
SysInternals' DebugView tool for example (www.sysinternals.com), so wherever
the problem is, it's obviously NOT in the OutputDebugString API, but rather
in the Developer Studio's IDE itself (since the problem only manifests it-
self when TRACEing with the Developer Studio IDE).

On top of that, MFC's AfxTrace function has a stupid built-in limit on how
much data one can TRACE in one call (i.e. there's a limit to how long the
resulting trace string can be) due to the stupid way the function is coded.

The 'FishLibAfxTrace' function works around both of those problems and thus
is the default. To not use FishLib's TRACE function, just specify the
NO_USE_FISHLIB_AFXTRACE preprocessor variable before #including "fishlib.h".


------------------------------------------------------------------------

                       VERSION 2.7.0


                 >>>  BREAKING CHANGES  <<<


With this new release of FishLib you *will* unfortunately have to make
some programming changes in order to get your programs which use it to
compile cleanly. Many of the fields in several classes were renamed and
re-typed to completely different types (e.g. int --> SSIZE_T). I hated
doing it but one does what one must do in order to accomplish what one
must accomplish. <shrug> Hopefully this won't have to happen again for
at least until when 128-bit systems become commonplace. :)


                   --  CHANGE SUMMARY  --


** ALL **:      VS2005 & x64 support requiring an increase
                in the size of some fields (BREAKING CHANGES)

CSMap:          Created/Added to FishLib (Sorted CMap template class)
StdioFileEx:    Created/Added to FishLib (auto-UNICODE file handling)
CScrollBar64:   Created/Added to FishLib (64-bit scrollbar handling)
BCMenu:         Created/Added to FishLib (Brent Corkum fancy menus)

FishLib:        New 32/64-bit names since VS8 incompatible w/VC++ 6.0
                #include "SpecialBuild.rc2" (common "SpecialBuild")
                Fix bug in "QuoteIfNeeded": escape ending back-slash.
                Fix MakePrt so it doesn't ASSERT when calling _istprint.
                FishLibAfxTrace: fuck MFC; just use OutputDebugString
                New helper function "StdMSWinCommandLineParse" added.
                New global variable "g_bWow64" added.

CHexEdit:       Added "HEO_FIXEDSIZE" option; no delete/insert allowed.
                Added "HEO_DATACALLBACK" option: external data source
                  callbacks to allow editing > 32-bit amounts of data
                Added IsEmpty() function.
                Added GetScreenLines() function.
                Fix bug in ExtendSel causing bad m_nDataPos to get set,
                Forgot to update 'm_nDataPos' in "SetSel()" (oops!)
                Use of BCMenu in right-click Copy/Paste context menus.

EditAction:     Deprecated ACTION_PASTE since it's not being used,

HyperLink:      Make SW_SHOW the default for GotoURL and make public
                Change GotoURL to use CreateProcess instead of WinExec
                Fix for hand cursor in SetDefaultCursor

WinPlace:       Added subkey support.

zDate:          New U.S. Daylight Saving Time (DST) rules starting 2007
                Fix bug in post-increment/decrement operator functions.
                Fix bug in IsDST() function causing dates >= EndDST


------------------------------------------------------------------------
