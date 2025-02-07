# 99_bottles_notes

## Introduction

These notes are my personal learnings and insights from studying "99 Bottles of OOP" by Sandi Metz, Katrina Owen, and TJ Stankus. While I aim to capture key concepts and code examples, these notes are not a substitute for reading the actual book. It's not explicit who wrote what parts but I attribute most to Sandi in the book purely out of convience.

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
- When extracting a class during refactoring:
   - TDD isn't always necessary if following established refactoring recipes (like Fowler's Extract Class)
   - Existing unit tests on the original class become integration tests for the new extracted class
   - This shows how the line between unit/integration tests is fluid and context-dependent
- Just executing the code and throwing it's value out can be a good way to test the code incrementally

## Chapter 6 - Achieving Openness

Key takeaways:
- Before adding new features, assess if code is truly open to modification
- Code still not open enough after previous refactorings
- Three main refactoring passes needed to achieve openness:
  1. Fix Data Clump smell first as it's the easiest to spot and fix
  2. Fix Repeated Switches using Replace Conditionals with Polymorphism
  3. Fix Liskov Substitution Principle problem
- Data Clump smell occurs when groups of data items appear together repeatedly
- Replace Conditionals with Polymorphism
  - This was chosen by the author because she had insight into the results of the refactoring although she leaves it to the reader to attempt fixing it with replace conditional with State/Strategy
  - Factory method is introduced
  - Small steps are taken and tests are run after each refactoring
  - Facotry method starts as a method on in the calling class but later is moved to the BottleNumber class while fixing the liskov substitution principle problem
- Fixing Liskov Substitution Principle problem
  - The book doesn't highlight the code smell of Liskov Substitution Principle problem, but the Liskov Substitution Principle is part of the SOLID principles
  - I think the code smell of primitive obsession could also apply, since the code otherwise has to manage the returned primitive type and create it itself
  - The factory method needed to be moved to the BottleNumber class, so the bottlenumber access it
  - It was interesting the method used to temporarily take mulitple types for it's parameter so the code wasn't broken at any point
