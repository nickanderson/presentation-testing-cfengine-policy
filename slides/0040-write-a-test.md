# Writing a test

Tests will automatically run the following bundles if present:

* `init`
* `test`
* `check`
* `destroy` ($(G.testdir) is automatically cleaned up by `testall)

`inputs` in `body common control` should include the path to `default.cf.sub`,
and `bundlesequence` should be set to `default("$(this.promise_filename)")`. 

**NOTE:** Since the class `ok` is used in most tests, never create a persistent
class called `ok` in any test. Persistent classes are cleaned up between test
runs, but better safe than sorry.

Output `$(this.promise_filename) Pass` for passing and
`$(this.promise_filename) FAIL` for failing.

---
# Simple example test

    !cf3
    body common control
    {
        inputs => { "../default.cf.sub" };
        bundlesequence  => { default("$(this.promise_filename)") };
    }
    bundle agent init
    {
      files:
        "$(G.testfile)"
          delete => tidy;
    }
    bundle agent test
    {
      meta:
        "description" string => "Test that a file gets created";
      files:
        "$(G.testfile)"
          create => "true",
          classes => scoped_classes_generic("namespace", "testfile");
    }
    bundle agent check
    {
      methods:
        "" usebundle => dcs_passif( "testfile_repaired", $(this.promise_filename) );
    }

---
# Running the test

    !shell
    $ ./testall example.cf
    ======================================================================
    Testsuite started at 2016-01-31 17:06:40
    ----------------------------------------------------------------------
    Total tests: 1
    
    CRASHING_TESTS: enabled
    NETWORK_TESTS: enabled
    STAGING_TESTS: disabled
    UNSAFE_TESTS: disabled
    LIBXML2_TESTS: enabled
    
    ./example.cf Pass
    
    ======================================================================
    Testsuite finished at 2016-01-31 17:06:41 (1 seconds)
    
    Passed tests:  1
    Failed tests:  0
    Skipped tests: 0
    Soft failures: 0
    Total tests:   1

## presenter notes

Don't forget to build the agent first.

* ./autogen.sh
* make

Other ways to run a test:

* `./testall --agent=/var/cfengine/bin/cf-agent 01_vars/01_basic/sysvars.cf`
* `cf-agent -Kf --define AUTO,DEBUG ./01_vars/01_basic/sysvars.cf`


