---
layout: default
title: Configuration Commands
parent: Commands
grand_parent: ecspresso
nav_order: 1
---

# Configuration Commands

## init

The `init` command creates configuration files from an existing ECS service.

```shell
ecspresso init --region us-west-2 --cluster my-cluster --service my-service
```

### Options

| Option | Description | Default |
|--------|-------------|---------|
| `--region` | AWS region | (from AWS config) |
| `--cluster` | ECS cluster name | - |
| `--service` | ECS service name | - |
| `--config` | Config file path | `ecspresso.yml` |
| `--task-definition-path` | Output path for task definition | `ecs-task-def.json` |
| `--service-definition-path` | Output path for service definition | `ecs-service-def.json` |

## verify

The `verify` command validates the configuration and definition files against the ECS API, without making any changes.

```shell
ecspresso verify
```

### Options

| Option | Description | Default |
|--------|-------------|---------|
| `--config` | Config file path | `ecspresso.yml` |
| `--verify-task-definition` | Verify task definition | `true` |
| `--verify-service-definition` | Verify service definition | `true` |

## render

The `render` command outputs the rendered configuration, task definition, or service definition to STDOUT.

```shell
ecspresso render --mode=value
```

### Options

| Option | Description | Default |
|--------|-------------|---------|
| `--config` | Config file path | `ecspresso.yml` |
| `--mode` | Render mode (config, task, service) | - |
