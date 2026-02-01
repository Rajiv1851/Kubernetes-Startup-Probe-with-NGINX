# 🧪 Kubernetes Startup Probe with NGINX (Complete Lab)

## 📌 Objective

In this lab, I learned how to configure and troubleshoot **Startup Probe** in Kubernetes using an NGINX Deployment.  
The goal was to understand how **startupProbe**, **livenessProbe**, and **readinessProbe** work together for slow-starting applications.

---

## 🧠 What is Startup Probe?

A **startupProbe** checks whether an application inside a container has successfully started.

### Key Points

- startupProbe runs first  
- Liveness and Readiness probes are disabled until startupProbe succeeds  
- If startupProbe fails too many times, Kubernetes restarts the container  
- Useful for slow-starting applications  

---

## 🧪 Lab Scenario

I intentionally delayed application startup using the following command:



command: [ "sh", "-c", "sleep 50 && nginx -g 'daemon off;'" ]


This simulates a slow-starting application.

### Behavior

- Container sleeps for 50 seconds  
- NGINX is not running during sleep  
- startupProbe starts checking early  
- startupProbe fails  
- Kubernetes restarts the container  

---

## 🗂️ Complete Deployment YAML (startupProbe.yml)

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: mydeploy
spec:
  replicas: 3
  selector:
    matchLabels:
      app: myapp
  template:
    metadata:
      labels:
        app: myapp
    spec:
      containers:
      - name: mycontainer
        image: nginx
        ports:
        - containerPort: 80

        # Delay application startup
        command: [ "sh", "-c", "sleep 50 && nginx -g 'daemon off;'" ]

        # Liveness Probe
        livenessProbe:
          httpGet:
            path: /index.html
            port: 80
          initialDelaySeconds: 5
          periodSeconds: 5
          timeoutSeconds: 2
          failureThreshold: 3

        # Readiness Probe
        readinessProbe:
          httpGet:
            path: /login.html
            port: 80
          initialDelaySeconds: 5
          periodSeconds: 5
          timeoutSeconds: 2
          failureThreshold: 3

        # Startup Probe
        startupProbe:
          httpGet:
            path: /index.html
            port: 80
          initialDelaySeconds: 5
          periodSeconds: 5
          timeoutSeconds: 2
          failureThreshold: 3

🧮 Why Startup Probe Failed
Startup Probe Configuration
initialDelaySeconds = 5
periodSeconds = 5
failureThreshold = 3

Maximum Allowed Startup Time
5 + (5 × 3) = 20 seconds

Actual Startup Delay
sleep 50 seconds

Result

❌ startupProbe fails
❌ Container is restarted
🔁 Pod enters restart loop

🔍 Commands Used
Apply Deployment
kubectl apply -f startupProbe.yml

Check Pod Status
kubectl get pods

Describe Pod (to see probe failures)
kubectl describe pod <pod-name>

📸 Observations

Startup probe failed events

Container restarts automatically

Liveness and readiness probes never activate

Pod goes into restart/back-off state

🏁 Key Learnings

startupProbe is designed for slow-starting applications

It runs before liveness and readiness probes

If startupProbe fails, Kubernetes restarts the container

Proper tuning prevents unnecessary restarts

Correct probe configuration improves application stability

🚀 Skills Practiced

Kubernetes Probes (Startup, Liveness, Readiness)

Debugging Pod Restarts

Kubernetes Deployment YAML

Application Health Checks

Real-world Kubernetes troubleshooting
