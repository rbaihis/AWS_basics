## basic
```bash
aws --version
aws --config
aws sts get-caller-identity
aws sts get-session-token --duration-seconds 18000
```
### EKS get cluster config access
```
aws eks update-kubeconfig --name my-first-terra-cluster --region eu-west-2
```
