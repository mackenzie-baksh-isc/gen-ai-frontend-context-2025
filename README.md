# Gen AI Frontend Context 2025

This repository contains **context files** and **rules** designed to work with [Cursor](https://cursor.com) and similar LLM development platforms.  
These files help structure, generate, and execute tasks in a repeatable, controlled workflow.  

---

## 📂 Folder Overview

### `.cursor/rules`

- Contains **rule files** that can be:
  - Manually applied by referencing them in Cursor chat (`@rule-file`)  
  - Set to **Always Apply** when viewing a rule file directly inside Cursor  
- Rules are portable and can be copied to other platforms that support rule-based contexts  

---

### `.context`

The `.context` folder defines **modular workflows** for product generation, repo bootstrapping, task execution, and more.

#### 🔹 **gen-*** (Generate)
Used to **create new things that do not exist** yet.

- **`gen-prd`** → Collects and generates **product requirements** can also tell the llm to "assume the best".  
  - Output feeds into `gen-tasks`.  
- **`gen-tasks`** → Creates a list of **parent tasks** with **sub-tasks** based on the product requirements.  
- **`gen-repo`** → Customized to **initialize a new project repo** to your liking.  

#### 🔹 **exec-*** (Execute)
Used to **act or execute** on previously generated files in a controlled workflow.

- **`exec-tasks`** → Executes tasks defined in the `gen-tasks` output.  
  - Operates with structured, parent/sub-task tracking.  
  - Commit approval is requested **after each parent task is complete**.  
  - Example: reference both `@exec-tasks` and `@tasks-file`.  
- **`exec-process-sample`** → Frontend-specific.  
  - Generates a **JSON design profile + page layout** for a given **sample/screenshot**.  
  - Produces clearer, structured output vs. using screenshots alone.  
  - Great for **multimodal platforms** (e.g. [V0](https://v0.dev)).  
- **`exec-code-format`** → Executes **code formatting rules** on a repo or directory.  
  - Acts as a **sanity check** if you don’t want to use the `code-format` rule under `.cursor/rules`.  
  - Can also be referenced manually like other rules.  

#### 🔹 **add-*** (Add)
Used for **redundant but dynamic implementations** that are frequently needed across projects.

- Example: **`add-rtk-slice`**  
  - Provides a reusable Redux Toolkit slice template.  
  - Dynamic parameters (e.g. `url`, `method: GET | POST`, `params`) can be adjusted per request.  
- Other `add-*` files follow the same pattern: small, modular, dynamic snippets for rapid reuse.  

---

## 🧭 Usage Philosophy

- **`gen-*`** → Create new structured assets (requirements, tasks, repos).  
- **`exec-*`** → Execute and manage tasks against generated files in a **controlled process**.  
- **`add-*`** → Provide **dynamic but reusable** building blocks across multiple projects.  

> Everything here is designed to be **customizable and improvable** over time.  
> Adapt the contexts to your workflow, repo structure, or team needs.  

---

## 🚀 Getting Started

1. Clone this repo.  
2. Open it in Cursor (or a compatible LLM dev platform).  
3. Reference context files in chat with `@filename`.  
   - Example: `@gen-prd` → Generate product requirements.  
   - Example: `@exec-tasks @tasks-file` → Execute tasks from a task list.  
4. Optionally set **Always Apply** on rules you want active by default.  

---

## 📌 Notes

- Rules in `.cursor/rules` can be **ported** to other AI dev environments that support context rules.  
- The `.context` workflows are **modular** — you can mix and match `gen`, `exec`, and `add` contexts depending on the project.  
- Everything is **extensible** — feel free to improve or customize as your projects evolve.  
