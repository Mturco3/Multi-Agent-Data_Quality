# Limitations and future work

## Observed Failure Modes

Several **concrete failure modes** emerged during development and shaped the final architecture. One recurring problem was the accidental damage of **already-valid values** by generic cleaning branches that matched broad string patterns before checking whether the input was already canonical. Another was the generation of values with the correct delimiter but the **wrong semantic order**, especially in date-like fields. Recoverable period encodings could also be dropped too aggressively if the logic treated partial information as unusable. At the code level, some generated cleaners failed because they were not truly **self-contained**. Finally, repeated local failure loops showed that generation quality does not automatically improve by simple repetition.

These failure modes are significant because they justify several safeguards that might otherwise appear overly cautious. The **early-exit preservation rule**, **host-side validation**, **repair-critic loop**, and **stagnation detector** all exist because specific classes of failure were encountered in practice, not because the architecture is trying to be cautious in the abstract.

## Limitations

The current system still has important limitations, many of which are **deliberate design constraints** rather than accidental gaps. Some intervention classes, especially **anomaly handling** and **near-duplicate row cases**, remain conservative and may require manual review rather than automated correction. The system is therefore **not** a universal autonomous cleaner for arbitrary datasets, nor is it intended to be interpreted as such.

Another limitation concerns scope. The system is optimized for **structured tabular validation** and **controlled normalization**, not for domain-complete semantic correction. If a value is syntactically valid but factually wrong in a way that requires external business knowledge, the current architecture may flag it as suspicious at best, but it will not necessarily be able to repair it safely.

A further limitation is that the pipeline **does not infer arbitrary logical relationships between columns**. Cross-column checks ([cross-column validation](validation.md#cross-column-validation-and-duplicate-detection)) apply explicit programmatic rules such as year-month-period consistency and date-order violations, but they do not capture domain-level logical constraints between arbitrary column pairs. An inconsistency that only becomes visible when the semantics of two columns are interpreted jointly - for example, a combination of category and amount that is internally contradictory - will not be detected unless a dedicated rule is defined.

Finally, the system has **limited effectiveness on free-text and general-purpose text columns**. The schema stage can classify a column as `free_text` and the anomaly stage can flag statistical outliers, but neither stage attempts to normalize or validate the content of narrative fields. Columns whose values are prose descriptions, names, or open-ended categorizations are deliberately excluded from most cleaning logic, because there is no stable canonical form against which to validate them.

## Future Work

Several natural extensions follow from the present implementation. Additional work could compare different stagnation-breaking strategies, alternative model choices, stronger duplicate-resolution policies, or richer verification criteria beyond format consistency alone.

From a broader engineering perspective, future implementations could also expand the system toward a more configurable policy layer in which different intervention tolerances can be selected depending on the dataset context. That would allow the same architecture to remain conservative in high-risk scenarios while being more permissive in exploratory settings. Another direction would be to integrate a more explicit **human-in-the-loop** component, in which the system surfaces findings and proposed repairs to a user interface for review and approval before application. That would make the pipeline more interactive and allow it to benefit from human judgment on ambiguous cases.

## Operational limitations and planned maintenance

- Generated cleaners execute locally with Python `exec`; behavioral validation does not provide process isolation.
- Bounded model prompts still contain sampled data, and optional traces can contain that evidence.
- The app and CLI use different orchestration paths. The app currently calls validation bundling after individual stages, causing anomaly, cross-column, and duplicate checks to run again.
- Current dataset path casing and upload storage need cleanup; see [environment and operation](architecture.md#environment-and-operation).
- Dependency auditing, locks, cross-platform verification, notebook retirement, and Docker support are planned maintenance work, not completed features.
- A hosted presentation viewer is deferred; the [PDF presentation](../ReplyProject.pdf) stays in the repository.

---

[Documentation index](README.md) · [Run the application](../README.md#setup)
