# NGINX를 이용해서 리버스 프록시 만들고 HTTPS 통신하기

```bash
sudo apt update

# nginx 설치
sudo apt install nginx

# certbot 설치
sudo apt install certbot

# nginx의 프록시 확인을 위한 플러그인
sudo apt install certbot python3-certbot-nginx
```

- **기본 reverse proxy**
```conf
server {
    server_name media.osxteams.net;
    listen 80;
    location / {
        proxy_pass http://192.168.1.5:8096;
    }
```


- **certbot을 이용한 https 통신**
```conf
server {
    client_max_body_size 0;
    server_name media.osxteams.net;
    location / {
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header Host $host;
        proxy_set_header X-Forwarded-Proto $scheme;
        proxy_pass http://192.168.1.5:8096;

        proxy_set_header X-Forwarded-Protocol $scheme;
        proxy_set_header X-Forwarded-Host $http_host;

        # 웹 소켓 통신
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection "upgrade";
    }

    listen 443 ssl; # managed by Certbot
    ssl_certificate /etc/letsencrypt/live/media.osxteams.net/fullchain.pem; # managed by Certbot
    ssl_certificate_key /etc/letsencrypt/live/media.osxteams.net/privkey.pem; # managed by Certbot
    include /etc/letsencrypt/options-ssl-nginx.conf; # managed by Certbot
    ssl_dhparam /etc/letsencrypt/ssl-dhparams.pem; # managed by Certbot

}server {
    if ($host = media.osxteams.net) {
        return 301 https://$host$request_uri;
    } # managed by Certbot
  
    server_name media.osxteams.net;
    listen 80;
    return 404; # managed by Certbot
}
```

참고 : Let's Encrypt 인증서는 90일 동안만 유효합니다. 하지만 `apt`로 설치하면 시스템에 **자동 갱신 타이머**가 등록되므로 신경 쓸 일이 거의 없습니다.

갱신 프로세스가 정상적으로 작동하는지 테스트해보려면 다음 명령어를 사용하세요:

```bash
sudo certbot renew --dry-run
```
