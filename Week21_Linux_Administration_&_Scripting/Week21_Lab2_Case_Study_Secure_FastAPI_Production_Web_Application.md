# Week 21 Lab 2: Secure FastAPI Production Web Application

## Incident Case Study: Uvicorn Process Failure and Automatic Service Recovery

**Author:** Fitzgerald Afari-Minta  
**Affected System:** linux-web01  
**Affected Service:** webapp.service  
**Application:** FastAPI / Uvicorn  
**Incident Type:** Simulated Application Process Failure  
**Environment:** Ubuntu Linux Home Lab

---

## 1. Incident Summary

During Week 21 Lab 2, I intentionally created an application failure to test whether my Linux server could recover without manually restarting the application.

The FastAPI application was running through Uvicorn and was managed by a systemd service called:

`webapp.service`

The service was configured with:

`Restart=on-failure`

and:

`RestartSec=5`

To test this configuration, I intentionally sent a SIGKILL signal to the application's main Uvicorn process.

The original process had PID:

`1092`

After the process was killed, systemd detected that the application had failed.

systemd then scheduled an automatic restart and started a new Uvicorn process.

The replacement process had PID:

`1930`

After the recovery, I tested the application's `/health` endpoint.

The application returned:

`{"status":"healthy"}`

The logs also recorded a successful:

`GET /health HTTP/1.1 200 OK`

The test confirmed that the automatic recovery configuration worked correctly.

---

## 2. Environment Before the Incident

Before creating the failure, the application was operating normally.

The environment consisted of:

- Ubuntu Linux server named `linux-web01`
- FastAPI web application
- Uvicorn application server
- systemd service management
- Nginx reverse proxy
- HTTPS/TLS
- UFW firewall
- Dedicated `webapp` service account

The request path was:

Client -> Nginx -> Uvicorn -> FastAPI

Nginx accepted web traffic on ports 80 and 443.

Uvicorn listened only on:

`127.0.0.1:8000`

The application health endpoint was responding normally.

---

## 3. Detection

Before causing the failure, I checked the application's main process using:

`systemctl show webapp -p MainPID`

The result was:

`MainPID=1092`

This identified PID 1092 as the main Uvicorn process being managed by systemd.

---

## 4. Failure Simulation

To simulate an unexpected application crash, I ran:

`sudo systemctl kill --signal=SIGKILL --kill-who=main webapp`

SIGKILL immediately terminated the main application process.

This was intentional and was performed in a controlled home lab environment.

The purpose was to test whether the application would remain unavailable or whether systemd would recover it automatically.

---

## 5. Initial Impact

When PID 1092 was killed, the application's main Uvicorn process stopped.

If automatic recovery had not been configured correctly, the FastAPI application would have remained unavailable until an administrator manually restarted it.

However, `webapp.service` was configured with:

`Restart=on-failure`

This instructed systemd to respond when the process ended because of a failure.

---

## 6. Investigation

I checked the service status using:

`systemctl status webapp --no-pager`

The service was already back to:

`active (running)`

However, an important detail had changed.

The new main process was:

`Main PID: 1930 (uvicorn)`

The original process was PID 1092.

This showed that the old Uvicorn process had not simply continued running. It had been terminated and replaced with a new process.

I then used `journalctl` to investigate the event in more detail.

---

## 7. Log Evidence

The system logs recorded the failure and recovery.

The logs showed that SIGKILL was sent to PID 1092.

They then reported:

`Main process exited, code=killed, status=9/KILL`

The service also reported:

`Failed with result 'signal'.`

systemd then logged:

`Scheduled restart job, restart counter is at 1.`

A few seconds later:

`Started webapp.service - Week 21 Lab 2 FastAPI Production Web Application.`

Uvicorn then reported that a new server process started.

The new process was PID 1930.

The application completed startup and began listening again on:

`http://127.0.0.1:8000`

This provided clear evidence of the full failure and recovery process.

---

## 8. Timeline

### Before Failure

The FastAPI application was operating normally.

The Uvicorn main process was:

`PID 1092`

### Failure Introduced

I intentionally sent SIGKILL to the main process.

### Failure Detected

systemd recorded:

`status=9/KILL`

and:

`Failed with result 'signal'.`

### Recovery Scheduled

systemd scheduled an automatic restart.

The configured restart delay was five seconds.

### Service Restarted

systemd started `webapp.service` again.

### New Process Created

Uvicorn started under:

`PID 1930`

### Application Startup Completed

The application returned to its normal listening state on:

`127.0.0.1:8000`

### Health Check

I sent a request to:

`/health`

The application returned:

`{"status":"healthy"}`

The server logs recorded:

`GET /health HTTP/1.1 200 OK`

---

## 9. Root Cause

The immediate cause of the outage was the intentional SIGKILL sent to the Uvicorn process.

The command used was:

`sudo systemctl kill --signal=SIGKILL --kill-who=main webapp`

Therefore, this was not an unexpected real-world crash.

It was a controlled failure created specifically to test the application's recovery configuration.

---

## 10. Recovery Mechanism

The main recovery control was systemd.

The service configuration contained:

`Restart=on-failure`

and:

`RestartSec=5`

Because the process ended as a failure, systemd recognized the event and scheduled a restart.

This meant I did not have to manually run:

`systemctl restart webapp`

