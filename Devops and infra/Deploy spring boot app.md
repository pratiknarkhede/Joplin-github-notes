

1. Dockerfile (bare minimum config)

Base image → eclipse-temurin:17-jre-jammy (JRE only).

Workdir → WORKDIR /app.

Copy jar → COPY target/*.jar app.jar.

Expose port → EXPOSE 8080.

Entrypoint → ENTRYPOINT ["java","-jar","/app/app.jar"].

(Optional) Multi-stage build with Maven for smaller images.



---

2. Jenkins + GitHub (pipeline trigger)

GitHub → Webhook → Jenkins job runs on push/PR.

Pipeline stages (mandatory):

1. Checkout → pull code from GitHub.


2. Build → mvn package.


3. Docker build + push → image goes to ECR (AWS image storage).


4. Deploy → Jenkins updates Kubernetes (EKS) with new image.



Jenkins stores AWS credentials and kubeconfig.



---

3. Kubernetes Deployment (key configs)

Deployment:

replicas: 3 → number of pods (instances).

Container →

name: springboot

image: <ECR_REPO>/myapp:tag

ports: containerPort: 8080

resources: (basic sizing)

requests:
  cpu: "250m"
  memory: "256Mi"
limits:
  cpu: "500m"
  memory: "512Mi"

readinessProbe + livenessProbe → /actuator/health.





---

4. Service (inside cluster)

Type: ClusterIP.

Maps stable name → pod port.

kind: Service
metadata:
  name: springboot-service
spec:
  selector:
    app: springboot-app
  ports:
    - port: 80
      targetPort: 8080



---

5. Ingress (expose to internet)

Requires AWS Load Balancer Controller.

Minimal structure:

kind: Ingress
metadata:
  name: springboot-ingress
  annotations:
    kubernetes.io/ingress.class: alb
    alb.ingress.kubernetes.io/scheme: internet-facing
spec:
  rules:
    - host: myapp.example.com
      http:
        paths:
          - path: /
            pathType: Prefix
            backend:
              service:
                name: springboot-service
                port:
                  number: 80



---

6. Config & Secrets

ConfigMap → non-sensitive values (application.properties).

Secret → sensitive values (DB password, API keys).

Use envFrom: to inject into container.



---

7. Flow Recap

1. Developer pushes code → GitHub.


2. GitHub Webhook → triggers Jenkins pipeline.


3. Jenkins:

Build jar (mvn package).

Build Docker image.

Push image → AWS ECR.

Update deployment in AWS EKS.



4. EKS runs pods with new image.


5. Service connects pods internally.


6. Ingress/ALB exposes app to internet (via domain).




---

🔄 Flow Diagram

Developer
   │
   ▼
 GitHub (code repo)
   │  webhook
   ▼
 Jenkins (CI/CD pipeline)
   │ 1. Build jar
   │ 2. Build Docker image
   │ 3. Push to ECR
   │ 4. Deploy to EKS
   ▼
 AWS ECR (Docker image storage)
   │
   ▼
 AWS EKS (Kubernetes cluster)
   │
   ▼
 ┌───────────────┐
 │ Deployment    │ → runs Pods (containers)
 │ (3 replicas)  │
 └─────┬─────────┘
       │
       ▼
 Service (ClusterIP)
       │
       ▼
 Ingress (ALB → internet)
       │
       ▼
   End User (browser / client)




