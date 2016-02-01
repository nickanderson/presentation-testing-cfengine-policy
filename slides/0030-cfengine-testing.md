# Core Acceptance Tests

Found in [tests/acceptance](https://github.com/cfengine/core/tree/master/tests/acceptance)

* Staged
* Meta Info
* Unsafe
* Parallel/Serial
* Timed
* Fault
* Network

---
# Core Acceptance Tests - Staged
* Staged tests
    * Not expected to pass, and skipped unless running `testall` with `--staging`
    * Can be placed in staging directory (not run automatically)
    * Now preferring the use of bundle meta info to not fail the build (run automatically)
        * But do not fail the build in our CI system


---
# Core Acceptance Tests - Bundle Meta Info

  * test_skip_unsupported - Skips a test because it makes no sense on that
    platform (e.g. symbolic links on Windows).
  * test_skip_needs_work - Skips a test because the test itself is not adapted
    to the platform (even if the functionality exists).
  * test_soft_fail (usually use this one)
  * test_suppress_fail - Runs the test, but will accept failure. Use this when
    there is a real failure, but it is acceptable for the time being. This
      variable requires a meta tag on the variable set to
      "redmine<number>", where <number> is a Redmine issue number.
      There is a subtle difference between the two. Soft failures will
      not be reported as a failure in the XML output, and is
      appropriate for test cases that document incoming bug reports.
      Suppressed failures will count as a failure, but it won't block
      the build, and is appropriate for regressions or bad test
      failures.

**Note:** If you are writing an acceptance test for a (not yet fixed) bug in
  Redmine, use **test_soft_fail**.

---
# Core Acceptance Tests - Bundle Meta Info Example
    !cf3
    bundle agent test
    {
      meta:
        "test_soft_fail"
          string => "any",           # Class expression describing platforms (hard classes)
          meta => { "redmineXXX" };
    }

---
# Core Acceptance Tests - Unsafe
* Unsafe tests
    * Modify the system outside of `/tmp`
    * Should be placed in a directory named `unsafe`
    * Can be run with `--unsafe` option to `testall`

---
# Core Acceptance Tests - Parallel/Serial
* Parallel/Serial tests
    * Run `n` tests in parallel `./testall -jobs=[n]`
    * Tests with `serial` in the name are run in strict lexical order

---
# Core Acceptance Tests - Timed
* Timed
    * Allows tests to wait for extended period of time
    * Use `dcs_wait( $(this.promise_filename), <seconds>)`

## presenter notes
The test suite will keep track of time, and run other tests while your test is
waiting. Some things to look out for though:

* During the wait time, your test is no longer running, so you cannot for
  example do polling.
* You cannot leave daemons running while waiting, because it may interfere with
  other tests. If you need that you will have to wait the traditional way, by
  introducing sleeps in the policy itself.
* The timing is not guaranteed to be accurate to the second. The test will be
  resumed as soon as the current test has finished running, but if it takes a
  long time, this will add to the wait time.

---
# Core Acceptance Tests - Fault
* Fault
    * Are expected to fault, for example invalid syntax
    * Should have suffix of `.x.cf`

---
# Core Acceptance Tests - Network
* Network tests
    * Use external networked resources
    * Should be placed in a directory named '`network`'
    * Can be disaled with '`--no-network`' option to `testall`

## presenter notes
* For example url_get fetches a file from cfengine.com
