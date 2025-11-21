# 🌟 RISC-V SoC Tapeout – Week-7: BabySoC Physical Design, Post-Route SPEF Generation and A Complete RTL-to-GDSII Physical Design with OpenROAD

💎 This week represents the culmination of your RISC-V SoC journey - where all theory meets practice in a complete, automated physical design flow. Let's transform VSDBabySoC from RTL to silicon! 

---

## 🌟 Program Overview

### 📚 Course Context

🔄 **Transition:** Manual stage-wise physical design → Full automation with OpenROAD-Flow-Scripts

🎯 **Design Target:** VSDBabySoC - Complete 8-bit RISC-V SoC with analog peripherals

💾 **Technology:** Sky130 HD PDK (130nm High Density Process Design Kit)

### 🎨 What Makes This Week Special

📦 **Integration Week** - Bringing together all previous stages into unified automation

🤖 **Full Automation** - Leveraging ORFS to streamline entire physical design process

🔬 **Real-World Experience** - Understanding production SoC design flow from RTL to silicon

💡 **End-to-End Flow** - Complete journey from Verilog source to manufacturable GDSII layout

🏗️ **Open-Source Tools** - Demonstrating powerful capabilities of open-source ASIC design

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

## 🧩 Design Integration - VSDBABYSOC

### 📁 Directory Structure Setup

#### 🗂️ Step 1: Create Design Directories

```bash
📂 Create design hierarchy
mkdir -p flow/designs/sky130hd/vsdbabysoc
mkdir -p flow/designs/src/vsdbabysoc
```

#### 📋 Step 2: File Organization

**🎯 Into `designs/sky130hd/vsdbabysoc/`:**

📐 **Layout Files:**
- 🟦 `gds/` → avsddac.gds, avsdpll.gds (GDSII macro layouts)
- 🟨 `lef/` → avsddac.lef, avsdpll.lef (Library Exchange Format abstracts)

📚 **Library Files:**
- 📖 `lib/` → avsddac.lib, avsdpll.lib (Timing and power characterization)

📄 **Configuration Files:**
- 📜 `include/` → all `.vh` Verilog header files
- ⏱️ `vsdbabysoc_synthesis.sdc` → Timing constraints file
- 🎯 `macro.cfg` → Macro placement configuration
- 📌 `pin_order.cfg` → Pin ordering specification

**🎯 Into `designs/src/vsdbabysoc/`:**

💻 **RTL Source Files:**
- 🔵 `vsdbabysoc.v` → Top-level SoC module
- 🟢 `rvmyth.v` → RISC-V processor core (MYTHcore)
- 🟡 `clk_gate.v` → Clock gating logic

### 📊 Design Components

🔷 **Digital Core:** RISC-V MYTHcore processor (8-bit architecture)

🔶 **Analog Macros:**
- 🎵 AVSD DAC (Digital-to-Analog Converter)
- ⏰ AVSD PLL (Phase-Locked Loop for clock generation)

🔸 **Clock Management:** Clock gating cells for power optimization

🔹 **Technology:** Sky130 HD standard cell library

---

## ⚙️ Configuration Setup

### 📝 Complete `config.mk` Configuration

