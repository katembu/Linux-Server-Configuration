<h1 id="linux-server-configuration">Linux Server Configuration</h1>



<h2 id="linux-apache-flask-postgres-docker-containers">Linux - Apache - Flask - Postgres - Docker Containers</h2>

<p>Script that takes a baseline Linux Server and automates the configuration that secures the system from a number of attack vectors, serves a Postgres database server and an Apache mod-wsgi server. <br>
The Postgres Database Server is configured to run in a <a href="https://www.docker.com"><strong>Docker</strong></a> container that uses a docker data volume for easy migrations, backups and restores. <br>
Th Apache server is dockerized and linked to the Database Server container for a more secure communication style.</p>

<h2 id="table-of-contents">Table of contents</h2>

<p><div class="toc">
<ul>
<li><a href="#linux-server-configuration">Linux Server Configuration</a><ul>
<li><a href="#linux-apache-flask-postgres-docker-containers">Linux - Apache - Flask - Postgres - Docker Containers</a></li>
<li><a href="#table-of-contents">Table of contents</a></li>
<li><a href="#how-to-use">How to use:</a><ul>
<li><ul>
<li><a href="#1-if-not-already-installed-install-openssh-ubuntu-1">1. If not already installed, install openssh Ubuntu 1</a></li>
<li><a href="#2-install-git">2. Install git:</a></li>
<li><a href="#3-clone-the-repository-into-src">3. Clone the repository into /src:</a></li>
<li><a href="#4-login-as-root-and-run-s1sh-from-the-shell-directory-cyberciti-1">4.  Login as root and run s1.sh from the “shell” directory Cyberciti 1</a></li>
<li><a href="#5-the-ssh-port-is-now-changed-to-2200-exit-to-your-machine-generate-an-rsa-key-and-upload-it-to-the-remote-server-on-port-2200-and-user-grader-digital-ocean-1">5. The ssh port is now changed to 2200, exit to your machine, generate an rsa key and upload it to the remote server on port 2200 and user grader Digital Ocean 1</a></li>
<li><a href="#6-login-as-root-and-run-the-seccond-script">6.  Login as root and run the seccond script</a></li>
<li><a href="#7-done-you-should-now-have-a-working-application-everything-set-up">7. DONE! You should now have a working application everything set up.</a></li>
</ul>
</li>
</ul>
</li>
<li><a href="#user-management">User Management</a><ul>
<li><ul>
<li><a href="#1-a-new-user-has-been-created-digital-ocean-2">1. A new user has been created Digital Ocean 2</a></li>
<li><a href="#2-user-grader-can-sudo-to-root-and-the-password-has-been-set-securely">2. User “grader” can sudo to root and the password has been set securely.</a></li>
<li><a href="#3-remote-users-other-then-grader-have-been-disabled">3. Remote users other then ‘grader’ have been disabled.</a></li>
</ul>
</li>
</ul>
</li>
<li><a href="#security-app-functionality-monitoring-feedback">Security / App Functionality Monitoring - Feedback</a><ul>
<li><ul>
<li><a href="#1-key-based-ssh-has-been-enforced-unixhelp">1. Key-based ssh has been enforced. UnixHelp</a></li>
<li><a href="#2-ssh-accessible-over-non-default-port-2200-digital-ocean-3">2. SSH accessible over non-default port 2200. Digital Ocean 3</a></li>
<li><a href="#4-the-firewall-has-been-configured-to-monitor-for-repeated-unsuccessful-attempts-appropriately-bans-attackers-and-provides-automated-security-feedback-digital-ocean-4">4. The firewall has been configured to monitor for repeated unsuccessful attempts, appropriately bans attackers and provides automated security feedback. Digital Ocean 4</a></li>
</ul>
</li>
</ul>
</li>
<li><a href="#other-application-functionality">Other Application Functionality</a></li>
<li><a href="#other-security-functionality-configurations">Other Security / Functionality Configurations</a><ul>
<li><ul>
<li><a href="#1-set-up-firewall-to-only-allow-connections-over-ports-123-2200-and-80-digital-ocean-7">1. Set up firewall to only allow connections over ports 123, 2200 and 80 Digital Ocean 7</a></li>
<li><a href="#2-install-ntp-for-better-time-synchronization-ntporg">2. Install NTP for better time synchronization ntp.org</a></li>
</ul>
</li>
</ul>
</li>
<li><a href="#description-of-the-system">Description of the System</a><ul>
<li><a href="#apache-flask-application-container">Apache - Flask Application Container</a><ul>
<li><a href="#1-packages-are-updated">1. Packages are updated:</a></li>
<li><a href="#3-flaskappwsgi-is-coppied-to-application-directory">3. flaskapp.wsgi is coppied  to application directory</a></li>
<li><a href="#4-flaskappconf-is-coppied-to-the-proper-directory">4. FlaskApp.conf is coppied to the proper directory</a></li>
<li><a href="#5-the-site-is-enabled-while-the-default-site-is-disabled">5. The site is enabled while the default site is disabled</a></li>
</ul>
</li>
<li><a href="#postgres-database-server-container">Postgres Database Server Container</a><ul>
<li><a href="#1-install-postgres-and-update-all-packages">1. Install Postgres and Update all packages</a></li>
<li><a href="#2-setup-database">2. Setup database</a></li>
</ul>
</li>
<li><a href="#tying-it-all-together">Tying it all together</a><ul>
<li><a href="#1-install-docker">1. Install Docker</a></li>
<li><a href="#2-build-the-images">2. Build the images</a></li>
<li><a href="#3-run-the-containers">3. Run the containers</a></li>
</ul>
</li>
</ul>
</li>
<li><a href="#documents">Documents</a></li>
</ul>
</li>
</ul>
</div>
</p>

