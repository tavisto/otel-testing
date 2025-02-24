.PHONY: install-demo install-prometheus dump-prometheus-values

.DEFAULT_GOAL := install-demo

check-context:
	@current_context=$$(kubectl config current-context); \
	if [ "$$current_context" != "colima" ]; then \
		echo "Error: Kubernetes context is not set to 'colima'. Current context is '$$current_context'."; \
		exit 1; \
	fi

install-demo: check-context
	@echo "Installing Otel Demo"
	helm repo add open-telemetry https://open-telemetry.github.io/opentelemetry-helm-charts
	helm repo update
	helm install my-otel-demo open-telemetry/opentelemetry-demo
	@echo "Done!"

install-prometheus: check-context
	@echo "Installing Prometheus operator"
	helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
	helm repo update
	kubectl get namespace monitoring || kubectl create namespace monitoring
	helm install my-prometheus-operator prometheus-community/kube-prometheus-stack --namespace monitoring --values prometheus-values.yaml
	@echo "Done!"

update-prometheus: check-context
	@echo "Updating Prometheus operator"
	helm upgrade my-prometheus-operator prometheus-community/kube-prometheus-stack --namespace monitoring --values prometheus-values.yaml
	@echo "Done!"

dump-prometheus-values: check-context
	@echo "Dumping Prometheus operator values"
	helm show values prometheus-community/kube-prometheus-stack > prometheus-values.yaml
	@echo "Values dumped to prometheus-values.yaml"
