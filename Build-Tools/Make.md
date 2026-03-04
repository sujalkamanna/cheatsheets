# Make Cheatsheet

![Make Logo](https://www.gnu.org/software/make/manual/make.html)

---

## Table of Contents
1. [What is Make?](#1-what-is-make)
2. [Core Concepts](#2-core-concepts)
3. [Installation](#3-installation)
4. [Makefile Basics](#4-makefile-basics)
5. [Rules and Targets](#5-rules-and-targets)
6. [Variables](#6-variables)
7. [Functions](#7-functions)
8. [Advanced Features](#8-advanced-features)
9. [Common Patterns](#9-common-patterns)
10. [Best Practices](#10-best-practices)
11. [Additional Resources](#11-additional-resources)

---

## 1. What is Make?

Make is a build automation tool that automatically builds executable programs and libraries from source code by reading specifications from a Makefile. It tracks file dependencies and only rebuilds what's necessary, making builds efficient.

**Key Benefits:**
- Simple and lightweight
- Language-agnostic
- Efficient incremental builds
- Dependency tracking
- Cross-platform support (Unix, Linux, Windows, macOS)
- No installation overhead
- Perfect for DevOps scripts

---

## 2. Core Concepts

| Concept | Description |
|---------|-------------|
| **Target** | Output file or action to build |
| **Prerequisite** | Dependencies needed before building |
| **Recipe** | Commands to execute for target |
| **Rule** | Target + prerequisites + recipe |
| **Variable** | Named value used in recipes |
| **Phony Target** | Not a real file, just a label |
| **Pattern Rule** | Generic rule for similar targets |
| **Default Goal** | First target executed |

---

## 3. Installation

### macOS Installation

```bash
# Already installed on macOS
make --version

# Update via Homebrew
brew install make

# GNU Make (if needed)
brew install gnu-make
```

### Linux Installation

```bash
# Ubuntu/Debian
sudo apt-get update
sudo apt-get install make

# CentOS/RHEL
sudo yum install make

# Verify
make --version
```

### Windows Installation

```bash
# Using Chocolatey
choco install make

# Using MinGW
# Download from http://www.mingw.org/
# Add to PATH

# Using WSL (Windows Subsystem for Linux)
wsl apt-get install make

# Verify
make --version
```

---

## 4. Makefile Basics

### Simple Makefile

```makefile
# Basic target
hello:
	echo "Hello World"

# Target with prerequisites
output.txt: input.txt
	cat input.txt > output.txt

# Phony target (not a real file)
.PHONY: clean
clean:
	rm -f output.txt
```

### Running Make

```bash
# Run default target (first one)
make

# Run specific target
make hello

# Run multiple targets
make clean build

# Force rebuild
make -B

# Show what would execute (dry run)
make -n

# Execute with verbosity
make -d

# Use specific Makefile
make -f Makefile.dev

# Set variable
make VERSION=1.0.0
```

### Basic Structure

```makefile
# Comments with #

# Variables (define at top)
NAME = myapp
VERSION = 1.0.0

# Rules/Targets
target: prerequisite1 prerequisite2
	command1
	command2

# Default target (runs when no target specified)
.DEFAULT_GOAL := build
```

---

## 5. Rules and Targets

### Basic Target

```makefile
# Simple target
build:
	echo "Building application"

# Target with prerequisites
app: main.o utils.o
	gcc -o app main.o utils.o

main.o: main.c
	gcc -c main.c

utils.o: utils.c
	gcc -c utils.c
```

### Phony Targets

```makefile
# Phony targets (not real files)
.PHONY: build test deploy clean

build:
	echo "Building..."

test:
	echo "Testing..."

deploy:
	echo "Deploying..."

clean:
	rm -rf build/
	rm -f *.o
```

### Target with Multiple Prerequisites

```makefile
# Multiple prerequisites
deploy: build test lint security-scan
	echo "Deploying application"

build:
	echo "Building..."

test:
	echo "Testing..."

lint:
	echo "Linting..."

security-scan:
	echo "Security scanning..."
```

### Target with Comments

```makefile
# Build target with description
.PHONY: build
build: ## Build application
	echo "Building..."

# List all targets with descriptions
help:
	@grep -E '^[a-zA-Z_-]+:.*?## ' $(MAKEFILE_LIST) | \
	  awk 'BEGIN {FS = ":.*?## "}; {printf "%-20s %s\n", $$1, $$2}'
```

---

## 6. Variables

### Define and Use Variables

```makefile
# Variable definition
NAME = myapp
VERSION = 1.0.0
SRC_DIR = src
BUILD_DIR = build
CC = gcc
CFLAGS = -Wall -O2

# Simple variable
output.txt: input.txt
	cp input.txt $(output)

# Reference variables
build:
	@echo "Building $(NAME) v$(VERSION)"
	$(CC) $(CFLAGS) -o $(BUILD_DIR)/$(NAME) src/*.c

# String substitution
OBJECTS = main.o utils.o
SOURCES = $(OBJECTS:.o=.c)
# SOURCES = main.c utils.c
```

### Variable Types

```makefile
# Simply expanded (evaluated once)
SIMPLE := value
X := $(SIMPLE)

# Recursively expanded (re-evaluated each use)
RECURSIVE = value
Y = $(RECURSIVE)

# Command substitution
DATE = $(shell date +%Y-%m-%d)
UNAME = $(shell uname -s)

# Conditional assignment (if not already set)
VAR ?= default_value

# Append
VAR += more_value
```

### Automatic Variables

```makefile
# $@ = target name
# $< = first prerequisite
# $^ = all prerequisites
# $* = stem of target

build: main.o utils.o
	gcc -o build/app $^  # All prerequisites

%.o: %.c
	gcc -c $< -o $@     # First prerequisite -> target

clean:
	rm -f build/$(TARGET)  # $(@D) = directory, $(@F) = filename
```

---

## 7. Functions

### Text Functions

```makefile
# patsubst (pattern substitution)
SOURCES = main.c utils.c
OBJECTS = $(patsubst %.c,%.o,$(SOURCES))

# subst (simple substitution)
PATHS = src/main.c src/utils.c
NAMES = $(subst src/,,$(PATHS))

# filter (filter words)
C_FILES = $(filter %.c,$(FILES))

# filter-out (exclude words)
OTHER_FILES = $(filter-out %.c,$(FILES))

# sort (sort words)
SORTED = $(sort main.o utils.o array.o)

# wildcard (file globbing)
SOURCES = $(wildcard src/*.c)
```

### Call Custom Function

```makefile
# Define function
define print_message
	@echo "Building $(1) version $(2)"
endef

# Use function
build:
	$(call print_message,myapp,1.0.0)
```

### Shell Functions

```makefile
# Execute shell command
DATE = $(shell date +%Y-%m-%d)
COMMIT = $(shell git rev-parse --short HEAD)
VERSION = $(shell cat version.txt)

info:
	@echo "Built on $(DATE)"
	@echo "Commit: $(COMMIT)"
	@echo "Version: $(VERSION)"
```

---

## 8. Advanced Features

### Conditional Statements

```makefile
# ifdef - check if variable is defined
ifdef DEBUG
	CFLAGS = -g -O0
else
	CFLAGS = -O2
endif

# ifeq - string comparison
ifeq ($(OS),linux)
	PLATFORM = linux
else ifeq ($(OS),darwin)
	PLATFORM = macos
else
	PLATFORM = windows
endif

# ifneq - not equal
ifneq ($(VERSION),)
	VERSION_FLAG = -DVERSION=$(VERSION)
endif
```

### Include Other Makefiles

```makefile
# Include another Makefile
include common.mk

# Conditional include
-include config.mk  # Don't fail if not found

# Include with directory
include $(COMMON_DIR)/rules.mk
```

### Pattern Rules

```makefile
# Pattern rule (% = any string)
%.o: %.c
	gcc -c $< -o $@

# Multiple pattern rules
%.pdf: %.tex
	pdflatex $<

%.html: %.md
	markdown $< > $@

# Double colon rule (multiple definitions allowed)
all::
	@echo "Rule 1"

all::
	@echo "Rule 2"
```

### Order-Only Prerequisites

```makefile
# Normal prerequisites (rebuild if change)
build/app: src/main.c | build/
	gcc -o build/app src/main.c

# Order-only prerequisites (don't rebuild if change)
build/:
	mkdir -p build/
```

---

## 9. Common Patterns

### Complete Build System

```makefile
# Variables
PROJECT = myapp
VERSION = 1.0.0
SRC_DIR = src
BUILD_DIR = build
DIST_DIR = dist

CC = gcc
CFLAGS = -Wall -O2 -I$(SRC_DIR)
LDFLAGS = -lm

SOURCES = $(wildcard $(SRC_DIR)/*.c)
OBJECTS = $(patsubst $(SRC_DIR)/%.c,$(BUILD_DIR)/%.o,$(SOURCES))
TARGET = $(BUILD_DIR)/$(PROJECT)

# Default target
.DEFAULT_GOAL := build

# Phony targets
.PHONY: build clean test deploy help

# Targets
build: $(TARGET)

$(TARGET): $(OBJECTS)
	$(CC) -o $@ $^ $(LDFLAGS)

$(BUILD_DIR)/%.o: $(SRC_DIR)/%.c | $(BUILD_DIR)
	$(CC) $(CFLAGS) -c $< -o $@

$(BUILD_DIR):
	mkdir -p $(BUILD_DIR)

test: build
	./$(TARGET) --test

deploy: clean build test
	mkdir -p $(DIST_DIR)
	cp $(TARGET) $(DIST_DIR)/
	tar -czf $(DIST_DIR)/$(PROJECT)-$(VERSION).tar.gz $(DIST_DIR)/

clean:
	rm -rf $(BUILD_DIR) $(DIST_DIR)

help:
	@echo "$(PROJECT) v$(VERSION)"
	@echo "Targets: build, test, deploy, clean"
```

### Development Tasks

```makefile
.PHONY: dev build lint test coverage format

# Development with watch
dev:
	while true; do \
		clear; \
		make build && make test; \
		inotifywait -r -e modify src/; \
	done

# Build
build:
	@echo "Building..."
	gcc -o app src/*.c

# Lint
lint:
	@echo "Linting..."
	splint src/*.c

# Test
test:
	@echo "Running tests..."
	./test_runner

# Code coverage
coverage: test
	@echo "Generating coverage..."
	gcov src/*.c

# Format code
format:
	@echo "Formatting code..."
	clang-format -i src/*.c
```

### Docker Targets

```makefile
.PHONY: docker-build docker-run docker-push

IMAGE_NAME = myapp
IMAGE_TAG = latest
REGISTRY = docker.io

docker-build:
	docker build -t $(REGISTRY)/$(IMAGE_NAME):$(IMAGE_TAG) .

docker-run: docker-build
	docker run -it $(REGISTRY)/$(IMAGE_NAME):$(IMAGE_TAG)

docker-push: docker-build
	docker push $(REGISTRY)/$(IMAGE_NAME):$(IMAGE_TAG)
```

### Deployment Targets

```makefile
.PHONY: deploy deploy-staging deploy-production

DEPLOY_HOST = prod.example.com
DEPLOY_USER = deploy
DEPLOY_PATH = /opt/myapp

deploy: build test
	@echo "Deploying..."

deploy-staging: build test
	ssh $(DEPLOY_USER)@staging.example.com \
		'cd $(DEPLOY_PATH) && git pull && make build'

deploy-production: build test
	ssh $(DEPLOY_USER)@$(DEPLOY_HOST) \
		'cd $(DEPLOY_PATH) && git pull && make build && systemctl restart myapp'
```

---

## 10. Best Practices

### Well-Organized Makefile

```makefile
# Header
.PHONY: help
help: ## Show this help message
	@grep -E '^[a-zA-Z_-]+:.*?## ' $(MAKEFILE_LIST) | \
	  awk 'BEGIN {FS = ":.*?## "}; {printf "  %-20s %s\n", $$1, $$2}'

# Variables section
PROJECT = myapp
VERSION = 1.0.0
BUILD_DIR = build
SRC_DIR = src

# Rules section
.PHONY: build test deploy clean

build: ## Build project
	@echo "Building $(PROJECT)..."
	@mkdir -p $(BUILD_DIR)
	@gcc -o $(BUILD_DIR)/$(PROJECT) $(SRC_DIR)/*.c

test: ## Run tests
	@echo "Testing..."
	@./$(BUILD_DIR)/$(PROJECT) --test

deploy: build test ## Deploy application
	@echo "Deploying..."

clean: ## Clean build artifacts
	@echo "Cleaning..."
	@rm -rf $(BUILD_DIR)
```

### Error Handling

```makefile
# Continue on error
-gcc main.c

# Return specific error code
check:
	@command || exit 1

# Error checking
build:
	@if [ ! -f src/main.c ]; then \
		echo "Error: src/main.c not found"; \
		exit 1; \
	fi
	gcc -o app src/main.c

# Prevent recipes from being echoed
silent:
	@echo "Silent output only"
```

### Best Practices

```makefile
# Use .PHONY for non-file targets
.PHONY: clean test deploy

# Use meaningful variable names
SOURCE_DIRECTORY = src
BUILD_DIRECTORY = build

# Avoid hard tabs (required for recipes)
# Use @echo to suppress command echo
# Use $@ for target, $< for first prerequisite, $^ for all

# Version targets
version:
	@echo "Version: $(VERSION)"

# Quiet mode
QUIET = @

$(QUIET)echo "This won't echo the command"

# Defensive defaults
export PATH := /usr/local/bin:$(PATH)
SHELL := /bin/bash
.SHELLFLAGS := -eu -o pipefail -c
```

---

## 11. Additional Resources

### Official Documentation

| Resource | URL |
|----------|-----|
| **GNU Make Manual** | https://www.gnu.org/software/make/manual/ |
| **Quick Reference** | https://www.gnu.org/software/make/manual/make.html#Quick-Reference |
| **Variables** | https://www.gnu.org/software/make/manual/make.html#Using-Variables |
| **Rules** | https://www.gnu.org/software/make/manual/make.html#Rules |
| **Functions** | https://www.gnu.org/software/make/manual/make.html#Functions |

### Community and Support

| Resource | URL |
|----------|-----|
| **GitHub Issues** | https://savannah.gnu.org/bugs/?group=make |
| **Stack Overflow** | https://stackoverflow.com/questions/tagged/makefile |
| **GNU Make FAQ** | https://www.gnu.org/software/make/manual/make.html#FAQ |

### Learning Resources

| Resource | URL |
|----------|-----|
| **Make Tutorial** | https://www.gnu.org/software/make/manual/make.html#Introduction |
| **Example Makefiles** | https://www.cs.colby.edu/maxwell/courses/tutorials/maketutor/ |
| **Advanced Patterns** | https://bost.ocks.org/mike/make/ |
| **PMake Documentation** | https://www.freebsd.org/cgi/man.cgi?make |

---

## Quick Reference

```bash
# Run Make
make                 # Run default target
make target         # Run specific target
make target1 target2 # Run multiple targets

# Options
make -n             # Dry run (show commands)
make -B             # Force rebuild
make -j4            # Parallel jobs (4)
make -f Makefile.dev # Use different Makefile
make VAR=value      # Set variable

# Clean and rebuild
make clean
make build

# Verbose output
make -d             # Debug
make -v             # Version

# List targets
make --print-data-base | grep '^[^.#]' | grep -v '^recipes'
```

---

## Common Makefile Examples

```makefile
# Node.js project
.PHONY: install build test deploy clean

install:
	npm install

build: install
	npm run build

test: build
	npm test

deploy: test
	npm run deploy

clean:
	rm -rf node_modules build/

# Python project
.PHONY: install test lint format deploy

install:
	pip install -r requirements.txt

test:
	pytest tests/

lint:
	pylint src/

format:
	black src/

deploy: test lint
	python setup.py sdist upload

# Go project
.PHONY: build test deploy clean

build:
	go build -o app main.go

test:
	go test ./...

deploy: build test
	./scripts/deploy.sh

clean:
	go clean
	rm -f app
```

---

**Last Updated:** 2026
**Make Version:** 4.x+
**License:** GNU General Public License

For latest information, visit [GNU Make Manual](https://www.gnu.org/software/make/manual/)