<h2 id="how-to-use">How to use:</h2>



<h4 id="1-if-not-already-installed-install-openssh-ubuntu-1">1. If not already installed, install openssh <a href="https://help.ubuntu.com/lts/serverguide/openssh-server.html">Ubuntu 1</a></h4>

<blockquote>
  <p>$<code>sudo apt-get openssh-server</code></p>
</blockquote>



<h4 id="2-install-git">2. Install git:</h4>

<blockquote>
  <p>$<code>sudo apt-get install git</code></p>
</blockquote>



<h4 id="3-clone-the-repository-into-src">3. Clone the repository into /src:</h4>

<blockquote>
  <p>$ <code>sudo git clone</code>[your source]  <code>/src</code></p>
</blockquote>



<h4 id="4-login-as-root-and-run-s1sh-from-the-shell-directory-cyberciti-1">4.  Login as root and run s1.sh from the “shell” directory <a href="http://www.cyberciti.biz/faq/run-execute-sh-shell-script/">Cyberciti 1</a></h4>

<blockquote>
  <p>$ <code>sudo su</code> <br>
  $ <code>sudo su</code> <br>
  $ <code>sh /src/shell/s1.sh</code></p>
  
  <blockquote>
    <p><em>here you will be asked to configure unattended-upgrades, timezone, and the password for the new user “grader”</em></p>
  </blockquote>
</blockquote>



<h4 id="5-the-ssh-port-is-now-changed-to-2200-exit-to-your-machine-generate-an-rsa-key-and-upload-it-to-the-remote-server-on-port-2200-and-user-grader-digital-ocean-1">5. The ssh port is now changed to 2200, exit to your machine, generate an rsa key and upload it to the remote server on port 2200 and user grader <a href="https://www.digitalocean.com/community/tutorials/how-to-set-up-ssh-keys--2">Digital Ocean 1</a></h4>

