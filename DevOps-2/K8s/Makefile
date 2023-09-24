.PHONY: run
run:
	minikube service flask-service

.PHONY: apply-deploy
apply-deploy:
	kubectl apply -f Deploy.yaml

.PHONY: apply-service
apply-service:
	kubectl apply -f Service.yaml

.PHONY: start
start:
	minikube start

.PHONY: dashboard
dashboard:
	minikube dashboard
