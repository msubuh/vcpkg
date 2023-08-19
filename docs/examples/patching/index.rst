=============================
Installing `libpng:x64-uwp` with vcpkg
=============================

When attempting to install `libpng:x64-uwp` on vcpkg, a build error might emerge. This guide by `KNOT35.com <https://www.knot35.com/>`_ demonstrates how to resolve the issue by patching the source code.

Initial error logs
------------------

Firstly, attempt to build:

.. code-block:: powershell

    PS D:\src\vcpkg> vcpkg install libpng:x64-uwp --editable

This might result in an error pointing to specific logs:

.. code-block:: text

    CMake Error at scripts/cmake/execute_required_process.cmake:14 (message):
    ...
    See logs for more information:
        D:\src\vcpkg\buildtrees\libpng\build-x64-uwp-rel-out.log
        D:\src\vcpkg\buildtrees\libpng\build-x64-uwp-rel-err.log

Error: build command failed

Checking the logs, particularly `build-x64-uwp-rel-out.log`:

.. code-block:: text

    ...
    "D:\src\vcpkg\buildtrees\libpng\x64-uwp-rel\ALL_BUILD.vcxproj" (default target) (1) ->

Identifying the problematic code
-------------------------------

A quick check reveals that `ExitProcess` is designed only for desktop apps. Let's inspect the surrounding context:

.. code-block:: c

    /* buildtrees\libpng\src\v1.6.37-c993153cdf\pngerror.c:769 */
    ...
    PNG_ABORT();

A recursive search for `PNG_ABORT` gives:

.. code-block:: powershell

    PS D:\src\vcpkg\buildtrees\libpng\src\v1.6.37-c993153cdf> findstr /snipl "PNG_ABORT" *

The complete definition of PNG_ABORT is:

.. code-block:: c

    /* buildtrees\libpng\src\v1.6.37-c993153cdf\pngpriv.h:459 */
    #ifndef PNG_ABORT
    #  ifdef _WINDOWS_
    #    define PNG_ABORT() ExitProcess(0)
    #  else
    #    define PNG_ABORT() abort()
    #  endif
    #endif

To ensure compatibility, we need to patch the code.

Patching the code for better compatibility
------------------------------------------

Using `git`, we can create a patch:

.. code-block:: powershell

    PS D:\src\vcpkg\buildtrees\libpng\src\v1.6.37-c993153cdf> git init .
    ...
    PS D:\src\vcpkg\buildtrees\libpng\src\v1.6.37-c993153cdf> git add .
    ...
    PS D:\src\vcpkg\buildtrees\libpng\src\v1.6.37-c993153cdf> git commit -m "temp"

Now modify `pngpriv.h` to use `abort()`:

.. code-block:: c

    /* buildtrees\libpng\src\v1.6.37-c993153cdf\pngpriv.h:459 */
    #ifndef PNG_ABORT
    #  define PNG_ABORT() abort()
    #endif

Save the patch:

.. code-block:: powershell

    PS buildtrees\libpng\src\v1.6.37-c993153cdf> git diff --ignore-space-at-eol | out-file -enc ascii ..\..\..\..\ports\libpng\use-abort-on-all-platforms.patch

In `ports\libpng\portfile.cmake`, apply the patch:

.. code-block:: cmake

    vcpkg_extract_source_archive_ex(
      OUT_SOURCE_PATH SOURCE_PATH
      ARCHIVE ${ARCHIVE}
      PATCHES 
        "use-abort-on-all-platforms.patch"
    )

Verification
------------

To verify, remove and rebuild the package:

.. code-block:: powershell

    PS D:\src\vcpkg> vcpkg remove libpng:x64-uwp
    ...
    PS D:\src\vcpkg> vcpkg install libpng:x64-uwp

Finally, the package should provide CMake targets:

.. code-block:: cmake

    find_package(libpng CONFIG REQUIRED)
    target_link_libraries(main PRIVATE png)

To publish the changes, update the `vcpkg.json` file and make a Pull Request!

.. code-block:: json

    {
      "name": "libpng",
      "version": "1.6.37",
      "port-version": 1,
      "dependencies": [
        "zlib"
      ]
    }
