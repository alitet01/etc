# Configuration of `togglebuf` operations

`togglebuf` holds its configuration details used by its operations in a file `settings.json`. 
By default it resides under the directory `~/.togglebuf`

# `togglebuf` configuration file JSoN schema:

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

From the above you understand that:
* you can specify source projects and clients for exclusion/inclusion.
* inclusion/exclustion rule is:
    * if nothing is included, then EVERYTHING is included
    * if nothing is excluded, then NOTHING is excluded
* it is possible to run into strange use cases when project is not excluded, but its client is, etc. `togglebuf` should be able to handle this.    

In `jq` syntax, the Mandatory fields are:

* `$.config`
* `$.config.version`
* `$.config.source`
* `$.config.source.api_key`
* `$.config.target`
* `$.config.target.api_key`
* `$.config.target.raw_suffix`


While setting up for the 1st run, please fill config file `settings.json` with actual data.