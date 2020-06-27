---
title: Finding all unique file types
date: 2020-06-27T08:18:45.596Z
description: Finding all unique file types recursively from a directory
---
`find . -type f | perl -ne 'print $1 if m/.([^./]+)$/' | sort -u`

``