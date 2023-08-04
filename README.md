# Nginx Forward Proxy

This project provides a Dockerized setup for configuring Nginx as a forward proxy. The configuration is inspired by the article [Configuring Nginx as a Forward Proxy](https://getconvoy.io/docs/forward-proxies/nginx/#configuring-nginx-as-a-forward-proxy) from the Convoy documentation.

## Prerequisites

Before running this project, ensure that you have the following installed:

- Docker

## Getting Started

To get started with this project, follow these steps:

1. Clone the repository:

   ```shell
   git clone git@github.com:yangtuooc/forward-proxies-docker.git
   ```

2. Change to the project directory:

   ```shell
   cd forward-proxies-docker
   ```

3. Build the Docker image:

   ```shell
   docker build -t forward-proxies-docker .
   ```

4. Run the Docker container:

   ```shell
   docker run -d -p 8080:8080 forward-proxies-docker
   ```

## Configuration

The Nginx configuration file is located at `nginx.conf`. You can modify this file to customize the proxy settings as per your requirements.

```nginx
load_module /usr/lib/nginx/modules/ngx_http_proxy_connect_module.so;

events {
	worker_connections 1024;
}

http {
	server {
		server_name "proxy.xxx.com";
		listen 8080;
		resolver 8.8.8.8; # DNS server, you can use the proxy server's DNS server
		proxy_connect; # enable proxy_connect
		proxy_connect_connect_timeout 10s;
		proxy_connect_data_timeout 10s;
		location / {
			proxy_pass $scheme://$http_host$request_uri;
			proxy_buffers 256 4k;
		}
	}
}
```

Make sure to update the `server_name` directive with your preferred server name.

## Usage

Once the Docker container is up and running, you can start using the Nginx forward proxy by configuring your client to use `proxy.xxx.com` on port `8080` as the proxy server. Replace `proxy.xxx.com` with the appropriate server address.

## License

This project is licensed under the [MIT License](LICENSE).

## Acknowledgements

This project is based on the article [Configuring Nginx as a Forward Proxy](https://getconvoy.io/docs/forward-proxies/nginx/#configuring-nginx-as-a-forward-proxy) from the Convoy documentation.
