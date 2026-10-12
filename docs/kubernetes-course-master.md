# Curso Completo de Kubernetes — Do Básico ao Avançado com .NET 10

## 1. Objetivo deste arquivo

Este arquivo é o **documento mestre** do curso de Kubernetes.

Ele deve ser usado como contexto em novos chats. Cada parte do curso deve, preferencialmente, ser estudada em um chat separado.

Ao iniciar um novo chat:

1. Anexe este arquivo.
2. Peça ao ChatGPT para ler o documento.
3. Informe qual **Parte/Fase** deseja estudar.
4. O ChatGPT deverá gerar somente o conteúdo da fase solicitada, seguindo todas as regras definidas neste documento.
5. O arquivo com o conteúdo deverá ser gerado na pasta /docs.

Exemplo de solicitação:

> Leia o arquivo kubernetes-course-master.md anexado e gere a **Parte 4 — Services e Service Discovery** completa, seguindo todas as regras e a estrutura de aula definidas no documento.

---

# 2. Perfil do curso

O curso deve partir do pressuposto de que o aluno:

- é iniciante em Kubernetes;
- já possui conhecimentos de desenvolvimento de software;
- utiliza C# e .NET;
- deseja aprender Kubernetes desde os fundamentos até tópicos avançados;
- deseja compreender o funcionamento interno do Kubernetes, e não apenas decorar comandos;
- deseja aprender práticas próximas de ambientes reais de produção.

O objetivo final é permitir que o aluno consiga:

- compreender a arquitetura do Kubernetes;
- criar, configurar e operar aplicações em Kubernetes;
- diagnosticar problemas;
- entender networking, storage, scheduling, observabilidade e segurança;
- trabalhar com autoscaling;
- utilizar Helm;
- implementar CI/CD e GitOps;
- compreender Kubernetes gerenciado em cloud;
- entender internals do Kubernetes;
- trabalhar com CRDs e Operators;
- projetar e operar aplicações .NET em Kubernetes com boas práticas de produção.

---

# 3. Ambiente padrão do curso

Todo o curso deve considerar o seguinte ambiente como padrão.

## Sistema operacional

Windows com:

- WSL2;
- Ubuntu.

Sempre que possível, os comandos devem ser fornecidos para execução dentro do Ubuntu no WSL2.

Preferir:

```bash
bash
kubectl
docker
k3d
helm
```

Evitar PowerShell e CMD, exceto quando alguma operação precisar obrigatoriamente ser executada no Windows.

Quando isso ocorrer, explicar claramente que o comando deve ser executado fora do WSL.

---

# 4. Stack utilizada

## Desenvolvimento

- .NET 10
- ASP.NET Core
- C#
- BackgroundService / Worker Service
- Entity Framework Core quando necessário
- Npgsql
- StackExchange.Redis
- OpenTelemetry

## Infraestrutura local

- Docker
- Docker Compose
- k3d
- Kubernetes
- kubectl
- Helm

## Componentes que podem ser adicionados durante o curso

- PostgreSQL
- Redis
- RabbitMQ ou AWS SQS
- Prometheus
- Grafana
- OpenTelemetry Collector
- Loki
- Tempo
- Jaeger
- Argo CD
- KEDA

---

# 5. Regra para exemplos de aplicação

Sempre que algum exemplo de aplicação for necessário, utilizar **.NET 10**.

Não substituir os exemplos por:

- Node.js;
- Python;
- Java;
- Go;
- PHP;
- outras linguagens.

Ferramentas escritas em outras linguagens podem ser utilizadas como componentes da infraestrutura, mas o código desenvolvido durante o curso deve usar C# e .NET 10.

---

# 6. Projeto utilizado durante o curso

O curso deve utilizar um projeto contínuo que evolui junto com o aprendizado.

Estrutura inicial:

```text
k8s-learning/
├── src/
│   ├── Catalog.Api/
│   └── Catalog.Worker/
├── docker/
├── k8s/
├── helm/
└── README.md
```

Inicialmente, somente `Catalog.Api` precisa existir.

