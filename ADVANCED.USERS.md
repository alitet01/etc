# togglebuf - advanced usage

## Togglebuf limitations

### Full Toggl data is visible only to workspace administrators.

Use tokens from administrative accounts to copy between Toggl accounts.

### Toggl users cannot be copied between Toggl accounts. 

1. Projects may have users list (team), Tasks and Time entries may have attribute User.
1. Users cannot be copied, invited only (need different e-mails)
1. The togglebuf script cannot link users to Projects-Tasks-Time_entries while copying.
1. Copied objects get following User info:
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

* Official Toggl:
    * Toggl API v8: [toggl_api.md](https://github.com/toggl/toggl_api_docs/blob/master/toggl_api.md)
    * Toggl Reports API v2: [reports.md](https://github.com/toggl/toggl_api_docs/blob/master/reports.md)
* Chosen Toggl REST python client: [pytoggl](https://github.com/jamespcole/pytoggl)
* Our Fork of that client: [pytoggle](https://github.com/ops-guru/pytoggle.git)