- Making the Easy Change
  - TDD is resumed (for many chapters no tests were added or changed)
  - Only change needed for the new requirement was to modify the test that tested all the verses together to test the new 6-pack variant
    - Kind of only a data change since the outside API didn't change
  - Tests were in a failing state for a while, but the author didn't mention it
    - I think Sandi could of instead reverted after the failing tests were generated, and add the following under green tests to be really safe
      - Add the new BottleNumber6 (empty for now)
      - Add the BottleNumber6 to the factory method (shouldn't break any tests)
      - Re-add the test changes, but only the quantity change to 1 for verse 7 and 6
      - Add the new new quantity to the BottleNumber6
      - Re-add the test change for six-pack
      - Add the new container method to the BottleNumber6
  - I thought we were going to override the toString method before I saw the implementation, cool that this is addressed later
    - Great quote from this "Clever shortcuts are a false economy. Invest in code that tells the truth. Just write it down."

## Chapter 7 - Manufacturing Intelligence

Key takeaways:
- Factory method ends up holding the conditional logic that was extracted from the shameless green implementation
- Factory methods with switch statements still violate Open/Closed Principle since they require modifying existing code - alternative approaches exist to make factories more open
- First alternative is using meta programming in the factory method
  - Downside to this is it's hard to search for the class name in the method
  - It's usually hard to understand the code at a glance
  - It usually has to use exception handling to handle unexpected values
- Second alternative is using a hash to map numbers to bottles
  - This is more readable and easier to search for
  - It's easier to understand the code at a glance compared to the meta programming, but not as easy as the switch method
  - It doesn't have to use exception handling to handle unexpected values
- Third alternative is classes returning if they handle the case
  - This is a weird in the middle solution I think, it still needs a list and the order of the list is important
  - Each class just kinda volunteers to handle the case, but it's a first to respond pattern
- Fourth alternative is using a registry
  - More indirection is necessary but very open to change
  - Distributed across the codebase the class registries, so finding all classes created by a factory method is harder
- ** Each factory has trade-offs it's up to the developer to choose the best option based on the trade-offs **

## Chapter 8 - Developing a Programming Aesthetic

Key takeaways:
- Chapter covers more vague and less formal refactoring recipes
- Commented out pseudo code is used to show what the code could look like if a more general request for similar songs was asked this helps to direct the coder that a new class should be extracted. This is a good example of how to avoid making a hacky solution that you then later have to refactor.
- Dependency Inversion Principle is covered, as I guess it's another aethetic choice that isn't exactly blocking any concrete requirements yet
  - Dependency is injected, and Sandi also shows how the law of demeter violation can be fixed by create a new static method on the class
- Law of Demeter
  - Seperate OOP guidance principles from SOLID principles
  - It's covered at great length in the book with diagrams and examples
  - Eastetically better because it reduces coupling between too many classes (otherwise classes end up knowning about not just their collaborators but also their collaborators collaborators, etc)
  - From wikipedia: creators named it after Demeter because it was suposed to be a tool to lead to growing software vs building
  - Also helps make testing a class easier, if you're having to mock many other classes to satisfy the collaborators then you're probably violating the law of demeter
- Pushing object creation to the edge
  - Code smell of The Blank Line ™ tells you you're violating the single responsibility principle
  - Creates the BottleNumber class in the static method that was created earlier in the chapter to avoid the Law of Demeter violation
  - Reduces the lyrics method to a single responsibility, but also methods that the static method it was moved into could be broken into if it was too complex


## Chapter 9 - Reaping the Benefits of Design

Key takeaways:
- Integration tests alone are not helpful to future developers, try to write unit tests for each class so developers don't have to only rely on integration tests that break far from their code
- Every class should normally have a unit test, but if the tests you'd have to write for them merly repeat their implementation then they're not adding value
  - This case is not normal and you should be able to validate and explain the reason why the test is left out
  - In the books example the class is only used by the BottleVerse class, so it defers to the BottleVerse tests to test of of BottleNumber's behavior
  - All code has a cost, and tests should also be evaluated
- BottleVerse's references to BottleNumber is private as it's not exposed as part of the public interface, Bottle class's reference to BottleVerse is public since it's passed to the constructor
  - Sandi mentions that it's default doesn't constitute a private relationship, and that the book will later change that code
- While building the unit test for BottleVerse, the integration tests that tested the same thing were slowly removed after they were copy and converted under green tests
- Great Chapter!
- Tests tell the story of the classes, if the tests aren't truthful in the story then you can mislead future developers
- Tests don't have to follow the same rules as the production code, exceptions made in having the pattern name in the class name for instance
- xUnit Test Patterns book breaks down the different types of test doubles
- Try to reduce noise in the tests, if they have too much concreate details that are like your production code it can be hard what the test is actually testing
  - Fakes are a good way to reduce noise
- The first test is usually the hardest to write
- objects that require a large, convoluted context are picky about their surroundings, they know too much about the outside world. They are then hard to reuse in different contexts
- Revists the 3 phase structure of tests
  1. defines an expectation
  2. executes some code
  3. asserts that the result matches the expectation
  - Or AAA (Arrange, Act, Assert)
- Team should agree on the style of tests to keep them consistent and easy to understand
- Code Signals vs. Comments
  - Jim Weirich used: do..end for side-effects, {..} for no side effects
  - More readable than comments, part of code style and team's conventions
- Roleplaying testing, reminds me of POODR book having examples of this too, but not needed for a typed language
- Cleaning up defaults when they're not needed or if they can demonstrate or signal to the programmer something


## Overall Observations and Key Insights

- Sandi Metz stesses the importance of SOLID principles to make code more open to modification. At the highest level if you software is likely to change often these are the primary principles to focus on. Otherwise if your software doesn't have a future, shameless green is the way to go. But shameless green is also the best way to start, so you're not choosing patterns too early.
- Reading the Javascript version of the book
- Dynamic typed languages like Javascript relies on explicit trust in the implicit contracts between objects
- There's an implied ranking of the SOLID principles. Here's what I think the order is:
  - Open/Closed - The most important principle
  - Liskov Substitution - The second most important principle
  - Single Responsibility - The third most important principle
  - Dependency Inversion - The fourth most important principle
  - Interface Segregation - The fifth most important principle and not relevant for dynamic typed languages
- New code smell - Empty line (usually in a method it shows changing the subject)
  - Is usually a clue that you're violating the single responsibility principle
  - Looking up the code small Long Function is a good way to find the next refactoring
  