A arquitetura deve evoluir progressivamente até algo próximo de:

```text
                         Internet
                            │
                            ▼
                     Ingress / Gateway
                            │
                            ▼
                       Catalog.Api
                      /      |      \
                     /       |       \
                    ▼        ▼        ▼
               PostgreSQL   Redis    Queue
                                      │
                                      ▼
                               Catalog.Worker
```

Componentes devem ser adicionados apenas quando fizerem sentido dentro da fase estudada.

Não apresentar toda a arquitetura pronta no começo.

---

# 7. Metodologia obrigatória de cada aula

Toda Parte/Fase do curso deve ser transformada em uma aula completa.

A aula deve seguir, sempre que aplicável, a estrutura abaixo.

## 7.1 Objetivos da aula

Explicar claramente:

- o que será estudado;
- por que o conteúdo existe;
- quais problemas ele resolve;
- o que o aluno deverá compreender ao final.

## 7.2 Pré-requisitos

Listar conhecimentos anteriores necessários.

Quando a fase depender de algo ainda não estudado, fazer apenas uma introdução curta e deixar o aprofundamento para a fase correta.

## 7.3 Teoria

Explicar os conceitos partindo do zero.

Não assumir conhecimento prévio específico de Kubernetes.

## 7.4 Funcionamento interno

Explicar o que acontece internamente no Kubernetes.

Sempre que possível, apresentar fluxos como:

```text
kubectl
   ↓
API Server
   ↓
etcd
   ↓
Controller
   ↓
Scheduler
   ↓
kubelet
   ↓
container runtime
```

O objetivo é evitar aprendizado baseado apenas em memorização de comandos.

## 7.5 Arquitetura

Mostrar como os componentes se relacionam.

Usar diagramas ASCII quando forem úteis.

## 7.6 Exemplos reais de produção

Mostrar onde o recurso ou conceito aparece em sistemas reais.

Explicar:

- por que empresas usam;
- quando faz sentido;
- quando não faz sentido;
- problemas comuns.

## 7.7 Relação com .NET 10

Sempre que possível, relacionar o conceito com ASP.NET Core ou aplicações .NET.

Exemplos:

- Health Checks;
- BackgroundService;
- graceful shutdown;
- configuração;
- variáveis de ambiente;
- logs;
- métricas;
- tracing;
- conexão com banco;
- filas.

## 7.8 Hands-on

Cada fase deve possuir prática no cluster local com k3d.

Sempre explicar cada comando antes ou depois de executá-lo.

## 7.9 YAML

Quando YAML for utilizado:

- apresentar o arquivo completo;
- explicar cada seção;
- explicar propriedades importantes;
- explicar valores possíveis quando relevante;
- explicar o impacto da configuração.

Não simplesmente entregar YAML sem explicação.

## 7.10 Inspeção

Depois de criar um recurso, ensinar como verificá-lo.

Exemplos:

```bash
kubectl get
kubectl describe
kubectl logs
kubectl exec
```

## 7.11 Experimentos

Sempre que possível, modificar ou quebrar algo propositalmente para observar o comportamento do Kubernetes.

Exemplo:

- deletar manualmente um Pod;
- provocar CrashLoopBackOff;
- reduzir memória até ocorrer OOMKilled;
- remover um Service;
- quebrar uma readiness probe.

## 7.12 Troubleshooting

Cada fase deve possuir uma seção explicando:

- sintomas;
- possíveis causas;
- como investigar;
- comandos úteis;
- como interpretar resultados.

## 7.13 Erros comuns

Mostrar erros comuns de iniciantes e como evitá-los.

## 7.14 Boas práticas

Separar claramente:

- o que funciona para aprendizado;
- o que é recomendado para produção.

## 7.15 Exercícios

Cada aula deve possuir:

### Exercícios básicos

Fixação dos conceitos.

### Exercícios intermediários

Exigem alguma investigação.

### Desafio

Problema sem solução totalmente guiada.

## 7.16 Resumo

Apresentar os principais conceitos aprendidos.

## 7.17 Relação com as fases anteriores

