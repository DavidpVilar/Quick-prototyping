# 📊 ÍNDICE - Sistema de Prototipado Agéntico Rápido

> Bienvenido PM. Sistema para crear **prototipos funcionales con una slide showcase estándar en 2h10-2h45**.

---

## 🎯 ¿Qué es esto?

Un sistema de 9 archivos que funcionan juntos para convertir tu idea en un **prototipo clickeable validado rapidísimamente**.

**Tú haces:** Escribes tu requisito (20+ caracteres) → ENVIAS → ESPERAS 2-2.5h  
**Sistema hace:** Todo lo demás automáticamente sin intervención

---

## 📋 Estructura de 9 Archivos

### 1️⃣ **INDEX.md** (este archivo)
Tu guía. Lee esto primero.

### 2️⃣ **01_ORCHESTRATOR.md**
El mapa de ruta. Define exactamente qué pasos seguirá la IA. **No lo edites, solo entiéndelo.**

### 3️⃣-️⃣9️⃣ **02-08_AGENT_*.md**
Instrucciones específicas para cada agente:
- `02_AGENT_CAPTURADOR.md` - Estructura tu idea
- `03_AGENT_VALIDADOR.md` - Valida viabilidad
- `04_AGENT_ESPECIFICADOR.md` - Genera user stories
- `05_AGENT_DEVELOPER.md` - Crea prototipo
- `06_AGENT_VISUAL.md` - Diseña wireframes
- `07_AGENT_QA.md` - Valida todo
- `08_AGENT_PRESENTADOR.md` - Crea una slide showcase HTML interactiva + resumen Markdown (estándar)

---

## 🚀 Cómo Usarlo

### **PASO 1: Abre este chat**
Estás aquí. ✓

### **PASO 2: Escribe tu requisito**
```
"Crea un prototipo que separe autoexclusión y revocación 
en dos apartados independientes..."
```

**Requisito mínimo:** 20 caracteres  
**Requisito ideal:** 50-200 caracteres (descripción clara)

Si adjuntas capturas, el Orquestador las procesa en memoria como base64 y formato, sin guardarlas en el sistema de archivos ni generar rutas o hashes. Si un adjunto no está disponible o no puede codificarse, el flujo se pausa. Los iconos que falten como archivos separados se recortan de esa captura actual y se pasan también codificados, nunca se inventan.

Si adjuntas varias capturas, cada una se codifica por separado. Si falta alguna, el Orquestador pausa y te pide volver a adjuntarla; no busca archivos para recuperarla.

Cada sesión empieza de cero: los agentes solo usan las instrucciones nombradas por el flujo, los datos recibidos ahora y los artefactos dentro de la carpeta de salida de esta sesión. Está prohibido buscar, listar o abrir archivos de otras sesiones en `outputs/`, `assets/` o cualquier otra carpeta, incluso con autorización del PM. Si falta un dato actual, se detienen y lo solicitan; no intentan recuperarlo de archivos anteriores.

Antes de generar artefactos, el Orquestador crea una carpeta nueva `outputs/prototipo_<tema>/`, donde `<tema>` es un slug breve relacionado con el requisito. Todos los entregables de esa sesión se guardan dentro de esa carpeta; las capturas de entrada solo se mantienen en memoria durante la sesión.

### **PASO 3: Yo (Claude) arranco automáticamente**
```
Yo leo INDEX.md → 01_ORCHESTRATOR.md → 02_AGENT_...
Ejecuto cada paso
Reporto progreso
Genero entregables
```

### **PASO 4: Espera resultados**
Timeline REAL:
- Capturador → Validador: 3 min
- Especificador: 5-10 min
- Developer + Visual (paralelo): 1-1.5 horas
- QA de prototipo: 10-15 min (máximo 3 ciclos Developer-QA)
- Presentador: una slide showcase + resumen Markdown (10-15 min)
- QA del showcase: iframe, interacciones, contenido y responsive (3-5 min)
- **TOTAL: 2h10-2h45** (incluye Presentador y QA del showcase)

La slide se genera por defecto con el estado QA real. Si QA queda en `FAIL` o `BLOCKED`, la slide lo indica y no presenta el prototipo como aprobado.

---

## 📊 Qué Pasa en Cada Paso (Tiempos Reales)

| Paso | Agente | Input | Output | ⏱️ |
|------|--------|-------|--------|------|
| 1 | Capturador | Tu requisito | JSON + Markdown | **2 min** |
| 2 | Validador | Idea estructurada | GO/CAUTION/NO-GO | **1 min** |
| 3 | Especificador | Idea validada | 2 US + 8 Criterios | **5-10 min** |
| 4 | Developer | Especificación | Código React funcional | **1-1.5h** |
| 5 | Visual | Especificación | 3-4 Wireframes | **20-30 min** |
| 6 | QA | Proto + wireframes | PASS o escala al PM (max 3 ciclos) | **10-15 min** |
| 7 | Presentador | Idea + spec + preview + QA | 1 slide HTML showcase + Markdown | **10-15 min** |
| 8 | QA del showcase | Slide + preview + QA real | PASS/FAIL independiente del QA funcional | **3-5 min** |
| | | | **TOTAL** | **2h10-2h45** |

---

## 🔄 Loop QA-Developer (Eficiente ⚡)

Si QA falla, el sistema entra en un **loop rápido de corrección**:

