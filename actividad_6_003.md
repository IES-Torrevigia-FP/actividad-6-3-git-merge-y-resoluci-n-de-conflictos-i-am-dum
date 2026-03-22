
# ⚔️ Actividad 6.3 – `git merge` y resolución de conflictos

> En la Actividad 6.2 has creado y usado ramas básicas.
> Ahora vas a aprender a **unir ramas (merge)** y a **resolver conflictos** cuando Git no puede decidir automáticamente qué cambios conservar.

---

## 🎯 Objetivos de la actividad

Al finalizar esta actividad serás capaz de:

- Realizar un **merge** sencillo entre ramas con `git merge`.
- Provocar y entender un **conflicto de merge**.
- Leer los **marcadores de conflicto** `<<<<<<< HEAD`, `=======`, `>>>>>>> rama`.
- Resolver el conflicto editando el archivo, haciendo `git add` y completando el merge con `git commit`.

---

## 🧠 Conceptos clave: ¿Qué es un merge? ¿Qué es un conflicto?

- `git merge` combina el historial de otra rama en la rama actual.
- Git intenta **fusionar automáticamente** los cambios.
- Si dos ramas han modificado **las mismas líneas** o una ha borrado un archivo que la otra ha editado, Git no sabe qué elegir y se produce un **conflicto de merge**.

En ese caso:

- Git marca el archivo con bloques así:

```text
<<<<<<< HEAD
versión de la rama actual
=======
versión de la rama que se está mergeando
>>>>>>> nombre-de-la-rama
```

- Tú decides cómo dejar el contenido final:
  - una de las versiones,
  - una combinación de ambas,
  - o algo nuevo.

---

## 🧩 Parte 1 – Preparar el escenario del merge

### Reto 1.1 – Crear repositorio base y commit inicial

1. Crea una carpeta para esta actividad:

   ```bash
   mkdir actividad-6-3-merge-conflictos
   cd actividad-6-3-merge-conflictos
   ```

2. Inicializa un repositorio:

   ```bash
   git init
   ```

3. Crea el archivo `historia.txt` con este contenido:

   ```text
   Línea 1: Érase una vez un proyecto.
   Línea 2: La historia comienza en la rama main.
   Línea 3: Aún no hay ramas alternativas.
   ```

4. Añade y haz commit:

   ```bash
   git add historia.txt
   git commit -m "Crea historia inicial en main"
   ```

5. Comprueba ramas e historial:

   ```bash
   git branch
   git log --oneline
   ```

### Checkpoint 1 – Evidencia

- Crea `cp1-estado-inicial.txt` con:
  - Salida de `git branch`.
  - Salida de `git log --oneline`.

---

## 🧩 Parte 2 – Rama sin conflicto (merge limpio)

### Reto 2.1 – Crear rama `capitulo-1`

1. Crea y cámbiate a una rama:

   ```bash
   git checkout -b capitulo-1
   ```

2. Añade una línea **nueva al final** de `historia.txt` (sin tocar las líneas existentes):

   ```text
   Línea 4: En la rama capitulo-1 añadimos un nuevo capítulo.
   ```

3. Añade y haz commit:

   ```bash
   git add historia.txt
   git commit -m "Añade línea para el capítulo 1"
   ```

### Reto 2.2 – Merge sin conflicto

1. Vuelve a `main`:

   ```bash
   git checkout main
   ```

2. Haz merge de la rama `capitulo-1`:

   ```bash
   git merge capitulo-1
   ```

3. Comprueba el archivo `historia.txt`:
   Debería incluir las 4 líneas (merge automático, sin conflicto).

### Checkpoint 2 – Evidencia

- Crea `cp2-merge-limpio.txt` con:
  - Comando que has usado para el merge.
  - Salida de `git log --oneline` tras el merge.
  - Una frase explicando qué es un **merge sin conflicto**.

---

## 🧩 Parte 3 – Preparar un conflicto de merge

### Reto 3.1 – Rama `version-main`

1. Asegúrate de estar en `main`:

   ```bash
   git checkout main
   ```

2. Crea una rama nueva desde `main`:

   ```bash
   git checkout -b version-alternativa
   ```

3. En esta rama, modifica **la línea 2** de `historia.txt` para que quede así:

   ```text
   Línea 2: La historia toma un camino alternativo en otra rama.
   ```

4. Añade y haz commit:

   ```bash
   git add historia.txt
   git commit -m "Modifica línea 2 en rama alternativa"
   ```

---

### Reto 3.2 – Volver a `main` y preparar la versión principal

1. Vuelve a la rama `main`:

   ```bash
   git checkout main
   ```

2. Ahora modifica **la misma línea 2** de `historia.txt` para que quede así:

   ```text
   Línea 2: La historia principal avanza en la rama main.
   ```

3. Añade y haz commit:

   ```bash
   git add historia.txt
   git commit -m "Modifica línea 2 en main"
   ```

Ahora `main` y `version-alternativa` han modificado la misma línea de forma distinta: eso provocará un conflicto.

---

## 🧩 Parte 4 – Provocar el conflicto de merge

### Reto 4.1 – Hacer el merge y observar el conflicto

1. Vuelve a la rama `main`:

   ```bash
   git checkout main
   ```

2. Intenta hacer merge de la rama `version-alternativa`:

   ```bash
   git merge version-alternativa
   ```

Git debería indicar que hay un **merge conflict** en `historia.txt`.

3. Ejecuta:

   ```bash
   git status
   ```

