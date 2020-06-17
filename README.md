[![Build Status](https://travis-ci.com/RENCI/tx-parallex.svg?branch=master)](https://travis-ci.com/RENCI/tx-parallex)

# `parallex`
## Introduction
A queue with dependencies

## Usage

```
from tx.parallex import run

ret = run(number_of_workers = 4, specf = "spec.py", dataf = "data.yml")
```

## Spec
Each task is given a dict called `data`.

### `let`
The `let` task sets `data`
```
type: let
obj: 
  <var>: <value>
  ...
  <var>: <value>
sub: <subtask>
```

### `map`
The `map` task reads a list `coll` from `data` and applies a list of subtasks to each member of the list. The members will be assigned to `var` in `data` passed to those tasks

```
type: map
coll: <variable name for collection>
var: <variable name>
sub: <subtask>
```

### `top`

The `top` task toplogically sorts subtasks. 

```
type: top
sub: <subtasks>
```

It reads the `depends_on` property of subtasks, which has format:

```
<task name>: [<param>, ..., <param>]
...
<task name>: [<param>, ..., <param>]
```
The result of a task will be assigned to the parameters that it maps to.

### `python`

The `python` task runs a python function. It reads parameters from `data`.
```
type: python
name: <name>
mod: <module>
func: <function>
params: <parameters>
depends_on: <dependencies>
ret: <returns>
```
`params` are the same format as `depends_on`

## `dsl`
A dsl block contains a subset of python.

Available syntax:

### assignment
```
<var> = <const> | <list> | <dict>
```
This translates to `let`.

### function application
```
<var> = <module>.<func>(<param>=<arg>, ...)
```
This translate to `python`.
where `<var>` is `name`

### parallel for

```
for <var> in <var>:
    ...
```
This translates to `map`.
### return
```
return <dict>
```
this translates to `ret` in `python`.


## Data

data can be arbitrary yaml

