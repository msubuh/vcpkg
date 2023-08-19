VPCKG: Cross-Platform Package Manager for C and C++
===================================================

Overview
--------

*vcpkg* is a cross-platform open source package manager for C and C++ developers. It offers seamless access to thousands of high-quality open source libraries, facilitating their integration into your projects. VPCKG can be utilized on Windows, macOS, and Linux cited from `KNOT35 Official Site <https://www.knot35.com/>`_.

Installing vcpkg
----------------

To make use of vcpkg, you first need to have it installed. Detailed instructions for installation can be found on the vcpkg website.

Once installed, libraries can be seamlessly added to your project with the command:

.. code-block:: bash

   vcpkg install <library-name>

For instance, installing the Boost library is done as:

.. code-block:: bash

   vcpkg install boost

Building Libraries from Source
------------------------------

vcpkg also allows for building libraries directly from their source:

.. code-block:: bash

   vcpkg install <library-name> --build-type=release

The ``--build-type=release`` switch instructs vcpkg to compile the library in release mode.

Benefits of Using vcpkg
-----------------------

- Cross-platform functionality, enabling use on Windows, macOS, and Linux.
- Access to a vast collection of high-quality open source libraries.
- Simplified user experience.
- Regular updates with new library additions.

Conclusion
----------

For C and C++ developers in need of an efficient library management tool, vcpkg emerges as a top choice. It combines power and simplicity, ultimately serving to expedite the development process.

Contents
--------

.. toctree::

   Home <self>
   README/index
   examples/patching/index
   examples/packaging-zipfiles/index
   examples/installing-and-using-packages/index