```makefile
# 🎯 Design Identification
export DESIGN_NICKNAME = vsdbabysoc
export DESIGN_NAME = vsdbabysoc
export PLATFORM    = sky130hd

# 📂 RTL Source Files
# export VERILOG_FILES_BLACKBOX = $(DESIGN_HOME)/src/$(DESIGN_NICKNAME)/IPs/*.v
# export VERILOG_FILES = $(sort $(wildcard $(DESIGN_HOME)/src/$(DESIGN_NICKNAME)/*.v))
# Explicitly list the Verilog files for synthesis
export VERILOG_FILES = $(DESIGN_HOME)/src/$(DESIGN_NICKNAME)/vsdbabysoc.v \
                       $(DESIGN_HOME)/src/$(DESIGN_NICKNAME)/rvmyth.v \
                       $(DESIGN_HOME)/src/$(DESIGN_NICKNAME)/clk_gate.v

# ⏱️ Timing Constraints
export SDC_FILE = $(DESIGN_HOME)/$(PLATFORM)/$(DESIGN_NICKNAME)/vsdbabysoc_synthesis.sdc

# 📁 Design Directory Path
export vsdbabysoc_DIR = $(DESIGN_HOME)/$(PLATFORM)/$(DESIGN_NICKNAME)

# 📚 Additional Design Files
export VERILOG_INCLUDE_DIRS = $(wildcard $(vsdbabysoc_DIR)/include/)
# export SDC_FILE      = $(wildcard $(vsdbabysoc_DIR)/sdc/*.sdc)
export ADDITIONAL_GDS  = $(wildcard $(vsdbabysoc_DIR)/gds/*.gds.gz)
export ADDITIONAL_LEFS = $(wildcard $(vsdbabysoc_DIR)/lef/*.lef)
export ADDITIONAL_LIBS = $(wildcard $(vsdbabysoc_DIR)/lib/*.lib)
# export PDN_TCL = $(DESIGN_HOME)/$(PLATFORM)/$(DESIGN_NICKNAME)/pdn.tcl

# ⏰ Clock Configuration (vsdbabysoc specific)
# export CLOCK_PERIOD = 20.0
export CLOCK_PORT = CLK
export CLOCK_NET = $(CLOCK_PORT)

# 📐 Floorplanning Configuration (vsdbabysoc specific)
export FP_PIN_ORDER_CFG = $(wildcard $(DESIGN_DIR)/pin_order.cfg)
# export FP_SIZING = absolute
export DIE_AREA  = 0 0 1600 1600
export CORE_AREA = 20 20 1590 1590

# 📌 Placement Configuration (vsdbabysoc specific)
export FP_PIN_ORDER_CFG = $(wildcard $(DESIGN_DIR)/pin_order.cfg)
export MACRO_PLACEMENT_CFG = $(wildcard $(DESIGN_DIR)/macro.cfg)
export PLACE_PINS_ARGS = -exclude left:0-600 -exclude left:1000-1600: -exclude right:* -exclude top:* -exclude bottom:*
# export MACRO_PLACEMENT = $(DESIGN_HOME)/$(PLATFORM)/$(DESIGN_NICKNAME)/macro_placement.cfg


# 🎯 Synthesis Optimization
export TNS_END_PERCENT = 100
export REMOVE_ABC_BUFFERS = 1

# 🔧 Magic Tool Configuration
export MAGIC_ZEROIZE_ORIGIN = 0
export MAGIC_EXT_USE_GDS = 1

# ⏰ CTS Tuning Parameters
export CTS_BUF_DISTANCE = 600
export SKIP_GATE_CLONING = 1
# export CORE_UTILIZATION=0.1  # Reduce this value to allow more whitespace for routing.
```

### 📋 Configuration Parameters Explained

🎯 **DESIGN_NICKNAME:** Short name for design identification

🖥️ **PLATFORM:** Technology node (sky130hd = SkyWater 130nm High Density)

📄 **VERILOG_FILES:** List of all RTL source files for synthesis

⏱️ **SDC_FILE:** Synopsys Design Constraints for timing specifications

⏰ **CLOCK_PORT/NET:** Primary clock signal identification

📐 **DIE_AREA:** Total chip dimensions (1600μm × 1600μm)

📦 **CORE_AREA:** Usable area for standard cells (20μm margin)

🎯 **MACRO_PLACEMENT_CFG:** Pre-defined macro locations

📌 **FP_PIN_ORDER_CFG:** I/O pin placement rules

🔧 **PLACE_PINS_ARGS:** Pin exclusion zones for routing

⚡ **TNS_END_PERCENT:** Timing optimization target (100% = complete)

🔄 **REMOVE_ABC_BUFFERS:** Synthesis buffer optimization

🎨 **MAGIC_ZEROIZE_ORIGIN:** Layout coordinate system reference

🔍 **MAGIC_EXT_USE_GDS:** Parasitic extraction from GDSII

⏰ **CTS_BUF_DISTANCE:** Maximum buffer spacing in clock tree (600μm)

🚫 **SKIP_GATE_CLONING:** Disable gate cloning for CTS

---

## 🚀 Flow Execution

### 🔧 Environment Preparation

```bash
# 📂 Navigate to ORFS directory
cd OpenROAD-flow-scripts

# 📁 Enter flow directory
cd flow

# 🌍 Source environment variables
source ../env.sh
```

---

## 1️⃣ Synthesis Stage

### 🔨 Execute Synthesis

```bash
# ⚙️ Run logic synthesis with Yosys
make DESIGN_CONFIG=./designs/sky130hd/vsdbabysoc/config.mk synth
```

### 📊 Synthesis Outputs

✅ **Gate-Level Netlist:** `1_synth.v` - RTL converted to standard cells

📈 **Timing Reports:** Setup/hold time analysis, WNS, TNS

