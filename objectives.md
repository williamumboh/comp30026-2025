# Learning Objectives

## 1. Understand models of computation
- Restricted models
  - Finite automata and regular expressions
  - Pushdown automata and context-free grammars
- General model: Turing machines

<!-- ## 2. Analyse computation -->
<!-- - Determine what an algorithm computes from its description -->
<!-- - Determine whether an algorithm correctly solves a given problem -->

<!-- Abstract version of what they've done for COMP10001 -->

## 2. Design algorithms
- Restricted computational models with constraints on input access and memory[^ilo]
- Reductions: Given a problem A that you want to solve, and Algorithm B that solves a different problem B, leverage Algorithm B to solve Problem A[^hammer]
   - Will job X be replaced by LLMs? Yes, if we can reduce job X to predicting the next token.
   - DRY principle in software engineering

[^ilo]: This corresponds to the ILO in the subject handbook Design abstract computational devices, such as finite-state automata and pushdown automata, which I find too abstract and boring.
[^hammer]: When all you have is a hammer, everything looks like a nail.

## 3. Understand and reason rigorously about the limits of computation
- Explain which problems cannot be solved by each of these computational models
- Give a rigorous argument for why a given computational problem cannot be solved by finite automata (Turing machine, resp.) using fooling set technique (reductions, resp.)

## Generic skill: Creative problem solving and ability to break down difficult tasks
