start-local:
	@mkdir -p /tmp/log/
	@mkdir -p /tmp/log/graph/
	@docker create -v /tmp/log/graph:/tmp/log/graph --name graph-logs busybox
	@docker run --name graph \
	-e "active.profile=default" \
	-e "configservice.url=http://configservice:8888" \
	-e "discovery.url=http://discovery:8761/eureka/" \
	--link redis-cluster:redis-cluster \
	--link infra_configservice_1:configservice \
	--link infra_discovery_1:discovery \
	--link memcache_cache1_1:cache1 \
	--link memcache_cache2_1:cache2 \
	--volumes-from graph-logs \
	-d -p 9595:9595 octi/graph:latest
	@echo "Started"

start-dev:
	@mkdir -p /tmp/log/
	@mkdir -p /tmp/log/graph/
	@docker create -v /tmp/log/graph:/tmp/log/graph --name graph-logs busybox
	@docker run --name graph \
	-e "active.profile=dev" \
	-e "configservice.url=http://configservice:8888" \
	-e "discovery.url=http://discovery:8761/eureka/" \
	--link infra_configservice_1:configservice \
	--link infra_discovery_1:discovery \
	--link memcache_cache1_1:cache1 \
	--link memcache_cache2_1:cache2 \
	--link redis-cluster:redis-cluster \
	--volumes-from graph-logs \
	-d -p 9595:9595 octi/graph:latest
	@echo "Started"

stop:
		@docker stop graph 2>/dev/null || echo "graph container not running"
		@docker rm graph 2>/dev/null || echo "graph container does not exist"
		@docker rm graph-logs 2>/dev/null || echo "graph-logs volume does not exist"
		@docker rmi $(docker images -q --filter "dangling=true") 2>/dev/null || echo "No dangling containers to remove"
clean:
		@docker stop graph 2>/dev/null || echo "graph container not running"
		@docker rm graph 2>/dev/null || echo "graph container does not exist"
		@docker rm graph-logs 2>/dev/null || echo "graph-logs volume does not exist"
		@docker rmi $(docker images -q --filter "dangling=true") 2>/dev/null || echo "No dangling containers to remove"
		@docker rmi octi/graph:latest 2>/dev/null || echo "local octi/graph image does not exist"

clean-data: clean
clean-images: clean
