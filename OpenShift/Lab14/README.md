# Lab 14: OpenShift LimitRange and CronJob Management

## Lab Scenario
As a Cluster Administrator, you are required to create a new project with the following characteristics:
- Every Pod automatically receives CPU and memory requests/limits
- Users can run jobs on a timed schedule (e.g., log rotation or backup tasks)

## Instructions

### Step 1: Create a New Project
Create a new project named "limitrange-lab" using the OpenShift CLI.

### Step 2: Define a LimitRange
Inside the "limitrange-lab" project, create a LimitRange object that applies the following constraints:

**CPU Limits:**
- Maximum: 1 core
- Minimum: 100m

**Memory Limits:**
- Maximum: 1Gi
- Minimum: 128Mi

**Default Limits (applied if not specified):**
- 500m CPU
- 512Mi Memory

**Default Requests (applied if not specified):**
- 200m CPU
- 256Mi Memory

### Step 3: Verify the LimitRange
Perform checks to ensure:
- The LimitRange object exists in the project
- The values for minimum, maximum, and defaults are correctly displayed
- All constraints are properly configured

### Step 4: Test with a Pod
- Create a simple nginx Pod without specifying any resource limits
- Verify that the default CPU and memory values (defined in Step 2) were automatically applied to the Pod
- Confirm the Pod runs successfully with the applied resource constraints

### Step 5: Create a CronJob
- Create a CronJob that runs every minute
- The job should execute a simple shell command that prints the current date and a message like "Task executed"
- Ensure the CronJob is properly configured with the correct schedule and command

### Step 6: Verify the CronJob
Perform checks to ensure:
- The CronJob object exists in the project
- It is scheduled to run every minute according to the cron syntax
- A Job is being created successfully when the scheduled time arrives
- The Job appears in the project's job list

### Step 7: View Job Logs
- Display the logs of the latest job created by the CronJob
- Confirm the job ran successfully by checking the output in the logs
- Verify that the expected message and timestamp appear in the log output

## Expected Results
![](Result.png)

## Success Criteria
- LimitRange constraints are properly applied to all Pods
- Default resource limits are automatically assigned when not specified
- CronJob creates new Jobs according to the scheduled interval
- Job logs confirm successful task execution
- All resources are properly contained within the "limitrange-lab" project

## Notes
- Ensure you have appropriate cluster administrator privileges
- Monitor resource usage to ensure limits are appropriate for your workloads
- CronJobs require careful scheduling to avoid resource conflicts
- LimitRanges help maintain cluster stability and resource fairness
