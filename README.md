# 📝 Deploy de WordPress com MariaDB no Kubernets

Este projeto realiza a implantação de um ambiente WordPress com banco de dados MariaDB utilizando Kubernet's, com separação clara de responsabilidades, persistência de dados, gerenciamento seguro de senhas e acesso externo via NodePort.

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

### Requisitos 
- _Docker_ instalado
- _Minikube_ instalado
- _kubectl_ instalado


> Você pode verificar se tudo foi instalado corretamente com:
> ```bash
> docker --version
> kubectl version --client
> minikube version
> ```

Iniciar Minikube: **minikube start**
Parar (preserva dados dos PVCs): **minikube stop**
Deletar (remove tudo): **minikube delete**


---

## ⚙️ Recursos Utilizados

- Kubernet's
- WordPress (imagem oficial `wordpress:6.5-apache`)
- MariaDB (imagem oficial `mariadb:11`)
- PVC para persistência de dados
- Secret para armazenar credenciais de forma segura
- Namespace para isolamento dos recursos
- Services (ClusterIP e NodePort)

---

## 🔧 Comandos Utilizados

```bash
kubectl apply -f 00-namespace.yaml
kubectl apply -f 01-secret.yaml
kubectl apply -f 02-pvc-mariadb.yaml
kubectl apply -f 03-pvc-wordpress.yaml
kubectl apply -f 04-deploy-mariadb.yaml
kubectl apply -f 05-svc-mariadb.yaml
kubectl apply -f 06-deploy-wordpress.yaml
kubectl apply -f 07-svc-wordpress.yaml
```

### Testar e Inspecionar Recursos

```bash
kubectl get all -n blog
kubectl describe pod <nome-do-pod> -n blog
kubectl logs <nome-do-pod> -n blog
kubectl exec -it <nome-do-pod> -n blog -- /bin/bash
```

### Recursos do Kubernet´s Utilizados

| Recurso      | Finalidade                                                                        |
| ------------ | --------------------------------------------------------------------------------- |
| `Namespace`  | Isolamento lógico dos recursos do projeto.                                        |
| `Secret`     | Armazenamento seguro de credenciais (senha do banco, usuário, etc).               |
| `PVC`        | Armazenamento persistente para banco de dados e arquivos do WordPress.            |
| `Deployment` | Gerenciamento dos pods (WordPress e MariaDB), garantindo alta disponibilidade.    |
| `Service`    | Comunicação entre os serviços (ClusterIP para MariaDB e NodePort para WordPress). |

---



## 🧠 Explicações Técnicas

A integração entre WordPress e MariaDB usando Kubernet's Deployments com Persistent Volumes (PVC) representa uma orquestração realista de serviços em ambientes de produção. Essa configuração permite a execução de aplicações web com banco de dados, mantendo persistência de dados, alta disponibilidade e modularidade na gestão dos recursos.

### 🔧 O que é?

Essa integração envolve:

- Um Deployment do WordPress, responsável por servir o CMS (Content Management System) aos usuários.

- Um Deployment do MariaDB, que atua como banco de dados relacional para armazenar dados do WordPress.

- Persistent Volumes Claims (PVCs) associados a cada serviço, garantindo que os dados não se percam mesmo que os pods sejam reiniciados ou reimplantados.

- Comunicação entre os serviços via Service, permitindo que o WordPress se conecte ao banco MariaDB internamente, de forma estável.

### 🧰 Para que serve?

Essa arquitetura serve para:

- Demonstrar uma aplicação real completa dentro de um cluster Kubernet's.

- Separar as responsabilidades entre frontend (WordPress) e backend (MariaDB).

- Garantir a persistência dos dados com volumes vinculados aos pods.

- Permitir escalabilidade, atualização e manutenção independentes entre banco e aplicação.

### 📍 Onde usar?

É amplamente usada em:

- Ambientes de desenvolvimento e testes com simulação real de produção.

- Ambientes de produção com múltiplos serviços interdependentes.

- Plataformas de hospedagem de sites gerenciados com WordPress.

- Sistemas onde a persistência de dados é crítica e a orquestração de containers é necessária.

### 📌 Benefícios da abordagem com Kubernet's:

✅ Escalabilidade Horizontal: os pods podem ser replicados para lidar com picos de tráfego.

✅ Recuperação automática: o Kubernet's reinicia automaticamente os pods com falhas.

✅ Configurações isoladas: variáveis de ambiente e secrets permitem separar configuração do código.

✅ Resiliência e alta disponibilidade: mesmo com falhas, o sistema se mantém operante.

> 💡 Nota: Com o uso de PVCs, mesmo que o banco de dados ou o WordPress seja reiniciado, os dados permanecem salvos — algo essencial para aplicações reais.

---

## 🧪 Testes Realizados (Completar)

### Acessar o Word_Press:

```bash
minikube service -n blog wordpress-svc
```

<img width="604" height="207" alt="image" src="https://github.com/user-attachments/assets/8e04a3f9-8ae0-4ced-8a8d-8f2cde7a167e" />

> Você envia (ou reaplica) o arquivo yaml com:
> ```bash
> kubectl apply -f wordpress-stack.yaml
> ```

-> **Rollout**
Processo do Kubernet's de aplicar atualizações de forma segura.
● Se você atualiza a imagem do container no Deployment,
● ou muda a quantidade de réplicas,
● o Kubernet's faz isso gradualmente, mantendo o serviço disponível.

> Como verificar se o rollout deu certo:
```bash
kubectl rollout status deployment/<nome>
```

> Esse comando junta vários recursos e mostra o estado atual de todos eles:
> ```bash
> kubectl get all -n blog
> ```

<img width="605" height="247" alt="image" src="https://github.com/user-attachments/assets/02c2764c-b6d0-4099-9a8e-737020a2c666" />


---

## ✅ Conclusão (Completar)

Fazer o deploy do WordPress com o banco MariaDB usando Kubernet´s mostra, na prática, como dois serviços podem funcionar juntos de forma organizada e eficiente. Com os volumes persistentes (PVCs), garantimos que os dados do site e do banco não sejam perdidos, mesmo se os pods forem reiniciados ou movidos.

Esse tipo de integração é muito útil em aplicações reais, principalmente quando precisamos de estabilidade, escalabilidade e facilidade para manter ou atualizar os serviços. Usar Kubernet´s dessa forma ajuda a deixar o ambiente mais controlado, flexível e preparado para crescer, sem complicação.

---

## 📸 Screenshots (Completar)


---

## ✍️ Autores

- **Felipe Parreiras e José Marconi**
- Curso: Engenharia de Computação - CEFET-MG

## 📚 Referências

- [Documentação oficial do Kubernetes](https://kubernetes.io/docs/tutorials/stateful-application/mysql-wordpress-persistent-volume/)
- [Documentação do WordPress Docker](https://hub.docker.com/_/wordpress)
- [Documentação do MariaDB Docker](https://mariadb.com/docs/server/server-management/install-and-upgrade-mariadb/installing-mariadb/binary-packages/automated-mariadb-deployment-and-administration/kubernetes-and-mariadb/kubernetes-operators-for-mariadb])
