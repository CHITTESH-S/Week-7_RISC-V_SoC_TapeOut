# 🌟 RISC-V SoC Tapeout – Week-7: BabySoC Physical Design, Post-Route SPEF Generation and A Complete RTL-to-GDSII Physical Design with OpenROAD

💎 This week represents the culmination of your RISC-V SoC journey - where all theory meets practice in a complete, automated physical design flow. Let's transform VSDBabySoC from RTL to silicon! 

---

## 🌟 What Makes This Week Special

📦 **Integration Week** - Bringing together all previous stages into a unified flow

🤖 **Full Automation** - Leveraging ORFS to streamline the entire physical design process

🔬 **Real-World Experience** - Understanding how production SoC designs move from logic to layout

💡 **End-to-End Flow** - From Verilog source code to manufacturable GDSII layout

---

## 🎯 Learning Objectives

### Primary Goals

✅ **Execute Complete RTL-to-GDSII Flow** - Run the full physical design sequence on BabySoC

📊 **Analyze Physical Design Metrics** - Understand placement density and routing topology impact on timing

⚡ **Master Parasitic Extraction** - Learn SPEF generation for accurate timing verification

🔧 **Environment Setup** - Install and configure OpenROAD-Flow-Scripts infrastructure

🏗️ **Design Integration** - Incorporate VSDBabySoC RTL and analog macros into the flow

📝 **Configuration Management** - Create comprehensive config.mk for ORFS automation

✓ **Output Verification** - Validate results through GUI inspection, logs, and reports

### Skills You'll Develop

🧩 **Macro Integration** - Understanding how macros, power rings, and standard cells coexist physically

🚦 **Congestion Management** - Insight into routing congestion and Design Rule Check (DRC) handling

🔗 **Timing Closure** - Practical knowledge of post-layout parasitic extraction bridging geometry to timing

---

## 🛠️ OpenROAD-Flow-Scripts Automation Pipeline

### Stage-by-Stage Automation

🔹 **Logic Synthesis** - Yosys transforms RTL to gate-level netlist

🔹 **Floorplanning** - TritonTools establishes die area, macro placement, and power grid

🔹 **Placement** - Global and detailed placement optimize cell locations

🔹 **Clock Tree Synthesis (CTS)** - Builds balanced clock distribution network

🔹 **Routing** - Global and detailed routing connect all nets

🔹 **DRC/LVS Checks** - Design Rule Check and Layout vs Schematic verification

🔹 **GDSII Generation** - Final silicon-ready layout database creation

---

## 💡 Why This Task Is Critical

### Understanding the Complete Flow

🔄 **Unified Experience** - Synthesis, floorplanning, and CTS explored separately now work together

🎨 **Physical Layout Insights** - See how logical descriptions transform into geometric shapes

⚙️ **Tool Interaction** - Learn how different EDA tools communicate through standardized formats

📈 **Performance Analysis** - Observe how physical design choices affect electrical characteristics

---

## 🏗️ OpenROAD Flow Setup

### 📥 Step 1: Clone the Repository

```bash
git clone --recursive https://github.com/The-OpenROAD-Project/OpenROAD-flow-scripts
cd OpenROAD-flow-scripts/
```
<div align="center">

<img width="1024" height="1024" alt="cloning" src="https://github.com/user-attachments/assets/f33c01a7-7405-41e1-a985-c181c75f03ad" />

</div>

> 📌 **Note:** The `--recursive` flag ensures all submodules are initialized properly.

<div align="center">

<img width="1024" height="1024" alt="listing" src="https://github.com/user-attachments/assets/8dc70097-a763-4303-ab40-a38e2afba3df" />

</div>

---

### 🔧 Step 2: Install Dependencies

```bash
sudo ./setup.sh
```
<div align="center">

<img width="1024" height="1024" alt="setup1" src="https://github.com/user-attachments/assets/d82ac23d-541d-4546-a54c-69d1ff72cb4e" />

</div>

<div align="center">

<img width="1024" height="1024" alt="setup2" src="https://github.com/user-attachments/assets/3f2c2d9e-9d91-40b1-8c1e-b3d60adc35ee" />

</div>

<div align="center">

<img width="1024" height="1024" alt="setup3" src="https://github.com/user-attachments/assets/03c55815-090d-492a-9241-3cd58988e236" />

</div>

<div align="center">

