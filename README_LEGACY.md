# Yakit & Yaklang — Legacy Overview (English Edition)

</div>

## 1. Introduction to the Yak Language

A group of seasoned cybersecurity engineers grew frustrated with the limitations of general-purpose programming languages in offensive security—they wanted an open-source language with **Go's high-concurrency performance**, **Python's clean and simple syntax**, and the portability of a **zero-dependency scripting language** that could be shared and executed immediately without installing third-party packages. Above all, it needed **built-in native security primitives**—such as port probing, stateless SYN scanning, and Nmap service fingerprinting—exposed directly as standard functions in a **Turing-complete**, easy-to-learn language.

That vision gave birth to **Yaklang**!

**Yak** is the world's first **Cybersecurity Domain-Specific Language (DSL)**. Hosted on Go, it inherits Go's concurrency model and library ecosystem while acting as a superset of common data-description languages, complete with dedicated VS Code tooling. Yak exposes low-level offensive and defensive capabilities directly as built-in functions, including port scanning, service fingerprinting, PoC frameworks, shell management, MITM interception, and an extensible plugin architecture.

---

## 2. Introduction to the Yakit Standalone Workbench

**Yakit** is an all-in-one cybersecurity workbench built on top of Yaklang, designed to cover the entire penetration testing lifecycle in a single graphical interface.

While CLI scripting in Yaklang is powerful, writing code raises the barrier to entry for day-to-day testing. To make Yaklang's native capabilities accessible to every security practitioner, we built a decoupled `yak grpc` server and paired it with the **Yakit GUI client**.

---

### 2.1 A Modern Challenger to Burp Suite

For over a decade, Burp Suite has been a staple for web security practitioners, yet common pain points persist: cracked builds carry high malware/supply-chain risks, commercial licenses are expensive, extensions can be cumbersome to write, and heavy Java runtime dependencies complicate deployment. Built on Yaklang, Yakit implements and extends core web proxy workflows to offer security engineers a fast, modern alternative.

Yakit includes request/response interception and tampering, **HTTP History**, **Repeater**, **Intruder**, and an innovative **GUI Fuzztag** engine inside **Web Fuzzer** for rapid vulnerability testing.

**HTTP History**：

![image-20220106162406937](./imgs/image-20220106162406937.png)

**WEB Fuzzer:**

![image-20220106162442487](./imgs/image-20220106162442487.png)

**GUI Fuzztag**

![image-20220106165619622](./imgs/image-20220106165619622.png)

---
### 2.2 Comprehensive PoC / Exploit Superset

During real-world engagements, identifying a target running frameworks like WebLogic or Struts2 typically forces testers to switch between dozens of standalone scripts, OS environments, or conflicting Python/Java dependencies just to verify known CVEs. Yakit consolidates this into a **one-stop detection workflow**: enter the target URL/IP, select the module or tag, and click **Start Scan**.

![image-20220106162908302](./imgs/image-20220106162908302.png)

Yakit natively embeds the **Nuclei** engine (MIT License) inside Yaklang while allowing security researchers to author custom `.yak` or `.yaml` checks in the Plugin System and pin them directly to the sidebar menu.
---

### 2.3 Next-Generation Full-Stack Plugin System

A modern security workbench thrives on extensibility. Traditionally, building a GUI-enabled security plugin requires both frontend (HTML/JS/CSS) and backend expertise. In Yakit, **100% of plugin logic and interactive UI bindings are written directly in Yaklang** using `yakit` UI helper libraries—eliminating frontend boilerplate so security researchers can publish visual tools in minutes.

* [Plugin Architecture Concepts](https://www.yaklang.io/products/professional/yak-script-system)
* [Plugin Authoring Guide](https://www.yaklang.io/products/professional/yakit-plugin-how-to)

![img](./imgs/image-20220106162406945.png)
---

### 2.4 Decoupled Teamserver & Intranet Bridge Design

Because the core `yak` engine and the `Yakit` desktop GUI are completely decoupled, Yakit supports flexible deployment topologies:

1. **Local Mode**: Automatically spawns a local `yak grpc` engine on a random port out of the box.
2. **Remote Mode**: Run `yak grpc` on any platform or network boundary, including:

* Cloud VPS / ECS instances
* Dedicated testing workstations
* Internal segmented lab hosts

1. **Bridge Mode**: Maps internal network engines back to a public relay via reverse tunneling—allowing operators to perform intranet lateral movement directly from the Yakit GUI without manually configuring external port forwarders like `frp`.

![image-20220106203601893](./imgs/image-20220106203601893.png)
---

### 2.5 Native Java Deserialization Support (JDK-Free)

Testing Java deserialization vulnerabilities traditionally relies on tools like [ysoserial](https://github.com/frohoff/ysoserial), which require installing specific legacy JDK versions and managing JVM flags. **Yaklang implements the Java Object Serialization protocol natively in Go/Yak**, allowing testers to generate gadget chains, inspect serialized streams, and mutate Java objects directly from `.yak` scripts with **zero Java/JDK dependencies**.

![image-20220106204618700](./imgs/image-20220106204618700.png)
---

### 3. Keep Both English READMEs Safe During Auto-Sync

In `.github/workflows/sync-upstream.yml`, update **Step 3** and **Step 4** slightly so upstream merges never overwrite your English `README.md`, `README-EN.md`, or `README_LEGACY.md`:

```yaml
      # Inside Step 3 (Backup before git merge):
      cp README_EN.md /tmp/backup/README_EN.md 2>/dev/null || true
      cp README_LEGACY.md /tmp/backup/README_LEGACY.md 2>/dev/null || true

      # Inside Step 4 (Restore after git merge):
      if [ -f "/tmp/backup/README_EN.md" ]; then
        cp /tmp/backup/README_EN.md README_EN.md
        cp /tmp/backup/README_EN.md README.md
        cp /tmp/backup/README_EN.md README-EN.md
      fi
      if [ -f "/tmp/backup/README_LEGACY.md" ]; then
        cp /tmp/backup/README_LEGACY.md README_LEGACY.md
      fi

```

---