<blockquote>
  <p><code>ssh-keygen -t rsa</code> <em>set a file location (usually: /Users/you/.ssh/yourfile) - Mac</em> <br>
  <code>ssh-copy-id -p 2200 -i /Users/you/.ssh/yourfile grader@ip_addr</code> <br>
  <code>ssh -p 2200 -i  /Users/you/.ssh/yourfile grader@ip_addr</code></p>
</blockquote>



<h4 id="6-login-as-root-and-run-the-seccond-script">6.  Login as root and run the seccond script</h4>

<blockquote>
  <p>$ <code>sh /src/shell/s2.sh</code></p>
</blockquote>



<h4 id="7-done-you-should-now-have-a-working-application-everything-set-up">7. DONE! You should now have a working application everything set up.</h4>



<h2 id="user-management">User Management</h2>



<h4 id="1-a-new-user-has-been-created-digital-ocean-2">1. A new user has been created <a href="https://www.digitalocean.com/community/tutorials/how-to-add-and-delete-users-on-an-ubuntu-14-04-vps">Digital Ocean 2</a></h4>

<blockquote>
  <p>Add a new user grader</p>
  
  <blockquote>
    <p>$ <code>adduser --gecos "" grader</code></p>
  </blockquote>
  
  <p>make a copy of the sudoers file to the temp /etc/sudoers.tmp</p>
  
  <blockquote>
    <p>$<code>cp /etc/sudoers /etc/sudoers.tmp</code></p>
  </blockquote>
  
  <p>make a backup of the sudoers file</p>
  
  <blockquote>
    <p>$<code>cp /etc/sudoers /etc/sudoers.bak</code></p>
  </blockquote>
  
  <p>change sudoers file</p>
  
  <blockquote>
    <p><em>extra:</em> effectively also remove root from sudoers <br>
    $ <code>word='root[[:space:]]*ALL=(ALL:ALL)[[:space:]]ALL'</code></p>
  </blockquote>
  
  <p>replace it with grader</p>
  
  <blockquote>
    <p>$ <code>rep="grader     ALL=(ALL:ALL) NOPASSWD: ALL"</code></p>
  </blockquote>
  
  <p>sed to execute this replacement.</p>
  
  <blockquote>
    <p><code>sed -i "s/${word}/${rep}/" /etc/sudoers.tmp</code></p>
  </blockquote>
  
  <p><em>extra:</em> remove the necessity for password for sudo, once done with dev remove “NOPASSWD: ” <a href="http://askubuntu.com/questions/235084/how-do-i-remove-ubuntus-password-requirement">AskUbuntu 1</a> Sed Resources<a href="#fn:sed" id="fnref:sed" title="See footnote" class="footnote">1</a>.</p>
  
  <blockquote>
    <p>$ <code>word='%sudo[[:space:]]*ALL=(ALL:ALL)[[:space:]]ALL'</code> <br>
    $<code>rep='%sudo ALL=(ALL:ALL) NOPASSWD: ALL'</code> <br>
    $<code>sed -i "s/${word}/${rep}/g" /etc/sudoers.tmp</code></p>
  </blockquote>
  
  <p>move the sudoers file back</p>
  
  <blockquote>
    <p>$ <code>mv /etc/sudoers.tmp /etc/sudoers</code></p>
  </blockquote>
  
  <p>make the sudoers readonly <a href="http://www.cyberciti.biz/faq/howto-set-readonly-file-permission-in-linux-unix/">Cyberciti 2</a></p>
  
  <blockquote>
    <p>$ <code>chmod 0444 /etc/sudoers</code></p>
  </blockquote>
</blockquote>



<h4 id="2-user-grader-can-sudo-to-root-and-the-password-has-been-set-securely">2. User “grader” can sudo to root and the password has been set securely.</h4>



<h4 id="3-remote-users-other-then-grader-have-been-disabled">3. Remote users other then ‘grader’ have been disabled.</h4>