<img width="1024" height="1024" alt="setup4" src="https://github.com/user-attachments/assets/48570f17-78d8-47b4-b14b-fd5628b9c27f" />

</div>

This installs all necessary dependencies and prepares the environment for compilation, including:

✅ `build-essential`, `cmake`, `tcl`  
✅ `libx11-dev`, `libxrender1`, `libxext6`  
✅ `yosys`, `magic`, `netgen`, and other EDA tools  

> ⚠️ **Important:** Verify gcc, g++, and make versions for build compatibility.

---

### 🏗️ Step 3: Build OpenROAD

```bash
./build_openroad.sh --local
```
<div align="center">

<img width="1024" height="1024" alt="build1" src="https://github.com/user-attachments/assets/44c64db2-32d2-4a31-b856-e1404044ab4c" />

</div>

This command compiles OpenROAD from source and installs the required flow binaries locally.

#### 📊 Expected Output:
The build process will compile various modules and may take 15-30 minutes depending on your system.

---

## 🐛 Troubleshooting Build Issues

### ⚠️ Common Error: Build Halts at ~67%

During the build process, the compilation may stop around **67%** due to conflicting CMake test targets or GPU definitions.

#### 🔧 Solution: Modify CMakeLists.txt

**1️⃣ Navigate to the OpenROAD source directory:**

```bash
cd ~/OpenROAD-flow-scripts/tools/OpenROAD
```

**2️⃣ Open and edit CMakeLists.txt:**

```bash
nano CMakeLists.txt
```

**3️⃣ Replace the file contents with this patched version:**

