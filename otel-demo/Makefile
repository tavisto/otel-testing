.PHONY: install-demo

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
