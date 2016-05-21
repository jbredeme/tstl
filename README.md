TSTL: the Template Scripting Testing Language
===========================================

TSTL is a "little" language that makes it easy to test software.  This
implementation targets Python.  There is also a (beta) Java version,
at https://github.com/flipturnapps/TSTL-Java.

TSTL produces a simple, universal interface for test generators to use
-- it essentially turns a definition of valid tests into a graph with
transitions, backtrack, replay, automatic reduction, and code coverage support.

For more details on TSTL, see the NASA Formal Methods (NFM) and
International Symposium on Software Testing and Analsysis (ISSTA) 2015
papers at http://www.cs.cmu.edu/~agroce/nfm15.pdf and
http://www.cs.cmu.edu/~agroce/issta15.pdf.

The published papers use an early version of TSTL syntax, which marks
pools and TSTL constructs with % signs.  "Modern" TSTL uses <> by
default, though if for some reason you need <> in your code (and to
prepare for a future C++ version) this can be turned off and only % supported.

Note that documentation below is preliminary.  A better guide to usage
might be to examine the examples directory and see real TSTL test
harnesses.  The generators directory includes some simple but useful
testing tools for finding bugs in systems defined in TSTL.  Only the
random tester is extensively tested and supports all TSTL features
well at this point.  Reading generators/randomtester.py provides
considerable guidance in how to (efficiently) use TSTL in a generic
testing tool, with TSTL providing an interface to the underlying
application/library to be tested.

Installation
------------


    git clone https://github.com/agroce/tstl.git
    cd tstl
    python setup.py install
    # Might need to do a sudo on the last step if not using virtualenv

The documentation below is preliminary, generated by some early
adapters of TSTL, and may not perfectly match the current version.
Use at your own risk.  Code in examples directories should be up to
date.

Using TSTL
------------

The simplest usage is to go to a directory with a .tstl file, and
type:

    tstl mytstlfile.tstl
    python <tstl-root>/generators/randomtester.py

The first line compiles mytstlfile.tstl and produces sut.py, which the
random tester will automatically import and test.  Both tstl and the
random tester take a variety of command line options, which --help
will show.

Example
-----

The easiest way to understand TSTL may be to examine
examples/AVL/avlnew.tstl (https://github.com/agroce/tstl/blob/master/examples/AVL/avlnew.tstl), which is a simple example file in the latest
language format (easier to read) (the file avlblocks.tstl has this
same harness in a different format).

If we test for 30 seconds, something like this will appear:

    ~/tstl/examples/AVL$ python ~/tstl/generators/randomtester.py --timeout 30
    Random testing using config=Config(swarmSwitch=None, verbose=False, fastQuickAnalysis=False, failedLogging=None, maxtests=-1, greedyStutter=False, exploit=None, seed=None, generalize=False, localize=False, uncaught=False, speed='FAST', internal=False, normalize=False, highLowSwarm=None, replayable=False, essentials=False, quickTests=False, coverfile='coverage.out', uniqueValuesAnalysis=False, swarm=False, ignoreprops=False, total=False, swarmLength=None, noreassign=False, profile=False, full=False, multiple=False, relax=False, swarmP=0.5, stutter=None, running=False, compareFails=False, nocover=False, swarmProbs=None, gendepth=None, quickAnalysis=False, exploitCeiling=0.1, logging=None, html=None, keep=False, depth=100, throughput=False, timeout=30, output=None, markov=None, startExploit=0)
      12 [2:0]
    -- < 2 [1:0]
    ---- < 1 [0:0] L
    ---- > 5 [0:0] L
    -- > 13 [1:-1]
    ---- > 14 [0:0] L
    set([1, 2, 5, 12, 13, 14])
    ...
      11 [2:0]
    -- < 5 [1:0]
    ---- < 1 [0:0] L
    ---- > 9 [0:0] L
    -- > 14 [1:-1]
    ---- > 18 [0:0] L
    set([1, 5, 9, 11, 14, 18])
    STOPPING TEST DUE TO TIMEOUT, TERMINATED AT LENGTH 17
    STOPPING TESTING DUE TO TIMEOUT
    80.8306709265 PERCENT COVERED
    30.0417540073 TOTAL RUNTIME
    236 EXECUTED
    23517 TOTAL TEST OPERATIONS
    10.3524413109 TIME SPENT EXECUTING TEST OPERATIONS
    0.751145362854 TIME SPENT EVALUATING GUARDS AND CHOOSING ACTIONS
    18.4323685169 TIME SPENT CHECKING PROPERTIES
    28.7848098278 TOTAL TIME SPENT RUNNING SUT
    0.179262161255 TIME SPENT RESTARTING
    0.0 TIME SPENT REDUCING TEST CASES
    224 BRANCHES COVERED
    166 STATEMENTS COVERED

Developer Info
--------------

There are no developer docs yet, which will hopefully change in the future.
The best shakedown test for tstl is to compile and run (using
generators/randomtester.py) the AVL
example.  Removing any call to the balancing function in the avl.py
code should cause TSTL to produce a failing test case.

