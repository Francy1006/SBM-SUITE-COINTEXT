# ☸️ DevOps & Platform Engineering

> Estrategia de DevOps y Platform Engineering para SBM Suite.
> 
> 
> Esta página define cómo automatizar, desplegar, operar y escalar la plataforma desde entornos locales con Docker Compose hasta una arquitectura Kubernetes reproducible, observable y segura.
> 
> El objetivo no es incorporar herramientas por apariencia, sino construir una plataforma técnica coherente que pueda demostrarse en un portafolio profesional.
> 

---

# 1. Objetivo

Diseñar una plataforma de desarrollo y operación que permita:

- estandarizar entornos;
- automatizar integración y despliegue;
- reducir errores manuales;
- versionar infraestructura;
- desplegar servicios de forma reproducible;
- incorporar Kubernetes;
- gestionar configuraciones y secretos;
- soportar múltiples aplicaciones;
- mejorar confiabilidad;
- integrar QA, seguridad y observabilidad;
- generar evidencia práctica de Platform Engineering.

---

# 2. Alcance

La estrategia cubre:

- `sbm-manager`;
- `sbm-api`;
- `dp-api`;
- `sbm-ai-assistant`;
- `sbm-db`;
- `sbm-comercial`;
- `sbm-digital-api`;
- `dp-store`;
- NGINX;
- Certbot;
- Redis;
- Celery;
- Kafka;
- Qdrant;
- PostgreSQL;
- Jenkins;
- Azure DevOps;
- GitHub Actions;
- Docker;
- Kubernetes;
- Helm;
- observabilidad;
- seguridad;
- entornos locales y productivos.

---

# 3. Estado actual

Actualmente SBM Suite utiliza:

- Docker;
- Docker Compose;
- contenedores independientes;
- NGINX;
- Certbot;
- Jenkins en producción;
- PostgreSQL en contenedor independiente;
- aplicaciones separadas por repositorio;
- configuración mediante variables de entorno;
- despliegues parcialmente automatizados.

## Contenedores principales actuales

```
sbm-manager
sbm-api
dp-api
sbm-db
sbm-ai-assistant
qdrant
nginx-proxy
cert-bot
```

## Limitaciones actuales

- cada proyecto mantiene su propio Compose;
- falta una visión de plataforma consolidada;
- falta estandarización de pipelines;
- falta observabilidad transversal;
- falta estrategia Kubernetes;
- falta gestión centralizada de secretos;
- falta definición formal de ambientes;
- falta automatización homogénea de QA y seguridad.

---

# 4. Principios

1. **Everything as Code**
    
    Configuración, infraestructura, pipelines y despliegues deben versionarse.
    
2. **Reproducibility**
    
    Cualquier entorno debe poder recrearse.
    
3. **Automation-first**
    
    Los procesos repetitivos deben automatizarse.
    
4. **Immutable artifacts**
    
    Las imágenes y artefactos no deben cambiar después de construirse.
    
5. **Separation of concerns**
    
    Cada servicio mantiene una responsabilidad clara.
    
6. **Environment parity**
    
    Desarrollo, test y producción deben ser coherentes.
    
7. **Observability by default**
    
    Todo servicio debe exponer health, logs y métricas.
    
8. **Security by default**
    
    Los pipelines deben incorporar controles DevSecOps.
    
9. **Progressive complexity**
    
    Se avanza desde Compose hacia Kubernetes sin romper el desarrollo actual.
    
10. **Portfolio evidence**
    
    Cada etapa debe generar demostraciones visibles.
    

---

# 5. Modelo de plataforma

```
Developer
   ↓
Git Repository
   ↓
Pull Request
   ↓
CI Pipeline
   ├── Lint
   ├── Tests
   ├── Coverage
   ├── Security Scan
   └── Build
   ↓
Container Registry
   ↓
Deployment Pipeline
   ↓
Docker Compose / Kubernetes
   ↓
Observability
   ↓
Feedback
```

---

# 6. Estrategia de repositorios

## Repositorios actuales y futuros

- `sbm-manager`;
- `sbm-api`;
- `dp-api`;
- `sbm-db`;
- `sbm-ai-assistant`;
- `sbm-comercial`;
- `sbm-digital-api`;
- `dp-store`;
- `nginx-proxy`;
- `cert-bot`;
- repositorio futuro de infraestructura;
- repositorio futuro de Helm charts.

