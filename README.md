# Ansible Role: AEM Security Test

[![Build Status](https://travis-ci.org/aem-design/ansible-role-aem-security-test.svg?branch=master)](https://travis-ci.org/aem-design/ansible-role-aem-security-test)

This role tests AEM Dispatcher instance for specific Security patterns.
> This role was developed as part of
> [AEM.Design](http://aem.design/)

## Requirements

None.

## Role Variables

Available variables are listed below, along with default values (see `defaults/main.yml`):

    test_target_url: "http://localhost:4502"
    is_admin_password_default: false
    admin_user: admin
    admin_password: admin
    type:
      - author
      - publish
      - dispatcher
      - writetest
      - services
    
To specify service that should be used as target for tests set the ```test_target_url``` variable

    test_target_url: "http://localhost:4502"

If you have default admin:admin credentials on the service set ```is_admin_password_default``` to true, this will use default admin:admin for service authentication when required

    is_admin_password_default: true

Running ansible playbook with this param will achieve desired outcome

     -e is_admin_password_default=true


If you have diffrent admin account set following variables to your specified values

    admin_user: admin
    admin_password: admin
    

Tests have types and they can be filtered by default all tests are executed

    type:
      - author
      - publish
      - dispatcher
      - writetest
      - services

You can run specific tests by specifying extra vars when running the playbook.
Following example will indicate that you want to only run dispatcher and services type:

    --extra-vars='{"type": [dispatcher,services]}'



## Test Template

Each test definition is used to drive Ansible uri_module. Following example template has all currently used options 

    test_group_list:
      - name: "Projects Access"
        type:
          - author
        user: "admin", 
        password: "{{ admin_password }}", 
        url: "{{ testtarget_url }}/projects.html"
        tests:
          - { 
            name: "Name of Test",
            user: "admin", 
            password: "{{ admin_password }}", 
            valid_status_code: "{{ 200 if (is_admin_password_default) else 401 }}" 
            body: "writetest=success", 
            timeout: 30,
            return_content: true,
            method: "POST",
            headers: { CQ-Handle: "/content", CQ-Path: "/content"}
            }

First level of the list the Groups of tests, this is used to create inherited ```user```, ```password``` and ```uri``` attributes, specifying these attributes at the group level does not require those attributes at child test level.

Each Group should have a ```type``` list that describes purpose for the group which is then used for filtering during test runs. 

Each Group should have a ```tests``` list that defines atomic url actions to be executed.

Each Test needs have following fields:
    - ```name``` field describing test outcome.
    - ```valid_status_code``` field with desired status code from url, defaults to 200
    - ```user``` field for desired user attribute for authentication, defaults to ''
    - ```password``` field for desired password attribute for authentication, defaults to ''
    - ```body``` field for sending data to url, defaults to ''
    - ```timeout``` field for duration to wait for a service, defaults to 5 sec
    - ```return_content``` field for setting if content should be returned, defaults to 'false'
    - ```method``` field describing action for url, defaults to 'GET'
    - ```headers``` field describing headers to be used for url, defaults to empty array
 

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