<blockquote>
  <blockquote>
    <p>add grader to the list of AllowedUsers for ssh <br>
    done this by adding AllowUsers grader to sshd_config <a href="https://www.digitalocean.com/community/tutorials/how-to-tune-your-ssh-daemon-configuration-on-a-linux-vps">Digital Ocean 3</a> <br>
    $ <code>if grep -q "AllowUsers" /etc/ssh/sshd_config; then</code> <br>
    $ <code>word='AllowUsers'</code> <br>
    $ <code>rep='AllowUsers grader'</code> <br>
    $ <code>sed -i "s/${word}/${rep}/" /etc/ssh/sshd_config</code> <br>
    $ <code>else</code> <br>
    $ <code>echo "AllowUsers grader" &gt;&gt; /etc/ssh/sshd_config</code> <br>
    $ <code>fi</code></p>
  </blockquote>
</blockquote>



<h2 id="security-app-functionality-monitoring-feedback">Security / App Functionality Monitoring - Feedback</h2>



<h4 id="1-key-based-ssh-has-been-enforced-unixhelp">1. Key-based ssh has been enforced. <a href="http://unixhelp.ed.ac.uk/CGI/man-cgi?sshd_config">UnixHelp</a></h4>

<blockquote>
  <p>changed to PasswordAuthentication = no in sshd_config file</p>
  
  <blockquote>
    <p>$ <code>word='#PasswordAuthentication[[:space:]]*yes'</code> <br>
    $ <code>rep='PasswordAuthentication no'</code> <br>
    $ <code>sed -i "s/${word}/${rep}/g" /etc/ssh/sshd_config</code></p>
  </blockquote>
</blockquote>



<h4 id="2-ssh-accessible-over-non-default-port-2200-digital-ocean-3">2. SSH accessible over non-default port 2200. <a href="https://www.digitalocean.com/community/tutorials/how-to-tune-your-ssh-daemon-configuration-on-a-linux-vps">Digital Ocean 3</a></h4>

<blockquote>
  <p>change port from 22 to 2200 in sshd_config</p>
  
  <blockquote>
    <p>$ <code>word='Port[[:space:]]22'</code> <br>
    $ <code>rep='Port 2200'</code> <br>
    $ <code>sed -i "s/${word}/${rep}/g" /etc/ssh/sshd_config</code> </p>
  </blockquote>
</blockquote>

<ol>
<li>Applications have been updated to the most recent updates. <a href="http://askubuntu.com/questions/94102/what-is-the-difference-between-apt-get-update-and-upgrade">AskUbuntu 2</a> <br>


<blockquote>
  <blockquote>
    $ <code>apt-get update</code> <br>
    $ <code>apt-get upgrade</code></blockquote></blockquote></li>
    </ol>
    
  


<h4 id="4-the-firewall-has-been-configured-to-monitor-for-repeated-unsuccessful-attempts-appropriately-bans-attackers-and-provides-automated-security-feedback-digital-ocean-4">4. The firewall has been configured to monitor for repeated unsuccessful attempts, appropriately bans attackers and provides automated security feedback. <a href="https://www.digitalocean.com/community/tutorials/how-to-install-and-use-fail2ban-on-ubuntu-14-04">Digital Ocean 4</a></h4>



<blockquote>
  <p>change bantime to 1800 sec</p>
  
  <blockquote>
    <p>$ <code>word="bantime[[:space:]]*=[[:space:]][[:digit:]]*"</code> <br>
    $ <code>rep="bantime = 1800"</code> <br>
    $ <code>sed -i "s/${word}/${rep}/" /etc/fail2ban/jail.local</code></p>
  </blockquote>
  
  <p>change the email to my email</p>
  
  <blockquote>
    <p>$ <code>word="destemail[[:space:]]*=[[:space:]]root@localhost"</code> <br>
    $ <code>rep="destemail = myemail@example.com"</code> <br>
    $<code>sed -i "s/${word}/${rep}/" /etc/fail2ban/jail.local</code></p>
  </blockquote>
  
  <p>change the ssh port in jail.local</p>
  
  <blockquote>
    <p>$ <code>sed -i "/\[ssh\]/{N</code> <br>
    <code>N</code> <br>
    <code>N</code> <br>
    <code>s/\(\[ssh\]\n.*\n.*\)\n.*/\1\nport=2200/}" /etc/fail2ban/jail.local</code> <br>
    make sure that feedback is provided via email in case of brute force attack attempts <br>
    $ <code>sed -i "s/\(action_)/(action_mwl)/" /etc/fail2ban/jail.local</code> <br>
    ensure that fail2ban can send the emails <br>
    $ <code>apt-get install -y sendmail</code></p>
  </blockquote>
