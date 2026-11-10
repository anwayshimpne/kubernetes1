# Final project submission — Kubernetes Guestbook

This repository includes the guestbook source and Kubernetes manifests. Replace `YOUR_NAMESPACE`/`REPLACE_WITH_NAMESPACE` with your IBM Cloud Container Registry namespace and run the commands in `README.md` in your own IBM Cloud account and cluster.

**Evidence note:** The repository cannot produce authentic IBM Cloud registry output, Kubernetes HPA state, rollout history, ReplicaSet state, or an image digest without access to the project owner's live account and cluster. The sections below specify exactly what to capture; they do not present invented terminal output as real evidence.

## Task 1 — Dockerfile (1 point)

The complete Dockerfile copies the web page into nginx's document root and exposes port 80:

```dockerfile
FROM nginx:1.27-alpine

COPY index.html /usr/share/nginx/html/index.html

EXPOSE 80
```

File: `Dockerfile`.

## Task 2 — Image pushed to IBM Cloud Container Registry with v1 tag (1 point)

Run `ibmcloud cr images` after pushing the image `us.icr.io/YOUR_NAMESPACE/guestbook:v1`. Paste the actual terminal output here; it must show the `v1` tag:

> Evidence to capture: `ibmcloud cr images` output from your account. Not available in this repository session.

## Task 3 — Original default HTML title and heading (1 point)

`versions/v1/index.html` preserves the v1 page:

```html
<title>Guestbook</title>
...
<h1>Guestbook</h1>
```

## Task 4 — HPA created while there are zero replicas (1 point)

Capture the output from your course's required command after setting the Deployment to zero and creating the HPA. Standard CPU-based HPA uses `minReplicas: 1` in `hpa.yml`; it cannot scale from zero using CPU metrics. Scale up to at least one replica before testing CPU autoscaling. Paste actual output here:

> Evidence to capture: `kubectl get hpa` and `kubectl get deployment guestbook` from the cluster. Not available in this repository session.

## Task 5 — Increased replicas showing autoscaling (2 points)

Run `kubectl get hpa guestbook --watch` and `kubectl get deployment guestbook --watch` during a successful scale-up. A CPU metric server, CPU requests, and sufficient CPU load are required; the simple static page may need the course's load generator. Paste actual output here:

> Evidence to capture: HPA current/desired replicas increasing and the Deployment's ready/available replicas. Not available in this repository session.

## Task 6 — Updated image push output with final digest (2 points)

Build and push the updated v2 image, then paste the entire actual `docker push` output, including its final `digest: sha256:...` line:

```sh
docker build -t us.icr.io/YOUR_NAMESPACE/guestbook:v2 .
docker push us.icr.io/YOUR_NAMESPACE/guestbook:v2
```

> Evidence to capture: output from your registry push. The digest is assigned by the registry and cannot be predetermined. Not available in this repository session.

## Task 7 — Updated deployment applied (1 point)

After substituting your registry namespace and setting the intended image tag in `deployment.yml`, run:

```sh
kubectl apply -f deployment.yml
```

Paste the real output here (typically a deployment configured/unchanged message):

> Evidence to capture: terminal output from your cluster. Not available in this repository session.

## Task 8 — Updated v2 HTML title and heading (2 points)

Current root `index.html` contains:

```html
<title>Guestbook – v2</title>
...
<h1>Guestbook – v2</h1>
```

## Task 9 — Rollout history with CPU-related changes (2 points)

Run `kubectl rollout history deployment/guestbook` after applying a Pod-template CPU/resource change (for example, changing the CPU request in `deployment.yml`), and paste the actual output:

> Evidence to capture: rollout history showing the revisions from your cluster. Not available in this repository session.

## Task 10 — ReplicaSets after rollback (2 points)

Run `kubectl rollout undo deployment/guestbook`, wait for the rollout, then run `kubectl get rs`. Paste the actual output:

> Evidence to capture: ReplicaSet list from your cluster after rollback. Not available in this repository session.

## Submission checklist

- [ ] Replace the registry namespace in the manifest and push v1 and v2 images.
- [ ] Deploy the v1 and v2 revisions, apply the HPA, and perform the CPU/resource update and rollback.
- [ ] Paste unedited terminal output for Tasks 2, 4–7, 9, and 10; include the final digest for Task 6.
- [ ] Save this file and commit the real evidence before submitting the assignment.
