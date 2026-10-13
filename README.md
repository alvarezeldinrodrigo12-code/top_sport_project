# value-betting

Proyecto personal de modelado de apuestas deportivas. El objetivo es construir un modelo probabilístico que identifique apuestas con **Valor Esperado (EV) positivo** en el mercado de fútbol 1X2 y Over/Under (más de 2.5 goles).

## Objetivo

- Ingestar datos de cuotas (apertura y cierre), resultados y estadísticas.
- Construir modelos de probabilidades calibradas (Poisson, Dixon‑Coles, Elo, o Machine Learning).
- Detectar situaciones donde la probabilidad implícita de la cuota es menor a la estimada por el modelo (edge).
- Registrar métricas de rendimiento: log‑loss, Brier, yield, ROI, CLV (closing line value).

## Estructura de carpetas

```
.
├── README.md
├── 01_data
│   ├── 011_raw          # Datos brutos descargados (cuotas, fixtures, stats)
│   └── 012_processed    # Datos limpios y features ingenierados
├── 02_src
│   └── ...              # Scripts Python: ingesta, feature engineering, modelado
├── 03_models
│   ├── baseline/        # Modelos simples (Poisson, Dixon‑Coles, Elo)
│   └── ml/              # Modelos avanzados (LightGBM, XGBoost, Bayesiano)
├── .gitignore
├── requirements.txt
├── env.json             # Variables de entorno (API keys, etc.)
├── SESSION_LOG.md       # Bitácora de sesiones: registro y próximos pasos
└── ...
```

## Próximos pasos (roadmap)

1. **Fase 1 – Datos**: Pipeline de ingesta de cuotas (The Odds API / football-data.co.uk) y resultados (FBref/understat). Guardar en `01_data/011_raw` y procesar a `01_data/012_processed`.
2. **Fase 2 – Baseline**: Implementar modelos Poisson/Dixon‑Coles y Elo como baseline. Ejecutar backtest walk‑forward sobre datos históricos.
3. **Fase 3 – Features**: Añadir ratings Elo, xG, alineaciones, contexto (clima, reposo), y movimiento de línea.
4. **Fase 4 – Modelado y calibración**: Entrenar modelos ML y calibrar probabilidades (isotonic regression / Platt scaling). Evaluar log‑loss y reliability diagrams.
5. **Fase 5 – Motor de valor**: Calcular EV unitario, fracción Kelly y estrategia de staking. Filtrar apuestas por umbral de confianza.
6. **Fase 6 – Validación**: Bootstrap de intervalos de confianza, significancia estadística, paper‑trading simulado.
7. **Fase 7 – Live**: Ejecución con stake mínimo, monitoreo continuo de CLV y ajuste del modelo.

## Setup rápido

1. Crear un entorno virtual (`conda create -n value-betting python=3.11` o `python -m venv .venv`).
2. Instalar dependencias: `pip install -r requirements.txt`.
3. Poner las claves de API en `env.json` o variables de entorno (nunca commitear credenciales).
4. Ejecutar `python -m src.data.ingest` (por definir en `02_src`).

## Colaboración y Flujo de Trabajo Git (Rodrigo & Fernando)

### Roles

- **Rodrigo (Científico de Datos / IA)**: Define el problema, selecciona modelos, valida resultados estadísticos (backtest, CLV, bootstrap), revisa y aprueba los cambios para `main`. Es el responsable de la estrategia y la evaluación de edge.
- **Fernando (Desarrollador / Vibecoding)**: Traduce las indicaciones y el código a Python funcional, crea scripts de ingesta, automatiza tareas, ejecuta lo que Rodrigo indique. Su trabajo se centra en la implementación y el bajo código con Claude.

### Ramas principales

- **`main`**: Código **revisado y aprobado**. Solo se admiten cambios que hayan pasado el filtro de revisión de Rodrigo. En `main` siempre debe haber un estado funcional y probado (backtest, métricas claras). **Nunca** se commitean experimentos sin validar directamente a `main`.
- **`dev`**: Rama de **desarrollo activo**. Aquí es donde Fernando realizará la mayor parte del vibecoding con Claude, experimentará con scripts, commiteará avances parciales y pruebas rápidas. Todo lo que no esté totalmente validado debe quedar en `dev`.

### Flujo de trabajo recomendado

1. **Inicio de una tarea**: Crear una rama corta derivada de `dev` (o trabajar directamente en `dev` si es una tarea pequeña).
   ```bash
   git checkout dev
   # o bien
   git checkout -b tarea/corto
   ```
