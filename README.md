# 99_bottles_notes

## Introduction

These notes are my personal learnings and insights from studying "99 Bottles of OOP" by Sandi Metz. While I aim to capture key concepts and code examples, these notes are not a substitute for reading the actual book.

I highly recommend purchasing and reading the book yourself to get the full depth of knowledge and understanding that Sandi provides. The book contains invaluable lessons about object-oriented design, refactoring, and writing maintainable code that are best absorbed through careful study of the complete text.

You can get the book at: https://sandimetz.com/99bottles

## Chapter 1-4 Notes Coming Soon...


## Chapter 5 - Separating Responsibilities

Key takeaways:
- Revisiting the original requirement to add "6-pack" variant to the song
- Current code is not yet open to this change - no single point of modification exists
- Need to identify and fix code smells first to make the code more flexible
- This illustrates how initial designs often need refactoring before new features can be cleanly added
- Looking for the next code smell to guide refactoring direction
- Using equality checks (`===`) in conditionals is preferable to range checks (`>`, `<`) as it:
  - Narrows the scope of what matches the condition
  - Makes the code's intent clearer
  - Reduces chance of edge cases/bugs from unexpected values in ranges
- When starting with a shameless green implementation, duplication is often the first and most pressing code smell to address before diving into deeper patterns
- Once initial duplication is handled, the following questions help identify deeper patterns and potential abstractions:
  1. Do any methods have the same shape?
  2. Do any methods take an argument of the same name?
  3. Do arguments of the same name always mean the same thing?
  4. If you were going to break this class into tow pieces, where's the dividing line?
  5. Do the tests in the conditionals have anything in common?
  6. How many branches do the conditionals have?
  7. Do the methods contain any code other that the conditional?
  8. Does each method depend more on the argument that got passed, or on the class as a whole?
