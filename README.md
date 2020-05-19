## Project Setup Details


### Instances with containers:
1. ride

		ride container

2. User

		user container

3. Orchestrator

		Rabbitmq container
		Zookeeper container
		Master container
		slave container
		orchestrator container


## Project Execution Details

Ride Instance:

	cd ~/a4
	sudo docker build -t rides:latest .
	sudo docker run \
		--name=rides_container \
		--volume=$(pwd):/usr/src/app \
		--network="host" \
		-it \
		rides:latest \
		python3 rides.py

User Instance:

	cd ~/a4
	sudo docker build -t users:latest .
	sudo docker run \
		--name=users_container \
		--volume=$(pwd):/usr/src/app \
		--network="host" \
		-it \
		users:latest \
		python3 user.py

Orchestrator Instance:

	sudo docker run \
		-d \
		--hostname my-rabbit \
		--name rabbit_container \
		-p 4369:4369 \
		-p 5671:5671 \
		-p 5672:5672 \
		-p 25672:25672 \
		rabbitmq:latest
	sudo docker run \
		-p 2181:2181 \
		zookeeper:latest
	cd ~/a4
	sudo docker build -t orch:latest ./orch
	sudo docker run \
		--name=orch_container \
		--volume=$(pwd)/orch/:/usr/src/app/ \
		-p 80:80 \
		orch:latest \
		python3 orch.py
	sudo docker build -t worker:latest ./worker
	sudo docker run \
		--name=worker_container \
		worker:latest \
		python3 worker.py
