ts := $(shell /bin/date "+%Y%m%d%H%M")

all:
		@docker exec -t postgres9.5 sh -c "pg_dumpall -c -Uocti" | gzip -c > ./postgres-$(ts).sql.gz

deploy-fresh:
		@docker create -v /var/lib/postgresql/data --name postgres9.5-data busybox
		@docker run --name postgres9.5 -p 5432:5432 -e POSTGRES_DB=octi -e POSTGRES_USER=octi -e POSTGRES_PASSWORD=octi -d --volumes-from postgres9.5-data octi/postgres

deploy:
		@docker run --name postgres9.5 -p 5432:5432 -e POSTGRES_DB=octi -e POSTGRES_USER=octi -e POSTGRES_PASSWORD=octi -d --volumes-from postgres9.5-data octi/postgres

stop:
		@docker stop postgres9.5 2>/dev/null || echo "postgres9.5 container not running"
		@docker run --volumes-from postgres9.5-data -v /tmp:/backup busybox tar -zcvf /backup/postgres-$(ts).tar.gz /var/lib/postgresql/data >/dev/null || echo "backup failed."
		@docker rm postgres9.5 2>/dev/null || echo "postgres9.5 container doesnot exist"

clean:
		@docker stop postgres9.5 2>/dev/null || echo "postgres9.5 container not running"
		@docker run --volumes-from postgres9.5-data -v /tmp:/backup busybox tar -zcvf /backup/postgres-$(ts).tar.gz /var/lib/postgresql/data >/dev/null || echo "backup failed."
		@docker rm postgres9.5 2>/dev/null || echo "postgres9.5 container doesnot exist"
		@docker rm postgres9.5-data 2>/dev/null || echo "postgres9.5-data container doesnot exist"
		@docker rmi octi/postgres 2>/dev/null || echo "local octi/postgres image does not exist"

shell:
		@docker run --rm -it --link postgres9.5:postgres postgres /bin/bash