</blockquote>

<ol>
<li>A monitoring software was installed to monitor system availability and status. <a href="http://shinken.readthedocs.org/en/branch-1.4/89_packages/glances.html">Python 1</a> <a href="http://glances.readthedocs.org/en/latest/glances-doc.html#introduction">Readthedocs 2</a> <br>


<blockquote>
  <blockquote>
    $ <code>apt-get install -y python-pip build-essential python-dev</code> <br>
    $ <code>pip install Glances</code></blockquote></blockquote></li>
    </ol>
    
  


<h2 id="other-application-functionality">Other Application Functionality</h2>

<ol>
<li><p>Web-server has been <a href="https://www.digitalocean.com/community/tutorials/docker-explained-how-to-containerize-python-web-applications">dockerized (1)</a> for security and portability; configured to serve the provided application and has been configured to automatically restart in case of critical failure.</p></li>
<li><p>Database Server has been <a href="https://docs.docker.com/examples/postgresql_service/">dockerized (2)</a> for security, portability, has been configured to use a <a href="https://docs.docker.com/userguide/dockervolumes/">data volume</a> for easy migrations, backups and restores.</p>

<blockquote>
  <p><strong>Note: </strong> <em>Even though it looks like remote connections have been enabled for the database it is important to notice that the database is not actually accessible remotely from any machine unless it is a purposefully <a href="https://docs.docker.com/userguide/dockerlinks/">linked docker container</a>. Technically by dockerizing the database server, another layer of security was added.</em></p>
</blockquote></li>
</ol>



<h2 id="other-security-functionality-configurations">Other Security / Functionality Configurations</h2>



<h4 id="1-set-up-firewall-to-only-allow-connections-over-ports-123-2200-and-80-digital-ocean-7">1. Set up firewall to only allow connections over ports 123, 2200 and 80 <a href="https://www.digitalocean.com/community/tutorials/how-to-setup-a-firewall-with-ufw-on-an-ubuntu-and-debian-cloud-server">Digital Ocean 7</a></h4>

<blockquote>
  <p>ufw allow 123/udp <br>
  ufw allow 2200/tcp <br>
  ufw allow 80/tcp <br>
  ufw enable</p>
</blockquote>



<h4 id="2-install-ntp-for-better-time-synchronization-ntporg">2. Install NTP for better time synchronization <a href="http://support.ntp.org/bin/view/Support/ConfiguringNTP#Section_6.10.">ntp.org</a></h4>

<blockquote>
  <p>apt-get install -y ntp</p>
</blockquote>



<h2 id="description-of-the-system">Description of the System</h2>

<p>The system is set up to allow connections only on port 2200 for SSH, 123 for NTP and 80 for HTTP. <br>
Port 80 is forwarded into the Docker container that runs the Apache server. <br>
The Apache server container and the Postgres container are built from a <a href="https://docs.docker.com/reference/builder/">Dockerfile</a> that has all the configuration settings.</p>

<p><img src="https://github.com/robertavram/project5/blob/master/system_diag.png?raw=true" alt="system diagram" title=""></p>



<h3 id="apache-flask-application-container">Apache - Flask Application Container</h3>

