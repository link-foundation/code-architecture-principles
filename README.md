# code-architecture-principles

Architecture principles for code design

## 🌍 Universal Principles (apply to any language or paradigm)

These are general software design principles that are useful in OOP, FP, and mixed styles.

### Structure & Modularity

- **Modularity**  
  Split the system into small parts (modules, files, components) so each part can be understood, changed, and tested separately.

- **Separation of Concerns**  
  Keep different kinds of work in different places (e.g., business rules vs. UI vs. database). Each part should focus on one “kind” of problem.

- **High Cohesion**  
  Things that belong together should live together. A module or file should do one focused job instead of many unrelated ones.

- **Low Coupling**  
  Parts of the system should know as little as possible about each other. Changing one part should not break others.

- **Composition Over Complexity**  
  Build larger behavior by combining simple pieces instead of writing huge, complex functions or classes.

- **Layers / Tiered Architecture**  
  Organize code into layers (e.g., UI, application/service, domain, infrastructure) where each layer has a clear role and only depends in one direction (usually from outer to inner).

- **Hexagonal / Ports and Adapters**  
  Put your core logic in the center and connect external things (databases, APIs, UI) via “ports” (interfaces) and “adapters” (implementations). This keeps your core logic independent of tools and frameworks.

- **Clean Architecture Dependency Rule**  
  Code that represents business rules should not depend on code that deals with frameworks, databases, UI, or external systems. The important logic is at the center; everything else points inward.

---

### Behavior, Interfaces & Abstraction

- **Abstraction**  
  Hide how something is done and show only what it does. Callers should not need to know internal details.

- **Stable Contracts**  
  Define clear, simple interfaces or function signatures and avoid changing them often. Change the inside, not the “shape” seen by other code.

- **Explicit Interfaces**  
  Make inputs, outputs, and effects explicit. Do not rely on invisible global state or hidden behavior.

- **Principle of Least Surprise**  
  Code should do what its name and interface suggest. It should not behave in surprising or tricky ways.

- **Clear Naming**  
  Use names that describe what the thing really does or represents. Avoid clever or vague names.

- **Extensible Public Surface**  
  Expose as much public functions, classes, or endpoints as possible. Everything "internal" should be pontentially be usable externanally to simplify extension of internal logic if some features are missing. Ideally there should be no irriplacable internal components, everything should be exposed publically and everything should be extensible or configurable or replacible.

- **Small Public Documentation**  
  Prefer documenting only parts of code that are frequencly used by users first. So only the smallest possible set of function, classes and endpoints of public interface are documented, that are mostly stable for a long time and used by users more frequently. Everything internal that can be potentinally misused dispite being public API can have no documentation if not yet stable or frequently used by users.

- **Intent-Revealing Interfaces**  
  Design APIs so it is obvious how to use them correctly and hard to use them incorrectly.

- **Protected Variations**  
  Put a clear boundary around things that might change (e.g., frameworks, external APIs, storage). Depend on abstractions instead of directly on volatile details.

---

### State & Data

- **Prefer Immutability**  
  Use values that do not change after they are created. If data must change, consider creating new values instead of modifying existing ones in place.

- **Controlled Mutability**  
  If you must mutate state, keep it local, well documented, and as small as possible. Avoid shared mutable state between many parts of the system.

- **Single Source of Truth**  
  Keep one clear place where a piece of important data is defined and owned to avoid conflicts and confusion.

- **Clear State Transitions**  
  Changes in state should be done through clear operations or functions, not by random field or variable mutations scattered everywhere.

- **Make Invalid States Impossible**  
  Design your data structures so that impossible or illegal combinations of values cannot be represented at all.

---

### Side Effects, I/O & Boundaries

- **Keep Side Effects at the Edges**  
  Reading/writing files, network calls, logs, and other side effects should be pushed to the boundary of the system. The core logic should be as free from side effects as possible.

- **Functional Core, Imperative Shell**  
  Put pure, calculation-only logic in the middle, and wrap it with a thin layer that performs side effects and orchestration.

- **Explicit Side Effects**  
  When a function or method reads external state, writes to a database, sends an email, etc., that should be obvious from its name, parameters, or documentation.

- **Idempotence**  
  For operations that might be retried (e.g. HTTP calls, jobs), design them so running them multiple times with the same input has the same result as running them once.

- **Stateless Processes (Where Possible)**  
  Prefer services and handlers that do not store long-term in-memory state between calls. Rely on databases and explicit parameters instead.

---

### Correctness, Types & Testing

- **Design for Testability**  
  Write code so it can be tested easily: small pieces, clear inputs and outputs, minimal hidden state, and replaceable dependencies.

