# Manual Procedures and Hidden Features



## Adding a new announcement \(sysadmin only\)

From the rails console on production: 

`Announcement.create(title:"A Nice Title", body:"The longer body with some very descriptive text. The longer body with some very descriptive text. The longer body with some very descriptive text.", created_at:"YYY-MM-DD HH:MM:SS")`

## Suppressing email announcements of fund allocations \(group admin only\)

Add a third column to the CSV upload with the word `FALSE` or `false` in it, eg:

`user@example.com, 456.78, false`







