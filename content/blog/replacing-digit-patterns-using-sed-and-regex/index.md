---
title: Replacing digit patterns using sed and regex
date: 2020-07-01T01:45:12.435Z
description: Use this snippet to replace digit occurences in a file
---
`sed 's/v[[:digit:]]*/v5/g' file`