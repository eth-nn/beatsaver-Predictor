docker-all: docker-build docker-start
	@echo "docker-all done"

docker-build:
	@echo "setting up network"
	docker network create bs-predictor-network
	@echo "network setup done"
	@echo "building the image from docker file..."
	docker build --no-cache --pull -t bs-predictor .
	@echo "image built"

docker-run:
	@echo "starting the service in container..."
	docker run -d -p 27017-27019:27017-27019 --name bs-mongodb -itd --network=bs-predictor-network mongo	
	docker run -v /home/:/home/ -p 8080:8080 -itd --network=bs-predictor-network bs-predictor

service:
	@echo "creating the service..."
	pip install --upgrade pip
	pip install -r requirements.txt

start:
	@echo "starting the service"
	python server.py

docker-stop:
	@echo "stopping the service"
	docker stop bs-predictor
	docker stop bs-mongodb
	@echo "service stopped"

docker-start:
	@echo "starting the service"
	docker start bs-predictor
	docker start bs-mongodb
	@echo "service started"

docker-remove:
	@echo "removing the image"
	docker rmi -f bs-predictor
	docker rmi -f bs-mongodb
	docker network rm bs-predictor-network
	@echo "image removed"

docker-clean: docker-stop docker-remove
	@echo "docker service stopped and image removed"

clean:
	kill $(lsof -t -i :4555)
	@echo "removing service files created"
	rm -rf $(CREATED)