```cmake
# SPDX-License-Identifier: BSD-3-Clause
# Copyright (c) 2019-2025, The OpenROAD Authors

cmake_minimum_required (VERSION 3.16)

# Use standard target names
cmake_policy(SET CMP0078 NEW)
cmake_policy(SET CMP0086 NEW)
cmake_policy(SET CMP0074 NEW)
cmake_policy(SET CMP0071 NEW)
cmake_policy(SET CMP0077 NEW)

# Interfers with Qt so off by default.
option(LINK_TIME_OPTIMIZATION "Flag to control link time optimization: off by default" OFF)
if (LINK_TIME_OPTIMIZATION)
  set(CMAKE_INTERPROCEDURAL_OPTIMIZATION TRUE)
endif()

# Allow to use external shared boost libraries
option(USE_SYSTEM_BOOST "Use system shared Boost libraries" OFF)

# Allow to use external shared opensta libraries
option(USE_SYSTEM_OPENSTA "Use system shared OpenSTA library" OFF)

# Allow to use external shared abc libraries
option(USE_SYSTEM_ABC "Use system shared ABC library" OFF)

# Disable tests completely
set(ENABLE_TESTS OFF)

# Sanitizer options (disabled by default)
option(ASAN "Enable Address Sanitizer" OFF)
option(TSAN "Enable Thread Sanitizer" OFF)
option(UBSAN "Enable Undefined Behavior Sanitizer" OFF)

project(OpenROAD VERSION 1 LANGUAGES CXX)

set(OPENROAD_HOME ${PROJECT_SOURCE_DIR})
set(OPENROAD_SHARE ${CMAKE_INSTALL_PREFIX}/share/openroad)

set(CMAKE_CXX_STANDARD 17 CACHE STRING "the C++ standard to use for this project")
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_CXX_EXTENSIONS OFF)

list(APPEND CMAKE_MODULE_PATH "${CMAKE_CURRENT_SOURCE_DIR}/cmake")

# Get version string in OPENROAD_VERSION
if(NOT OPENROAD_VERSION)
  include(GetGitRevisionDescription)
  git_describe(OPENROAD_VERSION)
  string(FIND ${OPENROAD_VERSION} "NOTFOUND" GIT_DESCRIBE_NOTFOUND)
  if(${GIT_DESCRIBE_NOTFOUND} GREATER -1)
    message(WARNING "OpenROAD git describe failed, using sha1 instead")
    get_git_head_revision(GIT_REFSPEC OPENROAD_VERSION)
  endif()
endif()

message(STATUS "OpenROAD version: ${OPENROAD_VERSION}")

# Default to building optimized/release executable
if(NOT CMAKE_BUILD_TYPE)
  set(CMAKE_BUILD_TYPE RELEASE)
endif()

if(CMAKE_CXX_COMPILER_ID STREQUAL "GNU")
  if(CMAKE_CXX_COMPILER_VERSION VERSION_LESS "8.3.0")
    message(FATAL_ERROR "Insufficient gcc version. Found ${CMAKE_CXX_COMPILER_VERSION}, but require >= 8.3.0.")
  endif()
elseif(CMAKE_CXX_COMPILER_ID STREQUAL "Clang")
  if(CMAKE_CXX_COMPILER_VERSION VERSION_LESS "7.0.0")
    message(FATAL_ERROR "Insufficient Clang version. Found ${CMAKE_CXX_COMPILER_VERSION}, but require >= 7.0.0.")
  endif()
elseif(CMAKE_CXX_COMPILER_ID STREQUAL "AppleClang")
  if(CMAKE_CXX_COMPILER_VERSION VERSION_LESS "12.0.0")
    message(FATAL_ERROR "Insufficient AppleClang version. Found ${CMAKE_CXX_COMPILER_VERSION}, but require >= 12.0.0.")
  endif()
else()
  message(WARNING "Compiler ${CMAKE_CXX_COMPILER_ID} is not officially supported.")
endif()

message(STATUS "System name: ${CMAKE_SYSTEM_NAME}")
message(STATUS "Compiler: ${CMAKE_CXX_COMPILER_ID} ${CMAKE_CXX_COMPILER_VERSION}")
message(STATUS "Build type: ${CMAKE_BUILD_TYPE}")
message(STATUS "Install prefix: ${CMAKE_INSTALL_PREFIX}")
message(STATUS "C++ Standard: ${CMAKE_CXX_STANDARD}")
message(STATUS "C++ Standard Required: ${CMAKE_CXX_STANDARD_REQUIRED}")
message(STATUS "C++ Extensions: ${CMAKE_CXX_EXTENSIONS}")

# Configure version header
configure_file(
  ${OPENROAD_HOME}/include/ord/Version.hh.cmake
  ${OPENROAD_HOME}/include/ord/Version.hh
)

################################################################
if (CMAKE_CXX_COMPILER_ID STREQUAL "GNU" AND CMAKE_CXX_COMPILER_VERSION VERSION_LESS "9.1")
  MESSAGE(STATUS "Older version of GCC detected. Linking against stdc++fs")
  link_libraries(stdc++fs)
endif()

set(CMAKE_EXPORT_COMPILE_COMMANDS 1)

add_subdirectory(third-party)

# Disable tests entirely
# (removed add_custom_target(build_and_test) and GoogleTest include)

add_subdirectory(src)

# add_subdirectory(test)

target_compile_definitions(openroad PRIVATE GPU)

if(BUILD_PYTHON)
  target_compile_definitions(openroad PRIVATE BUILD_PYTHON=1)
else()
  target_compile_definitions(openroad PRIVATE BUILD_PYTHON=0)
endif()

if(BUILD_GUI)
  target_compile_definitions(openroad PRIVATE BUILD_GUI=1)
else()
  target_compile_definitions(openroad PRIVATE BUILD_GUI=0)
endif()

####################################################################
# Build man pages (Optional)
option(BUILD_MAN "Enable building man pages" OFF)
if(BUILD_MAN)
  message(STATUS "man is enabled")
  include(ProcessorCount)
  ProcessorCount(PROCESSOR_COUNT)
  message(STATUS "Number of processor cores: ${PROCESSOR_COUNT}")
  add_custom_target(
    man_page ALL
    COMMAND make clean && make preprocess && make all -j${PROCESSOR_COUNT}
    WORKING_DIRECTORY ${OPENROAD_HOME}/docs
  )
  install(DIRECTORY ${OPENROAD_HOME}/docs/cat DESTINATION ${OPENROAD_SHARE}/man)
  install(DIRECTORY ${OPENROAD_HOME}/docs/html DESTINATION ${OPENROAD_SHARE}/man)
endif()

####################################################################

set(CMAKE_CXX_FLAGS_RELEASEWITHASSERTS "${CMAKE_CXX_FLAGS_RELEASE} -UNDEBUG" CACHE STRING "" FORCE)
set(CMAKE_C_FLAGS_RELEASEWITHASSERTS "${CMAKE_C_FLAGS_RELEASE} -UNDEBUG" CACHE STRING "" FORCE)
```

