# JavaScript Guidelines

This part of the methodology is still in alpha.

---

## Contents

1. [General style guidelines](#general-style-guidelines)
2. [Comment convention](#comment-convention)
3. [Conditionals](#conditionals)
4. [Functions](#functions)

---

## General style guidelines

- Maximum column count: 80, unless:
    - The expression or statement doesn't take too many extra characters, **and** rearranging it would make it less legible
    - The line is part of an HTML snippet that doesn't need to be read by the programmer

---

## Comment convention

It is based on the same style as with [CSS comments](css.md#comment-convention), with the introduction of 
section titles to denote blocks inside the same file:
```javascript
///////////////////
// SECTION TITLE //
///////////////////_____________________________________________________________
```

### Special comments

- **Warnings** are used *sparingly* to remember a condition that has a *high* possibility of being forgotten, and lead to *major* bugs
```javascript
// =============================================================================
//                   ⚠️⚠️⚠️ CAUTION / ATENCIÓN / ACHTUNG ⚠️⚠️⚠️
// =============================================================================
// Text explaining the warning.
```
- Refactoring candidates
```javascript
// =============================================================================
//                          🛠️ REFACTORING CANDIDATE 🛠️
// =============================================================================
// Text commenting about why the possibility refactoring this is so useful that 
// you decided to draw attention to it with a big reminder.
```

---

## Conditionals

- Each operand in a ternary operator expression is visually grouped (either by trimming any spaces or by adding parens) and an extra space is added to each side of the signs to make it more legible
```javascript
conditionStatement  ?  ifTrue  :  ifFalse;
```
- If the ternary operator expression takes more than defined number of columns, it is displayed like this:
```javascript
conditionStatement ? 
    veryLongStatementForTheTruthCase :  
    veryLongStatementForTheFalseCase;
```
- Long lists of conditions are nested
```javascript
if (
    // <first condition> <boolean operator at the end>
    // ...
) {
    // <expressions>
}
```

---

## Functions

### Nested function definitions

Whether or not this pattern is good or bad depends on the particular situation, however, it generally comes down to thinking what you gain with the closure (how much neater does it makes your code) vs what you loose with creating a new function object every time you call the parent. More on the topic [here](https://stackoverflow.com/questions/11873566/javascript-nesting-of-private-functions-good-or-bad) and [there](https://softwareengineering.stackexchange.com/questions/137495/should-i-nest-functions-in-languages-that-allow-me-to-do-that-or-should-i-rather).

### Higher-order functions

```javascript
// if all the expression fits in a single line
myThirdFn( mySecondFn( myFirstFn( myRawArgument ) ) );

// for longer expressions (names were exaggerated for educational purposes)
myThirdFunctionThatTakesTheSecondAsInput( 
    mySecondFunctionThatTakesTheFirstAsInput( 
        myFirstFunctionThatTakesANonFunctionInput( 
            myFirstRawArgument,
            yetAnotherRawArgument
        ) 
    ) 
);
```
