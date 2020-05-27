#### What have I done so far? 
**(Commit Link: [here](https://github.com/thisisshub/rpmlint/commit/d4864797fa713ffd2fbcff9c67f550c1f98f7c30))**


Written Succesful tests for,
- `test_check_no_spec_file`([here](https://github.com/thisisshub/rpmlint/blob/d4864797fa713ffd2fbcff9c67f550c1f98f7c30/test/test_speccheck.py#L70))
- `test_check_no_spec_file_not_applied`([here](https://github.com/thisisshub/rpmlint/blob/d4864797fa713ffd2fbcff9c67f550c1f98f7c30/test/test_speccheck.py#L79))
- `test_check_non_utf8_spec_file`([here](https://github.com/thisisshub/rpmlint/blob/d4864797fa713ffd2fbcff9c67f550c1f98f7c30/test/test_speccheck.py#L88))
- `test_check_non_utf8_spec_file_not_applied`([here](https://github.com/thisisshub/rpmlint/blob/d4864797fa713ffd2fbcff9c67f550c1f98f7c30/test/test_speccheck.py#L98))
- `test_check_invalid_spec_name`([here](https://github.com/thisisshub/rpmlint/blob/d4864797fa713ffd2fbcff9c67f550c1f98f7c30/test/test_speccheck.py#L108))
- `test_check_invalid_spec_name_not_applied`([here](https://github.com/thisisshub/rpmlint/blob/d4864797fa713ffd2fbcff9c67f550c1f98f7c30/test/test_speccheck.py#L117))

### Tests
-------------

**Explanation**

All of the tests have a brief description that can be 
found under  `rpmlint/descriptions`
but I'll copy them here so you can have a direct read,

**no-spec-file**=
No spec file was specified in your RPM metadata. Please specify a valid
SPEC file to build a valid RPM package.

**non-utf8-spec-file**=
The character encoding of the spec file is not UTF-8.

**invalid-spec-name**=
The spec file name (without the .spec suffix) must match the package name
('Name:' tag).

### Checks
------
So far I have implemented a single check `non-utf8-spec-file` locally on my system,
which works perfectly fine. It looks something like this,

```python
    def check_non_utf8_spec_file(self, pkg):
        """ Checks if no spec file is found in RPM meta data """
        self._spec_file = pkg.name
        if self._spec_file:
            if not Pkg.is_utf8(self._spec_file):
                self.output.add_info('E', pkg, 'non-utf8-spec-file')
```

### Doubt
----------

For implementation of checks `no-spec-file` and `invalid-spec-name`.
For they currently lie inside the same method called `check_source`([here](https://github.com/thisisshub/rpmlint/blob/d4864797fa713ffd2fbcff9c67f550c1f98f7c30/rpmlint/checks/SpecCheck.py#L126))

Would implementing them as,

```python
def check_source():
    ....
    def check_no_spec_file():
    ....
    def check_invalid_spec_name():
    .... 
```
be a good idea? Since taking them out of the `check_source` method fails the tests.
