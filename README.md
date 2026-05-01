<!-- markdownlint-disable MD007 -- Unordered list indentation -->
<!-- markdownlint-disable MD010 -- No hard tabs -->
<!-- markdownlint-disable MD033 -- No inline html -->
<!-- markdownlint-disable MD055 -- Table pipe style [Expected: leading_and_trailing; Actual: leading_only; Missing trailing pipe] -->
<!-- markdownlint-disable MD041 -- First line in a file should be a top-level heading -->
<div align="center">

[![!#/bin/bash](https://img.shields.io/badge/-%23!%2Fbin%2Fbash-1f425f.svg?logo=gnu-bash)](https://www.gnu.org/software/bash/)
![License: GPL v2](https://img.shields.io/badge/License-GPLv2-blue.svg)

</div>
<!--
![Go](https://img.shields.io/badge/Go-00ADD8?logo=go&logoColor=white)
[![!#/bin/bash](https://img.shields.io/badge/-%23!%2Fbin%2Fbash-1f425f.svg?logo=gnu-bash)](https://www.gnu.org/software/bash/)
![License: GPL v2](https://img.shields.io/badge/License-GPLv2-blue.svg)
![License: GPL v3](https://img.shields.io/badge/License-GPLv3-blue.svg)
![Lifecycle: Alpha](https://img.shields.io/badge/Lifecycle-Alpha-orange)
![Lifecycle: Beta](https://img.shields.io/badge/Lifecycle-Beta-yellow)
![Lifecycle: RC](https://img.shields.io/badge/Lifecycle-RC-blue)
![Lifecycle: Stable](https://img.shields.io/badge/Lifecycle-Stable-brightgreen)
![Lifecycle: Deprecated](https://img.shields.io/badge/Lifecycle-Deprecated-red)
![Status: Deprecated](https://img.shields.io/badge/Status-Deprecated-orange)
![Status: Archived](https://img.shields.io/badge/Status-Archived-lightgrey)
![Lifecycle: EOL](https://img.shields.io/badge/Lifecycle-EOL-lightgrey)
![Coverage](https://img.shields.io/badge/Coverage-25%25-red)
![Coverage](https://img.shields.io/badge/Coverage-50%25-orange)
![Coverage](https://img.shields.io/badge/Coverage-75%25-yellow)
![Coverage](https://img.shields.io/badge/Coverage-90%25-brightgreen)
![Status: Passing](https://img.shields.io/badge/Status-Passing-brightgreen)
![Status: Failing](https://img.shields.io/badge/Status-Failing-red)
-->

<!-- TOC ignore:true -->
# TOOBLIN: True Object-Oriented Bash, Lightweight and Idiomatic - with enforced data Normal forms

This is a fleshed-out high-level design document - with some deeper dives, definitions, and discussions where appropriate. There is no code yet, other than defined arrays and syntax definitions.

<!-- TOC ignore:true -->
## Table of contents

<!-- TOC -->

- [Introduction](#introduction)
- [TOOBLIN goals](#tooblin-goals)
	- [Present boring, bog-standard OOP syntax sugar that is immediately usable by any OOP programmer](#present-boring-bog-standard-oop-syntax-sugar-that-is-immediately-usable-by-any-oop-programmer)
	- [Provide strong OOP contracts with definitionally true and 100% complete "OOP"](#provide-strong-oop-contracts-with-definitionally-true-and-100%25-complete-oop)
	- [Support for optional more advanced OOP features](#support-for-optional-more-advanced-oop-features)
	- [Strongly-typed](#strongly-typed)
	- [Run as natively and bare-metal as possible](#run-as-natively-and-bare-metal-as-possible)
	- [Minimized wrapping and parsing after startup](#minimized-wrapping-and-parsing-after-startup)
	- [Leaky abstractions as a feature not a bug](#leaky-abstractions-as-a-feature-not-a-bug)
	- [Built-in data-safety features](#built-in-data-safety-features)
	- [Enforcement of five data Normal Forms with no extra work](#enforcement-of-five-data-normal-forms-with-no-extra-work)
	- [Back-end storage agnostic](#back-end-storage-agnostic)
	- [Serializable datasets and object states](#serializable-datasets-and-object-states)
- [Reference](#reference)
	- [Associative arrays](#associative-arrays)
	- [Index arrays](#index-arrays)
	- [UNQ: Unique Constraints - defines logical row and object uniqueness](#unq-unique-constraints---defines-logical-row-and-object-uniqueness)
- [How the TOOBLIN magic is done](#how-the-tooblin-magic-is-done)
	- [Unique constraints and fast lookups](#unique-constraints-and-fast-lookups)
	- [Real object variables](#real-object-variables)
	- [OOP syntax sugar goodness](#oop-syntax-sugar-goodness)
	- [Return a resultset from a substring query on a large number or rows, quickly](#return-a-resultset-from-a-substring-query-on-a-large-number-or-rows-quickly)
	- [Lean on sparse arrays](#lean-on-sparse-arrays)
- [Design](#design)
	- [Overview](#overview)
	- [Array name convention](#array-name-convention)
	- [Schema](#schema)
		- [Entities](#entities)
			- [Entity Traits](#entity-traits)
		- [Many-to-many relationship definitions](#many-to-many-relationship-definitions)
		- [Attributes](#attributes)
			- [Attribute Traits](#attribute-traits)
				- [Traits common to data and code members](#traits-common-to-data-and-code-members)
				- [Traits specific to data members: attributes, property setters, and fields](#traits-specific-to-data-members-attributes-property-setters-and-fields)
				- [Traits specific to code members: methods, property getters and setters, and events](#traits-specific-to-code-members-methods-property-getters-and-setters-and-events)
		- [Unique constraint definitions](#unique-constraint-definitions)
	- [Instanced data](#instanced-data)
		- [Rows](#rows)
			- [Row Trait overrides - aka class static member overrides](#row-trait-overrides---aka-class-static-member-overrides)
		- [Cells](#cells)
			- [Cell Trait overrides - aka class member overrides](#cell-trait-overrides---aka-class-member-overrides)
				- [Traits overrides common to data and code members](#traits-overrides-common-to-data-and-code-members)
				- [Traits overrides specific to data members: attributes, property setters, and fields](#traits-overrides-specific-to-data-members-attributes-property-setters-and-fields)
				- [Traits overrides specific to code members: methods, property getters and setters, and events](#traits-overrides-specific-to-code-members-methods-property-getters-and-setters-and-events)
		- [Data relationships and integrity](#data-relationships-and-integrity)
			- [Unique constraint instances](#unique-constraint-instances)
			- [Many-to-Many entity relationship instances](#many-to-many-entity-relationship-instances)
	- [Function definitions by usage example](#function-definitions-by-usage-example)
- [The rich existing landscape of Bash OOP projects](#the-rich-existing-landscape-of-bash-oop-projects)
	- [Common lightweight approaches](#common-lightweight-approaches)
	- [Example projects on Github](#example-projects-on-github)
- [To-do](#to-do)
- [History](#history)

<!-- /TOC -->

## Introduction

There are potentially countless repositories on github that allow the simulation of OOP in Bash. Many rely on a couple of useful idioms, and some fundamental truths about OOP:

- OOP support in all modern languages provide "syntactic sugar" on top of boring, flat, and sometimes hideously ugly data structures under the hood. The translation between the pretty programming models, and the inner guts, is managed by a runtime library, boiled away by a low-level optimizing static compiler, or somewhere in-between. With procedural scripting languages like Bash, you can often accomplish some of the same things - but at incredible complexity and performance cost.

- Bash OOP libraries and frameworks attempt to meet somewhere in the middle: add some OOP syntactic sugar/abstraction/safety, and hide the complexity of doing so - meanwhile minimizing the performance hit as much as possible. All OOP projects aim for some balance in-between those two unavoidable extremes.

The section [The rich existing landscape of Bash OOP projects](#the-rich-existing-landscape-of-bash-oop-projects) below, gives an overview of some common approaches.

## TOOBLIN goals

### Present boring, bog-standard OOP syntax sugar that is immediately usable by any OOP programmer

If the cognitive load is too high to learn a new, one-off, arcane and possibly inscrutable syntax just to achieve OOP-like Bash - then it's not going to have broad appeal.

Any OOP programmer that can also script in Bash, should be able to immediately pick this up without having to read pages of `readme`s.

### Provide strong OOP contracts with definitionally true and 100% complete "OOP"

This means code-level enforcement of:

- Abstraction
- Encapsulation
- Inheritance
- Polymorphism

### Support for optional more advanced OOP features

The features below aren't required to be used; everything is set up with sane defaults (that most other Bash OOP projects don't support at all and thus more or less default _only_ to). Including:

- Static class members available at the class level without object instantiation
- Access modifiers: public, private, and protected members
- Inheritance control: final, virtual, abstract, overridden
- Events

### Strongly-typed

- It's all ultimately strings and integers in Bash of course, but data types are defined with a typical standard menu of options, and validated and treated as such.

### Run as natively and bare-metal as possible

All of OOP's goodness - such as abstraction through private members, access control over inheritability, read-onlyness, and the RDBMS-like features including data relationship integrity and five normal forms - are doable without insane levels of wrapping, parsing, and contorting Bash into doing things it doesn't like.

Under the hood, it's 100% native with almost no subshells.

And as mentioned before, the necessary layer of syntax sugar can be bypassed and leaky abstractions structured in a way that can be taken advantage of, if necessary in performance-critical sections.

Other than that, most of the parsing heavy-lifting is done when the library is loaded, and from then on it's pretty low-level sailing.

### Minimized wrapping and parsing after startup

All class methods, functions, fields, etc. are loaded into memory and given unique names. Class and member definitions can be in-line - in a `HEREDOC` for example - or in one or more `.class` files.

After that, the only parsing done is to provide sytax sugar. But that can also by sidestepped if/when necessary, with...

### Leaky abstractions as a feature not a bug

Since this is a scripting language, and Bash has no OOP features baked-in - _any and all_ "OOP-like Bash framework" have very leaky abstractions. Including this one.

So the goal is not to hide the leaks or pretend they don't exist. The goal is to provide strong 100% OOP with all the syntax sugar goodness, _when using the framework_.

At the same time, we can design the guts under the hood in a way so that the leaks are a _feature_. For example, for performance-critical sections of a script (e.g. long-running nested loops), the underlying arrays and functions should be reasonably understandable and accessible, while losing minimal data integrity or OOP safety. (Just with loss of OOP syntax sugar, having to fall back to an arcane Bash-native syntax.)

This is literally the same idea behind C++'s leaky abstractions of C, and both being leaky abstractions of machine-level code. C++ is _notorious_ for leaky abstractions of its compatible C roots. But separately from that, given that _all_ languages are leaky abstractions on top of machine code, C and C++ as languages provide mechanism (in various iterations) to add machine-level assembler code into a source file. In that way, the designers chose to make that particular leakyness a feature in critical deterministic high-performance sections of code, not a bug.

### Built-in data-safety features

- Treat data as a first-class citizen.

- Focused on well-structured entities, attributes, and entity relationships as part of class definitions.

### Enforcement of five data Normal Forms with no extra work

- 1NF: Atomic values; no repeating groups or arrays in columns.

- 2NF: No partial dependencies. Non-key attributes depend on the whole primary key, not part of it.

- 3NF: No transitive dependencies. Non-key attributes depend only on the key, not on other non-key attributes.

- 4NF: No multi-valued dependencies.

- 5NF: No join dependencies that can't be inferred from candidate keys.

### Back-end storage agnostic

- Can use JSON, or a proper RDBMS database as the back-end.

### Serializable datasets and object states

## Reference

### Associative arrays

Bash associative arrays use a hashtable in the under the hood, and store key=value pairs. It comes with two attributes important to TOOBLIN:

- For any given array, the key is always, by definition, unique. (In the same meaning that a specific index value is always unique for a given indexed array.)

- Retrieval of a value by key is very fast, in O(1) time.

### Index arrays

Regular index arrays are accessed via an integer index, e.g. `myArray[5]="Bob"`. This number is very important in TOOBLIN, and used everywhere under the hood

It's referenced with the suffix `Idx` under the hood.

### UNQ: Unique Constraints - defines logical row and object uniqueness

A "unique constraint" (labeled "`UNQ`" in the code) is the natural data that uniquely defines a record, and is the most sacrosanct concept both for RDBMS design, and for TOOBLIN.

In hierarchical data structures, this is rarely one field by itself - but usually at least a parent ID plus a child "Label".

In this design, every "table" (or "class", "entity", or "group of arrays") must have one and only one unique constraint, in addition to the common array index across a group of arrays.

A `UNQ` is usually one of:

- A composite index. (E.g. `"${EntIdx}.${RowIdx}"`.)

- An index that enforces Label uniqueness when combined with a parent. (E.g. `"${ParentIdx}.${Label}"`.)

- At the highest level, 'Entity', has only one field as the UNQ: `Label`.

- If using bash arrays as the back-end, uniqueness is enforced via associative arrays.

## How the TOOBLIN magic is done

- TOOBLIN doesn't directly create data arrays from `.class` definitions, as most Bash OOP libraries do. Instead, it manages it's own efficient set of arrays under the hood, that are accessible through a thin and fast indirection layer. That array index is treated as an "object" by the syntax.

- Class member code is loaded into memory as named Bash functions, but also run through a thin and fast indirection layer. Not just to provide syntax sugar, but also to maintain data consistency and strict OOP contracts.

### Unique constraints and fast lookups

TOOBLIN uses associative arrays to:

- Enforce unique constraints - quickly and easily, by definition.

- Provide fast relationship mapping and lookups.

	These arrays, in TOOBLIN, usually have the word `UNQ` in them.

### Real object variables

In any OOP language, "object" references are just thinly-wrapped pointers or indexes.

To the user of TOOBLIN, an "object variable" is just an integer holding a reference to a unique `rowIdx`, which identifies both a class, and an instance of it. (Aka an entity and specific row.) But in true OOP-fashion, a thin layer of sytactic sugar lets us fully believe it's a real boy. I mean object variable.

### OOP syntax sugar goodness

After initial startup and optional `.class` file loading and parsing (where the most work is done to turn class structures into native Bash), the only time light parsing is performed, is to help with OOP syntax sugar.

But this parsing section isn't what guarantees data consistency or OOP contracts - that's all doable with native Bash syntax.

### Return a resultset from a substring query on a large number or rows, quickly

Not as quickly as SQL, but blazing fast for Bash.

This is a rare instance of needing to shell out of Bash, and using obtuse syntax in the process under the hood. But is well worth the performance tradeoff.

This example below returns a sparse array of just the matching subset - with original array indexes and values. This can be used under the hood to return a read/write (or read-only) "view" in RDBMS-speak, with a unique cursor instance. Or optionally, the raw sparse Bash array can be returned instead for more direct native access.

~~~bash
search=' (Bones|Ralph)[ ]?[0-9]+'
eval "declare -a result=($(declare -p big | grep -oP "\[\d+\]=\"(?:[^\"\\\\]|\\\\.)*${search}(?:[^\"\\\\]|\\\\.)*\""))"
~~~

### Lean on sparse arrays

The design includes many arrays. But most of them (especially Traits) are treated as sparse arrays, and so only consume memory if something is defined. (And almost all traits are optional, with defaults assumed in code, if not defined in array.)

But being "sparse", the indexes definitionally do not need to be contiguous. As long as they share the same index value with other arrays with the same named prefix, then things can be lean, fast, and organized.

## Design

### Overview

There are only a few main conceptual sets of arrays (or SQL tables or JSON object arrays) for everything:

- Schema:
	- Entities (aka _classes_)
		- Traits (fields and members)
	- Attributes (aka _class members_)
		- Traits (member metadata)
- Data:
	- Rows (aka _instanced objects_)
		- Trait overrides
	- Cells (aka _member instances_)
		- Trait overrides

There's also a set of arrays dedicated to storing and enforcing M:M entity relationships.

### Array name convention

Users of this library will never need to know an array name. This is just for design reference.

The arrays defined below have specific names, that carry specific meaning.

Most arrays are index arrays. Arrays with the same prefix should be considered "attributes of the same meta-class".

("Meta-class" meaning, classes that define the schema itself. The name of the meta-class, is the common array prefix. And "attributes" meaning, fields or properties')

- __`*_UNQ*`__

	One or more parts of what usually makes a unique schema record (or object). Examples from below:

	- `Attr_UNQ_EntIdx` + `Attr_UNQ_Label`

		An Entity Idx _and_ an Attribute Label, together, define a unique Attribute.

	- `Cell_UNQ_RowIdx` + `Cell_UNQ_AttrIdx`

		A Row Idx _and_ an Attribute Idx, together, define a unique Cell.

- `*_UNQ_Label`

	This subset of the `UNQ` definition above, holds a developer-meaningful (but not necessarily "user-friendly") string value that is defined as being unique in its context. Real-world examples:

	- Entity `Ent_UNQ_Label="Files"`

	- Attributes:

		- `Attr_UNQ_Label="FileName"`
		- `Attr_UNQ_Label="MTime"` (unique entity Idx and Label value)

- __`*_LookupUNQ`__

	These are Associative arrays, used to:

	- Enforce uniqueness at the schema level. Associative arrays are by definition unique. In this case the "key" is usually a composite value, for example for Attributes: `"${EntIdx}.${Label}"`

	- Facilitate fast lookups to obtain an index value for everything else.

### Schema

#### Entities

Aka "Classes", "Tables".

~~~bash
declare -a Ent_UNQ_ParentEntIdx    ## Parent entity, if inheriting another class.
declare -a Ent_UNQ_Label           ## Human-meaningful entity name, unique among same parent.
declare -A Ent_LookupUNQ           ## Composite unique key mapped to EntIdx.
declare -a Ent_RefCount            ## Keeps count of instantiated rows/objects, for garbage collection.
~~~

##### Entity Traits

Entity Traits are sparse index arrays that share the same indexes as Ent_*[] arrays.

Few Traits will typically be populated in practice; most entities will rely on coded defaults.

Traits that are normally used to describe attributes (like access, inheritance, read-only), at the entity level, are used to apply to all attributes of all instances, unless it can be and is overidden.

~~~bash
declare -a Trait_Ent_Access            ## All attrs: public (default), private, protected
declare -a Trait_Ent_Inheritance       ## All attrs: virtual (default), final, abstract
declare -a Trait_Ent_IsReadOnly        ## 1=The entire entity, traits, and instances are read-only
declare -a Trait_Ent_FriendlyTitle     ## Optional dev helper for UIs (e.g. TUIs).
declare -a Trait_Ent_ShortDescription  ## Optional dev helper for UIs (e.g. TUIs).
declare -a Trait_Ent_HelpText          ## Optional dev helper for UIs (e.g. TUIs).
declare -a Trait_Callback_Validate     ## Optionally allows canceling a save.
declare -a Trait_Event_PreSave         ## Optional FYI, can't be canceled.
declare -a Trait_Event_PostSave        ## An optional FYI
~~~

Whether a trait can be set or not, is contextual and ideally self-explanatory. Examples:

- `IsReadOnly` can be changed from `0` to `1`, but not from `1` to `0`.

- `IsReadOnly` can be always be set to `1` for attributes and/or entity instances. But can't be changed to `0` at any child level if it is set to `1` at a ancestor level.

#### Many-to-many relationship definitions

This group of arrays helps store and enforce many-to-many relationships. (One-to-many are easy, just add some ParentIdx attribute to your entity.)

It's reasonable or at least not too uncommon for the same two entities to have more than one M:M relationship. (Though if that's common then it's a warning of potentially poor design). Hence the 'RelationshipLabel', which will be undefined most of the time.

~~~bash
declare -a MtoMdef_UNQ_LeftEntIdx
declare -a MtoMdef_UNQ_RightEntIdx
declare -a MtoMdef_UNQ_RelationshipLabel
declare -A MtoMdef_LookupUNQ
~~~

#### Attributes

Aka "Members" (e.g. "Properties", "Methods"), "Columns", or "Fields"

~~~bash
declare -a Attr_UNQ_EntIdx
declare -a Attr_UNQ_Label   ## Developer-friendly name of attribute, unique to entity
declare -A Attr_LookupUNQ   ## Composite unique key mapped to AttrIdx.
~~~

##### Attribute Traits

Attribute Traits are sparse index arrays that share the same indexes as Attr*[] arrays.

###### Traits common to data and code members

~~~bash
declare -a Trait_Attr_MemberType             ## 'data' or 'code'
declare -a Trait_Attr_Access                 ## public (default), private, protected
declare -a Trait_Attr_Inheritance            ## virtual (default), final, abstract
declare -a Trait_Attr_IsStatic               ## 1=Only available at class level
declare -a Trait_Attr_IsReadOnly             ## 1=The attr is read-only (default for methods)
~~~

###### Traits specific to data members: attributes, property setters, and fields

Validation, callbacks, and events are processed in the order listed here.

~~~bash
declare -a Trait_Attr_StaticValue            ## Where class-level "static" values live (fields and props)
declare -a Trait_Attr_IsWORM                 ## Write-Once, Read Many
declare -a Trait_Attr_DataType               ## bool, int, float, str, datetime, base64u, any
declare -a Trait_Attr_Callback_Sanitize      ## Optional code to strip input of formatting.
declare -a Trait_Attr_IsRequired             ##
declare -a Trait_Attr_CanBeNull              ##
declare -a Trait_Attr_IsNull                 ##
declare -a Trait_Attr_DefaultVal             ##
declare -a Trait_Attr_CanBeEmpty             ##
declare -a Trait_Attr_MinChars               ## It's valid for CanBeEmpty=1 and MaxChars>0.
declare -a Trait_Attr_MaxChars               ##
declare -a Trait_Attr_MinVal                 ##
declare -a Trait_Attr_MaxVal                 ##
declare -a Trait_Attr_ValidRegex             ## Evaluated via `grep -Pq "..."` subshell
declare -a Trait_Attr_Callback_Validate      ## Optional extra validation, allows canceling.
declare -a Trait_Attr_Callback_Format        ## Formats input for output.
declare -a Trait_Attr_Value_Formatted        ## Read-only externally
declare -a Trait_Attr_Event_Changed          ## FYI, can't be canceled.
declare -a Trait_Attr_FriendlyTitle          ## Optional dev helper for UIs (e.g. TUIs).
declare -a Trait_Attr_ShortDescription       ## Optional dev helper for UIs (e.g. TUIs).
declare -a Trait_Attr_HelpText               ## Optional dev helper for UIs (e.g. TUIs).
~~~

Callbacks are invoked to provide the opportunity _change_ data and/or cancel an action before it happens.

Events are invoked to _inform_ the programmer that something happened.

###### Traits specific to code members: methods, property getters and setters, and events

~~~bash
declare -a Trait_Attr_CodeType               ## method, property, callback, event
declare -a Trait_Attr_Function               ## Name of the function to call
declare -a Trait_Attr_Function_PropSetter
~~~

Property setters, getters, callbacks, and events all have predefined interfaces, that user code must observe. This is pretty standard in languages, except that the editor and/or linter remind the user of the rules, and the compiler enforces them. (Methods are typically user-defined, observed here too.)

`Trait_Attr_Function_PropSetter` will only be populated, if the attribute is a property. The setter function is defined in `Trait_Attr_Function`. Otherwise the `Trait_Attr_Function_PropSetter` sparse array is...sparse. There is no "perfectly clean" way to provide getter and setter functions in bash, without some tradeoff somewhere. (While also enforcing unique member names.) This "redundancy" is arguably the least worst.

#### Unique constraint definitions

Each entity can have any number of unique constraints (though ideally just one).

~~~bash
declare -a UniqDef_UNQ_EntIdx
declare -a UniqDef_UNQ_AttrIdxs    ## The attributes involved in the unique constraint.
declare -A UniqDef_LookupUNQ       ## Composite unique key mapped to UnqDefIdx.
~~~

### Instanced data

#### Rows

Aka "instances", "objects", "records".

Unique constraints are managed by `Uniq_*` (and the library code utilizing it).

This may not look like much for a row definition but it gives us the only things we care about:

1. The entity in belongs to,
1. A RowIdx to attach traits to,
1. A RowIdx to hang multiple cells off of.
1. Half of what we need to enforce unique constraints. (The rest coming from the definition itself, and the cell values.)
1. Everything we need to know to track M:M relationships.

~~~bash
declare -a Row_EntIdx
~~~

##### Row Trait overrides - aka class static member overrides

~~~bash
declare -a Row_EntTraitOverride_UNQ_RowIdx
declare -A Row_EntTraitOverride_UNQ_TraitIdx
declare -A Row_EntTraitOverride_LookupUNQ          ## Composite unique key mapped to Row_EntTraitOverrideIdx.
declare -a Row_EntTraitOverride_Access             ## public (default), private, protected
declare -a Row_EntTraitOverride_Inheritance        ## final, overridden
declare -a Row_EntTraitOverride_IsReadOnly         ## 1=All cells and traits are read-only
declare -a Row_EntTraitOverride_Callback_Validate  ## Optionally allows canceling a save.
declare -a Row_EntTraitOverride_Event_PreSave      ## Optional FYI, can't be canceled.
declare -a Row_EntTraitOverride_Event_PostSave     ## An optional FYI
~~~

#### Cells

Aka "row.column", "record.field", "object.property", or "object.field".

~~~bash
declare -a Cell_UNQ_RowIdx
declare -a Cell_UNQ_AttrIdx
declare -A Cell_LookupUNQ
~~~

##### Cell Trait overrides - aka class member overrides

###### Traits overrides common to data and code members

~~~bash
declare -a Cell_AttrTraitOverride_Access                 ## public (default), private, protected
declare -a Cell_AttrTraitOverride_Inheritance            ## final, overridden
declare -a Cell_AttrTraitOverride_IsReadOnly             ## 1=read-only
~~~

###### Traits overrides specific to data members: attributes, property setters, and fields

Validation, callbacks, and events are processed in the order listed here.

~~~bash
declare -a Cell_AttrTraitOverride_IsWORM
declare -a Cell_AttrTraitOverride_Callback_Sanitize
declare -a Cell_AttrTraitOverride_IsRequired
declare -a Cell_AttrTraitOverride_CanBeNull
declare -a Cell_AttrTraitOverride_IsNull
declare -a Cell_AttrTraitOverride_DefaultVal
declare -a Cell_AttrTraitOverride_CanBeEmpty
declare -a Cell_AttrTraitOverride_MinChars
declare -a Cell_AttrTraitOverride_MaxChars
declare -a Cell_AttrTraitOverride_MinVal
declare -a Cell_AttrTraitOverride_MaxVal
declare -a Cell_AttrTraitOverride_ValidRegex
declare -a Cell_AttrTraitOverride_Callback_Validate
declare -a Cell_AttrTraitOverride_Callback_Format
declare -a Cell_AttrTraitOverride_Value_Formatted
declare -a Cell_AttrTraitOverride_Changed
declare -a Cell_AttrTraitOverride_FriendlyTitle
declare -a Cell_AttrTraitOverride_ShortDescription
declare -a Cell_AttrTraitOverride_HelpText
~~~

Callbacks are invoked to provide the opportunity _change_ data and/or cancel an action before it happens.

Events are invoked to _inform_ the programmer that something happened.

###### Traits overrides specific to code members: methods, property getters and setters, and events

~~~bash
declare -a Cell_AttrTraitOverride_CodeType               ## method, property, callback, event
declare -a Cell_AttrTraitOverride_Function               ## Name of the function to call
declare -a Cell_AttrTraitOverride_Function_PropSetter
~~~

#### Data relationships and integrity

##### Unique constraint instances

This helps enforce the defined unique constraints, in the instanced data.

~~~bash
declare -a Uniq_UNQ_UniqDefIdx  ## The unique definition Idx
declare -a Uniq_UNQ_Values      ## The values of the attributes involved in the unique constraint.
declare -A Uniq_LookupUNQ       ## Composite unique key mapped to UnqIdx.
declare -a Uniq_RowIdx          ## The specific RowIdx in question.
~~~

##### Many-to-Many entity relationship instances

~~~bash
declare -a MtoM_UNQ_LeftRowIdx
declare -a MtoM_UNQ_RightRowIdx
declare -a MtoM_UNQ_RelationshipLabel  ## A name for this overall relationship, usually undefined.
declare -A MtoM_LookupUNQ              ## This enforces the unique combination
~~~

### Function definitions by usage example

Code examples are WIP:

A user (developer) may wish to create classes, fields, properties, and methods in one or more typical '.class' files. But they can be done dynamically at runtime too, as illustrated below.

There are also two equivalent syntaxes to accomplish the same thing:

- Classic OOP `myThing = new  <thing>  <required constructor values>` syntax, and/or
- Typical "collection" object and database syntax, of `myThing = <thing>s.Add  <required constructor values>`

Use whichever one you're comfortable with, or which best fits the context.

~~~bash
## Create a new class/entity at runtime (even after .class files are loaded)
## Uning Class-style syntax
local -i class_Machine
oo  class_Machine=new Class  "Machine"

## Set one of the standard predefined properties
oo  class_Machine.FriendlyName="Generic machines"

## Add a custom class-level field at runtime
local -i field_FightSong
o  field_FightSong = new class_Machine.Field  Label="FightSong"  Value="We are machines and we will dominate."

## Set optional properties to really lock the field down, via standard properties
oo  field_FightSong.IsReadOnly=1  ## Can no longer be written to, only read.
oo  field_FightSong.IsStatic=1    ## Class-level, no instance needed to access.
oo  field_FightSong.IsFinal=1     ## Can't be overridden by subclasses.

## Create attributes ("collection"-style syntax while ignoring return values)
oo  class_Machine.Fields.Add  "SKU"
oo  class_Machine.Fields["SKU"].Sanitize = fStripNonNumbers()
oo  class_Machine.Fields["SKU"].Formatter = fMachine_Field_Formatter()
	  ## That's how the `.class` file importer would set it up, but
	  ## it could also be something generic.
oo  class_Machine.Fields.Add  Label="SerialNumber"  FriendlyName="S/N#"

## Create a method
local -i method_Temp
oo  method_Temp = new class_Machine.Method  "ShoutMyName"  fMachine_Method_ShoutMyName()
	  ## Loading functions into memory and assigning them to methods, would ordinarily be handled
	  ##   by the `.class` parser, but can also be done manually like this.
	  ## We don't HAVE to assign a return value, we can just blindly call 'class_Machine.Methods.Add'.

## Invoke fMachine_Method_ShoutMyName() via either one of:
oo  Classes["Machine"].ShoutMyName
oo  class_Machine.ShoutMyName
oo  method_Temp

## Create an instance of "Machine"
local -i objMachine1
oo  objMachine1=new class_Machine

## Set and get some data
oo  objMachine1.SKU="a123456789z"
oo  objMachine1.SerialNumber="0045678900"
oo  objMachine1.SerialNumber.IsReadOnly=1

## Garbage-collect the object
oo  objMachine1 = nothing
~~~

## The rich existing landscape of Bash OOP projects

### Common lightweight approaches

- Many if not most bash OOP projects provide the ability to instantiate any number of "objects" based on "class" definition files, with something like a factory method pattern:

	~~~bash
	obj() {

		## Args
		local -r className="$1"
		local -r uniqueInstanceName="$2"

		## Load the contents of specified class, from '.class' file.
		local classCode=$(<"${className}.class")

		## Replace dots in class code with _, in cases of objects as properties
		classCode="${classCode//./_}"  ## For

		## Replace generic '__OBJECT__' in class definition, with instance name
		## Then run the updated in-memory script, which creates the named variables.
		. <(printf '%s' "${classCode//'__OBJECT__'/"${uniqueInstanceName}"}")
	}
	~~~

	This pattern helps facilitate later on: crude encapsulation, composition, method overriding, static classes, and destructors.

- Syntactic sugar:

	- Bash supports "." dot-notation in function names, allowing a visual OO appearance. When used in the right way and consistently, it help can lend an "OOP"-feel.

	- With helper functions, you can achieve the ability to set and get properties like `myObj.Name = "Bob"`. For example, via something like:

		~~~bash
		## Generic property abstraction
		obj.Property(){
			{ [[ "$2" == "=" ]] \
				&& obj_Property[$1]="$3"; } \
				|| echo "${obj_Property[$1]}"
		}

		## Specific property getter/setter
		obj.FileName(){
			{ [[ "$1" == "=" ]] \
				&& obj.Property FileName = "$2"; } \
				|| obj.Property FileName
		}
		~~~

### Example projects on Github

- __[ba.sh](https://github.com/mnorin/ba.sh)__: "...it's not like any other OOP framework for bash you've ever seen. ba.sh is the only bash-native OOP framework with zero dependencies and zero runtime overhead. Technically it may be considered a framework and a design pattern at the same time (Metaprogramming Factory)."

- __[Bash Infinity](https://github.com/niieani/bash-oo-framework)__: "...is a standard library and a boilerplate framework for writing tools using bash. It's modular and lightweight, while managing to implement some concepts from C#, Java or JavaScript into bash. The Infinity Framework is also plug & play: include it at the beginning of your existing script to import any of the individual features such as error handling, and start using other features gradually.

- __[Object.sh](https://github.com/bgeschka/objectsh/blob/master/README.md)__: "PoC for Posix shell scripts with objects in ~66 lines. Objects with member functions; Prototypal multi-inheritance; $this, properly reflected on member functions/base classes; getters are deep, setters are shallow"

## To-do

- 20260429-150738: Rationalize references to, and discussions of: entities/attributes/fields, rows/columns/cells, and classes/objects/members.

	- Prefer: entity/attribute/cell. And annotations rather than properties.

## History

- 2026-04-27 JC: Created.
