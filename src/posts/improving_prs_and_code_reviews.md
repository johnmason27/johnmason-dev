---
title: Improving PRs and Code Reviews
layout: default.liquid
---
#### John Mason - 12-10-2021

## Improving PRs and Code Reviews

## Break down the main task 🔨
Start with the initial JIRA ticket which should contain:
<ul class="bullet-points">
    <li>Task descriptions</li>
    <li>Technical details</li>
    <li>Testing conciderations</li>
    <li>Automation testing conciderations</li>
    <li>Definition Of Done</li>
</ul>

Breaking down the ticket into sub-tasks - think of how you can break down the JIRA task into smaller tasks so we can complete the work in smaller stages and create more frequent and smaller pull requests. 

<ul class="bullet-points">
    <li>The tasks can then be created in JIRA using the sub-task feature - they don't need to contain any information, they can just be a title which the team knows about.</li>
    <li>You can then use the sub-task issue name as the branch name for your work.</li>
    <li>We can then create our smaller PR's into a feature branch or master once the work has been completed.</li>
</ul> 

When completing the ticket in JIRA we wouldn't mark the sub-task as **NextVersion**. The main task is the only thing that should be marked as **NextVersion** once all the sub-tasks are completed. 
	
## Small atomic commits ⚛
Use seperate commits for:

<ul class="bullet-points">
    <li>New features</li>
    <li>Refactoring</li>
    <li>Bug fixes</li>
</ul> 

Avoid batching multiple changes in a single commit.  
Each commit should leave the code base in a valid state: 

<ul class="bullet-points">
    <li>Tests passing.</li>
    <li>New tests added.</li>
    <li>No silly variable names or code smells.</li>
</ul> 

Work on one sub-task at a time so you can focus on that, complete it quicker and then create a small PR for the work. 

Focused commits make it easier to create focused pull requests.

<ul class="bullet-points">
    <li>You can easily undo commits and changes if your commit contains multiple changes or code smells.</li>
</ul> 

Pull requests are now easier to review because you can view the seperate commits and with each commit containing a valid single change you can review the commits one by one.

## Frequent pushes ⏩

<ul class="bullet-points">
    <li>Frequent feedback from peers.</li>
    <li>No risk of losing work.</li>
    <li>More oppurtinities for smaller PRs.</li>
</ul> 

## Frequent small PRs 🤏

<ul class="bullet-points">
    <li>Keep them small and focused.</li>
    <li>Move refactors into their own pull requests if they are big.</li>
    <li>A pull request should take <b>max ~ 5 mins to review</b>.</li>
    <li>Reviewer should be able to keep the entire PR in their head.</li>
    <li>Have 1 <b>focus</b> per pull request.</li>
</ul> 

## Bibliography
[Small commits and pull requests | learning-notes (mistermicheels.com)](https://learning-notes.mistermicheels.com/processes-techniques/small-commits-pull-requests/)