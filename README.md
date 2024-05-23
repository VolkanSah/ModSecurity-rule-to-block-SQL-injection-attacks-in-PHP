
# ModSecurity Rule to Block SQL Injection Attacks in PHP
###### Updated 2024

ModSecurity is an open-source web application firewall (WAF) that can protect your PHP applications from a variety of attacks, including SQL injection, cross-site scripting (XSS), and other common web exploits.

## Installation and Configuration of ModSecurity
ModSecurity is available as a module for popular web servers like Apache and Nginx. 

### Steps to Install ModSecurity
1. **For Apache:**
   ```bash
   sudo apt-get install libapache2-mod-security2
   ```
   After installation, enable ModSecurity:
   ```bash
   sudo a2enmod security2
   ```

2. **For Nginx:**
   First, install the necessary software:
   ```bash
   sudo apt-get install nginx modsecurity
   ```
   Then, activate the ModSecurity module in Nginx by adding the following to your server configuration:
   ```nginx
   modsecurity on;
   modsecurity_rules_file /etc/nginx/modsec/modsecurity.conf;
   ```

## Configuring ModSecurity
Configure your web server to load the ModSecurity rules and enable it for the relevant virtual host or directory.

### Basic Configuration Example
For Apache, add the following to your site's configuration:
```apache
<IfModule mod_security2.c>
    Include /etc/modsecurity/*.conf
    Include /etc/modsecurity/rules/*.conf
</IfModule>
```
For Nginx, you might add:
```nginx
server {
    location / {
        modsecurity on;
        modsecurity_rules_file /etc/nginx/modsecurity.d/*.conf;
    }
}
```

## Writing ModSecurity Rules
You will need to write ModSecurity rules to protect your PHP files from common web attacks. Here’s an example ModSecurity rule to block SQL injection attacks in PHP:

```php
SecRule REQUEST_FILENAME "\.php$" \
    "id:1,\
     phase:2,\
     block,\
     t:none,\
     msg:'SQL injection attempt',\
     chain"

SecRule ARGS "(select|union|from|where|having|order by|group by)" \
    "t:none,\
     t:urlDecodeUni,\
     t:htmlEntityDecode,\
     t:compressWhiteSpace,\
     deny,\
     status:403,\
     log,\
     msg:'SQL injection attempt'"
```

## Testing Your ModSecurity Rules
It's crucial to test your rules in a controlled environment to ensure they do not block legitimate requests or cause unexpected behavior.

### Tools for Testing
- ModSecurity’s built-in logging and debugging features.
- Online web application scanners.
- Third-party testing tools.

Remember, while ModSecurity can significantly enhance your application's security, it is not foolproof. Always use secure coding practices and input validation alongside ModSecurity to protect your PHP applications fully.

## Credits
[Volkan Sah](https://github.com/volkansah)
