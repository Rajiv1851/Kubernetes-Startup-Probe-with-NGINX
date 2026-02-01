# 🧪 Kubernetes Startup Probe with NGINX (Complete Lab)

## 📌 Objective

In this lab, I learned how to configure and troubleshoot **Startup Probe** in Kubernetes using an NGINX Deployment.  
The goal was to understand how **startupProbe**, **livenessProbe**, and **readinessProbe** work together for slow-starting applications.

---

## 🧠 What is Startup Probe?

A **startupProbe** checks whether an application inside a container has successfully started.

Key points:

- startupProbe runs first
- Liveness and Readiness probes are disabled until startupProbe succeeds
- If startupProbe fails too many times, Kubernetes restarts the container
- Useful for slow-starting applications

---

## 🧪 Lab Scenario

I intentionally delayed application startup using the following command:


