# Project background

This project was provided by **Reply** for the **Machine Learning** group project at **LUISS Guido Carli**, academic year **2025/2026**.

**Student team:** Michele Turco, Mattia Sebastiani, Sofia Bruni.

## Project Context and Institutional Setting

The project originates from a **data-quality scenario inspired by NoiPA**, the digital platform of the Italian Ministry of Economy and Finance that manages administrative and payroll-related data for employees of the Italian Public Administration. In this setting, **data** may arrive from **different sources** and in **different formats**, such as CSV files, JSON exports, or database extracts. Even when the information is present, it may **not be immediately reliable** for analysis or downstream processing, because the same concept can be encoded in inconsistent ways across rows, columns, or files.

This kind of context is particularly suitable for a data-quality project because the **main difficulty** is not the lack of data alone, but the **gap** between **availability and usability**. A dataset may look populated while still being difficult to trust. Dates can appear in several incompatible formats within the same column. Columns may contain numeric values mixed with textual decorations. Placeholder tokens may hide missingness behind apparently non-null strings. Distinct columns may duplicate one another semantically or contradict one another logically. If these issues are not isolated carefully, **later analysis inherits uncertainty** that is often **invisible at first sight**.

## Problem Statement

The problem addressed by the project is therefore broader than simple data cleaning. The **task** is to **design a system** that can receive a raw dataset, inspect it systematically, **understand which quality issues are actually present**, decide which **actions are safe to perform automatically**, **generate constrained transformations** when normalization is justified, and **verify that the transformations** improved the data instead of damaging it.

This **distinction** is essential. A **generic instruction** such as "clean this CSV" can easily **produce outputs** that look **plausible but are difficult to justify**. It may become unclear which evidence supported a change, whether valid values were accidentally rewritten, whether the transformation was appropriate for the semantic meaning of the column, and whether the resulting dataset is genuinely better than the original one. For a project that aims to be auditable and reliable, **this level of opacity is not acceptable**.

## Objective and contribution

The workflow produces a cleaned CSV and a quality report explaining detected issues, selected actions, and verification outcomes. Its contribution is the division of responsibility between deterministic profiling, typed agent decisions, generated cleaning functions, and local validation.

Python measures parse rates, missingness, shapes, duplicates, and anomalies. Agents interpret bounded profiles, infer target types, propose normalization functions, and summarize findings. Profiles can contain actual sampled cell values; they are not anonymized merely because they are bounded. Generated cleaners are checked against preservation and output requirements before application, and the resulting dataset is verified afterward.

This design supports inspection of intermediate evidence and decisions. It does not establish that every model interpretation is correct or that ambiguous business rules can be inferred automatically.

---

[Documentation index](README.md) · [Run the application](../README.md#setup)
