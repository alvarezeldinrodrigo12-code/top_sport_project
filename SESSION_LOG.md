# Bitácora de Sesiones

> **Cómo usar**: Al inicio de cada sesión de OpenCode o Claude, lee este archivo para recordar qué se hizo en la sesión anterior y cuál es el objetivo actual. 

## Estructura
1) Reglas rápidas (reglas definidas para mantener consistencia en el trabajo colaborativo)
2) Formato de entrada (como se debe de registrar lo que se hizo en la sesión)
3) Log (Historial de lo que se ha hecho)
4) Siguientes pasos del proyecto (Estado actual del proyecto conforme a lo establecido en README.md)
5) Pendientes a revisar juntos (Tareas que el equipo debe resolver juntos)

## Reglas rápidas

- Siempre añade una nueva entrada al **comenzar** la sesión (para que la más reciente se vea primero) o al **final** si prefieres cronológico; elige uno y sé constante.
- Mantén los objetivos cortos y accionables.
- Si algo se queda pendiente, anótalo en “Próximos pasos” para que la próxima sesión no pierda el hilo.
- **Hábito de Git**: al iniciar cada sesión, ejecuta `git status` y anota en el campo “Estado de Git” la rama actual y el último commit (mensaje corto + hash si es posible). Al terminar la sesión, vuelve a hacerlo para reflejar el estado actual después de tus cambios. Esto asegura que ambos sepan en qué punto del proyecto están Rodrigo y Fernando.

## Formato de entrada

```
## YYYY‑MM‑DD (Sesión N) – <Objetivo corto>
- **Avances**:
  - Punto 1 logrado.
  - Punto 2 en progreso.
- **Próximos pasos**:
  - Tarea A: <descripción>.
  - Tarea B: <descripción>.
- **Archivos modificados**:
  - `ruta/archivo1.py`
  - `ruta/archivo2.json`
- **Decisiones tomadas**:
  - Elección de liga/mercado para la próxima fase.
  - Duda pendiente: <pregunta para el próximo encuentro>.
```

##  Log

```
## 2026‑10‑12 (Sesión 1) – Scaffold del proyecto
- **Avances**:
  - Carpeta `01_data/` con subcarpetas `011_raw`, `012_processed`.
  - Carpeta `02_src/` y `03_models/`.
  - `README.md` con descripción y roadmap.
  - `.gitignore` ignoreando `__pycache__`, `.env`, `.db`.
- **Próximos pasos**: Poner el pipeline de ingesta de datos (Step 1).
- **Archivos modificados**: `README.md`, `01_data/`, `02_src/`, `03_models/`, `.gitignore`.
- **Decisiones tomadas**: Usar Premier League 2024‑25 como liga inicial; The Odds API para cuotas.
```

### Reglas añadidas

- **Hábito de Git**: al iniciar cada sesión, ejecuta `git status` y anota en el campo “Estado de Git” la rama actual y el último commit (mensaje corto + hash si es posible). Al terminar la sesión, vuelve a hacerlo para reflejar el estado actual después de tus cambios. Esto asegura que ambos sepan en qué punto del proyecto están Rodrigo y Fernando.

---

## Siguientes pasos del proyecto

**Fase actual:** Fase 1 – Datos (Pipeline de ingesta de cuotas y resultados).  
**Estado del repositorio:** Repositorio privado `top_sport_project` en GitHub con ramas `main` y `dev`. Commit inicial en `main`.  
**Próxima acción pendiente:** Definir liga/mercado inicial (Premier League 1X2/Over‑Under) y poner el pipeline de ingesta de datos en marcha.

> **Nota:** Este apartado refleja en qué fase del roadmap (definido en el `README.md`) nos encontramos y el estado actual del proyecto. Se actualiza cuando se completan hitos o se avanzan a la siguiente fase.

---

## Pendientes a revisar juntos

- **Skill de selección de modelo:** Definir el mecanismo para sugerir automáticamente el modelo OpenCode Zen más adecuado según el tipo de tarea (simple / mediana / compleja), con una tabla de mapeo que incluya modelos gratuitos y de pago (OpenAI, Gemini, Anthropic) y su relativa relación calidad‑precio.  
- **Tabla de mapeo de modelos:** Queda pendiente crear el documento o tabla definitiva que relacione cada categoría de tarea con el modelo gratuito recomendado y, opcionalmente, el de pago con mejor relación calidad‑precio.  
- **Formato de entrada estandarizado:** Confirmar el formato de las entradas fechas (cómo se anotan avances, próximos pasos, estado de Git) para que ambas partes tengan siempre el mismo esquema.

> **Nota:** Estos apartados están diseñados para permanecer en el archivo como referencia global, de modo que cada sesión nueva pueda comenzar leyéndolos para ponerse al día instantáneamente, y al finalizar la sesión se añadan las entradas fechaadas habituales debajo de estos apartados principales.



