# VAmPI Setup and Testing Instructions

This guide shows how to run and test VAmPI using Docker.

## What You Need

1. **Docker Desktop** - Download from docker.com if you don't have it
2. **curl** - Usually already installed
3. **jq** (optional) - For extracting tokens. On Windows you can use PowerShell instead which doesn't need jq.

**Windows users:** Use PowerShell for best results, or Git Bash if you prefer. Commands are provided for both.

---

## Quick Setup

### Step 1: Start the API

Navigate to the VAmPI folder, then run:

```bash
docker-compose up -d
```

This starts two instances:
- Port 5001: Secure version (vulnerable=0)
- Port 5002: Vulnerable version (vulnerable=1)

**Or use a single container:**

```bash
docker run -d --name vampi-vulnerable -e vulnerable=1 -p 5002:5000 erev0s/vampi:latest
```

Check it's running:
```bash
docker ps
```

### Step 2: Initialize Database

**Important:** Do this first or nothing will work.

**Windows (PowerShell):**
```powershell
Invoke-RestMethod -Uri http://127.0.0.1:5002/createdb
```

**Other platforms:**
```bash
curl http://127.0.0.1:5002/createdb
```

You should get: `{"message": "Database populated."}`

### Step 3: Check it's Working

Visit in your browser: `http://127.0.0.1:5002/`

Or use curl:
```bash
curl http://127.0.0.1:5002/
```

Should see `"vulnerable": 1` in the response.

---

## Basic Testing

### View Users
```bash
curl http://127.0.0.1:5002/users/v1
```

### Register a User

**PowerShell:**
```powershell
Invoke-RestMethod -Uri http://127.0.0.1:5002/users/v1/register -Method POST -ContentType "application/json" -Body '{"username": "testuser", "password": "test123", "email": "test@example.com"}'
```

**Other:**
```bash
curl -X POST http://127.0.0.1:5002/users/v1/register -H "Content-Type: application/json" -d '{"username": "testuser", "password": "test123", "email": "test@example.com"}'
```

### Login and Get Token

**PowerShell (easiest - no jq needed):**
```powershell
$response = Invoke-RestMethod -Uri http://127.0.0.1:5002/users/v1/login -Method POST -ContentType "application/json" -Body '{"username": "name1", "password": "pass1"}'
$env:TOKEN = $response.auth_token
echo $env:TOKEN
```

**Other platforms:**
```bash
export TOKEN=$(curl -s -X POST http://127.0.0.1:5002/users/v1/login -H "Content-Type: application/json" -d '{"username": "name1", "password": "pass1"}' | jq -r '.auth_token')
echo $TOKEN
```

**Default users (created by /createdb):**
- name1 / pass1
- name2 / pass2  
- admin / pass1

### Test Protected Endpoint

**PowerShell:**
```powershell
Invoke-RestMethod -Uri http://127.0.0.1:5002/me -Headers @{"Authorization" = "Bearer $env:TOKEN"}
```

**Other:**
```bash
curl http://127.0.0.1:5002/me -H "Authorization: Bearer $TOKEN"
```

---

## Compare Secure vs Vulnerable

If you used docker-compose, you can test both:

**Vulnerable (port 5002)** - Should allow unauthorized password change
**Secure (port 5001)** - Should block it

Try the password change test on both ports to see the difference.

---

## Troubleshooting

**"Connection refused" or nothing works:**
- Make sure Docker is running (check system tray)
- Check containers are up: `docker ps`
- Make sure you ran `/createdb` first

**Token extraction fails:**
- On Windows, use PowerShell method (doesn't need jq)
- On Mac/Linux, install jq: `brew install jq` or `sudo apt-get install jq`
- Make sure you use `export TOKEN=...` (or `$env:TOKEN` in PowerShell)

**Port already in use:**
- Windows: `netstat -ano | findstr :5002`
- Mac/Linux: `lsof -i :5002`
- Or just use a different port: `-p 5003:5000`

**"Invalid authorization header":**
- Token probably expired (default is 60 seconds)
- Just login again to get a new token

---

## Stop Everything

```bash
docker-compose down
```

Or for single container:
```bash
docker stop vampi-vulnerable
docker rm vampi-vulnerable
```

---

## Swagger UI

You can also test everything in the browser:
```
http://127.0.0.1:5002/ui/
```

Much easier than curl for exploring the API.