Mostrar como o conteúdo atual se conecta ao que já foi estudado.

## 7.18 Próxima fase

Explicar brevemente como a fase atual prepara o aluno para a próxima.

---

# 8. Regra de profundidade

O curso deve priorizar compreensão.

Não responder apenas:

> Deployment mantém Pods rodando.

Explicar algo como:

```text
Deployment
   ↓
Deployment Controller
   ↓
ReplicaSet
   ↓
Pods
```

E também explicar como esse estado é observado e reconciliado pelo Kubernetes.

Sempre que possível responder às perguntas:

- Quem cria esse recurso?
- Quem observa esse recurso?
- Onde o estado é armazenado?
- Quem decide em qual Node executar?
- Quem executa o container?
- Como o Kubernetes percebe que algo mudou?
- O que acontece quando algo falha?

---

# 9. Progressão do curso

O curso deve seguir as partes abaixo.

Cada parte pode gerar um chat independente.

---

# Parte 0 — Preparação do ambiente

## Objetivo

Preparar todos os conceitos e ferramentas necessários antes de iniciar Kubernetes.

## Conteúdo

### Linux básico necessário

- filesystem;
- processos;
- portas;
- variáveis de ambiente;
- permissões;
- comandos fundamentais;
- curl;
- ps;
- top;
- grep;
- cat;
- less;
- chmod;
- networking básico.

### Containers

- o que é um container;
- diferença entre container e máquina virtual;
- namespaces;
- cgroups;
- filesystem;
- processo PID 1.

### Docker

- imagem;
- container;
- Dockerfile;
- layers;
- build;
- run;
- exec;
- logs;
- networks;
- volumes.

### Docker Compose

- services;
- networks;
- volumes;
- dependências.

### Registry

- Docker Hub;
- registries privados;
- tags;
- push;
- pull.

### Ferramentas Kubernetes

Instalar e validar:

- Docker;
- kubectl;
- k3d.

Criar um cluster local básico.

## Projeto

Criar:

```text
Catalog.Api
```

Endpoints iniciais sugeridos:

```text
GET /health
GET /products
POST /products
```

Criar Dockerfile e executar a API como container.

---

# Parte 1 — Arquitetura do Kubernetes

## Objetivo

Entender a arquitetura geral do Kubernetes e os componentes responsáveis por manter um cluster funcionando.

## Conteúdo

### Cluster

- conceito;
- finalidade;
- estado desejado.

### Node

- Worker Node;
- Control Plane Node.

### Control Plane

Estudar:

- API Server;
- etcd;
- Scheduler;
- Controller Manager;
- Cloud Controller Manager.

### Worker Node

Estudar:

- kubelet;
- kube-proxy;
- container runtime.

### Fluxo interno

Compreender:

```text
kubectl
   ↓
API Server
   ↓
etcd
   ↓
Scheduler
   ↓
Node
   ↓
kubelet
   ↓
container runtime
```

### k3d e k3s

Explicar:

- diferença entre Kubernetes tradicional, k3s e k3d;
- como k3d executa k3s dentro de containers Docker;
- diferenças de implementação do datastore quando aplicável;
- limitações do laboratório local.

### Hands-on

Criar cluster:

```bash
k3d cluster create k8s-learning
```

Explorar:

```bash
kubectl cluster-info
kubectl get nodes
kubectl get pods -A
kubectl get namespaces
```

---

# Parte 2 — Pods

## Conteúdo

- Pod;
- container dentro de Pod;
- multi-container Pod;
- lifecycle;
- restartPolicy;
- Pod IP;
- armazenamento efêmero;
- comandos básicos;
- logs;
- exec;
- describe.

Estados importantes:

- Pending;
- Running;
- Succeeded;
- Failed;
- Unknown.

Erros:

- CrashLoopBackOff;
- ImagePullBackOff;
- ErrImagePull.

Criar o primeiro Pod executando `Catalog.Api`.

Explicar por que Pods normalmente não devem ser administrados diretamente em produção.

---

# Parte 3 — Deployments e ReplicaSets

## Conteúdo

