# 🏦 Agentic SDLC Harness — Banking Account Opening

> **Demo de Spec-Driven Development (SDD) y Spec-Driven Architecture (SDA)**  
> usando **GitHub Spec Kit** + **GitHub Copilot Agents** para cubrir el ciclo de vida 
> completo de desarrollo (SDLC) en un caso de uso bancario.

---

## 📋 Tabla de Contenidos

- [¿Qué es este demo?](#-qué-es-este-demo)
- [Arquitectura del Harness](#-arquitectura-del-harness)
- [Estructura del Proyecto](#-estructura-del-proyecto)
- [Prerrequisitos](#-prerrequisitos)
- [Instalación desde Cero](#-instalación-desde-cero)
- [Uso: Flujo SDLC Completo](#-uso-flujo-sdlc-completo)
- [Agentes Disponibles](#-agentes-disponibles)
- [Skills / Prompts Disponibles](#-skills--prompts-disponibles)
- [Caso de Uso: Apertura de Cuenta de Ahorros](#-caso-de-uso-apertura-de-cuenta-de-ahorros)
- [Artefactos Generados](#-artefactos-generados)
- [Referencia de Comandos Spec Kit](#-referencia-de-comandos-spec-kit)

---

## 🎯 ¿Qué es este demo?

Este proyecto demuestra cómo **GitHub Copilot** puede actuar como un **harness agéntico** 
que orquesta el ciclo de vida completo del software (SDLC) guiado por especificaciones.

### Conceptos Clave

| Concepto | Descripción |
|---------|-------------|
| **SDD** (Spec-Driven Development) | El desarrollo se guía por un spec estructurado, no por prompts vagos |
| **SDA** (Spec-Driven Architecture) | La arquitectura técnica se deriva directamente del spec aprobado |
| **Spec Kit** | Framework oficial de GitHub para SDD — convierte specs en planes, tareas y código |
| **Agentes Locales** | Agentes Copilot especializados por dominio bancario y por fase SDLC |
| **Skills Locales** | Prompts reutilizables con conocimiento bancario (KYC, AML, PCI-DSS) |

### Caso de Uso Bancario

> **"Un cliente desea abrir una cuenta de ahorros digital"**

El harness toma esta necesidad de negocio y, paso a paso, produce:
- ✅ User stories y criterios de aceptación
- ✅ Arquitectura técnica trazable al spec
- ✅ Código Angular implementado
- ✅ Suite de pruebas completa
- ✅ Reporte de seguridad y compliance
- ✅ PR listo para revisión

---

## 🏗️ Arquitectura del Harness

```
NEGOCIO
  │  "Quiero que clientes abran cuenta de ahorros online"
  ▼
┌─────────────────────────────────────────────────────────────────┐
│                  AGENTIC SDLC HARNESS                           │
│                                                                 │
│  ┌──────────────┐    ┌─────────────────────────────────────┐   │
│  │ SPEC KIT     │    │     BANKING AGENTS (locales)        │   │
│  │              │    │                                     │   │
│  │ constitution │◄───│  banking-requirements-agent         │   │
│  │ specify      │    │  (Fase 1: Requisitos)               │   │
│  │ clarify      │    │                                     │   │
│  │              │    │  banking-architect-agent            │   │
│  │ plan         │◄───│  (Fase 2: Arquitectura SDA)         │   │
│  │ checklist    │    │                                     │   │
│  │ analyze      │    │  banking-developer-agent            │   │
│  │              │◄───│  (Fase 3: Desarrollo)               │   │
│  │ tasks        │    │                                     │   │
│  │ implement    │    │  banking-tester-agent               │   │
│  │              │◄───│  (Fase 4: Pruebas)                  │   │
│  │ converge     │    │                                     │   │
│  └──────────────┘    │  banking-security-agent             │   │
│         │            │  (Fase 5: Seguridad & Compliance)   │   │
│         │            └─────────────────────────────────────┘   │
│         │                                                       │
│         │  ┌──────────────────────────────────────────────┐    │
│         │  │     SKILLS / PROMPTS (locales)               │    │
│         │  │  banking-spec-writer      (enriquece specs)  │    │
│         │  │  banking-domain-rules     (KYC/AML/PCI-DSS)  │    │
│         │  │  sdlc-orchestrator        (control del flujo)│    │
│         │  └──────────────────────────────────────────────┘    │
│         │                                                       │
└─────────┼───────────────────────────────────────────────────────┘
          ▼
     PR LISTO ✅
  (spec + arquitectura + código + tests + reporte seguridad)
```

---

## 📁 Estructura del Proyecto

```
banking-account-opening/
│
├── 📂 .specify/                        ← Spec Kit: estado y plantillas
│   ├── memory/
│   │   └── constitution.md             ← Principios del proyecto (Security-First, KYC/AML)
│   ├── templates/                      ← Plantillas de spec, plan, tasks, checklist
│   ├── scripts/powershell/             ← Scripts CLI generados por spec-kit
│   ├── workflows/                      ← Definición de flujos de trabajo
│   └── init-options.json               ← Configuración de spec-kit
│
├── 📂 .github/
│   ├── agents/                         ← Agentes Copilot locales del harness
│   │   ├── speckit.*.agent.md          ← Agentes base de spec-kit (generados)
│   │   ├── banking-requirements-agent.md  ← 🏦 Fase 1: Requisitos bancarios
│   │   ├── banking-architect-agent.md     ← 🏛️  Fase 2: Arquitectura SDA
│   │   ├── banking-developer-agent.md     ← 💻 Fase 3: Desarrollo
│   │   ├── banking-tester-agent.md        ← 🧪 Fase 4: Pruebas
│   │   └── banking-security-agent.md      ← 🔒 Fase 5: Seguridad & Compliance
│   │
│   ├── prompts/                        ← Prompts base de spec-kit (generados, no modificar)
│   │   └── speckit.*.prompt.md
│   └── skills/                         ← Skills locales del harness (formato oficial)
│       ├── speckit-*/SKILL.md          ← Skills de spec-kit (generados automáticamente)
│       ├── banking-spec-writer/
│       │   └── SKILL.md               ← 📝 Enriquecedor de specs bancarios
│       ├── banking-domain-rules/
│       │   └── SKILL.md               ← 📚 Reglas KYC/AML/PCI-DSS/GDPR
│       └── sdlc-orchestrator/
│           └── SKILL.md               ← 🎛️  Control del flujo SDLC completo
│
├── 📂 specs/                           ← Artefactos SDD generados (por feature)
│   └── 001-savings-account-opening/    ← Ejemplo: apertura de cuenta
│       ├── spec.md                     ← Especificación funcional
│       ├── plan.md                     ← Plan de arquitectura técnica
│       ├── tasks.md                    ← Lista de tareas de implementación
│       ├── analysis.md                 ← Análisis de consistencia spec↔plan
│       ├── convergence.md              ← Verificación de completitud
│       ├── checklists/
│       │   ├── requirements.md         ← Checklist de calidad del spec
│       │   └── architecture.md         ← Checklist de arquitectura
│       ├── test-coverage-report.md     ← Reporte de cobertura de pruebas
│       ├── compliance-test-matrix.md   ← Matriz spec ↔ tests de compliance
│       ├── security-report.md          ← Reporte de seguridad
│       └── compliance-coverage.md      ← Cobertura regulatoria
│
└── 📂 src/                             ← Código Angular generado
    ├── app/
    │   └── features/
    │       └── account-opening/
    │           ├── components/         ← Componentes Angular
    │           ├── services/           ← Servicios con lógica bancaria
    │           └── models/             ← Interfaces y modelos TypeScript
    └── api/
        └── api.spec.yaml               ← Contrato OpenAPI 3.0
```

---

## ✅ Prerrequisitos

| Herramienta | Versión mínima | Para qué |
|------------|---------------|---------|
| **Python** | 3.10+ | Ejecutar spec-kit CLI |
| **pipx** | 1.0+ | Instalación persistente de spec-kit |
| **Git** | 2.0+ | Control de versiones (spec-kit lo requiere) |
| **GitHub Copilot** | Plan con Agents | Ejecutar los agentes locales |
| **VS Code** | Última | Editor con soporte para Copilot Chat |
| **Node.js** | 18+ | (Opcional) Para ejecutar el código Angular generado |

### Verificar prerrequisitos

```powershell
python --version     # Python 3.x.x
pipx --version       # 1.x.x
git --version        # git version 2.x.x
node --version       # v18.x.x (opcional)
```

---

## 🚀 Instalación desde Cero

### Paso 1: Clonar o crear el directorio del proyecto

```powershell
# Opción A: Clonar este repositorio
git clone <repo-url>
cd banking-account-opening

# Opción B: Crear desde cero
mkdir banking-account-opening
cd banking-account-opening
git init
git config user.email "tu@email.com"
git config user.name "Tu Nombre"
```

### Paso 2: Instalar GitHub Spec Kit

```powershell
# Instalación persistente (recomendada)
pipx install git+https://github.com/github/spec-kit.git

# Verificar instalación
specify --version
```

> **Nota**: Si `specify` no está en el PATH después de instalar, reinicia la terminal 
> o ejecuta `pipx ensurepath`.

### Paso 3: Inicializar Spec Kit en el proyecto

```powershell
cd banking-account-opening

# Inicializar con integración Copilot y scripts PowerShell
"y" | specify init . --script ps
```

Esto genera:
- `.specify/` — Estado y plantillas de spec-kit
- `.github/agents/` — Agentes Copilot base (speckit.*)
- `.github/prompts/` — Prompts base (speckit.*)

### Paso 4: Copiar los agentes y skills bancarios

Si estás instalando desde cero (no clonando este repo), copia manualmente los archivos 
bancarios a las carpetas correspondientes:

```
.github/agents/
├── banking-requirements-agent.md
├── banking-architect-agent.md
├── banking-developer-agent.md
├── banking-tester-agent.md
└── banking-security-agent.md

.github/prompts/
├── banking-spec-writer.prompt.md
├── banking-domain-rules.prompt.md
└── sdlc-orchestrator.prompt.md
```

### Paso 5: Verificar estructura

```powershell
# Verificar que todo está en su lugar
Get-ChildItem -Recurse .github -Name
Get-ChildItem -Recurse .specify/memory -Name
```

---

## 🔄 Uso: Flujo SDLC Completo

### Resumen del flujo

```
specify init   →   constitution   →   specify   →   clarify
                                                         │
                         converge   ←   security   ←   plan
                            │                            │
                         tests    ←   implement   ←   checklist
                                          │              │
                                        tasks   ←   analyze
```

---

### 1️⃣ Definir la Constitución (una vez por proyecto)

En **GitHub Copilot Chat** (VS Code), escribe:

```
/speckit-constitution This is a Security-First banking application for digital account 
opening. All features must comply with KYC, AML, and PCI-DSS regulations. TDD is 
mandatory with minimum 80% coverage. No PII may be logged. Spec is the single 
source of truth. OAuth 2.0 + MFA required for all account operations.
```

**Resultado**: `.specify/memory/constitution.md` actualizado con los principios del proyecto.

> 💡 Este proyecto ya incluye una `constitution.md` pre-configurada con principios 
> bancarios. Puedes saltarte este paso en el demo.

---

### 2️⃣ Crear la Especificación

```
@banking-requirements-agent
/banking-spec-writer Quiero permitir a los clientes abrir una cuenta de ahorros digital
/speckit-specify Quiero permitir a los clientes abrir una cuenta de ahorros de forma 
digital, con verificación de identidad KYC, screening AML, y activación inmediata 
del producto bancario.
```

El agente `banking-requirements-agent` enriquece el spec con contexto de compliance 
bancario y produce `specs/001-savings-account-opening/spec.md`.

---

### 3️⃣ Clarificar Ambigüedades (si aplica)

```
/speckit-clarify Focus on KYC rejection flows and GDPR consent requirements
```

---

### 4️⃣ Generar Plan de Arquitectura

```
@banking-architect-agent
/speckit-plan Angular frontend with reactive forms, REST API with OpenAPI 3.0, 
PostgreSQL encrypted database, external KYC provider integration, 
event-driven compliance checks with audit trail
/speckit-checklist
```

---

### 5️⃣ Analizar Consistencia

```
/speckit-analyze
```

Detecta brechas entre el spec, el plan y los requisitos de compliance.

---

### 6️⃣ Implementar

```
@banking-developer-agent
/speckit-tasks
/speckit-implement
```

---

### 7️⃣ Generar Tests

```
@banking-tester-agent
```

El agente deriva casos de prueba directamente de los criterios de aceptación del spec.

---

### 8️⃣ Revisión de Seguridad

```
@banking-security-agent
```

Valida contra OWASP Top 10, PCI-DSS, KYC/AML, y GDPR.

---

### 9️⃣ Convergencia Final

```
/speckit-converge
```

Verifica que todo el trabajo planificado está completo. Si hay gaps, genera nuevas 
tareas y repite `/speckit-implement` hasta convergencia total.

---

## 🤖 Agentes Disponibles

| Agente | Archivo | Fase SDLC | Spec Kit Commands |
|--------|---------|-----------|-------------------|
| **banking-requirements-agent** | `.github/agents/banking-requirements-agent.md` | Requisitos | `/speckit.specify`, `/speckit.clarify` |
| **banking-architect-agent** | `.github/agents/banking-architect-agent.md` | Arquitectura (SDA) | `/speckit.plan`, `/speckit.checklist`, `/speckit.analyze` |
| **banking-developer-agent** | `.github/agents/banking-developer-agent.md` | Desarrollo | `/speckit.tasks`, `/speckit.implement`, `/speckit.converge` |
| **banking-tester-agent** | `.github/agents/banking-tester-agent.md` | Pruebas | (genera desde spec ACs) |
| **banking-security-agent** | `.github/agents/banking-security-agent.md` | Seguridad | (revisa código generado) |

### Agentes base de Spec Kit (incluidos automáticamente)

| Agente | Comando |
|--------|---------|
| speckit.constitution | `/speckit.constitution` |
| speckit.specify | `/speckit.specify` |
| speckit.clarify | `/speckit.clarify` |
| speckit.plan | `/speckit.plan` |
| speckit.checklist | `/speckit.checklist` |
| speckit.analyze | `/speckit.analyze` |
| speckit.tasks | `/speckit.tasks` |
| speckit.implement | `/speckit.implement` |
| speckit.converge | `/speckit.converge` |
| speckit.taskstoissues | `/speckit.taskstoissues` |

---

## 🛠️ Skills Disponibles

Los skills están en formato oficial `.github/skills/<nombre>/SKILL.md` e invocados con `/nombre-del-skill`.

### Banking Skills (creados para este harness)

| Skill | Carpeta | Invocación | Cuándo usar |
|-------|---------|-----------|-------------|
| **banking-spec-writer** | `.github/skills/banking-spec-writer/` | `/banking-spec-writer` | Antes de `/speckit-specify` — enriquece la descripción con contexto KYC/AML |
| **banking-domain-rules** | `.github/skills/banking-domain-rules/` | `/banking-domain-rules` | Referencia de reglas bancarias para cualquier agente |
| **sdlc-orchestrator** | `.github/skills/sdlc-orchestrator/` | `/sdlc-orchestrator` | Guía completa del flujo y diagnóstico de siguiente paso |

### Spec Kit Skills (generados automáticamente por `specify init`)

| Skill | Carpeta | Invocación |
|-------|---------|-----------|
| Constitution | `.github/skills/speckit-constitution/` | `/speckit-constitution` |
| Specify | `.github/skills/speckit-specify/` | `/speckit-specify` |
| Clarify | `.github/skills/speckit-clarify/` | `/speckit-clarify` |
| Plan | `.github/skills/speckit-plan/` | `/speckit-plan` |
| Checklist | `.github/skills/speckit-checklist/` | `/speckit-checklist` |
| Analyze | `.github/skills/speckit-analyze/` | `/speckit-analyze` |
| Tasks | `.github/skills/speckit-tasks/` | `/speckit-tasks` |
| Implement | `.github/skills/speckit-implement/` | `/speckit-implement` |
| Converge | `.github/skills/speckit-converge/` | `/speckit-converge` |

### Cómo invocar un skill en Copilot Chat

```
# Referenciar un skill directamente
/banking-spec-writer Quiero que clientes abran una cuenta corriente

# Consultar las reglas del dominio
/banking-domain-rules ¿Qué validaciones son obligatorias para source of funds?

# Saber qué ejecutar a continuación
/sdlc-orchestrator Tengo el spec aprobado, ¿cuál es el siguiente paso?
```

---

## 🏦 Caso de Uso: Apertura de Cuenta de Ahorros

### Flujo de Negocio

```
Cliente → Formulario Personal → Verificación Identidad (KYC) 
→ Screening AML → Evaluación Riesgo → Provisión Cuenta 
→ Cuenta Activa → Notificación Cliente
```

### Máquina de Estados de la Solicitud

```
DRAFT → SUBMITTED → KYC_PENDING → APPROVED → ACTIVE
                         ↓              ↓
                     REJECTED      ESCALATED
                                  (Compliance Officer)
```

### Checks de Compliance Cubiertos

| Check | Tipo | Obligatorio |
|-------|------|------------|
| Validación de documento de identidad | KYC | ✅ |
| Liveness check (selfie vs documento) | KYC | ✅ |
| Screening lista de sanciones (OFAC/ONU) | AML | ✅ |
| Detección de PEP (Persona Políticamente Expuesta) | AML | ✅ |
| Declaración de origen de fondos | AML | ✅ |
| Consentimiento GDPR (con versión y timestamp) | Privacy | ✅ |
| Registro en audit trail inmutable | Compliance | ✅ |

---

## 📦 Artefactos Generados

Al completar el flujo SDLC, el harness produce:

```
specs/001-savings-account-opening/
├── spec.md                      ← Especificación funcional aprobada
├── plan.md                      ← Plan de arquitectura técnico
├── tasks.md                     ← Lista de tareas implementadas
├── analysis.md                  ← Reporte de consistencia spec↔plan
├── convergence.md               ← Verificación de completitud
├── checklists/requirements.md   ← Calidad del spec
├── checklists/architecture.md   ← Calidad de arquitectura
├── test-coverage-report.md      ← Cobertura de pruebas
├── compliance-test-matrix.md    ← Trazabilidad spec ↔ tests
├── security-report.md           ← Hallazgos de seguridad
└── compliance-coverage.md       ← Cobertura KYC/AML/PCI-DSS/GDPR

src/
├── app/features/account-opening/ ← Feature Angular implementada
└── api/api.spec.yaml              ← Contrato OpenAPI 3.0
```

---

## 📚 Referencia de Comandos Spec Kit

| Comando | Descripción | Cuándo |
|---------|-------------|--------|
| `specify init .` | Inicializa Spec Kit en el proyecto | Una sola vez |
| `/speckit.constitution` | Define principios y reglas del proyecto | Una vez, o al cambiar principios |
| `/speckit.specify` | Crea la especificación funcional del feature | Inicio de cada feature |
| `/speckit.clarify` | Resuelve ambigüedades en el spec | Cuando hay items [NEEDS CLARIFICATION] |
| `/speckit.plan` | Genera el plan de arquitectura técnica | Después de spec aprobado |
| `/speckit.checklist` | Valida calidad del plan | Después de `/speckit.plan` |
| `/speckit.analyze` | Detecta inconsistencias spec↔plan↔tasks | Antes de implementar |
| `/speckit.tasks` | Genera lista de tareas ordenadas | Antes de implementar |
| `/speckit.implement` | Ejecuta la implementación | Con tareas listas |
| `/speckit.converge` | Verifica completitud y genera gaps pendientes | Después de implementar |
| `/speckit.taskstoissues` | Convierte tasks en GitHub Issues | Opcional, para tracking |

---

## 🔗 Recursos

- [GitHub Spec Kit — Documentación Oficial](https://github.github.com/spec-kit/)
- [Quickstart Guide](https://github.github.com/spec-kit/quickstart.html)
- [Microsoft Learn: Spec-Driven Development](https://learn.microsoft.com/en-us/training/modules/spec-driven-development-github-spec-kit-enterprise-developers/)
- [GitHub Copilot Agents](https://docs.github.com/en/copilot/customizing-copilot/building-custom-copilot-extensions)

---

*Generado con GitHub Copilot CLI · Powered by GitHub Spec Kit · Banking Domain Demo*