Verás que el archivo aparece como “both modified” y el merge está en curso.

4. Abre `historia.txt` y observa los marcadores:

   ```text
   Línea 1: Érase una vez un proyecto.
   <<<<<<< HEAD
   Línea 2: La historia principal avanza en la rama main.
   =======
   Línea 2: La historia toma un camino alternativo en otra rama.
   >>>>>>> version-alternativa
   Línea 3: Aún no hay ramas alternativas.
   Línea 4: En la rama capitulo-1 añadimos un nuevo capítulo.
   ```

### Checkpoint 3 – Evidencia

- Crea `cp3-conflicto.txt` con:
  - La salida de `git status` durante el conflicto.
  - Copia del fragmento de `historia.txt` donde se vean los marcadores (`<<<<<<<`, `=======`, `>>>>>>>`).

---

## 🧩 Parte 5 – Resolver el conflicto

### Reto 5.1 – Editar y decidir el contenido final

1. Edita `historia.txt` para **eliminar los marcadores de conflicto** y dejar una versión final coherente, por ejemplo:

   ```text
   Línea 1: Érase una vez un proyecto.
   Línea 2: La historia principal avanza en la rama main, pero también explora caminos alternativos.
   Línea 3: Aún no hay ramas alternativas.
   Línea 4: En la rama capitulo-1 añadimos un nuevo capítulo.
   ```

La clave es:
- Quitar `<<<<<<< HEAD`, `=======`, `>>>>>>> version-alternativa`.
- Dejar solo el texto definitivo que quieras conservar.

2. Marca el conflicto como resuelto añadiendo el archivo a staging:

   ```bash
   git add historia.txt
   ```

3. Completa el merge con un commit:

   ```bash
   git commit -m "Resuelve conflicto en línea 2 combinando ambas versiones"
   ```

4. Comprueba:

   ```bash
   git status
   git log --oneline
   ```

### Checkpoint 4 – Evidencia

- Crea `cp4-resolucion-conflicto.txt` con:
  - La salida de `git log --oneline` después de resolver el conflicto.
  - Una explicación (3–4 líneas) de qué pasos has seguido para resolver el conflicto.

---

## 🧩 Parte 6 – Opcional: abortar un merge

> Esta parte es opcional, pero muy útil si la cosa “se lía”.

### Reto 6.1 – Probar `git merge --abort`

1. Repite una situación de conflicto rápida (no hace falta que sea igual que la anterior).
2. Justo después de que Git muestre el conflicto, ejecuta:

   ```bash
   git merge --abort
   ```

Deberías volver al estado anterior al intento de merge.

3. Comprueba:

   ```bash
   git status
   ```

### Checkpoint 5 – Evidencia (opcional)

- Crea `cp5-abortar-merge.txt` con:
  - El comando `git merge --abort` usado.
  - Una frase que explique en qué situaciones te puede salvar este comando.

---

## 🧩 Parte 7 – Reflexión final

Crea un archivo `reflexion-6-3.md` y responde:

1. Define con tus palabras qué es un **conflicto de merge** y cuándo suele ocurrir.
2. Explica el significado de cada marcador en un conflicto:
   - `<<<<<<< HEAD`
   - `=======`
   - `>>>>>>> nombre-de-la-rama`
3. ¿Qué buenas prácticas pueden ayudar a reducir conflictos en un equipo (tamaño de los commits, frecuencia de merge, comunicación, etc.)?

---

## 📦 Entrega esperada en el repositorio

Tu repositorio de la Actividad 6.3 debe incluir al menos:

- `historia.txt` (versión final tras el conflicto).
- `cp1-estado-inicial.txt`
- `cp2-merge-limpio.txt`
- `cp3-conflicto.txt`
- `cp4-resolucion-conflicto.txt`
- `cp5-abortar-merge.txt` (si realizas la parte opcional)
- `reflexion-6-3.md`

---

## 🏁 Evaluación sugerida

- 4 pts – Uso correcto de `git merge` y comprensión de cuándo se produce un conflicto.
- 3 pts – Evidencias completas y claras en los checkpoints (estado, conflicto, resolución).
- 3 pts – Calidad de la reflexión en `reflexion-6-3.md` (entender el porqué, no solo el cómo).

**Puntuación máxima: 10 puntos.**

---

## 🤖 Evaluación automática y redacción de informe

Esta actividad incluye una evaluación automática similar a la de las actividades anteriores, adaptada a merges y conflictos:

- Workflow: `.github/workflows/evaluacion.yml`
- Script de evaluación: `tools/evaluar_repo.py`
- Script de redacción IA: `tools/ai_redactor.py`

### ¿Qué genera?

Al ejecutarse en GitHub Actions, se generan los siguientes archivos en `reportes/`:

- `informe.md` (resumen técnico de evaluación)
- `informe.json` (métricas en formato estructurado)
- `metricas.csv` (puntuaciones por criterio)
- `arbol.txt` (resumen del árbol del repositorio)

Si hay credenciales de IA configuradas en Secrets/Variables, también se generan:

- `retro.md` (retroalimentación redactada por IA)
- `retro.docx` (versión formal en Word)

### Parámetros del workflow (`workflow_dispatch`)

- `min_commits` (por defecto: `4`)
- `required_files` (por defecto: `historia.txt,cp1-estado-inicial.txt,cp2-merge-limpio.txt,cp3-conflicto.txt,cp4-resolucion-conflicto.txt,reflexion-6-3.md`)

---