# Lab 14: OpenShift LimitRange and CronJob Management

## Overview
[cite_start]This lab is part of the DevOps Engineer Diploma[cite: 16]. [cite_start]The objective is to practice creating and managing OpenShift projects, defining LimitRanges for resource management, automatically applying CPU/Memory limits to Pods, and managing CronJobs[cite: 25, 27, 28].

## Lab Scenario
As a Cluster Administrator, you are required to create a new project with the following characteristics:
* [cite_start]Every Pod automatically receives CPU and memory requests/limits[cite: 36].
* [cite_start]Users can run jobs on a timed schedule (e.g., log rotation or backup tasks)[cite: 37].

## Instructions

### Step 1: Create a New Project
* [cite_start]Create a new project named `limitrange-lab`[cite: 39].

### Step 2: Define a LimitRange
Inside the `limitrange-lab` project, create a LimitRange object that applies the following constraints:
* **CPU Limits:**
  * [cite_start]Maximum: 1 core [cite: 42]
  * [cite_start]Minimum: 100m [cite: 42]
* **Memory Limits:**
  * [cite_start]Maximum: 1Gi [cite: 43]
  * [cite_start]Minimum: 128Mi [cite: 43]
* **Default Limits (applied if not specified):**
  * [cite_start]500m CPU [cite: 43]
  * [cite_start]512Mi Memory [cite: 43]
* **Default Requests (applied if not specified):**
  * [cite_start]200m CPU [cite: 44]
  * [cite_start]256Mi Memory [cite: 44]

### Step 3: Verify the LimitRange
Perform checks to ensure:
* [cite_start]The LimitRange object exists[cite: 48].
* [cite_start]The values for minimum, maximum, and defaults are correctly displayed[cite: 49].

### Step 4: Test with a Pod
* [cite_start]Create a simple `nginx` Pod without specifying any resource limits[cite: 51].
* [cite_start]Verify that the default CPU and memory values (defined in Step 2) were automatically applied to the Pod[cite: 52].

### Step 5: Create a CronJob
* [cite_start]Create a CronJob that runs every minute[cite: 55].
* The job should execute a simple shell command. [cite_start]Example: Print the current date and a message like "Task executed"[cite: 56, 57].

### Step 6: Verify the CronJob
Perform checks to ensure:
* [cite_start]The CronJob object exists[cite: 64].
* [cite_start]It is scheduled to run every minute[cite: 65].
* [cite_start]A Job is being created successfully[cite: 66].

### Step 7: View Job Logs
* [cite_start]Display the logs of the latest job created by the CronJob to confirm it ran successfully[cite: 68].

## Result
[](Result.png)

