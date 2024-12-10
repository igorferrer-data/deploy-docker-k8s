# Criando uma Pipeline de Deploy com Docker e Kubernetes.  

## Criando uma pipeline de deploy da aplicação Node.js em um container Docker.  

## Deploy usando Docker e GitHub Actions  
O pipeline realiza as seguintes etapas:
- **Testes** da aplicação.
- **Build** da imagem Docker.
- **Execução** da aplicação em um contêiner Docker.
---
### Imagem Docker

```docker
docker build -t minha-app-node .
```

### Rodar o Container

```docker
docker run -p 3000:3000 minha-app-node
```

### Push para o Github

```bash
git checkout -b docker-deploy
git add Dockerfile
git commit -m "Add Dockerfile"
git push origin docker-deploy
```
---
### Como Funciona:
1. O pipeline testa a aplicação para garantir que ela está funcionando corretamente.
2. Em seguida, ele cria uma imagem Docker da aplicação Node.js.
3. Por fim, a imagem é executada como um contêiner na plataforma de CI/CD
---

# Criando uma pipeline de deploy em um cluster Kubernetes em nuvem utilizando o GCP.

## Deploy no Kubernetes (Azure)  
O pipeline faz o seguinte:
- **Testes** da aplicação.
- **Build** de uma imagem Docker.
- **Deploy** da imagem no Kubernetes hospedado no **Azure Kubernetes Service (AKS)**.
---
### Manifests Kubernetes

> **deployment.yml**

```YAML
apiVersion: apps/v1
kind: Deployment
metadata:
  name: node-app
spec:
  replicas: 3
  selector:
    matchLabels:
      app: node-app
  template:
    metadata:
      labels:
        app: node-app
    spec:
      containers:
      - name: node-app
        image: usuario/minha-app-node:latest
        ports:
        - containerPort: 3000
```

> **service.yml**

```YAML
apiVersion: v1
kind: Service
metadata:
  name: node-app-service
spec:
  selector:
    app: node-app
  ports:
    - protocol: TCP
      port: 80
      targetPort: 3000
  type: LoadBalancer
```

### Kubernetes

```Bash
kubectl apply -f deployment.yml
kubectl apply -f service.yml
```
---

### Como Funciona:
1. O pipeline realiza os testes da aplicação.
2. Em seguida, ele constrói uma imagem Docker.
3. A imagem é enviada para um repositório Docker.
4. Finalmente, a imagem é **deployed** em um cluster **Kubernetes** no **Azure**, permitindo que a aplicação seja escalada e gerenciada facilmente.

