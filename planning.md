# TakeMeter — planning.md

> Write this document before you write any pipeline code.
> Your spec and architecture diagram are what you'll use to direct AI tools (Claude, Copilot, etc.) to generate your implementation — the more specific they are, the more useful the generated code will be.
> Update the Retrieval Approach and Chunking Strategy sections if you change your approach during implementation.
> Update this file before starting any stretch features.

---

## Questions

### Community
<!-- What community did you choose and why?
     Why is this community a good fit for a classification task — what makes the discourse varied enough to be interesting? -->
Response

---

### Labels
<!-- What are your 2–4 labels?
     Define each in a complete sentence.
     Include 2 example posts per label. -->
Response

---

### Hard Edge Cases
<!-- What type of post will be genuinely ambiguous between two labels?
     How will you handle it when you encounter it during annotation? -->
Response

---

### Data Collection Plan
<!-- Where will you collect examples?
     How many per label?
     What will you do if a label is underrepresented after 200 examples? -->
Response

---

### Evaluation Metrics
<!-- Which metrics will you use to evaluate your model and why are those the right ones for this specific task?
     (Accuracy alone is not enough — explain what else you need and why.) -->
Response

---

### Definition of Success
<!-- What performance would make this classifier genuinely useful?
     What would you accept as "good enough" for deployment in a real community tool? -->
Response

---

## AI Tool Plan

### Label Stress-Testing
<!-- Give the AI your label definitions and edge case description, 
     and ask it to generate 5–10 posts that sit at the boundary between two labels.
     If it produces posts you can't classify cleanly, your definitions need tightening — do that now, before you annotate 200 examples. -->
Response

---

### Annotation Assistance
<!-- Decide whether you'll use an LLM to pre-label a batch of examples before reviewing them yourself.
     If yes, note which tool you'll use and how you'll track which examples were pre-labeled (for disclosure in your AI usage section). -->
Response

---

### Failure Analysis
<!-- Plan to give your list of wrong predictions to an AI tool and ask it to identify patterns before you write up your evaluation.
     Note what you'll look for and how you'll verify the patterns yourself. -->
Response

---
