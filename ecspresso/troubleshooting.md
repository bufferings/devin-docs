---
layout: default
title: Troubleshooting
parent: ecspresso
nav_order: 5
---

# Troubleshooting

This guide provides solutions to common issues encountered when using ecspresso.

## Deployment Failures

### Service Deployment Stuck

**Symptom**: The `ecspresso deploy` command is stuck waiting for the service to stabilize.

**Solutions**:
- Check the ECS service events:
  ```shell
  ecspresso status --events=20
  ```
- Verify that your container is healthy and passing health checks
- Check if there are resource constraints (CPU, memory) in your ECS cluster
- Verify that your container can start properly by checking the logs:
  ```shell
  aws logs get-log-events --log-group-name your-log-group --log-stream-name your-log-stream
  ```

### Task Definition Registration Failure

**Symptom**: Error when registering a new task definition.

**Solutions**:
- Verify your task definition JSON is valid:
  ```shell
  ecspresso verify --verify-task-definition
  ```
- Check IAM permissions for the task execution role
- Ensure container image exists in the repository

## Permission Issues

**Symptom**: Access denied errors when running ecspresso commands.

**Solutions**:
- Verify that your AWS credentials have the necessary permissions
- Required permissions include:
  - ecs:DescribeServices
  - ecs:UpdateService
  - ecs:RegisterTaskDefinition
  - ecs:DescribeTasks
  - ecs:RunTask
  - ecs:StopTask
  - ecs:ExecuteCommand (for exec)
  - application-autoscaling:* (for auto scaling operations)
  - codedeploy:* (for CodeDeploy operations)

## Configuration Issues

**Symptom**: ecspresso cannot find or load configuration files.

**Solutions**:
- Ensure ecspresso.yml exists in the current directory or specify with --config
- Verify that task_definition and service_definition paths are correct
- Check for JSON syntax errors in your definition files

## Common Error Messages

### "failed to describe service: ServiceNotFoundException"

**Cause**: The specified service does not exist in the cluster.

**Solutions**:
- Verify the service name and cluster name in ecspresso.yml
- If creating a new service, use `ecspresso deploy` which will create it

### "failed to register task definition: InvalidParameterException"

**Cause**: The task definition contains invalid parameters.

**Solutions**:
- Check the task definition for invalid values
- Verify that all required parameters are provided
- Ensure container image exists and is accessible
