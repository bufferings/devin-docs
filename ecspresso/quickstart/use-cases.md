---
layout: default
title: Common Use Cases
parent: Quickstart
grand_parent: ecspresso
nav_order: 2
---

# Common Use Cases

## Deployment Strategies

### Standard ECS Rolling Update

```shell
ecspresso deploy
```

This is the default deployment method that updates tasks in a rolling fashion, ensuring service availability during deployment.

### Blue/Green Deployment with CodeDeploy

To use blue/green deployment:

1. Configure CodeDeploy in your ecspresso.yml:
   ```yaml
   region: us-west-2
   cluster: my-cluster
   service: my-service
   task_definition: ecs-task-def.json
   service_definition: ecs-service-def.json
   code_deploy:
     application_name: AppECS-my-cluster-my-service
     deployment_group_name: DgpECS-my-cluster-my-service
     deployment_config_name: CodeDeployDefault.ECSAllAtOnce
   ```

2. Deploy with automatic rollback on failure:
   ```shell
   ecspresso deploy --rollback-events DEPLOYMENT_FAILURE
   ```

### Forcing a New Deployment

To redeploy the current task definition without changes:

```shell
ecspresso deploy --force-new-deployment --skip-task-definition
```

## Running One-off Tasks

Execute a one-time task using the current task definition:

```shell
ecspresso run --watch-container=web
```

Run a task with overrides:

```shell
ecspresso run --overrides='{
  "containerOverrides": [
    {
      "name": "web",
      "command": ["/bin/sh", "-c", "echo hello"]
    }
  ]
}'
```

## Executing Commands in Running Tasks

```shell
ecspresso exec --command "ls -la" --container web
```

## Multi-environment Management

Use environment variables for different environments:

```shell
# Dev environment
ecspresso --config ecspresso.yml --envfile dev.env deploy

# Production environment
ecspresso --config ecspresso.yml --envfile prod.env deploy
```
