elasticsearch-river-github
==========================

Elasticsearch river for GitHub events and issues. Gets and stores all the events the GitHub API
provides for a given repo, as well as all of the repository's issues.
Works for private repos as well if you provide authentication.

You can read more about what data is available [here](http://developer.github.com/v3/activity/events/).

##Easy install

Assuming you have elasticsearch's `bin` folder in your `PATH`:

```
plugin -i com.ubervu/elasticsearch-river-github/1.1.0
```

##Adding the river

```
curl -XPUT localhost:9200/_river/gh_river/_meta -d '{
    "type": "github",
    "github": {
        "owner": "gabrielfalcao",
        "repository": "lettuce",
        "interval": 3600,
        "authentication": {
            "username": "MYUSER",
            "password": "MYPASSWORD"
        }
    }
}'
```

Interval is given in seconds and it changes how often the river looks for new data.

The authentication bit is optional.

##Deleting the river

```
curl -XDELETE localhost:9200/_river/gh_river
```

##Indexes and types

The data will be stored in an index of format "%s-%s" % (owner, repo), i.e.
`gabrielfalcao-lettuce`.

For every API event type, there will be an elasticsearch type of the same name -
i.e. `ForkEvent`.

Issue data will be stored in the `IssueData` type.