<p>Sources: <a href="https://www.digitalocean.com/community/tutorials/how-to-deploy-a-flask-application-on-an-ubuntu-vps">flask.poccoo</a>, <a href="https://www.digitalocean.com/community/tutorials/how-to-deploy-a-flask-application-on-an-ubuntu-vps">Digital Ocean 5</a>, <a href="https://www.digitalocean.com/community/tutorials/docker-explained-how-to-containerize-python-web-applications">Digital Ocean 6</a></p>



<h4 id="1-packages-are-updated">1. Packages are updated:</h4>

<blockquote>
  <p><code>RUN apt-get -y update &amp;&amp; apt-get -y upgrade</code></p>
</blockquote>

<p></p><ol> <br>
<li>Flask, Sqlalchemy, psychopg2, apache2 and other prereqs are installed <br></li></ol><p></p>

<blockquote>
  <code>apt-get -y install python-flask python-sqlalchemy &amp;&amp; apt-get -y install python-psycopg2 &amp;&amp; apt-get -y install python-pip &amp;&amp;  apt-get -y install python2.7-dev &amp;&amp; apt-get -y install libjpeg-dev &amp;&amp; apt-get -y install zlib1g-dev &amp;&amp; apt-get -y install apache2 &amp;&amp; apt-get -y install python-setuptools &amp;&amp; apt-get -y install libapache2-mod-wsgi &amp;&amp; pip install pillow &amp;&amp; pip install oauth2client &amp;&amp; pip install dicttoxml</code>
  
  </blockquote>


<h4 id="3-flaskappwsgi-is-coppied-to-application-directory">3. flaskapp.wsgi is coppied  to application directory</h4>

<blockquote>
  <p><code>ADD flaskapp.wsgi /var/www/FlaskApp/</code></p>
  
  <blockquote>
    <p>flaskapp.wsgi contents:</p>
    
    <pre class="prettyprint"><code class=" hljs python"><span class="hljs-keyword">import</span> sys
<span class="hljs-keyword">import</span> logging
logging.basicConfig(stream=sys.stderr)
sys.path.insert(<span class="hljs-number">0</span>,<span class="hljs-string">"/var/www/FlaskApp/FlaskApp/"</span>)
<span class="hljs-keyword">from</span> application <span class="hljs-keyword">import</span> app <span class="hljs-keyword">as</span> application
application.secret_key = <span class="hljs-string">'super_secret_mumbo_jambo'</span></code></pre>
  </blockquote>
</blockquote>



<h4 id="4-flaskappconf-is-coppied-to-the-proper-directory">4. FlaskApp.conf is coppied to the proper directory</h4>

<blockquote>
  <p><code>ADD FlaskApp.conf /etc/apache2/sites-available/</code></p>
  
  <blockquote>
    <p>FlaskApp.conf contents:</p>
    
    <pre class="prettyprint"><code class=" hljs apache"><span class="hljs-tag">&lt;VirtualHost *:80&gt;</span>
                    <span class="hljs-keyword"><span class="hljs-common">ServerName</span></span><span class="hljs-sqbracket"> [SERVER IP]</span>
                    <span class="hljs-keyword">ServerAdmin</span><span class="hljs-sqbracket"> [admin@mywebsite.com]</span>
                    <span class="hljs-keyword">WSGIScriptAlias</span> / /var/www/FlaskApp/flaskapp.wsgi
                    <span class="hljs-keyword">ServerAlias</span><span class="hljs-sqbracket"> [HOSTNAME]</span>
                    <span class="hljs-tag">&lt;Directory /var/www/FlaskApp/FlaskApp/&gt;</span>
                        <span class="hljs-keyword"><span class="hljs-common">Order</span></span> allow,deny
                        <span class="hljs-keyword"><span class="hljs-common">Allow</span></span> from <span class="hljs-literal">all</span>
                    <span class="hljs-tag">&lt;/Directory&gt;</span>
                    <span class="hljs-keyword">Alias</span> /static /var/www/FlaskApp/FlaskApp/static
                    <span class="hljs-tag">&lt;Directory /var/www/FlaskApp/FlaskApp/static/&gt;</span>
                        <span class="hljs-keyword"><span class="hljs-common">Order</span></span> allow,deny
                        <span class="hljs-keyword"><span class="hljs-common">Allow</span></span> from <span class="hljs-literal">all</span>
                    <span class="hljs-tag">&lt;/Directory&gt;</span>
                    <span class="hljs-keyword">ErrorLog</span> <span class="hljs-cbracket">${APACHE_LOG_DIR}</span>/error.log
                    <span class="hljs-keyword">LogLevel</span> info
                    <span class="hljs-keyword">CustomLog</span> <span class="hljs-cbracket">${APACHE_LOG_DIR}</span>/access.log combined
