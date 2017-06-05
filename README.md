# Sobelow

Sobelow is a security-focused static analysis tool for the 
Phoenix framework. For security researchers, it is a useful 
tool for getting a quick view of points-of-interest. For 
project maintainers, it can be used to prevent introducing a 
number of common vulnerabilities. 

Currently Sobelow detects some types of the following 
security issues: 

* Insecure configuration
* Cross-Site Scripting
* SQL injection
* Command injection
* Directory traversal
* Unsafe serialization

Potential vulnerabilities are flagged in different colors 
according to confidence in their insecurity. High confidence is 
red, medium confidence is yellow, and low confidence is green.

A finding is typically marked "low confidence" if it looks 
like a function could be used insecurely, but it cannot 
reliably be determined if the function accepts user-supplied 
input. That is to say, green findings are not secure, they 
just require greater manual validation.

**Note:** This project is in constant development, and 
additional vulnerabilities will be flagged as time goes on. 
If you encounter a bug, or would like to request additional 
features or security checks, please open an issue!

## Installation

To install Sobelow, you must have a working Elixir environment. 
Then, execute the following from the command line: 

    $ mix archive.install hex sobelow
    
## Use

The simplest way to scan a Phoenix project is to run the 
following from the project root:

    $ mix sobelow

## Options

  * `--root -r` - Specify application root directory
  * `--with-code -v` - Print vulnerable code snippets
  * `--ignore -i` - Ignore modules
  * `--details -d` - Get module details
  * `--all-details` - Get all module details
  * `--private` - Skip update checks
  * `--router` - Specify router location
  * `--exit` - Return non-zero exit status
  
The `root` option takes a path argument:

    $ mix sobelow --root ../my_project

The `with-code` option takes no arguments:

    $ mix sobelow --with-code
    
The `ignore` option takes a comma-separated list of modules:

    $ mix sobelow -i XSS.Raw,Traversal
    
The `details` option takes a single module:

    $ mix sobelow -d Config.CSRF
    
The `exit` option accepts a confidence threshold (low, medium, or high), 
and will return a non-zero exit status above that threshold.

    $ mix sobelow --exit Low
    
## False Positives
Sobelow favors over-reporting versus under-reporting. As such, 
you may find a number of false positives in a typical scan. 
These findings may be individually ignored by adding a 
`@sobelow_skip` mark, along with a list of modules, before 
the function definition. 

```elixir
@sobelow_skip ["Traversal"]
def vuln_func(...) do
  ...
end
```

Then, run the scan with the `--skip` flag.

    $ mix sobelow --skip

Config findings cannot be skipped in this way. Instead, use 
the standard `ignore` option.

## Modules
Findings categories are broken up into modules. These modules 
can then be used to either ignore classes of findings (via the 
`ignore` and `skip` options) or to get vulnerability details (via the 
`details` option).
 
This list, and other helpful information, can be found on the 
command line:

    $ mix help sobelow

## Updates
When scanning a project, Sobelow will occasionally check for 
updates, and will print an alert if a new version is available. 
Sobelow keeps track of the last update-check by creating a 
`.sobelow` file in the root of the scanned project.

If this functionality is not desired, the `--private` flag can 
be used with the scan.

    $ mix sobelow --private
