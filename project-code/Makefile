docker-all: docker-build docker-start
	@echo "docker-all done"

docker-build:
	@echo "building the image from docker file..."
	docker build --no-cache --pull -t bsPredictor .
	@echo "image built - setting up network"
	docker network create bsPredictorNetwork
	@echo "network setup done"

docker-run:
	@echo "starting the service in container..."
	docker run -d -p 27017-27019:27017-27019 --name BSmongodb -itd --network=bsPredictorNetwork mongo	
	docker run -v /home/:/home/ -p 8080:8080 -itd --network=bsPredictorNetwork bsPredictor

service:
	@echo "creating the service..."
	pip install --upgrade pip
	pip install -r requirements.txt

start:
	@echo "starting the service"
	python server.py

docker-stop:
	@echo "stopping the service"
	docker stop bsPredictor
	docker stop BSmongodb
	@echo "service stopped"

docker-start:
	@echo "starting the service"
	docker start bsPredictor
	docker start BSmongodb
	@echo "service started"

docker-remove:
	@echo "removing the image"
	docker rmi -f bsPredictor
	docker rmi -f BSmongodb
	docker network rm bsPredictorNetwork
	@echo "image removed"

docker-clean: docker-stop docker-remove
	@echo "docker service stopped and image removed"

clean:
	kill $(lsof -t -i :4555)
	@echo "removing service files created"
	rm -rf $(CREATED)
