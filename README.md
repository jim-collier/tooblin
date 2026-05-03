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

This is a design document. There is not much code yet, other than defined arrays and syntax "definitions by example".

<!-- TOC ignore:true -->
## Table of contents

<!-- TOC -->

- [Introduction](#introduction)
- [The problem space](#the-problem-space)
	- [Finding a better balance between the opposing goals of OOP syntax sugar, and native Bash speed](#finding-a-better-balance-between-the-opposing-goals-of-oop-syntax-sugar-and-native-bash-speed)
	- [Targeting support for Bash versions released prior to 2014](#targeting-support-for-bash-versions-released-prior-to-2014)
	- [In a nutshell - hard requirements for a new Bash-OOP framework](#in-a-nutshell---hard-requirements-for-a-new-bash-oop-framework)
- [Who is this for, and why?](#who-is-this-for-and-why)
- [Who this isn't for](#who-this-isnt-for)
- [But no really...Why?](#but-no-reallywhy)
	- [Myths and realities of Bash](#myths-and-realities-of-bash)
		- [Myth: Bash is inappropriate for large tasks](#myth-bash-is-inappropriate-for-large-tasks)
		- [Myth: Bash can't be broken up into multi-person project files](#myth-bash-cant-be-broken-up-into-multi-person-project-files)
		- [Myth: Bash and Sh scripts are the same](#myth-bash-and-sh-scripts-are-the-same)
		- [Myth: Bash syntax is obtuse and arcane](#myth-bash-syntax-is-obtuse-and-arcane)
		- [Myth: Bash has no advanced editor support, linting, profiling, or live debugging](#myth-bash-has-no-advanced-editor-support-linting-profiling-or-live-debugging)
		- [Myth: There are no testing frameworks for Bash](#myth-there-are-no-testing-frameworks-for-bash)
		- [Myth: Bash is slow](#myth-bash-is-slow)
	- [Bash is already installed everywhere and has no inherent dependencies](#bash-is-already-installed-everywhere-and-has-no-inherent-dependencies)
	- [Shell scripting is its own specific domain that Bash is well-suited for, but the problems in the domain can nevertheless be complex, and/or rapidly grow in complexity unexpectedly once a project is well underway](#shell-scripting-is-its-own-specific-domain-that-bash-is-well-suited-for-but-the-problems-in-the-domain-can-nevertheless-be-complex-andor-rapidly-grow-in-complexity-unexpectedly-once-a-project-is-well-underway)
- [TOOBLIN goals](#tooblin-goals)
	- [Present boring, standard OOP syntax sugar that is immediately usable by any OOP programmer](#present-boring-standard-oop-syntax-sugar-that-is-immediately-usable-by-any-oop-programmer)
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
	- [Highly extensible for plug-ins and wrappers](#highly-extensible-for-plug-ins-and-wrappers)
- [Reference](#reference)
	- [OO and RDBMS are the same concept - separated in time, technologies and tools, targeted problems, and skillsets](#oo-and-rdbms-are-the-same-concept---separated-in-time-technologies-and-tools-targeted-problems-and-skillsets)
	- [Associative arrays](#associative-arrays)
	- [Index arrays](#index-arrays)
	- [UNQ: Unique Constraints - defines logical row and object uniqueness](#unq-unique-constraints---defines-logical-row-and-object-uniqueness)
- [How the TOOBLIN magic is done](#how-the-tooblin-magic-is-done)
	- [OOP syntax](#oop-syntax)
	- [Unique constraints and fast lookups](#unique-constraints-and-fast-lookups)
	- [Real object variables](#real-object-variables)
	- [OOP syntax sugar goodness](#oop-syntax-sugar-goodness)
	- [Return a resultset from a substring query on a large number of rows, quickly](#return-a-resultset-from-a-substring-query-on-a-large-number-of-rows-quickly)
	- [Lean on sparse arrays](#lean-on-sparse-arrays)
- [Design](#design)
	- [Overview](#overview)
	- [Array name convention](#array-name-convention)
	- [Schema](#schema)
		- [Entities](#entities)
			- [Entity Traits](#entity-traits)
		- [Attributes](#attributes)
			- [Attribute Traits](#attribute-traits)
				- [Traits common to data and code members](#traits-common-to-data-and-code-members)
				- [Traits specific to data members: attributes, property setters, and fields](#traits-specific-to-data-members-attributes-property-setters-and-fields)
				- [Traits specific to code members: methods, property getters and setters, and events](#traits-specific-to-code-members-methods-property-getters-and-setters-and-events)
		- [Data relationships and integrity definitions](#data-relationships-and-integrity-definitions)
			- [Unique constraint definitions](#unique-constraint-definitions)
			- [Many-to-many relationship definitions](#many-to-many-relationship-definitions)
	- [Instanced data](#instanced-data)
		- [Rows](#rows)
			- [Row Trait overrides - aka class overrides](#row-trait-overrides---aka-class-overrides)
			- [Views - filtered scrollable instances of rows](#views---filtered-scrollable-instances-of-rows)
		- [Cells](#cells)
			- [Cell Trait overrides - aka class member overrides](#cell-trait-overrides---aka-class-member-overrides)
				- [Traits overrides common to data and code members](#traits-overrides-common-to-data-and-code-members)
				- [Traits overrides specific to data members: attributes, property setters, and fields](#traits-overrides-specific-to-data-members-attributes-property-setters-and-fields)
				- [Traits overrides specific to code members: methods, property getters and setters, and events](#traits-overrides-specific-to-code-members-methods-property-getters-and-setters-and-events)
		- [Data relationships and integrity; instanced](#data-relationships-and-integrity-instanced)
			- [Unique constraint instances](#unique-constraint-instances)
			- [Many-to-Many entity relationship instances](#many-to-many-entity-relationship-instances)
	- [Function definitions by usage example](#function-definitions-by-usage-example)
- [The rich existing landscape of Bash-OOP projects](#the-rich-existing-landscape-of-bash-oop-projects)
	- [Common lightweight approaches](#common-lightweight-approaches)
	- [Example projects on Github](#example-projects-on-github)
- [To-do](#to-do)
- [History](#history)

<!-- /TOC -->

## Introduction

There are potentially countless repositories on github that allow the simulation of OOP in Bash. It's obviously a significant interest among many.

Many projects rely on some similar idioms, some of them discussed toward the bottom of this document.

The section [The rich existing landscape of Bash-OOP projects](#the-rich-existing-landscape-of-bash-oop-projects) below, gives an overview of some common approaches.

This project aims to find a better balance between "syntactic sugar" and "native Bash performance", by borrowing what already works, and incrementally improving on - or occasionally reinventing if all else fails - what doesn't.

It aims to accomplish pure OOP syntax and contractual integrity, with a thinner and faster layer over Bash than the others (and with alternative direct access to 100% Bash that still maintains OOP integrity) - in part by trying to dumb things down to their simplest necessary forms.

This latest spec iteration adds support for Prototypal Inheritance, direct object inheritance, and creating objects "from nothing" without a class. (When a namespace spec is introduced, it will allow JS-style object creation to be disabled - which if not intentional, can lead to accidental confusing behavior and logic errors for non-JS programmers.)

## The problem space

A fundamental truth about Object-Oriented Programming languages (and all programming languages):

- OOP in all modern languages is "syntactic sugar" on top of boring, flat data structures underneath. The translation between the elegant programming paradigms, and the inner guts, is either managed by a runtime library, or boiled away by a low-level optimizing static compiler - or somewhere in-between (as with Java and .NET).

- Bash is Turing-complete. By definition, it could be used to simulate any computer, compiler, and/or programming language. Including, with perfect fidelity, any OOP language.

The challenge though with a slow, line-by-line interpreted scripting language like Bash, is finding the right balance between the fidelity of the simulation, and performance on present-day hardware.

### Finding a better balance between the opposing goals of OOP syntax sugar, and native Bash speed

Most Bash-OOP implementations, in this author's estimation, get the balance skewed too far one way or the other. Either favoring:

- A more native, unwrapped Bash approach that requires a steep learning curve to be able to use a fully custom syntax, as required by a potentially otherwise useful OOP engine; and/or,

- Full-blown OOP syntax that programmers are immediately comfortable with - but which require complicated boilerplate and setup, and/or steep processing overhead where everything is wrapped and parsed to death. And in many cases, they seem to be more academic exercises "just because", rather than fully-featured practical solutions. (And the pot should be very careful calling things colors, on this point.)

The main problem with Bash-OOP solutions that introduce their own custom syntax - often in an effort to eliminate any syntax parsing layer - is this: __Why bother learning a whole new one-off syntax for _Bash scripting_, when you might as well put that effort into learning a new "real" language__? Or a more modern, advanced shell scripting language like [Powershell](https://github.com/PowerShell/PowerShell), [YSH](https://oils.pub/ysh.html), [Nu](https://www.nushell.sh/), [Xonsh](https://xon.sh/), or one of [countless other shell languages](https://github.com/oils-for-unix/oils/wiki/Alternative-Shells)?

- _To help answer that question of Bash replacements, here's a [system shell script language comparison](https://github.com/jim-collier/x9bash5-template/blob/main/shell_script_comparison.md) from onother project by this same author. It comes at the problem from the perspective of "I want to move away from Bash for shell scripting - what is the best replacement?", with the earnest attempt to find one. It identified no clear winner - only a few definite losers._

### Targeting support for Bash versions released prior to 2014

Bash 4.3, released in 2014 and by now included in all major Linux distros (most with 5+), supports without problem, two critical features necessary for efficient OOP modeling:

1. Associative Arrays
1. Passing variables to functions by reference.

There are arguably three main reasons for handicapping a Bash-OOP project by targeting a version of Bash that is now (in 2026) 12 years old:

1. Some Bash-OOP frameworks were written during, and for, < Bash 4.3. Even if a project has been updated to take advantage of >= 4.3 since then, it may still be stuck with its own older "API", for backward compatibility.

	__Counterpoint__: If choosing a Bash-OOP framework now, you can just pick one that requires Bash >= 4.3. All else being equal, it will generally meet the challenge better.

1. Wanting to offer universal macOS Darwin support

	...without making the arguably reasonable ask (for the user's great benefit), to upgrade to >= Bash 4.3.

	__Counterpoint__: If a user is savvy enough to be doing Bash scripting complicated enough to warrant a Bash-OOP library (which already involves its own download/installation process), then they are almost certainly capable enough to follow simple online instructions, and run a couple of Zsh commands to install Brew, install Bash 5, and make it the default shell. (As they likely have already done long ago.)

1. Aiming for POSIX-compliance, for some godforsaken reason. (POSIX doesn't even support _arrays_.) Such Bash-OOP projects are all but unusable. (There seems to be, mercifully, only one such project remaining - [clash](https://github.com/lhoursquentin/clash) - but it seems like there used to be more.) Although the POSIX standard has had minor tweaks through at least 2024, its core - and scripting limitations - were published in 1992. Such projects have _severely_ hamstrung themselves and their users, by 34 years. (As of 2026.) And usually for no good reason.

	__Counterpoint__: The only place POSIX-compliance is generally a hard requirement (besides supporting company legacy scripts for example), is in the Linux startup stage, where some scripts are run 'sourced' in `sh`, regardless of the script's shebang. But needing a Bash-OOP in the lean startup stage, could be a symptom of a bigger problem.

The only real solution to the "outdated Bash" dilemma, if you want a viable Bash-OOP solution and you're running macOS Darwin or some versions of BSD: __Get GNU Bash 5+__ (released in 2019). Or at least >= 4.3. There's just no way around it for a solid Bash-OOP solution. (This may seem like quaint advice in ten years with Bash v6 or 7.)

If you are running any updated Linux distro, you're almost certainly already good. All major distros default to at least Bash 4.3, most 5+. macOS Darwin's version of Bash is frozen at 3.2.57 due to the change to GPL v3 after that, but can be updated with Brew or MacPorts package managers.

### In a nutshell - hard requirements for a new Bash-OOP framework

The fundamental balance of any Bash-OOP library or framework, necessarily boils down to finding the right balance between the two directly competing objectives:

1. Not requiring users to learn a new syntax.

	For a framework that claims to offer Bash-OOP, from a user's perspective, a familiar OOP syntax is the easiest hurdle to overcome. And practically the most important requirement. It's a hard-sell to get someone to learn an all-new syntax... for _Bash_.

1. Reducing parsing layers and other performance overhead as much as possible. This can look like:

	- Being narrow with OOP syntax vocabulary, not being _too_ flexible (as many OOP languages are); and thus being able to make more assumptions, execute fewer dead-end namespace lookups, and generally be more efficient in the syntax parsing layer. And/or,

	- Offering an alternate access path to all the same OOP goodness, for performance-critical sections, via 100% non-wrapped native Bash syntax. (That is invariably going to be more difficult to learn, read, understand, and maintain.)

This project aims to accomplish both: Pure OOP-syntax (with narrow syntax flexibility to ease parsing load), and offering parallel alternate Bash-only paths for performance-critical sections.

## Who is this for, and why?

This is mainly targeted at the intersection of:

- Terminal users, developers, and sysadmin shell scripters who make heavy use of Bash for one-off tasks that too often grow into larger permanent tools,

- who may need to update the script ten to twenty years into the future, and don't want to deal with the hassle of getting the correct historical tooling and compiler versions (or JIT runtime) set up for a compiled program,

	- (Which may not even be possible due to system-breaking dependency problems - at least not without a container, VM, and/or Flatpak/AppImage/NixOS-derivation packages, etc.) And...

- who are also current or former OOP programmers,

- who deal often with large amounts of structured data (e.g. filesystems, filesystem metadata, media file metadata such as EXIF/XMP, etc.),

- for problems that need quite a bit more complex logic or manipulation than `grep`, `sed`, and/or `awk` can accomplish in bulk in an easy-to-accomplish manner,

- who find that they often spend much of their time writing (or copying) boilerplate script to accomplish the same kinds of heavy data-oriented tasks over and over again, and

- who need strong data relationship integrity enforcement with no extra effort - and/or strong data typing.

## Who this isn't for

- Programmers who need compiled native machine code for optimal performance and/or minimal distribution dependencies.

- Users who prefer to get shell automation tasks done with elegant compiled languages, rather than boring procedural script.

- Utility authors who can't rely on their users having `coreutils` installed on their systems, and of the right version. (Notably macOS Darwin and some BSDs.)

- Users who prefer to get shell automation tasks done with more advanced and/or JIT compiled scripting languages such as Powershell or Python (and don't mind the occasional version breakage and dependency issues - especially notorious in the latter case).

- Bash purists

- OOP purists

- RDBMS purists

- Purists

- People who hate adventure and probably also puppies.

## But no really...Why?

Fair question.

OK first let's get this out of the way...

### Myths and realities of Bash

[This blog post](https://medium.com/capital-one-tech/bashing-the-bash-replacing-shell-scripts-with-python-d8d201bc0989) somewhat hilariously tries to demonstrate that scripting system tasks in Python is superior than doing the same thing in Bash.

But it winds up sort of demonstrating the opposite pretty clearly. It starts with a short Bash script, and turns it into a comparatively absurdly complex Python script with many more lines of code. For example, just shelling out to an external program, waiting for it to finish, and retrieving its results is a difficult and cumbersome task. (As it is for most non-shell languages. That's not what they were designed for.)

Python is inarguably a superior, more elegant "language" than Bash. But better suited to task as a shell or even system scripting language? If the post is to provide the answer, I think most reasonable people (who weren't paid to program in - and apparently evangelize - Python) would answer "No".

The post also repeats many of the myths below - possibly all of them. The published date on the blog is 2017 - Bash v4.3 had been out for about three years by that point. (And all of the key features existed in 4.0 by 2009 - eight years earlier.) By 2017, most of those specific criticisms were either already false, based on old myths - or to try to most charitably steelman and not even correctly, "were only three years out of date at the time". (Or Alternatively: it's just opinions man, who cares?)

The peice also misreprensents other common Linux `coreutils` - for example `sort`, by implying that it can't sort on different and even multiple keys. Whether doing so in native Python is better or not (probably and there's no subshell involved), isn't the point. The point is the confidently asserted misinformation, stated as an assumed fact, as an aside even.

The point is not to prove some random nine year-old opinion peice "wrong". It is only presented as evidence that "pervasive common myths exist about Bash", including passionately held by apparently visible tech influencers. (And keep in mind, this author isn't even the biggest fan of Bash. I'm a veteran former OOP programmer. I like Python, love C# - and Go even more. I mean, I'm the one wanting to make Bash OOP...)

As crimes against humanity go - its pretty low on the list. Probably even forgivable without punishment, retribution, or even forced reparations.

And I'm not sure the honor of Bash needs "defending" from such slights. But here we go:

#### Myth: Bash is inappropriate for large tasks

Aka "After 100 lines of script, just switch to Python."

- __Reality__: A project in Bash can be arbitrarily large, spanning arbitrarily many files. As with any software project, it's all about organization and testing.

	There is nothing inherent about Bash that makes it any more difficult, slow, or fragile when growing in size, that any software project doesn't face. It always comes down mainly to organization, and regression and performance testing. Performance testing and profiling is important in Bash to identify and eliminate performance bottlenecks.

	If you give Bash to a non-programmer, expect inexpert and possibly sloppy, slow, and/or bug-ridden results.

	If you give Python or Rust to a non-programmer, expect inexpert and possibly sloppy, slow, and/or bug-ridden results.

	Either way, here are a few examples that objectively crush this myth:

	| Project       | # of Bash files | Lines of Bash | # of active contributors
	| :--           | --:             | --:           | --:
	| acme.sh       | 258  | 42,878 | 109
	| LinuxGSM      | 155  | 17,111 | 20
	| easy-rsa      | 6    | 6,001  | 2
	| Pi-hole       | 22   | 5,225  | 22
	| Bats-core     | 68   | 4,495  | 16

- __Why__ the myth: Possibly because _most_ Bash scripts are written by non-programmers, single-file, poorly-organized, not very maintainable, with no CI/CD, and distinctly _not_ multi-person projects?

	Either way, once any myth like this spreads, and gets echoed by well-regarded experts - it grows durable and persistent roots, with constant reinforcement via Selection Bias of sloppy, poorly-written scripts.

#### Myth: Bash can't be broken up into multi-person project files

- __Reality__: The previous point objectively disproves this.

	A single Bash project can be trivially broken up into multiple source files, that a single small script combines at runtime by way of the `source` keyword.

	Each file can each have their own independent, versioned interfaces that remain stable while the internals change. Just as with any other language.

	The challenge - as always across time and space for any software project - is how to best chop a project up for optimal "asynchronous" development. That's the tech lead's job.

#### Myth: Bash and Sh scripts are the same

- __Reality__: They don't _have_ to be remotely similar. A reasonably apt metaphor is:

	_Bash is to Sh, like C++ is to C._

	C++ is way more advanced, but you can still write C in C++.

	Users writing POSIX script in Bash without a good reason (or even knowing there are better idiomatic alternatives), is very much like someone writing C in a C++ project without a good reason (or even knowing there are better idiomatic alternatives).

- __Why__ the myth: When casual users think of a "Bash script", they are undoubtedly used to seeing arcane, crusty, crude syntax written by non-programmers - who themselves mistakenly believed that Bash shell commands have to be POSIX-compliant from 1992. (Or more likely, that's just the syntax they learned.) And certainly weren't using more advanced features and syntax of Bash >= 4.3.

#### Myth: Bash syntax is obtuse and arcane

- __Reality__: This is arguably the most subjective measure in this list. And is tough to debunk because it's too often true, even in online tutorials.

	But modern Bash supports well-structured C-like syntax, for example:

	- C-style arithmetic: `((x = a + b * c))`, `((x++))`, `((x += 5))`
	- C-style loops: `for ((i=0; i<n; i++)); do ... done`
	- Conditionals inside `((...))` and `[[...]]` use C operators, e.g. (`==`, `!=`, `<`, `&&`, `||`)
	- Inline ternary operations like `{ ((isFlagSet)) && fMyFunction "something"; } || printf "Bite me."`
	- Pointer-ish indirection via `${!var}` and/or `local -n nameref`
	- Locally-scoped variables
	- Integer variables, indexed arrays, and super-efficient associative (hashmap) arrays
	- K&R brace style on functions
	- You can put semicolons everywhere - legal even when redundant.

	- __Why__ the myth: Probably because there's a large kernel of truth: It's true for POSIX-compliant Sh scripts, in most system Bash scripts that ship with Linux, and with most online examples and instruction.

	It _can_ be obtuse and arcane, and too often is. But it doesn't have to be, and "shouldn't" be.

#### Myth: Bash has no advanced editor support, linting, profiling, or live debugging

- __Reality__: All untrue.

	Each of those things are easily accomplished, at least on Linux, in VS Code or Codium. (And surely other products and platforms.) Granted, it's not as simple as installing a single VS Code extension, but pretty close.

	With a couple of system packages and VS Plugins, you get a similar level of editor support, automatic linting, and live debugging you can with any other major language.

	Profiling can be accomplished with frameworks like [L_bash_profile](https://github.com/Kamilcuk/L_bash_profile), including visual call graphs.

- __Why__ the myth: It used to be true, comparatively speaking. But things change.

#### Myth: There are no testing frameworks for Bash

- __Reality__: [Bats](https://github.com/bats-core/bats-core) is a sophisticated TAP-compliant Bash script testing framework. There are several others.

#### Myth: Bash is slow

- __Reality__: Well OK that's not a myth. It's objectively comparatively true, _but_ ...context and perspective are important.

	"Speed" is not why people use Bash.

	Python, for example is also exceedingly slow, especially for the problem domains it's often used in. It also can't do true, uninhibited multithreading (though that's being addressed).

	Though to be fair, Python is usually used more as an orchestration layer for fast multithreaded libraries - in science, math, and data warehousing.

	Possibly because of those compiled code libraries for Python, the perception that "Python is fast and powerful" seems to be just as pervasive and misinformed, as "Bash is slow". (When in reality, Python's main "speed" advantage is it's good bindings interface to C programs.)

	In controlled testing by some- pure, simple Python is roughly 10x faster than the same simple operations in Bash. But both are dwarfed by compiled programs such as Rust or Go being up to _50 to 100x_ faster than Python.

	Nor does Bash get magically "slower" with large projects. As long as basic profiling and performance-testing is done, with the lowest-hanging fruit addressed first, "more lines of code" have no direct necessary corelation to "slower".

	In any project, "more lines of code" usually means "more unique code paths" - not necessarily "more code executed for every action". Core, critical code paths are usually just as optimal in a version 1, as a version 10.

	Things that must be addressed in any program, are important to address in Bash, too - such as unnecessarily deep loop nesting, recursion, call stacks that are too deep, using idioms that cause repeated subshells in long-running nested loops, etc.

	But there are some non-trivial, non-corner cases where Bash - or any shell scripting language - can actually be significantly _faster_ than Python. And that is, processing massive amounts of data by subshelling out to the highly optimized `grep`, `awk`, and/or `sed`. (Etc.) Or piping them together in one call. (Much like Python orchestrating C programs.) Although shell commands can also be done from Python, treating it as a shell language like that and waiting for processes to finish, is no trivial task. (Although, that is the very problem Xonsh - a Python shell interpreter - solves.)

- __Why__ the myth: Probably because:

	- It's not a myth. (But it is at least significantly faster than most other shell scripting languages, like Nu, YSH, etc.)

	- It's easy if possibly not even attractive for non-programmers to write poor Bash script, and there's a lot of it. Even in online tutorials and examples.

	- Once a non-programmer learns Bash, maybe it becomes the only hammer available to hit everything that starts looking like a nail with?

### Bash is already installed everywhere and has no inherent dependencies

Even on Windows with WSL.

And unlike Python and other scripted languages (even Powershell), it doesn't involve _dependency hell_.

There may be a minimum Bash version requirement, which can be checked for at runtime. But that's not _dependency hell_.

There may be incompatible CLI programs on the system that any shell scripting language depends on - but that's still not Python-style _dependency hell_. The minimum required versions (and/or existence of) external programs can also be checked at runtime, with explicit, easy-to-follow recommendations given on how to solve each one.

This is not a matter of nitpicky semantics. Python dependency hell with third-party packages is a well-documented disaster: `pip` vs `poetry` vs `pipenv` vs `conda` vs `uv` vs `hatch`, transitive conflicts, native-extension build failures, PEP 668's externally-managed-environment errors on modern Linux, virtualenv lifecycle management, lockfile drift, etc. These are the bane of the Python developer's existence.

### Shell scripting is its own specific domain that Bash is well-suited for, but the problems in the domain can nevertheless be complex, and/or rapidly grow in complexity unexpectedly once a project is well underway

This is really the main point, and reason TOOBLIN exists - or may hopefully someday exist. There's really no effective counter, other than, "Well if the project rapidly grew in complexity unexpectedly, maybe you should have planned better". Except, such a hypothetical response wouldn't be useful or realistic.

## TOOBLIN goals

### Present boring, standard OOP syntax sugar that is immediately usable by any OOP programmer

If the cognitive load is too high to learn a new, one-off, unfamiliar syntax just to achieve OOP-like Bash - then it's not going to have broad appeal.

Any OOP programmer that can also script in Bash, should be able to immediately pick this up without having to read pages of `readme`s. If that's not the case, it's a fail.

But _which_ OOP syntax, you might ask? As most programmers have learned, if you know one, you can essentially learn them all pretty easily. If you know two or three OOP languages, then you have probably come to understand OOP at a more fundamental level, and the specific syntax sugars use start to become irrelevant.

The TOOBLIN syntax aims to be as generic OOP as possible, and borrows heavily from C#, Java, and Kotlin.

### Provide strong OOP contracts with definitionally true and 100% complete "OOP"

This means code-level enforcement of:

- Abstraction
- Encapsulation
- Inheritance
- Polymorphism

### Support for optional more advanced OOP features

The features below aren't required to be used; everything is set up with sane defaults. Many Bash-OOP projects don't support these features at all, effectively defaulting to the simpler behavior. Features include:

- Static class members available at the class level without object instantiation
- Access modifiers: public, private, and protected members
- Inheritance control: final, virtual, abstract, overridden
- Callbacks and Events

### Strongly-typed

- It's all ultimately strings and integers in Bash of course, but data types are defined with a typical standard menu of options, and validated and treated as such. (Options: `string`, `int`, `float`, `datetime`, `base64u`, `any`.)

### Run as natively and bare-metal as possible

All of OOP's goodness - such as abstraction through private members, access control over inheritability, read-onlyness, and the RDBMS-like features including data relationship integrity and five normal forms - are doable without insane levels of wrapping, parsing, and contorting Bash into doing things it doesn't like.

Under the hood, it's 100% native with almost no subshells. It's mostly just a bunch of indexes pointing at stuff.

### Minimized wrapping and parsing after startup

All class methods, functions, fields, etc. are loaded into memory and given unique names. Class and member definitions can be in-line - in a `HEREDOC` for example - or in one or more `.class` files.

After that, the only parsing done is to provide syntax sugar.

### Leaky abstractions as a feature not a bug

Since this is a scripting language, and Bash has no OOP features baked-in - _any and all_ Bash-OOP frameworks have very leaky abstractions. Including this one.

So the goal is not to hide the leaks or pretend they don't exist. The goal is to provide strong 100% OOP with all the syntax sugar goodness - _when using the framework_ - whether or not the syntax sugar is used.

The framework core can (and will) be designed so that the leaks are a _feature_. For example, for performance-critical sections of a script (e.g. long-running nested loops), the underlying arrays and functions should be reasonably understandable and accessible, while losing minimal data integrity or OOP safety. (Just with loss of OOP syntax sugar, having to fall back to an arcane Bash-native syntax that the core itself uses.)

This is literally the same idea behind C++'s leaky abstraction of C, and both being leaky abstractions of machine-level code. C++ is _notorious_ for leaky abstractions of its compatible C roots. But separately from that, given that _all_ languages are leaky abstractions on top of machine code, C and C++ as languages provide a mechanism (in various iterations) to add machine-level assembler code into a source file. In that way, the designers chose to make that particular leakiness a feature in critical deterministic high-performance sections of code, not a bug.

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

### Highly extensible for plug-ins and wrappers

Such as:

- Arbitrary alternate back-ends such as service-based SQL servers

- Automatic ORM wrapping of existing SQL databases

- Filterable filesystem scanner and attribute mapping

- EXIF/IPTC/XMP, ID3, and/or video metadata scanner/wrapper

## Reference

### OO and RDBMS are the same concept - separated in time, technologies and tools, targeted problems, and skillsets

This fundamental truth forms the core philosophy of TOOBLIN. Consider:

| OOP term                          | RDBMS term                                            | Term used by TOOBLIN | Comments
| :--                               | :--                                                   | :--                  | :--
| Class                             | Table schema                                          | Entity
| Class member (Field, Property)    | Column                                                | Attribute | For OOP, especially Fields and Properties
| Object, Instance                   | Row, Record                                           | Row       | TOOBLIN object/record guts are more RDBMS-like
| Object member                     | Row&Column, Field                                     | Cell
| Method                            | Stored Procedure                                      | Method
| Callback                          | Trigger                                               | Callback
| Event                             | Listener                                              | Event
| Metadata, Annotations, Decorators | Constraints, Properties, Attributes, Modifiers        | Traits

As of 2026 (and for a long time prior), the concept of the __RDBMS__ has been more about large scale, well-structured data storage, management, retrieval, and consistency.

Meanwhile __OOP__ has been more about programming, code safety readability and maintenance, and to some extent easier UI integration - independent of solutions for permanent storage.

But at their core meaning, the terms and root concepts are essentially identical. As such, the technology-specific terms are used largely interchangeably in this document.

### Associative arrays

Bash associative arrays are very efficient. They use a hashtable under the hood, and store key=value pairs mapped to memory locations. It comes with two attributes important to TOOBLIN:

- For any given array, the key is always, by definition, unique. (In the same meaning that a specific index value is always unique for a given indexed array.)

- Retrieval of a value by key is exceptionally fast, in roughly O(1) time regardless of size. (Inserts get slower as data grows though, but still done in machine code, not script.)

### Index arrays

Regular index arrays are accessed via an integer index, e.g. `myArray[5]="Bob"`. This number is very important in TOOBLIN, and used everywhere under the hood.

They are treated as first-class object variables, in the runtime syntax. (Specifically, row indexes.)

### UNQ: Unique Constraints - defines logical row and object uniqueness

A "unique constraint" (labeled "`UNQ`" in the code) is/are the attribute[s] that naturally defines a unique record or object, and is the most sacrosanct concept both for RDBMS design, and for TOOBLIN.

In hierarchical data structures, this is rarely one field by itself - but usually at least a parent ID plus a child "Label".

In this design, every "table" (or "class", "entity", or "group of arrays") must have one and only one unique constraint, in addition to the common array index across a group of arrays.

A `UNQ` is usually a composite index that enforces Label uniqueness when combined with a parent. (E.g. `"${ParentIdx}.${Label}"`.)

## How the TOOBLIN magic is done

### OOP syntax

- Classes can be defined in traditional OOP style, in one or more traditional class files - with OOP decorators and attributes - but the executable portions are pure Bash.

- Runtime syntax is prefaced with `oop `. This adds the small cost of a thin layer of indirection and parsing, for the payoff of pure OOP syntax sugar.

	But as described in the section above about leaky abstractions, the library can be run with no parsing layer at all, side-by-side with syntax sugar versions. You might choose to do so because, for example, you need the speed in critical code sections such as long-running nested loops.

- Member code is loaded into memory as uniquely named Bash functions, but also run through a very thin and fast native layer. Not just so that syntax sugar can be provided, but also to maintain data consistency and strict OOP contracts even if you skip the extra syntax layer.

### Unique constraints and fast lookups

TOOBLIN uses associative arrays to:

- Enforce unique constraints - quickly and easily, by definition.

- Provide fast key lookups and relationship mapping.

These arrays, in TOOBLIN, usually have the word `UNQ` in them.

### Real object variables

In any OOP language, "object" references are just thinly-wrapped pointers or indexes.

To the user of TOOBLIN, an "object variable" is just an integer holding a reference to an array index, which identifies both a class, and an instance of it. But in true OOP-fashion, a thin layer of syntactic sugar lets us fully believe it's a real boy...I mean object variable.

### OOP syntax sugar goodness

After initial startup and optional `.class` file loading and parsing (where the most work is done to turn class structures into native Bash), the only time parsing is performed, is to help with OOP syntax sugar.

But this parsing section isn't what guarantees data consistency or OOP contracts - that's all doable with native Bash syntax.

### Return a resultset from a substring query on a large number of rows, quickly

Not as quickly as SQL, but fast for Bash.

This is a rare instance of needing to shell out of Bash, and using obtuse syntax in the process under the hood. But is well worth the performance tradeoff.

This example below returns a sparse array of just the matching subset - with original array indexes and values. This can be used under the hood to return a read/write (or read-only) "view" in RDBMS-speak, with a unique cursor instance. Or optionally, the raw sparse Bash array can be returned instead for more direct native access.

~~~bash
search=' (Bones|Ralph)[ ]?[0-9]+'
eval "declare -a result=($(declare -p big | grep -oP "\[\d+\]=\"(?:[^\"\\\\]|\\\\.)*${search}(?:[^\"\\\\]|\\\\.)*\""))"
~~~

### Lean on sparse arrays

The design includes many arrays. But most of them (especially Traits) are treated as sparse arrays, and so only consume memory if something is defined. But almost all traits are optional, with defaults assumed in code, if not defined in array - so for the most part, Trait arrays won't be very big.

By being "sparse", the indexes definitionally do not need to be contiguous. As long as they share the same index value with other arrays with the same named prefix, then things can be lean, fast, and organized.

## Design

### Overview

There are only a few main conceptual sets of arrays (or SQL tables or JSON object arrays) for everything:

- Schema:
	- Entities (aka _classes_)
		- Traits (fields and members)
	- Attributes (aka _class members_)
		- Traits (member metadata)
	- Many-to-many entity relationship definitions
	- Unique constraint definitions
- Data:
	- Rows (aka _instanced objects_)
		- Trait overrides
	- Cells (aka _member instances_)
		- Trait overrides
	- Many-to-Many entity relationship instances
	- Unique constraint instances

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
declare -a Ent_UNQ_ParentEntIdx  ## Parent entity, if inheriting another class.
declare -a Ent_UNQ_Label         ## Human-meaningful entity name, unique among same parent.
declare -A Ent_LookupUNQ         ## Composite unique key mapped to EntIdx.
declare -a Ent_RefCount          ## Keeps count of instantiated rows/objects, for garbage collection.
~~~

When the TOOBLIN library loads, it automatically creates an "Entity 0", that:

- All other entities inherit unless told otherwise, and

- Helps facilitate optional JS-style prototypal inheritance, where `EntIdx=0` serves as the original object prototype.

##### Entity Traits

Entity Traits are sparse index arrays that share the same indexes as Ent_*[] arrays.

Few Traits will typically be populated in practice; most entities will rely on coded defaults.

Traits that are normally used to describe attributes (like access, inheritance, read-only), at the entity level, are used to apply to all attributes of all instances, unless it can be and is overridden.

~~~bash
declare -a Trait_Ent_Access             ## All attrs: public (default), private, protected
declare -a Trait_Ent_Inheritance        ## All attrs: virtual (default), final, abstract
declare -a Trait_Ent_IsReadOnly         ## 1=The entire entity, traits, and instances are read-only
declare -a Trait_Ent_FriendlyTitle      ## Optional dev helper for UIs (e.g. TUIs).
declare -a Trait_Ent_ShortDescription   ## Optional dev helper for UIs (e.g. TUIs).
declare -a Trait_Ent_HelpText           ## Optional dev helper for UIs (e.g. TUIs).
declare -a Trait_Ent_Constructor        ## A method with user-defined arguments
declare -a Trait_Ent_Callback_Validate  ## Optionally allows canceling a save.
declare -a Trait_Ent_Event_PreSave      ## Optional FYI, can't be canceled.
declare -a Trait_Ent_Event_PostSave     ## An optional FYI
declare -a Trait_Ent_Destructor         ## A method with user-defined arguments
~~~

Whether a trait can be set or not, is contextual and ideally self-explanatory. Examples:

- `IsReadOnly` can be changed from `0` to `1`, but not from `1` to `0`.

<!-- CLAUDE: "a ancestor" → "an ancestor" (article agreement). -->
<!-- CLAUDE: Removed duplicated word — "can be always be set" → "can always be set". -->
- `IsReadOnly` can always be set to `1` for attributes and/or entity instances. But can't be changed to `0` at any child level if it is set to `1` at an ancestor level.

#### Attributes

Aka "Members" (e.g. "Properties", "Methods"), "Columns", or "Fields"

~~~bash
declare -a Attr_UNQ_EntIdx
declare -a Attr_UNQ_Label   ## Developer-friendly name of attribute, unique to entity
declare -A Attr_LookupUNQ   ## Composite unique key mapped to AttrIdx.
~~~

##### Attribute Traits

Like entity traits, attribute traits are sparse index arrays that share the same indexes as Attr*[] arrays.

###### Traits common to data and code members

~~~bash
declare -a Trait_Attr_MemberType   ## 'data' or 'code'
declare -a Trait_Attr_Access       ## public (default), private, protected
declare -a Trait_Attr_Inheritance  ## virtual (default), final, abstract
declare -a Trait_Attr_IsStatic     ## 1=Only available at class level
declare -a Trait_Attr_IsReadOnly   ## 1=The attr is read-only (default for methods)
~~~

###### Traits specific to data members: attributes, property setters, and fields

Validation, callbacks, and events are processed in the order listed here.

~~~bash
declare -a Trait_Attr_StaticValue        ## Where class-level "static" values live (fields and props)
declare -a Trait_Attr_IsWORM             ## Write-Once, Read Many
declare -a Trait_Attr_DataType           ## bool, int, float, str, datetime, base64u, any
declare -a Trait_Attr_Callback_Sanitize  ## Optional code to strip input of formatting.
declare -a Trait_Attr_IsRequired         ##
declare -a Trait_Attr_CanBeNull          ##
declare -a Trait_Attr_IsNull             ##
declare -a Trait_Attr_DefaultVal         ##
declare -a Trait_Attr_CanBeEmpty         ##
declare -a Trait_Attr_MinChars           ## It's valid for CanBeEmpty=1 and MaxChars>0.
declare -a Trait_Attr_MaxChars           ##
declare -a Trait_Attr_MinVal             ##
declare -a Trait_Attr_MaxVal             ##
declare -a Trait_Attr_ValidRegex         ## Evaluated via `grep -Pq "..."` subshell
declare -a Trait_Attr_Callback_Validate  ## Optional extra validation, allows canceling.
declare -a Trait_Attr_Callback_Format    ## Formats input for output.
declare -a Trait_Attr_Value_Formatted    ## Read-only externally
declare -a Trait_Attr_Event_Changed      ## FYI, can't be canceled.
declare -a Trait_Attr_FriendlyTitle      ## Optional dev helper for UIs (e.g. TUIs).
declare -a Trait_Attr_ShortDescription   ## Optional dev helper for UIs (e.g. TUIs).
declare -a Trait_Attr_HelpText           ## Optional dev helper for UIs (e.g. TUIs).
~~~

Callbacks are invoked to provide the opportunity _change_ data and/or cancel an action before it happens.

Events are invoked to _inform_ the programmer that something happened.

###### Traits specific to code members: methods, property getters and setters, and events

~~~bash
declare -a Trait_Attr_CodeType             ## method, property, callback, event
declare -a Trait_Attr_Function             ## Name of the function to call
declare -a Trait_Attr_Function_PropSetter
~~~

Property setters, getters, callbacks, and events all have predefined interfaces, that user code must observe. This is pretty standard in languages, except that the editor and/or linter remind the user of the rules, and the compiler enforces them. (Methods are typically user-defined, observed here too.)

`Trait_Attr_Function_PropSetter` will only be populated, if the attribute is a property. The setter function is defined in `Trait_Attr_Function`. Otherwise the `Trait_Attr_Function_PropSetter` sparse array is...sparse. There is no "perfectly clean" way to provide getter and setter functions in bash, without some tradeoff somewhere. (While also enforcing unique member names.) This "redundancy" is arguably the least worst.

#### Data relationships and integrity definitions

##### Unique constraint definitions

Each entity can have any number of unique constraints (though ideally just one).

~~~bash
declare -a UniqDef_UNQ_EntIdx
declare -a UniqDef_UNQ_AttrIdxs  ## The attributes involved in the unique constraint.
declare -A UniqDef_LookupUNQ     ## Composite unique key mapped to UnqDefIdx.
~~~

##### Many-to-many relationship definitions

This group of arrays helps store and enforce many-to-many relationships. (One-to-many are easy, just add some ParentIdx attribute to your entity.)

It's reasonable or at least not too uncommon for the same two entities to have more than one M:M relationship. (Though if that's common then it's a warning of potentially poor design.) Hence the 'RelationshipLabel', which will be undefined most of the time.

~~~bash
declare -a MtoMdef_UNQ_LeftEntIdx
declare -a MtoMdef_UNQ_RightEntIdx
declare -a MtoMdef_UNQ_RelationshipLabel
declare -A MtoMdef_LookupUNQ
~~~

### Instanced data

#### Rows

Aka "instances", "objects", "records".

Unique constraints are managed by `Uniq_*` (and the library code utilizing it).

~~~bash
declare -a Row_EntIdx
declare -a Row_ParentEntIdx
~~~

The addition of `Row_ParentEntIdx` allows for optional direct object inheritance (in addition to standard OOP class-based inheritance).

Furthermore, the code will facilitate creating an object "from scratch" - or more accurately from the original object prototype (ala JS), by basing it on EntIdx=0 (the original prototype).

##### Row Trait overrides - aka class overrides

~~~bash
declare -a Row_EntTraitOverride_UNQ_RowIdx
declare -A Row_EntTraitOverride_UNQ_TraitIdx
declare -A Row_EntTraitOverride_LookupUNQ          ## Composite unique key mapped to Row_EntTraitOverrideIdx.
declare -a Row_EntTraitOverride_Access             ## public (default), private, protected
declare -a Row_EntTraitOverride_Inheritance        ## final, overridden
declare -a Row_EntTraitOverride_IsReadOnly         ## 1=All cells and traits are read-only
declare -a Row_EntTraitOverride_Constructor
declare -a Row_EntTraitOverride_Callback_Validate  ## Optionally allows canceling a save.
declare -a Row_EntTraitOverride_Event_PreSave      ## Optional FYI, can't be canceled.
declare -a Row_EntTraitOverride_Event_PostSave     ## An optional FYI
declare -a Row_EntTraitOverride_Destructor
~~~

##### Views - filtered scrollable instances of rows

This is a common idiom to RDBMSes. When you create a read/write or read-only view, you get your own virtual "instance" of the result of a query (in this case a whole entity or filter of it), that points back to the underlying data. In this case you also get a "cursor" to navigate through the rows with.

~~~bash
declare -a View_UNQ_EntIdx
declare -a View_UNQ_Label      ## Unique name, auto-generated GUID
declare -A View_LookupUNQ
declare -a View_CurrentRowIdx  ## Cursor
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
declare -a Cell_AttrTraitOverride_Access       ## public (default), private, protected
declare -a Cell_AttrTraitOverride_Inheritance  ## final, overridden
declare -a Cell_AttrTraitOverride_IsReadOnly   ## 1=read-only
~~~

###### Traits overrides specific to data members: attributes, property setters, and fields

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

###### Traits overrides specific to code members: methods, property getters and setters, and events

~~~bash
declare -a Cell_AttrTraitOverride_CodeType             ## method, property, callback, event
declare -a Cell_AttrTraitOverride_Function             ## Name of the function to call
declare -a Cell_AttrTraitOverride_Function_PropSetter
~~~

#### Data relationships and integrity; instanced

##### Unique constraint instances

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

A user (developer) may wish to create classes, fields, properties, and methods in one or more typical `.class` files. But they can be done dynamically at runtime too, as illustrated below.

There are also two equivalent syntaxes to accomplish the same thing:

- Classic OOP syntax:
	- `myThing = new <thing> <required constructor values>`
- Typical "collection" object or ORM syntax:
	- `myThing = <thing>s.Add <required constructor values>`

Use whichever one you're comfortable with, or better yet - which best fits the context, because "Collections" feature heavily in this design.

There are a few "convention over configuration" standards that must be observed - to speed up the syntax sugar parsing (in part by avoid necessary dead-end searches), and keep things clearer:

- Object instances (which you'll notice are actually bash integer variables), _must_ start with lower-case.

- Class and member names, whether predefined or custom, _must_ start with upper-case.

- Function "pointers" _must_ end with `()`

For `=` assignment, it doesn't matter if there's a space or not. If it feels more Bash-native to not use spaces, go for it. But if it helps remind you that the optional syntax sugar layer is _not_ native Bash, then adding spaces with the `=` assignment might help make that clear. The examples below use both just to illustrate the point.

~~~bash
## Create a new class/entity at runtime
## (even if you used .class files at startup)
## Using Class-style syntax:
local -i class_Machine
oop class_Machine=new Class "Machine"

## Set one of the standard predefined properties
oop class_Machine.FriendlyName="Generic machines"

## Add a custom class-level field at runtime
local -i field_FightSong
oop field_FightSong = new class_Machine.Field Label="FightSong" Value="We are machines and we will dominate."

## Set optional properties to really lock the field down, via standard properties
oop field_FightSong.IsReadOnly=1  ## Can no longer be written to, only read.
oop field_FightSong.IsStatic=1    ## Class-level, no instance needed to access.
oop field_FightSong.IsFinal=1     ## Can't be overridden by subclasses.

## Create attributes ("collection"-style syntax while ignoring return values)
oop class_Machine.Fields.Add "SKU"
oop class_Machine.Fields["SKU"].Sanitize = fStripNonNumbers()
oop class_Machine.Fields["SKU"].Formatter = fMachine_Field_Formatter()
	## That's how the `.class` file importer would set it up, but
	## it could also point to a generic function.
oop class_Machine.Fields.Add Label="SerialNumber" FriendlyName="S/N#"

## Create a method
local -i method_Temp
oop method_Temp = new class_Machine.Method "ShoutMyName" fMachine_Method_ShoutMyName()
	## Loading functions into memory and assigning them to methods, would ordinarily be handled
	## by the `.class` parser at script startup, but can also be done manually like this.
	## We don't HAVE to assign a return value, we could just blindly call
	## 'class_Machine.Methods.Add' with constructor arguments.

## Invoke fMachine_Method_ShoutMyName() via either one of:
oop Classes["Machine"].ShoutMyName
oop class_Machine.ShoutMyName
oop method_Temp

## Create an instance of "Machine"
local -i objMachine1
oop objMachine1=new class_Machine

## Populate some data
oop objMachine1.SKU="a123456789z"
oop objMachine1.SerialNumber="0045678900"
oop objMachine1.SerialNumber.IsReadOnly=1

## Create a view and a cursor instance
local i myView
oop myView = new class_Machine.View

## Move through the data idiomatically
oop myView.MoveFirst
while oop myView.IsInBounds; do
	oop myView.Row.Attrs["SKU"].Value.Print
	oop myView.MoveNext
done

## Alternatively,
local -i myView_Count
oop myView_Count=myView.Count
oop myView.MoveFirst
for ((i=0; i<myView_Count; i++)); do
	oop myView.Row.Attrs["SKU"].Value.Print
	oop myView.Row[]["SKU"].Print
		## Less clear VB6-style "defaults" syntax to do the same thing
		## If implemented, would be far down the list.
		## Also probably violates the requirement of narrow syntax to avoid
		##   too many dead-end namespace lookups in syntax parsing.
	oop myView.MoveNext
done

## Garbage-collect the object
oop objMachine1 = nothing
	## This reference is now invalid, but the "object" still exists until myView is released
oop myView = nothing
	## Now they're both gone.
~~~

## The rich existing landscape of Bash-OOP projects

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
		classCode="${classCode//./_}" ## For

		## Replace generic '__OBJECT__' in class definition, with instance name
		## Then run the updated in-memory script, which creates the named variables.
		. <(printf '%s' "${classCode//'__OBJECT__'/"${uniqueInstanceName}"}")
	}
	~~~

	This pattern helps facilitate later on: crude encapsulation, composition, method overriding, static classes, and destructors.

- Syntactic sugar:

	- Bash supports "." dot-notation in function names, allowing a visual OO appearance. When used in the right way and consistently, it can lend an "OOP"-feel.

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
			&& obj.Property FileName = "$2"; }
			|| obj.Property FileName
		}
		~~~

### Example projects on Github

- __[ba.sh](https://github.com/mnorin/ba.sh)__: "...it's not like any other OOP framework for bash you've ever seen. ba.sh is the only bash-native OOP framework with zero dependencies and zero runtime overhead. Technically it may be considered a framework and a design pattern at the same time (Metaprogramming Factory)."

- __[Bash Infinity](https://github.com/niieani/bash-oo-framework)__: "...is a standard library and a boilerplate framework for writing tools using bash. It's modular and lightweight, while managing to implement some concepts from C#, Java or JavaScript into bash. The Infinity Framework is also plug & play: include it at the beginning of your existing script to import any of the individual features such as error handling, and start using other features gradually."

- __[Object.sh](https://github.com/bgeschka/objectsh/blob/master/README.md)__: "PoC for Posix shell scripts with objects in ~66 lines. Objects with member functions; Prototypal multi-inheritance; $this, properly reflected on member functions/base classes; getters are deep, setters are shallow"

## To-do

- Write some code.

## History

- 2026-04-27 JC: Created.
- 2026-05-01 JC: Github project created.
- 2026-05-02 JC:
	- Added section "But no really...Why?"
	- AI-assisted spelling, grammar, and fact-check.
	- Added `MoveNext` etc. examples.
