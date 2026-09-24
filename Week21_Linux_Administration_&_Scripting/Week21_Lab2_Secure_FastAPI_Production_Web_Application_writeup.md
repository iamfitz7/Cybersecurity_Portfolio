# Week 21 Lab 2: Secure FastAPI Production Web Application Deployment & Recovery

## Technical Analysis Lab Write-Up

**Author:** Fitzgerald Afari-Minta  
**System:** linux-web01  
**Environment:** Ubuntu Linux Virtual Machine / Oracle VirtualBox  
**Lab Type:** Linux Administration, Web Application Deployment, Security Hardening, and Service Recovery

---

## 1. Overview

In this lab, I built and tested a small production-style web application environment on an Ubuntu Linux server named `linux-web01`.

The main goal was to go beyond simply running a Python application from the command line. I wanted the application to operate more like a service that could be used on a real Linux server.

I deployed a FastAPI application with Uvicorn as the application server. I then created a systemd service to manage the application automatically. Nginx was placed in front of the application as a reverse proxy. I also configured HTTPS/TLS, firewall rules, a dedicated service account, file permissions, health checking, and automatic service recovery.

After the environment was working, I intentionally killed the application's main process to test what would happen during a service failure. systemd detected the failure and automatically started a new Uvicorn process. I then used `journalctl`, `systemctl`, `curl`, `ss`, and other Linux tools to verify the recovery.

This lab gave me hands-on practice with Linux administration, production-style application deployment, troubleshooting, networking, service management, security controls, and incident recovery.

---

## 2. Lab Objectives

The main objectives of this lab were to:

- Deploy a FastAPI web application on Linux.
- Run the application through Uvicorn.
- Create a dedicated `webapp` Linux account for the application.
- Manage the application with systemd.
- Configure automatic restart after a service failure.
- Configure Nginx as a reverse proxy.
- Redirect HTTP traffic to HTTPS.
- Configure TLS for encrypted web traffic.
- Keep Uvicorn limited to the local loopback interface.
- Configure UFW firewall rules.
- Test the application's health endpoint.
- Simulate an application failure.
- Confirm that systemd automatically recovers the application.
- Review system logs to understand the failure and recovery process.
- Validate the final environment.

---

## 3. Technologies and Tools Used

The main technologies and tools used in this lab included:

- Ubuntu Linux
- Oracle VirtualBox
- Python
- FastAPI
- Uvicorn
- systemd
- Nginx
- UFW
- OpenSSL
- TLS/HTTPS
- curl
- journalctl
- ss
- Linux users, groups, ownership, and permissions

---

## 4. System Architecture

The final application followed this basic traffic flow:

Client
  |
  | HTTP/HTTPS
  v
Nginx
  |
  | Reverse Proxy
  v
127.0.0.1:8000
  |
  v
Uvicorn
  |
  v
FastAPI Application

Nginx was responsible for accepting web traffic on ports 80 and 443.

The FastAPI application itself was not directly exposed on the network. Uvicorn listened on:

`127.0.0.1:8000`

This meant that the backend application could only be reached locally from the server.

Nginx communicated with Uvicorn and returned the application's responses to the client.

---

## 5. Dedicated Service Account

Instead of running the web application under my normal Linux account, the application used a dedicated account named:

`webapp`

I verified the account with:

`id webapp`

The system returned a dedicated user and group for the application.

The application directory was located at:

`/opt/webapp`

I also checked the ownership and permissions of the application files.

Using a separate service account helps separate the application from normal user activity and limits what the application process should be able to access.

---

## 6. FastAPI Application

The application was created using FastAPI and served by Uvicorn.

The application included a normal endpoint that returned information showing that the service was online.

A successful request returned information similar to:

`{"status":"online","server":"linux-web01","service":"Week 21 Lab 2 Production Web Application"}`

The application also included a health endpoint:

`/health`

The health endpoint returned:

`{"status":"healthy"}`

This provided a simple way to check whether the application was responding correctly.

---

## 7. systemd Service Configuration

I created a systemd service called:

`webapp.service`

The service configuration included:

