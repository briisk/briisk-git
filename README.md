# Briisk git Style Guide

## Table of Contents

  1. [Naming conventions](#naming-conventions)
  1. [Pull Request](#pull-request)
  1. [Update the Pull Request](#update-pull-request)
  1. [Pull Request Review](#pull-request-review)
  1. [Pull changes from the server](#pull-changes)
  1. [Main Branches](#main-branches)
  1. [Reset](#reset)
  1. [Reflog](#reflog)

  WIP:
  1. [Work on the same branch](#work-same-branch)
  1. [Revert commits](#revert-commits)
  1. [Rebase](#rebase)
  1. [Releases](#releases)
  1. [Stash](#stash)
  1. [Cherry Pick](#cherry-pick)


## Naming conventions

  <a name="naming-conventions--branches"></a><a name="1.1"></a>
  - [1.1](#naming-conventions--branches) **Naming Conventions: Branches**: You should always create branch from the JIRA ticket (create branch link). It should follow this convention:

    `type/JIRA-Ticket-branch-name`

    **type:**
    + **bugfix:** when you are fixing bug
    + **FEATURE:** when you are implementing a whole user story
    + **feature:** when you are implementing a feature/part of the user story
    + **hotfix:** when you are fixing bug on production/staging

    **JIRA-Ticket:** ID of the JIRA ticket

    **branch-name:** beginning of the JIRA ticket title follwed with hypens

    For example:

    `git checkout -b bugfix/BB-123-comments-are-not-displaying`

    > Why? We have all important informations about feature in the branch name. It will be usefull when we will be switching between branches.

  <a name="naming-conventions--commits"></a><a name="1.2"></a>
  - [1.2](#naming-conventions--commits)  **Naming Conventions: Commits**: Commits messages should follow this convention:

    `JIRA-Ticket: commit message`

    **JIRA-Ticket:** ID of the JIRA ticket

    **commit-message:** it should be written in the present simple, it should describe what the commit is about, it can be written in technical way. Describe what your commit change, not a problem which is fixed by it.

    Banned words: 'fix', 'typo', 'whitespace'

    For example:

    ```
    JIRA ticket: "EX-01 User can not change his password"
    Bad commit message: "EX-01 fix problem with changing password"
    Good commit message: "EX-01 add missing user email to the payload of changePassword POST request"
    ```

    > Why? We have all important informations about feature in one commit. It will be helpful, when you will need to check file history, restore or revert some code etc. 'fix', 'typo', 'whitespace' words doesn't tell helpful to the developer.

**[⬆ back to top](#table-of-contents)**

## Pull Requests

  <a name="pull-request--title"></a><a name="2.1"></a>
  - [2.1](#pull-request--title) **Pull Requests: Title**: The title should describe in few words what PR is about, JIRA ticket should be at the beginning, if there is only one commit, then it can be the same as commit message.
    `BB-123: Implement comments component`

    > Why? It tells every one what this commit is about before openning PR.

  <a name="pull-request--description"></a><a name="2.2"></a>
  - [2.2](#pull-request--description) **Pull Requests: Description**: It should describe what PR is about in more detailed version than title. It can contain JIRA tickets, mentions, links, resources. The more, the better. Remember that PR is connected with JIRA ticket by integrations, so it can be found in the future.

    > Why? It tells in detailed way what the PR is about to the reviewer. Also it will be stored, so it will be useful when someone will be looking for code history.


  <a name="pull-request--merging"></a><a name="2.3"></a>
  - [2.3](#pull-request--merging) **Pull Requests: Merging**: You can merge your PR if you have at least two approvals and at least one from the developer who is working in the project (it concerns projects where at least two developers are available).

    > Why? You can make some mistakes and developer who is not in the project can't see/know everything about the project. Internal developer is more familiar with the entire project, so it will be easier to find any mistakes.

  <a name="pull-request--destination"></a><a name="2.4"></a>
  - [2.4](#pull-request--destination) **Pull Requests: Destination**: You should always create PR with functionality/bugfix to the workspace branch. If the funcitonality is not ready yet, then create feature branch and create PR to it, so there will be less code to check.

    > Why? You shouldn't create big PRs. The more code reviewer has to check, the more mistakes he/she can skip. Also there shouldn't be unfinished functionality in the main branch.

  <a name="pull-request--reviewers"></a><a name="2.5"></a>
  - [2.5](#pull-request--reviewers) **Pull Requests: Reviewers**: Add all possible developers as reviewers. The more, the better.

    > Why? They will find more issues.

  <a name="pull-request--commits"></a><a name="2.6"></a>
  - [2.6](#pull-request--commits) **Pull Requests: Commits**: There should be one commit for one JIRA ticket in the PR, if commit is related to more than one ticket, then it can contain more JIRA tickets. You can't create more than one commit for one JIRA ticket in one PR.

    > Why? Beacuse reviewers should also check commit message if it fits requirements. And for managing application it is easier to have one commit per JIRA ticket.

  <a name="pull-request--strategy"></a><a name="2.7"></a>
  - [2.7](#pull-request--strategy) **Pull Requests: Merging strategy**: Always use squash strategy for PRs which destination is workspace branch. NEVER use squash strategy for release branches. Use following convention:

    `type/JIRA-Ticket: commit message (pull request #PRNumber)`

    For example:

    `feature/DK-1: first feature (pull request #3)`

    > Why? It will be easier to maintain application with plain commits (reverting, restoring etc.). Also default message stores information about PR number.

**[⬆ back to top](#table-of-contents)**

## Update Pull Requests

  <a name="update-pull-request--commit-message"></a><a name="3.1"></a>
  - [3.1](#update-pull-request--commit-message) **Update Pull Requests: Commit message**: use git reset if you want to update only commit message

    ```
    git reset --soft HEAD~1
    git commit -m "new message"
    git push -f
    ```

    you can also use git ammend which will perform exactly the same operations with one line of code:

    ```
    git commit -m --amend "new message"
    git push -f
    ```

  <a name="update-pull-request--strategy"></a><a name="3.2"></a>
  - [3.2](#update-pull-request--strategy) **Update Pull Requests: Strategy**: Use one of the following strategies if you want to update code in the PR:

    ```
    git reset --soft HEAD~1
    git add files-to-add
    git commit -m "same or updated message"
    git push -f
    ```

    you can also use git rebase which will perform exactly the same operations:

    ```
    git add files-to-add
    git commit -m "new message"
    git rebase HEAD~2 -i
    // Now in the editor that is open, instead of pick in the second commit, write f and save it
    git push -f
    ```

**[⬆ back to top](#table-of-contents)**

## Pull Request Review

  <a name="pull-request-review--what-to-check"></a><a name="4.1"></a>
  - [4.1](#pull-request-review--what-to-check) **Pull Request Review: What to check**:

  + PR title and description
  + commit message
  + code
  + as a internal developer, check if code fits in the application from the glboal perspective
  + check if all dependencies are needed (maybe it can be done in the easier way)

  <a name="pull-request-review--how-to-write-comments"></a><a name="4.2"></a>
  - [4.2](#pull-request-review--how-to-write-comments) **Pull Request Review: How to write comments**:

  + comment code, not a person
  + attach links to the style-guide, to remind about rules
  + attach links to sites, where you found solution
  + create tasks for issues that should be fixed before merging

  <a name="pull-request-review--how-to-read-comments"></a><a name="4.3"></a>
  - [4.3](#pull-request-review--how-to-read-comments) **Pull Request Review: How to read comments**:

  + 'can' keyword means that you can implement but it's not necessary
  + 'should' keyword means that you should implement, it's necessary

**[⬆ back to top](#table-of-contents)**

## Pull changes from the server

  <a name="pull-changes--rebase"></a><a name="5.1"></a>
  - [5.1](#pull-changes--rebase) **Pull changes from the server: Rebase**:

  Always use rebase flag when you want to pull changes from the remove branch. All PRs with merge commits will be declined.

  `git pull origin development --rebase`

  > Why? To not include merge commits, which are hard to maintain.

  > Hint: Before pull, make one commit with your changes or stash them
  
  > Hint: set rebase as default pull strategy: `git config --global pull.rebase true`

**[⬆ back to top](#table-of-contents)**

## Main branches

  <a name="main-branches"></a><a name="6.1"></a>
  - [6.1](#main-branches) **Main branches** which should be in every project

  * **workspace** - main branch for developing, there should be merged every new feature/bugfix, from there we should create minor and major releases (x.x.0), history shouldn't be modified

  * **staging** - main branch for testing, there should be merged every minor and major release (x.x.0) and hotfixes, from there we should create patch releases (1.2.x), history shouldn't be modified

  * **master** - main branch for production, there should be only releases from staging or very urgent hotfixes, history can be modified (to revert changes), it shouldn't be used for developing


**[⬆ back to top](#table-of-contents)**


## Reset

  <a name="reset"></a><a name="7.1"></a>
  - [7.1](#main-branches) **Git Reset:** Reset current HEAD to the specified state [docs](https://git-scm.com/docs/git-reset)

  **Where you can reset:**

  * **HEAD~number** - number is the number of commits which you want to reset

    `git reset HEAD~2` means that you want to reset last two commits

  * **id** - means that you want to reset to a specific commit (or commit from `reflog`)

    `git reset 1234` means that you want to reset to `1234` commit


  **Flags:**

  * **--soft** - Does not touch the index file or the working tree at all (but resets the head to <commit>, just like all modes do). This leaves all your changed files "Changes to be committed", as git status would put it. (Green files in the `git status` output)

  * **--mixed** - Resets the index but not the working tree (i.e., the changed files are preserved but not marked for commit) and reports what has not been updated. This is the default action. (Red files in the `git status` output)

  * **--hard** - Resets the index and working tree. Any changes to tracked files in the working tree since <commit> are discarded. (files are removed from disc)


**[⬆ back to top](#table-of-contents)**

## Reflog

  <a name="reset"></a><a name="8.1"></a>
  - [8.1](#main-branches) **Git Reflog:** Shows history of all your git actions. [docs](https://git-scm.com/docs/git-reflog)

  **What you can do?:**

  You can reset to the specific point in the history. For example you made a mistake with git pull:

  ```
  git reflog (copy id of the commit)
  git reset commitId
  ```

  You can use `--hard` flag to remove all the files from disc.


**[⬆ back to top](#table-of-contents)**
