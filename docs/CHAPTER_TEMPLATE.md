# Standard Chapter Template

Every chapter should follow the same skeleton so the book feels cohesive. Use the headings below in the exact order. Each section must exist even if you insert a short paragraph or `TODO` note.

1. **Motivation and Learning Objectives**  
   - Two short paragraphs explaining why the chapter matters and what readers will learn.  
   - Introduce at least one concrete outcome or decision point.

2. **Core Concepts**  
   - At least three subsections that break the topic into digestible areas.  
   - Each subsection can include narrative, lists, tables, or diagrams.  
   - If no diagram/table exists yet, add a `TODO` placeholder referencing what is planned.

3. **Anti-patterns and Common Mistakes**  
   - Provide a table or list of 3–5 pitfalls with consequences and alternatives.  
   - Keep the tone practical (focus on what engineers should stop doing).

4. **Worked Example**  
   - Point to runnable example code, diagram, or pseudo-code.  
   - Mention how to execute it locally and in CI.  
   - Summarize the automation insight it illustrates.

5. **Checklist**  
   - 4–6 actionable bullets that the reader can verify in their workflow.  
   - Items should connect to the worked example or core concepts.

6. **Exercises**  
   - Provide 6–10 activities (pair/practical/analysis) that reinforce the chapter.  
   - Indicate expected outputs or verification steps wherever possible.

7. **Summary**  
   - Recap the key lessons (2–3 bullets or sentences) tied to the chapter promise.

8. **What's Next**  
   - Provide one to two sentences pointing to the following chapter or action.

9. **Further Reading**  
   - List 2–3 curated resources (books, articles, docs) relevant to the chapter.

Use consistent terminology (e.g., “tests,” “pyramids,” “feedback loops”) across chapters. Wrap all code references in `\texttt{}` and escape underscores as `\_`. Include diagrams via `\lstinputlisting` when available, otherwise include a TODO placeholder referencing the desired visual.