2. **Desarrollo**: Fernando escribe o genera código con Claude. Commits frecuentes y mensajes claros:
   ```bash
   git add .
   git commit -m "feat: pipeline básico de descarga de cuotas The Odds API"
   ```
3. **Revisión (Rodrigo)**: Revisar los cambios, ejecutar los tests o backtest que correspondan. Si todo está correcto y tiene sentido, hacer merge a `main`:
   ```bash
   git checkout main
   git pull origin main
   git merge dev   # o la rama corta
   # resolver conflictos si los hay
   git push origin main
   ```
4. **Después del merge**: Volver a `dev` para continuar el siguiente fragmento.
   ```bash
   git checkout dev
   git pull origin dev
   ```

### Reglas de oro

- **Solo lo revisado a `main`**: Si Rodrigo no ha comprobado que el código produce el resultado esperado (ej. el backtest muestra CLV positivo, los datos se guardan bien), **no hagan merge a `main`**.
- **Lo desarrollado en `dev`**: Experimentaciones, cambios de último minuto, pruebas de nuevas features y código "suelto" quedará en `dev` hasta que Rodrigo lo valida.
- **Conflictos**: Si `dev` y `main` se han divergido, resolver los conflictos en `dev`, hacer el merge a `main` y empujar. Si el experimento no funciona, se puede desechar el merge y volver a `dev` con el código anterior.

### Commites recomendados

- Mensajes en pasado, claros:
  - `feat: pipeline básico de descarga de cuotas The Odds API`
  - `fix: corregir nombre de columna en matches.parquet`
  - `docs: actualizar README con sección de flujo de Git`
- **Un cambio lógico por commit.** Si hacen varios cambios sin relación, hagan commits separados para facilitar la revisión.

## Bitácora de Sesiones (SESSION_LOG.md)

El archivo **`SESSION_LOG.md`** se encuentra en la raíz del proyecto (al lado del `README.md`). Su propósito es registrar qué se hizo en cada sesión de OpenCode y cuáles son los próximos pasos, para que tanto Rodrigo como Fernando puedan retomar el contexto al instante. **Además, este archivo debe reflejar el estado actual de las ramas de Git al final de cada sesión.**

### Formato rápido (añade una entrada al comenzar cada sesión)

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
- **Estado de Git**:
  - Rama actual: `main` / `dev` / `tarea/x`
  - Último commit: <mensaje del commit> ([hash corto](URL del repo))
  - Observaciones varias (opcional): <cualquier cosa relevante para retomar el hilo>
```

### Ejemplo de entrada

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
- **Estado de Git**: Rama actual `main`; Último commit "Scaffold inicial del proyecto value-betting" (a1b2c3d). Observaciones: Repo recién creado, aún sin datos.
```

### Cómo usarlo

- **Al inicio de cada sesión de OpenCode**, lee este archivo (puedes hacer `/read SESSION_LOG.md` en la terminal o hacerlo visualmente en la UI). **Además, ejecuta `git status` anota la rama y el último commit en el campo "Estado de Git"**.
- **Al terminar la sesión**, añade una nueva entrada al principio del archivo (para que la más reciente se vea primero) con los avances, pending tasks, decisiones y el estado actual de Git (ruba, último commit).
- Esto evita perder el hilo cuando alguien vuelve al proyecto después de unos días y garantiza que ambos sepan en qué rama están y qué último commit tienen.

### Reglas rápidas

- Siempre añade una nueva entrada al **comenzar** la sesión (para que la más reciente se vea primero) o al **final** si prefieres cronológico; elige uno y sé constante.
- Mantén los objetivos cortos y accionables.
- Si algo se queda pendiente, anótalo en “Próximos pasos” para que la próxima sesión no pierda el hilo.
- **Hágalo hábito**: `git status` al iniciar y al terminar cada sesión de OpenCode; anota esos dos datos (rama + último commit) en el session log. Es el mecanismo de seguridad principal.

---

## Notas

- **Juego responsable:** Este modelo es para uso personal y análisis. Nunca apuestes más de lo que estás dispuesto a perder.
- **Edge es pequeño:** Se espera un retorno del 1‑3% sobre el volumen apostado; se necesitan miles de apuestas para distinguir skill del ruido.
- **Git**: Recuerda no commitear claves de API ni archivos de base de datos; están en `.gitignore`.