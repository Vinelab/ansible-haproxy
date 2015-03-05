# Ansible HAProxy
Install HAProxy on Centos/Red Hat with Ansible

## Installation
- Clone this repository inside your ```roles``` direcotry

```bash
git clone https://github.com/Vinelab/ansible-haproxy.git roles/haproxy
```

or add as submodule: 

```bash
git submodule add git@github.com:Vinelab/ansible-haproxy roles/haproxy
```

- In your playbook:

```yaml
roles:
  - haproxy
```

## Synopsis
Installs haproxy and configures backends and frontends, also includes the default haproxy configuration but will use ```templates/haproxy.cfg.j2``` in the process.

Make sure to have the ACL titles unique as they will be used as match conditions. i.e.
```title: static``` with acl ```path_beg -i /static /images /javascript /stylesheets``` will result in
```
acl is_static path_beg -i /static /images /javascript /stylesheets
use_backend static if is_static
```

## Usage

```

haproxy:
  default: toronto
  backends:
    - title: atlanta
      acls:
        - hdr_dom(host) -i blog.website.url
      servers:
        - title: web-1
          address: 172.17.0.3:80
        - title: web-2
          address: 172.17.0.4:80

    - title: toronto
      acls:
        - path_beg -i /static /images /javascript /stylesheets
      servers:
        - title: cdn-holy
          address: 172.17.0.6:80
        - title: cdn-guacamole
          address: 172.17.0.9:80

```
