Role Name
=========

Installs and configures Gerrit Code Review...https://www.gerritcodereview.com/

Requirements
------------

None

Role Variables
--------------

````
---
# defaults file for ansible-gerrit
gerrit_account_info:
  - user: gerrit
    comment: Gerrit User
    home: '{{ gerrit_site_dir }}'
    group: gerrit
gerrit_auth_type: OPENID  #defines authorization type...OPENID, LDAP...
gerrit_db_info:
  - host: localhost
    type: h2
    db: db/ReviewDB
    user: gerrit
    pass: gerrit
#  - host: localhost
#    type: mysql
#    db: reviewdb
#    user: gerrit
#    pass: gerrit
gerrit_dl_info:
  - url: https://www.gerritcodereview.com/download
    filename: 'gerrit-{{ gerrit_version }}.war'
    sha256sum: cb1794ccdf22da4e0ba5a431b832578017bbe53152ce028f46d2ebbb611705aa
gerrit_gitweb_integration: false  #defines is gerrit should be integrated with gitweb...gitweb is not controlled via gerrit using this method...
gerrit_install_plugins: []
gerrit_install_dir: /opt/gerrit
gerrit_java_home: /usr/lib/jvm/java-7-openjdk-amd64/jre
gerrit_mysql_connector_file: mysql-connector-java-5.1.21.jar
gerrit_mysql_connector_url: http://repo2.maven.org/maven2/mysql/mysql-connector-java/5.1.21
gerrit_plugins:
  - url: https://storage.cloud.google.com/gerritcodereview-plugins/plugins/master/commit-message-length-validator
    name: commit-message-length-validator.jar
  - url: https://storage.cloud.google.com/gerritcodereview-plugins/plugins/master/download-commands
    name: download-commands.jar
  - url: https://gerrit-ci.gerritforge.com/job/plugin-github-mvn-stable-2.11/lastSuccessfulBuild/artifact/github-oauth/target
    name: github-oauth-2.11.jar
  - url: https://gerrit-ci.gerritforge.com/job/plugin-github-mvn-stable-2.11/lastSuccessfulBuild/artifact/github-plugin/target
    name: github-plugin-2.11.jar
  - url: https://storage.cloud.google.com/gerritcodereview-plugins/plugins/master/replication
    name: replication.jar
  - url: https://storage.cloud.google.com/gerritcodereview-plugins/plugins/master/reviewnotes
    name: reviewnotes.jar
  - url: https://storage.cloud.google.com/gerritcodereview-plugins/plugins/master/singleusergroup
    name: singleusergroup.jar
gerrit_service_name: gerrit
gerrit_site_dir: /var/gerrit
gerrit_smtp_server: 'smtp.{{ pri_domain_name }}'
gerrit_sshd_listen_port: 29418
gerrit_version: 2.11.4
gerrit_http_listen_port: 8080
gitweb_cgi_path: /usr/share/gitweb/gitweb.cgi
pri_domain_name: example.org
````

Dependencies
------------

None

Example Playbook
----------------
````
---
- name: Installs Gerrit Code Review
  hosts: gerrit-servers
  sudo: true
  vars:
    - gerrit_db_info:
#        - host: localhost
#          type: h2
#          db: db/ReviewDB
#          user: gerrit
#          pass: gerrit
        - host: localhost
          type: mysql
          db: reviewdb
          user: gerrit
          pass: gerrit
    - install_mysql: true
  roles:
    - ansible-apache2
    - { role: ansible-mariadb-mysql, when: install_mysql is defined and install_mysql }
    - ansible-gerrit
  tasks:
````

Notes
-----
Portions of this role have been borrowed from https://github.com/kbrebanov/ansible-gerrit

License
-------

BSD

Author Information
------------------

Larry Smith Jr.
- @mrlesmithjr
- http://everythingshouldbevirtual.com
- mrlesmithjr [at] gmail.com