📊 **Area Report:** Cell count, total area, utilization statistics

🔍 **Check Report:** `synth_check.txt` - Design rule violations

📉 **Statistics:** `synth_stat.txt` - Cell type distribution

### 🎯 Key Metrics to Verify

✓ **Total Cells:** Approximately 30,000 standard cells

✓ **Sequential Elements:** Flip-flops, latches count

✓ **Combinational Logic:** Gate count by type

✓ **Macro Instances:** DAC and PLL integration verified

✓ **Timing Estimate:** Initial WNS/TNS before physical design

---

## 2️⃣ Floorplan Stage

### 📐 Execute Floorplanning

```bash
# 🗺️ Run floorplan generation
make DESIGN_CONFIG=./designs/sky130hd/vsdbabysoc/config.mk floorplan
```

### 🎨 Floorplan Achievements

📏 **Die Sizing:** 1600μm × 1600μm total die area established

📦 **Core Area:** 1570μm × 1570μm usable placement region

🎯 **Macro Placement:** DAC and PLL positioned per macro.cfg

📌 **Pin Assignment:** I/O pins placed according to pin_order.cfg

⚡ **Power Grid:** Power rings and straps generated

🔌 **VDD/VSS Network:** Complete power distribution structure

```bash
# 🖼️ Open GUI to visualize floorplan
make DESIGN_CONFIG=./designs/sky130hd/vsdbabysoc/config.mk gui_floorplan
```

### 🖼️ Visual Verification

🟦 **Die Boundary:** Outer rectangle defining chip edges

🟨 **Core Area:** Inner placement region for standard cells

🟥 **Macro Blocks:** Rectangular analog IP placements

🟩 **Power Rings:** Metal straps forming power distribution

🟪 **I/O Pins:** Metal rectangles at die periphery

---

## 3️⃣ Placement Stage

### 🎯 Execute Placement

```bash
# 📍 Run global and detailed placement
make DESIGN_CONFIG=./designs/sky130hd/vsdbabysoc/config.mk place
```

### 🏗️ Placement Process

🌍 **Global Placement:** Initial cell positioning optimizing wirelength

🎯 **Detailed Placement:** Legalization ensuring design rule compliance

📊 **Density Optimization:** Balancing cell distribution for routability

🔍 **Congestion Analysis:** Identifying potential routing bottlenecks

```bash
# 🖼️ Open GUI to visualize placement
make DESIGN_CONFIG=./designs/sky130hd/vsdbabysoc/config.mk gui_place
```

### 📊 Analysis Heatmaps

#### 📌 Routing Congestion Map
🟥 **Hot Spots:** Areas with high net density (potential routing issues)
🟨 **Medium Density:** Moderate routing complexity
🟩 **Low Density:** Easy routing regions

#### 📌 Estimated Congestion (RUDY)
📏 **RUDY Metric:** Rectangular Uniform wire Density
🎯 **Purpose:** Predicting routing difficulty before actual routing

#### 📌 IR Drop Analysis
⚡ **Voltage Drop:** Power supply degradation across chip
🔴 **Critical Areas:** Regions with significant IR drop (>10% VDD)
🟢 **Safe Regions:** Adequate power delivery (<5% drop)

#### 📌 Pin Density Distribution
📍 **High Density:** Areas with many cell pins
🎯 **Routing Impact:** Pin clusters require more routing resources

#### 📌 Placement Density Map
📦 **Utilization:** Percentage of area occupied by cells
🎯 **Target:** 55-65% for VSDBabySoC (allows routing flexibility)

#### 📌 Power Density Visualization
⚡ **Power Hotspots:** Areas with high switching activity
🌡️ **Thermal Concerns:** Regions requiring cooling consideration

### 🔬 Cell-Level Inspection

🔍 **Zoom View:** Individual standard cells visible in layout

📐 **Row Structure:** Cells aligned in horizontal placement rows

🔌 **Well Taps:** Power/ground connections at regular intervals

🎨 **Macro Boundaries:** Clear separation between analog and digital

---

## 4️⃣ Clock Tree Synthesis

### ⏰ Execute CTS

```bash
# 🌳 Build clock distribution tree
make DESIGN_CONFIG=./designs/sky130hd/vsdbabysoc/config.mk cts
```

### 🌳 CTS Deliverables

⚖️ **Balanced Clock Tree:** Equal path lengths to all sequential elements

🔄 **Buffer Insertion:** Clock buffers added for signal integrity

