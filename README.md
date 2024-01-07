# poc-content-security-policy

**Explore content-security-policy header**

### demo

    docker compose up -d

### index.html


```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <link href="https://fonts.googleapis.com/css2?family=Roboto:wght@400;700&display=swap" rel="stylesheet">
    <link rel="icon" type="image/png" sizes="32x32" href="favicon.png">

    <title>My Web Page</title>
    <style>
        body {
            font-family: 'Roboto', sans-serif;
        }
    </style>
</head>
<body>
    <h1>Welcome to My Web Page</h1>
    <p>This is a paragraph of text styled with a font from Google Fonts.</p>
    <script>
        window.onload = function() {
            alert('Welcome to my web page!');
        };
    </script>
</body>
</html>
```

### default.conf

```html
server {
    listen 80;

    location / {
        root /usr/share/nginx/html;
        index index.html index.htm;

        # Add headers
        add_header X-Content-Type-Options "nosniff" always;
        add_header X-XSS-Protection "1; mode=block" always;
        add_header X-Frame-Options "DENY" always;
        add_header X-Frame-Options "DENY" always;
        add_header Content-Security-Policy "default-src 'self'; style-src 'self' https://fonts.googleapis.com 'unsafe-inline'; font-src 'self' https://fonts.gstatic.com ;script-src 'unsafe-inline';" always;

    }
}
```

### docker-compose.yaml

```yaml
version: '3'
services:
  web:
    image: nginx:latest
    ports:
      - "8080:80"
    volumes:
      - ./html:/usr/share/nginx/html
      - ./default.conf:/etc/nginx/conf.d/default.conf
```