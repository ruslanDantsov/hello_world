.PHONY: start-local
start-local:
	@echo "Starting memcache..."
	@cd ../memcache && make start && cd  ../platform
	@echo "Starting redis..."
	@cd ../redis && make start-local && cd ../platform
	@echo "Starting postgres..."
	@cd ../postgres && make deploy-fresh && cd ../platform
	@echo "Starting configservice and discovery service..."
	@cd ../infra && make start && cd ../platform
	@sleep 30
	@echo "Starting users service..."
	@cd ../users && make start-local && cd ../platform
	@echo "Starting octis service..."
	@cd ../octis && make start-local && cd ../platform
	@echo "Starting graph service..."
	@cd ../graph && make start-local && cd ../platform
	@echo "Starting feeds service..."
	@cd ../feeds && make start-local && cd ../platform

.PHONY: start-dev
start-dev:
	@echo "Starting memcache..."
	@cd ../memcache && make start && cd  ../platform
	@echo "Starting redis..."
	@cd ../redis && make start && cd ../platform
	@echo "Starting configservice and discovery service..."
	@cd ../infra && make start && cd ../platform
	@echo "Waiting for configservice and discovery service to initialize..."
	@sleep 30
	@echo "Starting users service..."
	@cd ../users && make start-dev && cd ../platform
	@echo "Starting octis service..."
	@cd ../octis && make start-dev && cd ../platform
	@echo "Starting graph service..."
	@cd ../graph && make start-dev && cd ../platform
	@echo "Starting feeds service..."
	@cd ../feeds && make start-dev && cd ../platform

.PHONY: stop
stop:
	@echo "Stopping feeds service..."
	@cd ../feeds && make stop && cd ../platform
	@echo "Stopping graph service..."
	@cd ../graph && make stop && cd ../platform
	@echo "Stopping octis service..."
	@cd ../octis && make stop && cd ../platform
	@echo "Stopping users service..."
	@cd ../users && make stop && cd ../platform
	@echo "Stopping configservice and discovery service..."
	@cd ../infra && make stop && cd ../platform
	@echo "Stopping postgres..."
	@cd ../postgres && make clean && cd ../platform
	@echo "Stopping redis..."
	@cd ../redis && make stop && cd ../platform
	@echo "Stopping memcache..."
	@cd ../memcache && make stop && cd ../platform
	@docker rmi $(docker images -q --filter "dangling=true") 2>/dev/null || echo "No dangling containers to remove"

.PHONY: clean
clean:
	@echo "Cleaning feeds service..."
	@cd ../feeds && make clean && cd ../platform
	@echo "Cleaning configservice and discovery service..."
	@cd ../infra && make clean && cd ../platform
	@echo "Cleaning redis..."
	@cd ../redis && make clean && cd ../platform
	@echo "Cleaning postgres..."
	@cd ../postgres && make clean && cd ../platform
	@echo "Stopping memcache..."
	@cd ../memcache && make clean && cd ../platform
	@docker rmi $(docker images -q --filter "dangling=true") 2>/dev/null || echo "No dangling containers to remove"
