# Capstone Project: End-to-End MLOps Pipeline with MLFlow, DVC, Flask, Docker, EKS, Prometheus & Grafana

This repository demonstrates a full Machine Learning (ML) lifecycle setup using industry-grade tools including **MLFlow**, **DVC**, **Dagshub**, **Docker**, **Flask**, **EKS**, **Prometheus**, and **Grafana**.

---

## 🚀 Project Structure Setup

1. Create a GitHub repository and clone it locally.
2. Create a virtual environment:
   ```bash
   conda create -n atlas python=3.10
   conda activate atlas

    Install Cookiecutter and generate the structure:

    pip install cookiecutter
    cookiecutter -c v1 https://github.com/drivendata/cookiecutter-data-science

    Rename src.models to src.model.

    Add files to Git, commit, and push.

📦 MLFlow Setup on Dagshub

    Create a repo on Dagshub and connect your GitHub repo.

    Copy the MLFlow experiment tracking URL and credentials.

    Install required libraries:

pip install dagshub mlflow

Run your experiment notebooks and commit changes:

    git add . && git commit -m "Add MLFlow notebooks" && git push

🔄 DVC Setup

    Initialize DVC:

dvc init

Create a temporary local storage directory:

mkdir local_s3
dvc remote add -d mylocal local_s3

Implement the following in src/:

    logger.py

    data_ingestion.py

    data_preprocessing.py

    feature_engineering.py

    model_building.py

    model_evaluation.py

    register_model.py

Add dvc.yaml and params.yaml, then run:

    dvc repro
    dvc status
    git add . && git commit -m "Add DVC pipeline" && git push

☁️ S3 Remote Setup for DVC

    Create an S3 bucket and IAM user with full access.

    Install required tools:

    pip install dvc[s3] awscli
    aws configure  # Set your AWS keys
    dvc remote add -d myremote s3://<bucket-name>

🌐 Flask API and Requirements

    Create a flask_app/ directory and implement API.

    Install and run:

pip install flask
python app.py

Save dependencies:

    pip freeze > requirements.txt

🔁 CI/CD with GitHub Actions

    Add workflow: .github/workflows/ci.yaml

    Generate a Dagshub token and add it as a GitHub Secret.

    Add test scripts in tests/ and scripts/.

🐳 Docker Setup

    Install pipreqs and generate minimal requirements:

pip install pipreqs
cd flask_app
pipreqs . --force

Create a Dockerfile, then:

    docker build -t capstone-app:latest .
    docker run -p 8888:5000 -e CAPSTONE_TEST=<your_token> capstone-app:latest

⚙️ ECR & AWS Secrets

    Setup GitHub Secrets:

        AWS_ACCESS_KEY_ID

        AWS_SECRET_ACCESS_KEY

        AWS_REGION

        ECR_REPOSITORY

        AWS_ACCOUNT_ID

☸️ EKS Cluster Setup

    Uninstall conflicting AWS CLI (if installed via conda):

pip uninstall awscli

Install & verify:

    AWS CLI

    kubectl

    eksctl

Create the cluster:

eksctl create cluster --name flask-app-cluster --region us-east-1 --nodegroup-name flask-app-nodes --node-type t3.small --nodes 1 --managed

Verify and update Kube config:

aws eks --region us-east-1 update-kubeconfig --name flask-app-cluster
kubectl get nodes

Deploy via CI/CD pipeline, expose port 5000, and access using:

    kubectl get svc flask-app-service
    curl http://<external-ip>:5000

📊 Prometheus Setup

    Launch Ubuntu EC2 (port 9090 open).

    Install Prometheus and configure prometheus.yml with:

scrape_configs:
  - job_name: "flask-app"
    static_configs:
      - targets: ["<external-ip>:5000"]

Run Prometheus:

    /usr/local/bin/prometheus --config.file=/etc/prometheus/prometheus.yml

📈 Grafana Setup

    Launch another EC2 (port 3000 open), install Grafana.

    Access at: http://<ec2-ip>:3000 (admin/admin)

    Add Prometheus (URL: http://<prometheus-ec2-ip>:9090) as a data source.

🧹 AWS Resource Cleanup

kubectl delete deployment flask-app
kubectl delete service flask-app-service
eksctl delete cluster --name flask-app-cluster --region us-east-1

    Remove secrets, ECR, S3 if not needed.

    Confirm cleanup on CloudFormation.

🧠 CloudFormation & EKS

    eksctl creates EKS clusters using CloudFormation stacks.

    These stacks manage both control plane and node groups.

    Cleanup is automated if cluster is deleted via eksctl.

📘 Kubernetes Concepts
What is a PVC?

    A PersistentVolumeClaim (PVC) is a way for pods to request storage.

    It binds to a PersistentVolume (PV).

    Often backed by AWS EBS in EKS.

✅ Done!

You're now ready with a robust, production-grade ML system deployed via EKS with monitoring & CI/CD in place.
