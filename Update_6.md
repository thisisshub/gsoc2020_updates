## What have I done so far?

#### Changes in SpecCheck.py

- Removed baseless header [here](https://github.com/thisisshub/rpmlint/commit/fb9c70f45cb5ebef711ca073eb58f1dbc00bb280#)
- Changed variables to snake-case naming style [here](https://github.com/thisisshub/rpmlint/commit/5a10db56670b1f5cb9639d27d153aab0a8ac3b01)
- Refactor checks to indviual methods (the checks present after the ugly "for" loop 
  inside `check_spec()` method) see it [here](https://github.com/thisisshub/rpmlint/commit/fb9c70f45cb5ebef711ca073eb58f1dbc00bb280)

#### Changes in test_speccheck.py

- Add test for prereq [here](https://github.com/thisisshub/rpmlint/commit/e84ab7f3be382c313599699cd8a5bd1bfde2ae6b#r40235346)
- Add test for patch-fuzz-is-changed [here](https://github.com/thisisshub/rpmlint/commit/e84ab7f3be382c313599699cd8a5bd1bfde2ae6b#r40235361)

###### Travis CI status pass in fedora32, fedoradev, opensusetw

###### Link to [trello](https://trello.com/b/bGANgSB7/gsoc-2020-refactor-old-rpmlint-checks)
