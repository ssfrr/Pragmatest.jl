# DO NOT USE - DEVELOPMENT IS JUST BEGINNING

# Pragmatest

## The Pragmatic Unit Test Runner

[![Build Status](https://travis-ci.org/ssfrr/Pragmatest.jl.svg?branch=master)](https://travis-ci.org/ssfrr/Pragmatest.jl)

## Motivation

I wanted a testing framework that provided the following features:

* Pretty-print test results in a compact report
* Allow tests to be grouped with a useful name
* Support setup and teardown functions, and run them for each test case
* Minimize boilerplate
* Automatically find and run test files
* Print results of failed comparison
* allow natural test definition, e.g. `@test 1 < 2`
* capture STDOUT during tests and only print on test failures

At the time of writing, there are the following test frameworks for Julia:

* Base.Test
    * lacks test grouping and running features
    * only recently prints failed comparison results
* [zachallaun/FactCheck.jl] (https://github.com/zachallaun/FactCheck.jl)
    * test output is verbose, has extra newlines
    * doesn't support setup/teardown functions
    * requires tests to use special `=>` operator and define comparisons as
      functions
* [pao/QuickCheck.jl] (https://github.com/pao/QuickCheck.jl)
    * very cool, but not a general-purpose test runner
* [burrowsa/Fixtures.jl] (https://github.com/burrowsa/Fixtures.jl)
    * lots of features for handling various setup/teardown testing
    * doesn't handle printing of results or running tests
    * something feels a little overcomplicated about the fixture definition
* [burrowsa/RunTests.jl] (https://github.com/burrowsa/RunTests.jl)
    * handles pretty-printing test results
    * captures STDOUT
    * pretty awesome, I'm not 100% sure yet how it combines with Fixtures.jl
* [arypurnomoz/JulieTest.jl] (https://github.com/arypurnomoz/JulieTest.jl)
    * pretty slick (especially test output)
    * Not super sold on the `@is` macro and test syntax, it's not very natural to me
    * test grouping seems very simple and clean
    * doesn't seem to support setup/teardown
* [analyzere/UnitTest.jl] (https://github.com/analyzere/UnitTest.jl)
    * can generate xunit XML file (not important to me, but a cool feature)
    * auto-runs `test_*` functions, very simple
    * setup and teardown per-module based on function names
    * documentation is a bit sparse, and example file doesn't run
    * assertion macros (e.g. `assert_true`) seem unnecessary
* [Vera-cus/testfast.jl] (https://github.com/Vera-cus/testfast.jl)
    * very small and simple
    * supports setup/teardown, applies to each test in the file
    * very compact output
    * doesn't capture STDOUT
    * doesn't seem to separate failures and exceptions
    * doesn't print failed comparisons
* [milktrader/Jig.jl] (https://github.com/milktrader/Jig.jl)
    * not really documented
    * doesn't seem to be maintained
* [milktrader/Saute.jl] (https://github.com/milktrader/Saute.jl)
    * doesn't seem to be maintained, and is quite old and undocumented

## Test Runner

To set up the test runner, just create a file in your package called
`test/runtests.jl` that looks something like:

    import Pragmatest

    Pragmatest.run()

Pragmatest will find all files in your `test` directory of the form `Test*.jl`
and execute them as scripts, inside a context that will print out the results
in a useful report.

## Minimal Test File

The simplest test file could just be a collection of @test statements. When run with
`Pragmatest.run()` as above, the results will be collected and displayed.

    # TestSample1.jl

    @test 1 == 1
    @test 1 == 2


## Grouping Tests

You can group tests by placing them inside a Module. You can have multiple test
modules inside a single test file, or split them across separate files.

    # TestSample2.jl

    module EqualityTests

    @test 1 == 1
    @test 1 == 2

    end

    module InequalityTests

    @test 1 <= 1
    @test 1 <= 2

    end
