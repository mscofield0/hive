# Hive RFCs

This branch holds the latest approved RFCs for Hive.

# Contribute

- Fork the repo
- Switch to branch rfc/
- Copy 0000-template.md to text/0000-my-feature.md (where "my-feature" is descriptive). Don't assign an RFC number yet; This is going to be the PR number and we'll rename the file accordingly if the RFC is accepted.
- Fill in the RFC. Put care into the details: RFCs that do not present convincing motivation, demonstrate lack of understanding of the design's impact, or are disingenuous about the drawbacks or alternatives tend to be poorly-received.
- Submit a pull request. As a pull request the RFC will receive design feedback from the larger community, and the author should be prepared to revise it in response. Mark the PR as an RFC.
- Now that your RFC has an open pull request, use the issue number of the PR to update your 0000- prefix to that number.
- Each pull request will be labeled with the most relevant sub-team, which will lead to its being triaged by that team in a future meeting and assigned to a member of the subteam.
- Build consensus and integrate feedback. RFCs that have broad support are much more likely to make progress than those that don't receive any comments. Feel free to reach out to the RFC assignee in particular to get help identifying stakeholders and obstacles.
- The sub-team will discuss the RFC pull request, as much as possible in the comment thread of the pull request itself. Offline discussion will be summarized on the pull request comment thread.
- RFCs rarely go through this process unchanged, especially as alternatives and drawbacks are shown. You can make edits, big and small, to the RFC to clarify or change the design, but make changes as new commits to the pull request, and leave a comment on the pull request explaining your changes. Specifically, do not squash or rebase commits after they are visible on the pull request.
- At some point, a member of the subteam will propose a "motion for final comment period" (FCP), along with a disposition for the RFC (merge, close, or postpone).
    - This step is taken when enough of the tradeoffs have been discussed that the subteam is in a position to make a decision. That does not require consensus amongst all participants in the RFC thread (which is usually impossible). However, the argument supporting the disposition on the RFC needs to have already been clearly articulated, and there should not be a strong consensus against that position outside of the subteam. Subteam members use their best judgment in taking this step, and the FCP itself ensures there is ample time and notification for stakeholders to push back if it is made prematurely.
    - For RFCs with lengthy discussion, the motion to FCP is usually preceded by a summary comment trying to lay out the current state of the discussion and major tradeoffs/points of disagreement.
    - Before actually entering FCP, all members of the subteam must sign off; this is often the point at which many subteam members first review the RFC in full depth.
- In most cases, the FCP period is quiet, and the RFC is either merged or closed. However, sometimes substantial new arguments or ideas are raised, the FCP is canceled, and the RFC goes back into development mode.
