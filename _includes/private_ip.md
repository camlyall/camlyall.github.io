```bash
PRIV_IP=$(ip addr show ens5 | grep "inet\b" | awk '{print $2}' | cut -d/ -f1)
echo $PRIV_IP
```
