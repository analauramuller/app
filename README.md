## Projeto
Este projeto foi realizado para otimizar o aprendizado sobre containers, orquestração de aplicações e testes de infraestrutura. Abaixo estão detalhadas as etapas que foram realizadas até o momento:

### Instalação do Kind

```bash
curl -Lo ./kind https://kind.sigs.k8s.io/dl/v0.28.0/kind-linux-amd64
chmod +x ./kind
sudo mv ./kind /usr/local/bin/kind
```

### Criação de cluster nomeado (kind.yaml)

```bash
kind: Cluster
apiVersion: kind.x-k8s.io/v1alpha4
nodes:
  - role: control-plane
  - role: worker
  - role: worker
  - role: worker
```

```bah
kind create cluster --config kind.yaml --name devops
```
### Criação de Aplicação com FastAPI

```bash
python -m venv .venv
source .venv/bin/activate
pip install fastapi uvicorn
```

### Código da aplicação (main.py)
```bash
from fastapi import FastAPI

app = FastAPI()

@app.get("/")
def read_root():
    return {"message": "Hello World!"}

if __name__ == '__main__':
    import uvicorn
    uvicorn.run(app, host="0.0.0.0", port=8000)
```

### Criação do Dockerfile na raiz do projeto

```bash
FROM python:3.13-slim
WORKDIR /app
COPY app/requirements.txt requirements.txt
RUN pip install --no-cache-dir -r requirements.txt
COPY app/main.py main.py
CMD ["python", "main.py"]
```
### Publicação da imagem no DockerHub
```bash
docker login
docker push seu_usuario/hello-fastapi-k8s
```


