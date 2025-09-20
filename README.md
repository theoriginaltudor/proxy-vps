# proxy-vps

Simple Nginx reverse proxy for a VPS with Cloudflare origin certs.

## Shared Docker network

This stack expects an external Docker network named `shared_net`. Other compose projects (frontend/backend) should also attach to this network and provide network aliases so Nginx can route by name:

Example in your app repo:

```yaml
services:
	frontend:
		# ...
		expose:
			- "3000"
		networks:
			shared:
				aliases: [frontend]
	backend:
		# ...
		expose:
			- "8000"
		networks:
			shared:
				aliases: [api]

networks:
	shared:
		external: true
		name: shared_net
```

Then Nginx proxies to `http://frontend:3000` and `http://api:8000` (see `nginx/nginx.conf`). The GitHub Actions deploy workflow will create `shared_net` if it doesn't exist.
