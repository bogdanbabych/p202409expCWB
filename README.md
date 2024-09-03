# p202409expCWB
experiments with CWB

## local prototype : running via localhost + apache server
### description of the installation / configuration steps:

(```./docs/cwb-mac.txt``` file contains  the output of these commands)

#### 1. Installing CWB on local machine (instructions tested for Mac Sonoma 14.6.1 (23G93))

documentation on: https://cwb.sourceforge.io/install.php

command:

~~~
brew install cwb3
~~~


#### 2. creating directories for the registry and data

~~~
/Users/bogdan/corpora
/opt/homebrew/share/cwb/registry
~~~

#### 3. update cpan for installing perl libraries (required by old implementation)
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

#### 4. as indicated above, update the file ```.bash_profile ```

the results are in ```./docs/.bash_profile*``` files


#### 5. install cpan CWB libraries:

~~~
cpan CWB
sudo cpan CWB::CL
sudo cpan CWB::Web
sudo cpan CWB::CQI
~~~

#### 6. install and configure apache2 web server on localhost (for prototyping)

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

Running / stopping apache:

~~~

sudo /usr/sbin/apachectl restart
apachectl configtest
sudo apachectl graceful
sudo /usr/sbin/apachectl -k restart

~~~

to test Apache:
type in a browser:

```http://localhost/```


#### 7. test if cgi scripts can run, and configure if necessary:
copy the perl and python scripts from ```./docs/first.pl ; ./docs/firstpython.py``` into the executable directory:

```/Library/WebServer/CGI-Executables```

~~~
http://localhost/cgi-bin/first.pl
http://localhost/cgi-bin/firstpython.py
~~~

if the cgi scripts do not run, e.g., you get the error message:

~~~
Internal Server Error
The server encountered an internal error or misconfiguration and was unable to complete your request.

Please contact the server administrator at you@example.com to inform them of the time this error occurred, and the actions you performed just before this error.

More information about this error may be available in the server error log.
~~~

and in the log file ```/private/var/log/apache2/error_log```:

~~~


[Tue Sep 03 11:57:11.978335 2024] [cgi:error] [pid 8784] [client ::1:61166] End of script output before headers: firstpython.py
[Tue Sep 03 11:57:31.008452 2024] [cgi:error] [pid 8782] [client ::1:61167] AH01215: (1)Operation not permitted: exec of '/Library/WebServer/CGI-Executables/first.pl' failed: /Library/WebServer/CGI-Executables/first.pl


~~~

, you can follow the steps under this link:  
https://discussions.apple.com/docs/DOC-250007792

-- also a copy in the local directory: ```./docs/DOC-250007792.html``` , or ./docs .pdf file.

Basically, you update configuration files for your apache web server, the page tells you how to do it for your local website in the home directory /Users/bogdan/Sites, not system-wide, but possibly you can adjust the correct lines also for the system-wide configuration (I haven't tried this).

Then the local files should work, so instead you put html files and run the scripts from:

~~~
http://localhost/~bogdan/
http://localhost/~bogdan/first.pl
http://localhost/~bogdan/firstpython.py
~~~

After this, the apache2 server is configured










