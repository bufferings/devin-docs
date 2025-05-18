---
layout: default
title: Additional Commands
parent: Commands
grand_parent: ecspresso
nav_order: 4
---

# Additional Commands

## register

The `register` command registers a new task definition without updating a service.

```shell
ecspresso register
```

### Options

| Option | Description | Default |
|--------|-------------|---------|
| `--dry-run` | Dry run (no changes to AWS) | `false` |

## deregister

The `deregister` command deregisters a task definition.

```shell
ecspresso deregister --revision=5
```

### Options

| Option | Description | Default |
|--------|-------------|---------|
| `--revision` | Revision of the task definition | - |

## run

The `run` command runs a one-time task.

```shell
ecspresso run
```

### Options

| Option | Description | Default |
|--------|-------------|---------|
| `--dry-run` | Dry run (no changes to AWS) | `false` |
| `--count` | Number of tasks to run | `1` |
| `--group` | Task group name | - |
| `--container-instances` | Container instances ARNs to place tasks | - |
| `--watch` | Watch task until it stops | `false` |
| `--wait-until` | Wait until tasks start or tasks stop | - |
| `--timeout` | Timeout duration | `10m` |
| `--launch-type` | Launch type (EC2, FARGATE) | - |
| `--network-configuration` | Network configuration JSON | - |
| `--overrides` | Task override JSON | - |
| `--propagate-tags` | Propagate tags from task definition or service | - |
| `--revision` | Revision of the task definition | - |
| `--skip-task-definition` | Skip register a new task definition | `false` |
| `--latest-task-definition` | Use the latest task definition | `false` |

## diff

The `diff` command shows differences between the local definitions and the deployed resources.

```shell
ecspresso diff
```

### Options

| Option | Description | Default |
|--------|-------------|---------|
| `--unified` | Use unified diff format | `false` |
| `--unified-context` | Context lines for unified diff | `3` |
| `--exclude-attrs` | Exclude attributes in comparison | - |

## revisions

The `revisions` command shows task definition revisions.

```shell
ecspresso revisions
```

### Options

| Option | Description | Default |
|--------|-------------|---------|
| `--max-rows` | Max rows to show | `5` |

## appspec

The `appspec` command outputs AppSpec YAML for CodeDeploy.

```shell
ecspresso appspec
```

### Options

| Option | Description | Default |
|--------|-------------|---------|
| `--task-definition` | Task definition ARN | (current) |
