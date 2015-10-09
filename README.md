# Bulk Puppet File Validator

Any Puppet file or manifest with the extension '.pp' can be validated with the command `puppet parser validate $file`. The validator checks for valid puppet syntax, and returns an error with a line number if validation fails. 

Best practice is to validate each puppet file after any edits are made before deployment to cut down on the number of errors, but this can become tedious if a large number of files are edited, or if the puppet codebase is large. 

This tool validates '.pp' files in batches, depending on command line options.

Save it somewhere in your $PATH and make it executable.

## Examples:

Validate every '.pp' file in the current directory and all subdirectories:
```
modules $: validate-pp -a
Validating /etc/puppetlabs/puppet/modules/base/manifests/init.pp...
Validating /etc/puppetlabs/puppet/modules/base/manifests/ssh.pp...
Validating /etc/puppetlabs/puppet/modules/base/manifests/sudoers.pp...
Validating /etc/puppetlabs/puppet/modules/base/tests/init.pp...
Validating /etc/puppetlabs/puppet/modules/base/tests/ssh.pp...
Validating /etc/puppetlabs/puppet/modules/base/tests/sudoers.pp...
Validating /etc/puppetlabs/puppet/modules/localusers/manifests/groups/finance.pp...
Validating /etc/puppetlabs/puppet/modules/localusers/manifests/groups/wheel.pp...
Validating /etc/puppetlabs/puppet/modules/localusers/manifests/init.pp...
Validating /etc/puppetlabs/puppet/modules/localusers/tests/init.pp...
No errors found, all parsed files appear valid.
```

Validate only the files that have been added, edited, copied, or moved according to git:
```
### This is significantly faster than checking every file,
### especially as the codebase increases in size.

modules $: git status
On branch master
Your branch is up-to-date with 'origin/master'.
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

  modified:   base/manifests/ssh.pp

Untracked files:
  (use "git add <file>..." to include in what will be committed)

  base/manifests/httpd.pp

no changes added to commit (use "git add" and/or "git commit -a")
modules $: validate-pp -g
Validating /etc/puppetlabs/puppet/modules/base/manifests/ssh.pp...
Validating /etc/puppetlabs/puppet/modules/base/manifests/httpd.pp...
No errors found, all parsed files appear valid.
```

Try `validate-pp --help` for the full list of options.
