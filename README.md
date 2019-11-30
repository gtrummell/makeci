MakeCI
======
A continuous integration tool for `make`-defined pipelines in monolithic repositories

What problem does MakeCI solve?
-------------------------------
MakeCI is intended to solve a problem common to monolithic and shared repositories:
limiting CI tests to only changed components.

As of 2019, most Continuous Integration tools are designed to work with atomic
repositories.  They do not have a mechanism for determining which components of a
monolithic or shared repository have changed, and thus a means to target changes for
testing or skip unchanged files.

This behavior suggests that most CI tools will treat a monolithic or shared repository
as if it were an atomic repository.  In order to ensure that any changed component is
always tested when it is changed, these CI tools will need to run all tests for all
components on every push, or at least every pull request.  At best, this is slow and
inefficient; at worst it is risky and/or expensive.

While this problem sounds daunting, the solution is not.  Simply insert a shim into any
CI tool that compares a changed branch to a merge target, determines which components
have changes, and runs tests for them.  MakeCI implements this solution in Bash with
`git` and `make`.

How MakeCI works
----------------
MakeCI is intended to be a very small and lightweight tool with no options that does one
job.  MakeCI's workflow is as follows:
1. Compare a target branch and a changed branch with `git`.
2. From the `git` changelist, make a list of changed components.
3. Change directory to each component and run `make ci`.

Future Design Goals for MakeCI
------------------------------
* Code a version in Golang
* Write other features (i.e. Buildkite dynamic pipelines)
