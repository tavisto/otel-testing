.PHONY: check-context helm-setup demo stop

.DEFAULT_GOAL := demo

check-context:
	@current_context=$$(kubectl config current-context); \
	if [ "$$current_context" != "colima" ]; then \
		echo "Error: Kubernetes context is not set to 'colima'. Current context is '$$current_context'."; \
		exit 1; \
	fi

helm-setup: check-context
	@echo "Installing Otel Helm"
	helm repo add open-telemetry https://open-telemetry.github.io/opentelemetry-helm-charts
	helm repo update
	@echo "Done!"

demo: check-context helm-setup
	@echo "Installing Otel Demo"
	helm install my-otel-demo open-telemetry/opentelemetry-demo --namespace otel-demo --create-namespace
	@echo "Done!"

stop: check-context
	@echo "Uninstalling Otel Demo"
	helm uninstall my-otel-demo open-telemetry/opentelemetry-demo --namspace otel-demo
	@echo "Done!"
