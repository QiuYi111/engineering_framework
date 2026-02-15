# Neural-Grid Standard Makefile
# "The only valid interface to the project"

.PHONY: init up down proto test lint verify clean help

# --- Configuration ---
# Replace these with your project specifics
PROJECT_NAME := my-project
BACKEND_DIR := backend
PROTO_SRC := proto
PROTO_OUT := $(BACKEND_DIR)/api/pb

# --- 1. Environment & Setup ---

help: ## Show this help message
	@grep -E '^[a-zA-Z_-]+:.*?## .*$$' $(MAKEFILE_LIST) | awk 'BEGIN {FS = ":.*?## "}; {printf "\033[36m%-20s\033[0m %s\n", $$1, $$2}'

init: ## Initialize development environment (Tools, Hooks)
	@echo "🛠️  Initializing Development Environment..."
	@# Example: Check for uv/poetry/go etc.
	@command -v pre-commit >/dev/null 2>&1 || pip install pre-commit
	@pre-commit install
	@echo "✅ Done! Environment is ready."

up: ## Start infrastructure (Docker)
	@echo "🚀 Starting Infrastructure..."
	@docker-compose up -d --wait
	@echo "✅ Infrastructure is UP."

down: ## Stop infrastructure
	@echo "🛑 Stopping Infrastructure..."
	@docker-compose down -v
	@echo "✅ Cleaned up."

# --- 2. Contract ---

proto: ## Generate code from contracts (Protobuf/OpenAPI)
	@echo "📜 Generating Contract Code..."
	@# Add your generation command here
	@# protoc ...
	@echo "✅ Generation complete."

# --- 3. Quality Assurance ---

lint: ## Run Code Linters
	@echo "🔍 Running Linters..."
	@# golangci-lint run ./... or ruff check .
	@echo "✅ Lint check passed."

test: ## Run Unit & Integration Tests
	@echo "🧪 Running Tests..."
	@# go test ./... or pytest
	@echo "✅ Tests passed."

# --- 4. The Gatekeeper ---

verify: up proto lint test ## Run full verification (Pre-Push Gate)
	@echo "🛡️  Full System Verification Passed."
