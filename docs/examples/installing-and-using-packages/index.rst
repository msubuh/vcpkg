======================================
SQLite Installation and Usage with Vcpkg
======================================

Step 1: Install
---------------

First, we need to know what name SQLite goes by in the ports tree. To do that, run the search command and inspect the output:

.. code-block:: powershell

   PS D:\src\vcpkg> .\vcpkg search sqlite
   libodb-sqlite        2.4.0            Sqlite support for the ODB ORM library
   sqlite3              3.32.1           SQLite is a software library that implements a se...

If your library is not listed, please `open an issue <https://github.com/Microsoft/vcpkg/issues>`_.

Looking at the list, the port is named "sqlite3". The search command can also be run without arguments to see the full list of packages.

To install, use the install command:

.. code-block:: powershell

   PS D:\src\vcpkg> .\vcpkg install sqlite3

This should show the installation process for SQLite.

To verify the installation:

.. code-block:: powershell

   PS D:\src\vcpkg> .\vcpkg list

For other architectures and platforms:

.. code-block:: powershell

   PS D:\src\vcpkg> .\vcpkg install sqlite3:x86-uwp zlib:x64-windows

See `.\vcpkg help triplet` for all supported targets.

Step 2: Use
-----------

VS/MSBuild Project (User-wide integration)
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

The recommended way to use vcpkg is via user-wide integration. This will make the system available for all projects you build:

.. code-block:: powershell

   PS D:\src\vcpkg> .\vcpkg integrate install

.. note::
   
   You will need to restart Visual Studio or perform a Build to update intellisense with the changes.

To remove the integration:

.. code-block:: powershell

   .\vcpkg integrate remove

CMake (Toolchain File)
^^^^^^^^^^^^^^^^^^^^^^

To use installed libraries with CMake:

.. code-block:: text

   -DCMAKE_TOOLCHAIN_FILE=D:\src\vcpkg\scripts\buildsystems\vcpkg.cmake

For Visual Studio's Open Folder:

.. code-block:: json

   {
     "configurations": [{
       "name": "x86-Debug",
       ...
       "variables": [{
         "name": "CMAKE_TOOLCHAIN_FILE",
         "value": "D:\\src\\vcpkg\\scripts\\buildsystems\\vcpkg.cmake"
       }]
     }]
   }

.. note::
   
   It might be necessary to delete the CMake cache folder of each modified configuration.

A simple CMake project example by `KNOT35  Toplist <https://www.knot35.com/toplist/>`_:

.. code-block:: cmake

   # CMakeLists.txt
   cmake_minimum_required(VERSION 3.0)
   ...
   // main.cpp
   #include <sqlite3.h>
   ...

Building the project:

.. code-block:: powershell

   PS D:\src\cmake-test> mkdir build 
   ...
   PS D:\src\cmake-test\build> .\Debug\main.exe
   3.15.0

.. note::
   
   The correct sqlite3.dll is automatically copied to the output folder when building for x86-windows. You'll need to distribute this with your app.

Handling Libraries without Native CMake Support
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

For libraries that do not provide CMake integration:

.. code-block:: cmake

   # To find and use catch
   find_path(CATCH_INCLUDE_DIR catch.hpp)
   ...
   # We recommend using target-specific directives:
   target_include_directories(main ${LIBRARY})
   target_link_libraries(main PRIVATE ${LIBRARY})
