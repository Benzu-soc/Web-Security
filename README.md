# Web-Security
# Ben's Web Security Hardening Cheat Sheet

Professional Apache, WordPress, and cPanel security hardening reference guide.

---

# 1. Force HTTPS Redirect

```apache
RewriteEngine On
RewriteCond %{HTTPS} !=on
RewriteRule ^ https://%{HTTP_HOST}%{REQUEST_URI} [L,R=301]
```

### Purpose
- Redirects all traffic to HTTPS
- Prevents insecure HTTP communication

---

# 2. Disable Directory Listing

```apache
Options -Indexes
```

### Purpose
- Prevents attackers from browsing server directories

---

# 3. Protect Sensitive Files

```apache
<FilesMatch "^(\.htaccess|\.env|config\.php|composer\.json|composer\.lock|package\.json|package-lock\.json|\.git)">
Order allow,deny
Deny from all
</FilesMatch>
```

### Purpose
- Blocks access to:
  - `.env`
  - `.git`
  - `composer.json`
  - config files

---

# 4. Hide Server Information

```apache
ServerSignature Off
FileETag None
```

### Purpose
- Reduces server fingerprinting

---

# 5. Block Hidden Files

```apache
RedirectMatch 403 /\..*$
```

### Purpose
- Blocks access to hidden system files

---

# 6. Disable PHP Execution in Uploads

```apache
<FilesMatch "\.(php|phar)$">
Require all denied
</FilesMatch>
```

### Purpose
- Prevents malicious PHP uploads from executing

---

# 7. Disable XML-RPC

```apache
<Files xmlrpc.php>
Require all denied
</Files>
```

### Purpose
- Prevents:
  - brute force attacks
  - XML-RPC abuse
  - pingback abuse

---

# 8. Protect wp-config.php

```apache
<Files wp-config.php>
Require all denied
</Files>
```

### Purpose
- Protects WordPress database credentials

---

# 9. Block WordPress Fingerprinting Files

```apache
<FilesMatch "^(readme.html|license.txt)$">
Require all denied
</FilesMatch>
```

### Purpose
- Prevents WordPress version disclosure

---

# 10. WordPress Login Rule

```apache
<Files wp-login.php>
Order Allow,Deny
Allow from all
</Files>
```

### Purpose
- Controls access handling for WordPress login

---

# 11. Content Security Policy (CSP)

```apache
Header set Content-Security-Policy "default-src * data: blob: 'unsafe-inline' 'unsafe-eval'; img-src * data: blob:; script-src * 'unsafe-inline' 'unsafe-eval' https://maps.googleapis.com https://maps.gstatic.com; style-src * 'unsafe-inline'; font-src * data:; frame-src *; connect-src *;"
```

### Purpose
- Mitigates:
  - XSS attacks
  - malicious scripts
  - unauthorized resources

---

# 12. X-Frame-Options

```apache
Header always set X-Frame-Options "SAMEORIGIN"
```

### Purpose
- Prevents clickjacking attacks

---

# 13. MIME Sniffing Protection

```apache
Header set X-Content-Type-Options "nosniff"
```

### Purpose
- Prevents MIME-type sniffing vulnerabilities

---

# 14. HSTS (Strict HTTPS)

```apache
Header always set Strict-Transport-Security "max-age=31536000; includeSubDomains; preload"
```

### Purpose
- Forces browsers to use HTTPS only

---

# 15. Cross-Origin Policies

```apache
Header set Cross-Origin-Opener-Policy "same-origin"
Header set Cross-Origin-Resource-Policy "same-origin"
Header set Cross-Origin-Embedder-Policy "require-corp"
```

### Purpose
- Improves browser isolation security

---

# 16. Permissions Policy

```apache
Header set Permissions-Policy "geolocation=(), microphone=(), camera=()"
```

### Purpose
- Disables unnecessary browser permissions

---

# 17. Disable WordPress File Editing

## Add inside `wp-config.php`

```php
define('DISALLOW_FILE_EDIT', true);
```

### Purpose
- Prevents plugin/theme editing from dashboard

---

# 18. Disable Debug Display

## Add inside `wp-config.php`

```php
define('WP_DEBUG', false);
define('WP_DEBUG_DISPLAY', false);
@ini_set('display_errors', 0);
```

### Purpose
- Prevents exposure of:
  - PHP errors
  - file paths
  - stack traces

---

# 19. Uploads Folder Protection

## `.htaccess` inside `/wp-content/uploads`

```apache
<FilesMatch "\.php$">
Require all denied
</FilesMatch>
```

### Purpose
- Prevents PHP shell execution inside uploads folder

---

# 20. Backup & Recovery

## JetBackup Verification
- Daily backups
- Weekly backups
- Restore points

### Purpose
- Disaster recovery
- Ransomware recovery
- Rollback capability

---

# 21. MFA / 2FA

## cPanel Path

```text
cPanel → Security → Two-Factor Authentication
```

### Purpose
- Adds additional authentication protection

---

# 22. Security Testing Commands

## Check Security Headers

```bash
curl -I https://yourdomain.com
```

---

## WordPress Vulnerability Scan

```bash
wpscan --url https://yourdomain.com
```

---

## Port Scanning

```bash
nmap yourdomain.com
```

---

## SSL Verification

```bash
openssl s_client -connect yourdomain.com:443
```

---

# Author

**Ben Enzuga**  
Cybersecurity | WordPress Security Hardening | Web Security