## Recomendación

Mantener repositorios separados por responsabilidad y agregar repositorios transversales para:

- infraestructura;
- despliegues;
- charts;
- observabilidad;
- documentación de plataforma.

---

# 7. Docker

## Objetivo

Mantener Docker como base de desarrollo y empaquetado.

## Buenas prácticas

- imágenes pequeñas;
- multi-stage builds;
- usuario no root;
- versiones fijas;
- `.dockerignore`;
- health checks;
- variables externas;
- logs por stdout/stderr;
- sin secretos en imágenes;
- capas optimizadas;
- escaneo con Trivy.

## Estándar mínimo por servicio

```
Dockerfile
.dockerignore
health endpoint
environment example
README
resource requirements
build command
run command
security scan
```

---

# 8. Docker Compose

## Rol

Docker Compose seguirá siendo la herramienta principal para desarrollo local.

## Estrategia

No es obligatorio eliminar los Compose individuales.

Se recomienda:

1. mantener Compose por repositorio;
2. crear un Compose transversal para levantar la suite completa;
3. permitir perfiles;
4. evitar duplicar configuraciones;
5. compartir redes;
6. centralizar servicios comunes.

## Estructura objetivo

```
infrastructure/
├── compose/
│   ├── docker-compose.base.yml
│   ├── docker-compose.dev.yml
│   ├── docker-compose.ai.yml
│   ├── docker-compose.observability.yml
│   └── docker-compose.full.yml
```

## Perfiles sugeridos

- core;
- frontend;
- ai;
- data;
- messaging;
- observability;
- full.

---

# 9. Entornos

| Environment | Propósito |
| --- | --- |
| Local | Desarrollo individual |
| Test | Pruebas automatizadas |
| Integration | Validar servicios reales |
| Staging | Validación previa a producción |
| Production | Operación real |

## Requisitos

- configuraciones separadas;
- secretos separados;
- dominios separados;
- logs;
- backups;
- recursos definidos;
- pipelines diferenciados;
- datos controlados.

---

# 10. CI/CD Strategy

## CI

Debe ejecutar:

- lint;
- format check;
- unit tests;
- integration tests;
- coverage;
- SonarQube;
- SAST;
- dependency scan;
- secret scan;
- container build;
- image scan;
- SBOM.

## CD

Debe ejecutar:

- validación de artefactos;
- migraciones;
- despliegue;
- smoke tests;
- health checks;
- rollback;
- reporte de release.

---

# 11. Jenkins

## Rol

Jenkins seguirá siendo una plataforma importante para demostrar experiencia DevOps.

## Uso actual y futuro

- pipelines de producción;
- builds;
- tests;
- análisis;
- publicación de imágenes;
- despliegues;
- integración con Kubernetes.

## Mejoras planificadas

- Jenkinsfile por repositorio;
- shared libraries;
- agentes Docker;
- agentes Kubernetes;
- credenciales seguras;
- stages estandarizados;
- reportes;
- rollback;
- integración con SonarQube;
- integración con Trivy.

---

# 12. Azure DevOps

## Rol

Azure DevOps será el centro técnico empresarial.

## Componentes

- Azure Boards;
- Azure Repos;
- Azure Pipelines;
- Azure Wiki;
- dashboards;
- artifacts como opción futura.

## Estrategia

- GitHub se mantiene como vitrina pública;
- Azure DevOps se utiliza para gestión empresarial;
- Azure Pipelines se integra gradualmente;
- un Self-Hosted Agent ejecuta pipelines localmente.

## Casos

- backlog técnico;
- PR;
- pipelines;
- releases;
- documentación;
- métricas;
- trazabilidad.

---

# 13. GitHub Actions

## Rol

Complemento para repositorios públicos.

## Casos

- tests;
- lint;
- security scans;
- build;
- publicación en GHCR;
- validación de README;
- demos públicas.

## Principio

No se deben mantener tres pipelines completamente distintos sin estandarización. Jenkins, Azure Pipelines y GitHub Actions deben reutilizar scripts comunes.

---

# 14. Scripts compartidos

## Objetivo

Evitar lógica duplicada entre plataformas CI/CD.

## Ejemplo

