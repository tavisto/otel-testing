.PHONY: install-elastic-operator build-cluster delete-cluster check-context

.DEFAULT_GOAL := build-cluster

check-context:
	@current_context=$$(kubectl config current-context); \
	if [ "$$current_context" != "colima" ]; then \
		echo "Error: Kubernetes context is not set to 'colima'. Current context is '$$current_context'."; \
		exit 1; \
	fi

install-elastic-operator: check-context
	@echo "Installing Elastic Operator"
	helm repo add elastic https://helm.elastic.co
	helm repo update
	helm install elastic-operator elastic/eck-operator --namespace elastic-system --create-namespace
	@echo "Done!"

build-cluster: check-context
	@echo "Building Elastic Cluster"
	kubectl apply -f elastic-cluster.yaml
	@echo "Done!"

delete-cluster: check-context
	@echo "Deleting Elastic Cluster"
	kubectl delete -f elastic-cluster.yaml
	@echo "Done!"

run-kibana: check-context
	@echo "Running Kibana"
	kubectl apply -f kibana.yaml
	@echo "Done!"

delete-kibana: check-context
	@echo "Deleting Kibana"
	kubectl delete -f kibana.yaml
	@echo "Done!"

stop: check-context delete-kibana delete-kibana
	@echo "Stopping Elastic Operator"
	helm uninstall elastic-operator --namespace elastic-system
	@echo "Done!"
