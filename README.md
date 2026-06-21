<div align="center">

<img src="github/assets/logo.png" width="400" alt="pyvanity">

<h2>Virtual Private/Peer-to-Peer Networking</h2>

<p>
<i>• Open Source •</i>
</p>

</div>

## Installation

### 1. Dependencies

```bash
git clone https://github.com/alexx307/vp2n
cd vp2n

# Wireguard
sudo apt update -y && sudo apt upgrade -y
sudo apt install -y wireguard-tools iptables curl git build-essential pkg-config libssl-dev

# Rust 
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh -s -- -y
. "$HOME/.cargo/env"

# Go
curl -sSL https://go.dev/dl/go1.25.0.linux-amd64.tar.gz | tar -C /usr/local -xz
export PATH=$PATH:/usr/local/go/bin
echo "export PATH=$PATH:/usr/local/go/bin" >> ~/.bashrc
```

### 2. Compiler

```bash
cd server && go build -o vp2n-server
cd client && cargo build --release
cp target/release/vp2n /usr/local/bin/vp2n

# Test
vp2n --version
```

### 3. Generate random token

```bash
openssl rand -hex 16

# output example YOUR_TOKEN
# c4ca4238a0b923820dcc509a6f75849b
```

### 4. Firewall

```bash
systl -w net.ipv4.ip.forward=1
echo 'net.ipv4.ip_forward=1' >> /etc/sysctl.conf
```

### 5. Allow Wireguard and VP2N ports

```bash
ufw allow 7743/tcp 2>/dev/null; ufw allow 51820/udp 2>/dev/null
```

### 6. Run server

```bash
cd vp2n/server
./vp2n-server -addr :7743 -tls-auto /vp2n/certs -token YOUR_TOKEN
```

### 7. Test client

```bash
curl -sk https://127.0.0.1:7743/api/v1/health
```

### 8. Run and Help

```bash
# Start VP2N Public
sudo vp2n start

# Start VP2N Private
sudo vp2n start --token YOUR_TOKEN

# Help Command
sudo vp2n help
```