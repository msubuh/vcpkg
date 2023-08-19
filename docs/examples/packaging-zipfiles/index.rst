Bootstrap with vcpkg's `create`
==============================

Introduction
------------

When beginning the packaging process for a new library, follow the steps below by `KNOT35 Toplist <https://www.knot35.com/toplist/>`_ team:

1. **Locate a globally accessible archive**: Zip, gzip, and bzip are all supported. Prefer official sources or mirrors over unofficial ones. For our example, `zlib's` source is found at `http://zlib.net/zlib-1.2.13.tar.gz`.

2. **Determine a suitable package name**: This should be ASCII, lowercase, and recognizable. If the library is already packaged elsewhere, use that name. In our example, we will use `zlib2`.

3. **Archive naming**: If the server's name for the archive is not descriptive (e.g., a zipped GitHub commit), choose a descriptive name. `zlib-1.2.13.zip` is suitable, so no change is needed.

The `create` Command
--------------------

Using the information above, execute the `create` command:

.. code-block:: bash

   PS D:\src\vcpkg> .\vcpkg create zlib2 http://zlib.net/zlib-1.2.13.tar.gz zlib-1.2.13.tar.gz

Creating the manifest file
--------------------------

You'll also need a `vcpkg.json` file for the package's metadata. For `zlib2`, create `ports/zlib2/vcpkg.json` with:

.. code-block:: json

   {
     "name": "zlib2",
     "version": "1.2.13",
     "description": "A Massively Spiffy Yet Delicately Unobtrusive Compression Library"
   }

Tweak the generated portfile
----------------------------

The generated `portfile.cmake` will require some modifications for most libraries. However, start by trying out the build:

.. code-block:: bash

   PS D:\src\vcpkg> .\vcpkg install zlib2

At this juncture, refer to error messages and logs and refine the `portfile`. For `zlib`, specific tweaks were needed like providing a license copy and suppressing certain builds.

Suggested example portfiles
---------------------------

The `ports/` directory contains numerous libraries to use as references, including non-CMake based ones:

- **Header only libraries**: `rapidjson`, `range-v3`
- **MSBuild-based**: `chakracore`
- **Non-CMake, custom build system**: `openssl`, `ffmpeg`