- A dedicated `webapp` user.
- A dedicated `webapp` group.
- `/opt/webapp` as the working directory.
- Uvicorn as the application process.
- `127.0.0.1:8000` as the listening address.
- Automatic restart after a failure.
- A five-second restart delay.
- Additional security settings.

The main command used by systemd was:

`/opt/webapp/venv/bin/uvicorn app:app --host 127.0.0.1 --port 8000`

The service also included:

`Restart=on-failure`

and:

`RestartSec=5`

This became especially important during the failure test later in the lab.

I also used:

`NoNewPrivileges=true`

and:

`PrivateTmp=true`

These settings added additional limits to the service environment.

I confirmed that the service was active and enabled.

---

## 8. Nginx Reverse Proxy

Nginx was configured as the public-facing web server.

Instead of exposing Uvicorn directly to the network, Nginx forwarded requests to:

`http://127.0.0.1:8000`

The reverse proxy configuration included:

`proxy_pass http://127.0.0.1:8000;`

I also configured forwarded request headers such as:

- Host
- X-Real-IP
- X-Forwarded-For
- X-Forwarded-Proto

This allowed Nginx to pass useful request information to the backend application.

I tested the Nginx configuration using:

`sudo nginx -t`

The test reported that the configuration syntax was valid and the configuration test was successful.

---

## 9. HTTPS and TLS

The web server was configured to use HTTPS.

TLS 1.2 and TLS 1.3 were enabled.

I tested normal HTTP traffic using:

`curl -I http://127.0.0.1/`

The server returned:

`HTTP/1.1 301 Moved Permanently`

and redirected the request to HTTPS.

I then tested HTTPS using:

`curl -k https://127.0.0.1/`

The application returned its expected JSON response.

I also checked the HTTP status code and received:

`HTTPS status code: 200`

This confirmed that the HTTPS request was successfully processed.

Because this was a home lab environment, I used a self-signed TLS certificate rather than a certificate from a public certificate authority.

I inspected the certificate using OpenSSL and confirmed the certificate information and validity dates.

---

## 10. Network Exposure

I used the `ss` command to examine the listening ports.

The results showed Nginx listening on:

- Port 80
- Port 443

The Uvicorn application was listening on:

`127.0.0.1:8000`

This was an important security decision.

Ports 80 and 443 were available through Nginx, while the application backend remained available only through the server's loopback interface.

This reduced unnecessary direct network exposure of the application server.

---

## 11. Firewall Configuration

UFW was enabled on the Linux server.

The default firewall policy denied incoming traffic unless a rule allowed it.

The rules allowed traffic from the lab network:

`192.168.50.0/24`

The allowed services included:

- TCP 22 for SSH management.
- TCP 80 for HTTP and redirection.
- TCP 443 for HTTPS.

This allowed systems on my lab network to reach the required services while maintaining a default-deny inbound firewall policy.

Port 8000 did not need to be exposed through UFW because Uvicorn was only listening on `127.0.0.1`.

---

## 12. Application Validation

I tested the main application endpoint using:

`curl -k https://127.0.0.1/`

The application returned:

`{"status":"online","server":"linux-web01","service":"Week 21 Lab 2 Production Web Application"}`

I tested the health endpoint using:

`curl -k https://127.0.0.1/health`

The application returned:

`{"status":"healthy"}`

These results confirmed that the request path was working through Nginx and into the FastAPI application.

---

## 13. Failure Simulation

One of the most important parts of this lab was testing how the application would respond to a process failure.

First, I identified the application's main process ID with:

`systemctl show webapp -p MainPID`

The original main process was:

`MainPID=1092`

I then intentionally killed the main process using SIGKILL:

`sudo systemctl kill --signal=SIGKILL --kill-who=main webapp`

This simulated an unexpected application process failure.

I then checked the service again.

Instead of remaining offline, systemd restarted the service.

The new Uvicorn process had:

`Main PID: 1930`

This showed that the original process had actually been terminated and replaced by a new process.

---

## 14. Log Investigation

I used `journalctl` to investigate what happened during the failure.

The logs showed that systemd sent SIGKILL to the original Uvicorn process.

The logs then reported that the main process exited because it was killed and that the service failed with a signal result.

systemd then scheduled a restart job.

A few seconds later, the logs showed that `webapp.service` started again.