📊 **Skew Optimization:** Minimizing clock arrival time differences

⚡ **Slew Control:** Managing clock edge transition times

```bash
# 🖼️ Open GUI to visualize clock tree
make DESIGN_CONFIG=./designs/sky130hd/vsdbabysoc/config.mk gui_cts
```

### 🎯 CTS Verification Metrics

✓ **Clock Skew:** < 1 ns (target achieved)

✓ **WNS (Worst Negative Slack):** ≈ 0 ns (timing met)

✓ **TNS (Total Negative Slack):** ≈ 0 ns (no timing violations)

✓ **Clock Latency:** Insertion delay from source to sinks

✓ **Buffer Count:** Number of buffers/inverters in clock path

### 🔍 Clock Tree Structure

🌲 **Tree Topology:** H-tree or fishbone structure for balance

🔵 **Root:** Clock source (PLL output or primary input)

🟢 **Branches:** Hierarchical distribution levels

🟡 **Leaves:** Clock pins of flip-flops/registers

🔴 **Buffers:** Intermediate drivers maintaining signal strength

---

## 5️⃣ Routing Stage

### 🛣️ Execute Routing

```bash
# 🗺️ Run global and detailed routing
make DESIGN_CONFIG=./designs/sky130hd/vsdbabysoc/config.mk route
```

### 🎯 Routing Process

🌍 **Global Routing:** High-level path planning for all nets

🔍 **Track Assignment:** Allocating routing resources per net

🎨 **Detailed Routing:** TritonRoute performs precise metal routing

📏 **DRC Fixing:** Iterative correction of design rule violations

✓ **Verification:** Final DRC check ensuring manufacturability

### 📊 Routing Outputs

✅ **Routed DEF:** Complete design with all metal connections

🔌 **Power Grid:** VDD/VSS mesh completed with droplet connections

🌐 **Signal Nets:** All logical connections physically implemented

📏 **Via Insertion:** Vertical connections between metal layers

🚫 **DRC Status:** 0 violations (clean layout)

```bash
# 🖼️ Open GUI to visualize routing
make DESIGN_CONFIG=./designs/sky130hd/vsdbabysoc/config.mk gui_route
```

### 🎨 Routing Visualization

🟦 **Metal Layers:** Different colors for M1, M2, M3, M4, M5

🔵 **Horizontal Routing:** Even metal layers (M2, M4)

🟣 **Vertical Routing:** Odd metal layers (M1, M3, M5)

🟡 **Vias:** Small squares connecting different metal layers

🟢 **Power Straps:** Thick metal lines for power distribution

---

## 6️⃣ Parasitic Extraction

### ⚡ Generate SPEF File

```bash
# 🔬 Extract post-route parasitics
make DESIGN_CONFIG=./designs/sky130hd/vsdbabysoc/config.mk parasitics
```

### 📊 SPEF Output Location

📂 **File Path:** `results/sky130hd/vsdbabysoc/parasitics/vsdbabysoc.spef`

### 🧠 Understanding SPEF

📖 **Format:** Standard Parasitic Exchange Format (IEEE 1481)

🔌 **Contents:** Net-by-net resistance and capacitance values

⚡ **RC Network:** Distributed RC model of each interconnect

🎯 **Purpose:** Accurate delay calculation for timing signoff

### 💡 SPEF Components

**📏 Resistance Effects:**
- 🔴 Wire resistance (Ω per unit length)
- 🔵 Via resistance at layer transitions
- 🟢 Impact: Voltage drop and delay increase

**⚡ Capacitance Effects:**
- 🟡 Wire-to-ground capacitance
- 🟣 Coupling capacitance between adjacent nets
- 🟠 Impact: Increased delay and potential crosstalk

### 📈 Example Parasitic Impact

💡 **1mm wire on Sky130:**
- 📏 Adds ≈ 100 fF capacitance
- 🔴 Adds ≈ 10 Ω resistance
- ⏱️ Results in > 1 ns additional delay

### 🎯 SPEF Usage in STA

📊 **Back-Annotation:** SPEF loaded into OpenSTA timing engine

⚡ **Accurate Delays:** Real parasitic delays replace wire load models

✓ **Signoff Analysis:** Final timing verification with actual layout

🎯 **Timing Closure:** Iterative optimization until timing met

---

## 🧾 Final Verification Summary

### 📊 Design Metrics

