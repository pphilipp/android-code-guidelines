## Valtech Clean Code Guidelines (based on Smells and Heuristics Clean Code by Robert Martin) 

## COMMENTS

## C1: Inappropriate Information

It is inappropriate for a comment to hold information better held in a different kind of system such as your source code control system, your issue tracking system, or any other record-keeping system. In general, meta-data such as authors, last-modified-date, SPR number, and so on should not appear in comments. Comments should be reserved for technical notes about the code and design.

## C2: Obsolete Comment

A comment that has gotten old, irrelevant, and incorrect is obsolete. It is best not to write a comment that will become obsolete. If you find an obsolete comment, it is best to update it or get rid of it as quickly as possible. Obsolete comments tend to migrate away from the code they once described. 
 
## C3: Redundant Comment
 
A comment is redundant if it describes something that adequately describes itself (for example: `i++; // increment i`). Comments should say things that the code cannot say for itself.

## C4: Poorly Written Comment

A comment worth writing is worth writing well. If you are going to write a comment, take the time to make sure it is the best comment you can write. Choose your words carefully. Use correct grammar and punctuation. Don’t ramble. Don’t state the obvious. Be brief.

## C5: Commented-Out Code

When you see commented-out code, delete it! Don’t worry, the source code control system still remembers it. If anyone really needs it, he or she can go back and check out a previous version. Don’t suffer commented-out code to survive.
 
##  FUNCTIONS

## F1: Too Many Arguments

Functions should have a small number of arguments. No argument is best, followed by one, two, and three. More than three is very questionable and should be avoided with prejudice. 

## F2: Output Arguments

Output arguments are counterintuitive. Readers expect arguments to be inputs, not outputs. If your function must change the state of something, have it change the state of the object it is called on. 

## F3: Flag Arguments

Boolean arguments loudly declare that the function does more than one thing. They are confusing and should be eliminated. 

## F4: Dead Function

Methods that are never called should be discarded. Keeping dead code around is wasteful. Don’t be afraid to delete the function. Remember, your source code control system still remembers it.

## GENERAL

## G1: Multiple Languages in One Source File

The ideal is for a source file to contain one, and only one, language. Realistically, we will probably have to use more than one. But we should take pains to minimize both the number and extent of extra languages in our source files.

##  G2: Obvious Behavior Is Unimplemented
Following “The Principle of Least Surprise,” any function or class should implement the behaviors that another programmer could reasonably expect. When an obvious behavior is not implemented, readers and users of the code can no longer depend on their intuition about function names. They lose their trust in the original author and must fall back on reading the details of the code.

## G3: Incorrect Behavior at the Boundaries

Every boundary condition, every corner case, every quirk and exception represents something that can confound an elegant and intuitive algorithm. Don’t rely on your intuition. Look for every boundary condition and write a test for it.

## G5: Duplication

This is one of the most important rules and you should take it very seriously. One of the core principles of Extreme Programming and called it: “Once, and only once.” Every time you see duplication in the code, it represents a missed opportunity for abstraction. That duplication could probably become a subroutine or perhaps another class outright. By folding the duplication into such an abstraction, you increase the vocabulary of the language of your design. Other programmers can use the abstract facilities you create. Coding becomes faster and less error prone because you have raised the abstraction level.
The most obvious form of duplication is when you have clumps of identical code. These should be replaced with simple methods.
A more subtle form is the `switch/case` or `if/else` chain that appears again and again in various modules, always testing for the same set of conditions. These should be replaced with polymorphism.
Still more subtle are the modules that have similar algorithms, but that don’t share similar lines of code. This is still duplication and should be addressed by using the `TEMPLATE METHOD`, or `STRATEGY` pattern.

## G6: Code at Wrong Level of Abstraction

It is important to create abstractions that separate higher level general concepts from lower level detailed concepts. Sometimes we do this by creating abstract classes to hold the higher level concepts and derivatives to hold the lower level concepts. When we do this, we need to make sure that the separation is complete. We want all the lower level concepts to be in the derivatives and all the higher level concepts to be in the base class.
For example, constants, variables, or utility functions that pertain only to the detailed implementation should not be present in the base class. The base class should know nothing about them.

## G7: Base Classes Depending on Their Derivatives

