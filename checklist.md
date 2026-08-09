## Tool Validation

Run the following command to verify that the required tools are installed and available in `$PATH`:

```bash
for cmd in \
nmap ffuf sqlmap \
john hashcat searchsploit \
subfinder httpx nuclei katana naabu dnsx \
gau waybackurls arjun
do
    printf "%-15s" "$cmd"
    command -v "$cmd" || echo "NOT FOUND"
done
```

Installed tools will display their binary path. If `NOT FOUND` appears, the tool is either not installed or not available in `$PATH`.