```
scripts/
├── lint.sh
├── test.sh
├── security.sh
├── build.sh
├── publish.sh
└── deploy.sh
```

## Beneficio

Los mismos scripts pueden ser invocados desde:

- Jenkins;
- Azure Pipelines;
- GitHub Actions;
- desarrollo local.

---

# 15. Container Registry

## Opciones

| Registry | Uso |
| --- | --- |
| GitHub Container Registry | Principal para portafolio |
| Azure Container Registry | Futuro enterprise |
| Docker Hub | Alternativa pública |
| Registry local | Desarrollo |

## Reglas

- tags inmutables;
- versionado semántico;
- tag por commit;
- escaneo;
- limpieza;
- control de acceso;
- SBOM.

---

# 16. Kubernetes

## Objetivo

Incorporar Kubernetes de forma realista y progresiva.

## Uso inicial

- despliegue local;
- demostración profesional;
- pruebas de escalado;
- health checks;
- configuración;
- seguridad;
- observabilidad.

## Distribución elegida

- K3s;
- k3d.

## Justificación

- menor consumo;
- fácil creación de clúster;
- compatible con MacBook M2;
- suficiente para portafolio;
- permite Helm, ingress y observabilidad.

---

# 17. Arquitectura Kubernetes objetivo

```
Kubernetes Cluster
│
├── Namespace: sbm-core
│   ├── sbm-api
│   ├── dp-api
│   └── sbm-digital-api
│
├── Namespace: sbm-frontends
│   ├── sbm-manager
│   ├── sbm-comercial
│   └── dp-store
│
├── Namespace: sbm-ai
│   ├── sbm-ai-assistant
│   └── qdrant
│
├── Namespace: sbm-messaging
│   ├── redis
│   ├── celery workers
│   └── kafka
│
├── Namespace: observability
│   ├── prometheus
│   ├── grafana
│   ├── loki
│   └── tempo
│
└── Namespace: ingress
    ├── ingress-nginx
    └── cert-manager
```

---

# 18. Kubernetes Resources

| Resource | Uso |
| --- | --- |
| Deployment | Aplicaciones stateless |
| StatefulSet | Servicios con estado |
| Service | Networking interno |
| Ingress | Acceso externo |
| ConfigMap | Configuración |
| Secret | Datos sensibles |
| Job | Tareas únicas |
| CronJob | Tareas periódicas |
| PersistentVolumeClaim | Almacenamiento |
| HorizontalPodAutoscaler | Escalado |
| NetworkPolicy | Seguridad de red |
| ServiceAccount | Identidad de workload |

---

# 19. Helm

## Objetivo

Empaquetar y versionar despliegues Kubernetes.

## Estructura sugerida

```
charts/
├── sbm-api/
├── dp-api/
├── sbm-ai-assistant/
├── sbm-manager/
├── sbm-comercial/
└── sbm-platform/
```

## Values

- `values-local.yaml`;
- `values-test.yaml`;
- `values-staging.yaml`;
- `values-production.yaml`.

## Beneficios

- reutilización;
- configuración por entorno;
- versionado;
- rollback;
- releases consistentes.

---

# 20. Ingress and TLS

## Tecnologías

- ingress-nginx;
- cert-manager;
- Let’s Encrypt.

## Responsabilidades

- routing;
- TLS;
- dominios;
- subdominios;
- redirects;
- headers;
- rate limiting inicial;
- exposición controlada.

---

# 21. Configuración y secretos

## ConfigMaps

Para:

- URLs;
- flags;
- nombres de servicios;
- configuraciones no sensibles.

## Secrets

Para:

- claves API;
- credenciales;
- tokens;
- contraseñas;
- certificados.

## Herramientas

- Kubernetes Secrets;
- Doppler Free;
- Azure DevOps Variable Groups;
- GitHub Secrets;
- Jenkins Credentials.

---

# 22. Recursos y límites

Cada workload debe definir:

- CPU request;
- CPU limit;
- memory request;
- memory limit.

## Objetivo

- evitar saturación;
- mejorar scheduling;
- medir consumo;
- soportar autoscaling;
- controlar carga en hardware limitado.

---

# 23. Health Checks

## Liveness

Indica si el proceso debe reiniciarse.

## Readiness

Indica si puede recibir tráfico.

## Startup

