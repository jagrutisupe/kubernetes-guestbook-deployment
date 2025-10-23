# Kubernetes Guestbook Application Deployment

## Project Overview
This project demonstrates the complete lifecycle of deploying, scaling, and managing a containerized guestbook application on Kubernetes using IBM Cloud Container Registry.

## Author
**Your Name** - [Your GitHub Username]

## Technologies Used
- Docker & Multi-Stage Builds
- Kubernetes
- IBM Cloud Container Registry
- Horizontal Pod Autoscaler (HPA)
- Go (Golang 1.20)
- HTML/CSS/JavaScript

## Project Structure
```
guestbook/
├── v1/
│   └── guestbook/
│       ├── Dockerfile
│       ├── deployment.yml
│       ├── main.go
│       └── public/
│           ├── index.html
│           ├── script.js
│           └── style.css
└── v2/
```

## Features Implemented
1. ✅ Multi-stage Docker build for optimized image size
2. ✅ Kubernetes deployment with resource limits
3. ✅ Horizontal Pod Autoscaler (1-10 replicas)
4. ✅ Rolling updates with zero downtime
5. ✅ Rollback capabilities to previous versions
6. ✅ IBM Cloud Container Registry integration

## Deployment Steps

### 1. Build and Push Docker Image
```bash
export MY_NAMESPACE=sn-labs-$USERNAME
docker build . -t us.icr.io/$MY_NAMESPACE/guestbook:v1
docker push us.icr.io/$MY_NAMESPACE/guestbook:v1
```

### 2. Deploy to Kubernetes
```bash
kubectl apply -f deployment.yml
kubectl port-forward deployment.apps/guestbook 3000:3000
```

### 3. Configure Autoscaling
```bash
kubectl autoscale deployment guestbook --cpu-percent=5 --min=1 --max=10
kubectl get hpa guestbook --watch
```

### 4. Rolling Updates
```bash
# Update application
docker build . -t us.icr.io/$MY_NAMESPACE/guestbook:v1
docker push us.icr.io/$MY_NAMESPACE/guestbook:v1
kubectl apply -f deployment.yml

# View rollout history
kubectl rollout history deployment/guestbook

# Rollback if needed
kubectl rollout undo deployment/guestbook --to-revision=1
```

## Screenshots

### Application Deployment
![Deployed Guestbook Application](screenshots/app.png)

### Horizontal Pod Autoscaler
![HPA Scaling](screenshots/hpa2.png)

### Rolling Update
![Updated Application](screenshots/up-app.png)

## Key Learnings
- Docker multi-stage builds reduce image size significantly
- Kubernetes HPA automatically scales based on CPU metrics
- Rolling updates enable zero-downtime deployments
- Rollback capabilities provide safety for production deployments

## Challenges Faced
1. **Go Module Issues**: Resolved by initializing go.mod in Dockerfile
2. **Load Generator 404 Errors**: Fixed by using internal service URL
3. **HPA Metrics Delay**: Understanding Kubernetes metrics collection timing

## Future Enhancements
- [ ] Add Redis backend for persistent storage
- [ ] Implement Ingress for external access
- [ ] Add Prometheus monitoring
- [ ] Configure CI/CD pipeline
- [ ] Add unit and integration tests

## License
MIT License

## Acknowledgments
- IBM Cloud Container Registry
- Kubernetes Documentation
- Docker Best Practices Guide
