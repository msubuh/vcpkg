Using vcpkg with Visual Studio
==============================

Integration of `vcpkg` with Visual Studio allows developers to easily include and manage libraries in their projects. This guide from `KNOT35.com <https://www.knot35.com/>`_ outlines the process of setting up and using `vcpkg` with Visual Studio:

1. **Install vcpkg**:
   ------------------

   First, clone the `vcpkg` repository and initiate the bootstrap process:

   .. code-block:: bash

      git clone https://github.com/microsoft/vcpkg.git
      cd vcpkg
      ./bootstrap-vcpkg.sh  # On Linux/macOS
      .\bootstrap-vcpkg.bat  # On Windows

2. **Install Libraries**:
   ----------------------

   Using `vcpkg`, you can install the necessary libraries. For instance:

   .. code-block:: bash

      .\vcpkg install boost:x64-windows  # For Windows 64-bit

3. **Integrate vcpkg with Visual Studio**:
   ---------------------------------------

   Execute the integration command to allow Visual Studio to recognize libraries installed through `vcpkg`:

   .. code-block:: bash

      .\vcpkg integrate install

   After running the command, you may receive an output similar to:

   .. code::

      Applied user-wide integration for this vcpkg root.
      
      All MSBuild C++ projects can now #include any installed libraries.
      Linking will be handled automatically.
      Installing new libraries will make them instantly available.
      
      CMake projects should use: "-DCMAKE_TOOLCHAIN_FILE=[vcpkg-root]\scripts\buildsystems\vcpkg.cmake"

4. **Using with Visual Studio**:
   -----------------------------

   - **For MSBuild (traditional Visual Studio projects)**:
      Just include headers and utilize functions or classes from your installed library. With the integration command in effect, Visual Studio will auto-detect where to find headers and linked libraries.

   - **For CMake projects in Visual Studio**:
      For those employing the CMake integration in Visual Studio, adjust the CMake toolchain file to the `vcpkg` toolchain. This can be achieved by adding the subsequent argument to your CMake settings:

      .. code::

         -DCMAKE_TOOLCHAIN_FILE=[vcpkg-root]\scripts\buildsystems\vcpkg.cmake

      Replace `[vcpkg-root]` with your vcpkg installation's path.

5. **Building and Running your Project**:
   --------------------------------------

   With everything set up, proceed to build and execute your Visual Studio project as you normally would. `vcpkg`-installed libraries will be linked automatically.

Final Thoughts
--------------

Ensure you select the right triplet (e.g., `x64-windows`, `x86-windows`, etc.) while installing libraries, based on your target platform. By adhering to the steps above, `vcpkg` integration with Visual Studio becomes effortless, granting you access to a wide array of C and C++ libraries.
