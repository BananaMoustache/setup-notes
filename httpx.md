## Remove Default HTTPX

Check which package owns the default `httpx` binary:

```bash
dpkg -S /usr/bin/httpx
```

Simulate the removal first:

```bash
sudo apt -s purge python3-httpx
```

Review the `REMOVED` packages carefully. If no important dependencies will be removed, proceed:

```bash
sudo apt purge python3-httpx
```

Refresh the shell:

```bash
hash -r
```

Verify that the default binary is removed:

```bash
command -v /usr/bin/httpx || echo "Default HTTPX removed"
```

Then install the ProjectDiscovery version using PDTM:

```bash
pdtm -i httpx
```

Final validation:

```bash
which -a httpx
httpx -version
```
