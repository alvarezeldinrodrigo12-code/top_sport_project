# Bitácora de Sesiones

> **Cómo usar**: Al inicio de cada sesión de OpenCode, lee este archivo para recordar qué se hizo en la sesión anterior y cuál es el objetivo actual. Después de terminar, añade una nueva entrada al principio (más reciente arriba).

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

## Ejemplo de entrada

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

## Reglas rápidas

- Siempre añade una nueva entrada al **comenzar** la sesión (para que la más reciente se vea primero) o al **final** si prefieres cronológico; elige uno y sé constante.
- Mantén los objetivos cortos y accionables.
- Si algo se queda pendiente, anótalo en “Próximos pasos” para que la próxima sesión no pierda el hilo.
- **Hábito de Git**: al iniciar cada sesión, ejecuta `git status` y anota en el campo “Estado de Git” la rama actual y el último commit (mensaje corto + hash si es posible). Al terminar la sesión, vuelve a hacerlo para reflejar el estado actual después de tus cambios. Esto asegura que ambos sepan en qué punto del proyecto están Rodrigo y Fernando.