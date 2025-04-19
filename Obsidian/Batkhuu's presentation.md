
--- 
### Global terms:
- Technical dept
- 12 factor app
- data source name - DSN
- pattern / anti-pattern
- BUILD > RELEASE (BUILD + config ) > RUN ? ROLLBACK 
---

Write dependency metadata (requriements.txt package.json)
config: yaml, json, toml (not xml) easier in docker (also .env) 
- data source names
	- should be configurable
	- should check by requesting in some interval
- frontend and backend

envionment should be the same in every environment (dev, prod, main)


several services running together > docker-compose.yml
to connect with third party services, use their own connection services like protocols




