# CUnit-2.1-3
CUnit 2.1-3 framework used for EFREI project

			CUnit : A Unit Testing Framework
			  http://cunit.sourceforge.net

CUnit is a Unit testing framework for C.

The basic framework is platform/version independent and should be
portable to all platforms.  CUnit provides various interfaces to
the framework, some of which are platform dependent (e.g. curses on
*nix).  Building other interfaces should be straightforward with
the facilities provided in the framework.

CUnit is built as either a static or shared library which provides 
framework support when linked into user testing code.  The framework 
complies with the conventional structure of test cases bundled into 
suites which are registered with the framework for running.  See the
documentation for more about the structure and use of the framework.

Note - the windows-specific gui interface is not yet written.  It is
still necessary to use either the automated, basic, or console
interfaces to CUnit on Windows at this time.

-------------------------------------------------------
Important Note - Changes to CUnit Structure & Interface
-------------------------------------------------------

As of version 2.0, the interface functions used to interact with the
CUnit framework have changed.  The original interface did not attempt
to protect user code from name clashes with public CUnit functions and
variables.  To minimize such name clashes, all CUnit public functions
are now prefixed with 'CU_'.

The old public names are deprecated as of Version 2.0, but continue
to be supported with conversion macros.  In order to compile older code
using the original interface, it is now necessary to compile with the
macro -USE_DEPRECATED_CUNIT_NAMES defined.  If there are any problems
compiling older code, please file a bug report.

In addition, the DTD and XSL files for output from the automated test
interface have been updated to support both old and new file structure.
That is, a List or Run file generated using the version 1.1 library
should be (1) valid under the version 2 DTD's and (2) formatted
correctly by the version 2 XSL's.  Note, however, that this has not
been extended to the Memory-Dump DTD and XSL files.  That is, memory
dumps created using version 1.1 are ill-formed and incorrectly
formatted using the version 2 DTD and XSL files.

Another exception to backward compatibility occurs if the user has
directly manipulated the global variables in version 1.1.  The
original CUnit structure included global variables error_number and
g_pTestRegistry which have been removed from the global namespace as
of Version 2.0.  Any user code which directly accessed these variables
will break.  The variables must be retrieved using the accessor
functions CU__get_error() and CU_get_registry().

Similarly, user code retrieving the active test registry and directly
manipulating the uiNumberOfFailures or pResult members will break.
These members have been moved to the TestRun.c part of the framework
and are no longer available in the test registry as of version 2.0.

Another change in Version 2.0 is the update of the framework terminology.
What were termed 'test groups' in the original structure are now called
"suites", and "test cases" are now just "tests".  This change was made to
bring CUnit in conformance with standard testing terminology, and results
in a change in the name of some functions (e.g. run_group_tests() is
now CU_run_suite().

---------------------------------------
Building the CUnit Library and Examples
---------------------------------------

All Platforms:

  As of Version 2.0, a set of Jamfiles is provided for cross-platform
  building of the library, examples, and tests.  The jam build system was
  implemented using ftjam 2.3.2 (http://www.freetype.org/jam/index.html).
  It may work under the original Perforce jam implementation, but has not
  been tested.  It has been tested under gcc on Linux, and Borland C++ 5.5, 
  VC7, MinGW 3.2.3, and Open Watcom 1.3 on Windows.  Due to the nature of 
  jam, the build system should be readily extensible to other platforms.

  The jam file set supports both standard and custom symbolic build
  targets (install, uninstall, clean, libcunit, examples, test).  A
  customized Jambase file is provided which sorts out some of the
  rough edges for MinGW and Watcom on Windows.  It may not be necessary
  when using gcc, Borland, or VC, but it won't hurt either.
  
  The files generated during the build are placed in a subdirectory
  given by <CONFIG>/<PLATFORM>, where:
  
    <CONFIG> = Debug or Release
    <PLATFORM> = bcc, mingw, msvc, watcom, linux
    
  This allows easy switching between compilers without overlap of the output 
  files.  The <CONFIG> is determined by whether NODEBUG is defined in your
  Jamrules file.  <PLATFORM> is set automatically in Jamrules.

  To build using jam:

    1. Set the working directory to the top of the source tree
    
    2. Generate Jamrules
       a. On Linux, run autoconf & configure
       b. On Windows, copy Jamrules.in to Jamrules

    3. Edit the top section of Jamrules to match your preferences

    4. jam -f Jambase install

Linux:

  In addition to jam, the standard GNU build system is still supported.
  The usual sequence of steps should succeed in building and installing CUnit:
    1. aclocal  (if necessary)
    2. autoconf (if necessary)
    3. automake (if necessary)
    4. chmod u+x configure (if necessary)
    5. ./configure --prefix <Your choice of directory for installation>
    6. make
    7. make install

  What's installed:
    1. libcunit.a (Library file)
    2. CUnit Header files
    3. DTD and XSL files supporting xml output files in share directory
    4. Man Pages in relevant man directories under the installation path.
    5. HTML users guide in the doc subdirectory of the installation path.
    6. Example & test programs in the share subdirectory of the install path.

Windows:

  Jam is the preferred build system for Windows.  A set of old VC6 project
  files is included which have been partially updated but not tested.  If
  they don't work and you get them working, we'd be happy to include them
  in the CUnit distribution.  A set of Visual Studio 2003/VC7 solution and
  project files has also been provided in the VC7 subdirectory.  Similarly,
  a set of Visual Studio 2005/VC8 solution and project files is located in
  the VC8 subdirectory.  The latter should work with all versions of VC 2005,
  including the Express edition.