| Parameter              | Result / Observation                             |
| :--------------------- | :----------------------------------------------- |
| 🎯 **Design**          | VSDBabySoC – 8-bit RISC-V SoC Core + Peripherals|
| 💾 **Technology**      | Sky130 HD PDK (130nm High Density)              |
| 📦 **Core Utilization**| ≈ 55 – 65 % (optimal for routing)               |
| 🔢 **Total Instances** | ≈ 30,000 standard cells                         |
| ⏰ **Clock Skew**      | < 1 ns (well-balanced tree)                     |
| 🌐 **Routed Nets**     | All nets successfully connected                  |
| 🚫 **DRC Violations**  | 0 (clean, manufacturable layout)                |
| ⚡ **SPEF File**        | Generated successfully for post-route STA        |
| 📐 **Die Area**        | 1600 μm × 1600 μm                               |
| 📦 **Core Area**       | 1570 μm × 1570 μm                               |
| 🔌 **Macro Count**     | 2 (AVSD DAC + AVSD PLL)                         |

---

## 📒 Key Learnings — Week 7

### 🛠️ Tools and Frameworks Mastered

✔ **OpenROAD** - Complete physical design automation platform

✔ **Yosys** - Open-source RTL synthesis engine

✔ **OpenSTA** - Static timing analysis for signoff

✔ **TritonTools** - Suite for floorplan, placement, CTS, routing

✔ **Sky130 PDK** - Technology libraries for ASIC implementation

✔ **ORFS Environment** - Standardized scripts for repeatable flows

✔ **Magic** - Layout viewer and parasitic extraction

✔ **KLayout** - GDSII visualization and verification

### 🔹 Workflow Achievements

#### 1️⃣ Environment Setup & Verification
- 🔧 ORFS installation and dependency resolution
- 🏗️ Built OpenROAD locally from source
- ✅ Verified Yosys, OpenROAD, TritonTools binaries

#### 2️⃣ Design Integration
- 📝 Added VSDBabySoC RTL files to ORFS structure
- 🎯 Integrated analog macros (DAC, PLL) with LEF/GDS/LIB
- ⚙️ Configured floorplan, timing, and placement parameters

#### 3️⃣ Full Flow Execution
- 🔨 **Synthesis:** Gate-level netlist, timing, area reports
- 📐 **Floorplan:** Die/core dimensions, pin/macro placement
- 🎯 **Placement:** Global and detailed cell positioning
- ⏰ **CTS:** Balanced clock tree with skew optimization
- 🛣️ **Routing:** DRC-clean metal connections
- 📊 **SPEF:** Parasitic extraction for accurate timing

#### 4️⃣ Analysis and Optimization
- 📊 Congestion heatmap analysis
- ⚡ IR drop identification and mitigation
- 📍 Density distribution optimization
- 🔍 Timing path analysis and fixing

### 💡 Technical Insights Gained

🎯 **Floorplan Impact:** Macro placement critically affects routing congestion

⚡ **Timing Closure:** CTS and routing can significantly alter timing results

🔌 **Power Planning:** Adequate power grid prevents IR drop issues

📊 **Utilization Trade-off:** Higher density saves area but complicates routing

🎨 **Visualization:** Heatmaps essential for identifying design bottlenecks

🔍 **Iteration:** Physical design is iterative, not one-shot process

---

## 🧾 Command Reference Summary

| Stage                     | Command                                          |
| :------------------------ | :----------------------------------------------- |
| 🔨 **Synthesis**          | `make DESIGN_CONFIG=...config.mk synth`          |
| 📐 **Floorplan**          | `make DESIGN_CONFIG=...config.mk floorplan`      |
| 🎯 **Placement**          | `make DESIGN_CONFIG=...config.mk place`          |
| ⏰ **CTS**                | `make DESIGN_CONFIG=...config.mk cts`            |
| 🛣️ **Routing**           | `make DESIGN_CONFIG=...config.mk route`          |
| ⚡ **Parasitic Extract**  | `make DESIGN_CONFIG=...config.mk parasitics`     |
| 🖼️ **GUI Floorplan**      | `make DESIGN_CONFIG=...config.mk gui_floorplan`  |
| 🖼️ **GUI Placement**      | `make DESIGN_CONFIG=...config.mk gui_place`      |
| 🖼️ **GUI CTS**            | `make DESIGN_CONFIG=...config.mk gui_cts`        |
| 🖼️ **GUI Routing**        | `make DESIGN_CONFIG=...config.mk gui_route`      |
| 🔍 **Full Flow**          | `make DESIGN_CONFIG=...config.mk`                |

---

