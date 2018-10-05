MakeCI
======
A continuous integration tool for make-defined pipelines in monolithic repositories

What problem does MakeCI solve?
-------------------------------
MakeCI is intended to solve the problem of limiting CI tests in a monolithic repository to only changed components.  As discussed in this post by blah, most Continuous Integration tools are designed to work with atomic repositories, and as such are not tuned for determining which components of a monolithic repository have changed and how to run tests only against those components.  The specific problem this behavior suggests is that the CI tool must run every test against every component for every pull request - at best inefficient, and at worst risky.

While this problem sounds daunting, the solution is not: use a simple utility to detect which components have changed, then run tests specific to those components.  MakeCI implements this solution by detecting differences between a base branch and a changed branch of a Git repository, then running `make ci` in subdirectories that represent changed components.

How MakeCI works
----------------
MakeCI is intended to be a well-behaved unix-like command line utility.  It is intended to work on its own by hand, or be called by CI tools.  MakeCI interacts with Git to detect changes, and reads standard Makefiles to find and run tests.

MakeCI's workflow is as follows:
1. Compare a base branch and a changed branch.
2. Make a list of changed components.  This should be a list of directories that contain Makefiles, child directories containing changed files.
3. Run `make ci` for each component.

Future Design Goals for MakeCI
------------------------------
* Read global configuration from Consul, Vault, etc.
* Read configuration from CLI, Config file, Environment.
* Write other configs (i.e. Buildkite dynamic pipelines)
* Configurable child Makefile handling