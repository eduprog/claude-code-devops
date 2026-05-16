# Relatório do Cluster Kubernetes
**Data:** 16 de Maio de 2026  
**Cluster:** do-nyc1-k8s-aula  
**Provedor:** DigitalOcean (DOKS)  
**Região:** New York 1 (nyc1)  
**Versão Kubernetes:** v1.35.1

---

## Índice
1. [Inventário de Hardware](#1-inventário-de-hardware)
2. [Pods em Execução](#2-pods-em-execução)
3. [Aplicações em Execução](#3-aplicações-em-execução)
4. [Status de Saúde do Cluster e das Aplicações](#4-status-de-saúde-do-cluster-e-das-aplicações)
5. [Sugestões de Melhorias](#5-sugestões-de-melhorias)

---

## 1. Inventário de Hardware

### Nodes do Cluster

| Node | IP Interno | IP Externo | Status | Criado em |
|------|------------|------------|--------|-----------|
| pool-g1669fym6-33r5dp | 10.116.0.2 | 137.184.110.229 | Ready | 16/05/2026 19:51 |
| pool-g1669fym6-33r5ds | 10.116.0.3 | 64.227.7.210 | Ready | 16/05/2026 19:51 |

### Especificações por Node (idênticas)

| Recurso | Capacidade Total | Alocável |
|---------|-----------------|----------|
| CPU | 2 vCPUs | 1900m |
| Memória | ~2 GB (2.020.960 Ki) | ~1,5 GB (1.499.744 Ki) |
| Armazenamento Efêmero | ~80 GB (82.355.004 Ki) | ~72 GB |
| Máx. de Pods | 110 | 110 |

### Totais do Cluster

| Recurso | Total |
|---------|-------|
| Nodes | 2 |
| vCPUs | 4 |
| Memória RAM | ~4 GB |
| Armazenamento Efêmero | ~160 GB |
| Tipo de Instância | s-2vcpu-2gb (DigitalOcean) |

### Informações do Sistema

- **OS:** Debian GNU/Linux 13 (trixie)
- **Kernel:** 6.12.86+deb13-amd64
- **Arquitetura:** amd64
- **Container Runtime:** containerd 1.7.29
- **CNI:** Cilium (eBPF)

---

## 2. Pods em Execução

### Namespace: kube-news (Aplicação)

| Pod | Status | Node | Criado em |
|-----|--------|------|-----------|
| kube-news-7649dfcf6b-mdg6r | Running | pool-g1669fym6-33r5ds | 16/05/2026 20:54 |
| postgres-7d5bb967f7-stfzb | Running | pool-g1669fym6-33r5ds | 16/05/2026 20:54 |

### Namespace: kube-system (Sistema)

| Pod | Status | Node | Função |
|-----|--------|------|--------|
| cilium-59tqq | Running | pool-g1669fym6-33r5ds | CNI / Rede |
| cilium-84jtv | Running | pool-g1669fym6-33r5dp | CNI / Rede |
| coredns-b59ff8687-8jrcz | Running | pool-g1669fym6-33r5ds | DNS |
| coredns-b59ff8687-ftrtw | Running | pool-g1669fym6-33r5dp | DNS |
| cpc-bridge-proxy-ebpf-5h8kc | Running | pool-g1669fym6-33r5ds | Proxy eBPF |
| cpc-bridge-proxy-ebpf-c7567 | Running | pool-g1669fym6-33r5dp | Proxy eBPF |
| csi-do-node-xmt75 | Running | pool-g1669fym6-33r5ds | Storage CSI |
| csi-do-node-xt6vr | Running | pool-g1669fym6-33r5dp | Storage CSI |
| do-node-agent-mg6hj | Running | pool-g1669fym6-33r5dp | Agente DO |
| do-node-agent-t7s9x | Running | pool-g1669fym6-33r5ds | Agente DO |
| doks-telemetry-config-reloader-s6klh | Running | pool-g1669fym6-33r5ds | Telemetria |
| doks-telemetry-config-reloader-wz7sp | Running | pool-g1669fym6-33r5dp | Telemetria |
| hubble-relay-586f444678-rxvxt | Running | pool-g1669fym6-33r5ds | Observabilidade Cilium |
| hubble-ui-6b7f58bdd5-kfxwk | Running | pool-g1669fym6-33r5dp | UI Observabilidade |
| konnectivity-agent-54d6c8bf4-4xw5p | Running | pool-g1669fym6-33r5ds | Conectividade API |
| konnectivity-agent-54d6c8bf4-nqnwk | Running | pool-g1669fym6-33r5dp | Conectividade API |

**Total de Pods:** 18 (2 de aplicação + 16 de sistema)  
**Pods com problema:** 0

---

## 3. Aplicações em Execução

### Namespace: kube-news

#### Deployments

| Deployment | Réplicas | Status |
|------------|----------|--------|
| kube-news | 1/1 | Saudável |
| postgres | 1/1 | Saudável |

#### Services

| Service | Tipo | Finalidade |
|---------|------|-----------|
| kube-news | **LoadBalancer** | Acesso externo à aplicação web |
| postgres | ClusterIP | Comunicação interna com o banco de dados |

### Arquitetura da Aplicação

```
Internet
    │
    ▼
[LoadBalancer]
    │
    ▼
[Service: kube-news]
    │
    ▼
[Pod: kube-news] ──► [Service: postgres (ClusterIP)] ──► [Pod: postgres]
```

### Namespaces do Cluster

| Namespace | Status | Finalidade |
|-----------|--------|-----------|
| default | Active | Namespace padrão |
| kube-news | Active | Aplicação kube-news |
| kube-node-lease | Active | Heartbeat dos nodes |
| kube-public | Active | Recursos públicos do cluster |
| kube-system | Active | Componentes do sistema |

---

## 4. Status de Saúde do Cluster e das Aplicações

### Saúde dos Nodes

| Condição | Node 1 (33r5dp) | Node 2 (33r5ds) |
|----------|-----------------|-----------------|
| Ready | ✅ True | ✅ True |
| MemoryPressure | ✅ False | ✅ False |
| DiskPressure | ✅ False | ✅ False |
| PIDPressure | ✅ False | ✅ False |
| NetworkUnavailable | ✅ False (Cilium OK) | ✅ False (Cilium OK) |

### Utilização de Recursos por Node

| Recurso | Node 1 (33r5dp) | Node 2 (33r5ds) |
|---------|-----------------|-----------------|
| CPU Requests | 602m / 1900m (31%) | 602m / 1900m (31%) |
| CPU Limits | 1000m / 1900m (52%) | 1000m / 1900m (52%) |
| Memória Requests | 625Mi / 1499Mi (42%) | 625Mi / 1499Mi (42%) |
| Memória Limits | 1494Mi / 1499Mi (**102%**) | 1494Mi / 1499Mi (**102%**) |

> ⚠️ **Atenção:** Os limites de memória estão **overcommitted (102%)** nos dois nodes. Isso significa que, se todos os pods consumirem seus limites máximos simultaneamente, o node pode ficar sem memória e acionar o OOM Killer.

### Saúde das Aplicações

| Componente | Réplicas | Status | Requests definidos | Limits definidos |
|------------|----------|--------|--------------------|-----------------|
| kube-news | 1/1 | ✅ Running | ❌ Não | ❌ Não |
| postgres | 1/1 | ✅ Running | ❌ Não | ❌ Não |

### Status Geral

| Área | Status |
|------|--------|
| Nodes | ✅ Todos saudáveis |
| Rede (Cilium) | ✅ Operacional |
| DNS (CoreDNS) | ✅ Operacional (2 réplicas) |
| Aplicação kube-news | ✅ Em execução |
| Banco de dados postgres | ✅ Em execução |
| Pods com falha | ✅ Nenhum |

---

## 5. Sugestões de Melhorias

### 🔴 Alta Prioridade

#### 1. Definir Resource Requests e Limits nos Pods da Aplicação
Os pods `kube-news` e `postgres` estão sem `requests` e `limits` de CPU e memória. Isso causa:
- Dificuldade do scheduler em distribuir pods adequadamente
- Risco de um pod consumir todos os recursos do node (noisy neighbor)
- Memória já overcommitted nos nodes

**Ação:** Adicionar ao manifesto do Deployment:
```yaml
resources:
  requests:
    cpu: "250m"
    memory: "256Mi"
  limits:
    cpu: "500m"
    memory: "512Mi"
```

#### 2. Resolver o Overcommit de Memória
Os limits de memória nos dois nodes estão em **102%** da capacidade alocável. Isso é crítico.

**Ação:** Aumentar o tipo de instância para `s-2vcpu-4gb` ou `s-4vcpu-8gb`, ou adicionar um terceiro node ao pool.

---

### 🟡 Média Prioridade

#### 3. Aumentar Réplicas da Aplicação para Alta Disponibilidade
Atualmente `kube-news` tem apenas **1 réplica**. Se o pod falhar, a aplicação fica indisponível.

**Ação:** Escalar para pelo menos 2 réplicas e distribuir entre os dois nodes:
```yaml
replicas: 2
```

#### 4. Adicionar Liveness e Readiness Probes
Nenhum pod da aplicação possui health checks configurados. O Kubernetes não consegue detectar automaticamente se a aplicação travou.

**Ação:** Adicionar ao container `kube-news`:
```yaml
livenessProbe:
  httpGet:
    path: /
    port: 8080
  initialDelaySeconds: 30
  periodSeconds: 10
readinessProbe:
  httpGet:
    path: /
    port: 8080
  initialDelaySeconds: 10
  periodSeconds: 5
```

#### 5. Usar PersistentVolume para o PostgreSQL
O banco de dados está usando armazenamento efêmero do pod. Se o pod for reiniciado, **os dados são perdidos**.

**Ação:** Criar um `PersistentVolumeClaim` e montá-lo no container do postgres.

#### 6. Guardar Credenciais do Banco em Secrets
Verificar se as credenciais do PostgreSQL estão em variáveis de ambiente no manifesto. Caso estejam em texto claro, mover para `Kubernetes Secrets`:
```yaml
env:
  - name: POSTGRES_PASSWORD
    valueFrom:
      secretKeyRef:
        name: postgres-secret
        key: password
```

---

### 🟢 Baixa Prioridade

#### 7. Todos os Pods no Mesmo Node (33r5ds)
Os pods `kube-news` e `postgres` estão ambos no node `pool-g1669fym6-33r5ds`. Se esse node falhar, a aplicação fica totalmente indisponível.

**Ação:** Usar `podAntiAffinity` para garantir distribuição entre nodes:
```yaml
affinity:
  podAntiAffinity:
    preferredDuringSchedulingIgnoredDuringExecution:
      - weight: 100
        podAffinityTerm:
          labelSelector:
            matchLabels:
              app: kube-news
          topologyKey: kubernetes.io/hostname
```

#### 8. Configurar HorizontalPodAutoscaler (HPA)
Com métricas de CPU/memória, o HPA pode escalar automaticamente os pods da aplicação sob carga.

#### 9. Configurar NetworkPolicy
Atualmente não há políticas de rede isolando os pods. O pod `kube-news` poderia se comunicar com qualquer outro pod no cluster. Adicionar `NetworkPolicy` para restringir o tráfego apenas ao necessário.

#### 10. Ativar o Hubble UI para Observabilidade
O cluster já possui **Hubble** (observabilidade do Cilium) instalado, mas o acesso à UI não está exposto externamente. Aproveitar essa ferramenta para monitorar fluxos de rede.

---

## Resumo Executivo

| Indicador | Valor |
|-----------|-------|
| Nodes ativos | 2/2 ✅ |
| Pods saudáveis | 18/18 ✅ |
| Deployments saudáveis | 6/6 ✅ |
| Overcommit de memória | ⚠️ 102% |
| Resource limits na app | ❌ Não configurados |
| Alta disponibilidade da app | ❌ 1 réplica apenas |
| Persistência de dados | ⚠️ Verificar PVC |
| Health checks | ❌ Não configurados |
