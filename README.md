# Ansible-Script.yml
Test description

git remote add origin https://github.com/prafulbusa/Ansible-Script.yml.git
git push -u origin master

---
- hosts: webservers
  vars:
    http_port: 80
    max_clients: 200
  remote_user: root
  tasks:
  - name: ensure apache is at the latest version
    yum:
      name: httpd
      state: latest
  - name: write the apache config file
    template:
      src: /srv/httpd.j2
      dest: /etc/httpd.conf
    notify:
    - restart apache
  - name: ensure apache is running (and enable it at boot)
    service:
      name: httpd
      state: started
      enabled: yes
  handlers:
    - name: restart apache
      service:
        name: httpd
        state: restarted
