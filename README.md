# Ansible Role: AEM Security Test

[![Build Status](https://travis-ci.org/aemdesign/ansible-role-aem-security-test.svg?branch=master)](https://travis-ci.org/aemdesign/ansible-role-aem-security-test)

This role tests AEM Dispatcher instance for specific Security patterns.
> This role was developed as part of
> [AEM.Design](http://aem.design/)

## Requirements

None.

## Role Variables

Available variables are listed below, along with default values (see `defaults/main.yml`):

    testtarget_url: "http://localhost:4502"
    is_admin_password_default: false
    admin_user: admin
    admin_password: admin
    test_group_list:
      - name: "Projects Access"
        type:
          - author
        authenticated: true
        category: "General"
        url: "{{ testtarget_url }}/projects.html"
        tests:
          - { 
            name: "Name of Test",
            user: "admin", 
            password: "{{ admin_password }}", 
            valid_status_code: "{{ 200 if (is_admin_password_default) else 401 }}" 
            body: "writetest=success", 
            method: "POST",
            headers: { CQ-Handle: "/content", CQ-Path: "/content"}
            }

Running ansible with variables will 

This will indicate that you have a default admin:admin login creds

     -e is_admin_password_default=true

This will indicate that you want to only run writetest

    -e type=writetest


## Dependencies

None.

## Example Playbook

```yaml
- hosts: all
  roles:
    - aemdesign.aem-security-test
```

## License

Apache 2.0

## Author Information

This role was created by [Max Barrass](https://aem.design/).