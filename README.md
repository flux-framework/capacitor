_NOTE: flux-capacitor is being actively developed and is not yet stable._ The
github issue tracker is the primary way to communicate with the developers.

### Capacitor

Capacitor implements an aggregate job launcher and manager for
[flux](http://github.com/flux-framework), offering features similar to those
provided by job arrays or tools like
[CRAM](http://github.com/scalability-llnl/cram).

### Installing capacitor

For now, simply move the `flux-capacitor` script into your path, and use as
any other flux command.  This is very likely to be changed/expanded soon, so
keep an eye out.

### Running batches with capacitor

There are currently two ways to specify work for capacitor to execute, through
a stream of commands or a CRAM file.

#### Command stream

We use the term stream simply because it may be a file, a pipe, a socket or
any number of sources, as long as it follows the file format.  Capacitor
expects one command per line, comments (starting with #) and blanks are
ignored, where each command is treated as though it were a set of command line
arguments to capacitor itself.  For example:

```
-n 40 40-task-mpi-job
-N 4 4-node-mpi-job
```

Each of these lines is treated as a job, the first runs on 40 cores, the
second one task each on four exclusively allocated nodes.  Running a command
file is done like this:

    flux capacitor --command_file <command file>

Note that the command file can contain references to other command files, cram
files or anything else.  This can be a powerful way to reuse work, but take
care to avoid recursion!

#### CRAM file

Create a CRAM pack as usual with the CRAM tools, then while in a flux session
run:

    flux capacitor --cramfile <cram file>

Unlike CRAM, capacitor does not require enough resources to run all jobs in
the pack simultaneously, just enough to run the largest job in the pack.  Jobs
will be scheduled on available resources as they become available.
