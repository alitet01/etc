# togglebuf - buffer for toggl

## Overview

**togglbuf** is a command-line interface for toggl.com accounts.

This application buffers between 2 toggl accounts - **src** and **dst**

It allows to separate how R&D team reports the working hours vs. how customer can view the progress of budget utilization/time spent/break down reports


## Requirements

1. access to this repository
2. pytoggl library
3. PyYAML


## Installation

* Install toggl library, branch Togglebuf_support

`pip install git+git://github.com/ops-guru/pytoggl.git@Togglebuf_support`

* Install PyYAML

`pip install pyyaml`

* Install application

`git clone git@github.com:ops-guru/togglebuf.git`

or

`download togglebuf.tar.gz from https://github.com/ops-guru/togglebuf/blob/Code_style_improvement/togglebuf.tar.gz`

#### Note:

* Some platforms have `pip3` instead of `pip` and `python3` instead of `python` commands


## Configuration

the configuration usually resides under the directory `~/.togglebuf` and comprises the following files:
- `config.yml`: general configuration

### Configuration file "config.yml" schema:

```
config:
  version: 1.0
  source:
    api_key: <api key>
    projects:
      include:
      - proj2
      - proj3
      - ...
      exclude:
      - proj1
      - projB
    clients:
      include:
      - clientA
      - clientC
      exclude:
      - clientB
      - clientD
  target:
    api_key: <api key>
    raw_suffix: '_raw'
```

Please fill file "config.yml" with actual data. Fields version, source-api_key, target-api_key
and target-raw_suffix are mandatory.

#### New version note:
If You used previous version of togglebuf
1. Please copy Toggl Api tokens for source and target connections from `settings.json` to `config.yml`.
2. Please reinstall Pytoggl library to get last version.


## Usage

1. Go to togglebuf folder
2. Check You have write access. If not set permission to write.
3. Run `python togglebuf.py [options] command`
4. Commands:

| Command | Description |
|---------|-------------|
| **clients** | show clients in both source and target toggl connections |
| **projects** | show projects |
| **tasks** | show tasks |
| **sync** | syncronize target clients, projects, tasks with source |
| **cpte date_from date_till** | copy time entries started in date range, date format: YYYY-MM-DD (year-month-date) |
| **exit** | exit interactive mode |
|---------| |
| Options | |
| **-h,--help** | print help |
| **-i,--init** | initialize/generate config file |
| **-c,--config /path/to/alternative/conf.yml** | use alternative config file |
| **-s,--shell** | interactive mode |

### Example usages
* read help (`-h|--help`):
```
togglebuf --help
```

* initialize/generate config file (`-i|--init`):
```
togglebuf --init
```

* use alternative config file location (`-c|--config`):
```
togglebuf --config /path/to/alternative/conf.yml
```

* use interactive mode (`-s|--shell`):
```
togglebuf --shell
```
Command-line interface will be opened with prompt **$>**

* list client objects (`clients`):
```
togglebuf clients
```

## Target Toggl object structure.

togglebuf create two target Projects for each source one: same name and with suffix
(raw_suffix from configuration file). All tasks and time entries get link to suffixed
projects while copying. After some of then could be moved to Projects with source
names manually.


## Copy objects order

* Syncronize target clients, projects, tasks with source first before copy time entries.
Time entries may be linked to this objects ids. The mismatch may cause the error
`[Sync error]: no target object... for object:...`

* Wait at list 30 seconds after copying objects for changes to take effect. Then to see
results in web interface You have to reload page.

* Copy time entries dates cover the period from date_from 00:00 Your time till date_till
23:59 Your time. It is better to set Timezone UTC+00 in user profile for the user
which token set to access source Toggl.

* Time entries without start & stop time (duration only) do not copied.

* Configuration file has options to set include and exclude lists for Clients, Projects,
Tasks to be copied. Include list working like white list and limit copied object names.
Exclude list work as black list. Exclude list has more priority so if object name is
in both include and exclude lists it will not be copied.

#### Please use include and exclude lists carefully. There are some linked objects.
If some Project excluded from copying, all it's tasks must be excluded too.
Same is for clients and projects. The mismatch may cause the error
`[Sync error]: no target object... for object:...`

#### Please do not change target suffix after 1st use it.
Next copy operation after suffix will be changed will add new set of Projects
with new suffix.


## Toggl limitations

### Full Toggl data is visible only to workspace administrators.

Use tokens from administrative accounts to copy between Toggl accounts.

### Toggl users cannot be copied between Toggl accounts. 

Projects may have users list (team), Tasks and Time entries may have attribute User.

Users cannot be copied, invited only (need different e-mails)

The togglebuf script cannot link users to Projects-Tasks-Time_entries while copying.

Copied objects get following User info:

* For projects Anyone.
* For tasks Anyone
* For time_entries the user, whose target Toggl token used to copy objects.

#### Please do not assign Users to copied Projects in target Toggl account.

If Users will be assigned this can cause the error
`Cannot create target entry ... User cannot access the selected project`

#### Please do not rename objects.

This will cause object duplication at next copy operation.

#### Please do not delete objects if You are planning to use it later.

Toggl use object ids but not names to link objects. New object same name
will get different id and all object links to it will be lost.


## References:

- Documentation for the Toggl API v8: <https://github.com/toggl/toggl_api_docs/blob/master/toggl_api.md>

- The Toggl Reports API v2: <https://github.com/toggl/toggl_api_docs/blob/master/reports.md>

- Toggl REST python client: <https://github.com/jamespcole/pytoggl>

## Authors:

- Victor Timohin <vvt@opsguru.io>
- Max Kovgan <max@opsguru.io>
