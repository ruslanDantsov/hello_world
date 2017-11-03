start-local:
	@mkdir -p /tmp/log/
	@mkdir -p /tmp/log/octis/
	@docker create -v /tmp/log/octis:/tmp/log/octis --name octis-logs busybox
	@docker run --name octis \
	-e "active.profile=default" \
	-e "configservice.url=http://configservice:8888" \
	-e "discovery.url=http://discovery:8761/eureka/" \
	--link redis-cluster:redis-cluster \
	--link infra_configservice_1:configservice \
	--link infra_discovery_1:discovery \
	--link memcache_cache1_1:cache1 \
	--link memcache_cache2_1:cache2 \
	--link postgres9.5:db \
	--volumes-from octis-logs \
	-d -p 8085:8085 octi/octis:latest
	@echo "Started"

start-dev:
	@mkdir -p /tmp/log/
	@mkdir -p /tmp/log/octis/
	@docker create -v /tmp/log/octis:/tmp/log/octis --name octis-logs busybox
	@docker run --name octis \
	-e "active.profile=dev" \
	-e "configservice.url=http://configservice:8888" \
	-e "discovery.url=http://discovery:8761/eureka/" \
	--link infra_configservice_1:configservice \
	--link infra_discovery_1:discovery \
	--link memcache_cache1_1:cache1 \
	--link memcache_cache2_1:cache2 \
	--link redis-cluster:redis-cluster \
	--volumes-from octis-logs \
	-d -p 8085:8085 octi/octis:latest
	@echo "Started"

stop:
	@docker stop octis 2>/dev/null || echo "octis container not running"
	@docker rm octis 2>/dev/null || echo "octis container does not exist"
	@docker rm octis-logs 2>/dev/null || echo "octis-logs volume does not exist"
	@docker rmi $(docker images -q --filter "dangling=true") 2>/dev/null || echo "No dangling containers to remove"
clean:
	@docker stop octis 2>/dev/null || echo "octis container not running"
	@docker rm octis 2>/dev/null || echo "octis container does not exist"
	@docker rm octis-logs 2>/dev/null || echo "octis-logs volume does not exist"
	@docker rmi $(docker images -q --filter "dangling=true") 2>/dev/null || echo "No dangling containers to remove"
	@docker rmi octi/octis:latest 2>/dev/null || echo "local octi/octis image does not exist"

clean-data: clean
clean-images: clean
