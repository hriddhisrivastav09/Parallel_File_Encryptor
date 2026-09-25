
# Parallel File Encryption System with Task Scheduling

A C++-based parallel file encryption system that uses **multithreading, task scheduling, and AES-256-GCM encryption** to securely encrypt and decrypt files. The system is designed to explore operating system concepts such as process synchronization, thread management, task scheduling, concurrency, and performance optimization.

---

## Project Overview

The **Parallel File Encryption System with Task Scheduling** is an operating systems project that combines secure cryptography with parallel processing.

Traditional file encryption processes files sequentially, which can become time-consuming when handling multiple files. This project addresses this limitation by using a thread pool and task scheduling to process multiple encryption and decryption tasks concurrently.

The system uses the **AES-256-GCM encryption algorithm**, which provides both confidentiality and integrity. Tasks are managed through a thread-safe queue and executed by multiple worker threads according to the selected scheduling policy.

---

## Objectives

- Implement secure file encryption and decryption using AES-256-GCM.
- Use multithreading to process multiple file operations in parallel.
- Implement a thread-safe task queue for managing encryption jobs.
- Support task scheduling policies such as FCFS and Priority Scheduling.
- Apply operating system concepts including synchronization and resource management.
- Measure execution time and system performance.
- Prevent incomplete or corrupted output files using temporary file handling.
- Develop a modular and maintainable C++ application.

---

## Features

### Secure File Encryption

- AES-256-GCM authenticated encryption.
- Password-based key derivation using PBKDF2-HMAC-SHA256.
- Random salt generation for each file.
- Random initialization vector (IV/nonce) for each encryption operation.
- Authentication tag verification during decryption.

### Parallel Processing

- Multiple worker threads process tasks concurrently.
- Thread pool architecture for efficient thread management.
- Thread-safe task queue using mutexes and condition variables.

### Task Scheduling

- First-Come, First-Served (FCFS) scheduling.
- Priority-based task scheduling.
- Arrival sequence used to resolve scheduling ties.
- Centralized task management.

### Performance Monitoring

- Track task execution time.
- Measure encryption and decryption performance.
- Compare sequential and parallel processing.
- Monitor completed and failed tasks.

### File Safety

- Stream-based file processing.
- 64 KiB processing chunks.
- Temporary output files during encryption and decryption.
- Final output created only after successful completion.

---

## System Architecture

```text
                 ┌──────────────────────┐
                 │      CLI / User      │
                 │       Interface     │
                 └──────────┬───────────┘
                            │
                            ▼
                 ┌──────────────────────┐
                 │     Task Manager     │
                 │  Create & Manage     │
                 │       Tasks         │
                 └──────────┬───────────┘
                            │
                            ▼
                 ┌──────────────────────┐
                 │   Thread-Safe Queue  │
                 │   FCFS / Priority    │
                 └──────────┬───────────┘
                            │
                            ▼
                 ┌──────────────────────┐
                 │     Worker Pool      │
                 │   Multiple Threads   │
                 └──────────┬───────────┘
                            │
                            ▼
                 ┌──────────────────────┐
                 │    Crypto Engine     │
                 │     AES-256-GCM      │
                 └──────────┬───────────┘
                            │
                            ▼
                 ┌──────────────────────┐
                 │   File & Metrics     │
                 │  Output and Reports  │
                 └──────────────────────┘
```

---

## Workflow

1. The user submits an encryption or decryption task.
2. The Task Manager validates the task details.
3. The task is inserted into the thread-safe queue.
4. The scheduler selects the next task based on the scheduling policy.
5. A worker thread retrieves the task.
6. The Crypto Engine processes the file using AES-256-GCM.
7. The output is written to a temporary file.
8. After successful processing, the temporary file is renamed to the final output.
9. Execution metrics are recorded.
10. The task is marked as completed or failed.

---

## Encryption Design

The project uses AES-256-GCM through the OpenSSL EVP API.

### Key Derivation

The encryption key is derived from the user's password using:

```text
Password
   │
   ▼
PBKDF2-HMAC-SHA256
   │
   ▼
256-bit Encryption Key
```

### Per-File Security Parameters

| Parameter | Value |
|---|---|
| Encryption Algorithm | AES-256-GCM |
| Key Size | 256 bits |
| Key Derivation | PBKDF2-HMAC-SHA256 |
| Salt Size | 16 bytes |
| IV / Nonce Size | 12 bytes |
| Authentication Tag | 16 bytes |
| Processing Chunk Size | 64 KiB |

### Encrypted File Layout

```text
┌────────────┬────────────┬──────────┬────────────────┬──────────────┐
│ Magic      │ Salt       │ IV       │ Ciphertext     │ GCM Tag      │
│ 4 bytes    │ 16 bytes   │ 12 bytes │ Variable size  │ 16 bytes     │
│ PFE1       │            │          │                │              │
└────────────┴────────────┴──────────┴────────────────┴──────────────┘
```

The salt and IV are stored with the encrypted file. The encryption key is derived from the password and is not stored in the file.

> **Security Note:** A strong password is required for secure encryption. The password itself must never be stored in the encrypted file.

---

## Operating System Concepts

This project demonstrates the following operating system concepts:

### 1. Multithreading

Multiple worker threads process independent file tasks concurrently.

### 2. Thread Synchronization

Mutexes and condition variables are used to coordinate access to shared resources.

### 3. Producer-Consumer Model

- **Producer:** Task Manager adds tasks to the queue.
- **Consumer:** Worker threads retrieve and execute tasks.
- **Shared Resource:** Thread-safe task queue.