Permite tiempo inicial adicional.

## Requisitos

Cada servicio debe exponer endpoints claros y no depender de validaciones excesivamente costosas.

---

# 24. Autoscaling

## Opciones

- Horizontal Pod Autoscaler;
- escalado manual;
- métricas personalizadas;
- KEDA como investigación futura.

## Aplicaciones potenciales

- APIs;
- workers Celery;
- consumidores Kafka;
- `sbm-ai-assistant`.

## Regla

No escalar sin métricas y límites claros.

---

# 25. PostgreSQL

## Estrategia inicial

Mantener PostgreSQL fuera del clúster Kubernetes.

## Razones

- simplificar;
- reducir consumo;
- evitar complejidad de persistencia;
- facilitar backups;
- mantener estabilidad.

## Futuro

Evaluar:

- operador PostgreSQL;
- alta disponibilidad;
- almacenamiento persistente;
- backups automatizados;
- recuperación.

---

# 26. Redis y Celery

## Redis

- broker;
- caché;
- locks;
- estado temporal.

## Celery

- workers;
- tareas asíncronas;
- retries;
- procesos pesados.

## Celery Beat

- scheduler.

## Flower

- monitoreo.

## Kubernetes

- Deployment para workers;
- Deployment para Beat;
- Service para Flower;
- autoscaling futuro.

---

# 27. Kafka

## Arquitectura

- Kafka KRaft;
- Kafka UI;
- Schema Registry;
- productores;
- consumidores.

## Kubernetes

Primero puede ejecutarse fuera del clúster o en Compose.

Luego se evaluará:

- Strimzi;
- topics gestionados;
- operadores;
- observabilidad.

## Principio

Kafka se incorpora después de estabilizar Celery y las APIs.

---

# 28. Service Discovery

En Docker Compose:

- DNS interno por nombre de servicio.

En Kubernetes:

- Services;
- DNS interno;
- namespaces;
- nombres estables.

## Regla

No utilizar IPs fijas entre servicios.

---

# 29. Networking

## Docker

- redes internas;
- exposición mínima;
- NGINX como punto de entrada.

## Kubernetes

- Services;
- Ingress;
- Network Policies;
- namespaces;
- TLS;
- puertos mínimos.

---

# 30. Infrastructure as Code

## Herramientas

- Docker Compose;
- Helm;
- Kubernetes manifests;
- Terraform;
- scripts Bash;
- Azure Pipelines YAML;
- Jenkinsfile;
- GitHub Actions YAML.

## Terraform

Se utilizará posteriormente para:

- recursos Azure;
- infraestructura cloud;
- networking;
- storage;
- registries;
- servicios administrados.

---

# 31. Platform Engineering

## Objetivo

Crear una experiencia interna consistente para desarrollar y desplegar.

## Capacidades de plataforma

- plantillas de repositorio;
- pipelines reutilizables;
- imágenes base;
- scripts estándar;
- documentación;
- entornos reproducibles;
- observabilidad integrada;
- seguridad integrada;
- autoservicio controlado.

## Golden Path

```
Create Service
   ↓
Use Template
   ↓
Add Business Logic
   ↓
Run Local Stack
   ↓
Open Pull Request
   ↓
Automatic Quality and Security
   ↓
Build Image
   ↓
Deploy
   ↓
Observe
```

---

# 32. Developer Experience

## Objetivos

- onboarding rápido;
- comandos claros;
- documentación suficiente;
- entorno local estable;
- feedback rápido;
- herramientas consistentes.

## Comandos estándar futuros

```
make setup
make dev
make test
make lint
make security
make build
make deploy
```

## Herramientas posibles

- Makefile;
- Taskfile;
- scripts Bash;
- Dev Containers como opción futura.

---

# 33. Release Management

## Versionado

- Semantic Versioning;
- tags;
- changelog;
- release notes;
- commit SHA.

## Estrategias

- rolling deployment;
- blue-green como investigación;
- canary como investigación;
- rollback automático.

---

# 34. Database Migrations

## Reglas

- migraciones versionadas;
- prueba antes de release;
- backup cuando corresponda;
- compatibilidad;
- ejecución controlada;
- rollback documentado;
- bloqueo de migraciones destructivas no revisadas.

## Herramientas

- Flyway;
- Django migrations;
- pipelines;
- Testcontainers.

