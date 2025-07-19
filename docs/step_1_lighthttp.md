# Lighttpd Web Server Setup Guide for CGI Development

This guide sets up Lighttpd on AlmaLinux 9 for CGI development with a single Rust application.

## Prerequisites

- Complete WSL 2 AlmaLinux 9 development environment (previous guide)
- Administrator access in your WSL environment

## Step 1: Install Lighttpd

```bash
sudo dnf install lighttpd -y
```

## Step 2: Create Directory Structure

```bash
# Create main development directory
sudo mkdir -p /var/www/dev/{htdocs,cgi-bin,data,logs,config}

# Set proper ownership
sudo chown -R $USER:$USER /var/www/dev

# Set proper permissions
chmod 755 /var/www/dev/{htdocs,cgi-bin,logs,config}
chmod 700 /var/www/dev/data
```

## Step 3: Directory Structure

```
/var/www/dev/
├── htdocs/                     # Public web files
├── cgi-bin/                    # CGI executable
│   └── app.cgi                # Main application binary
├── data/                       # Private data (NOT web accessible)
├── logs/                       # Web server logs
└── config/                     # Configuration files
    └── lighttpd.conf
```

## Step 4: Configure Lighttpd

```bash
# Lighttpd Configuration for Development
server.document-root = "/var/www/dev/htdocs"
server.port = 8080
server.bind = "127.0.0.1"

# User and group
server.username = "bansible"
server.groupname = "bansible"

# Modules
server.modules = (
    "mod_access",
    "mod_alias",
    "mod_cgi",
    "mod_accesslog"
)

# Logging
accesslog.filename = "/var/www/dev/logs/access.log"
server.errorlog = "/var/www/dev/logs/error.log"

# MIME types
mimetype.assign = (
    ".html" => "text/html",
    ".htm" => "text/html",
    ".txt" => "text/plain",
    ".css" => "text/css",
    ".js" => "application/javascript",
    ".json" => "application/json"
)

# CGI Configuration
cgi.assign = ( ".cgi" => "" )
alias.url += ( "/cgi-bin/" => "/var/www/dev/cgi-bin/" )
$HTTP["url"] =~ "^/cgi-bin/" {
    cgi.assign = ( "" => "" )
}

# Security: Deny access to data directory
$HTTP["url"] =~ "^/data/" {
    url.access-deny = ( "" )
}

# Index files
index-file.names = ( "index.html", "index.htm" )
```

## Step 5: Start Lighttpd

**Test run (foreground):**
```bash
lighttpd -f /var/www/dev/config/lighttpd.conf -D
```

**Background service:**
```bash
sudo tee /etc/systemd/system/lighttpd-dev.service > /dev/null << EOF
[Unit]
Description=Lighttpd Development Server
After=network.target

[Service]
Type=forking
User=$USER
Group=$USER
ExecStart=/usr/sbin/lighttpd -f /var/www/dev/config/lighttpd.conf
ExecReload=/bin/kill -HUP \$MAINPID

[Install]
WantedBy=multi-user.target
EOF

sudo systemctl daemon-reload
sudo systemctl enable lighttpd-dev
sudo systemctl start lighttpd-dev
```

## Step 6: Deploy Your Rust Application

```bash
# Build your Rust project
cd /path/to/your/rust/project
cargo build --release

# Copy to cgi-bin
cp target/release/your-app /var/www/dev/cgi-bin/app.cgi
chmod +x /var/www/dev/cgi-bin/app.cgi
```

## Testing

```bash
# Test CGI
curl http://localhost:8080/cgi-bin/app.cgi

# Check logs
tail -f /var/www/dev/logs/error.log
tail -f /var/www/dev/logs/access.log
```

---

**Web Server Setup Complete!** Your Lighttpd server is configured for CGI development.