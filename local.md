## ロカール環境に動作確認
1. httpd_my.conf にCI4の設定追加
```bash
Alias /pl_api_ci4 /var/www/html/pl_api_ci4/public
<Directory "/var/www/html/pl_api_ci4/public">
    DirectoryIndex index.php
    Options Indexes FollowSymLinks
    AllowOverride All
    Require all granted
</Directory>
```
#変更内容を反映
```bash 
docker cp ./amazonlinux/httpd_my.conf link_web:/etc/httpd/conf.d/httpd_my.conf
```
#設定を反映
```bash
docker exec -it link_web systemctl restart httpd
```

2. .htaccess修正（cors対策）
```bash
<IfModule mod_rewrite.c>
    RewriteEngine On
    RewriteBase /pl_api_ci4/

    # OPTIONS は処理しない
    RewriteCond %{REQUEST_METHOD} OPTIONS
    RewriteRule ^(.*)$ - [L]

    # 存在しないファイルは index.php へ
    RewriteCond %{REQUEST_FILENAME} !-f
    RewriteCond %{REQUEST_FILENAME} !-d
    RewriteRule ^(.*)$ index.php/$1 [L]
</IfModule>

<IfModule mod_headers.c>
    Header always set Access-Control-Allow-Origin "*"
    Header always set Access-Control-Allow-Methods "GET,POST,OPTIONS,PUT,DELETE"
    Header always set Access-Control-Allow-Headers "Content-Type,Authorization"
</IfModule>
```
#設定を反映
```bash
systemctl restart httpd
```