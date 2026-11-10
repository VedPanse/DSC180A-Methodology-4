---
layout: default
---

# Methodology Assignment 4

**Name:** Ved Panse  
**UCSD email:** [vpanse@ucsd.edu](mailto:vpanse@ucsd.edu)  
**Section:** D33 — Safe Agentic AI: Data-Centric Methods for Auditing, Aligning, and Hardening LLM Agents  
**Mentor:** Babak Salimi

**What is the most interesting topic covered in your domain this quarter?**

The most interesting topic for me has been how to give a safety monitor enough evidence to judge an agent's actions. A change can look harmless on its own but become a problem when another file uses it later. That was part of what drew me to DeepTrace and dependency graphs. I initially thought that supplying more of those connections would naturally make the monitor better. Our experiments made me less confident about that assumption.

In a small synthetic test, the monitor reported the current facts correctly but still made incorrect policy decisions. Selected history also didn't clearly outperform unselected history. That distinction matters to me because retrieving the right evidence and making the right judgment are separate problems. A graph could help with one without fixing the other. It has made me more interested in understanding where a monitoring pipeline fails before trying to improve its overall accuracy.

**Describe a potential investigation you would like to pursue for your Quarter 2 Project.**

I'd like to investigate whether separating evidence extraction from policy checking makes a monitor more reliable on a narrow set of repository changes. Instead of asking for one open-ended safety verdict, the monitor would identify specific facts and their supporting code locations. A separate checker would then apply an explicit rule, with an unknown outcome when the evidence is incomplete. This would be a related direction we could pursue if the history selection idea doesn't show a useful advantage.

I'd start with a small collection of reproducible Python examples involving command execution, including unsafe changes, harmless refactors and changes that restore a restriction. I'd compare the combined approach with a monitor that makes the decision directly, using the same evidence. False accusations on safe edits would matter as much as detecting unsafe ones. We already have a diagnostic result suggesting this separation is worth testing, but it needs new cases and independent review. I'd keep the claims limited to rules we can actually specify and check, rather than treating this as a general solution to agent sabotage.

**What is a potential change you'd make to the approach taken in your current Quarter 1 Project?**

I'd change how we choose evaluation cases before spending more time improving the selector. In the synthetic cases we've tested, the current facts already contain enough information to determine the answer. That makes them useful for checking whether history confuses a monitor, but they don't really test whether history helps locate evidence that would otherwise be difficult to find. I think we need to be clearer about which of those questions an experiment can answer.

For the next comparison, I'd look for a small number of verified repository cases where a relevant dependency is far from the proposed edit. Both methods should have access to the same current repository, with comparable search and context budgets. The baseline should be allowed to search properly. I'd also require a known starting state and complete action records so we can check what actually changed. If we can't obtain those records, I'd keep the evaluation narrow and describe that limitation instead of presenting partial traces as a realistic replay.

**What other techniques would you be interested in using in your project?**

I'm interested in combining program slicing with retrieval. Starting from an action such as a command execution, a backward slice could identify the assignments and conditions that affect it. That might give us a more focused set of dependencies than sending a large repository graph to the monitor. I'd want to compare this with something much simpler, like searching for the relevant function names and following their callers, before assuming the more complicated method is worthwhile.

Another technique I'd like to explore is generating small counterexamples for rules we can formalize. If we claim that an input restriction prevents an unsafe command, can we find an input that violates that claim? These examples could help us catch mistakes in our own analysis and explain a finding to someone reviewing it. Neither technique would cover arbitrary Python behavior, especially dynamic calls or external services. I'd explicitly mark unsupported cases as unknown and measure how often that happens, since a method that avoids every difficult case may look accurate while being of little practical use.
