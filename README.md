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


#### B.4. Test CWB in the command line
Now the CWB has the data and can be tested on them, so make sure this runs in your terminal in the command line:

~~~
cqp
[no corpus]>
~~~

The manual on using cqp interface is here:  
https://cwb.sourceforge.io/files/CQP_Manual.pdf

(it is also in the ./docs/ directory in this repository)

The cqp session:
~~~

Bogdans-MBP:~ bogdan$ cqp
[no corpus]> show corpora;
System corpora:
 D: DICKENS
 G: GERMAN-LAW
[no corpus]> DICKENS;
DICKENS> "preserve";
    42790: d dog-kennel are - a very <preserve> of butterflies , as I rem
   469137: cled in their orbits , to <preserve> inviolate a system of whi
   480711: e covered up - perhaps to <preserve> it for the son with whom
   547487: ghton , Sussex , ' and to <preserve> them in his desk with gre
   579253: e could do no better than <preserve> her image in his mind as
   791335: ions , and as I choose to <preserve> the decencies of life , s
   855396: atly relieved . I wish to <preserve> the good opinion of all h
  1151311: creature , barely able to <preserve> her sitting posture by st
  1161944: iss Louisa that you still <preserve> that bottle . Well ! If y
  1162727: it too strong , and so we <preserve> an understanding . I say
  1265120: aming countenance he will <preserve> for half-an-hour afterwar
  1294118: , in order that she might <preserve> that appearance of being
  1324634: to a trunk were charms to <preserve> its owner from sorrow and
  1421919: onal inducement to her to <preserve> the strictest silence reg
  1518169: ldly ; ' it would help to <preserve> habits of frugality , you
  1633852: is head , and prosper and <preserve> him . She was hurrying pa
  1648475: actions , and that we may <preserve> the self-respect which a
  1655346:  situated as Squeers , to <preserve> such a friend as himself-
  1751566: awing forth her dagger to <preserve> the one at the cost of th
  2038824: mposure I would desire to <preserve> ) , and your mother , are
  2123621:  destroy it , in order to <preserve> the property to his broth
  2330196:  exerting every muscle to <preserve> the advantage they have g
  2355400: ly open ? Nonsense . Just <preserve> the order for an autograp
  2377358: ardswoman is appointed to <preserve> order , and a similar reg
  2464296: l ineffectual attempts to <preserve> his perpendicular , the y
  2532203: eman , winking upon us to <preserve> silence , won a pair of g
  2532831: hey cannot do better than <preserve> and maintain - we say , a
  2533110: ife will be , to gain and <preserve> the esteem of your husban
  2547103: they are still careful to <preserve> their character for amiab
  2551743: ntle trot , the better to <preserve> the circulation , and bri
  2642877: id the Marquis , " I will <preserve> the honour and repose of
  2696198: . It was a hard matter to <preserve> the innocent deceit of wh
  2757818:  after several efforts to <preserve> his gravity , burst into
  2833250: yed himself to and fro to <preserve> his balance , and , looki
  2859861: nt to do , but meaning to <preserve> him or be killed herself
  2915948: h , but is constrained to <preserve> a decent solemnity , and
  3062070: nsport of frenzy . ' Lord <preserve> us ! ejaculated Mr. Pickw
  3224852: their arms around them to <preserve> them from danger . An ins
  3248360:  the table a good deal to <preserve> order ; then he had a con
  3299062: smiling , but managing to <preserve> his gravity , he drew for
  3305584: at the back of a chair to <preserve> his perpendicular . Mr. S
DICKENS> GERMAN-LAW;
GERMAN-LAW> "bewahren";
    55171: eiten Verschwiegenheit zu <bewahren> . Dies gilt nicht f�r Mit
   538194: instweilen vor Schaden zu <bewahren> . � 363 . ( 1 ) Anweisung
   646075: ngt , Verschwiegenheit zu <bewahren> ; � 138 des Strafgesetzbu
GERMAN-LAW> exit;
Bogdans-MBP:~ bogdan$

~~~

If this runs, the cqp is properly configured.

### C. Test CWB throught perl CGI API via the web interface























