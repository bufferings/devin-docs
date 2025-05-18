---
layout: default
title: Monitoring Commands
parent: Commands
grand_parent: ecspresso
nav_order: 3
---

# Monitoring Commands

## status

The `status` command displays the current status of an ECS service.

```shell
ecspresso status
```

### Output Example

```
Service: my-service
Cluster: my-cluster
TaskDefinition: my-task:123
Deployments:
  PRIMARY my-task:123 desired:2 pending:0 running:2
Events:
  2023-11-01T12:00:00+09:00 (service my-service) has reached a steady state.
```

### Options

| Option | Description | Default |
|--------|-------------|---------|
| `--events` | Number of events to show | `10` |

## wait

The `wait` command waits until the service reaches a stable state.

```shell
ecspresso wait
```

### Options

| Option | Description | Default |
|--------|-------------|---------|
| `--timeout` | Timeout duration | `10m` |
| `--wait-until` | Wait until service stable or deployed | `stable` |

## exec

The `exec` command executes a command inside a running task.

```shell
ecspresso exec --command "ls -la" --container web
```

### Options

| Option | Description | Default |
|--------|-------------|---------|
| `--command` | Command to execute | - |
| `--container` | Container name | - |
| `--timeout` | Timeout duration | `10m` |
| `--task-id` | Specific task ID | (auto-select) |
| `--interactive` | Enable interactive mode | `false` |
| `--filter` | Filter tasks using a custom command | - |

## tasks

The `tasks` command lists tasks in a service or having the same task definition family.

```shell
ecspresso tasks
```

### Options

| Option | Description | Default |
|--------|-------------|---------|
| `--status` | Filter tasks by status | `RUNNING` |
| `--id-only` | Show only task IDs | `false` |
| `--output` | Output format (table, json, tsv) | `table` |
