```yaml
---
- name: nxinx for otus
  hosts: web
  become: true

  tasks:
    - name: install nginx
      ansible.builtin.apt:
        name: nginx
        state: latest
        update_cache: yes

    - name: Fix port nginx
      ansible.builtin.lineinfile:
        path: /etc/nginx/sites-enabled/default
        line: '        listen 8080 default_server;'
        search_string: '        listen 80 default_server;'
        mode: "0644"
      notify:
        - Restart nginx

    - name: NGINX | Create NGINX config file from template
      ansible.builtin.copy:
        src: src/nginx.conf.j2
        dest: /tmp/nginx.conf
        remote_src: fasle
      notify:
        - Start nginx

  handlers:
    - name: Restart nginx
      ansible.builtin.systemd_service:
        name: nginx
        state: restarted
...
```
inventory 
vm1
vm2
vm3
vm4
vm5
[web]
vm2