- Deployment;
- ReplicaSet;
- replicas;
- desired state;
- reconciliation;
- rolling update;
- rollout;
- rollback;
- deployment strategies básicas.

Arquitetura:

```text
Deployment
   ↓
ReplicaSet
   ↓
Pod
Pod
Pod
```

Experimentos:

- aumentar replicas;
- diminuir replicas;
- deletar Pod manualmente;
- alterar imagem;
- acompanhar rollout;
- executar rollback.

---

# Parte 4 — Services e Service Discovery

## Conteúdo

- volatilidade do IP dos Pods;
- Service;
- selectors;
- ClusterIP;
- NodePort;
- LoadBalancer;
- ExternalName;
- EndpointSlice;
- DNS interno.

Arquitetura:

```text
Client
  ↓
Service
  ↓
Pod
Pod
Pod
```

DNS:

```text
catalog-api
catalog-api.default
catalog-api.default.svc.cluster.local
```

Adicionar comunicação entre componentes .NET quando apropriado.

---

# Parte 5 — ConfigMaps e Secrets

## Conteúdo

- configuração externa;
- ConfigMap;
- Secret;
- environment variables;
- volumes;
- configuração ASP.NET Core;
- reload;
- segurança.

Relacionar com:

```csharp
builder.Configuration
```

Explicar claramente que Secret não significa automaticamente armazenamento seguro ou criptografia forte.

---

# Parte 6 — Health Checks e Probes

## Conteúdo

- ASP.NET Core Health Checks;
- startupProbe;
- livenessProbe;
- readinessProbe;
- failureThreshold;
- periodSeconds;
- timeoutSeconds;
- initialDelaySeconds.

Criar:

```text
/health/live
/health/ready
```

Explicar a diferença conceitual:

```text
Startup:
a aplicação terminou de iniciar?

Liveness:
a aplicação continua saudável?

Readiness:
a aplicação pode receber tráfego?
```

Criar falhas propositalmente.

---

# Parte 7 — CPU, memória, Requests e Limits

## Conteúdo

- recursos computacionais;
- CPU;
- millicores;
- memória;
- requests;
- limits;
- QoS Classes;
- CPU throttling;
- OOMKilled;
- Scheduler.

Exemplo:

```yaml
resources:
  requests:
    cpu: 100m
    memory: 128Mi
  limits:
    cpu: 500m
    memory: 512Mi
```

---

# Parte 8 — Storage

## Conteúdo

- filesystem efêmero;
- Volume;
- PersistentVolume;
- PersistentVolumeClaim;
- StorageClass;
- dynamic provisioning;
- CSI.

Arquitetura:

```text
Pod
 ↓
PVC
 ↓
PV
 ↓
Storage
```

Adicionar PostgreSQL ao projeto.

Testar persistência após destruição e recriação de Pods.

---

# Parte 9 — StatefulSets e DaemonSets

## StatefulSet

Estudar:

- identidade estável;
- hostname;
- armazenamento estável;
- ordenação;
- Headless Services;
- diferenças para Deployment.

## DaemonSet

Estudar:

- um Pod por Node;
- agentes de logs;
- monitoramento;
- networking;
- segurança.

---

# Parte 10 — Jobs e CronJobs

## Conteúdo

- Job;
- CronJob;
- completions;
- parallelism;
- backoffLimit;
- restartPolicy;
- schedules.

Criar aplicação .NET específica para tarefas batch quando necessário.

Exemplo:

```text
Catalog.CleanupWorker
```

---

# Parte 11 — Ingress e Gateway API

## Conteúdo

- exposição HTTP;
- Ingress;
- Ingress Controller;
- host routing;
- path routing;
- TLS;
- certificados;
- Gateway API.

Arquitetura:

```text
Internet
   ↓
Ingress Controller
   ↓
Ingress
   ↓
Service
   ↓
Pod
```

Comparar Ingress e Gateway API.

---

# Parte 12 — Namespaces e controle de recursos

## Conteúdo

- Namespace;
- isolamento lógico;
- nomes;
- ambientes;
- ResourceQuota;
- LimitRange.

Exemplos:

