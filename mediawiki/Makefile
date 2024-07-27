

.PHONY:all docker_image docker_image_clean  docker_container docker_container_clean  deploy git_submodule_update pack unpack

all:deploy

git_submodule_update:
	git submodule update --init

docker_image:git_submodule_update
	if [ -f Dockerfile ]; then docker build -t mediawiki.hyhsystem.cn .; else true; fi

docker_image_clean:docker_container_clean
	if [ -f Dockerfile ]; then docker image rm mediawiki.hyhsystem.cn; else true; fi

docker_container: docker_image
	if [ -f docker-compose.yml ]; then COMPOSE_PROJECT_NAME=mediawiki_hyhsystem_cn  docker-compose down && COMPOSE_PROJECT_NAME=mediawiki_hyhsystem_cn docker-compose up -d; else true; fi;
	if [ -f html/LocalSettings.php ]; then docker cp html/LocalSettings.php mediawiki.hyhsystem.cn:/var/www/html/; else true; fi
	if [ -f html/.htaccess ]; then docker cp html/.htaccess mediawiki.hyhsystem.cn:/var/www/html/; else true; fi

docker_container_clean:
	if [ -f docker-compose.yml ]; then COMPOSE_PROJECT_NAME=mediawiki_hyhsystem_cn  docker-compose down; else true; fi;

deploy:docker_container

clean:docker_image_clean

pack:
	@if [ -f html/LocalSettings.php ];\
	then echo  mediawiki is installed!;\
		tar -acv -f mediawiki_data.tar.gz html/;\
	else echo  mediawiki is not installed!;\
	fi;\


unpack:
	@if [ -f docker-compose.yml ];\
	then echo  "mediawiki is installed(ignored)!";\
		if [ -f mediawiki_data.tar.gz ];\
		then echo data is found!;\
			${MAKE} clean;\
			tar -xv --strip-components=1 -C html/  -f mediawiki_data.tar.gz;\
			${MAKE};\
		else echo data is not found!please download it!;\
			false;\
		fi;\
	else echo  mediawiki is not installed!;\
	fi;\