- **Deterministic Core Logic**  
  Core calculations should return the same result given the same inputs, without depending on time, randomness, or external systems.

- **Use Types or Schemas to Model Reality**  
  Use type systems, JSON schemas, or similar mechanisms to describe valid shapes of data and catch errors early.

- **Fail Fast and Clearly**  
  When something goes wrong, fail early with clear error messages instead of hiding errors or continuing in a broken state.

- **Validation at Boundaries**  
  Validate input when it enters the system (e.g., HTTP request, queue message) so inner code can assume it is well-formed. So validation can done in a single place, all internal code should assume validation was already done, and can be simplified to not double check everything.

---

### Domain & Understanding

- **Ubiquitous Language**  
  Use the same language in code, documentation, and conversations as the business or problem domain uses. Method names and type names should reflect real-world terms.

- **Domain-Centered Design**  
  Design from the business or problem domain first, then choose technical patterns. Do not let frameworks force your domain model.

- **Bounded Contexts**  
  Split large domains into smaller, clearly defined areas where words have precise meanings. Avoid one giant model that covers everything.

- **Value Objects**  
  Use small, immutable types to represent domain concepts like money, dates, ranges. Keep logic related to these values close to the values themselves.

- **Aggregates / Consistency Boundaries**  
  Define clear clusters of data that must stay consistent together and update them through controlled operations.

---

### Simplicity, Clarity & Maintenance

- **Simplicity Over Cleverness**  
  Prefer simple, boring solutions that are easy to read and maintain over tricky, “smart” solutions. The code should be keep as simple and as short as possible, unless it explicitly wrapped with validation or optimized for performance. That will make the code easy to read, understand and maintain.

- **Self-documented Code**  
  Prefer writing code in such a way, that it can be read similar to statements in natural languages. Prefer full english words over abbriviations or shortened words. If the code contains code comments in implementation (not in constract) or it is so unclear that it is tempting to add new commends, then consider extracting separate functions or making variables/constants names more clear for english reader, so by reorganizing the code we can effectefly eliminate the need of comments between lines of code.

- **Minimize Cognitive Load**  
  Arrange code so a reader does not have to keep many details in their head at once to understand it.

- **Minimize nesting**  
  Prefer multiple early exit from function on a single level instead of nesting conditions inside each other. Try to minimize nesting of loops or recursion, that can lead to performance benefits, but only if that specific function is a bottleneck of the system, or minimizing the nesting actually serves simplification.

- **Small Units**  
  Functions, methods, and classes should be short and focused. Long ones are a sign that you should refactor.

- **Continuous refactoring**  
  Continuously improve structure as you add features or fix bugs. Do not wait for a “big rewrite.”

- **The Unix “Do One Thing Well” Principle**  
  Each tool, module, or function should do one thing and do it well. Combine them to achieve bigger goals.

- **Programs Should Work Together**  
  Design modules and services so they can be easily combined, piped, or composed with others.

---

### APIs, Services & Deployment

- **Consistent API Design**  
  Similar operations should look and behave similarly. Follow consistent naming, response formats, and error handling.

- **Backwards Compatibility When Possible**  
  When you change APIs, try not to break existing clients. Use versioning or additive changes.

- **Configuration vs. Code**  
  Keep configuration (like URLs, secrets, feature flags) out of the code and inject it at runtime.

- **Declarative Dependencies**  
  Explicitly declare dependencies (libraries, frameworks) and manage them through a standard mechanism (package managers, manifests).

- **Logs as Streams, Not Internal Storage**  
  Treat logs as time-ordered events for debugging and monitoring, not as a data store the code relies on.

- **Environment Parity**  
  Keep development, staging, and production environments as similar as practical to reduce “works on my machine” issues.

---

## 🧬 Functional Programming Principles

These assume you are using a functional style (in a pure FP language or a hybrid one).

- **Pure Functions by Default**  
  Write functions that do not depend on or change external state. Given the same input, they always return the same output.

- **Immutability as the Norm**  
  Do not change values after they are created. Use new values instead of modifying in place.

- **Function Composition**  
  Build complex behavior by chaining simple functions together, instead of writing one large function.

- **Declarative Style**  
  Focus on describing *what* you want (maps, filters, folds) instead of *how* to do it step by step with loops and mutations.

- **Higher-Order Functions**  
  Pass functions as arguments, return them from other functions, and store them in data structures to create reusable patterns.

- **Algebraic Data Types (Sum and Product Types)**  
  Build types from combinations (e.g., “either this or that”, “both this and that”) to exactly match your domain. This helps the compiler catch mistakes.

