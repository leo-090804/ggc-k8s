# HelloWorld GKE

Ứng dụng Flask đơn giản triển khai trên Google Kubernetes Engine (GKE).

## Cấu trúc dự án

- `app.py`: Ứng dụng Flask trả về "Hello World".
- `Dockerfile`: Định nghĩa cách build Docker image cho ứng dụng.
- `deployment.yaml`: Manifest Kubernetes để deploy ứng dụng lên GKE.
- `service.yaml`: Manifest Kubernetes để expose ứng dụng qua LoadBalancer.
- `.dockerignore`: Các file/folder bị loại trừ khi build Docker image.

## Hướng dẫn sử dụng

### 1. Google Cloud Initialization

```bash
gcloud init
```
Sau đó là các bước config Google Cloud
![alt text](demo/cfggc.png)

### 2. Tải Kubectl
```bash
gcloud components install kubectl
```

### 3. Tạo repo trong Artifact Registry
```bash
gcloud artifacts repositories create hello-repo \
    --project=$projectID \
    --repository-format=docker \
    --location=us-central1 \
    --description=""
```
![](demo/repo_create.png)

### 4. Build và Push
```bash
gcloud builds submit --tag us-central1-docker.pkg.dev/rosy-embassy-454206-f4/hello-repo/helloworld-gke .
```
![](demo/build_push.png)

### 5. Tạo Cluster
```bash
gcloud container clusters create-auto helloworld-gke --location us-central1
```
![](demo/cluster_cre.png)

### 6. Deployement
```bash
kubectl apply -f deployment.yaml
kubectl apply -f service.yaml
```
![alt text](demo/deployment.png)
![alt text](demo/service.png)


### 7. Mornitoring trên Cloud 
![alt text](demo/mnt.png)
![alt text](demo/mnt2.png)

### 8. Scalability
![alt text](demo/horizontal.png)

Stress test với Jmeter
![alt text](demo/stress.png)

Trước quá trình stress test, deployment chỉ có 1 pod, còn sau đó
![alt text](demo/pods.png)