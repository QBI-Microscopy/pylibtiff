In order to compile this module in Windows7 I performed the following steps:

1 - Copy this file:
	
C:\Program Files (x86)\Microsoft Visual Studio 9.0\VC\bin\vcvars64.bat

To this follow folder and rename the file (vcvars64.bat to vcvarsamd64.bat):
	
C:\Program Files (x86)\Microsoft Visual Studio 9.0\VC\bin\amd64\vcvarsamd64.bat

2 - Edit the file msvc9compiler.py inside of directory C:\Python27\Lib\distutils\msvc9compiler.py

After the line 651 approximately, find this line: – ld_args.append(‘/MANIFESTFILE:’ + temp_manifest)
Add the following line after the above line:
	
ld_args.append('/MANIFEST')

3 - Edit the file msvccompiler.py inside of directory C:\Python27\Lib\distutils\msvccompiler.py

At line 153 approximately, insert this line: return 9.0, as following, in this piece of code:
	
def get_build_version():
    """Return the version of MSVC that was used to build Python.
 
    For Python 2.3 and up, the version number is included in
    sys.version.  For earlier versions, assume the compiler is MSVC 6.
    """
    return 9.0
    prefix = "MSC v."
    i = string.find(sys.version, prefix)
    if i == -1:
        return 6

4 - Execute this command to compile, build and install the module:
	
python setup.py build --compiler msvc
python setup.py install

NOTE 1: 

I also had to copy the system libtiff into the site-packages/libtiff directory and add this to the system PATH. 

The latest svn version from https://code.google.com/p/pylibtiff/ tries to import the system libtiff as libtiff3.dll.

In this version I changed the library import as follows:

    if getattr(sys, 'frozen', False):
        lib = os.path.abspath(sys._MEIPASS) + r'\libtiff.dll'
    else:
        lib = os.path.abspath(os.path.dirname(__file__)) + r'\libtiff.dll'
		
To avoid having to use 'ctypes.util.find_library' which expects the libtiff dll to be in the PATH.

I am checking for 'frozen' because I am using libtiff in an executable app.

Note 2:
The libtiff3.dll downloadable from GnuWin32 is a 32 dll which not work with Win64.

Note 3:

I added methods to the 'libtiff_ctypes' module called 'write_tile' to write a single tile to an open tiff image and
'set_description' which calls SetField to add an IMAGEDESCRIPTION TIFFTAG to an open image.

=================================
PyLibTiff - a Python tiff library
=================================

:Authors:

  Pearu Peterson <pearu.peterson AT gmail DOT com>

:Website:

  http://pylibtiff.googlecode.com/

:License:

  New BSD License

History
=======

 * Started numpy.memmap wrapper of tiff files in April 2010.
 * Project published on April 22, 2009.

Download
========

The latest release can be downloaded from pylibtiff website.

The latest development code is available via SVN. To check it out,
run::

  svn checkout http://pylibtiff.googlecode.com/svn/trunk/ pylibtiff-svn
  cd pylibtiff-svn

Installation
============

To use pylibtiff, the following is required:

  * Python 2.5 or newer
  * libtiff library
  * nose for running pylibtiff tests

To install pylibtiff, unpack the archive file, change to the
pylibtiff source directory ``pylibtiff-?.?*`` (that contains setup.py
file and pylibtiff module), and run::

  python setup.py install

Testing
=======

To test pure Python pylibtiff from source directory, run::

  nosetests libtiff/tests/

Basic usage
===========

Import pylibtiff with

>>> from libtiff import *

that will provide a class TIFF to hold a tiff file:

>>> tiff = TIFF.open('filename')
>>> image = tiff.read_image()

The TIFF class provides ctypes based wrapper to the C libtiff library.

Additional documentation is available online in Pylibtiff website.

Help and bug reports
====================

You can report bugs at the pylibtiff issue tracker:

  http://code.google.com/p/pylibtiff/issues/list

Any comments and questions can be sent also to the authors.

