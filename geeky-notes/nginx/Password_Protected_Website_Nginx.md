### Passowrd Protection in Website running on Nginx Hosted on Linux Remote Server or VPS

- Install Util
```sh
sudo apt install apache2-utils
```
- Create Login Credentials
```sh
sudo htpasswd -c /etc/nginx/.htpasswd user_name
```
- Verify that Credential has been Created
```sh
cat /etc/nginx/.htpasswd
```
- Go to /etc/nginx/sites-available
```sh
cd /etc/nginx/sites-available
```
- Open the Required Virtual Host File
```sh
sudo nano website_host_file
```
- Add below Content
```sh
location /admin {
  autoindex on;
  try_files $uri $uri/ =404;
  auth_basic "admin area";
  auth_basic_user_file /etc/nginx/.htpasswd;
}
```
- Or You can Add below Content For Specific Domain allowed and redirect to specific page
```sh
location /storage {
    autoindex on;
    try_files $uri $uri/ =404;

    set $realm "admin area";
            set $realmpass "/etc/nginx/.htpasswd";

    location ~* \.(jpg|jpeg|gif|png|webp) {
        if ($http_referer ~* (https?://(www\.)?form.nexxtapp\.com)) {
    set $realm "off";
        }

        auth_basic $realm;
        auth_basic_user_file $realmpass;
 error_page 401 = /403.html;
    }
}
```
- Check Syntax structure is valid
```sh
sudo nginx -t OR sudo nginx -tV
```
- Restart Nginx
```sh
sudo service nginx restart
```
