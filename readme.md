![DevJungle logo](DevJungle.webp)
# DevJungle : linux based server setup  ___(ubuntu)___

### Welcome to DevJungle were by we code~evolve~thrive, this project will help you how to set up linux based web ser using nginx !

> #### ___please feel free to fork the project and update it, community thanks for it___


___

### Step 1 : install nginx

#### note: if you have already intalled nginx please skip to step **2**


### ___*Method 1 ( recommended )*___

+ installing brew and using it to download nginx

#### brew installation : 

```bash
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"

```

#### using brew to install nginx

```bash
brew install nginx
```

### Method 2 ( not recommended ) :

+ downloadind manually and exctract then install

#### step 1 : download nginx from officail website

> [click here to download](http://nginx.org/en/download.html)

#### step 2 : follow step by step instruction from official documentation

it quite long 😊 , better use method 1 , just two line of code or even one if you have brew already installed !

_______________________________________________________________


## Tip : understand nginx directory 

The way nginx and its modules work is determined in the configuration file. By default, the configuration file is named nginx.conf and placed in the directory /**usr/local/nginx/conf**, **/etc/nginx**, or **/usr/local/etc/nginx**.

______________________________________________________________

## Starting, Stopping, and Reloading Configuration

after installing nginx, you will need to start or rather initialize it 

```bash
sudo nginx
```

> To start nginx, run the executable file. Once nginx is started, it can be controlled by invoking the executable with the -s parameter. Use the following syntax:  
> ``` sudo nginx -s signal```

Where signal may be one of the following:
+ stop — fast shutdown
+ quit — graceful shutdown
+ reload — reloading the configuration file
+ reopen — reopening the log files

**example :**
```bash
sudo nginx -s reload
```

## Viewing default page of nginx

you can go see and default page on a browser

if you are using a vps with no doamin yet you can access by inserting your ip address gine by your provider 

**exaample :** http://192.168.1.2/

you will nginx welcome page 

**note :** use http protocol and not https , by default nginx uses http and is not yet configured to use https


## Serving static contetnt

### step 1 : navigate to nginx www folder

by defalut ngnix keeps its www folder in :
```bash
cd /var/www
```

by default you will see a folder called html that nginx created, you can updated in there ,however i will create a new folder for better understanding 😊 

> create new folder inside of ***www folder*** : ``` sudo mkdir myWebsite ```

> navigate to ***myWebsite*** folder : ``` cd myWebsite ```

> inside the folder create ***index.html*** and write as your desire ❤️ 

> since im a nice guy, you can copy my code below and paste into your index.html folder for demonstration purpose 😊  :

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Welcome to Kali Server</title>
    <style>
        body {
            display: flex;
            justify-content: center;
            align-items: center;
            height: 100vh;
            margin: 0;
            font-family: Arial, sans-serif;
            background-color: #282c34;
            color: #61dafb;
        }
        h1 {
            font-size: 3rem;
        }
    </style>
</head>
<body>
    <h1>Welcome to my website</h1>
</body>
</html>
```


### step 2 : Point your new awesome website by configuring nginx.conf file

> navigate to [nginx folder](#tip--understand-nginx-directory)

> ***open nginx.config file*** ,it is importatnt for you to open this file with priveleges, otherwise you wont be able to modify it : ``` sudo nano nginx.conf ```

> delete all content inside and copy code below, ill explain it : 

```nano

user www-data;
worker_processes auto;
worker_cpu_affinity auto;
pid /run/nginx.pid;
error_log /var/log/nginx/error.log;
include /etc/nginx/modules-enabled/*.conf;



http {

        server {
                listen 80;
                root /var/www/myWebsite;
                index index.html;



        location / {
                try_files $uri $uri/ =404;
        }


        }



        include /etc/nginx/conf.d/*.conf;
        include /etc/nginx/sites-enabled/*;

}

events {

}


```

1. firstly we need to add ***http*** context and ***events*** context, you might notice that its empty, we ***need it neverthelesss for nginx to work properly*** 

> inside http we need to define another context called ***server*** and inside a server this is going to be where will define a bunch of directive so that we can configure nginx

```markdown nano

👉 http{
    
  👉  server {

    }

}

👉 events{

}

```


2. On sever context add **directive  'listen'** , is basically saying to nginx (your server) to listen http request on specified port 

```markdown nano

http{
    
    server {

      👉 listen 8080;

    }

}

events{

}

```

3. Again on server context add **directive 'root'*, this allows you to set a file path to point to a bucnh containing files that you want to be serve.
+ 💡 remeber we define our custume website file in [/var/www/myWebsite](#step-1--navigate-to-nginx-www-folder) directory, please check in server and locate the folder, whereby inside has index.html (example) ___do whatever you want___

```markdown nano

http{
    
    server {

        listen 8080;
     👉 root /var/www/myWebsite

    }

}

events{

}

```

> ⚠️ after updating nginx.conf file you might notice no change, well this because we need to reload nginx with this new configuration  ``` sudo nginx -s reload ``` . If the no change has happened restart nginx ``` sudo systemctl restart nginx ``` .

## Error that you might find when updating nginx files 

### If you are facing this type of error : 

> to see erros run ```journalctl -xeu nginx.service``` 

```nano
░░ The job identifier is 5264 and the job result is failed.
Mar 19 10:01:44 kali systemd[1]: Starting nginx.service - A high performance web server and a reverse proxy server...
░░ Subject: A start job for unit nginx.service has begun execution
░░ Defined-By: systemd
░░ Support: https://www.debian.org/support
░░ 
░░ A start job for unit nginx.service has begun execution.
░░ 
░░ The job identifier is 5366.
Mar 19 10:01:44 kali nginx[8171]: nginx: [emerg] bind() to 0.0.0.0:8080 failed (98: Address already in use)
Mar 19 10:01:45 kali nginx[8171]: nginx: [emerg] bind() to 0.0.0.0:8080 failed (98: Address already in use)
Mar 19 10:01:45 kali nginx[8171]: nginx: [emerg] bind() to 0.0.0.0:8080 failed (98: Address already in use)
Mar 19 10:01:46 kali nginx[8171]: nginx: [emerg] bind() to 0.0.0.0:8080 failed (98: Address already in use)
Mar 19 10:01:46 kali nginx[8171]: nginx: [emerg] bind() to 0.0.0.0:8080 failed (98: Address already in use)
Mar 19 10:01:47 kali nginx[8171]: nginx: [emerg] still could not bind()
Mar 19 10:01:47 kali systemd[1]: nginx.service: Control process exited, code=exited, status=1/FAILURE
░░ Subject: Unit process exited
░░ Defined-By: systemd
░░ Support: https://www.debian.org/support
░░ 
░░ An ExecStart= process belonging to unit nginx.service has exited.
░░ 
░░ The process' exit code is 'exited' and its exit status is 1.
Mar 19 10:01:47 kali systemd[1]: nginx.service: Failed with result 'exit-code'.
░░ Subject: Unit failed
░░ Defined-By: systemd
░░ Support: https://www.debian.org/support
░░ 
░░ The unit nginx.service has entered the 'failed' state with result 'exit-code'.
Mar 19 10:01:47 kali systemd[1]: Failed to start nginx.service - A high performance web server and a reverse proxy server.
░░ Subject: A start job for unit nginx.service has failed
░░ Defined-By: systemd
░░ Support: https://www.debian.org/support
░░ 
░░ A start job for unit nginx.service has finished with a failure.
░░ 
░░ The job identifier is 5366 and the job result is failed.

```

if you see similar error indicates that nginx is failing to start because it is unable to bind to port 8080, as that port is already in use by another process.

1. ***Check which process is using port 8080***

You need to find out which process is occupying port 8080. You can do this with the lsof or netstat command.

```bash
sudo lsof -i :8080

```
 > you might see similiar to this :  
|COMMAND | PID  | USER |  FD  |  TYPE  | DEVICE |  SIZE/OFF  | NODE  | NAME                | 
|--------|------|------|------|--------|--------|------------|-------|---------------------|
|nginx   | 7649 | dev  |  6u  |  IPv4  | 110551 |       0t0  |  TCP  | *:http-alt (LISTEN) |
|nginx   | 7650 | dev  |  6u  |  IPv4  | 110551 |       0t0  |  TCP  | *:http-alt (LISTEN) |
