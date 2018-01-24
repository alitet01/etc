# togglebuf - buffer for toggl

## Overview

`togglbuf` is backup/duplication command-line utility for toggl.com accounts

This application buffers between 2 toggl accounts - **src** and **dst**

It allows to separate how R&D team reports the working hours vs. how customer can view the progress of budget utilization/time spent/break down reports


## Requirements

1. Access to this repository (via ssh, using the SSH keys)
2. [pytoggle](https://github.com/ops-guru/pytoggle.git) library


## Installation

* Install toggl API client library:
        
        pip install git+ssh://git@github.com/ops-guru/pytoggle.git@master#egg=pytoggle
* Install application
        
        pip install git+ssh://git@github.com/ops-guru/togglebuf.git@master#egg=togglebuf

#### Note:

* Some platforms have `pip3` instead of `pip` and `python3` instead of `python` commands

## First time setup

1. Generate skeleton configuration file, by running:
        
        togglebuf --init
1. you can safely delete the following attributes for starters:
    * `"projects"`
    * `"clients"`


## Every time:

1. run sync to ensure all `source`'s projects, tasks and clients are present in `target`:
        
        togglebuf sync
1. (optional) dump source to json file
        
        togglebuf --start-date ${mystart} --end-date ${myend} backup

1. copy time entries by running:
        
        togglebuf --start-date ${mystart} --end-date ${myend} cpte


## Configuration

The configuration resides under the directory `~/.togglebuf` by default, and comprises the following files:
- `settings.json`: general configuration

### Configuration file JSoN schema:

```
{
    "config": {
        "version": "1.0",
        "source": {
            "api_key": "<api key>",
            "projects": {
                "include": [
                    "proj2",
                    "proj3",
                    "..."
                ],
                "exclude": [
                    "proj1",
                    "projB"
                ]
            },
            "clients": {
                "include": [
                    "clientA",
                    "clientC"
                ],
                "exclude": [
                    "clientB",
                    "clientD"
                ]
            }
        },
        "target": {
            "api_key": "<api key>",
            "raw_suffix": "_raw"
        }
    }
}
```

Please fill config file `settings.json` with actual data.
In `jq` syntax, the Mandatory fields are:

* `$.config`
* `$.config.version`
* `$.config.source`
* `$.config.source.api_key`
* `$.config.target`
* `$.config.target.api_key`
* `$.config.target.raw_suffix`

#### New version note:

If You used previous version of togglebuf:
1. Please copy Toggl Api tokens for source and target connections from `settings.json` to `settings.json`.
1. Uninstall any previously installed versions of `PyToggl`, or `pytoggl`, or `toggl`
1. Install the `pytoggle` version from the above


## Usage

1. For documentation, please consult `togglebuf -h`

### Example usages
* initialize/generate config file (`-i|--init`):
```
togglebuf --init
```

* use alternative config file location (`-c|--config`):
```
togglebuf --config /path/to/alternative/settings.json
```

* list client objects (`clients`):
```
togglebuf clients
```

* backup source objects, time entries started in date range:
```
togglebuf backup --start-date 2018-01-01 --end-date 2018-01-31
```

## Target Toggl object structure

togglebuf create two target Projects for each source one: same name and with suffix
(raw_suffix from configuration file). Tasks are copied to both Projects. Time entries
get link to suffixed projects while copying. Later some of them could be moved to
Projects with source names manually.


## Copy objects order

* Syncronize target clients, projects, tasks with source first before copy time entries.
Time entries may be linked to this objects ids. The mismatch may cause the error
`[Sync error]: no target object... for object:...`

* Wait at list 30 seconds after copying objects for changes to take effect. Then You have
to reload page to see results in web interface.

* Copy time entries dates cover the period from date_from 00:00 till date_till 23:59
according to timezone of user which token set to access source Toggl. It is better
to set Timezone UTC+00 in user profile for this user.

* Time entries without start & stop time (duration only) do not copied.

* Configuration file has options to set include and exclude lists for Clients, Projects,
Tasks to be copied. Include list working like white list and limit object names to copy.
Exclude list work as black list. Exclude list has more priority so if object name is
in both include and exclude lists it will not be copied.

#### Please use include and exclude lists carefully. There are some linked objects.
Project could be linked to client. Task usually is linked to project. Time entry
could be linked to project and to task. It may cause the copy error.

For example if some Project excluded from copying, all it's tasks must be excluded too.
The mismatch may cause the error `[Sync error]: no target object... for object:...`
All copy operations with errors will not be finished.

#### Please do not change target suffix after 1st use it.
If suffix will be changed next copy operation will add new set of Projects
with new suffix at the end.


## Backup

### Backup command action
The backup command read clients, projects, tasks and time entries from source Toggl.
Then it stores all data in json file `backup_date_from_date_till.json` in current directory.

Backup time entries dates cover the period date_from 00:00 till date_till 23:59
according to timezone of user which token set to access source Toggl. It is better
to set Timezone UTC+00 in user profile for this user.

### Current version assumes the backup scenario:
Backup period is from date_from to date_till, format: YYYY-MM-DD (year-month-date)
1. run `togglebuf backup date_from date_till`
2. go to source Toggl interface and manual download csv file from Reports-Detailed section
of Toggl with date_from date_till period set.
3. pack files `backup_date_from_date_till.json`, `Toggl_time_entries_date_from_to_date_till.csv`
into archieve `backup_date_from_date_till.tar.gz`
4. Upload the archieve to AWS S3 bucket 'toggl-backup'

* Note: at restore stage while import time entries from csv backup file Toggl will set
start/stop time using current timezone of user set in each record. Correction must be
finished manually in csv file before import to Toggl.


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
