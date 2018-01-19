# togglebuf - buffer for toggl

## Overview

`togglbuf` is backup/duplication command-line utility for toggl.com accounts

This application buffers between 2 toggl accounts - **`src`** and **`dst`**

It allows to separate how R &amp; D team reports the working hours vs. how customer can view the progress of budget utilization/time spent/break down reports

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

#### Note:

* if `--start-date` and `--end-date` options are not set the `backup` and `cpte` commands
use default period from the begin of current year till today


## Configuration

For fine tuned Configuration, please consult [CONFIGURATION](CONFIGURATION.md)

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


## Togglebuf limitations and caveats

* Togglebuf use default workspaces.
* Full Toggl data is visible only to workspace administrators.
* Toggl users cannot be copied between Toggl accounts 
* Task name must be unique in source Toggl workspace.
* Do not assign **Users** to copied Projects in target Toggl account.
* Do not rename objects (clients, projects, tasks)
* Do not delete objects if You are planning to use it later.


## Authors:

- Victor Timohin <vvt@opsguru.io>
- Max Kovgan <max@opsguru.io>
