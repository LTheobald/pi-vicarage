# host_vars for pumpkin-pi

Put host-specific variables here, e.g.

```
app_branch: main
wifi_ssid: your-ssid
```

Sensitive values should go into a `vault.yml` created with:
```
ansible-vault create host_vars/pumpkin-pi/vault.yml
```
