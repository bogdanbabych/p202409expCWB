# p202409expCWB
experiments with CWB

## local prototype : running via localhost + apache server
### description of the installation steps:

(```./docs/cwb-mac.txt``` file contains  the output of these commands)

1. Installing CWB

documentation on: https://cwb.sourceforge.io/install.php

command:

~~~
brew install cwb3
~~~


2. creating directories for the registry and data

~~~
/Users/bogdan/corpora
/opt/homebrew/share/cwb/registry
~~~

3. update cpan for installing perl libraries (required by old implementation)
~~~
cpan
~~~
output:
~~~
*** Remember to add these environment variables to your shell config
    and restart your shell before running cpan again ***

PATH="/Users/bogdan/perl5/bin${PATH:+:${PATH}}"; export PATH;
PERL5LIB="/Users/bogdan/perl5/lib/perl5${PERL5LIB:+:${PERL5LIB}}"; export PERL5LIB;
PERL_LOCAL_LIB_ROOT="/Users/bogdan/perl5${PERL_LOCAL_LIB_ROOT:+:${PERL_LOCAL_LIB_ROOT}}"; export PERL_LOCAL_LIB_ROOT;
PERL_MB_OPT="--install_base \"/Users/bogdan/perl5\""; export PERL_MB_OPT;
PERL_MM_OPT="INSTALL_BASE=/Users/bogdan/perl5"; export PERL_MM_OPT;

~~~

4. as indicated above, update the file ```.bash_profile ```

the results are in ```./docs/.bash_profile*``` files


5. install cpan CWB libraries:

~~~
cpan CWB
sudo cpan CWB::CL
sudo cpan CWB::Web
sudo cpan CWB::CQI
~~~

6. install apache web server on localhost (for prototyping)

(output and links in file ```./docs/apache-run.txt```)

Location of apache files (configuartion and logs):

~~~
apache files:

/Library/WebServer

apache configuration:

/private/etc/apache2
(link:)
/etc/apache2

/private/etc/apache2/users/bogdan.conf
/private/etc/apache2/httpd.conf
/private/var/log/apache2/access_log
/private/var/log/apache2/error_log

files for local websites:
/Users/bogdan/Sites

~~~

