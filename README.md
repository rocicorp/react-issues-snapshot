This was remarkably easy to get once I figured out how.

A company called Clickhouse that makes a fast analytics database
maintains a searchable index of all of github. They have a great
little playground where you can do queries, here:

https://ghe.clickhouse.tech/

The schema is also described there.

To actually export the data, you need the client program, which
you download here:

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