The most common reason for partitioning concepts into base and derivative classes is so that the higher level base class concepts can be independent of the lower level derivative class concepts. Therefore, when we see base classes mentioning the names of their derivatives, we suspect a problem. In general, base classes should know nothing about their derivative.
There are exceptions to this rule, of course. Sometimes the number of derivatives is strictly fixed, and the base class has code that selects between the derivatives. We see this a lot in finite state machine implementations. However, in that case the derivatives and base class are strongly coupled and always deploy together in the same `jar` file. In the general case we want to be able to deploy derivatives and bases in different `jar` files.
Deploying derivatives and bases in different jar files and making sure the base `jar` files know nothing about the contents of the derivative jar files allow us to deploy our systems in discrete and independent components. When such components are modified, they can be redeployed without having to redeploy the base components. This means that the impact of a change is greatly lessened, and maintaining systems in the field is made much simpler.

## G8: Too Much Information

Well-defined modules have very small interfaces that allow you to do a lot with a little. Poorly defined modules have wide and deep interfaces that force you to use many different gestures to get simple things done. A well-defined interface does not offer very many functions to depend upon, so coupling is low. A poorly defined interface provides lots of functions that you must call, so coupling is high.
Good software developers learn to limit what they expose at the interfaces of their classes and modules. The fewer methods a class has, the better. The fewer variables a function knows about, the better. The fewer instance variables a class has, the better.
Hide your data. Hide your utility functions. Hide your constants and your temporaries. Don’t create classes with lots of methods or lots of instance variables. Don’t create lots of protected variables and functions for your subclasses. Concentrate on keeping interfaces very tight and very small. Help keep coupling low by limiting information.

## G9: Dead Code

Dead code is code that isn’t executed. You find it in the body of an if statement that checks for a condition that can’t happen. You find it in the catch block of a try that never throws. You find it in little utility methods that are never called or switch/case conditions that never occur.
The problem with dead code is that after awhile it starts to smell. The older it is, the stronger and sourer the odor becomes. This is because dead code is not completely updated when designs change. It still compiles, but it does not follow newer conventions or rules. It was written at a time when the system was different. When you find dead code, do the right thing. Give it a decent burial. Delete it from the system.

## G10: Vertical Separation

Variables and function should be defined close to where they are used. Local variables should be declared just above their first usage and should have a small vertical scope. We don’t want local variables declared hundreds of lines distant from their usages.
Private functions should be defined just below their first usage. Private functions belong to the scope of the whole class, but we’d still like to limit the vertical distance between the invocations and definitions. Finding a private function should just be a matter of scanning downward from the first usage.

## G11: Inconsistency

If you do something a certain way, do all similar things in the same way. This goes back to the principle of least surprise. Be careful with the conventions you choose, and once chosen, be careful to continue to follow them.
If within a particular function you use a variable named response to hold an HttpServletResponse, then use the same variable name consistently in the other functions that use HttpServletResponse objects. If you name a method processVerificationRequest, then use a similar name, such as processDeletionRequest, for the methods that process other kinds of requests.
Simple consistency like this, when reliably applied, can make code much easier to read and modify.

## G12: Clutter

Of what use is a default constructor with no implementation? All it serves to do is clutter up the code with meaningless artifacts. Variables that aren’t used, functions that are never called, comments that add no information, and so forth. All these things are clutter and should be removed. Keep your source files clean, well organized, and free of clutter.

## G13: Artificial Coupling

Things that don’t depend upon each other should not be artificially coupled. For example, general enums should not be contained within more specific classes because this forces the whole application to know about these more specific classes. The same goes for general purpose static functions being declared in specific classes.
In general an artificial coupling is a coupling between two modules that serves no direct purpose. It is a result of putting a variable, constant, or function in a temporarily convenient, though inappropriate, location. This is lazy and careless.
Take the time to figure out where functions, constants, and variables ought to be declared. Don’t just toss them in the most convenient place at hand and then leave them there.

## G14: Feature Envy

