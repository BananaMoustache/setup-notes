# Setup Go (Golang) — Manual Installation

This guide explains how to configure Go after installing it manually from a `.tar.gz` archive, so the `go` command and binaries installed with `go install` can be executed directly from the Kali Linux terminal.

## 1. Recommended Directory Structure

For a manual Go installation, the recommended structure is:

```text
/usr/local/go/              # Go installation directory
~/go/                       # GOPATH
~/go/bin/                   # Binaries installed with go install
```

Example:

```text
/usr/local/go/bin/go
/home/<username>/go/bin/
```

---

## 2. Check the Go Installation

Make sure the Go binary exists:

```bash
ls -l /usr/local/go/bin/go
```

Test Go directly:

```bash
/usr/local/go/bin/go version
```

Expected output:

```text
go version go1.xx.x linux/amd64
```

If the command above works, but:

```bash
go version
```

returns:

```text
command not found: go
```

then Go has not been added to your `PATH`.

---

## 3. Add Go to PATH

Kali Linux uses `zsh` as the default shell in most installations.

Open your shell configuration:

```bash
nano ~/.zshrc
```

Add the following lines at the bottom:

```bash
export GOROOT=/usr/local/go
export GOPATH=$HOME/go
export PATH=$PATH:$GOROOT/bin:$GOPATH/bin
```

Save the file, then reload the configuration:

```bash
source ~/.zshrc
```

---

## 4. Verify the Configuration

Check the installed Go version:

```bash
go version
```

Check where the `go` binary is being loaded from:

```bash
which go
```

Expected output:

```text
/usr/local/go/bin/go
```

Check `GOROOT`:

```bash
go env GOROOT
```

Expected output:

```text
/usr/local/go
```

Check `GOPATH`:

```bash
go env GOPATH
```

Example:

```text
/home/<username>/go
```

Check your current `PATH`:

```bash
echo $PATH
```

Make sure it contains:

```text
/usr/local/go/bin
```

and:

```text
/home/<username>/go/bin
```

---

## 5. Binaries Installed with `go install`

When you run:

```bash
go install <repository>@latest
```

the compiled binary is usually stored in:

```text
$GOPATH/bin
```

which normally resolves to:

```text
~/go/bin
```

Because this directory has already been added to `PATH`, installed tools can be executed directly from the terminal.

Example:

```bash
go install github.com/projectdiscovery/subfinder/v2/cmd/subfinder@latest
```

Then:

```bash
subfinder -version
```

The same applies to other Go-based tools such as:

```text
subfinder
httpx
nuclei
katana
naabu
```

---

## 6. Manual Go Installation from `.tar.gz`

If Go has not been extracted yet, remove any previous manual installation first:

```bash
sudo rm -rf /usr/local/go
```

Extract the Go archive:

```bash
sudo tar -C /usr/local -xzf go<version>.linux-amd64.tar.gz
```

Example:

```bash
sudo tar -C /usr/local -xzf go1.xx.x.linux-amd64.tar.gz
```

Then reload your shell configuration:

```bash
source ~/.zshrc
```

Verify the installation:

```bash
go version
```

---

## 7. Find the Go Installation Directory

If you do not remember where Go was installed, search for the binary:

```bash
sudo find / -type f -path "*/go/bin/go" 2>/dev/null
```

Example results:

```text
/usr/local/go/bin/go
```

or:

```text
/home/<username>/tools/go/bin/go
```

If Go is installed somewhere other than `/usr/local/go`, update `GOROOT`.

Example:

```bash
export GOROOT=$HOME/tools/go
export GOPATH=$HOME/go
export PATH=$PATH:$GOROOT/bin:$GOPATH/bin
```

Then reload:

```bash
source ~/.zshrc
```

---

## 8. Troubleshooting

### `go: command not found`

Check whether the binary exists:

```bash
ls -l /usr/local/go/bin/go
```

Then check your `PATH`:

```bash
echo $PATH
```

If `/usr/local/go/bin` is missing, add it:

```bash
export PATH=$PATH:/usr/local/go/bin
```

For a permanent configuration, add it to:

```text
~/.zshrc
```

---

### Tools Installed with `go install` Are Not Found

Example:

```text
subfinder: command not found
```

Check whether the binary exists:

```bash
ls ~/go/bin/
```

Check your current `PATH`:

```bash
echo $PATH
```

If `$HOME/go/bin` is missing, run:

```bash
echo 'export PATH=$PATH:$HOME/go/bin' >> ~/.zshrc
source ~/.zshrc
```

Then try again:

```bash
subfinder -version
```

---

## 9. Verify the Entire Go Environment

Run:

```bash
go version
which go
go env GOROOT
go env GOPATH
echo $PATH
```

A normal configuration should look similar to:

```text
GOROOT=/usr/local/go
GOPATH=/home/<username>/go
Go Binary=/usr/local/go/bin/go
Go Tools=/home/<username>/go/bin/
```

---

## Recommended Final Configuration

Your `~/.zshrc` should contain:

```bash
# Golang
export GOROOT=/usr/local/go
export GOPATH=$HOME/go
export PATH=$PATH:$GOROOT/bin:$GOPATH/bin
```

Reload the configuration:

```bash
source ~/.zshrc
```

Verify:

```bash
go version
go env GOROOT
go env GOPATH
```

After this configuration, Go and tools installed using `go install` should be available directly from any terminal session.
