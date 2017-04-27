Dwarvesf uses self-host Gitlab as version control system.

Every team member should follow these conventions.

# Init project
To start a project, team leader/owner will be in charge in creating the repository and inviting related team members.

Usually, project separated into frontend(Web, iOS, Android) and backend and belongs to the same group.

For example, to start Canlead project, the Owner create the group `canlead`.

<img src="https://raw.githubusercontent.com/dwarvesf/team/master/img/add-group.png">

Then create fellow repositories:

<img src="https://raw.githubusercontent.com/dwarvesf/team/master/img/add-project.png">

Next, add members with roles:

<img src="https://raw.githubusercontent.com/dwarvesf/team/master/img/add-new-member.png">

There are few roles in Gitlab, we use:
- `Master` for who manage this project
- `Developer` for those who in active development
- `Guest` for clients on reviewing purposes.


# Setup repositories

Each repository must have those files as initialisation.

- `README.md`:

This file documents all things need to be noted in this repo. Depends on type of repo, README.md covers:

  - Requirement installation
  - How to compile and run
  - Development(architecture, how to extend new feature)
  - How to deploy

- `.gitlab/issue_templates/ISSUE_TEMPLATE.md`

We uses `issues board` for task management. Every issue must follow this template.

```
<!--
Please use the following template to submit your issue. Following this template will allow us to quickly investigate and help you with your issue. Please be aware that issues which do not conform to this template may be closed.
-->

### Status

BUG REPORT / TASK

### Checklist

Add checklist if this is a task

- [x] Add Facebook login
- [ ] Support X

### Steps to reproduce

1. First step
2. Second step
3. Third step

### Expected behaviour

How do you think the program should work? Add screenshots and code blocks if necessary.

### Actual behaviour

How does the program work in its current state?

### Environment

You may write here the specifications like the version of the project, operating system, or hardware if applicable.

### Logs / Stack trace
(Insert your log/stack trace here)
```

- `.gitlab/merge_request_templates/Feature.md`

We do use format for merge request(pull request) as well.

```
<!--
# Title
- Put [WIP] in front of the Pull Request that you are working on
- Clear [WIP] after you completed and ping the reviewer

# Body
- You have to fill out all the sections
- Put N/A if the section is nil
-->

#### Status

READY / IN DEVELOPMENT / HOLD

#### What's this PR do?

- [x] Add new routes for user
- [ ] Add instruction for using docker-compose
- [ ] ...

#### Where should the reviewer start?

#### How should this be manually tested?

#### Any background context you want to provide?

#### What are the relevant Git/YouTrack tickets?

// Put in link to Git Issue or YouTrack ticket

#### Screenshots (if appropriate)

#### Questions:

- Is there a blog post?
- Does the knowledge base need an update?
- Does this add new dependencies which need to be added to?

```

# Workflow

Dwarvesf has plenty of team members across the world, including part time and full time dwarves. To make sure the synchronization and colloboration, we has our own working flow.

At the begining of the week, team members gather together to have quick meeting. After that, a leader who control the progress of project will create a bunch of issues as current sprint tasks.

<img src="https://raw.githubusercontent.com/dwarvesf/team/master/img/issue-board.png">

To start working, each team member picks an issue, and assign to themself.

Before working on issue, estimation is a must. Developer can use spash command `/estimate` on gitlab like this:

<img src="https://raw.githubusercontent.com/dwarvesf/team/master/img/estimate.png">

When team member finish developing a feature, create a merge request and assign to the reviewer.

<img src="https://raw.githubusercontent.com/dwarvesf/team/master/img/merge-request.png">

After taking a look(maybe actual run), reviewer will give feedbacks then assign back to assignee or accept this merge request. Developers have responsibility to resolve all feedbacks.


When merge request accepted, assignee must record this ticket by `/spend` to log how much time spent on this issue.

<img src="https://raw.githubusercontent.com/dwarvesf/team/master/img/spend.png">

In development, we use [git-flow](https://github.com/nvie/gitflow) for git branching model.

There are several rules sumarized:
 * `master` and `develop` branches are protected.
 * `master` has the buildable source code always ready for production.
 * `develop` is in active development.
 * Every feature branch must has prefix `feature/`. Feature branchs only separated from develop and will be merge to develop.
 * Merge request can be merged if there are more than 2 thumbs up.

## Work in progress(WIP) merge request

If the task is taking longer than expected, assignee must provide the temporary merge request with prefix (WIP) and link to the related issue.

<img src="https://raw.githubusercontent.com/dwarvesf/team/master/img/wip-mr.png">


