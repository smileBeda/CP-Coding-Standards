# CP-Coding-Standards
An attempt of satisfying everyone and still have some standards.

Public discussion that lead to this ruleset can be found [here](https://forums.classicpress.net/t/adopt-wpcs-for-themes-and-plugin-directory/3755/)

These Coding standards are aimed towards Theme and Plugin _reviewers_.
Developers are still encouraged to actually use a more thorough Standard ([WPCS `WordPress` standard](https://github.com/WordPress/WordPress-Coding-Standards) would be highly recommended).
The ruleset should help when revieweing plugins for security issues and not serve as a referece of what quality code looks like.

This rulest is _NOT_ to be used for CP Core either.

While crafting code compliant with this ruleset _will_ make it easier for your plugin to be listed on the CP Directory, it does _not_ guarantee it, since there are many other conditions to be fullfilled.

Again, this is NOT a "good" coding standard to improve your CodeBase in general. 
It would be better to use the more complete WPCS, and if you follow that, it will pass this Ruleset without issues.

---

### PRINCIPLES
- only security related issues should be flagged with the exception:
    - text domain is passed to localisation functions
    - text domain passed in function matches declared domain
- no styling, formatting or documentation issues should be flagged
- flags should not be silenced
- should not sniff CSS or JS files. Should sniff _only_ PHP Files.
- should check for a minimum supported _WP_ Version
- should not be considered a final judgement - instead a human being should evaluate the results and work on a solution with the developer

---

### USAGE

1. Install WPCS as outlined [here](https://github.com/WordPress/WordPress-Coding-Standards#installation)
2. Download the phpcs.xml file of this repo and put it into the directory you want to analyse
3. `cd` into that directory and run `phpcs --standard=/path/to/phpcs.xml project-folder-or-file`

This will produce an error log like this example 
```
FILE: path/to/file.php
---------------------------------------------------------------------------------------------------------------------------------------------------
FOUND 11 ERRORS AND 4 WARNINGS AFFECTING 11 LINES
---------------------------------------------------------------------------------------------------------------------------------------------------
  30 | ERROR   | eval() is a security risk so not allowed.
 100 | ERROR   | Missing $domain arg.
 200 | ERROR   | All output should be run through an escaping function
     |         | '$var'.
 450 | WARNING | Processing form data without nonce verification.

---------------------------------------------------------------------------------------------------------------------------------------------------
```

The Standard also includes a (by default not active) PHP Compatibility Sniff.
To use that, uncomment this section in the XML file:
```
<!-- <config name="testVersion" value="5.2-"/>
<rule ref="PHPCompatibility">
    <include-pattern>*\.php$</include-pattern>
</rule> -->
```
Then make sure you have `PHPCompatibility` installed (if not, [these steps](https://github.com/PHPCompatibility/PHPCompatibility#installation-via-a-git-check-out-to-an-arbitrary-directory-method-2) work great).

The error report will now include by default also PHP Compatibility Details.
To change the minimum and manximum supported PHP version, amend the line `<config name="testVersion" value="5.2-"/>` as you prefer, for example `<config name="testVersion" value="5.5-7.4"/>`

---

### CONTRIBUTORS

To contribute, you will need to know the exact rule you want to add or remove so to add it to the XML file.

The rule throwing an error can be found by passing parameter `-s` to the phpcs command like so 
`phpcs --standard=/path/to/phpcs.xml -s project-folder-or-file`
This will then produce a log like this:
```
FILE: /Users/bedaschmid/Desktop/vars-master/classes/UpdateClient.class.php
---------------------------------------------------------------------------------------------------------------------------------------------------
FOUND 1 ERROR AFFECTING 1 LINE
---------------------------------------------------------------------------------------------------------------------------------------------------
 100 | ERROR   | Missing $domain arg. (WordPress.WP.I18n.MissingArgDomain)
...
---------------------------------------------------------------------------------------------------------------------------------------------------
```
`WordPress.WP.I18n.MissingArgDomain` is the rule throwing the error. 

Consult the existing rules in the XML file and the related rule documentations to see how to remove, add, configure it more granularly.

