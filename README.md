# 📝 Deploy de WordPress com MariaDB no Kubernetes

Este projeto realiza a implantação de um ambiente WordPress com banco de dados MariaDB utilizando Kubernetes, com separação clara de responsabilidades, persistência de dados, gerenciamento seguro de senhas e acesso externo via NodePort.

---

## 📂 Estrutura dos Arquivos

O projeto está dividido em múltiplos arquivos YAML, organizados por funcionalidade:

| Arquivo                     | Descrição                                                                 |
|----------------------------|---------------------------------------------------------------------------|
| `00-namespace.yaml`        | Cria o namespace `blog` para isolar os recursos do projeto.               |
| `01-secret.yaml`           | Armazena as credenciais do banco de dados MariaDB de forma segura.        |
| `02-pvc-mariadb.yaml`      | PersistentVolumeClaim para armazenamento do MariaDB.                      |
| `03-pvc-wordpress.yaml`    | PersistentVolumeClaim para armazenamento do WordPress.                    |
| `04-deploy-mariadb.yaml`   | Cria o Deployment do MariaDB com volume persistente e variáveis secretas. |
| `05-svc-mariadb.yaml`      | Service interno (ClusterIP) para o banco de dados MariaDB.                |
| `06-deploy-wordpress.yaml` | Cria o Deployment do WordPress com volume persistente e acesso ao banco.  |
| `07-svc-wordpress.yaml`    | Service externo (NodePort) para acessar o WordPress no navegador.         |

---

## 🚀 Como Aplicar os Arquivos

1. **Crie o namespace**:
   ```bash
   kubectl apply -f 00-namespace.yaml
   ```

2. **Aplique os recursos na ordem correta**:
   ```bash
   kubectl apply -f 01-secret.yaml
   kubectl apply -f 02-pvc-mariadb.yaml
   kubectl apply -f 03-pvc-wordpress.yaml
   kubectl apply -f 04-deploy-mariadb.yaml
   kubectl apply -f 05-svc-mariadb.yaml
   kubectl apply -f 06-deploy-wordpress.yaml
   kubectl apply -f 07-svc-wordpress.yaml
   ```

3. **Verifique se os pods estão rodando**:
   ```bash
   kubectl get pods -n blog
   ```

4. **Acesse o WordPress no navegador**:
   - Se estiver rodando em um ambiente local (como Minikube), use:
     ```bash
     minikube service wordpress-svc -n blog
     ```
   - Ou acesse via IP do Node e porta `30080`.

---

## ⚙️ Recursos Utilizados

- Kubernetes
- WordPress (imagem oficial `wordpress:6.5-apache`)
- MariaDB (imagem oficial `mariadb:11`)
- PVC para persistência de dados
- Secret para armazenar credenciais de forma segura
- Namespace para isolamento dos recursos
- Services (ClusterIP e NodePort)

---

## 🧠 Explicações Técnicas (Completar)



---

## 🧪 Testes Realizados (Completar)


---

## ✅ Conclusão (Completar)


---

## 📸 Screenshots (Completar)


---

## ✍️ Autores

- **Felipe Parreiras e José Marconi**
- Curso: Engenharia de Computação - CEFET-MG
- Projeto de Kubernetes com WordPress + MariaDB
