# Flutter mdbook Style Guide & Template

This document defines the consistent structure and formatting standards for all chapters in the Flutter MVVM learning path.

---

## Chapter Types & Structure

### Type A: Procedural Chapters (Ch 1-2)
Used for setup, project initialization, and step-by-step guides.

**Structure:**
```markdown
# Chapter Title

## [Section Name]
1. [Step]
2. [Step]
3. [Step]

## [Section Name]
[Content]

## References
- [Link](url)
```

**Rules:**
- No "Theory," "Practice," or "Solution" sections
- Numbered steps for clarity
- Include screenshots/images where helpful
- Can be flexible in organization

---

### Type B: Concept Chapters (Ch 3-10)
Used for teaching Flutter concepts, widgets, architecture patterns, and building features.

**Required Structure:**
```markdown
# Chapter Title

## Learning Outcome
Brief statement of what students will understand/build by chapter end.
Use 1-2 sentences. Example:
  "By the end of this chapter, you'll create custom widgets and understand how to compose 
   them into more complex layouts using Row and Column."

## Theory / Explanation
Conceptual content explaining the topic. Include:
- High-level concepts
- Key principles
- Examples (code snippets, diagrams)
- References to Flutter documentation when relevant

## Practice
Numbered exercises guiding students through hands-on implementation. Each exercise has its own hidden solution block.

**Structure for each exercise:**
```markdown
1. [Exercise description and instructions]
   - Include any helper code or hints
   - Can use >[!TIP] blockquotes for guidance

<details>
<summary>Solution 1</summary>

[Code and explanation for this exercise]

</details>

2. [Next exercise description and instructions]

<details>
<summary>Solution 2</summary>

[Code and explanation for this exercise]

</details>
```

**Rules for Practice:**
- Provide specific, actionable instructions (NOT vague "try yourself" prompts)
- Number each exercise (1., 2., 3., etc.)
- Include TIPs or helpful hints with `>[!TIP]` blockquotes
- Be detailed enough that students know what to do without guessing
- Can include file paths, folder structures, or code references
- **Each exercise must have its own collapsible Solution block** immediately after the exercise description

**Rules for Solution blocks within Practice:**
- Use collapsible `<details>` HTML tags
- Label as `<summary>Solution 1</summary>`, `<summary>Solution 2</summary>`, etc.
- Include file paths in code comments: `// lib/path/to/file.dart`
- Show folder structure for multi-file exercises (ASCII diagram)
- For chapters 3-5: Include complete, working code
- For chapters 6-10: Include placeholder code blocks with TODO comments ready for author to fill in
- Students can reveal solutions one at a time without spoiling later exercises

## Recap
Summary of key achievements (bullet list).