- **Pattern Matching**  
  Deconstruct values by their shape and handle all possible cases explicitly. Avoid default “catch-all” branches that hide missing cases.

- **Total Functions Where Possible**  
  Prefer functions that are defined for every possible valid input of their type, so they never crash unexpectedly.

- **Referential Transparency**  
  Any expression can be replaced by its value without changing the program’s behavior. This makes reasoning and refactoring easier.

- **Effects as Data or Types**  
  Represent side effects (like I/O, errors, asynchronous behavior) explicitly through types or special constructs, rather than hiding them.

- **No Shared Mutable State**  
  Avoid different parts of the program writing to the same mutable data. Use messages, values, or effect systems instead.

- **Small, Focused Pipelines**  
  Organize logic as pipelines (data flowing through a sequence of transformations) for clarity and reuse.

- **Use Recursion or Combinators Instead of Raw Loops (Where Idiomatic)**  
  Prefer built-in operations like `map`, `filter`, and `reduce` instead of manual loops that manage indexes and mutable accumulators.

- **Modeling Errors with Types**  
  Represent errors as values (e.g., “success or error” types) instead of throwing exceptions everywhere. This forces the caller to handle them.

- **Lazy Evaluation (Where Available and Appropriate)**  
  When the language supports it, use laziness to build infinite sequences or expensive computations that are only evaluated when needed, but keep track of potential performance traps.

---

## 🧩 Object-Oriented Programming Principles

These assume you are using classes, objects, and methods as primary building blocks.

- **Encapsulation of Data and Behavior**  
  Keep data (fields) and functions that operate on that data (methods) together. Hide internal details and expose only what is needed.

- **Information Hiding**  
  Use private or internal fields/methods to prevent other parts of the system from depending on your internal structure.

- **Single Responsibility for Classes**  
  Each class should have one main reason to change. If it changes for many different reasons, split it.

- **Open for Extension, Closed for Modification**  
  Design classes so behavior can be extended (e.g., via new subclasses or injected strategies) without editing existing working code.

- **Liskov Substitution (Safe Subtypes)**  
  A subclass should be usable anywhere its parent class is expected without breaking behavior. Do not violate expectations of the base type.

- **Small, Focused Interfaces**  
  Interfaces or abstract base classes should declare a small, coherent set of methods that belong together.

- **Depend on Abstractions, Not Concrete Implementations**  
  Code should refer to interfaces or base classes, not specific subclasses, so implementations can be swapped easily.

- **Composition Over Inheritance**  
  Prefer building objects by combining smaller objects (has-a) instead of always using inheritance (is-a), especially to share behavior.

- **Avoid Deep Inheritance Hierarchies**  
  Keep inheritance trees shallow. Deep hierarchies are hard to understand and change safely.

- **Entities and Value Objects**  
  Use entities when identity over time matters (e.g., User, Order). Use small, immutable value objects for simple concepts (e.g., Money, Address) where identity does not matter.

- **Tell, Don’t Ask**  
  Ask objects to do things rather than pulling their data out and doing the work elsewhere. This keeps behavior near data.

- **Rich Domain Model Instead of Anemic Model**  
  Put business rules and behaviors inside domain objects, not only in separate services that act on plain data structures.

- **Controller / Coordinator Objects**  
  Assign orchestration responsibilities to specific objects (e.g., services, controllers) instead of spreading workflow logic everywhere.

- **Creator Responsibility**  
  Give object creation responsibility to classes that have the necessary information or clear ownership, or centralize it in factories/builders when useful.

- **Use Polymorphism to Replace Conditionals Where It Clarifies**  
  Instead of giant `if` or `switch` statements on types, use polymorphism (different subclasses implementing different behavior) when it makes the code easier to understand.

- **Encapsulated State Changes**  
  Expose operations that represent meaningful actions in the domain (e.g., `approve()`, `cancel()`), not just generic setters for every field.

- **Avoid Feature Envy**  
  If one class is constantly reaching into another class’s data to do work, that behavior probably belongs on the other class.

- **Law of Demeter (Don’t Talk to Strangers)**  
  A method should talk only to its own fields, its parameters, or created objects—not to deep chains of objects (like `a.getB().getC().doSomething()`).

- **Use Design Patterns as Names, Not as Goals**  
  Patterns (like Strategy, Observer, Factory) are reusable solutions to recurring problems. Use them when they simplify your code, not because you “need to use patterns.”

- **Replace Inheritance with Delegation Where More Flexible**  
  When inheritance feels too rigid, have an object hold another object and delegate calls to it instead of inheriting from it.
