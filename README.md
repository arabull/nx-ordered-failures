# nx-ordered-failures
Sample repository to show excessive failures when using 'strictlyOrderedTargets'.

# structure

There are four projects:
* one
* two
* three
* four.

Via implicit dependencies, project "four" depends on project "two", and project "three" depends on project "one".

Each project has a single `do-stuff` target.

Project "two" will always fail.

## instructions

1. `npm i`
1. `nx run-many --all --target=do-stuff`

## expected

Only projects "two" and "four" should fail. Project "two" should fail due to its command in `workspace.json`, and "four" should fail because it depends on "two".

## actual

Projects "two" and "four" fail as expected. Project "three" also fails without even running, simply because it was slotted to run after "two" and "four". This is because "three" runs in a later stage than "two" within the tasks runner.

Using `--parallel` doesn't change anything because "three" is still in a later stage.

# summary

There is no reason "three" should fail simply because an unrelated project failed. It should run and pass/fail on its own merits.
