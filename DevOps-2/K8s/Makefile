.PHONY: run
run:
	# Start service from k8s cluster
	minikube service flask-service

.PHONY: apply-deploy
apply-deploy:
	# apply deploy file
	kubectl apply -f Deploy.yaml

.PHONY: apply-service
apply-service:
	# apply service file
	kubectl apply -f Service.yaml

.PHONY: start
start:
	# start minikube
	minikube start

.PHONY: dashboard
dashboard:
	# start k8s dashboard
	minikube dashboard
