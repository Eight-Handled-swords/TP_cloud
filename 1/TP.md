🌞 **Installer Docker votre machine Azure**

```bash
sudo apt-get update
sudo apt-get install ca-certificates curl
sudo install -m 0755 -d /etc/apt/keyrings
sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc
sudo chmod a+r /etc/apt/keyrings/docker.asc
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/ubuntu \
  $(. /etc/os-release && echo "${UBUNTU_CODENAME:-$VERSION_CODENAME}") stable" | \
sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
sudo apt-get update
sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
sudo docker run hello-world
sudo usermod -aG docker $(whoami)
exit
ssh azureuser@20.117.121.6
```

🌞 **Utiliser la commande `docker run`**

```bash
$ docker run --name web -d -p 9999:80 nginx
```

🌞 **Rendre le service dispo sur internet**

```bash
PS C:\Users\Johan> curl 20.117.121.6:9999


StatusCode        : 200
StatusDescription : OK
Content           : <!DOCTYPE html>
                    <html>
                    <head>
                    <title>Welcome to nginx!</title>
                    <style>
                    html { color-scheme: light dark; }
                    body { width: 35em; margin: 0 auto;
                    font-family: Tahoma, Verdana, Arial, sans-serif; }
                    </style...
RawContent        : HTTP/1.1 200 OK
                    Connection: keep-alive
                    Accept-Ranges: bytes
                    Content-Length: 615
                    Content-Type: text/html
                    Date: Fri, 14 Mar 2025 10:28:06 GMT
                    ETag: "67a34638-267"
                    Last-Modified: Wed, 05 Feb 2025 ...
Forms             : {}
Headers           : {[Connection, keep-alive], [Accept-Ranges, bytes], [Content-Length, 615], [Content-Type,
                    text/html]...}
Images            : {}
InputFields       : {}
Links             : {@{innerHTML=nginx.org; innerText=nginx.org; outerHTML=<A href="http://nginx.org/">nginx.org</A>;
                    outerText=nginx.org; tagName=A; href=http://nginx.org/}, @{innerHTML=nginx.com;
                    innerText=nginx.com; outerHTML=<A href="http://nginx.com/">nginx.com</A>; outerText=nginx.com;
                    tagName=A; href=http://nginx.com/}}
ParsedHtml        : mshtml.HTMLDocumentClass
RawContentLength  : 615



PS C:\Users\Johan>
```

🌞 **Custom un peu le lancement du conteneur**

super.conf :
```bash
server {
  # on définit le port où NGINX écoute dans le conteneur
  listen 7777;

  # on définit le chemin vers la racine web
  # dans ce dossier doit se trouver un fichier index.html
  root /usr/share/nginx/html;
}
```

```bash
$ docker run --name meow -d -v /etc/nginx/super/super.conf:/etc/nginx/conf.d/super.conf -v /etc/nginx/super/index.html:/usr/share/nginx/html/index.html -p 9999:7777 -m 512m nginx

$ docker ps
CONTAINER ID   IMAGE   COMMAND                CREATED         STATUS    
fde55396fff4   nginx   "/docker-entrypoint.…" 4 seconds ago   Up 4 seconds

PORTS                                                 NAMES
80/tcp, 0.0.0.0:9999->7777/tcp, [::]:9999->7777/tcp   meow
```

```bash
PS C:\Users\Johan> curl 20.117.121.6:9999


StatusCode        : 200
StatusDescription : OK
Content           : Welcome to my docker nginx thingy meo

                    h

RawContent        : HTTP/1.1 200 OK
                    Connection: keep-alive
                    Accept-Ranges: bytes
                    Content-Length: 41
                    Content-Type: text/html
                    Date: Fri, 14 Mar 2025 11:42:23 GMT
                    ETag: "67d4118d-29"
                    Last-Modified: Fri, 14 Mar 2025 11...
Forms             : {}
Headers           : {[Connection, keep-alive], [Accept-Ranges, bytes], [Content-Length, 41], [Content-Type,
                    text/html]...}
Images            : {}
InputFields       : {}
Links             : {}
ParsedHtml        : mshtml.HTMLDocumentClass
RawContentLength  : 41



PS C:\Users\Johan>
```

