# Accountability Roulette

A single-page web application that randomly selects team roles using an interactive spinning wheel. Built with vanilla HTML/CSS/JavaScript and packaged in a lightweight nginx Docker container for Kubernetes deployment.

## Features

- Interactive canvas-based spinning wheel
- Keyboard shortcuts and touch support
- Responsive design for all devices
- Lightweight Docker container (~10MB)
- Google Analytics integration
- PWA-ready with web manifest

## Quick Start

### Local Development

Open `index.html` in a web browser. No build process required.

### Docker

```bash
docker build -t wheel-of-blame:latest .
docker run -p 8080:80 wheel-of-blame:latest
```

Visit http://localhost:8080

### GitHub Pages

The site is deployed at: https://lucianoadonis.github.io/wheel-of-excuses/

## Deployment

### Push to ECR

```bash
aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin <account>.dkr.ecr.us-east-1.amazonaws.com
docker tag wheel-of-blame:latest <account>.dkr.ecr.us-east-1.amazonaws.com/wheel-of-blame:latest
docker push <account>.dkr.ecr.us-east-1.amazonaws.com/wheel-of-blame:latest
```

### Deploy to Kubernetes

```bash
# Update image reference in infra/kubernetes-deployment.yaml
kubectl apply -f infra/kubernetes-deployment.yaml
kubectl get service wheel-of-blame-service
```

## Controls

- **Click** or **Touch** wheel to spin
- **Space** or **Enter** for random spin
- **Letter keys** (I/D/Q/V/P/C) to select specific team
- **?** to show/hide help menu
- **8** for hidden easter egg

## Customization

Edit the `roles` array in `index.html` (around line 998):

```javascript
const roles = [
    {
        name: 'INFRA',
        color: '#0063e5',
        key: 'I',
        excuses: ['...', '...', '...']
    },
    // ... 5 more roles
];
```

Note: The wheel requires exactly 6 segments. Changing this requires updating canvas math.

## Architecture

- Single-file HTML/CSS/JavaScript application (no build process)
- Vanilla JavaScript (no frameworks)
- Canvas API for wheel rendering
- Nginx Alpine container
- Container size: ~10MB
- Resource limits: 64Mi RAM / 100m CPU (request), 128Mi / 200m CPU (limit)

## Tech Stack

- HTML5 Canvas
- CSS Variables for theming
- Vanilla JavaScript (ES6+)
- Google Fonts (Montserrat)
- Google Analytics (GA4)
- Docker + nginx:alpine
- Kubernetes ready

## License

MIT
