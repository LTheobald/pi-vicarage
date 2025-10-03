# host_vars for pi-nas

Put host-specific variables here, e.g.

```
app_branch: main
wifi_ssid: your-ssid
```

Sensitive values should go into a `vault.yml` created with:
```
ansible-vault create host_vars/pi-nas/vault.yml
```
