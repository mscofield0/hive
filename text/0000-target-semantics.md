- Feature Name: target-semantics
- Start Date: 2021-11-26
- RFC PR: [hive/rfc#0000](https://github.com/mscofield0/hive/pull/0000)
- Hive Issue: [hive#0000](https://github.com/mscofield0/hive/issues/0000)

# Summary
[summary]: #summary

Defining target semantics inside the build system. The build system should use an isolated target system where each target and each dependency are unique (not shared, with the exception of cached targets) for each target that depends on a given target.

# Motivation
[motivation]: #motivation

It is of utmost importance that target semantics are done right, otherwise Hive might end up like Meson or CMake.

Having isolated target semantics means that we can have independent, unique targets for each target. This allows, for example, having an "app" target which consumes and depends on a target "lib" which can be configured according to the needs of the "app" target, while still permitting a target "test" which also consumes and depends on the target "lib" and configures it according to its needs. And the user does not have to touch the build configuration apart from what was declaratively defined in the targets.

With isolated targets, we also get reproducible builds for free.

The expected outcome is a self-contained project with robust configurability.

# Guide-level explanation
[guide-level-explanation]: #guide-level-explanation

## Named concepts

Introducing the following named concepts relevant in defining the features:

### Target

A target is a single unit of execution. It consists of properties and dependencies.

Properties are all options that determine how the target is configured.
Dependencies are all external targets that the current target depends on.

A target's exported properties are mutable from its dependants so that the dependant can modify the dependency according to its needs. For example, the dependant can set the dependency to be built in debug mode.

A target can be an executable, a library (shared, static or pure (no binary)) or a command target. Executable and command targets are a final stop, they cannot be used as dependencies.

Command targets can only execute a command or a series of commands. These commands can be used to call Hive. Recursive calls should be prevented.

In order to prevent redundant target generation, targets should be cached based on their _used_ configuration. The word _used_ is especially important, since targets should only ever be generated when they are invoked either as a dependency or as a standalone target. As an analogy, think of it as C++ templates, they are only generated when a specific instance of the template is created. We can think of library targets as such sort of blueprint, which only exists when instantiated. 

### Dependencies

A dependency is an external target that a particular target depends on. If a dependency exports properties, those properties are visible to the dependant and are mutable.

Circular dependencies should result in a hard error.

Dependencies should be importable from system-installed libraries, a repository or a wrap file (for non-Hive projects).

# Reference-level explanation
[reference-level-explanation]: #reference-level-explanation

## Circular dependencies

Circular dependencies should be detected by checking the dependant name in the dependency's list of dependencies.

## Library target instantiation

To support library target instantiation, we need to be able to build a configuration cache. To do that, Hive needs to hash all the configurations of a target.

## Recursive command target calls

To prevent infinite recursive calls to Hive, a command target will implicitly pass in a flag to Hive that tells it to hard error on the second next invocation of a command target.

# Drawbacks
[drawbacks]: #drawbacks

None that I can think of at the moment.

# Rationale and alternatives
[rationale-and-alternatives]: #rationale-and-alternatives

This design is superior because it's easy and clear to determine the various links between targets and how they are built together. 

What I've personally found troublesome in both CMake and Meson is that things that _could_ be automated, just aren't. It's cumbersome managing different profiles and build configurations. This design would allow automating all the things that could've been automated in the mentioned solutions.

The impact of not doing this is reinventing the wheel. There is no point in _yet-another-build-system_ if the build system does the same as everyone else.

# Prior art
[prior-art]: #prior-art

One project that was particularly interesting to me was ModernCppStarter, which had a project structure with isolated targets which all referred to the main library via the CPM.cmake wrapper over ExternalProject. This approach allowed calling targets via `cmake -Stest -Bbuild/test` which was one step in the right direction. Sadly for ModernCppStarter, CPM.cmake has a fatal flaw. Once you used CPM.cmake to fetch a dependency, this also meant that all dependants on that target fetched the dependency via CPM.cmake as well, which isn't ideal if you wanted a different way of importing the dependency. Well, that, and being restricted by CMake.

Meson is a step in the right direction, but I disagreed with some of their design choices, such as their target system and lack of build configuration automation. Though they definitely did a lot of things right. I appreciate the simple and clean syntax of the Meson build language and the Wrap dependency system. 

# Unresolved questions
[unresolved-questions]: #unresolved-questions

I expect that the wording and specifications will have to be fleshed out fully before stabilization. 

Some of the issues that are out of scope for this RFC that could be addressed in the future is the question of target and dependency implementation, along with the question of Hive's wrap dependency system.

# Future possibilities
[future-possibilities]: #future-possibilities

None at the moment.