start-local:
	@mkdir -p /tmp/log/
	@mkdir -p /tmp/log/feeds/
	@docker create -v /tmp/log/feeds:/tmp/log/feeds --name feeds-logs busybox
	@docker run --name feeds \
	-e "active.profile=default" \
	-e "configservice.url=http://configservice:8888" \
	-e "discovery.url=http://discovery:8761/eureka/" \
	--link redis-cluster:redis-cluster \
	--link infra_configservice_1:configservice \
	--link infra_discovery_1:discovery \
	--link memcache_cache1_1:cache1 \
	--link memcache_cache2_1:cache2 \
	--volumes-from feeds-logs \
	-d -p 9090:9090 octi/feeds:latest
	@echo "Started"

start-dev:
	@mkdir -p /tmp/log/
	@mkdir -p /tmp/log/feeds/
	@docker create -v /tmp/log/feeds:/tmp/log/feeds --name feeds-logs busybox
	@docker run --name feeds \
	-e "active.profile=dev" \
	-e "configservice.url=http://configservice:8888" \
	-e "discovery.url=http://discovery:8761/eureka/" \
	--link infra_configservice_1:configservice \
	--link infra_discovery_1:discovery \
	--link memcache_cache1_1:cache1 \
	--link memcache_cache2_1:cache2 \
	--link redis-cluster:redis-cluster \
	--volumes-from feeds-logs \
	-d -p 9090:9090 octi/feeds:latest
	@echo "Started"

stop:
		@docker stop feeds 2>/dev/null || echo "feeds container not running"
		@docker rm feeds 2>/dev/null || echo "feeds container does not exist"
		@docker rm feeds-logs 2>/dev/null || echo "feeds-logs volume does not exist"
		@docker rmi $(docker images -q --filter "dangling=true") 2>/dev/null || echo "No dangling containers to remove"
clean:
		@docker stop feeds 2>/dev/null || echo "feeds container not running"
		@docker rm feeds 2>/dev/null || echo "feeds container does not exist"
		@docker rm feeds-logs 2>/dev/null || echo "feeds-logs volume does not exist"
		@docker rmi $(docker images -q --filter "dangling=true") 2>/dev/null || echo "No dangling containers to remove"
		@docker rmi octi/feeds:latest 2>/dev/null || echo "local octi/feeds image does not exist"

clean-data: clean
clean-images: clean