```text
development
staging
production
```

Discutir quando namespaces devem ou não representar ambientes.

---

# Parte 13 — Networking avançado

## Conteúdo

- modelo de rede Kubernetes;
- Pod-to-Pod;
- Pod-to-Service;
- Service-to-Pod;
- CNI;
- kube-proxy;
- iptables;
- IPVS;
- eBPF;
- DNS;
- NetworkPolicy.

Exemplos de regras:

```text
Catalog.Api → PostgreSQL permitido
Catalog.Worker → Redis permitido
outros acessos bloqueados
```

---

# Parte 14 — Scheduling avançado

## Conteúdo

- nodeSelector;
- labels de Node;
- Node Affinity;
- Pod Affinity;
- Pod Anti-Affinity;
- taints;
- tolerations;
- topology spread constraints.

Exemplo:

Distribuir réplicas da API entre Nodes diferentes.

---

# Parte 15 — Autoscaling

## Conteúdo

### HPA

- CPU;
- memória;
- métricas customizadas;
- comportamento de scale up e scale down.

### VPA

- recomendações;
- atualização de requests.

### Cluster Autoscaler

- crescimento e redução de Nodes.

### KEDA

Autoscaling orientado a eventos.

Exemplo:

```text
SQS
 ↓
KEDA
 ↓
Catalog.Worker replicas
```

---

# Parte 16 — Observabilidade

## Conteúdo

Três pilares:

- logs;
- métricas;
- tracing.

### .NET

- ILogger;
- logs estruturados;
- OpenTelemetry.

### Kubernetes

- métricas do cluster;
- métricas da aplicação.

### Ferramentas

- Prometheus;
- Grafana;
- OpenTelemetry Collector;
- Loki;
- Tempo;
- Jaeger quando apropriado.

Instrumentar `Catalog.Api` e `Catalog.Worker`.

---

# Parte 17 — Helm

## Conteúdo

- Chart;
- Release;
- Repository;
- Values;
- Templates;
- functions;
- helpers;
- dependencies.

Estrutura:

```text
helm/
└── catalog/
    ├── Chart.yaml
    ├── values.yaml
    └── templates/
```

Criar configurações diferentes:

```text
values-development.yaml
values-production.yaml
```

---

# Parte 18 — Segurança

## Conteúdo

### Identidade

- ServiceAccount.

### Autorização

- RBAC;
- Role;
- ClusterRole;
- RoleBinding;
- ClusterRoleBinding.

### Segurança de containers

- SecurityContext;
- runAsNonRoot;
- readOnlyRootFilesystem;
- capabilities;
- seccomp.

### Kubernetes

- Pod Security Standards.

### Secrets

- Kubernetes Secrets;
- encryption at rest;
- External Secrets;
- Vault;
- AWS Secrets Manager.

### Supply Chain

- image scanning;
- SBOM;
- vulnerabilidades;
- assinatura de imagens.

---

# Parte 19 — Resiliência e alta disponibilidade

## Conteúdo

- PodDisruptionBudget;
- maxUnavailable;
- maxSurge;
- terminationGracePeriodSeconds;
- lifecycle hooks;
- preStop;
- graceful shutdown;
- SIGTERM.

Relacionar com .NET:

- CancellationToken;
- BackgroundService;
- IHostedService;
- shutdown do ASP.NET Core.

Criar testes desligando Pods durante requisições.

---

# Parte 20 — Estratégias avançadas de Deployment

## Conteúdo

- Recreate;
- Rolling Update;
- Blue/Green;
- Canary.

Exemplo:

```text
Catalog.Api v1
Catalog.Api v2
```

Simular atualização progressiva.

Discutir ferramentas adicionais quando necessário.

---

# Parte 21 — GitOps

## Conteúdo

Conceito:

```text
Git
 ↓
GitOps Controller
 ↓
Kubernetes
```

Ferramentas:

- Argo CD;
- Flux.

Dar preferência inicial ao Argo CD.

Estudar:

- desired state no Git;
- sync;
- drift;
- rollback;
- reconciliation;
- environments;
- promotion.

