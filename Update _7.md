## I have categorized checks in the table below to give a quick overview

Disclaimer:

- Essential Tags:
    - Cannot be tested since they are required to make an rpm itself, there is an ongoing discussion with the upstream developers [here](https://github.com/rpm-software-management/rpmlint/issues/442)
- Checks that have no definition in TagsCheck.toml and even on the internet:
    - I cannot find the absolute definition of these checks which is making it harder for me to write the tests for it, to solve this issue
      I will backtrack and read the code to test it and then make a description and definiton in the required .toml file
      
      NOTE:
        - Since these checks do not have a definition availible anywhere I think there is a high possibility that they were either frowned upon for usage 
        or are no longer needed by the packagers which makes them a valid candidate for the column `Checks I think can be dropped` (below)
- Broken:
    - These checks are down right broken and I will fix them as soon as writing tests for the remaining legacy code is over
- Done:
    - These checks are done and I have commited them to the most recent fork branch gsoc-TagsCheck
- Checks I think can be dropped:
    - This check is no longer used anywhere (fedora, openSUSE, mageia, rosalab) and can be dropped. 

|Essential Tags|Checks that have no definition in TagsCheck.toml and even on the internet|Broken                     |Done                                |Checks I think can be dropped|
|--------------|-------------------------------------------------------------------------|---------------------------|------------------------------------|-----------------------------|
|no-summary-tag|no-dependency-on                                                       |non-coherent-filename    |description-line-too-long         |requires-on-release        |
|no-version-tag|no-version-dependency-on                                               |summary-on-multiple-lines|devel-package-with-non-devel-group|                             |
|no-release-tag|no-major-in-name                                                       |invalid-license          |invalid-dependency                |                             |
|no-name-tag   |no-buildhost-tag                                                        |no-group-tag               |invalid-url                       |                             |
|no-license    |invalid-buildhost                                                      |tag-not-utf8             |invalid-version                   |                             |
|              |description-use-invalid-word                                           |invalid-dependency       |name-repeated-in-summary          |                             |
|              |not-standard-release-extension                                         |no-changelogname-tag      |no-description-tag                 |                             |
|              |                                                                         |                           |no-url-tag                         |                             |
|              |                                                                         |                           |'olete-not-provided             |                             |
|              |                                                                         |                           |self-obsoletion                   |                             |
|              |                                                                         |                           |summary-ended-with-dot            |                             |
|              |                                                                         |                           |summary-not-capitalized           |                             |
|              |                                                                         |                           |summary-too-long                  |                             |
|              |                                                                         |                           |tag-in-description                |                             |
|              |                                                                         |                           |unexpanded-macro                  |                             |
|              |                                                                         |                           |unreasonable-epoch                |                             |
|              |                                                                         |                           |useless-provides                  |                             |
|              |                                                                         |                           |invalid-license-exception           |                             |


#### Details on some of the checks
- invalid-dependency


This check is called upon twice in `TagsCheck.py` and is partially broken as 

```python
for r in self.invalid_requires:
                if r.search(d[0]):
                    self.output.add_info('E', pkg, 'invalid-dependency', d[0])
```
code does not work

but this does
```python
if d[0].startswith('/usr/local/'):
                self.output.add_info('E', pkg, 'invalid-dependency', d[0])
```

- The `Epoch:` tag

This tag is posing an issue in the OBS build service since any package that contains an `Epoch:` tag either gets broken or gets failed upon build
here is a [forum issue](https://forums.opensuse.org/archive/index.php/t-522085.html) 

Because of this issue the checks containing `epoch` in them for example
- `unreasonable-epoch`
- `no-epoch-in-%s-tag`
- `no-epoch-in-dependency`

cannot be tested

### Final Words

I think some of the checks in the `TagsCheck.py` can be dropped which will lead to minimization of the the `TagsCheck.py` file


I propose to open two/three different issues the upstream regarding,

- Issue-1 : This issue will track the broken checks, I plan to write an issue something like [this](https://gist.github.com/thisisshub/1fd01555e4351f9bd162f0ad3fab893b)
- Issue-2 : This issue will track the checks that can be dropped.

Optional:
- Issue-3 : This issue will track the `Epoch:` tag error in the OBS build service which is posing an issue to test the checks containing `epoch` in them.


