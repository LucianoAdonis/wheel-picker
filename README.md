# Who to Blame Today? 🎯

A fun, retro game show-style wheel of fortune to randomly select which team to "blame" for the day. Built as a lightweight, containerized web application for EKS deployment.

## Features

- 🎨 Bold retro game show aesthetic with dramatic animations
- 🎪 Smooth spinning wheel with physics-based deceleration
- 📱 Fully responsive design
- 🚀 Lightweight Docker container (~10MB)
- ☸️ Ready for Kubernetes/EKS deployment

## Roles on the Wheel

- Infra
- Data Development
- QA
- Product
- Scrum Master
- Cloud

## Local Development

Simply open `index.html` in a web browser to run locally.

## Docker Deployment

### Build the Docker Image

```bash
docker build -t wheel-of-blame:latest .
```

### Run Locally with Docker

```bash
docker run -p 8080:80 wheel-of-blame:latest
```

Then visit `http://localhost:8080` in your browser.

### Push to Container Registry (ECR Example)

```bash
# Authenticate with ECR
aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin <account-id>.dkr.ecr.us-east-1.amazonaws.com

# Tag the image
docker tag wheel-of-blame:latest <account-id>.dkr.ecr.us-east-1.amazonaws.com/wheel-of-blame:latest

# Push to ECR
docker push <account-id>.dkr.ecr.us-east-1.amazonaws.com/wheel-of-blame:latest
```

## Kubernetes/EKS Deployment

### Prerequisites

- An EKS cluster configured and running
- `kubectl` configured to connect to your cluster
- Docker image pushed to ECR or another container registry

### Deploy to EKS

1. Update the image reference in `kubernetes-deployment.yaml` with your ECR image URL:

```yaml
image: <account-id>.dkr.ecr.us-east-1.amazonaws.com/wheel-of-blame:latest
```

2. Apply the Kubernetes manifests:

```bash
kubectl apply -f kubernetes-deployment.yaml
```

3. Get the LoadBalancer URL:

```bash
kubectl get service wheel-of-blame-service
```

Wait a few minutes for the LoadBalancer to provision, then access the app using the `EXTERNAL-IP` address.

### Verify Deployment

```bash
# Check pods are running
kubectl get pods -l app=wheel-of-blame

# Check service status
kubectl get svc wheel-of-blame-service

# View logs
kubectl logs -l app=wheel-of-blame
```

## Architecture

- **Frontend**: Pure HTML/CSS/JavaScript (no frameworks)
- **Web Server**: Nginx (Alpine Linux)
- **Container Size**: ~10MB
- **Resources**: 64Mi RAM / 100m CPU (request), 128Mi RAM / 200m CPU (limit)

## Customization

To modify the roles on the wheel, edit the `roles` array in the `<script>` section of `index.html`:

```javascript
const roles = [
    'Your Role 1',
    'Your Role 2',
    'Your Role 3',
    'Your Role 4',
    'Your Role 5',
    'Your Role 6'
];
```

Note: The wheel is designed for exactly 6 segments. Adding more or fewer roles will require adjusting the CSS segment rotations.

## License

MIT