**4️⃣ Save and exit:**
- Press `Ctrl + O`, then `Enter` to save
- Press `Ctrl + X` to exit

**5️⃣ Return to main directory and rebuild:**

```bash
cd ~/OpenROAD-flow-scripts
./build_openroad.sh --local
```
<div align="center">

<img width="1024" height="1024" alt="build2" src="https://github.com/user-attachments/assets/3ae99b68-c161-43c7-9f98-85d48b0f04f7" />

</div>

### ✅ What This Fix Achieves:

- ✔️ Proper handling of GCC/Clang versions
- ✔️ Disables test builds that cause conflicts
- ✔️ Avoidance of Qt and test-related build issues
- ✔️ Ensures GPU flags are properly handled
- ✔️ Links `stdc++fs` for older GCC versions (< 9.1)
- ✔️ Successful local build of OpenROAD binaries

### 🐛 Additional Common Errors:

**❌ Missing `spdlog` dependency:**
```bash
sudo apt-get install libspdlog-dev
```

**❌ Missing `gtest` (Google Test):**
```bash
sudo apt-get install libgtest-dev
```

**❌ Missing build.log file:**
- Ensure you're in the correct directory
- Check write permissions

---

## ✅ Verification Steps

### 🔍 Step 4: Verify Installation

```bash
source ./env.sh
yosys -help
openroad -help
```
<div align="center">

<img width="1024" height="1024" alt="env_help" src="https://github.com/user-attachments/assets/c9802d4e-da6e-4553-8f4d-259a74fcd6a3" />

</div>

<div align="center">

<img width="1024" height="1024" alt="openroad" src="https://github.com/user-attachments/assets/59c9b13f-8fa2-476a-96da-6c053d0a5925" />

</div>

**Expected Output:**  
Both `yosys` and `openroad` should respond successfully with their help documentation — this confirms a valid installation.

You can also check the version:

```bash
./build/src/openroad --version
```

---

## 🚀 Running the Flow

### 📐 Step 5: Execute Floorplan and Placement

```bash
cd flow/
make
```
<div align="center">

<img width="1024" height="1024" alt="make1" src="https://github.com/user-attachments/assets/bdaa7b70-e659-4cca-9726-14917fce8bc7" />

</div>

<div align="center">

<img width="1024" height="1024" alt="make2" src="https://github.com/user-attachments/assets/8a5e89af-f815-48de-85b4-098388ce17dc" />

</div>

This runs the flow using built-in example designs (such as `gcd` with the Nangate45 PDK).

**📊 What This Does:**
- Executes synthesis using Yosys
- Performs floorplanning
- Runs placement optimization
- Generates timing reports

---

### 🖼️ Step 6: Visualize the Layout

```bash
make gui_final
```
<div align="center">

<img width="1024" height="1024" alt="gui_final" src="https://github.com/user-attachments/assets/172100b3-464a-4871-be90-5ce3fd2b0237" />

</div>

<div align="center">

<img width="1024" height="1024" alt="gui_setup" src="https://github.com/user-attachments/assets/d722272d-edf6-4817-a664-59c111453d29" />

</div>

<div align="center">

<img width="1024" height="1024" alt="gui_hold" src="https://github.com/user-attachments/assets/4a5ee16d-6711-4056-9757-267c2583a220" />

</div>

<div align="center">

<img width="1024" height="1024" alt="gui_chart" src="https://github.com/user-attachments/assets/32b06059-7a1c-4695-a7ff-bac167e993e8" />

</div>

This opens the **OpenROAD GUI** showing the final placement and floorplan visualization.

**✅ You should now see:**
- The core area and standard cell placement
- Die boundaries and core regions
- Timing and slack charts within the OpenROAD GUI
- Visual representation of how cells are arranged

---

## 📂 Directory Structure

### 🗂️ Main Repository Structure

