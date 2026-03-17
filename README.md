# 🐉 Software Reverse Engineering: An Introduction to Ghidra
> Detailed static analysis of a binary executable to identify anti-debugging techniques and de-obfuscate stack strings.

---

## 📑 Table of Contents
1. [About the Tool: Ghidra](#-about-the-tool-ghidra)
2. [Practical Case: Static Malware Analysis](#-practical-case-static-malware-analysis)
3. [Phase 1: Anti-Debugging Techniques](#-phase-1-identification-of-anti-analysis-techniques-anti-debugging)
4. [Phase 2: String Obfuscation (Stack Strings)](#-phase-2-string-obfuscation-analysis-stack-strings)
5. [Conclusion](#-conclusion)

---
## 🛠️ About the Tool: Ghidra
**Ghidra** is a powerful, open-source Software Reverse Engineering (SRE) framework developed and maintained by the National Security Agency (NSA). Released to the public in 2019, it has become an industry standard for cybersecurity professionals, malware analysts, and vulnerability researchers.

### Key Features:
* **Advanced Decompiler:** This is Ghidra's crown jewel. It automatically translates complex, low-level machine code (Assembly) into readable C-like pseudocode, drastically reducing the time needed to understand a binary.
* **Multi-Architecture Support:** It can analyze code compiled for Windows, macOS, Linux, Android, and iOS, supporting architectures like x86, x64, ARM, and MIPS.
* **Interactive Disassembler:** Allows analysts to map execution flows, rename variables, and patch instructions dynamically.

Below is a practical example of how Ghidra is used in a real-world scenario to analyze a hostile executable.

---

## 📝 Practical Case: Static Malware Analysis

### Executive Summary
This section contains the reverse engineering analysis of an executable binary (`SimpleCrackme.exe`). The primary objective was to perform an **Advanced Static Analysis** using Ghidra to understand the program's execution flow, identify its protection mechanisms, and inspect the validation logic without executing the code dynamically.

---

## 🔍 Phase 1: Identification of Anti-Analysis Techniques (Anti-Debugging)

During the analysis of the program's main entry point, a classic defense mechanism used by malware developers to hinder the analyst's work was identified.

As observed in the code decompiled by Ghidra, the program makes a call to the Windows API `IsDebuggerPresent()` (line 92). 


![debug 1](https://github.com/user-attachments/assets/9e0b7d81-6860-4ae5-ab43-9797f6efbe9e)

* **Detected Technique:** The program evaluates whether it is being executed under the supervision of a dynamic debugger (such as x64dbg). 
* **Behavior:** If the function returns a non-zero value (true), the execution flow is diverted to a routine that prints the message `"[-] Debugger Detected!"` and stops the validation process, preventing real-time memory inspection.

---

## 🧩 Phase 2: String Obfuscation Analysis (Stack Strings)

Continuing with the static analysis after conceptually bypassing the *Anti-Debugging* barrier, we proceeded to analyze the logic responsible for displaying the success message upon entering the correct password.

Instead of storing the text string in plaintext, the binary's creator utilized a data obfuscation technique known as **Stack Strings**.

![debug 1 2](https://github.com/user-attachments/assets/aa392383-2cbc-41b9-8fda-a2fb72742cda)


* **Detected Technique:** The program constructs the success message (`"success good try but you are found at pass 01..."`) by dividing it into small blocks or syllables and assigning them sequentially to local variables on the Stack during execution.
* **Attacker's Goal:** To hide critical information from basic static analysis tools (like `strings`). Thanks to Ghidra's powerful decompilation engine, it was possible to visually reconstruct this memory puzzle directly from the C pseudocode.

---

## 💡 Conclusion
The use of Ghidra has proven to be fundamental for conducting comprehensive static reconnaissance. It allowed us to map the binary's defenses and unravel data concealment techniques completely safely. This highlights why Ghidra is a mandatory tool in any modern malware analyst's arsenal.