<span class="hljs-tag">&lt;/VirtualHost&gt;</span></code></pre>
  </blockquote>
</blockquote>



<h4 id="5-the-site-is-enabled-while-the-default-site-is-disabled">5. The site is enabled while the default site is disabled</h4>

<blockquote>
  <p><code>RUN a2ensite FlaskApp</code> <br>
  <code>RUN a2dissite 000-default</code></p>
</blockquote>



<h3 id="postgres-database-server-container">Postgres Database Server Container</h3>

<p>Sources: <a href="http://www.postgresonline.com/downloads/special_feature/postgresql83_psql_cheatsheet.pdf">Docker 2</a> <a href="http://www.postgresql.org/docs/9.1/static/server-start.html">postgresql</a></p>



<h4 id="1-install-postgres-and-update-all-packages">1. Install Postgres and Update all packages</h4>

<blockquote>
  <p><code>RUN apt-key adv --keyserver keyserver.ubuntu.com --recv-keys B97B0AFCAA1A47F044F244A07FCC7D46ACCC4CF8</code></p>
  
  <p><code>RUN echo "deb http://apt.postgresql.org/pub/repos/apt/ precise-pgdg main" &gt; /etc/apt/sources.list.d/pgdg.list</code></p>
  
  <p><code>RUN apt-get update &amp;&amp; apt-get install -y python-software-properties software-properties-common postgresql-9.3 postgresql-client-9.3 postgresql-contrib-9.3</code></p>
</blockquote>



<h4 id="2-setup-database">2. Setup database</h4>

<blockquote>
  <p>Change user to postgres</p>
  
  <blockquote>
    <p><code>USER postgres</code></p>
  </blockquote>
  
  <p>Start database server, create user catalog with limited permissions, <br>
  Create db catalog with owner catalog <br>
  Remove all other users ability to modify the public schema of catalog</p>
  
  <blockquote>
    <p><code>RUN    /etc/init.d/postgresql start &amp;&amp;\ <br>
        psql --command "CREATE USER catalog WITH CREATEDB LOGIN PASSWORD 'catalogpasswordhere';" &amp;&amp;\ <br>
        createdb -O catalog catalog &amp;&amp;\ <br>
        psql -U postgres -d catalog -c "REVOKE ALL ON SCHEMA public FROM public;" &amp;&amp;\ <br>
        psql -U postgres -d catalog -c "GRANT ALL ON SCHEMA public TO catalog;"</code></p>
  </blockquote>
  
  <p>Adjust PostgreSQL configuration so that remote connections to the database are possible. <br>
  <strong>Note</strong>: <em>These connections are only possible from within other docker containers.  The ports are not exposed to the host machine unless specified so when running the container. Technically remote connections are impossible to the db.</em></p>
  
  <blockquote>
    <p><code>RUN echo "host all  all    0.0.0.0/0  md5" &gt;&gt; /etc/postgresql/9.3/main/pg_hba.conf</code> <br>
    <code>RUN echo "listen_addresses='*'" &gt;&gt; /etc/postgresql/9.3/main/postgresql.conf</code></p>
  </blockquote>
