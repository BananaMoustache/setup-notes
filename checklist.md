## Tool Validation

Run the following command to verify that the required tools are installed and available in `$PATH`:

```bash
for cmd in \
subfinder httpx nuclei katana naabu dnsx \
nmap ffuf sqlmap arjun \
gau waybackurls \
amass feroxbuster \
whatweb wafw00f masscan \
dalfox trufflehog gitleaks \
mitmproxy jq tmux \
msfconsole searchsploit hashcat john \
wireshark
do
    printf "%-18s" "$cmd"
    command -v "$cmd" || echo "NOT FOUND"
done
```

Installed tools will display their binary path. If `NOT FOUND` appears, the tool is either not installed or not available in `$PATH`.
