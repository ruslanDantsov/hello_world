start-local:
	@docker run --name redis-cluster -p 6379:6379 -d redis redis-server
start-dev:

stop:
	@docker stop redis-cluster  2>/dev/null || echo "redis-cluster not running"
	@docker rm redis-cluster 2>/dev/null || echo "redis-cluster container does not exist"

clean:
	@docker stop redis-cluster  2>/dev/null || echo "redis-cluster not running"
	@docker rm redis-cluster 2>/dev/null || echo "redis-cluster container does not exist"

