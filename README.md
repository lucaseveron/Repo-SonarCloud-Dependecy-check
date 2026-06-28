# sonarcloud-dependency-check-jenkins

Pipeline de Jenkins que integra análisis estático de código (SAST) con SonarCloud 
sobre una aplicación Laravel, ejecutado vía Docker sin necesidad de instalar 
SonarScanner localmente.

## ¿Qué hace?

1. **Checkout** — clona la rama `main` del repositorio objetivo (Laravel Blog)
2. **SonarScanner** — ejecuta el análisis estático con `sonarsource/sonar-scanner-cli` 
   en Docker, enviando resultados a SonarCloud

## Stack

`Jenkins` · `SonarCloud` · `Docker` · `SonarScanner CLI` · `PHP / Laravel`

## Estructura del pipeline

## Cómo funciona

El pipeline usa `sonarsource/sonar-scanner-cli` como imagen Docker, montando 
el código fuente como volumen. Esto evita instalar dependencias en el agente 
de Jenkins y garantiza siempre la versión correcta del scanner.

```groovy
docker run --rm \
  -e SONAR_HOST_URL \
  -e SONAR_TOKEN \
  -v "$(pwd):/usr/src" \
  sonarsource/sonar-scanner-cli \
  -Dsonar.projectKey="..." \
  -Dsonar.organization="..."
```

## Configuración requerida

Las credenciales deben configurarse como **Jenkins Credentials** y referenciarse 
con `credentials()`, nunca hardcodeadas en el Jenkinsfile:

| Variable | Descripción |
|---|---|
| `SONAR_TOKEN` | Token de autenticación de SonarCloud (usar Jenkins Secret) |
| `SONAR_HOST_URL` | URL de SonarCloud (`https://sonarcloud.io/`) |
| `SONAR_PROJECT_KEY` | Project key del proyecto en SonarCloud |
| `SONAR_ORG` | Organización en SonarCloud |

## Nota de seguridad

Este repo documenta un pipeline de aprendizaje. En producción, los tokens 
deben gestionarse siempre como secretos del CI (Jenkins Credentials, 
GitHub Secrets, variables de entorno cifradas) y nunca incluirse directamente 
en el código. Herramientas como **Gitleaks** detectan automáticamente este 
tipo de exposición en pipelines DevSecOps.

## Requisitos

- Jenkins con agente Docker disponible
- Cuenta en [SonarCloud](https://sonarcloud.io/) con proyecto configurado
- Docker instalado en el agente de Jenkins
