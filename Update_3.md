#### What have I done so far? 
**(Commit Link: [here](https://github.com/thisisshub/rpmlint/commit/8e4848009ea2b9dc61a4ced40e2be0f5f32543fb))**


Refactored Succesful checks for,
- `no-spec-file`
- `invalid-spec-name`
- `non-utf8-spec-file`

### Tests
-------------
Paste of test output, 

`sudo podman run -v $(pwd):/usr/src/rpmlint/ opensusetw python3 -m pytest --color=yes -v test/test_speccheck.py`

**Linked here: [here](https://0bin.net/paste/YX7x2-EUaetYrnAG#JpVa5-m9JojVxGqTiV9VV2N7wfR48tbDAFT5uC6UO13)**

### Checks
------
- `no-spec-file` **([here](https://github.com/thisisshub/rpmlint/blob/8e4848009ea2b9dc61a4ced40e2be0f5f32543fb/rpmlint/checks/SpecCheck.py#L143))**
- `invalid-spec-name` **([here](https://github.com/thisisshub/rpmlint/blob/8e4848009ea2b9dc61a4ced40e2be0f5f32543fb/rpmlint/checks/SpecCheck.py#L150))**
- `non-utf8-spec-file` **([here](https://github.com/thisisshub/rpmlint/blob/8e4848009ea2b9dc61a4ced40e2be0f5f32543fb/rpmlint/checks/SpecCheck.py#L165))**

### Suggestions
----------
Suggest a better `except` statement for,

```python
except Exception as indentifier:
   print(str(indentifier))
```
**Permalinked here: [here](https://github.com/thisisshub/rpmlint/blob/8e4848009ea2b9dc61a4ced40e2be0f5f32543fb/rpmlint/checks/SpecCheck.py#L162)**