### 4. Task Scheduling

Tasks are selected according to a scheduling policy such as FCFS or Priority Scheduling.

### 5. Mutual Exclusion

Mutexes prevent race conditions when multiple threads access shared data.

### 6. Resource Management

The worker pool controls the number of active threads and supports orderly shutdown.

### 7. Performance Optimization

Parallel execution is evaluated against sequential processing to understand the benefits and limitations of concurrency.

---

## Technology Stack

| Technology | Purpose |
|---|---|
| C++17 | Core programming language |
| CMake | Build system |
| OpenSSL | Cryptographic operations |
| Windows / MSVC | Development environment |
| Git & GitHub | Version control |
| VS Code | Development environment |

---

## Prerequisites

Before building the project, install the following:

- Windows 10 or Windows 11.
- Visual Studio Build Tools or Visual Studio with C++ development tools.
- MSVC compiler.
- CMake 3.16 or later.
- Git.
- OpenSSL development libraries and headers.
- VS Code (recommended).

### Required Visual Studio Components

- MSVC C++ build tools.
- Windows SDK.
- C++ CMake tools for Windows.

---

## Installation and Setup

### 1. Clone the Repository

```bash
git clone https://github.com/<your-username>/ParallelFileEncryption.git
cd ParallelFileEncryption
```

Replace `<your-username>` with your GitHub username.

### 2. Configure OpenSSL

Install OpenSSL using a compatible Windows installation method.

If OpenSSL is installed in the default directory, the installation path may be:

```text
C:\Program Files\OpenSSL-Win64
```

The actual path depends on the selected installer and installation settings.

Make sure the OpenSSL installation contains the required development headers and libraries.

### 3. Configure CMake

Create a build directory:

```powershell
cmake -S . -B build -G "Visual Studio 17 2022" -A x64
```

If your Visual Studio installation uses a different generator, select the appropriate generator installed on your system.

> **Note:** The CMake configuration must be updated if OpenSSL is installed outside the default search paths.

### 4. Build the Project

```powershell
cmake --build build --config Release
```

### 5. Run the Application

```powershell
.\build\Release\pfe.exe
```

The executable name may change if the CMake target is modified.

---

## Usage

The command-line interface will support file encryption and decryption operations.

### Planned Encryption Command

```powershell
pfe.exe encrypt --input data/input.txt --output data/output.enc
```

### Planned Decryption Command

```powershell
pfe.exe decrypt --input data/output.enc --output data/recovered.txt
```

### Planned Scheduling Configuration

```powershell
pfe.exe encrypt --input data/ --policy fcfs --threads 4
```

```powershell
pfe.exe encrypt --input data/ --policy priority --threads 4
```

> The commands above represent the planned interface. They should be used after the corresponding CLI functionality has been implemented.

---

## Testing Plan

The project will include tests for the following scenarios:

### Functional Testing

- Encrypt a single file.
- Decrypt an encrypted file.
- Verify that decrypted content matches the original content.
- Process multiple files.
- Handle empty files.
- Handle large files.

### Security Testing

- Verify authentication tag validation.
- Test decryption with an incorrect password.
- Test corrupted encrypted files.
- Verify unique salt and IV generation.
- Confirm that incomplete output files are not treated as successful results.

### Concurrency Testing

- Submit multiple tasks simultaneously.
- Verify thread-safe queue operations.
- Test multiple worker threads.
- Verify correct task completion counts.
- Test worker pool shutdown.

### Scheduling Testing

- Test FCFS scheduling.
- Test priority-based scheduling.
- Verify ordering for tasks with equal priority.
- Compare scheduling behavior under different workloads.

---

## Security Considerations

- Use authenticated encryption with AES-256-GCM.
- Generate a unique random salt for each file.
- Generate a unique random IV for each encryption operation.
- Never store the user's password in plaintext.
- Validate the authentication tag before accepting decrypted data.
- Avoid overwriting the original file unless explicitly requested.
- Write to temporary files before finalizing the output.
- Do not commit private files, passwords, encrypted test data, or build artifacts to Git.

---

## Current Development Status

| Component | Status |
|---|---|
| Project structure | In progress |
| C++17 configuration | Planned / configured |
| Compiler detection | Completed |
| CMake setup | In progress |
| OpenSSL setup | In progress |
| Crypto Engine | Planned |
| Task Manager | Planned |
| Thread-safe Queue | Planned |
| Worker Pool | Planned |
| Scheduling Policies | Planned |
| Metrics Module | Planned |
| CLI | Planned |
| Testing | Planned |
| Performance Evaluation | Planned |

The project is being developed incrementally, beginning with the development environment and dependency configuration.

---

## 🔮 Future Enhancements

- Support additional scheduling policies.
- Add a graphical user interface.
- Implement cancellation of queued tasks.
- Add progress reporting for large files.
- Support configurable worker thread counts.
- Improve performance monitoring.
- Add automated unit and integration testing.
- Generate detailed execution reports.
- Explore hardware acceleration where appropriate.

---

## Learning Outcomes

Through this project, we aim to gain practical knowledge of:

- Operating system scheduling algorithms.
- Multithreaded programming in C++.
- Thread synchronization and concurrency.
- Cryptographic APIs and secure file processing.
- CMake-based project organization.
- Performance measurement and optimization.
- Software testing and modular system design.

---

## ⭐ Acknowledgements

- OpenSSL for cryptographic functionality.
- CMake for cross-platform build configuration.
- Microsoft Visual Studio and MSVC for C++ development tools.
