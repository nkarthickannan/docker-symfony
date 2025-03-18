# PHPStorm Xdebug Configuration with Docker

This guide walks through configuring PHPStorm for debugging PHP applications running in a Docker container with Xdebug 3.

## Configure PHP Interpreter
1. **Open Settings**: `File > Settings > PHP`
2. **Add CLI Interpreter**:
    - Click `...` next to CLI Interpreter
    - Choose `From Docker, Vagrant... > Docker`
    - Configuration:
        - Server: Docker (default)
        - Image name: `your_project-phpfpm`
        - PHP executable: `/usr/local/bin/php`

## Server Configuration
1. **Navigate**: `Settings > PHP > Servers`
2. **Add New Server**:
    - Name: `Docker App`
    - Host: `${DOMAIN}` (from your .env)
    - Port: `80`
    - Debugger: `Xdebug`
3. **Path Mapping**:
    - Local path: `./app`
    - Remote path: `/var/www/html`

## Xdebug Settings
1. **Debug Settings**: `Settings > PHP > Debug`
    - External connections section:
        - Break at first line in PHP scripts: `Unchecked`
    - Xdebug section:
        - Debug port: `9003`
2. **DBGp Proxy**: `Settings > PHP > Debug > DBGp Proxy`
     - IDE key: `PHPSTORM` (from your .env)
     - Host: `${DOMAIN}`
     - Port: `80`

## Debug Configuration
1. **Create Run/Debug Config**: `Run > Edit Configurations`
2. **Add PHP Remote Debug**:
    - Server: `Docker App`
    - IDE key: `PHPSTORM`
3. **Advanced**:
    - Filter debug connection by IDE key: `Checked`

## Testing the Setup
1. Create `test.php` in your docroot directory:
```php
<?php
echo "Hello";
phpinfo();
```
2. Set breakpoint at line 2
3. Click on "Start listening for PHP Debug connections" icon (a bug icon on the top right) or `Run > Start listening for PHP Debug connections`
4. Access `https://${DOMAIN}/test.php` in browser
5. The execution should break at the 2nd line

# Troubleshooting
## Common Issues
1. **No connection**:
    - Verify Xdebug is active:
      ```bash
      docker-compose exec phpfpm php -v | grep Xdebug
      ```
    - Check port listening:
      ```bash
      nc -vz localhost 9003
      ```

2. **Path mapping errors**:
    - Confirm server path mappings match container structure
    - Ensure files are synchronized between host/container

3. **Session not starting**:
    - Check request cookies contain `XDEBUG_SESSION=PHPSTORM`

## Debug Logging
Add to `xdebug.ini`:
```ini
xdebug.log=/tmp/xdebug.log
xdebug.log_level=3
```

Access logs via:
```bash
docker-compose exec phpfpm tail -f /tmp/xdebug.log
```

---
