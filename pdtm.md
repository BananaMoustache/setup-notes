# Setup PDTM

This guide explains how to configure **PDTM (ProjectDiscovery Tools Manager)** on Kali Linux, especially when tools installed through PDTM cannot be executed directly from the terminal because the PDTM binary directory is not included in the system `PATH`.

## 1. Check PDTM

Verify that PDTM is installed:

```bash
pdtm -version
```

Example output:

```text
Current pdtm version v0.1.3 (latest)
```

You can also run:

```bash
pdtm
```

to open the interactive ProjectDiscovery tools installer.

---

## 2. PDTM Installation Directory

By default, PDTM stores installed Go-based ProjectDiscovery binaries under:

```text
$HOME/.pdtm/go/bin
```

For example:

```text
/home/<username>/.pdtm/go/bin
```

When running as `root`, this becomes:

```text
/root/.pdtm/go/bin
```

Installed ProjectDiscovery tools will normally be placed inside this directory.

To check the directory:

```bash
ls -la $HOME/.pdtm/go/bin
```

---

## 3. PDTM PATH Warning

After installing PDTM, you may see a warning similar to:

```text
[WRN] Run `source ~/.zshrc` to add $PATH
[INF] Path to download project binary: $HOME/.pdtm/go/bin
[INF] PDTM binary path is not configured in environment variable $PATH
```

This means PDTM itself may work, but binaries installed through PDTM cannot yet be executed globally from the terminal.

For example:

```bash
nuclei
subfinder
httpx
katana
naabu
```

may return:

```text
command not found
```

---

## 4. Add PDTM to PATH

Kali Linux commonly uses `zsh`.

Open the shell configuration:

```bash
nano ~/.zshrc
```

Add the following line:

```bash
export PATH=$PATH:$HOME/.pdtm/go/bin
```

Save the file and reload the shell configuration:

```bash
source ~/.zshrc
```

---

## 5. Verify the PATH

Check whether the PDTM directory has been added:

```bash
echo $PATH
```

The output should contain:

```text
$HOME/.pdtm/go/bin
```

You can also check it directly:

```bash
echo $PATH | tr ':' '\n' | grep pdtm
```

Expected output:

```text
/home/<username>/.pdtm/go/bin
```

or, when running as root:

```text
/root/.pdtm/go/bin
```

---

## 6. Install ProjectDiscovery Tools

Start PDTM:

```bash
pdtm
```

The interactive menu will display available ProjectDiscovery tools.

Examples include:

```text
nuclei
subfinder
httpx
naabu
katana
dnsx
notify
interactsh-client
tlsx
uncover
```

Select the tools you want to install.

PDTM will download the binaries into:

```text
$HOME/.pdtm/go/bin
```

---

## 7. Verify Installed Tools

List installed binaries:

```bash
ls -la $HOME/.pdtm/go/bin
```

Then test individual tools.

Example:

```bash
nuclei -version
```

```bash
subfinder -version
```

```bash
httpx -version
```

```bash
katana -version
```

```bash
naabu -version
```

---

## 8. Check Binary Locations

Use `which` to verify that the correct binaries are being executed:

```bash
which nuclei
which subfinder
which httpx
which katana
which naabu
```

Expected output should look similar to:

```text
/home/<username>/.pdtm/go/bin/nuclei
/home/<username>/.pdtm/go/bin/subfinder
/home/<username>/.pdtm/go/bin/httpx
/home/<username>/.pdtm/go/bin/katana
/home/<username>/.pdtm/go/bin/naabu
```

When running as root:

```text
/root/.pdtm/go/bin/nuclei
```

---

## 9. Troubleshooting

### PDTM reports that its path is not configured

Example:

```text
PDTM binary path is not configured in environment variable $PATH
```

Add the PDTM binary directory:

```bash
echo 'export PATH=$PATH:$HOME/.pdtm/go/bin' >> ~/.zshrc
```

Then reload:

```bash
source ~/.zshrc
```

---

### Installed tool returns `command not found`

Example:

```text
nuclei: command not found
```

First, check whether the binary exists:

```bash
ls $HOME/.pdtm/go/bin
```

If `nuclei` exists, check your PATH:

```bash
echo $PATH
```

Then reload the shell configuration:

```bash
source ~/.zshrc
```

Test again:

```bash
nuclei -version
```

---

### Check PDTM Directory

Run:

```bash
ls -lah $HOME/.pdtm/go/bin
```

If the directory exists and contains binaries, but they cannot be called directly, the issue is most likely related to `PATH`.

---

### Find PDTM Binaries

If you are unsure where PDTM stored its binaries:

```bash
find $HOME -type d -path "*/.pdtm/go/bin" 2>/dev/null
```

Or search for a specific tool:

```bash
find $HOME -type f -name "nuclei" 2>/dev/null
```

---

## 10. Root vs Normal User

Be aware that `$HOME` changes depending on which user runs PDTM.

For a regular user:

```text
$HOME=/home/<username>
```

Therefore:

```text
$HOME/.pdtm/go/bin
```

becomes:

```text
/home/<username>/.pdtm/go/bin
```

For the root user:

```text
$HOME=/root
```

Therefore:

```text
$HOME/.pdtm/go/bin
```

becomes:

```text
/root/.pdtm/go/bin
```

This is important because tools installed using PDTM as `root` will not automatically be available to a normal user.

For example, if PDTM is executed as root:

```bash
sudo pdtm
```

the binaries may be installed under:

```text
/root/.pdtm/go/bin
```

Instead of:

```text
/home/<username>/.pdtm/go/bin
```

For a personal Kali Linux environment, it is generally cleaner to run PDTM as your normal user unless root privileges are specifically required.

---

## 11. Recommended Configuration

Add this to your `~/.zshrc`:

```bash
# ProjectDiscovery Tools Manager
export PATH=$PATH:$HOME/.pdtm/go/bin
```

If Go is also manually installed, the configuration can be combined:

```bash
# Golang
export GOROOT=/usr/local/go
export GOPATH=$HOME/go
export PATH=$PATH:$GOROOT/bin:$GOPATH/bin

# ProjectDiscovery Tools Manager
export PATH=$PATH:$HOME/.pdtm/go/bin
```

Then reload:

```bash
source ~/.zshrc
```

Verify everything:

```bash
go version
pdtm -version
which nuclei
which subfinder
which httpx
echo $PATH
```

After this configuration, ProjectDiscovery tools installed through PDTM should be executable directly from any terminal session.