**Rules for Recap:**
- 3-5 bullet points
- Summarize what students built and learned
- Link to glossary terms where applicable: [ChangeNotifier](#glossary)
- Example:
  - ✓ Created a custom `FilmCard` StatefulWidget
  - ✓ Learned how to toggle state with `setState()`
  - ✓ Practiced conditional rendering based on state

## Next Steps
Brief bridge to next chapter.

**Rules for Next Steps:**
- 1-2 sentences
- Reference the next chapter title
- Set context for why the next chapter matters
- Example:
  "Now that you understand how widgets render and compose, we'll learn how to manage 
   data across your app using the Model layer and the Ghibli API."
```

---

## Global Formatting Standards

### Headings
- **H1 (#)**: Chapter title only
- **H2 (##)**: Main sections (Learning Outcome, Theory, Practice, Solution, Recap, Next Steps)
- **H3 (###)**: Subsections within sections (used sparingly)
- **H4 (####)**: File path headers in solutions, subsections of subsections

**Examples:**
```markdown
# Stateless Widgets  ← H1: Chapter title

## Learning Outcome  ← H2: Main section

## Theory  ← H2: Main section

### Widget Structure  ← H3: Subsection (if needed)

## Practice  ← H2: Main section

## Solution  ← H2: Main section

#### lib/views/film_title.dart  ← H4: File path in solution
```

### Code Blocks

**Format:**
- Always include language identifier: ` ```dart `, ` ```json `, etc.
- Include file path in first line as comment:
  ```dart
  // lib/models/film_model.dart
  class Film {
    // ...
  }
  ```

**Multi-file solutions:**
1. Show folder structure first (ASCII tree):
   ```
   lib/
   ├── models/
   │   └── film_model.dart
   ├── views/
   │   └── films/
   │       ├── films_view.dart
   │       └── widgets/
   │           ├── film_card.dart
   │           └── film_title.dart
   └── main.dart
   ```

2. Then provide each file with clear heading:
   ```markdown
   #### lib/models/film_model.dart
   \`\`\`dart
   // code
   \`\`\`

   #### lib/views/films/films_view.dart
   \`\`\`dart
   // code
   \`\`\`
   ```

### Images

**Alt Text:**
Replace all "alt text" with descriptive captions.

**Format:**
```markdown
![Expected FilmTitle widget with red border](image-5.png)
```

**Naming:**
- Use descriptive, specific text describing what students should see
- Example: NOT `![alt text](image-5.png)`, but `![Red-bordered movie title widget](image-5.png)`

### Scaffolding Elements

#### Tips (>[!TIP])
Use for helpful guidance, IDE tricks, or common gotchas.

**Rules:**
- 2-3 TIPs per chapter (Ch 3-10)
- Use markdown syntax: `>[!TIP]`
- Keep tips concise (1-3 sentences)

**Example:**
```markdown
>[!TIP]
>You can use the IDE refactoring tool to wrap widgets automatically.
>Right-click on any widget > select **"Wrap with Container"**.
```

#### Verification Checklist
Use after Practice section to help students verify their work.

**Format:**
```markdown
### Verification Checklist
Before moving to the Solution, verify that:
- [ ] You've created the `lib/models/film_model.dart` file
- [ ] The Film class has all required properties
- [ ] `Film.fromJson()` factory method is defined
- [ ] No compile errors in your project
```

#### Glossary Links
Link to glossary terms when introducing technical vocabulary.

**Format:**
```markdown
A [**ChangeNotifier**](#glossary) is a class from Flutter that...
```

### Internal Notes & Comments

**DO NOT** include internal editor notes in rendered markdown.

**Examples of what to REMOVE:**
- `#@todo, hide by default`
- `#@todo, should instantiate service in ViewModel`
- `/* Lines 91-122 omitted */`

**If you need to make notes:**
- Create a separate `.notes.md` file for each chapter (not rendered)
- Or use comments in code blocks only

---

## Placeholder Code Blocks (Chapters 6-10)

For Solution sections in Chapters 6-10, provide **skeleton code** with TODO comments for the author to complete.

**Guidelines:**
- Show the correct class structure and method signatures
- Include comments indicating what code should be added
- Maintain proper Dart syntax (don't leave incomplete syntax)
- Use `// TODO:` comments to mark sections needing implementation

**Example for Chapter 9 (ViewModel):**
```dart
// lib/view_models/films_view_model.dart
import 'package:flutter/foundation.dart';

class FilmsViewModel extends ChangeNotifier {
  List<Film> films = [];
  
  // TODO: Add loading and error state properties
  
  void fetchFilms() {
    // TODO: Call the FilmService to fetch films from the API
    // TODO: Handle errors gracefully
    // TODO: Call notifyListeners() to update the UI
  }
}
```

---

## Checklist for Consistent Chapters

Use this checklist when reviewing/creating chapters 3-10:

- [ ] **Structure**: Has all required sections in order
  - [ ] Learning Outcome
  - [ ] Theory
  - [ ] Practice
  - [ ] Solution
  - [ ] Recap
  - [ ] Next Steps

- [ ] **Headings**: Proper hierarchy
  - [ ] H1 only for chapter title
  - [ ] H2 for main sections
  - [ ] No H3+ unless necessary for subsections

- [ ] **Content Quality**
  - [ ] Practice steps are specific and actionable (not "try yourself")
  - [ ] Recap summarizes key achievements
  - [ ] Next Steps bridges to next chapter
  - [ ] Tone is friendly and encouraging

- [ ] **Formatting**
  - [ ] All image alt-text is descriptive
  - [ ] Code blocks include file paths
  - [ ] No internal `#@todo` comments in rendered content
  - [ ] 2-3 TIP blockquotes included

- [ ] **Solutions** (Ch 3-5 complete; Ch 6-10 placeholders)
  - [ ] Solution section is collapsible
  - [ ] File structure is clear
  - [ ] Code snippets are syntactically valid
  - [ ] Placeholder code has TODO comments (Ch 6-10)

- [ ] **Cross-References**
  - [ ] "Next Steps" in Chapter X introduces Chapter X+1 topic
  - [ ] Glossary links are used for technical terms
  - [ ] Links to Flutter documentation where relevant

- [ ] **No Artifacts**
  - [ ] No `#@todo` comments
  - [ ] No `/* Lines X-Y omitted */` placeholders
  - [ ] No vague "alt text" in images

---

## File Organization

```
flutter-lab-widget-to-layered-architecture/
├── GUIDELINES.md  ← This file
├── book.toml
├── src/
│   ├── SUMMARY.md
│   ├── introduction.md  (Ch 1)
│   ├── create_a_flutter_project.md  (Ch 2)
│   ├── widgets_introduction.md  (Ch 3)
│   ├── widgets_composition.md  (Ch 4)
│   ├── stateless_widgets.md  (Ch 5)
│   ├── class_model.md  (Ch 6)
│   ├── stateful_widgets.md  (Ch 7)
│   ├── mvvm_view.md  (Ch 8)
│   ├── mvvm_view_model.md  (Ch 9)
│   └── mvvm_model.md  (Ch 10)
└── book/  (generated HTML)
```

---

## Additional Notes

### Next Steps for Book Maintenance
1. Review all chapters against the "Checklist for Consistent Chapters"
2. Add Learning Outcome, Recap, and Next Steps to all chapters
3. Ensure Solution sections follow placeholder/complete guidelines
4. Verify all image alt-text is descriptive
5. Remove all internal `#@todo` comments
6. Consider adding a Glossary appendix defining key terms

### Future Enhancements
- Add Mermaid diagrams to MVVM chapters showing data flow
- Create a standalone Glossary chapter
- Add integration tests as a bonus appendix
- Create companion code repository with example solutions

---

**Last Updated:** 2026-09-09
