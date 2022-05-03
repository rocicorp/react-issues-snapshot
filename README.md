# About

This repo provides tooling to be able to pull github issues utilizing multiple strategies. 
- Github Syncer 
- Clickhouse
Currently this is all the github issues for the `facebook/react` project and their associated comments.

---
# Notes on the schema

The `issues.json` file contains all the proper issues (not PRs) in `facebook/react`. But the `comments.json` contains comments from both issues _and_ PRs. So for our purposes we need to filter out those non-issue-comments when importing.

# Where it came from

This was remarkably easy to get once I figured out how.

A company called Clickhouse that makes a fast analytics database maintains a searchable index of all of GitHub as a demo.

They have a great little playground where you can do queries, here:

https://ghe.clickhouse.tech/

The schema is also described there.

To actually export the data, you need the client program, which you download here:

https://clickhouse.com/ (under "Quick Start")

Once you have it:

```bash
./clickhouse client --secure --host play.clickhouse.com --user explorer
```

```sql
SELECT * FROM github_events
    WHERE event_type = 'IssuesEvent'
        AND repo_name = 'facebook/react'
        AND action = 'opened'
    INTO OUTFILE 'issues.json'
    FORMAT JSON

SELECT * FROM github_events
    WHERE repo_name = 'facebook/react'
        AND event_type = 'IssueCommentEvent'
        AND action = 'created'
    INTO OUTFILE 'comments.json'
    FORMAT JSON
```
