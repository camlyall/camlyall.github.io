```bash
sudo systemctl daemon-reload
sudo systemctl enable --now {{ include.content }}
sudo systemctl status {{ include.content }}
```
