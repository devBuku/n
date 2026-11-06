# Unified Memory Architecture (UMA)

### Traditional PC (Intel/AMD)

- The CPU (Central Processing Unit) and RAM (Random Access Memory) are separate components.
- The CPU processes instructions, while RAM stores the data that programs are currently using.
- The CPU and GPU (Graphics Processing Unit) typically have separate memory pools.

### Apple Silicon (M-series)

- Apple combines the CPU, GPU, and other components into a single chip called an SoC (System on a Chip).
- The CPU and GPU share the same physical RAM. This is called Unified Memory Architecture (UMA).
- This reduces the need to copy data between separate memory pools.

# Programs and Processes

### Program

- A program is a set of instructions stored on disk, along with the data and resources it needs to run.
- Installing a program means copying its files to the disk and setting it up so the operating system can run it.
- A program sitting on the disk cannot do anything by itself. It needs to be executed by the processor.

### Process

- A process is a program that is currently running.
- The operating system gives each process its own virtual memory space to work with.
- Each process has a unique number called a PID (Process ID), which the operating system uses to identify it.
- One program can have multiple processes running at the same time.

### Browser Example

- A browser may have one process for the main browser and separate processes for its tabs.
- This helps isolate tabs from one another. If something goes wrong in one tab, it is less likely to affect the others.
- Browsers use multiple security mechanisms, including process isolation, to protect users from potentially harmful websites.

### macOS App Bundle

- A macOS app is usually a bundle, which is a folder containing the program and its related files.
- Finder makes this folder appear as a single app icon.
- The icon in the Dock is not the app itself. It is a shortcut to the app bundle.
![[macos_bundle.png]]
 
## What Does the Operating System Do All Day?

1. The operating system manages the hardware and shares its resources between running processes.
2. Regular programs cannot directly access hardware whenever they want. They must follow rules set by the operating system.
3. The OS manages which process gets CPU time, which memory it can access, which files it can open, and what it can display on the screen.

### Why Do We Need a Bridge?

- Imagine two programs trying to draw different things on the same part of the screen.
- Without proper management, one program could overwrite the other's work.
- The OS acts as a bridge, managing access to shared resources and preventing programs from interfering with each other.

### The Kernel

- The kernel is the core part of the operating system that manages hardware and system resources.
- It handles tasks such as CPU scheduling, memory management, and access to files and devices.
- The kernel runs with special privileges, while regular programs run with fewer privileges in **user space** (also called userland).
- A code editor, terminal, and browser are examples of programs running in user space.
- A typical operating system has one kernel managing the system.

### System Calls

- A system call is a way for a program running in user space to request a service from the kernel.
- For example, when a program needs to read a file, it can make a system call to request that the kernel do it.
- Reducing unnecessary system calls can improve performance because switching between user space and kernel space has some overhead.

### fs_usage

- `fs_usage` is a macOS command-line tool that shows file system activity, including operations made by processes through the kernel.

```bash
sudo fs_usage
```

![[system_call_workflow.png]]