The service manager handled the recovery automatically.

---

## 11. Validation After Recovery

I validated the recovered application using:

`curl -k https://127.0.0.1/health`

The response was:

`{"status":"healthy"}`

I also reviewed the logs and found:

`GET /health HTTP/1.1 200 OK`

This confirmed that the new application process was able to successfully process requests after the failure.

I also validated the wider environment.

Nginx remained operational.

The Nginx configuration passed:

`sudo nginx -t`

The web application continued to work through HTTPS.

Uvicorn continued listening only on:

`127.0.0.1:8000`

This showed that the recovery did not break the intended network design.

---

## 12. Security Observations

The incident also demonstrated why the surrounding Linux configuration mattered.

### Dedicated Service Account

The application was running as the `webapp` user instead of my personal Linux account.

### Backend Isolation

Uvicorn was listening only on:

`127.0.0.1:8000`

The backend was therefore not directly exposed to other systems on the lab network.

### Reverse Proxy

Nginx handled external web requests on ports 80 and 443.

### Firewall

UFW used a default-deny incoming policy and allowed required traffic from the lab network.

### HTTPS

The web application supported encrypted HTTPS traffic.

### Process Supervision

systemd watched the application process and automatically responded to its failure.

Together, these controls made the deployment more secure and easier to manage.

---

## 13. Lessons Learned

The biggest lesson from this incident was that having a working application is not enough.

Applications can fail.

A better system should provide a way to detect the failure, record what happened, recover when appropriate, and allow an administrator to verify that the service is healthy again.

I also learned why logs are important.

If I had only checked `systemctl status webapp` after the failure, I would have seen that the service was active again.

That would not explain what happened.

`journalctl` showed the complete story:

Original process running -> process killed -> failure detected -> restart scheduled -> new process started -> application initialized -> health request succeeded.

This made the failure much easier to understand.

---

## 14. Troubleshooting Process

If I encountered a similar application outage without already knowing the cause, I would use a process similar to this:

1. Check `systemctl status webapp`.
2. Review `journalctl` for service errors.
3. Identify the application's current PID.
4. Check whether port 8000 is listening.
5. Test the backend locally if needed.
6. Check the Nginx configuration.
7. Check whether Nginx is active.
8. Check ports 80 and 443.
9. Review UFW rules.
10. Test the HTTPS application endpoint.
11. Test `/health`.
12. Confirm that the application remains stable.

This approach would help me narrow down whether the problem was related to the application, systemd, networking, Nginx, TLS, or the firewall.

---

## 15. Corrective and Preventive Controls

Several controls in this lab helped reduce the impact of the simulated failure.

### Automatic Restart

`Restart=on-failure` allowed systemd to recover the application automatically.

### Restart Delay

`RestartSec=5` provided a short delay before restarting the failed process.

### Health Endpoint

The `/health` endpoint gave me a quick way to verify that the application was functioning after recovery.

### Central Service Logs

systemd and `journalctl` provided useful records of the failure and restart.

### Reverse Proxy Architecture

Nginx remained separate from the backend application process.

### Limited Backend Exposure

The application server was limited to the loopback interface instead of being directly exposed to the network.

---

## 16. Possible Future Improvements

If this were expanded beyond a home lab, I would consider adding:

- Centralized log collection.
- Monitoring and alerting.
- Automated health monitoring.
- Notifications when the service repeatedly fails.
- Rate limiting.
- Regular TLS certificate management and renewal.
- A publicly trusted certificate where appropriate.
- Configuration management or infrastructure automation.
- Application dependency monitoring.
- Backup and recovery procedures.
- More systemd hardening options after compatibility testing.
- A separate non-login service account configuration.
- Automated deployment testing.

These improvements would make the environment easier to monitor and manage at a larger scale.

---

## 17. Skills Demonstrated During the Incident

This incident case study demonstrates hands-on experience with:

- Linux incident troubleshooting
- systemd
- Process management
- Service failure simulation
- SIGKILL
- journalctl
- Linux log analysis
- Application recovery
- Health checks
- FastAPI
- Uvicorn
- Nginx
- Reverse proxies
- HTTPS/TLS
- Firewall validation
- TCP port analysis
- curl
- ss
- Root cause identification
- Recovery validation
- Security hardening

---

## 18. Final Outcome

The simulated incident was successfully detected, investigated, and recovered.

The original Uvicorn process:

`PID 1092`

was intentionally terminated.

systemd detected the failure and recorded:

`status=9/KILL`

The service was automatically restarted.

The replacement Uvicorn process became:

`PID 1930`

The application then returned:

`{"status":"healthy"}`

and the logs confirmed a successful:

`GET /health HTTP/1.1 200 OK`

No manual application restart was required.

The test confirmed that the systemd recovery configuration worked as intended.

---

## 19. Conclusion

This incident simulation gave me practical experience with more than simply installing and configuring Linux software.

I was able to observe an application before a failure, intentionally create the failure, investigate the system logs, identify how Linux responded, verify that a new process was created, and confirm that the application became healthy again.

The main lesson I took from this exercise is that reliability is not only about preventing failures. It is also about preparing the system to respond when failures happen.

By combining systemd process supervision, logging, health checks, Nginx, TLS, firewall controls, and a dedicated application account, I created a small environment where I could practice both Linux administration and real troubleshooting.