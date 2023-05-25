<h1 align="center">Quantum Hazel</h1>

<h4 align="center">
  <a href="https://github.com/tjira/hazel#%EF%B8%8F-compilation">Compilation</a>
  ·
  <a href="https://github.com/tjira/hazel#-examples">Examples</a>
  ·
  <a href="https://tjira.github.io/hazel/">Docs</a>
</h4>

<p align="center">
    <a href="https://github.com/tjira/hazel/pulse">
        <img src="https://img.shields.io/github/last-commit/tjira/hazel?logo=github&logoColor=white&style=for-the-badge"/>
    </a>
    <a href="https://github.com/tjira/hazel/blob/master/LICENSE.md">
        <img src="https://img.shields.io/github/license/tjira/hazel?logo=gitbook&logoColor=white&style=for-the-badge"/>
    </a>
    <a href="https://github.com/tjira/hazel/stargazers">
        <img src="https://img.shields.io/github/stars/tjira/hazel?logo=apachespark&logoColor=white&style=for-the-badge"/>
    </a>
    <a href="https://github.com/tjira/hazel">
        <img src="https://img.shields.io/github/languages/code-size/tjira/hazel?logo=databricks&logoColor=white&style=for-the-badge"/>
    </a>
    <br>
    <a href="https://github.com/tjira/hazel/releases/latest">
        <img src="https://img.shields.io/github/v/release/tjira/hazel?display_name=tag&logo=sharp&logoColor=white&style=for-the-badge"/>
    </a>
    <a href="https://github.com/tjira/hazel/releases/latest">
        <img src="https://img.shields.io/github/downloads/tjira/hazel/total?logo=markdown&logoColor=white&style=for-the-badge"/>
    </a>
</p>

<p align="center">
Hazel is a collection of various electronic structure methods. It works similarly to the other available quantum programs. It is provided with an input file with system and calculation specification and prints the results to the terminal. Part of the software is also a molecular viewer.
</p>

## ✨ Features

Below are all the important features of Hazel divided into categories.

### Quantum Mechanical Methods

* Hartree-Fock Method (restricted & unrestricted)
* Møller–Plesset Perturbation Theory (MP2)

### Convergence Accelerators

* Direct Inversion in the Iterative Subspace

### Additional Calculations

* Numerical Gradient and Hessian
* Analytical Gradient for RHF
* Molecular Optimization
* Frequency Analysis
* Mulliken Analysis

## 🛠️ Compilation

The software requires the [libint](https://github.com/evaleev/libint) library. On Linux you can compile it yourself following the instructions on the webpage (or better yet execute `./script/libint.sh` from the root directory).

### Linux

Before compilation make sure that you have boost and eigen libraries installed. You can do that on debian-based distros using the following command.

```bash
sudo apt install libboost-all-dev libeigen3-dev
```

Make sure that you add the libint headers and library to the environmental variables `CPLUS_INCLUDE_PATH` and `LIBRARY_PATH`. You can then configure the project by the following command.

```bash
cmake -B build/release -DCMAKE_BUILD_TYPE=Release .
```

You will also need some X11 libraries if you plan to compile the molecular viewer. Most of the time you will already have them but if not, follow the CMake instructions. If the configuration finished without errors, compile the project by running the following command.

```bash
cmake --build build/release --parallel 4
```

After the compilation the bin folder will be created along with all the executables. You can test the correct working of the program by running tests.

```bash
cmake --build build/release --target test-build && cmake --build build/release --target test
```

All tests should pass, if not please report it.

### Windows

On a Windows machine compilation of the hazel binary is currently not possible. You can compile only the hview binary using the following command

```bash
cmake -B build/release -DCMAKE_BUILD_TYPE=Release -DCMAKE_C_COMPILER=gcc -DCMAKE_CXX_COMPILER=g++ -DCMAKE_MAKE_PROGRAM=mingw32-make -DCOMPILE_LIBINT=ON -G "Unix Makefiles" --target hview .
```

After that you can build it with the following.

```bash
cmake --build build/release --parallel 4
```

Keep in mind that I assume that you have [MinGW](https://www.mingw-w64.org) installed.

## 📦 Examples

To run a simple single point calculation, create a json file (for example *input.json*) with the following contents.

```json
{ "hazel" : [

{
    "title" : "molecule",
    "path" : "/path/to/your/molecule.xyz",
    "basis" : "STO-3G"
},

{
    "title" : "hf",
    "diis" : {}
}

]}
```

You can then run the calculation with the `./hazel input.json` command.

## ⭐ Credits

* [argparse](https://github.com/p-ranav/argparse) - Argument Parser for Modern C++.
* [boost](https://github.com/boostorg/boost) - Boost provides free peer-reviewed portable C++ source libraries.
* [eigen](https://gitlab.com/libeigen/eigen) - Eigen is a C++ template library for linear algebra.
* [glad](https://github.com/Dav1dde/glad) - Multi-Language Vulkan/GL/GLES/EGL/GLX/WGL Loader-Generator based on the official specs.
* [glfw](https://github.com/glfw/glfw) - A multi-platform library for OpenGL, OpenGL ES, Vulkan, window and input .
* [glm](https://github.com/g-truc/glm) - OpenGL Mathematics.
* [imgui](https://github.com/ocornut/imgui) - Bloat-free Graphical User Interface for C++ with minimal dependencies.
* [imguifiledialog](https://github.com/aiekick/ImGuiFileDialog) - File dialog for Dear ImGui.
* [implot](https://github.com/epezent/implot) - Immediate mode plotting for ImGui.
* [json](https://github.com/nlohmann/json) - JSON for modern C++.
* [libint](https://github.com/evaleev/libint) - High-performance library for computing Gaussian integrals in quantum mechanics.
* [stb](https://github.com/nothings/stb) - Single-file public domain libraries for C/C++.
