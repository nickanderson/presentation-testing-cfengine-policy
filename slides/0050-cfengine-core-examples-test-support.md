#

<img src="../images/but_wait_theres_more.jpg">

---
# Killing 2 birds with one stone

<img src="../images/two_birds_one_stone.jpg" height=200px>

Improve documentation and testing using the test support for examples.

---
# CFEngine Core Examples with test support
* [Example with test support](https://github.com/cfengine/core/blob/master/examples/readintrealstringlist.cf)
     * Optional
       [prep](https://github.com/cfengine/core/blob/master/examples/readintrealstringlist.cf#L23#L35)
       section to prepare the environment for testing.
      * Required
       [cfengine3](https://github.com/cfengine/core/blob/master/examples/readintrealstringlist.cf#L37#L58)
       section containing policy to excercise the test
      * Required (for test support) [example_output](https://github.com/cfengine/core/blob/master/examples/readintrealstringlist.cf#L60#L72)
* [Example doc usage](https://raw.githubusercontent.com/cfengine/documentation/master/reference/functions/readintrealstringlist.markdown)
* [Example doc result](https://docs.cfengine.com/docs/3.8/reference-functions-readintrealstringlist.html)

## presenter notes

* Policies in `core/examples` that contain an `example_output` block are tested from `core/tests/acceptance/04_examples`
