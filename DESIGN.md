## DESIGN.md
```markdown
# DESIGN — Project 1 lbs → kg Service


## Overview
This project is a minimal, stateless HTTP API that converts pounds to kilograms. Implemented with `Node.js/Express`, reverse proxied by `NGINX`, and deployed on a single EC2 instance via `systemd`.

## Instance Information
This has the following configurations:
- OS: Ubuntu LTS (I'm much more familiar with Ubuntu than Amazon Linux)
- Type: t3.micro (t2.micro is not available on the free AWS plan)
- RSA key pair created to my local device to SSH in.
- 8 GB storage (Free plan)

## Security Group Rule Summary:

Rule 1:
- Protocol/Port: TCP 22 (SSH)
- Source: My IP

Rule 2: 
- Protocol/Port: TCP 80 (HTTP)
- Source: 0.0.0.0/0

## API format
- `GET /api/convert?lbs=<number>` → `{ "lbs": number, "kg": number, "function": "kg = lbs * 0.45359237"}`

## Nginx configuration
- NGINX was configured to reverse proxy at http://127.0.0.1:8080 (in the instance) to a publicly available HTTP listening on port 80. The configuration code can be located in deploy/nginx/p1.conf. Additionally, it was configured to have headers that help log and regulate traffic. I found the headers [here](https://linuxize.com/post/nginx-reverse-proxy).

## Systemd configuration
- This service is provided through a service created through systemd. The service code can be located in deploy/systemd/p1.service. The configuration of this system was copied directly from the assignment document.
## 

## Error Handling
- Missing `lbs` → `400 { error: 'Query param lbs is required and must be a number' }`
- Non‑numeric `lbs` → `422 { error: 'lbs must be a non-negative, finite number' }`
- Unknown route → `404 { error: 'Not found' }`


## Operations
- Managed by `systemd` for restart on failure and boot‑time start.
- Logs via `journalctl -u p1` and NGINX logs.
