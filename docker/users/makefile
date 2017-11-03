start-local:
	@mkdir -p /tmp/log/
	@mkdir -p /tmp/log/users/
	@docker create -v /tmp/log/users:/tmp/log/users --name users-logs busybox
	@docker run --name users \
	-e "active.profile=default" \
	-e "configservice.url=http://configservice:8888" \
	-e "discovery.url=http://discovery:8761/eureka/" \
	--link redis-cluster:redis-cluster \
	--link infra_configservice_1:configservice \
	--link infra_discovery_1:discovery \
	--link memcache_cache1_1:cache1 \
	--link memcache_cache2_1:cache2 \
	--link postgres9.5:db \
	--volumes-from users-logs \
	-d -p 8585:8585 octi/users:latest
	@echo "Started"

start-dev:
	@mkdir -p /tmp/log/
	@mkdir -p /tmp/log/users/
	@docker create -v /tmp/log/users:/tmp/log/users --name users-logs busybox
	@docker run --name users \
	-e "active.profile=dev" \
	-e "configservice.url=http://configservice:8888" \
	-e "discovery.url=http://discovery:8761/eureka/" \
	--link infra_configservice_1:configservice \
	--link infra_discovery_1:discovery \
	--link memcache_cache1_1:cache1 \
	--link memcache_cache2_1:cache2 \
	--link redis-cluster:redis-cluster \
	--volumes-from users-logs \
	-d -p 8585:8585 octi/users:latest
	@echo "Started"

stop:
		@docker stop users 2>/dev/null || echo "users container not running"
		@docker rm users 2>/dev/null || echo "users container does not exist"
		@docker rm users-logs 2>/dev/null || echo "users-logs volume does not exist"
		@docker rmi $(docker images -q --filter "dangling=true") 2>/dev/null || echo "No dangling containers to remove"
clean:
		@docker stop users 2>/dev/null || echo "users container not running"
		@docker rm users 2>/dev/null || echo "users container does not exist"
		@docker rm users-logs 2>/dev/null || echo "users-logs volume does not exist"
		@docker rmi $(docker images -q --filter "dangling=true") 2>/dev/null || echo "No dangling containers to remove"
		@docker rmi octi/users:latest 2>/dev/null || echo "local octi/users image does not exist"

clean-data: clean
clean-images: clean
