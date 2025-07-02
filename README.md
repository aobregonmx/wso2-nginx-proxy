# WSO2 API Manager with NGINX Reverse Proxy

This repository contains the configuration files and instructions to set up a WSO2 API Manager (APIM) with an NGINX reverse proxy. The purpose of this setup is to ensure that the internal URLs of the WSO2 APIM are not exposed to the public, and all traffic is routed through the NGINX reverse proxy.

## Purpose

The main goal of this repository is to provide a secure and efficient way to manage API traffic using WSO2 API Manager, while hiding the internal URLs and ports behind an NGINX reverse proxy. This setup ensures that users only interact with the public-facing URLs, enhancing security and simplifying URL management.

## Prerequisites

- Docker
- Docker Compose

## Repository Structure

```
├── nginx/
│   ├── conf.d/
│   │   └── nginx.conf     # NGINX configuration for reverse proxy
│   └── ssl/
│       ├── selfsigned.crt # Self-signed certificate for HTTPS
│       └── selfsigned.key # Private key for SSL certificate
├── wso2/
│   └── conf/
│       └── deployment.toml # WSO2 API Manager configuration file
├── docker-compose.yml     # Docker compose configuration
├── LICENSE                # License file
├── README.md              # This documentation file
└── CONTRIBUTING.md        # Guidelines for contributing
```

## Setup Instructions

1. **Clone the Repository**

```sh
git clone https://github.com/aobregon/wso2-nginx-proxy.git
cd wso2-nginx-proxy
```

2. **Configure SSL Certificates**

Generate a self-signed SSL certificate or use your own SSL certificate. If you choose to generate a self-signed certificate, you can use the following command:
```sh
openssl req -x509 -nodes -days 365 -newkey rsa:2048 \
  -keyout ./nginx/ssl/selfsigned.key -out ./nginx/ssl/selfsigned.crt \
  -subj "/C=MX/ST=CDMX/L=México City/O=wso2.com/OU=IT/CN=apim.localhost"
``` 
3. **Update Configuration Files (optional)**
Update the following configurations in the respective files for your environment:

> **Self-signed SSL certificate**:
> - The command above generates a self-signed certificate and key.
> - The certificate is valid for 365 days and can be customized with your organization's details.
> - Customize the `-subj` values (`/C`, `/ST`, `/L`, `/O`, `/OU`, `/CN`) to match your organization's details and location.

> **nginx.conf**:
> - Replace `apim.localhost` with your desired domain name.
> - Update the SSL certificate and key paths to match the generated or provided certificates.

> **deployment.toml**:
> - Set the `hostname` property under `[server]` to match your public-facing domain (e.g., `apim.localhost`).

> **compose.yml**:
> - Ensure the `extra_hosts` entry points to the correct IP address of your NGINX container. This is typically the IP address of the NGINX service in your Docker network.
> - Obtain the IP of NGINX with:
> `docker inspect -f '{{range .NetworkSettings.Networks}}{{.IPAddress}}{{end}}' nginx-proxy`
> - If the IP changes, update this value.

4. **Start the containers using Docker Compose**

Return to the root directory of the repository and run:
```sh
cd ..
```
Run the following command to start the containers:
```sh
docker-compose up -d --build
```

5. **Access the Services**
- WSO2 API Manager: https://apim.localhost

> **Note**: Ensure that your local DNS or `/etc/hosts` file is configured to resolve `apim.localhost` `gw.localhost` to the appropriate IP address (e.g., `127.0.0.1` for local setups).
```sh
127.0.0.1       apim.localhost
127.0.0.1       gw.localhost
```
## Configuration Details

**NGINX Configuration**
The NGINX configuration file (nginx/conf.d/nginx.conf) is set up to forward requests to the WSO2 API Manager while hiding the internal URLs and ports. It uses proxy_redirect and sub_filter directives to ensure that all internal URLs are replaced with the public-facing URLs.

**WSO2 API Manager Configuration**
The deployment.toml file is configured to use apim.localhost as the hostname. This ensures that all internal references within WSO2 APIM use the public-facing URL.

**Docker Compose**
The compose.yml file sets up two services:

- wso2_apim: The WSO2 API Manager container.
- nginx: The NGINX reverse proxy container.

**Troubleshooting**
If you encounter any issues, check the logs for both the NGINX and WSO2 APIM containers:
```sh
docker compose logs nginx
docker compose logs wso2_apim
```

**License**
This project is licensed under the MIT License. See the [LICENSE](./LICENSE) file for details.
**License**
This project is licensed under the MIT License. See the LICENSE file for details.

**Contributing**
Contributions are welcome! Please refer to the [CONTRIBUTING.md](CONTRIBUTING.md) file for detailed guidelines. You can also open an issue or submit a pull request for any improvements or bug fixes.

## Portals

The WSO2 API Manager consists of several web interfaces, all accessible through the NGINX reverse proxy:
The default password for the admin user is `admin` and the username is `admin`.

- **Admin Portal**: [https://apim.localhost/admin](https://apim.localhost/admin) - For API administration and governance tasks, such as managing users, roles, and permissions, and monitoring API usage.
- **Publisher Portal**: [https://apim.localhost/publisher](https://apim.localhost/publisher) - For API creation, documentation, and lifecycle management, including defining API resources, policies, and publishing APIs.
- **Developer Portal**: [https://apim.localhost/devportal](https://apim.localhost/devportal) - For discovering, subscribing, and testing APIs, allowing developers to explore available APIs and integrate them into their applications.
- **Carbon Console**: [https://apim.localhost/carbon](https://apim.localhost/carbon) - For server administration, such as configuring system settings, managing tenants, and monitoring server health.