---

# 35. Rollback

## Debe contemplar

- rollback de aplicación;
- rollback de Helm release;
- rollback de configuración;
- rollback de imagen;
- rollback de migración cuando sea posible.

## Principio

Toda release debe saber cómo volver a un estado estable.

---

# 36. Observability Integration

Todo despliegue debe integrar:

- logs;
- métricas;
- trazas;
- health checks;
- alertas;
- dashboards;
- correlation IDs.

Las herramientas específicas se documentan en **Observability & Monitoring**.

---

# 37. DevSecOps Integration

Cada pipeline debe incorporar:

- SAST;
- dependency scanning;
- secret scanning;
- container scanning;
- IaC scanning;
- SBOM;
- DAST;
- gates.

Las herramientas específicas se documentan en **Security & DevSecOps**.

---

# 38. Cost Control

## Objetivo

Demostrar capacidades sin generar infraestructura cloud permanente innecesaria.

## Estrategia

- desarrollo local;
- k3d;
- self-hosted agents;
- herramientas open source;
- servicios cloud solo cuando aporten evidencia;
- detener recursos no utilizados;
- monitorear uso;
- presupuestos y alertas.

---

# 39. Roadmap de implementación

## Etapa 1 — Estandarización Docker

1. revisar Dockerfiles;
2. agregar multi-stage builds;
3. agregar health checks;
4. agregar usuarios no root;
5. revisar `.dockerignore`;
6. estandarizar variables;
7. crear Compose transversal.

## Etapa 2 — CI/CD

1. Jenkinsfile por repo;
2. Azure Pipelines;
3. GitHub Actions públicos;
4. scripts compartidos;
5. registry;
6. versionado;
7. reportes.

## Etapa 3 — Kubernetes local

1. instalar k3d;
2. crear clúster;
3. namespaces;
4. primer deployment;
5. services;
6. ingress;
7. ConfigMaps;
8. Secrets;
9. probes;
10. Helm.

## Etapa 4 — Servicios de plataforma

1. Redis;
2. Celery;
3. Flower;
4. Kafka;
5. Qdrant;
6. observabilidad.

## Etapa 5 — Seguridad y escalado

1. RBAC;
2. Network Policies;
3. resource limits;
4. autoscaling;
5. scans;
6. Falco;
7. disaster recovery.

---

# 40. Prioridad actual

## Urgente

1. estabilizar APIs;
2. implementar QA;
3. implementar seguridad;
4. documentar repositorios;
5. preparar Azure DevOps;
6. estandarizar Docker;
7. crear scripts comunes.

## Corto plazo

1. Compose transversal;
2. GHCR;
3. Jenkinsfiles;
4. Azure Pipelines;
5. Redis;
6. Celery;
7. Celery Beat.

## Mediano plazo

1. Kafka;
2. k3d;
3. Helm;
4. ingress;
5. cert-manager;
6. observabilidad;
7. autoscaling.

## Largo plazo

1. Terraform;
2. cloud híbrida;
3. operadores;
4. alta disponibilidad;
5. canary deployments;
6. internal developer platform más completa.

---

# 41. Evidencia para portafolio

## Entregables

- diagramas de plataforma;
- Dockerfiles optimizados;
- Compose transversal;
- Jenkins pipelines;
- Azure Pipelines;
- GitHub Actions;
- imágenes publicadas;
- Helm charts;
- clúster k3d;
- ingress;
- dashboards;
- security reports;
- runbooks;
- demos en video.

---

# 42. Criterio de finalización

Una capacidad DevOps se considera implementada cuando:

1. está versionada;
2. puede reproducirse;
3. tiene documentación;
4. tiene validación;
5. integra seguridad;
6. integra observabilidad;
7. permite rollback;
8. tiene evidencia;
9. funciona localmente;
10. puede migrarse a cloud cuando corresponda.

---

# 43. Visión final

```
Source Code
    +
Automated Quality
    +
Automated Security
    +
Immutable Containers
    +
Kubernetes Platform
    +
Observability
    +
Controlled Releases
```

SBM Suite debe evolucionar hacia una plataforma reproducible, segura, observable y automatizada, capaz de demostrar conocimientos reales de DevOps y Platform Engineering sin depender inicialmente de infraestructura cloud costosa.