c'est quoi un meow même

🌞 **Call me**

**PART 2-----------------------**

🌞 **Construire votre propre image**
🌞 **Dans les deux cas, j'attends juste votre `Dockerfile` dans le compte-rendu**

le voici :

Dockerfile :
```bash
FROM ubuntu

RUN apt update -y && apt install -y apache2

RUN mkdir -p /etc/apache2/logs

COPY apache2.conf /etc/apache2/apache2.conf
COPY index.html /var/www/html/index.html

CMD ["apache2ctl", "-D", "FOREGROUND"]
```

```bash
docker build . -t apash
docker run -d -p 8888:80 apash
```

**PART 3-----------------------**

🌞 **Installez un WikiJS** en utilisant Docker
Un wiki javascript ? :P

```bash
docker compose up -d
```

🌞 **Meow ?** 

```bash
docker compose up -d

PS C:\Users\Johan> curl 20.117.121.6                                                                                    

StatusCode        : 200
StatusDescription : OK
Content           : <h1>Add key</h1>
                    <form action="/add" method = "POST">

                    Key:
                    <input type="text" name="key" >

                    Value:
                    <input type="text" name="value" >

                    <input type="submit" value="Submit">
                    </form>

                    <h1>Check key</h1>
                    ...
RawContent        : HTTP/1.1 200 OK
                    Connection: close
                    Content-Length: 342
                    Content-Type: text/html; charset=utf-8
                    Date: Tue, 18 Mar 2025 08:11:23 GMT
                    Server: Werkzeug/3.1.3 Python/3.13.2

                    <h1>Add key</h1>
                    <form act...
Forms             : {, }
Headers           : {[Connection, close], [Content-Length, 342], [Content-Type, text/html; charset=utf-8], [Date, Tue,
                    18 Mar 2025 08:11:23 GMT]...}
Images            : {}
InputFields       : {@{innerHTML=; innerText=; outerHTML=<INPUT name=key>; outerText=; tagName=INPUT; name=key},
                    @{innerHTML=; innerText=; outerHTML=<INPUT name=value>; outerText=; tagName=INPUT; name=value},
                    @{innerHTML=; innerText=; outerHTML=<INPUT type=submit value=Submit>; outerText=; tagName=INPUT;
                    type=submit; value=Submit}, @{innerHTML=; innerText=; outerHTML=<INPUT name=key>; outerText=;
                    tagName=INPUT; name=key}...}
Links             : {}
ParsedHtml        : mshtml.HTMLDocumentClass
RawContentLength  : 342



PS C:\Users\Johan>
```


**PART 4-----------------------**


🌞 **Prouvez que vous pouvez devenir `root`**

```bash
docker run -v /:/mnt --rm -it alpine chroot /mnt sh
# whoami
root
# cat etc/shadow
root:*:20140:0:99999:7:::
daemon:*:20140:0:99999:7:::
bin:*:20140:0:99999:7:::
sys:*:20140:0:99999:7:::
sync:*:20140:0:99999:7:::
games:*:20140:0:99999:7:::
man:*:20140:0:99999:7:::
lp:*:20140:0:99999:7:::
mail:*:20140:0:99999:7:::
news:*:20140:0:99999:7:::
uucp:*:20140:0:99999:7:::
proxy:*:20140:0:99999:7:::
```
je vais pas mettre tout le retour de la commande

🌞 **Utilisez Trivy**

nginx

erreur pour wikijs et apache :(

🌞 **Utilisez l'outil Docker Bench for Security**

Section C - Score

[INFO] Checks: 117
[INFO] Score: 5

note : rapide et simple a lancer
