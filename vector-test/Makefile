.PHONY: check-context helm-repo aggregator agent stop

.DEFAULT_GOAL := agent

check-context:
	@current_context=$$(kubectl config current-context); \
	if [ "$$current_context" != "colima" ]; then \
		echo "Error: Kubernetes context is not set to 'colima'. Current context is '$$current_context'."; \
		exit 1; \
	fi

helm-repo: check-context
	@echo "Installing Vector"
	helm repo add vector https://helm.vector.dev
	helm repo update
	@echo "Done!"

aggregator: check-context
	@echo "Installing Vector"
	helm upgrade --install vector-agg vector/vector --create-namespace --namespace vector --values aggregator-values.yaml
	@echo "Done!"

agent: check-context
	@echo "Installing Vector"
	helm upgrade --install vector-agent vector/vector --create-namespace --namespace vector --values agent-values.yaml
	@echo "Done!"

stop: check-context
	@echo "Stopping Vector"
	helm uninstall vector-agent --namespace vector
	helm uninstall vector-agg --namespace vector
	@echo "Done!"
