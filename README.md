# ansible-rickrolling

Rickrolling for 404 requests


## Description

Every webserver log is spammed with [404 (Not Found)](https://en.wikipedia.org/wiki/HTTP_404) requests. The setup in this repository allows to redirect such requests to the famous [Rickrolling](https://en.wikipedia.org/wiki/Rickrolling) [Video](https://www.youtube.com/watch?v=dQw4w9WgXcQ).


## Ansible

To ease configuration and deployment, Ansible is used and generates a configuration file. The input URLs (the 404) are listed in two files:

* redirect.txt: simple URLs, including the full request, no regexp required
* redirectmatch.txt: complex URLs, a regexp is required to match the URL

The "Redirect" and "RedirectMatch" names are based on the Apache2 [mod_alias Module](https://httpd.apache.org/docs/current/mod/mod_alias.html). It is not necessary to enable the more complex _mod_rewrite_ module for simple redirects.


### Data Source

Check out this repository and point your Playbook to the _redirect.txt_ and _redirectmatch.txt_ files.

```
- name: Read block lists and set config
  set_fact:
    rickrolling_destination: "https://www.youtube.com/watch?v=dQw4w9WgXcQ"
    rickrolling_redirect: "{{ lookup('file', '/path/to/ansible-rickrolling/redirect.txt').splitlines() }}"
    rickrolling_redirectmatch: "{{ lookup('file', '/path/to/ansible-rickrolling/redirectmatch.txt').splitlines() }}"


- name: Install block list
  template:
    src: "/path/to/ansible-rickrolling/apache2.conf"
    dest: "/etc/apache2/rickrolling.conf"
    owner: www-data
    group: www-data
    mode: 0755
```


## Support

You can send PRs with additional URLs, or config examples for other webservers.
