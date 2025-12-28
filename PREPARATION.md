# Inception Pre-Coding Setup

## Recommendations

### Workspace

The subject requires the final app to run in a VM.
If working from home or multiple computers, a cloud VM is recommended for development!
I used the smallest droplet on DigitalOcean, it is very user friendly to set up.
You will need to install Docker if it is not already installed - instructions on their website.

Note: Create a user. If you use you login as the user, you don't have to change it later.

Please look up / research this part as there are some security concerns(!):
Add the user to the docker group (This essentially can give user access to the entire system through docker as docker group has access to the kernel and Docker runs as root)
This is a major security risk, effectively granting the user root-equivalent privileges without the safeguards of sudo.
But, it makes development easier as you don't have to type "sudo docker" instead of "docker" every time.

Options:
Add code to your repo in your favorite IDE locally, push and pull in the vm via ssh
You can ssh into the droplet and use VIM/Nano/Neovim
You can also SSH into the VM from your local VSCode - this requires a stable internet connection

### Easiest start

Create all files and directories that are required - the files can be empty, but this will help understand the paths / structures
See the subject(!!!), here is the simplified view:
```
inception/
├── Makefile
└── srcs/
    ├── .env                    <-- Your environment variables
    ├── docker-compose.yml      <-- The main orchestrator
    ├── secrets/                <-- Contains Passwords as files (one password per file so you can cat it, no new line at the end)
    └── requirements/
        ├── mariadb/
        │   ├── Dockerfile
        │   ├── conf/
        │   │   └── 50-server.cnf      <-- Custom DB config (bind-address=0.0.0.0)
        │   └── tools/
        │       └── mariadb_start.sh   <-- Startup script (init DB, create users)
        │
        ├── nginx/
        │   ├── Dockerfile
        │   ├── conf/
        │   │   └── nginx.conf         <-- NGINX config (server blocks, SSL, PHP handoff)
        │   └── tools/
        │       └── nginx_start.sh     <-- Script to generate SSL certs & start nginx
        │
        └── wordpress/
            ├── Dockerfile
            ├── conf/
            │   └── www.conf           <-- PHP-FPM pool config (listen = 9000)
            └── tools/
                └── wpscript.sh        <-- Script to download WP, create users, start PHP
``
