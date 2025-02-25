# WSO2 API Manager with NGINX Reverse Proxy

This repository contains the configuration files and instructions to set up a WSO2 API Manager (APIM) with an NGINX reverse proxy. The purpose of this setup is to ensure that the internal URLs of the WSO2 APIM are not exposed to the public, and all traffic is routed through the NGINX reverse proxy.

## Purpose

The main goal of this repository is to provide a secure and efficient way to manage API traffic using WSO2 API Manager, while hiding the internal URLs and ports behind an NGINX reverse proxy. This setup ensures that users only interact with the public-facing URLs, enhancing security and simplifying URL management.

## Prerequisites

- Docker
- Docker Compose

## Repository Structure

- `nginx/conf.d/nginx.conf`: NGINX configuration file.
- `deployment.toml`: Configuration file for WSO2 API Manager.
- `docker-compose.yml`: Docker Compose file to run the containers.

## Setup Instructions

1. **Clone the Repository**

   ```sh
   git clone https://github.com/yourusername/wso2-nginx-proxy.git
   cd wso2-nginx-proxy
   ```

2. **Configure SSL Certificates**

Place your SSL certificates in the nginx/ssl directory. Ensure you have the following files:

- apim.localhost.crt
- apim.localhost.key

3. **Update Configuration Files**

Ensure the nginx.conf and deployment.toml files are correctly configured for your environment.

4. **Run Docker Compose**

Start the containers using Docker Compose:

```sh
docker-compose up -d
```

5. **Access the Services**

- WSO2 API Manager: https://apim.localhost

**Configuration Details**

**NGINX Configuration**
The NGINX configuration file (nginx/conf.d/nginx.conf) is set up to forward requests to the WSO2 API Manager while hiding the internal URLs and ports. It uses proxy_redirect and sub_filter directives to ensure that all internal URLs are replaced with the public-facing URLs.

**WSO2 API Manager Configuration**
The deployment.toml file is configured to use apim.localhost as the hostname. This ensures that all internal references within WSO2 APIM use the public-facing URL.

**Docker Compose**
The docker-compose.yml file sets up two services:

- wso2_apim: The WSO2 API Manager container.
- nginx: The NGINX reverse proxy container.

**Troubleshooting**
If you encounter any issues, check the logs for both the NGINX and WSO2 APIM containers:
```sh
docker-compose logs nginx
docker-compose logs wso2_apim
```

Ensure that the SSL certificates are correctly placed and that the configuration files are properly set up.

**License**
This project is licensed under the MIT License. See the LICENSE file for details.

**Contributing**
Contributions are welcome! Please open an issue or submit a pull request for any improvements or bug fixes.