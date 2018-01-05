# Togglebuf application

## Overview

**togglbuf** is a command-line interface for toggl.com accounts.

This application buffers between 2 toggl accounts - `src` and `dst`

It allows to separate how R&D team reports the working hours vs. how customer can view the progress of budget utilization/time spent/break down reports


## Requirements

1. access to this repository
2. pytoggl library


## Installation

* Install toggl library, branch Togglebuf_support
pip install git+git://github.com/ops-guru/pytoggl.git@Togglebuf_support

* Install application
git clone git@github.com:ops-guru/togglebuf.git
or
download togglebuf-master.zip from https://github.com/ops-guru/togglebuf


## Configuration

* Toggl Api tokens for source and target connections must be filled in settings.json file. 


## Usage

1. Go to togglebuf folder
2. Check You have write access if not set permission to write
3. Run python togglebuf.py

Command-line interface will be opened

4. Commands:
* **clients**                      - show clients in both source and target toggl connections
* **projects**                     - show projects
* **tasks**                        - show tasks
* **sync**                         - syncronize target clients, projects, tasks with source
* **cpte date_from date_till**     - copy time entries started in date range, date format: YYYY-MM-DD (year-month-date)
* **exit**                         - exit togglebuf

## Toggl limitations

### Toggl users cannot be copied between Toggl accounts. 

Projects may have users list (team), Tasks and Time entries may have attribute User.

Users cannot be copied, invited only (need different e-mails)

togglebuf script cannot link users to Projects-Tasks-Time_entries while copying
before they activate their accounts.

Copy objects without Users set:

* For Projects without team (Everybody in workspace can see this project mode)
Toggl API set same mode for copied Projects and shows anyone in Project list.
* For Projects with team Toggl API set for copied Projects team with one
member = the user, whose token used to copy objects.
* For Tasks Toggl API set anyone
* For Time_entries Toggl API set the user, whose token used to copy objects.

### togglebuf script do not link Users to copied objects in target Toggl account.

This can cause the error `Cannot create target entry ... User cannot access the selected project`

**Best is to use single target token for all copy operations.**

## Notes

* To see sync results in web interface You have to reload page

* Syncronize target clients, projects, tasks with source first before copy time entries
Time entries may have links to this objects ids

* Wait at list 30 seconds after copy time entries for changes to take effect

* Copy time entries dates cover the period from date_from 00:00 Your time till date_till
23:59 Your time. It is better to set Timezone UTC+00 in user profile for the user
which token set to access source Toggl.

* Please do not delete clients, projects, tasks objects. If You will create new ones
with same name links to objects will be lost because new objects will get other ids.

* Target toggl account must not have any clients, projects, tasks not present in
source account. Togglebuf use this check to avoid duplicates while sync operation

## References:

- Documentation for the Toggl API v8: <https://github.com/toggl/toggl_api_docs/blob/master/toggl_api.md>
and the Toggl Reports API v2: <https://github.com/toggl/toggl_api_docs/blob/master/reports.md>
- Toggl REST python client: <https://github.com/jamespcole/pytoggl>

## Authors:

- Victor Timohin <vvt@opsguru.io>
- Max Kovgan <max@opsguru.io>
