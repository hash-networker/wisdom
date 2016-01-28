---
title: ER Warn before commit confirmed rollback
permalink: /ER_Warn_before_commit_confirmed_rollback/
---

Problem
-------

Currently, no warning is provided on the console before a "commit confirmed" rollback occurs. Busy users may accidentally forget to re-commit their configuration, resulting in an unnecessary configuration rollback.

Solution
--------

`[edit]`
`user@router# commit confirmed 5`
`Message from mgd@router on ttyp0 at 05:02 ...`
`Warning - Automatic rollback in 1 minute unless configuration is confirmed`
`Message from mgd@router on ttyp0 at 05:03 ...`
`Warning - Automatic rollback in 30 seconds unless configuration is confirmed`
`Message from mgd@router on ttyp0 at 05:03 ...`
`Warning - Automatic rollback in 10 seconds unless configuration is confirmed`
`[edit]`
`user@router# commit`

Status
------

Implemented in JUNOS ... 8.x?

[Category:Feature Requests](/Category:Feature_Requests "wikilink")