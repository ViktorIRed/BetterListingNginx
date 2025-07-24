# betterlistingnginx

🗂️ **betterlistingnginx** — лёгкая замена стандартному `autoindex` в NGINX с возможностью стилизации через HTML-шаблоны.  
Работает полностью на стороне NGINX, без PHP, CGI или внешнего рендеринга.

---

## 📁 Пример конфигурации

Рабочая конфигурация с шаблонами, размещёнными в `/var/html/nginx/betterlisting/`:

```nginx
server {
    listen 80;
    server_name your-domain.com;

    root /var/html/nginx;

    # Вставка HTML-шаблонов до и после autoindex-вывода
    add_before_body /betterlisting/top.html;
    add_after_body /betterlisting/bot.html;

    # Очистка стандартной разметки autoindex
    sub_filter '<html>' '';
    sub_filter '<head><title>Index of $uri</title></head>' '';
    sub_filter '<h1>Index of $uri</h1>' '';
    sub_filter '<body bgcolor="white">' '';
    sub_filter '</body>' '';
    sub_filter '</html>' '';
    sub_filter_once on;

    location /files/ {
        autoindex on;
        try_files $uri $uri/ =404;
    }
}
