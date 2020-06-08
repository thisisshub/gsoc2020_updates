## Pyrpm
------------------------------------------------
###### What is PyRpm?
Python-Rpm-Spec is a python library for parsing RPM spec files.

###### What does it parse currently?
It can parse tags listed [here](https://github.com/bkircher/python-rpm-spec/blob/d8fcf8813ff1bee447425630fbc00e96cfb9a445/pyrpm/spec.py#L166)

###### Total number of checks in SpecCheck currently -> 51
###### Total number of distinct checks in SpecCheck currently -> 44
###### Rought estimate that can be refactored with pyrpm -> 25-30
###### Remaining checks -> (51 - 25 = 26) or (51 - 30 = 21)

&nbsp;
**(Why have I not calculated with [distinct checks - rough estimate]? Because every check is output under different condition inside SpecCheck hence it is fairly obvious to treat every check as a distinct object here)**
&nbsp;

###### List of checks that I think can be refactored with PyRpm. 
- no-spec-file
- invalid-spec-name
- non-utf8-spec-file
- non-break-space
- rpm-buildroot-usage
- setup-not-quiet
- setup-not-in-prep
- %autopatch-not-in-prep
- %autosetup-not-in-prep
- macro-in-%changelog
and so on. One may think of them as simple checks.

###### Checks that may pose issues while refactoring with PyRpm.
- hardcoded-library-path
- hardcoded-path-in-buildroot-tag
- hardcoded-packager-tag
- hardcoded-prefix-tag
- patch-fuzz-is-changed
- %ifarch-applied-patch
- patch-not-applied
and similar checks. One may think of them as comparitively complex checks.

###### Advantages of using PyRpm
- A module that can parse specfile itself. Hence removal of unneccessary code from SpecCheck.
- Uses a much better and concise way of parsing specfile than being used in SpecCheck currently.
- We can introduce new methods for parsing specfile inside pyrpm for missing checks / tags. Read the **Current Status** in [README](https://github.com/bkircher/python-rpm-spec/blob/master/README.md) for a clear understanding.
- Eventually refactored checks would have better construction and future contributors will have it easy to understand the codebase.

###### Disadvantages of using PyRpm
-   **IMPORTANT**
Introduction of a third party module in the current codebase may lead to unwanted issues. As pyrpm is a single person maintained project. Abandonment of the project by him would lead to deprecated codebase in SpecCheck for the future contributors.
-   Not all checks can be refactored currently with the pyrpm which can lead to "uneven" refactoring of SpecCheck since not all checks will follow the usage of module(pyrpm).
-   As mentioned in advantages(above) not all tags can be parsed using the pyrpm (mentioned in the pyrpm [README](https://github.com/bkircher/python-rpm-spec/blob/master/README.md)). This can be an advantage and disadvantage altogether, since introducing a new method in pyrpm module(if needed) can be time-consuming and may result in regression.
-   Refactoring of the issues with pyrpm can lead to changing of "approach" taken to detect a check. A contributor will have to be very precise as to not hinder the approach taken for a particular check.

##### My Personal Opinion on PyRpm
I suggest we do **not use** PyRpm. Since SpecCheck then would rely on a single module(pyrpm) for most of its checks which may and may not have future contributors to maintain it for the future python updates.


## What have I done so far?

----
#### Tests
-----
&nbsp;
Written tests for checks, check commit **[here](https://github.com/thisisshub/rpmlint/commit/8f9ed17b358bfd2cdf6858df64f6c5ee700b0b8e)**
This commit contains tests for,
- rpm-buildroot-usuage
- make-check-outside-check-section
- setup-not-quiet
- setup-not-in-prep
- autopatch-not-in-prep
- autosetup-not-in-prep
- use-of-RPM-SOURCE-DIR
- configure-without-libdir-spec
- hardcoded-library-path
- obsolete-tag
- hardcoded-path-in-buildroot-tag
- buildarch-instead-of-exclusivearch-tag
- hardcoded-packager-tag
- hardcoded-prefix-tag
- buildprereq-use
- bsisr (read the function comment)
- coid (read the function comment)
- unversioned-explicit-version
- unversioned-explicit-obsoletes
- macro-in-changelog
- libdir-macro-in-noarch-package
- deprecated-grep
- macro-in-comment
- no-build-root-tag
- no-essential-section
- more-than-one-changelog-section
- lib-package-without-mklibname
- depscript-without-disabling-depgen
- mixed-use-of-spaces-and-tabs
- ifarch-applied-patch
- patch-not-applied
- invalid-url

and their test_check_*_not_applied tests as well.
&nbsp;
----
#### Specfiles
-----
Written specfiles for the tests. Check commit **[here](https://github.com/thisisshub/rpmlint/commit/45305ce8fb210e11b82e580190a8edf30002659b)**


&nbsp;
----
#### Problems I faced during writing tests
-----

I have unwritten tests for some checks which can be seen **[here](https://github.com/thisisshub/rpmlint/blob/45305ce8fb210e11b82e580190a8edf30002659b/test/test_speccheck.py#L435)**
###### Total unwritten tests = 5 and they are,
- prereq check (already discussed)
- forbidden-controlchar-found (Since there is no package package with such in openSUSE Factory, 
  it is harder to reproduce this error)
- non-standard-group (I tried applying a non-standard group for example 'Group: Something/docs' but it didn't worked)
- patch-fuzz-is-changed (I had a hard time realizing what this patch fuzz actually means. 
  Sadly I could not reproduce it either, there is a package 'xemacs' that has this error but even after checking their spec
  I could not work out of what "causes" this error.)
- specfile-error (This one is the weirdest of all. I simply cannot understand this error. I need more explanation on it.)

In all I think I can revisit these unwriiten tests at the time of their refactoring. 
This would not only buy you some time on understanding these errors but also me on thinking of creative ways to create them.
these errors but also me for thinking on ways to introduce it.

&nbsp;
----
#### Overall status of test_speccheck.py
-----

###### Distinct Checks in SpecCheck -> 44 (Remember Checks are repeated) 
###### Tests written by me -> 73 (as of commit [here](https://github.com/thisisshub/rpmlint/commit/8f9ed17b358bfd2cdf6858df64f6c5ee700b0b8e))
###### Status of Test Passing -> All TEST PASS
###### Status of FLAKE8 Passing -> FLAKE8 Check pass
