# 🐫 IITM OCaml Reading Group

Welcome to the **OCaml Reading Group** at **IIT Madras**  discussion-oriented space for anyone curious about **functional programming**, **type systems**, and **OCaml**.

We are reading through _[Real World OCaml](https://dev.realworldocaml.org/)_, covering one or two chapters every week. Prior experience with OCaml or functional programming is **not required**. This is a space for **collective exploration**, where we learn together through code, examples, discussions, and occasional deep dives into PL concepts.

> 📅 We meet weekly (tentatively Saturdays) — see the schedule below.

## 📚 Schedule

| Date           | Discussion Starter | Chapter(s) | Title                                                |
| -------------- | ------------------ | ---------- | ---------------------------------------------------- |
| August 16th    | TBD                | 1          | A Guided Tour                                        |
| August 23rd    | TBD                | 2–3        | Variables, Functions, Lists, and Patterns            |
| August 30th    | TBD                | 4–5        | Files, Modules, Programs, and Records                |
| September 6th  | TBD                | 6–8        | Variants, Error Handling, and Imperative Programming |
| September 13th | TBD                | 9–11       | GADTs, Functors, and First-Class Modules             |
| September 20th | TBD                | 12–13      | Objects and Classes                                  |
| September 27th | TBD                | 14–15      | Maps, Hash Tables, and Command-Line Parsing          |
| October 4th    | TBD                | 16–17      | Async Programming and Testing                        |
| October 11th   | TBD                | 18–20      | JSON, Parsing, and S-Expressions                     |
| October 18th   | TBD                | 21         | The OCaml Platform                                   |
| October 25th   | TBD                | 22–24      | FFI, Memory Representation, and Garbage Collection   |
| November 1st   | TBD                | 25–26      | Compiler Frontend and Backend                        |

## 🧪 Getting Started — OCaml in JupyterLab

We’ll begin with syntax and interactive experimentation using **JupyterLab**, which includes OCaml.

### ✅ Prerequisites
- [Docker](https://www.docker.com/get-started) installed on your system

### 🔧 Quick Start

Clone the base environment:

``` bash
git clone https://github.com/prismlab/iitm_ocaml_reading_group
cd iitm_ocaml_reading_group/chapters
docker run -it -p 8888:8888 -v "$(pwd)":/chapters durwasa/ocaml_reading_group
### Open the link that appears in your terminal (http://127.0.0.1:8888) to access the environment.

```

Our JupyterLab environment is adapted from the [CS3100 Docker configuration](https://github.com/kayceesrk/cs3100_m25/tree/main/_docker) developed by Prof. KC Sivaramakrishnan.
This image comes preconfigured with:
 - OCaml kernel for interactive notebooks
 - RISE (Presentation mode for slides inside notebooks)
 

All .ipynb notes taken during sessions will be saved locally. We encourage participants to keep notes collaboratively and dive deeper into any area that sparks curiosity.

🛠️ Future Plans

We aim to gradually:
- Setup an OCaml IDE/testbed (e.g., VSCode with Merlin or Emacs)
- Explore projects, tooling, and build systems (Dune)
- Learn how to write tests, use libraries, and understand the OCaml ecosystem
- Integrate with package managers, version control, and more

🗣️ Join the Discussion

We use Discord for asynchronous discussion, help, and coordination:

[Join Discord](https://discord.gg/hDZSGVMNgV)


✏️ Contributing

If you want to help facilitate a session, add notes, or improve the environment setup, feel free to open a PR or reach out on Discord!
