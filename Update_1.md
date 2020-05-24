#### What have I done so far?

I have basically written tests for checks [1] and [2]
placed at `test/test_speccheck.py`. 

### Tests
-------------
**[1]** `non-utf8-spec-file`
Reading through the test you'll find that I used `[spec/SpecCheck6]` 
in [1] which doesn't exist in the official repository yet. Since non-utf8-spec-file 
must find if a specfile follows UTF-8 character encoding, I reproduced a UTF-16 
endcoded file (which is the bug) with command `iconv` (later). 

**[2]** `no-spec-file`
The test for check no-spec-file seems to be posing an issue as I cannot seem to 
reproduce the rpmlint error of 
**"No spec file was specified in your RPM metadata. Please specify a valid SPEC file to build a valid RPM package"**.
Explanation:
It was fairly easy to generate the error in non-utf8-spec-file since any other encoding apart from
UTF-8 would have generated the error which I could then tested was present in rpmlint error output.
**What have I tried to reproduce the error?**
Passing a pacakge named `[spec/SpecCheck5]` inside the `pytest.mark.parameterize` statement.
Where infact `SpecCheck5.spec` does not exist inside the `rpmlint/test/spec/` directory as it 
should hence trying to create an error for `SpecCheck5.spec file not found`.

Since my test fails above,
**How does one produce the error of no specfile found?**

**Steps to reproduce my test**
```
$ cd rpmlint/test/spec/
$ iconv -f ASCII -t UTF-16 SpecCheck2.spec > SpecCheck6.spec
$ cd rpmlint/test/
$ vim test_speccheck.py # add the code in [1] and [2] at the end of the file
$ sudo podman run -v $(pwd):/usr/src/rpmlint/ opensusetw python3 -m pytest --color=yes -v test/test_speccheck.py
```

**[1] test for check non-utf8-spec-file**
*(works perfectly fine)* 
```python
@pytest.mark.parametrize('package', ['spec/SpecCheck6']) 
def test_check_non_utf8_spec_file(package, speccheck): 
    """ Checks if spec file has UTF-8 character encoding """ 
    output, test = speccheck pkg = get_tested_spec_package(package) 
    test.check_spec(pkg) 
    out = output.print_results(output.results) 
    assert 'E: non-utf8-spec-file' in out
```

**[2] test for check no-spec-file**
(*has issues*)

```python
@pytest.mark.parametrize('package', ['spec/SpecCheck5'])
def test_check_no_spec_file(package, speccheck):
    """ Checks if spec file is present """
    output, test = speccheck
   # pkg = get_tested_spec_package(package)
   # test.check_spec(pkg)
    out = output.print_results(output.results)
    assert 'E: no-spec-file' in out

```
### Checks
------
For refactoring/implementing the either of the checks I had two approaches but I'll explain using the `non-utf8-spec-file` check of what I meant.

**Approach 1:**
As you can see the [**check_spec(self, pkg)**](https://github.com/rpm-software-management/rpmlint/blob/031859b8ffc71d954b41bbd1b12be366fcb041a0/rpmlint/checks/SpecCheck.py#L149) method contains 24 variables. I thought about implementing  something like [**this**](https://gist.github.com/thisisshub/fff7a92fb16f4310063719aeffbb898b) look from line 63 but it has a drawback of making all the 24 variables that were used throughout the program to `local scope`. What this would mean is to either `globalise` all those variables or use Approach 2 (see later)

**Approach 2:**
In this approach I have implemented the methods something like [**this**](https://gist.github.com/thisisshub/462ca52641664a419719b2944d384524) look from line 35 where I wrote the method before `check_spec(self, pkg)` but just with the drawback of re-initializing every variable in every method where it shall be needed. I am personally in favor of this approach since making every variable global is not good.

### Doubt
----------
- One of the biggest doubt that I have is to how do you connect/associate `objects` of the new/refactored check `methods/functions` that one would make to the the existing `rpmlint` command. 
Explanation:
The `test_check_non_utf8_spec_file` inside `test_speccheck.py` PASSES as long as

```python
if self._spec_file:
            if not Pkg.is_utf8(self._spec_file):
                self.output.add_info('E', pkg, 'non-utf8-spec-file')
```
lies inside the `def check_spec()` method. If I do make an indiviual `method` of it such as,

```python
def check_no_utf8_spec_file(self, pkg):
    if self._spec_file:
                if not Pkg.is_utf8(self._spec_file):
                    self.output.add_info('E', pkg, 'non-utf8-spec-file')
```
and place it as I did in **Approach 2** the test fails.

Hence, How would I associate the `objects` of my new/refactored `methods/functions` in the SpecCheck so that they can be used in the `rpmlint` command.
