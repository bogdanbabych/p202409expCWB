# p202409expCWB
experiments with CWB

## local prototype : running via localhost + apache server
### A. Description of the installation / configuration steps:

(```./docs/cwb-mac.txt``` file contains  the output of these commands)

#### A.1. Installing CWB on local machine (instructions tested for Mac Sonoma 14.6.1 (23G93))

documentation on: https://cwb.sourceforge.io/install.php

command:

~~~
brew install cwb3
~~~


#### A.2. creating directories for the registry and data

~~~
/Users/bogdan/corpora
/opt/homebrew/share/cwb/registry
~~~

#### A.3. update cpan for installing perl libraries (required by old implementation)
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

#### A.4. as indicated above, update the file ```.bash_profile ```

the results are in ```./docs/.bash_profile*``` files


#### A.5. install cpan CWB libraries:

~~~
cpan CWB
sudo cpan CWB::CL
sudo cpan CWB::Web
sudo cpan CWB::CQI
~~~

#### A.6. install and configure apache2 web server on localhost (for prototyping)

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


#### A.7. test if cgi scripts can run, and configure if necessary:
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
[Tue Sep 03 11:49:06.228461 2024] [cgi:error] [pid 8783] [client ::1:61027] AH01215: (1)Operation not permitted: exec of '/Library/WebServer/CGI-Executables/first.pl' failed: /Library/WebServer/CGI-Executables/first.pl
[Tue Sep 03 11:49:06.229403 2024] [cgi:error] [pid 8783] [client ::1:61027] End of script output before headers: first.pl
[Tue Sep 03 11:57:08.426033 2024] [cgi:error] [pid 1516] [client ::1:61157] AH01215: (1)Operation not permitted: exec of '/Library/WebServer/CGI-Executables/first.pl' failed: /Library/WebServer/CGI-Executables/first.pl
[Tue Sep 03 11:57:08.426690 2024] [cgi:error] [pid 1516] [client ::1:61157] End of script output before headers: first.pl
[Tue Sep 03 11:57:09.524812 2024] [cgi:error] [pid 8783] [client ::1:61165] AH01215: (1)Operation not permitted: exec of '/Library/WebServer/CGI-Executables/firstpython.py' failed: /Library/WebServer/CGI-Executables/firstpython.py
[Tue Sep 03 11:57:09.525722 2024] [cgi:error] [pid 8783] [client ::1:61165] End of script output before headers: firstpython.py
[Tue Sep 03 11:57:11.977947 2024] [cgi:error] [pid 8784] [client ::1:61166] AH01215: (1)Operation not permitted: exec of '/Library/WebServer/CGI-Executables/firstpython.py' failed: /Library/WebServer/CGI-Executables/firstpython.py
[Tue Sep 03 11:57:11.978335 2024] [cgi:error] [pid 8784] [client ::1:61166] End of script output before headers: firstpython.py
[Tue Sep 03 11:57:31.008452 2024] [cgi:error] [pid 8782] [client ::1:61167] AH01215: (1)Operation not permitted: exec of '/Library/WebServer/CGI-Executables/first.pl' failed: /Library/WebServer/CGI-Executables/first.pl
[Tue Sep 03 11:57:31.009494 2024] [cgi:error] [pid 8782] [client ::1:61167] End of script output before headers: first.pl
[Tue Sep 03 11:57:32.982370 2024] [cgi:error] [pid 14195] [client ::1:61169] AH01215: (1)Operation not permitted: exec of '/Library/WebServer/CGI-Executables/first.pl' failed: /Library/WebServer/CGI-Executables/first.pl
[Tue Sep 03 11:57:32.983293 2024] [cgi:error] [pid 14195] [client ::1:61169] End of script output before headers: first.pl
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

After this, the apache2 server is configured.

### B. Copy existing corpora to test cwb

The links to downloads of the demo corpora are here:  
https://cwb.sourceforge.io/install.php#other

Copies are also in the ```./corpora/``` directory in this repository


#### B.1. move the corpora to the directory where binary corpus data will be stored, e.g. ```~/corpora/``` & unpack them

~~~
cd ~/corpora
wget https://cwb.sourceforge.io/files/DemoCorpus-German-1.0.tar.gz
wget https://cwb.sourceforge.io/files/Dickens-1.0.tar.gz 
tar xvzf DemoCorpus-German-1.0.tar.gz
tar xvzf Dickens-1.0.tar.gz
~~~

#### B.2. create corpus registry directory, as specified during cwb installation process (stage A.1.)

Copy registry files from :
~~~
/Users/bogdan/corpora/Dickens-1.0/registry
/Users/bogdan/corpora/DemoCorpus-German/registry
~~~

to this corpus register directory:
~~~
/opt/homebrew/share/cwb/registry
~~~

#### B.3. edit registry files for each corpus in the corpus registry directory, so they point to the correct data directory for each corpus:

E.g.: in ```dickens``` file edit these lines, putting the right location of the HOME directory for the corpus and INFO file:

~~~
# path to binary data files
HOME /Users/bogdan/corpora/Dickens-1.0/data
# optional info file (displayed by "info;" command in CQP)
INFO /Users/bogdan/corpora/Dickens-1.0/data/.info
~~~

, and in the ```german-law``` file, change these lines:

~~~
HOME /Users/bogdan/corpora/DemoCorpus-German/data
INFO /Users/bogdan/corpora/DemoCorpus-German/data/.info
~~~

Now the CWB has the data and can be tested on them, so make sure this runs in your command line:















