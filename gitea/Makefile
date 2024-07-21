

.PHONY:all docker_image docker_image_clean  docker_container docker_container_clean  deploy git_submodule_update pack unpack

all:deploy

git_submodule_update:
	git submodule update --init

docker_image:git_submodule_update
	if [ -f Dockerfile ]; then docker build -t gitea.hyhsystem.cn .; else true; fi

docker_image_clean:docker_container_clean
	if [ -f Dockerfile ]; then docker image rm gitea.hyhsystem.cn; else true; fi

docker_container: docker_image
	if [ -f docker-compose.yml ]; then COMPOSE_PROJECT_NAME=gitea_hyhsystem_cn  docker-compose down && COMPOSE_PROJECT_NAME=gitea_hyhsystem_cn docker-compose up -d; else true; fi;

docker_container_clean:
	if [ -f docker-compose.yml ]; then COMPOSE_PROJECT_NAME=gitea_hyhsystem_cn  docker-compose down; else true; fi;

deploy:docker_container

clean:docker_image_clean

pack:
	@if [ -f data/gitea/conf/app.ini ];\
	then echo  gitea is installed!;\
		tar -acv -f gitea_data.tar.gz data/git/repositories data/git/lfs  data/gitea/gitea.db;\
	else echo  gitea is not installed!;\
	fi;\


unpack:
	@if [ -f data/gitea/conf/app.ini ];\
	then echo  gitea is installed!;\
		if [ -f gitea_data.tar.gz ];\
		then echo data is found!;\
			COMPOSE_PROJECT_NAME=gitea_hyhsystem_cn  docker-compose down;\
			tar -xv --strip-components=1 -C data/  -f gitea_data.tar.gz;\
			COMPOSE_PROJECT_NAME=gitea_hyhsystem_cn  docker-compose up -d;\
		else echo data is not found!please download it!;\
			false;\
		fi;\
	else echo  gitea is not installed!;\
	fi;\