```
OpenROAD-flow-scripts/
├── 📁 bazel/                    → Bazel build configuration files
├── 🔧 build_openroad.sh         → Script to locally build the OpenROAD toolchain
├── 📄 build_openroad.log        → Build log file for OpenROAD compilation
├── 📁 dependencies/             → Installed libraries and dependencies
│   ├── 📁 bin/                  → Dependency executables
│   ├── 📁 include/              → Header files for dependencies
│   ├── 📁 lib/                  → Shared/static libraries
│   ├── 📁 share/                → Shared dependency resources
│   └── 📄 README.md             → Notes about dependency setup
├── 🔧 dev_env.sh                → Developer environment setup script
├── 📁 docker/                   → Docker build definitions
│   ├── 🐳 Dockerfile.builder    → Builder image configuration
│   └── 🐳 Dockerfile.dev        → Development image configuration
├── 📁 docs/                     → Documentation, Sphinx configs, and tutorials
│   ├── 📁 images/               → Reference images for documentation
│   ├── 📁 tutorials/            → User and contributor tutorials
│   ├── ⚙️ conf.py               → Sphinx documentation configuration
│   └── 📄 README.md             → Documentation overview
├── 📁 etc/                      → Helper shell scripts for dependencies and Docker
│   ├── 🔧 DependencyInstaller.sh
│   ├── 🔧 DockerHelper.sh
│   └── 🔧 DockerTag.sh
├── 📁 flow/                     → ⭐ Core RTL-to-GDSII flow environment
│   ├── 📁 designs/              → Example RTL designs (e.g., gcd)
│   ├── 📁 platforms/            → Technology libraries and PDK files (Nangate45, Sky130)
│   ├── 📁 scripts/              → Flow automation Tcl scripts
│   ├── 📁 reports/              → Generated timing/area reports
│   ├── 📁 results/              → Flow outputs (ODB, DEF, GDS, logs, etc.)
│   ├── 📁 logs/                 → Stepwise tool logs (synthesis, placement, etc.)
│   ├── ⚙️ Makefile              → Defines and controls the end-to-end flow
│   └── 📁 tutorials/            → Example runs for new users
├── 📁 jenkins/                  → Regression and CI test configurations
├── 📁 tools/                    → ⭐ Installed EDA tools and utilities
│   ├── 📁 OpenROAD/             → Compiled OpenROAD binaries
│   │   ├── ⚙️ CMakeLists.txt    → ✅ Edit this file to fix build issues
│   │   ├── 📁 src/              → OpenROAD source code (C++ modules)
│   │   ├── 📁 third-party/      → Third-party libraries (OpenSTA, Boost)
│   │   ├── 📁 include/          → Header files
│   │   ├── 📁 cmake/            → CMake helper scripts
│   │   ├── 📁 docs/             → Documentation
│   │   ├── 📁 test/             → (Disabled) Test modules
│   │   └── ⚙️ WORKSPACE         → Bazel workspace file
│   ├── 📁 yosys/                → Yosys logic synthesis tool
│   ├── 📁 yosys-slang/          → Yosys slang front-end
│   ├── 📁 yosys_util/           → Utility scripts for Yosys
│   ├── 📁 AutoTuner/            → Optimization modules
│   └── 📁 codespace/            → Developer support scripts
├── 🔧 env.sh                    → Environment setup script (source before running flow)
├── 🔧 setup.sh                  → System dependency installation script
├── 📄 LICENSE_BUILD_RUN_SCRIPTS → License file for the build/run scripts
├── 📄 README.md                 → Main repository overview
└── ⚙️ WORKSPACE.bazel           → Bazel workspace descriptor
```

### 🗂️ Flow Directory Details

```
flow/
├── 📁 designs/                  → User design setup
│   ├── 📁 nangate45/            → Nangate45 PDK designs
│   ├── 📁 sky130hd/             → Sky130 PDK designs
│   └── 📁 asap7/                → ASAP7 PDK designs
├── 📁 platforms/                → Technology libraries
│   ├── 📄 *.lef                 → Library Exchange Format files
│   ├── 📄 *.lib                 → Liberty timing files
│   └── 📄 *.gds                 → GDSII layout files
├── 📁 scripts/                  → Stage-specific Tcl scripts
│   ├── 📄 synth.tcl             → Synthesis script
│   ├── 📄 floorplan.tcl         → Floorplan script
│   ├── 📄 place.tcl             → Placement script
│   └── 📄 route.tcl             → Routing script
├── 📁 results/                  → Final outputs
│   ├── 📄 *.def                 → Design Exchange Format
│   ├── 📄 *.odb                 → OpenROAD database
│   └── 📄 *.gds                 → Final GDSII layout
├── 📁 logs/                     → Step-by-step execution logs
└── 📁 reports/                  → Timing, area, power reports
```

---











