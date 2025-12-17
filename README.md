# Simple Run Guide (Part B)

Quick instructions to get crAPI running and test it with the fuzzer.

Navigate to `Part_B/crAPI_with_fuzzer/deploy/docker`

*You will need to download and unzip crAPI_with_fuzzer*

Link: https://github.com/ipapidi/ITC4247_Final_Project.git

## Prerequisites

- Docker and Docker Compose installed
- Docker Compose version 1.27.0 or higher

Check your versions:
```bash
docker --version
docker compose version
```

## Starting crAPI with Docker

1. Navigate to the docker directory:
   ```bash
   cd deploy/docker
   ```

2. Start all services:
   ```bash
   docker compose -f docker-compose.yml --compatibility up -d
   ```

3. Wait for services to start (check status):
   ```bash
   docker compose ps
   ```

4. Access the application:
   - **Web Interface:** http://localhost:8888

## Stopping crAPI

Stop all services:
```bash
cd deploy/docker
docker compose -f docker-compose.yml --compatibility down
```

## Running the Security Fuzzer

1. Install Python dependencies:
   ```bash
   pip install -r requirements-fuzzer.txt
   ```

2. Make sure crAPI is running (see above)

3. Run the fuzzer:
   ```bash
   python3 fuzzer.py
   ```

   The fuzzer will:
   - Automatically test all endpoints
   - Generate an HTML report
   - Start a web interface on http://localhost:9999

### Fuzzer Options

- Test against a different URL:
  ```bash
  python3 fuzzer.py -u http://192.168.1.100:8888
  ```

- Run without authentication:
  ```bash
  python3 fuzzer.py --no-auth
  ```

- Run comprehensive tests (with and without auth):
  ```bash
  python3 fuzzer.py --all
  ```

For detailed fuzzer documentation, see [FUZZER_README.md](FUZZER_README.md)

## Troubleshooting

**Port already in use:**
- Check what's using the port: `lsof -i :8888` (Linux/macOS) or `netstat -ano | findstr :8888` (Windows)
- Stop conflicting services or change ports in `docker-compose.yml`

**Services not starting:**
- Check logs: `docker compose logs`
- Check status: `docker compose ps`
- Restart: `docker compose restart`
- Force recreate web service: `docker compose up -d --force-recreate crapi-web`

**Fuzzer connection errors:**
- Ensure crAPI is running: `docker compose ps` in `deploy/docker`
- Check the URL: `python3 fuzzer.py -u http://localhost:8888`