Uvicorn created a new server process, completed application startup, and began listening again on:

`http://127.0.0.1:8000`

I then sent another request to the health endpoint.

The logs showed:

`GET /health HTTP/1.1 200 OK`

This confirmed that the application recovered successfully after the simulated failure.

---

## 15. Final Validation

At the end of the lab, I performed a final validation of the environment.

I confirmed:

- Hostname: `linux-web01`
- `webapp.service`: active
- `webapp.service`: enabled
- Nginx: enabled
- Nginx configuration: valid
- HTTPS application endpoint: online
- Health endpoint: healthy
- Nginx listening on ports 80 and 443
- Uvicorn listening only on `127.0.0.1:8000`

These checks confirmed that the major parts of the deployment were working together correctly.

---

## 16. Security Controls Implemented

Several security controls were included in the deployment.

### Dedicated Service Account

The application ran under the `webapp` account instead of my normal user account.

### Local-Only Backend

Uvicorn listened on:

`127.0.0.1:8000`

This prevented the backend application server from being directly exposed to the lab network.

### Reverse Proxy

Nginx handled client-facing web traffic and forwarded requests to the backend.

### HTTPS

Web traffic could use TLS encryption through HTTPS.

### HTTP Redirect

HTTP requests were redirected to HTTPS.

### Firewall

UFW used a default-deny incoming policy and allowed required services from the lab network.

### Service Hardening

The systemd configuration included:

`NoNewPrivileges=true`

and:

`PrivateTmp=true`

### Automatic Recovery

systemd was configured to restart the application when the main process failed.

---

## 17. Troubleshooting Approach

This lab helped me understand that troubleshooting a Linux web application requires checking different layers instead of immediately assuming the application itself is the problem.

A useful troubleshooting order for this environment is:

1. Check whether the application service is running.
2. Check the systemd logs.
3. Check whether Uvicorn is listening on port 8000.
4. Test the application locally.
5. Check the Nginx configuration.
6. Check whether Nginx is running.
7. Check ports 80 and 443.
8. Check the firewall.
9. Test HTTP and HTTPS.
10. Check the health endpoint.

Commands such as `systemctl`, `journalctl`, `ss`, `curl`, `nginx -t`, `ufw`, and `openssl` each provide a different part of the troubleshooting picture.

---

## 18. What I Learned

This lab helped me better understand how several Linux technologies work together.

Before completing this lab, it would have been easy to think of a web application as simply starting Python and opening a port.

A production-style deployment has more layers.

I learned how the application process can be managed by systemd, how Nginx can sit in front of the application, how HTTPS can protect web traffic, and how firewall rules can control which systems can connect.

The failure test was especially useful.

Seeing the process fail, finding the failure in `journalctl`, watching systemd restart the service, and confirming that the health endpoint worked again made automatic service recovery much easier for me to understand.

---

## 19. Skills Demonstrated

This lab provided hands-on practice with:

- Linux server administration
- Linux command-line troubleshooting
- systemd service creation and management
- Process management
- Linux users and groups
- File ownership and permissions
- Python virtual environments
- FastAPI
- Uvicorn
- Nginx
- Reverse proxy configuration
- HTTP and HTTPS
- TLS certificates
- OpenSSL
- UFW firewall configuration
- TCP ports and listening services
- curl
- ss
- journalctl
- Application health checks
- Service failure simulation
- Automatic service recovery
- Security hardening
- Log analysis
- Production-style application deployment

---

## 20. Conclusion

In Week 21 Lab 2, I deployed and secured a FastAPI web application on an Ubuntu Linux server.

The final environment used Nginx for client-facing HTTP and HTTPS traffic while Uvicorn remained limited to `127.0.0.1:8000`. The application ran through a dedicated `webapp` service account and was managed by systemd.

I also configured firewall restrictions, TLS, HTTP-to-HTTPS redirection, service hardening, and application health checking.

Most importantly, I did not only verify that the application worked under normal conditions. I intentionally killed the application's main process and investigated the event through system logs. systemd detected the failure, restarted the application with a new process, and the health endpoint returned successfully afterward.

This lab gave me practical experience with both deploying a Linux service and troubleshooting what happens when that service fails.