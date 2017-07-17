Dwarvesf uses **self-host Gitlab** as version control system. We do have some conventions that every team member should follow.

# Terms
Some terms we are using in our process.

## Issue
We use issues as the smallest object in management. Issues fall into serveral kinds:

Type:

- `Feature` which converted from a user story. Feature has another name as parent issue, contains list of child issue.
- `Enhancement`is a task that can be handled independently.
- `Bug` is another kind of task but not expected upfront.

Priority:

- Critical
- High
- Medium
- Low

Status:

- Todo
- Doing
- Review
- Blocked
- Done

We use labels for categorize kind of issues.

Every issue has to follow the template below:

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

#### Materials (if appropriate)
<!--
This might come from design or the previous version of application.
-->

### Logs / Stack trace
(Insert your log/stack trace here)
```

The person who in charge creating issue can config this template at `.gitlab/merge_request_templates/Feature.md`

## Merge request

Merge or pull requests are a feature that makes it easier for a team member finish his task. When merge request created, it will be assigned to the competent that can review and give feedbacks.

Every merge request should follow the template:

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
The person who in charge creating issue can config this template at  `.gitlab/issue_templates/ISSUE_TEMPLATE.md`

If the task is taking longer than expected, assignee must provide the **temporary merge request** with prefix (WIP) and link to the related issue.

<img src="https://raw.githubusercontent.com/dwarvesf/team/master/img/wip-mr.png">

## Project

To start a project, team leader/owner will be in charge in creating the repository and inviting related team members.
We use single repository for all kinds of platforms, including frontend, backend, ios and android.

A project have to be well descripted by writing `README.md`. A good README.md should cover:
   * Requirement installation
   * How to compile and run
   * Development(architecture, how to extend new feature)
   * How to deploy

## Version
- We use [sematic version](http://semver.org/) to manage our versions.
- In development phase, version format will be 0.x.0, start from 0.1.0
- In production phase, version format will be x.y.z, start from 1.0.0
- Tag's name is version with `v` prefix.

## Milestone

Milestones are tools used in project management to mark specific points along a project timeline.

<img src="https://raw.githubusercontent.com/dwarvesf/team/master/img/milestone.png">

Milestones most of the time are used to time series marking, which mean every task, every feature must stick into one milestone.
Some points we use milestone in Dwarves Foundation:
- Milestones will be spawned corresponding to sprint time.
- Milestones name are application versions that will be released.


# Workflow

Dwarvesf has plenty of team members across the world, including part time and full time. To make sure the synchronization and collaboration, we has our own working flow.

At the very begining, team leader and project manager take a seat together, plan for these things:
- Estimate how many milestones in this project.
- Construct backlog (user stories converted into feature)

Each sprint begins with a planning meeting. During the meeting, the product owner and the development team agree upon exactly what work will be accomplished during the sprint. The development team has the final say when it comes to determining how much work can realistically be accomplished during the sprint, and the product owner has the final say on what criteria need to be met for the work to be approved and accepted.

After a sprint begins, the product owner must step back and let the team do their work. A leader who controls the progress of project will create a bunch of issues as current sprint tasks, mark them all with to-do label.


To start working, each team member picks an issue, and assign to themself.

<img src="https://raw.githubusercontent.com/dwarvesf/team/master/img/board.png">

Before working on issue, estimation is a must. Developers can use spash command **/estimate** on gitlab like this:

<img src="https://raw.githubusercontent.com/dwarvesf/team/master/img/estimate.png">

When team member finish developing a feature, create a merge request and assign to the reviewer.

<img src="https://raw.githubusercontent.com/dwarvesf/team/master/img/merge-request.png">

After taking a look(maybe actual run), reviewer will give feedbacks then assign back to assignee or accept this merge request. Developers have responsibility to resolve all feedbacks.


When merge request accepted, assignee must record this ticket by **/spend** to log how much time spent on this issue.

<img src="https://raw.githubusercontent.com/dwarvesf/team/master/img/spend.png">

In development, we use [git-flow](https://github.com/nvie/gitflow) for git branching model.

There are several rules sumarized:
 * **master** and **develop** branches are protected.
 * **master** has the buildable source code always ready for production.
 * **develop** is in active development.
 * Every feature branch must has prefix **feature/**. Feature branchs only separated from develop and will be merge to develop.
 * Merge request can be merged if there are more than 2 thumbs up.

Dwarves prefer **CLI** to **GUI** git client.
