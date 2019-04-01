# Ansible Role: AEM Security Test

This role tests AEM Dispatcher instance for specific Security patterns.
> This role was developed as part of
> [AEM.Design](http://aem.design/)

## Requirements

None.

## Role Variables

Available variables are listed below, along with default values (see `defaults/main.yml`):

    test_url: http://localhost:8080


## Dependencies

None.

## Example Playbook

```yaml
- hosts: all
  roles:
    - geerlingguy.docker
```

## License

Apache 2.0

## Author Information

This role was created by [Max Barrass](https://aem.design/).