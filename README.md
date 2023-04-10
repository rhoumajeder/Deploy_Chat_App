# Doc

........................................................................................
server {
    listen 80;
    server_name 3.8.187.241 jedder.net www.jedder.net;

    location / {
        proxy_pass http://localhost:31321;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
}

    location /test {
        proxy_pass http://localhost:5000;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
}
}
........................................................................................

........................................................................................
    location /dash {
        proxy_pass http://localhost:8001/api/v1/namespaces/kubernetes-dashboard/services/https:kubernetes-dashboard:/proxy/;
        # Fix for the subpath issue
        proxy_set_header X-Forwarded-Uri /dash;
        rewrite ^/dash/(.*)$ /$1 break;
        #/dash/api/v1/namespaces/kubernetes-dashboard/services/https:kubernetes-dashboard:/proxy/#/login

    }
........................................................................................
