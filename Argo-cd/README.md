# Argo-cd

This folder contains Argo CD configuration files for GitOps-based continuous deployment.

## Files
- `argo-application.yaml` - Application manifest for Argo CD
- `argo-cd.yml` - Argo CD server configuration

## Usage
Install Argo CD and apply the configurations:
```bash
kubectl apply -f argo-cd.yml
kubectl apply -f argo-application.yaml
```

## References
- [Argo CD Documentation](https://argo-cd.readthedocs.io/)



# 🚀 ArgoCD Application – Clear Understanding Guide

## 🧠 Core Concept (Remember This Forever)

ArgoCD does **NOT read your GitHub automatically**

👉 You must **create an Application inside Kubernetes**

---

## 🔥 How ArgoCD Works

```
Step 1: Create Application (kubectl apply OR UI)
Step 2: ArgoCD reads GitHub repo
Step 3: ArgoCD deploys YAML to Kubernetes
```

---

## 📦 Your Repo Structure (Correct Way)

```
Kubernetes/
 ├── Argo-cd/
 │    └── argo-application.yaml   (ONLY defines apps)
 │
 ├── Kind/
 │    ├── deployment.yaml        (Actual app YAML)
 │    ├── service.yaml
```

---

## ✅ Application YAML Example

```
apiVersion: argoproj.io/v1alpha1
kind: Application
metadata:
  name: kind-app
  namespace: argocd

spec:
  project: default
  source:
    repoURL: https://github.com/himwasnik/Kubernetes.git
    targetRevision: HEAD
    path: Kind
  destination:
    server: https://kubernetes.default.svc
    namespace: nginx
  syncPolicy:
    automated:
      prune: true
      selfHeal: true
```

---

## 🚨 IMPORTANT RULE

👉 Just pushing YAML to GitHub = ❌ NOTHING happens

👉 You MUST run:

```
kubectl apply -f <your-application.yaml>
```

---

## 🔁 When You Update GitHub

| Change                 | Need kubectl apply? |
| ---------------------- | ------------------- |
| Change deployment.yaml | ❌ No                |
| Change service.yaml    | ❌ No                |
| Add new Application    | ✅ YES               |

---

## 🌐 UI vs YAML

| UI           | YAML          |
| ------------ | ------------- |
| Fill form    | Write YAML    |
| Click create | kubectl apply |
| Same result  | Same result   |

---

## ⚠️ Common Mistakes

❌ Thinking ArgoCD auto-detects new Application
❌ Wrong path in repo
❌ Missing Kubernetes YAML files
❌ Namespace not created

---

## ✅ Quick Commands

Create app:

```
kubectl apply -f argo-application.yaml
```

Check:

```
kubectl get applications -n argocd
```

---

## 🧠 Final Mental Model

```
Application YAML → tells ArgoCD WHERE repo is
ArgoCD → reads YAML from repo
Kubernetes → gets deployed
```

---

## 🚀 Pro Tip

👉 Jenkins builds image
👉 ArgoCD deploys using YAML

---

## 🎯 Summary

* ArgoCD = Deployment tool (GitOps)
* GitHub = Source of truth
* kubectl apply = creates connection

---

✅ If you remember just ONE line:

👉 **“ArgoCD needs Application created first, then it reads GitHub”**

---

