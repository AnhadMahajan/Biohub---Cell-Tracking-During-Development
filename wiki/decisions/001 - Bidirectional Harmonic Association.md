---
type: decision
date: 2026-08-26
title: "ADR 001: Bidirectional Harmonic Probability Association Fusion"
status: accepted
tags:
  - decision
  - adr
  - association
ai-first: true
---

## For future agent
Record of the architectural decision to replace arithmetic forward edge probabilities with bidirectional harmonic mean probability fusion ($w=0.30$) across adjacent frames, unlocking the 0.927 milestone.

---

## Context & Rationale
Forward-only edge prediction is vulnerable to spurious forward links when a cell moves rapidly near distractors. Reverse-time association ($t+1 \to t$) acts as an independent validator: true biological transitions exhibit high mutual probability in both time directions.
