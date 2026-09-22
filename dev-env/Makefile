COMPOSE   := docker compose
SERVICE   := jupyter
URL       := http://localhost:8888

.DEFAULT_GOAL := help

.PHONY: help build up down restart logs shell status open versions clean

help:  ## Show this help
	@grep -E '^[a-z-]+:.*?## ' $(MAKEFILE_LIST) \
	  | awk -F':.*?## ' '{printf "  make %-9s %s\n", $$1, $$2}'

build:  ## Build the image (first run, or after editing the Dockerfile)
	$(COMPOSE) build

up:  ## Start Jupyter in the background
	$(COMPOSE) up -d
	@echo "Jupyter is starting. Open $(URL)"

down:  ## Stop and remove the container
	$(COMPOSE) down

restart:  ## Restart the container
	$(COMPOSE) restart

logs:  ## Follow the container log (Ctrl-C to stop watching)
	$(COMPOSE) logs -f $(SERVICE)

shell:  ## Open a shell inside the running container
	$(COMPOSE) exec $(SERVICE) bash

status:  ## Show whether the container is running
	$(COMPOSE) ps

open:  ## Open Jupyter in your browser
	@open $(URL) 2>/dev/null || xdg-open $(URL) 2>/dev/null || echo "Open $(URL)"

versions:  ## Print the library versions inside the image
	$(COMPOSE) run --rm --no-deps $(SERVICE) python -c \
	  "import sys, numpy, pandas, matplotlib, seaborn, scipy, sklearn; \
	   print('python      ', sys.version.split()[0]); \
	   print('numpy       ', numpy.__version__); \
	   print('pandas      ', pandas.__version__); \
	   print('matplotlib  ', matplotlib.__version__); \
	   print('seaborn     ', seaborn.__version__); \
	   print('scipy       ', scipy.__version__); \
	   print('scikit-learn', sklearn.__version__)"

clean:  ## Remove the container and the image
	$(COMPOSE) down --rmi local --volumes --remove-orphans
