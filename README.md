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

How to use MakeCI
-----------------
Most CI tools have a configuration file or directory that is normally kept in the root
directory of the repository.  We will change this configuration file so that it only
calls `makeci` instead of running your tests directly in its proprietary language.

You can call `makci` directly from your own repository by copying it there, or you can
call it from this repository by using `curl` and GitHub's raw text view.  Like this:

    `bash <(curl -s https://github.com/gtrummell/makeci/blob/master/makeci.sh)`

1. Create a `Makefile` for each component or project in your repository.  Ensure there
   is a target named `ci` that runs your tests, or calls other make targets that run
   your tests.
2. Edit your CI tool's config file.  Remove all original references to your tests and
   replace them with a single call to `makeci`
3. Commit the change.  Test by making changes in two different projects and ensuring
   they both build.

Future Design Goals for MakeCI
------------------------------
* Code a version in Golang
* Write other features (i.e. Buildkite dynamic pipelines)