---

# Parte 22 — CI/CD

## Conteúdo

Criar fluxo semelhante a:

```text
Git Push
   ↓
dotnet restore
   ↓
dotnet build
   ↓
dotnet test
   ↓
docker build
   ↓
security scan
   ↓
push registry
   ↓
update manifests
   ↓
Argo CD
   ↓
Kubernetes
```

Pode utilizar:

- Azure DevOps;
- GitHub Actions.

Dar preferência a Azure DevOps quando for necessário escolher apenas um.

---

# Parte 23 — Kubernetes gerenciado em Cloud

## Conteúdo

Comparar:

- EKS;
- AKS;
- GKE.

Dar atenção especial a EKS e AKS.

Estudar:

- Managed Control Plane;
- Node Groups;
- IAM;
- identities;
- cloud load balancers;
- cloud storage;
- CSI;
- Cloud Controller;
- autoscaling;
- custos;
- upgrades.

Comparar com k3d/k3s.

---

# Parte 24 — Troubleshooting avançado

## Objetivo

Desenvolver uma metodologia de investigação.

Problemas:

- CrashLoopBackOff;
- ImagePullBackOff;
- OOMKilled;
- Pending;
- Evicted;
- Node NotReady;
- Service sem Endpoint;
- DNS;
- Ingress;
- PVC Pending;
- probes;
- networking;
- CPU;
- memória.

Metodologia sugerida:

```text
kubectl get
   ↓
kubectl describe
   ↓
kubectl logs
   ↓
kubectl events
   ↓
kubectl exec
   ↓
métricas
```

Ferramentas adicionais:

- kubectl top;
- stern;
- k9s.

---

# Parte 25 — Kubernetes Internals

## Objetivo

Revisitar a arquitetura inicial com profundidade.

## Conteúdo

- API Server;
- etcd;
- Scheduler;
- Controller Manager;
- kubelet;
- declarative API;
- desired state;
- reconciliation loop;
- Watch API;
- List/Watch;
- controllers;
- informers;
- shared caches;
- Admission Controllers;
- Admission Webhooks;
- mutating admission;
- validating admission;
- finalizers;
- ownerReferences;
- garbage collection;
- resourceVersion;
- optimistic concurrency.

Explicar Kubernetes como um sistema composto por control loops.

---

# Parte 26 — CRDs e Operators

## Conteúdo

- extensibilidade da API;
- CustomResourceDefinition;
- Custom Resource;
- controller;
- reconciliation;
- Operator Pattern.

Exemplo:

```yaml
apiVersion: learning.mauricio.dev/v1
kind: DotnetApi
metadata:
  name: catalog
spec:
  replicas: 3
  image: catalog-api:1.0
```

Construir progressivamente um controller para reconciliar esse recurso.

Relacionar profundamente com a Parte 25.

---

# Parte 27 — Projeto final

## Objetivo

Consolidar todo o curso em uma arquitetura próxima de produção.

Arquitetura conceitual:

```text
                          Internet
                             │
                             ▼
                      Load Balancer
                             │
                             ▼
                     Ingress/Gateway
                             │
                             ▼
                       Catalog.Api
                      /      |      \
                     /       |       \
                    ▼        ▼        ▼
             PostgreSQL    Redis     Queue
                                       │
                                       ▼
                               Catalog.Worker
```

O projeto deve utilizar, quando apropriado:

- Deployments;
- Services;
- ConfigMaps;
- Secrets;
- Health Checks;
- Requests;
- Limits;
- HPA;
- KEDA;
- PersistentVolumes;
- PersistentVolumeClaims;
- NetworkPolicies;
- Ingress ou Gateway API;
- TLS;
- OpenTelemetry;
- Prometheus;
- Grafana;
- Helm;
- RBAC;
- Pod Security;
- PodDisruptionBudget;
- graceful shutdown;
- GitOps;
- Argo CD;
- CI/CD.

O projeto final deve incluir situações de troubleshooting e simulação de falhas.

---

# 10. Regra para criação de novos chats

