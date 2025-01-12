# Documentation for the `stack` Folder

## Introduction
The `stack` folder contains configurations and files for running the application using Docker Compose and `.gitignore` rules to manage file exclusions in the version control system.

---

## Folder Structure
```
stack/
├── .gitignore
└── compose.yml
```

---

## Files

### `.gitignore`
This file excludes temporary files, logs, directories such as `node_modules/`, and tool caches (e.g., `.playwright-cache/`).
Example:
```plaintext
.DS_Store
*.log
node_modules/
.playwright-cache/
```

### `compose.yml`
Defines Docker Compose services:

- **`reverse-proxy` (Traefik):** A reverse proxy handling HTTP/HTTPS and automatic SSL certificates.
- **`api`:** The application backend running on port `8000`, using the `.env` file for configuration.
- **`watchtower`:** Automatically updates containers every `600` seconds.
- **`whoami`:** A simple test service for verifying the configuration.

Example configuration:
```yaml
version: '3'
services:
  reverse-proxy:
    image: traefik:v3.2
    ports:
      - "80:80"
      - "443:443"
    volumes:
      - ./letsencrypt:/letsencrypt
      - /var/run/docker.sock:/var/run/docker.sock
  api:
    image: cubeofdestiny/api-image
    ports:
      - "8000:8000"
    env_file:
      - .env
  watchtower:
    image: containrrr/watchtower
    command: --interval 600
```

---

## Running the Services
1. Start the services:
   ```bash
   docker-compose up -d
   ```
2. Verify the API (`http://localhost:8000`) and the test service (`http://localhost`).

---

## Notes
- **Security:** Protect the `.env` file.
- **Extensibility:** Add services by modifying `compose.yml`.