```
Developer entrega → QA valida (máximo 8 criterios críticos)
                    ├─ PASS ✅ → Listo (mayoría de casos)
                    └─ FAIL → Feedback detallado a Developer
                             Developer corrige (15-30 min)
                             QA re-valida
```

### **⚡ LÍMITE: máximo 3 ciclos Developer-QA**

```
  Ciclo 1: QA PASS ✅ → Listo
           QA FAIL → Developer recibe feedback específico

  Ciclo 2: QA PASS ✅ → Listo
           QA FAIL → Developer corrige solo el feedback recibido

  Ciclo 3: QA PASS ✅ → Listo
           QA FAIL → ⏸️ STOP - Decisión del PM requerida

No se inicia un cuarto ciclo sin autorización explícita del PM.
```

Si en intento 2 sigue fallando:
- El sistema **PARA**
- Genera **reporte con pistas específicas**
- Indica **exactamente qué está fallando**
- PM decide si revisar especificación o intentar de nuevo

**Por qué funciona:** Developer ya sabe exactamente qué validará QA desde el principio.

---

## 📈 Estados Posibles

### ✅ **ÉXITO**
```
Validador: GO
Especificador: 2 US + 8 criterios Gherkin ✓
Developer: Código clickeable ✓
Visual: Wireframes ✓
QA: PASS (1-3 intentos) ✓
Presentador: 1 slide showcase con iframe funcional ✓
QA del showcase: PASS ✓
→ ENTREGABLE LISTO
```

### ⚠️ **BLOQUEO TEMPRANO** (Validador NO-GO)
```
Requisito recibido
Validador dice: NO-GO
Riesgo crítico identificado
→ STOP aquí
→ Reporta bloqueante
```

### ❌ **FALLO EN QA (3+ intentos)**
```
QA FAIL en intento 1
Developer corrige
QA FAIL en intento 2
Developer corrige
QA FAIL en intento 3
→ STOP - ALERTA CRÍTICA
→ Reporta problemas específicos
→ Necesita revisión manual
```

---

## 📁 Entregables (Cuando Termina)

Si **ÉXITO**, recibirás en `outputs/prototipo_<tema>/`:

```
📦 CORE (6 archivos):
  ├─ PROTO_SPEC_2026-10-01-AUTOEX-001.json    [Idea + 2 US + 8 Criterios]
  ├─ PROTO_CODE_2026-10-01-AUTOEX-001.jsx     [Código React - clickeable]
  ├─ PROTO_PREVIEW_2026-10-01-AUTOEX-001.html [Preview standalone para QA/Presentador]
  ├─ PROTO_WIREFRAMES_2026-10-01-AUTOEX-001.md [3-4 Wireframes clave]
  ├─ PROTO_QA_REPORT_2026-10-01-AUTOEX-001.json [Resultado QA real: PASS/FAIL/BLOCKED]
  └─ PROTO_STATE_2026-10-01-AUTOEX-001.json   [Historial del flujo]

📊 SHOWCASE (estándar en todas las sesiones):
  ├─ PRESENTATION_2026-10-01-AUTOEX-001.html [Una slide visual con iframe funcional]
  └─ PRESENTATION_2026-10-01-AUTOEX-001.md   [Resumen de la slide y QA real]
```

**Total: 8 archivos** (6 de prototipo/QA + 2 de showcase)

---

## ❓ Preguntas Frecuentes

### **¿Puedo cambiar el requisito a mitad del flujo?**
No recomendado. Mejor: termina este, luego nuevo requisito nuevo flujo.

### **¿Qué pasa si Developer no logra hacer el prototipo clickeable?**
El loop QA fallará 3 veces y alertará. Entonces necesitas revisar las instrucciones en 05_AGENT_DEVELOPER.md.

### **¿Necesito hacer algo durante el flujo?**
Solo esperar. Excepción: si QA falla 1-2 veces, Developer dice "corregido", tú confirmas y continúa.

### **¿Cuánto cuesta esto?**
Cero. Es todo en Claude, sin infraestructura, sin APIs externas.

### **¿Se genera una presentación en cada sesión?**
Sí. El Presentador crea una slide con cambios y User Stories a la izquierda, el estado QA real y el preview HTML dentro de un frame móvil.

### **¿La slide siempre dice PASS?**
No. Muestra el estado y los conteos reales de QA. Un estado `FAIL` o `BLOCKED` no se presenta como aprobado.

### **¿Se guarda todo?**
Sí. Cada sesión guarda sus archivos en una carpeta nueva `outputs/prototipo_<tema>/`.

---

## 🎯 Próximos Pasos

1. **Lee esto completamente** ✓ (estás aquí)
2. **Prepara tu requisito** - Describe qué quieres
3. **Escribe aquí:** "Crea un prototipo que..."
4. **Yo arrancaré automáticamente**

---

## 📌 Referencia Rápida

**Si quieres entender el flujo:**
→ Lee `01_ORCHESTRATOR.md`

**Si quieres ver cómo funciona Capturador:**
→ Lee `02_AGENT_CAPTURADOR.md` y su ejemplo

**Si necesitas saber cómo se valida:**
→ Lee `07_AGENT_QA.md` (ahí está la lógica de loop)

---

**¡Listo! Escribe tu requisito abajo y comenzamos.**
