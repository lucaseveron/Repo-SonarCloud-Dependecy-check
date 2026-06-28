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