This is one of Martin Fowler’s code smells. The methods of a class should be interested in the variables and functions of the class they belong to, and not the variables and functions of other classes. When a method uses accessors and mutators of some other object to manipulate the data within that object, then it envies the scope of the class of that other object. It wishes that it were inside that other class so that it could have direct access to the variables it is manipulating. 
Specifically, the Single Responsibility Principle, the Open Closed Principle, and the Common Closure Principle.
G15: Selector Arguments
Not only is the purpose of a selector argument difficult to remember, each selector argument combines many functions into one. Selector arguments are just a lazy way to avoid splitting a large function into several smaller functions. Of course, selectors need not be boolean. They can be enums, integers, or any other type of argument that is used to select the behavior of the function. In general it is better to have many functions than to pass some code into a function to select the behavior.

## G16: Obscured Intent

We want code to be as expressive as possible. Run-on expressions, Hungarian notation, and magic numbers all obscure the author’s intent. It is worth taking the time to make the intent of our code visible to our readers.

## G17: Misplaced Responsibility

One of the most important decisions a software developer can make is where to put code.  The principle of least surprise comes into play here. Code should be placed where a reader would naturally expect it to be. The `PI` constant should go where the trig functions are declared. The `OVERTIME_RATE` constant should be declared in the `HourlyPayCalculator` class.

## G18: Inappropriate Static

`Math.max(double a, double b)` is a good static method. It does not operate on a single instance; indeed, it would be silly to have to say `new Math().max(a,b)` or even `a.max(b)`. 
In general you should prefer nonstatic methods to static methods. When in doubt, make the function nonstatic. If you really want a function to be static, make sure that there is no chance that you’ll want it to behave polymorphically.

## G19: Use Explanatory Variables

One of the more powerful ways to make a program readable is to break the calculations up into intermediate values that are held in variables with meaningful names.
It is hard to overdo this. More explanatory variables are generally better than fewer. It is remarkable how an opaque module can suddenly become transparent simply by breaking the calculations up into well-named intermediate values.

## G20: Function Names Should Say What They Do

If you have to look at the implementation (or documentation) of the function to know what it does, then you should work to find a better name or rearrange the functionality so that it can be placed in functions with better names.

## G21: Understand the Algorithm

Before you consider yourself to be done with a function, make sure you understand how it works. It is not good enough that it passes all the tests. You must know that the solution is correct.
There is a difference between knowing how the code works and knowing whether the algorithm will do the job required of it. Being unsure that an algorithm is appropriate is often a fact of life. Being unsure what your code does is just laziness.
Often the best way to gain this knowledge and understanding is to refactor the function into something that is so clean and expressive that it is obvious how it works.

## G23: Prefer Polymorphism to If/Else or Switch/Case

Use the following “ONE SWITCH” rule: There may be no more than one switch statement for a given type of selection. The cases in that switch statement must create polymorphic objects that take the place of other such switch statements in the rest of the system.

## G24: Follow Standard Conventions

Every team should follow a coding standard based on common industry norms. This coding standard should specify things like where to declare instance variables; how to name classes, methods, and variables; where to put braces; and so on. The team should not need a document to describe these conventions because their code provides the examples.
Everyone on the team should follow these conventions. This means that each team member must be mature enough to realize that it doesn’t matter a whit where you put your braces so long as you all agree on where to put them.

## G25: Replace Magic Numbers with Named Constants

It is a bad idea to have raw numbers in your code. You should hide them behind well-named constants.
For example, the number 86,400 should be hidden behind the constant `SECONDS_PER_DAY`. If you are printing 55 lines per page, then the constant 55 should be hidden behind the constant `LINES_PER_PAGE`.

## G26: Be Precise

When you make a decision in your code, make sure you make it precisely. Know why you have made it and how you will deal with any exceptions. Don’t be lazy about the precision of your decisions. If you decide to call a function that might return null, make sure you check for null. If you query for what you think is the only record in the database, make sure your code checks to be sure there aren’t others. If you need to deal with currency, use integers and deal with rounding appropriately. If there is the possibility of concurrent update, make sure you implement some kind of locking mechanism.
Ambiguities and imprecision in code are either a result of disagreements or laziness. In either case they should be eliminated.

## G27: Structure over Convention

Enforce design decisions with structure over convention. Naming conventions are good, but they are inferior to structures that force compliance. For example, `switch/cases` with nicely named enumerations are inferior to base classes with abstract methods. No one is forced to implement the `switch/case` statement the same way each time; but the base classes do enforce that concrete classes have all abstract methods implemented.

