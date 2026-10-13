# AGENTS.md

## Roles

- **Rodrigo (Científico de Datos / IA)**: Define el problema, selecciona modelos, valida resultados estadísticos (backtest, CLV, bootstrap), revisa y aprueba los cambios para `main`. Es el responsable de la estrategia y la evaluación de edge.
- **Fernando (Desarrollador / Vibecoding)**: Traduce las indicaciones y el código a Python funcional, crea scripts de ingesta, automatiza tareas, ejecuta lo que Rodrigo indique. Su trabajo se centra en la implementación y el bajo código con Claude.

## Flujo de trabajo con Git

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

El archivo **`SESSION_LOG.md`** se encuentra en la raíz del proyecto (al lado del `README.md`). Su propósito es registrar qué se hizo en cada sesión de OpenCode y cuáles son los próximos pasos, para que tanto Rodrigo como Fernando puedan retomar el contexto al instante.

**Además, este archivo debe reflejar el estado actual de las ramas de Git al final de cada sesión.**

### Formato de entrada (por sesión)

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
  - Observaciones diversas (opcional): <cualquier cosa relevante para retomar el hilo>
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

## Skill: Selección de modelo según tipo de tarea

**Objetivo:** Antes de iniciar cualquier proceso clave o extenso, sugerir el modelo OpenCode Zen más adecuado de la lista gratuita, para optimizar el uso de créditos.

### Tabla de mapeo de tareas a modelos (gratuitos)

| Categoría | Tarea ejemplo | Modelo gratuito recomendado | Comentario |
|---|---|---|---|
| **Simple** | • Lectura/escritura de un archivo<br>• Pregunta de sintaxis<br>• Limpieza básica de datos<br>• Comandos git básicos | `opencode/big-pickle` <br>o `opencode/nemotron-3.5-lightning-free` | Costo 0. Suficiente para tareas ligeras. |
| **Mediana** | • Feature engineering básico<br>• Backtest walk‑forward con < 500 registros<br>• Generación de reportes simples<br>• Ajuste sencillo de modelos | `opencode/nemotron-3.5-lightning-free` | Costo 0. Buen equilibrio entre capacidad y precio. |
| **Compleja** | • Entrenamiento extensivo con LightGBM/XGBoost<br>• Backtest walk‑forward con miles de registros<br>• Optimización de hiperparámetros | `opencode/big-pickle` (aunque muy lento) | Costo 0. Para problemas realmente difíciles, se recomienda usar modelos de pago (ver abajo). |

### Modelo de pago recomendado (mejor relación calidad‑precio)

| Categoría | Modelo de pago recomendado | Aprox. costo por 1M tokens (input / output) | Justificación |
|---|---|---|---|
| **Simple** | **No es necesario pagar** – los modelos gratuitos tienen suficiente capacidad. | - | Usar un modelo de pago aquí sería tirar dinero sin ganar casi nada. |
| **Mediana** | `opencode/openai/gpt-4o` <br>o `opencode/gemini-1.5-flash` | OpenAI: ~$2.50 / $10 <br>Gemini: ~$0.35 / $1.05 | Son **mucho más baratos** que Claude Sonnet (~$2/$10) y ofrecen un notable salto de calidad sobre el gratuito `nemotron`. La mejor relación calidad‑precio para tareas medianas. |
| **Compleja** | `opencode/openai/gpt-4` <br>o `opencode/gemini-1.5-pro` | OpenAI: ~$10 / $30 <br>Gemini: ~$1.00 / $3.5 | Modelos de gama alta. Su costo es alto, pero si la tarea es realmente difícil y se necesita el mejor resultado posible, es la inversión justificada. Si el presupuesto es limitado, se puede recurrir al gratuito `big-pickle` y dividir el problema en sub‑problemas más pequeños. |

### Flujo de trabajo sugerido

1. **Identificar la categoría** (simple / mediana / compleja) de la tarea que se va a realizar.
2. **Mostrar la fila correspondiente** de la tabla, indicando el modelo gratuito y el de pago opcional.
3. **Preguntar al usuario**: "¿Usamos el modelo gratuito recomendado o tienes crédito y prefieres el de pago?"
4. **Registrar en `SESSION_LOG.md`** qué modelo se efectivamente usó, para construir un historial de eficacia.

## Pendientes a revisar juntos

1. **Confirmar la creación correcta de `agents.md` con Claude/Gemini/OpenAI** – Tras crear este archivo, se verificará que el contenido se muestra correctamente en la interfaz de OpenCode y que tanto Rodrigo como Fernando pueden acceder y leerlo sin problemas. Se probará abriendo el archivo desde el repositorio tanto en la UI como mediante la terminal.
2. **Definir la skill de selección de modelo en práctica** – Una vez validado el archivo, probar la skill invocándola al inicio de sesiones reales y registrar el modelo usado en cada `SESSION_LOG.md`.
3. **Ajustar la tabla de mapeo si es necesario** – Basándose en las primeras sesiones, la tabla de categorías y modelos puede refinarse (por ejemplo, añadir o quitar categorías, o cambiar un modelo por otro que resulte más efectivo en la práctica).

## Cómo usar este archivo

- **Al inicio de cada sesión de OpenCode**, Rodrigo o Fernando deben leer este archivo (`AGENTS.md`) para recordar los roles, el flujo de Git y la sugerencia de modelo.
- **Al terminar la sesión**, añadir una entrada fechaada en `SESSION_LOG.md` haciendo referencia a los apartados de "siguientes pasos" y "pendientes a revisar juntos" aquí definidos.
- Cualquier cambio en los roles, el flujo de Git o la tabla de modelos debe realizarse actualizando este archivo y cometiendo los cambios al repositorio.

---

> **Nota importante:** Este archivo forma parte de la colaboración entre Rodrigo y Fernando. Cualquier modificación debe acordarse mutuamente y cometerse en el repositorio para mantener el historial y la coherencia del proyecto.