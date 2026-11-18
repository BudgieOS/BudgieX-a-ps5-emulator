ProsperoX
ProsperoX is an experimental, open-source PlayStation 5 (PS5) and PC emulator/launcher and High-Level Emulation (HLE) compatibility layer written in modern C++20.
Rather than emulating raw x86-64 CPU instructions, ProsperoX leverages the native x86-64 host architecture while translating Prospero OS system calls, kernel primitives, and custom AMD RDNA 2 graphics commands to modern desktop APIs (Vulkan).
> \*\*Disclaimer:\*\* ProsperoX is an educational and research project. It does not include any copyrighted Sony firmware, encryption keys, or games. You must dump your own legally owned console firmware and game backups to use this software.
---
Architecture Overview
Unlike legacy console emulators that rely on dynamic recompilers (JIT) for non-x86 processors, ProsperoX operates similarly to a translation layer combined with low-level hardware virtualisation:
CPU (`src/cpu/`): Native x86-64 execution with custom thread context switching, TLS (Thread Local Storage) patching, and signal/SEH handlers to intercept privileged instructions and memory faults.
GPU (`src/gpu/`): Intercepts low-level RDNA 2 command buffers (`PM4` packets), translates custom GCN/RDNA shader bytecode to SPIR-V, and renders via Vulkan 1.3.
HLE Kernel & Modules (`src/hle/`): Re-implements FreeBSD-derived Prospero OS syscalls, virtual memory management (`vm\_map`), and core dynamic libraries (`libSceSystemService`, `libScePad`, `libSceUserService`, etc.).
Audio (`src/audio/`): Emulates custom audio output pipelines and translates spatial audio calls to standard host backends via SDL2/Cubeb.
---
Current Status
ProsperoX is currently in early alpha.
[x] ELF & SELF binary loader and relocation parser
[x] Basic HLE kernel thread scheduling and mutex/semaphore primitives
[x] Vulkan backend initialization and basic compute/graphics pipeline translation
[x] DualSense controller input mapping (via SDL2)
[ ] Full shader compiler coverage (Tessellation / Geometry / Raytracing)
[ ] Tempest 3D AudioTech spatial translation
[ ] Commercial game bootability (Currently boots homebrew ELFs and basic SDK samples)
---
System Requirements
Because PS5 titles are heavily multithreaded and rely on modern GPU features, a high-end host system is required:
Component	Minimum	Recommended
OS	Windows 10 (64-bit) / Ubuntu 22.04+	Windows 11 / Arch Linux (Kernel 6.5+)
CPU	6-core x86-64 with AVX2 support	8-core+ AMD Zen 3 / Intel 12th Gen+
GPU	Vulkan 1.3 capable (4GB VRAM)	AMD RX 6700 XT / NVIDIA RTX 3060+ (8GB+ VRAM)
RAM	16 GB	32 GB (PS5 uses 16GB unified GDDR6)
Storage	SSD	NVMe Gen4 SSD (for asset