</blockquote>



<h3 id="tying-it-all-together">Tying it all together</h3>



<h4 id="1-install-docker">1. Install Docker</h4>

<blockquote>
  <p>Source: <a href="https://docs.docker.com/installation/ubuntulinux/">Docker 3</a>  <br>
  Make sure wget is installed:</p>
  
  <blockquote>
    <p>$ <code>apt-get install -y wget</code></p>
  </blockquote>
  
  <p>Make sure the key is added to apt</p>
  
  <blockquote>
    <p>$ <code>wget -qO- https://get.docker.com/gpg | apt-key add</code></p>
  </blockquote>
  
  <p>Get the latest Docker package</p>
  
  <blockquote>
    <p>$ <code>wget -qO- https://get.docker.com/ | sh</code></p>
  </blockquote>
</blockquote>



<h4 id="2-build-the-images">2. Build the images</h4>

<blockquote>
  <p>$ <code>docker build -t flutterhub:v1 /src/.</code> <br>
  $ <code>docker build -t flutterhubdb:v1 /src/db/.</code></p>
</blockquote>



<h4 id="3-run-the-containers">3. Run the containers</h4>

<blockquote>
  <p>Create the data volume</p>
  
  <blockquote>
    <p>$ <code>docker create -v /etc/postgresql -v /var/log/postgresql -v /var/lib/postgresql --name dbdata flutterhubdb:v1 /bin/true</code></p>
  </blockquote>
  
  <p>Start db container with the volumes from dbdata</p>
  
  <blockquote>
    <p>$ <code>docker run --restart=always -d --volumes-from dbdata --name db flutterhubdb:v1</code></p>
  </blockquote>
  
  <p>Start app container linked to the db container, and add the apache log dir on the host machine as a volume in the container in order to be able to monitor the logs from the host machine.</p>
  
  <blockquote>
    <p>$ <code>docker run --restart=always -d -v /var/log/apache2:/var/log/apache2 -p 80:80 --name web --link db:db flutterhub:v1</code></p>
  </blockquote>
</blockquote>



<h2 id="misc">Misc</h2>

<table>
<thead>
<tr>
  <th>Misc Problems</th>
  <th>Resource</th>
</tr>
</thead>
<tbody><tr>
  <td>Append multiple lines to a file with bash</td>
  <td><a href="http://unix.stackexchange.com/questions/77277/how-to-append-multiple-lines-to-a-file-with-bash">StackExchange</a></td>
</tr>
<tr>
  <td>Fail2Ban vs DenyHosts vs Iptables</td>
  <td><a href="http://serverfault.com/questions/128962/denyhosts-vs-fail2ban-vs-iptables-best-way-to-prevent-brute-force-logons">Serverfault</a></td>
</tr>
<tr>
  <td>Installing packages in linux</td>
  <td><a href="http://ceph.com/docs/v0.69/install/debian/">Ceph.org</a></td>
</tr>
<tr>
  <td>Docker/Apache/Posgress server not running on install</td>
  <td><a href="https://github.com/klaemo/docker-couchdb/issues/19">Github</a></td>
</tr>
</tbody></table>




<h2 id="footnotes">Footnotes</h2><div class="footnotes"><hr><ol><li id="fn:sed">Sed Resources - <a href="http://stackoverflow.com/questions/4437901/find-and-replace-string-in-a-file">StackOverflow 1</a>, <a href="http://pubs.opengroup.org/onlinepubs/009695399/utilities/sed.html">Opengroup 1</a>, <a href="http://pubs.opengroup.org/onlinepubs/009695399/basedefs/xbd_chap09.html#tag_09_03">Opengroup 2</a>, <a href="http://www.grymoire.com/Unix/Sed.html#uh-51">Grymoire</a>. <a href="#fnref:sed" title="Return to article" class="reversefootnote">↩</a></li></ol></div>