Cada Parte/Fase deve, preferencialmente, ser estudada em um chat independente.

Exemplo:

```text
Chat 01 → Parte 0
Chat 02 → Parte 1
Chat 03 → Parte 2
...
Chat 28 → Parte 27
```

Isso permite que cada chat seja aprofundado sem misturar contextos demais.

---

# 11. Prompt padrão para gerar uma fase

Utilizar o seguinte prompt ao iniciar um novo chat:

> Leia integralmente o arquivo `kubernetes-course-master.md` anexado.
>
> Quero estudar agora a **Parte X — NOME DA PARTE**.
>
> Gere uma aula completa somente sobre essa parte.
>
> Siga todas as regras definidas no documento, principalmente:
>
> - considere Windows + WSL2 + Ubuntu;
> - utilize Bash/Linux por padrão;
> - utilize k3d como cluster local;
> - utilize .NET 10 para todos os exemplos de código de aplicação;
> - explique os conceitos partindo do zero;
> - explique funcionamento interno do Kubernetes;
> - explique cada comando utilizado;
> - explique os YAMLs propriedade por propriedade;
> - inclua teoria, arquitetura, exemplos reais, hands-on, troubleshooting, erros comuns, boas práticas e exercícios;
> - relacione o conteúdo com o projeto `k8s-learning`;
> - não avance para a próxima fase;
> - não omita assuntos definidos para esta parte no arquivo.
>
> Quero compreender o assunto, não apenas copiar comandos.

---

# 12. Prompt para continuação de uma fase

Quando uma fase ficar grande demais para uma única resposta:

> Continue a Parte X exatamente do ponto em que paramos. Não reinicie a aula e não repita conteúdo já explicado. Continue seguindo o arquivo mestre do curso.

---

# 13. Prompt para tirar dúvidas durante uma fase

Durante a aula:

> Minha dúvida está dentro da Parte X. Responda considerando o contexto da aula atual e depois continue tratando essa parte como o assunto principal do chat.

---

# 14. Regra para não antecipar conteúdo

Conceitos futuros podem ser mencionados quando forem necessários para compreensão, mas não devem ser aprofundados antes da fase correspondente.

Exemplo:

Durante Services, pode mencionar Ingress.

Porém, o estudo detalhado de Ingress deve permanecer para a Parte 11.

Da mesma forma, reconciliation pode ser explicado de forma simples nas primeiras fases, mas o aprofundamento de controllers, watches e informers deve permanecer para a Parte 25.

---

# 15. Regra para versões das ferramentas

Kubernetes, k3d, kubectl, Helm, .NET e outras ferramentas evoluem.

Ao gerar uma nova fase, se houver possibilidade de comandos, versões ou recomendações terem mudado, o ChatGPT deve verificar a documentação atual antes de apresentar instruções dependentes de versão.

A aula deve indicar quando determinado comportamento depende da versão utilizada.

---

# 16. Resultado esperado ao final do curso

Ao concluir todas as partes, o aluno deve conseguir explicar e operar um fluxo semelhante a:

```text
Developer
    │
    ▼
Git
    │
    ▼
CI Pipeline
    │
    ├── build
    ├── tests
    ├── container image
    └── security scan
    │
    ▼
Container Registry
    │
    ▼
GitOps Repository
    │
    ▼
Argo CD
    │
    ▼
Kubernetes API Server
    │
    ▼
Controllers / Scheduler
    │
    ▼
Nodes
    │
    ▼
Pods
    │
    ├── ASP.NET Core APIs
    └── .NET Workers
    │
    ├── PostgreSQL
    ├── Redis
    └── Message Queue
```

E compreender não apenas **como configurar**, mas também **o que está acontecendo internamente em cada etapa**.

---

# 17. Princípio central do curso

Sempre priorizar:

```text
Entender
   ↓
Experimentar
   ↓
Quebrar
   ↓
Investigar
   ↓
Corrigir
   ↓
Aplicar boas práticas
```

O curso não deve ser tratado como uma coleção de YAMLs e comandos.

O objetivo é desenvolver capacidade de raciocinar sobre Kubernetes.
