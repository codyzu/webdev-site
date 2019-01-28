# Web ☁️ Development
---

---

# Introduction 👋
---


```javascript
const me = {
    // Full name
    name: 'Cody Zuschlag',
    // Where do I work?
    employer: 'Hubware',
    // What experience do I have?
    experience: ['C', 'C++', 'C#', 'Java', 'JavaScript'],
    // What languages do I speak?
    languages: ['english', 'french', 'franglais'],
};
```


# This course

- 📦 6 modules
- 📝 2 tests
- 💻 homework (devoir)


<!-- ._slide: data-background-image="./images/code.png" data-background-size="100px" data-background-position="top 10px left 10px" data-background-opacity="0.75" -->
<!-- .slide: data-background-image="./images/code.png" data-background-size="auto 10%" data-background-position="bottom" data-background-opacity="0.75" -->
# Activity
---
## where are you from?

---

# Module 1: JavaScript
---

---

# History ⌛️
---


* 1995: Brendan Eicht 👉 Netscape
* "LiveScript" -> "JavaScript" (marketing)
* 1997: ECMAScript standard 1
  * es3, ~~es4~~, es5, es2016...

notes:
* created to make web an application platform


<!-- ._element: style="max-width: 150px; border: 0px; padding: 10px 10px;" -->
<!-- .slide: data-background-image="./images/nodejs.png" data-background-size="auto 10%" data-background-position="bottom" data-background-opacity="1" -->
* Google Chrome v8
* 2009: Ryan Dahl 👉 node.js


<!-- .slide: data-background-image="./images/dockerkubernetes-transparent.png" data-background-size="auto 10%" data-background-position="bottom" data-background-opacity="1" -->
# Containerization

* 2013: Docker
* 2014/2015: Kubernetes


<!-- .slide: data-background-image="./images/faas-transparent.png" data-background-size="auto 10%" data-background-position="bottom" data-background-opacity="1" -->
# Serverless / `FaaS`

* 2010: PiCloud (python)
* 2014: Amazon AWS
* 2016: MS Azure Functions (GA)
* 2018: Google Cloud Functions (GA)


# Under Attack!
# 💣 🔫 ⚔️
* Microsoft: JScript, silverlight, asp.net, typescript
* Adobe: flash

---

# 👊 JS vs Java
---
## or .NET


## vs Java
* no relation to Java
* "JavaScript" 👉 marketing
* _inspired_ by Java


<!-- .slide: data-background-image="./images/marker.png" data-background-size="auto 7%" data-background-position="bottom" data-background-opacity="0.70" -->
vs Java
<small>

* Execution
* Compilation
* Types
* Portability
* Numeric types
* OO
* Inheritance
* Concurrency
* Function overloading
* First class functions
* XML + JSON
* ~~Exception Handling (try, catch, finally)~~
* Deployment / Scaling

</small>

---

# How to 💡 JavaScript
---
This course 👈 **modern** JavaScript (ECMAScript)

---

# Types 🎟
---


* Number
* String
* Boolean
* Object
* Array


* Number
* String
* Boolean
* Object
  * Function
  * Array
  * Date
  * RegExp
* null
* undefined


# Number 🔢
* IEEE Standard for Floating-Point Arithmetic (IEEE 754)
* "only 1" number type (+ BigInt)
* parseInt, parseFloat
* NaN
* safe integer range
* FP math
* math built-in


# String 🔤
* similarities to Java
* String.prototype
* `a + b` vs `${}`
* searching
* RegExp


# Boolean ✔️ ❌
* comparison operators
* don't use `==` and `!=`
* Boolish
* `!`, `&&`, `||`
* `NaN`, inverse comparison, De Morgan's


# Object 📦
* everything excpet `null` and `undefined`
* similar to hash table, map, record, struct, dictionary
* object literals `{}`
* keys
* Object.assign()
* Object.prototype
* Object.freeze()


# Array 📜
* "spcial" Object: magic length
* creation
* `typeof` vs `Array.isArray()`
* destructive operations
* Array.prototype
* functional
* sorting
* concatination and substrings


# `null` & `undefined` ❓
* `||` & `&&`
* 2 bottom values
* language uses undefined (let, uninitialized args)
* typeof
* use `undefined` unless required
  * `Object.create(null)`
  * `JSON.stringify({}, null, 2)`

---

# Variables & Scope 👀
---


# scope
* global
* module
* function
* block


# Variables
* var
* let
* const


# `'use strict'`
* es5
* why?
* examples
* babel

---

# Control Structures 🔁
---


* conditional `if` & ternary
* loops
  * for, while, do
  * break, continue 😑
  * consider `while(true)` to allow breaking where needed
* switch


# recursion & tail calls
* es6 (but not implemented 😿)
* only if function returns result of calling a function
* optimation: recusrsion as fast as loops

---

# Functions 📞
---


* default arguments
* declarations vs expressions
* this
* function objects

---

# Inheritance
# 👴 👨 👶
---


# Classes
* What is a class?
* C++ Classes
* Java Classes


# Prototypal Inheritance
* objects inherit from other objects
* no vtable
* lighter and expressive compared to class inheritance


* objects are containers of properties
* prototypes are objects
* methods are functions, stored in objects
* missing props 👉 undfined, unless prototype exists


* may object can share same prototype
  * _similar_ to classes
* prototypes typically store methods
  * similar objects 👉 similar methods
  * saves memory
* `this`:
  * lets prototype methods now which object
  * dynamically bound ⚠️


# JavaScript Inheritance


## legacy construtors 👎
* magic `new` operator


## class 👍 👎
* "syntatic sugar" over legacy construtors, not true classes
* `this` or that?
* usefule for migrating from other languages


## factories 👍
## 🏭
* memory?
* encapsulation with closures (private vars possible)
* no `this` 👉 no errors


<!-- .slide: data-background-image="./images/typescript.png" data-background-size="auto 10%" data-background-position="bottom" data-background-opacity="1" -->
# Typescript
* MS
* requires annotations (allows interfaces)
* large project with big teams
* static type controversy

---

# Semicolons `;`
---
* Automatic Semicolon Insertion (ASI)
* danger! ☣️

---

# Bonus: FP
---
* 1st class function
* dividing

---

# Homework (Devoir)
---
1. create google account
1. create github account
1. email me:
  * google email address
  * github username
