- Feature Name: `target-impl`
- Start Date: 2021-11-26
- RFC PR: [hive/rfc#0000](https://github.com/mscofield0/hive/pull/0000)
- Hive Issue: [hive#0000](https://github.com/mscofield0/hive/issues/0000)

# Summary
[summary]: #summary

Defines the target system, its structure and how it operates. 

# Motivation
[motivation]: #motivation

Refer to [RFC#0001](0001-target-semantics.md) for the motivation.

# Guide-level explanation
[guide-level-explanation]: #guide-level-explanation

I propose the following structure for Hive:

- **\<target-name>.hive** - Target file
- **\<name>.incl** - Arbitrary Hive include file (used to clean up target files by spreading out the functionality elsewhere)

Hive should locate all the targets in the project and be aware of them on every setup and build event. This is a requirement because a target can show up anywhere in the project's directory. Though this happens on every setup and build event, this is cheap since all Hive needs to do is determine their locations.



Explain the proposal as if it was already included in the build system and you were teaching it to another Hive user. That generally means:

- Introducing new named concepts.
- Explaining the feature largely in terms of examples.
- Explaining how Hive users should *think* about the feature, and how it should impact the way they use Hive. It should explain the impact as concretely as possible.
- If applicable, provide sample error messages, deprecation warnings, or migration guidance.
- If applicable, describe the differences between teaching this to existing and new Hive users.

# Reference-level explanation
[reference-level-explanation]: #reference-level-explanation

This is the technical portion of the RFC. Explain the design in sufficient detail that:

- Its interaction with other features is clear.
- It is reasonably clear how the feature would be implemented.
- Corner cases are dissected by example.

The section should return to the examples given in the previous section, and explain more fully how the detailed proposal makes those examples work.

# Drawbacks
[drawbacks]: #drawbacks

Why should we *not* do this?

# Rationale and alternatives
[rationale-and-alternatives]: #rationale-and-alternatives

- Why is this design the best in the space of possible designs?
- What other designs have been considered and what is the rationale for not choosing them?
- What is the impact of not doing this?

# Prior art
[prior-art]: #prior-art

Discuss prior art, both the good and the bad, in relation to this proposal.
A few examples of what this can include are:

- For build system, language, tools and proposals: Does this feature exist in other build systems and what experience have their community had?
- For community proposals: Is this done by some other community and what were their experiences with it?
- For other teams: What lessons can we learn from what other communities have done here?
- Papers: Are there any published papers or great posts that discuss this? If you have some relevant papers to refer to, this can serve as a more detailed theoretical background.

This section is intended to encourage you as an author to think about the lessons from other languages, provide readers of your RFC with a fuller picture.
If there is no prior art, that is fine - your ideas are interesting to us whether they are brand new or if it is an adaptation from other languages.

Note that while precedent set by other build systems is some motivation, it does not on its own motivate an RFC.
Please also take into consideration that Hive sometimes intentionally diverges from common language features.

# Unresolved questions
[unresolved-questions]: #unresolved-questions

- What parts of the design do you expect to resolve through the RFC process before this gets merged?
- What parts of the design do you expect to resolve through the implementation of this feature before stabilization?
- What related issues do you consider out of scope for this RFC that could be addressed in the future independently of the solution that comes out of this RFC?

# Future possibilities
[future-possibilities]: #future-possibilities

Think about what the natural extension and evolution of your proposal would
be and how it would affect the language and project as a whole in a holistic
way. Try to use this section as a tool to more fully consider all possible
interactions with the project and language in your proposal.
Also consider how this all fits into the roadmap for the project
and of the relevant sub-team.

This is also a good place to "dump ideas", if they are out of scope for the
RFC you are writing but otherwise related.

If you have tried and cannot think of any future possibilities,
you may simply state that you cannot think of anything.

Note that having something written down in the future-possibilities section
is not a reason to accept the current or a future RFC; such notes should be
in the section on motivation or rationale in this or subsequent RFCs.
The section merely provides additional information.
