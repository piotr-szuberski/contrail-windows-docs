# RFCs for team decisions
### Method for implementing group decision making.

Author: Michał Kostrzewa

---

## Abstract

The following is a meta-document describing the way of adopting RFC-style documents
as a method of participatory decision making. The document itself is proposed in
RFC style. Team members are invited to review it using the described process.

## Introduction

The discussion about race condition bug highlighted some weaknesses of our team:
* forgetting the reasons behind technical decisions made days before, which caused
repeatualy going back to already visited ideas,
* disturbing the rest of the team by having a discussion in the team room,
* making it hard for other team members to join the discussion due to lack of context.

There is some success in adopting RFC-style documents in software development teams
([Link](https://buriti.ca/6-lessons-i-learned-while-implementing-technical-rfcs-as-a-management-tool-34687dbf46cb)).
Based on the linked article, adopting this methodology would not only
fix the aforementioned problems, but also:

* enable individual team members to feel responsible for parts of the system,
* allow domain experts to have input,
* allow team members to be participate when they want, not when a meeting is scheduled.

## What is an RFC

RFC is a Request For Comments document created by IETF ([Link](https://www.ietf.org/standards/rfcs/)).
It was designed for reaching consensus on technical decisions by different engineers.

## Workflow

The following describes the proposed RFC workflow. Github repository for storing RFCs
will be provided.

### When to propose

An RFC should be proposed when:
* a new tool, component or library is to be introduced,
* a larger refactor/rewrite idea is proposed,
* a new dependency is getting added,
* a contract between two systems is being made,
* a result of a spike or technical research suggests changes to the system,
* there is doubt or controversy about the way of implementing a feature,
* there is doubt about the need of RFC.

### How to propose

1. Somebody on the Team raises the need of an RFC. The RFC card is hanged on the Kanban
board in the *To do* stage.
2. RFC Author is selected by the Team. It does not have to be a person that raised
the need for it. It does not have to be a person that has the most domain knowledge
about the issue. RFC card moves to *In progress* stage on the Kanban board.
3. RFC Author has a set amount of time in which to write an RFC document - by default, 
between one to three days. Different timeframes are allowed if agreed to by the team. 
4. Format of the document should be based on this one. Refer to 'RFC format'
chapter for details.
5. The Author submits a Pull Request to RFC repository. RFC card moves to *In review*
stage on the Kanban board.
6. All team members are selected as reviewers of this Pull Request.

### Participation

1. RFC remains open for comments for at least two days, but maximum of a week. Different
timeframes are allowed if agreed to by the team. The timebox must be noted on the RFC card.
2. All team members *can* read the RFC and provide comments in the form of GitHub reviews.
3. Participation is time-boxed. If a team member does not participate in the timeframe, 
they lose the opportunity to be included in the decision making process.
4. The Author must address the changes discussed during review as commits added to the 
Pull Request.
5. Decision is reached in a democratic vote at the end of the timeframe.If there is a
draw, or very close to a draw (+- 1 vote) in the timeframe, the Team Lead has a right
to make a final decision to reject or accept the RFC based on presented arguments.
6. The Pull Request is merged. This signifies that the RFC has been accepted. RFC card
moves to *Done* stage on the Kanban board.

### Addendums to existing RFC

Pull requests can be made to modify/maintain old RFCs. This follows the same process as 
proposing a new RFC, with the exception that the existing RFC document is modified. The 
author of the change should be added at the top of the document.

However, RFCs don't have to be maintained and kept up-to-date. They should be revisited
only if a need arises.

### RFC format

* RFC document MUST be a Markdown file.
* RFC file name MUST be the same as document title (but with spaces replaced by dashes). 
If document title is too long, it should be split into two: the title and subtitle. The 
title must fit within a reasonable-length filename.
* RFC Author's name MUST be present close to the top of the document.
* RFC formatting CAN be based off of this document.
