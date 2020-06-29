
## What have I done so far?
- Submit a PR [here](https://github.com/rpm-software-management/rpmlint/pull/436)
- Complete Trello tasks under [Sprint Backlog](https://trello.com/b/bGANgSB7/gsoc-2020-refactor-old-rpmlint-checks)

### E: lib-package-without-%mklibname

- #### What is it?
	The package name must be built using %mklibname to allow lib64 and lib32
coexistence.
- #### Mentioned in trello [here](https://trello.com/c/XDkUoV41/22-e-lib-package-without-mklibname) 

- #### Ongoing discussion as of 25th June 2020 at github [rpmlint/issues/9](https://github.com/rpm-software-management/rpmlint/issues/9)

- #### Could it be dropped?
	- Number of packages that have `lib-package-without-%mklibname` : None

	Based upon the research none of the packages present under [rpmlint.opensuse.org](https://rpmlint.opensuse.org/) contain the `%mklibname` error and on the other hand @scop made an interesting point where he mentioned in the github issue (above) that Mandriva (a linux distro discontinued since 2011) does contain this rpm issue. 
	Also, an updated list of rpmlint errors  at [Fedora](https://fedoraproject.org/wiki/Common_Rpmlint_issues#Links) (last update at 2016) does not contain this error and package checks at [openSUSE](https://en.opensuse.org/openSUSE:Packaging_checks) suggests the same.
Hence if upstream developers agree on the discussion above then this check can be dropped.


### E: broken-syntax-in-scriptlet-requires

- #### What is it?
	Comma separated context marked dependencies are silently broken in some
versions of rpm.  One way to work around it is to split them into several ones,
eg. replace 'Requires(post,preun): foo' with 'Requires(post): foo' and
'Requires(preun): foo'.
- #### Mentioned in trello [here](https://trello.com/c/O38Wfq7G/23-e-broken-syntax-in-scriptlet-requires)
- #### Is it still current?
	If by current, you meant does any of the packages contain it in [rpmlint.opensuse.org](https://rpmlint.opensuse.org/)  then the answeris no.
	- Number of packages that have `broken-syntax-in-scriptlet-requires` : None
	
- #### Was it fixed?
	Based upon my research there has been a patch for this error on distros with rpm >= 4.7.1 [here](https://bugzilla.redhat.com/show_bug.cgi?id=1053075)

### Add docstrings and comments to SpecCheck
I am adding the docstring and comments for future contributors as you asked. I'll merge it in the next commit as soon as my [PR](https://github.com/rpm-software-management/rpmlint/pull/436) gets merged.
### Miscellaneous
(Found under Trello Sprint Backlog)
- [x] Remove useless header in SpecCheck 
- [x]   Use better names for variables

Will commit these along with the next PR upon revision by you.