## G28: Encapsulate Conditionals

Boolean logic is hard enough to understand without having to see it in the context of an if or while statement. Extract functions that explain the intent of the conditional (for example: `if (shouldBeDeleted(timer))` is preferable to `if (timer.hasExpired() && !timer.isRecurrent()))`.

## G29: Avoid Negative Conditionals

Negatives are just a bit harder to understand than positives. So, when possible, conditionals should be expressed as positives. For example:  `if (buffer.shouldCompact())` is preferable to `if (!buffer.shouldNotCompact()))`.

## G30: Functions Should Do One Thing

Functions of this kind do more than one thing, and should be converted into many smaller functions, each of which does one thing.
Each of these functions does one thing. (“Do One Thing”)

## G31: Hidden Temporal Couplings

Temporal couplings are often necessary, but you should not hide the coupling. Structure the arguments of your functions such that the order in which they should be called is obvious. 
If the order of the three functions is important (for example let’s take 3 functions). Each function must produces a result that the next function needs, so there is no reasonable way to call them out of order.

## G32: Don’t Be Arbitrary

Have a reason for the way you structure your code, and make sure that reason is communicated by the structure of the code. If a structure appears arbitrary, others will feel empowered to change it. If a structure appears consistently throughout the system, others will use it and preserve the convention. 

## G33: Encapsulate Boundary Conditions

Boundary conditions are hard to keep track of. Put the processing for them in one place. Don’t let them leak all over the code. We don’t want swarms of `+1`-s and `-1-`s scattered hither and yon. 

## G34: Functions Should Descend Only One Level of Abstraction

The statements within a function should all be written at the same level of abstraction, which should be one level below the operation described by the name of the function. This may be the hardest of these heuristics to interpret and follow. This points out that when you break a function along lines of abstraction, you often uncover new lines of abstraction that were obscured by the previous structure.

## G35: Keep Configurable Data at High Levels

If you have a constant such as a default or configuration value that is known and expected at a high level of abstraction, do not bury it in a low-level function. Expose it as an argument to that low-level function called from the high-level function.

## G36: Avoid Transitive Navigation

In general we don’t want a single module to know much about its collaborators. More specifically, if `A` collaborates with `B`, and `B` collaborates with `C`, we don’t want modules that use `A` to know about `C`. (For example, we don’t want `a.getB().getC().doSomething()`)
This is sometimes called the Law of Demeter. The Pragmatic Programmers call it “Writing Shy Code.” In either case it comes down to making sure that modules know only about their immediate collaborators and do not know the navigation map of the whole system.

## JAVA

## J1: Avoid Long Import Lists by Using Wildcards

If you use two or more classes from a package, then import the whole package with `import package.*;`. There are times when the long list of specific imports can be useful. For example, if you are dealing with legacy code and you want to find out what classes you need to build mocks and stubs for, you can walk down the list of specific imports to find out the true qualified names of all those classes and then put the appropriate stubs in place. However, this use for specific imports is very rare. Furthermore, most modern IDEs will allow you to convert the wildcarded imports to a list of specific imports with a single command. So even in the legacy case it’s better to import wildcards.
Wildcard imports can sometimes cause name conflicts and ambiguities. Two classes with the same name, but in different packages, will need to be specifically imported, or at least specifically qualified when used. This can be a nuisance but is rare enough that using wildcard imports is still generally better than specific imports.

## J2: Don’t Inherit Constants

Do not puts some constants in an interface and then gains access to those constants by inheriting that interface. 

## J3: Constants vs Enums

Now that enums have been added to the language (Java 5), use them! Don’t keep using the old trick of public static final ints. The meaning of ints can get lost. The meaning of enums cannot, because they belong to an enumeration that is named.
What’s more, study the syntax for enums carefully. They can have methods and fields. This makes them very powerful tools that allow much more expression and flexibility than ints. 

## NAMES

## N1: Choose Descriptive Names

Don’t be too quick to choose a name. Make sure the name is descriptive. Remember that meanings tend to drift as software evolves, so frequently reevaluate the appropriateness of the names you choose.
This is not just a “feel-good” recommendation. Names in software are 90 percent of what make software readable. You need to take the time to choose them wisely and keep them relevant. Names are too important to treat carelessly.

## N2: Choose Names at the Appropriate Level of Abstraction

Don’t pick names that communicate implementation; choose names the reflect the level of abstraction of the class or function you are working in. This is hard to do. Again, people are just too good at mixing levels of abstractions. Each time you make a pass over your code, you will likely find some variable that is named at too low a level. You should take the opportunity to change those names when you find them. Making code readable requires a dedication to continuous improvement.

## N3: Use Standard Nomenclature Where Possible

Names are easier to understand if they are based on existing convention or usage. For example, if you are using the DECORATOR pattern, you should use the word Decorator in the names of the decorating classes. For example, AutoHangupModemDecorator might be the name of a class that decorates a Modem with the ability to automatically hang up at the end of a session.

## N4: Unambiguous Names

Choose names that make the workings of a function or variable unambiguous. 

## N5: Use Long Names for Long Scopes

The length of a name should be related to the length of the scope. You can use very short variable names for tiny scopes, but for big scopes you should use longer names.
Variable names like i and j are just fine if their scope is five lines long. On the other hand, variables and functions with short names lose their meaning over long distances. So the longer the scope of the name, the longer and more precise the name should be.

## N6: Avoid Encodings

Names should not be encoded with type or scope information. Prefixes such as m_ or f are useless in today’s environments. Also project and/or subsystem encodings such as vis_ (for visual imaging system) are distracting and redundant. Again, today’s environments provide all that information without having to mangle the names. Keep your names free of Hungarian pollution.

## N7: Names Should Describe Side-Effects

Names should describe everything that a function, variable, or class is or does. Don’t hide side effects with a name. Don’t use a simple verb to describe a function that does more than just that simple action. 

## TESTS

## T1: Insufficient Tests

How many tests should be in a test suite? Unfortunately, the metric many programmers use is “That seems like enough.” A test suite should test everything that could possibly break. The tests are insufficient so long as there are conditions that have not been explored by the tests or calculations that have not been validated.

## T2: Use a Coverage Tool!

Coverage tools reports gaps in your testing strategy. They make it easy to find modules, classes, and functions that are insufficiently tested. Most IDEs give you a visual indication, marking lines that are covered in green and those that are uncovered in red. This makes it quick and easy to find if or catch statements whose bodies haven’t been checked.

## T3: Don’t Skip Trivial Tests

They are easy to write and their documentary value is higher than the cost to produce them.

## T4: An Ignored Test Is a Question about an Ambiguity

Sometimes we are uncertain about a behavioral detail because the requirements are unclear. We can express our question about the requirements as a test that is commented out, or as a test that annotated with `@Ignore`. Which you choose depends upon whether the ambiguity is about something that would compile or not.

## T5: Test Boundary Conditions

Take special care to test boundary conditions. We often get the middle of an algorithm right but misjudge the boundaries.

## T6: Exhaustively Test Near Bugs

Bugs tend to congregate. When you find a bug in a function, it is wise to do an exhaustive test of that function. You’ll probably find that the bug was not alone.

## T7: Patterns of Failure Are Revealing

Sometimes you can diagnose a problem by finding patterns in the way the test cases fail. This is another argument for making the test cases as complete as possible. Complete test cases, ordered in a reasonable way, expose patterns.
As a simple example, suppose you noticed that all tests with an input larger than five characters failed? Or what if any test that passed a negative number into the second argument of a function failed? Sometimes just seeing the pattern of red and green on the test report is enough to spark the “Aha!” that leads to the solution. 

## T8: Test Coverage Patterns Can Be Revealing

Looking at the code that is or is not executed by the passing tests gives clues to why the failing tests fail.

## T9: Tests Should Be Fast

A slow test is a test that won’t get run. When things get tight, it’s the slow tests that will be dropped from the suite. So do what you must to keep your tests fast.


## CONCLUSION

This list of heuristics and smells could hardly be said to be complete. Such a list can ever be complete. But perhaps completeness should not be the goal, because what this list does do is imply a value system.
Indeed, that value system has been the goal. Clean code is not written by following a set of rules. You don’t become a software craftsman by learning a list of heuristics. Professionalism and craftsmanship come from values that drive disciplines.