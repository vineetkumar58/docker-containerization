# Hands-on Task: Run and Manage a “Hello Web App” (httpd)
# Objective:
# Deploy and manage a simple Apache-based web server and:
   - verify it is running
   - modify it
   - scale it
   - debug it

## Task: Deploy a Simple Web Application (Apache httpd)
   - You will run an Apache server instead of nginx.


### Switch context to docker desktop
![Docker Image](../taskk8sscreenshots/1.png)

### Step 1: Run a Pod and check it
![Docker Image](../taskk8sscreenshots/2.png)

### Step 2: Inspect Pod
- Focus:
   - container image = httpd
   - ports (default 80)
   - events
     
![Docker Image](../taskk8sscreenshots/3.png)

### Step 3: Access the App
- Open:
  - http://localhost:8081
- You should see: → Apache default page (“It works!”)
  
![Docker Image](../taskk8sscreenshots/4.png)

![Docker Image](../taskk8sscreenshots/5.jpeg)

### Step 4: Delete Pod
- Insight
- Same as before:
    - Pod disappears permanently
    - No self-healing
      
![Docker Image](../taskk8sscreenshots/6.png)

## Task: Convert to Deployment

### Step 5: Create Deployment and check it

![Docker Image](../taskk8sscreenshots/7.png)

### Step 6: Expose Deployment and acess it

![Docker Image](../taskk8sscreenshots/8.png)

![Docker Image](../taskk8sscreenshots/9.jpeg)

## Task: Modify Behavior

### Step 7: Scale Deployment and check it
- Observe
   - Multiple pods running same app

![Docker Image](../taskk8sscreenshots/10.png)

### Step 8: Test Load Distribution (Basic)
- Run port-forward again and refresh browser multiple times.
- same thing even after refreshing
  
![Docker Image](../taskk8sscreenshots/9.jpeg)

## Task: Debugging Scenario

### Step 9: Break the App

![Docker Image](../taskk8sscreenshots/11.png)

### Step 10: Diagnose
- Look for:
   - ImagePullBackOff
   - error messages

![Docker Image](../taskk8sscreenshots/12.png)

### Step 11: Fix It
![Docker Image](../taskk8sscreenshots/13.png)

## Task: Explore Inside Container (Important Skill)

### Step 12: Exec into Pod
- Now inside container:
     - ls /usr/local/apache2/htdocs
- This is where web files are stored.
  
![Docker Image](../taskk8sscreenshots/14.png)

## Task: Observe Self-Healing

### Step 13: Delete One Pod
- Insight
   - Deployment recreates pod automatically
     
![Docker Image](../taskk8sscreenshots/15.png)

## Task: Cleanup

![Docker Image](../taskk8sscreenshots/16.png)

## What You Learned (Important)
- This task is better than nginx because:
    - You accessed actual web output
    - You explored container filesystem
    - You practiced debugging real errors
    - You saw scaling + recovery

## Optional Next Challenge
-Modify container at runtime:

![Docker Image](../taskk8sscreenshots/17.png)

![Docker Image](../taskk8sscreenshots/18.png)

![Docker Image](../taskk8sscreenshots/20.jpeg)
