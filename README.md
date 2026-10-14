# 🚀 SISTEMA DE PROTOTIPADO AGÉNTICO - RÁPIDO Y FUNCIONAL

> Has recibido 9 archivos Markdown que forman un sistema para crear **prototipos funcionales en 2-2.5 horas**.
> 
> **Máxima velocidad + Calidad prototipo + PM intervención mínima**

---

## 📦 QUÉ RECIBISTE

### **9 Archivos Markdown:**

```
1. INDEX.md                      ← LÉEME PRIMERO (PM)
2. 01_ORCHESTRATOR.md            ← Instrucciones principales (IA)
3. 02_AGENT_CAPTURADOR.md        ← Paso 1: Captura idea
4. 03_AGENT_VALIDADOR.md         ← Paso 2: Valida viabilidad
5. 04_AGENT_ESPECIFICADOR.md     ← Paso 3: Genera user stories
6. 05_AGENT_DEVELOPER.md         ← Paso 4: Crea prototipo
7. 06_AGENT_VISUAL.md            ← Paso 5: Wireframes
8. 07_AGENT_QA.md                ← Paso 6: Valida (loop 3 intentos)
9. 08_AGENT_PRESENTADOR.md       ← Paso 7: Genera deck (opcional)
```

**Todos en:** una carpeta nueva `outputs/prototipo_<tema>/` por sesión

---

## ✅ CÓMO USARLO

### **Para PM (Tú)**

1. **Lee INDEX.md** - Entiende qué pasa
2. **Escribe tu requisito aquí** - En este mismo chat
   ```
   "Crea un prototipo que separe autoexclusión y revocación..."
   ```
3. **Yo arrancaré automáticamente**
4. **Espera x horas** de ejecución
5. **Recibe entregables** en outputs/

### **Para IA Ejecutora (Yo)**

1. **Leo INDEX.md** - Orientación
2. **Leo 01_ORCHESTRATOR.md** - Entiendo el flujo
3. **Leo 02_AGENT_CAPTURADOR.md** - Ejecuto paso 1 (2 min)
4. **Leo 03_AGENT_VALIDADOR.md** - Ejecuto paso 2 (1 min)
   - Si NO-GO → STOP aquí
5. **Leo 04_AGENT_ESPECIFICADOR.md** - Ejecuto paso 3 (5-10 min)
6. **Leo 05_AGENT_DEVELOPER.md + 06_AGENT_VISUAL.md** - Ejecuto en paralelo (1-1.5h)
7. **Leo 07_AGENT_QA.md** - Valido (máximo 3 ciclos Developer-QA, 10-15 min)
    - Si FAIL en ciclo 3 → Detiene y pide decisión al PM antes de continuar
   - Si PASS → Continúa
8. **Leo 08_AGENT_PRESENTADOR.md** - Genero deck HTML + Markdown (SOLO si PM lo solicita, 30-45 min opt)
9. **Reporto resultado final**

---

## 🎯 NUEVAS CARACTERÍSTICAS

### ✨ Loop QA con Límite de 3 Ciclos

```
Ciclo 1: QA PASS ✅ → Listo
         QA FAIL → Developer corrige solo el feedback recibido

Ciclo 2: QA PASS ✅ → Listo
         QA FAIL → Developer corrige solo el feedback recibido

Ciclo 3: QA PASS ✅ → Listo
         QA FAIL → ⏸️ STOP - PM decide si detiene o autoriza una nueva sesión

No se inicia un cuarto ciclo sin autorización explícita del PM.
```
Si falla el ciclo 3, el sistema:
Si falla el ciclo 3, el sistema:
- ❌ Para completamente
- 📋 Reporta qué está fallando exactamente
- 🔧 Proporciona pistas de corrección
- ⚠️ PM decide si continuar o revisar especificación
- ⚠️ PM decide si detener, revisar alcance o autorizar una nueva sesión

**Nota:** La mayoría de las veces, Developer pasa en intento 1 porque ya conoce exactamente qué QA va a validar.

---

## 📊 FLUJO VISUAL - TIMELINE REAL

```
INICIO (Tú escribes requisito)
    ↓
CAPTURADOR (2 min)
    ↓
VALIDADOR (1 min)
    ├─ ¿NO-GO? → STOP
    └─ ¿GO/CAUTION? → Continuar
    ↓
ESPECIFICADOR (5-10 min) [2 US, 8 criterios]
    ↓
DEVELOPER + VISUAL (EN PARALELO, 1-1.5 horas)
    ├─ Developer (1-1.5h) [Usa template, código simple]
    └─ Visual (20-30 min) [3-4 wireframes clave]
    ↓
QA - MÁXIMO 3 CICLOS DEVELOPER-QA (10-15 min)
    ├─ Ciclo 1: PASS ✅ → Listo | FAIL → Feedback
    ├─ Ciclo 2: PASS ✅ → Listo | FAIL → Feedback
    └─ Ciclo 3: PASS ✅ → Listo | FAIL → Pausa y decisión del PM
    ↓
PRESENTADOR (OPCIONAL, solo si PM solicita) [30-45 min]
    ↓
ENTREGA (Prototipo clickeable + Documentación)

⏱️ TOTAL: 2-2.5 horas (sin Presentador)
```

---

## 📋 ENTREGABLES FINALES

Si **TODO SALE BIEN**, recibirás en `outputs/prototipo_<tema>/`:

```
PROTO_SPEC_2026-10-01-AUTOEX-001.json    ← Idea + 2 US + 8 Criterios
PROTO_CODE_2026-10-01-AUTOEX-001.jsx     ← Componentes React (funcional)
PROTO_PREVIEW_2026-10-01-AUTOEX-001.html ← Preview standalone para QA/Presentador
PROTO_WIREFRAMES_2026-10-01-AUTOEX-001.md ← 3-4 Wireframes clave
PROTO_QA_REPORT_2026-10-01-AUTOEX-001.json ← Validación (8/8 PASS)
PROTO_STATE_2026-10-01-AUTOEX-001.json   ← Historial completo
PRESENTATION_2026-10-01-AUTOEX-001.html ← Deck visual con wireframe interactivo (OPCIONAL)
PRESENTATION_2026-10-01-AUTOEX-001.md   ← Resumen del deck (OPCIONAL)
```

**Total: 6-8 archivos** (sin Presentador si PM no lo solicita)

---

## 🚨 ALERTA: Qué Pasa si QA Falla 3 Ciclos

```
🚨 ⚠️ ALERTA - QA DETENIDO ⚠️

❌ QA falló en 3 ciclos Developer-QA consecutivos

Criterios que NO PASAN:
  • CA-001: Botón no deshabilita cuando debería
  • CA-004: Layout móvil no es responsive

Pistas de fix proporcionadas para Developer.

Posibles causas:
  1. Especificación poco clara
  2. Desconexión Developer-Especificador
  3. Prototipo necesita repensarse

Acciones:
  → Revisar especificación vs criterios
  → Considerar cambios en requisitos
    → Solo iniciar otra sesión si el PM lo autoriza explícitamente

FLUJO SE DETIENE AQUÍ - PM interviene
```

---

## 🔑 PUNTOS CLAVE

### ✅ OPTIMIZACIONES CLAVE

- ✅ **Prototipo clickeable en 1-1.5h** (no 4-6h)
- ✅ **Wireframes en 20-30 min** (no 2-3h)
- ✅ **Timeline total: 2-2.5 horas** (no 8-10h)
- ✅ **Loop QA con máximo 3 ciclos** y pausa para decisión del PM
- ✅ **Entregables consolidados: 5-6 archivos** (no 10+)
- ✅ **Developer usa template** (velocidad)
- ✅ **Especificación ligera** (8 criterios, 2 US)
- ✅ **PM intervención mínima** (automático)

### ℹ️ CARACTERÍSTICAS

- ℹ️ Sin autenticación real
- ℹ️ Sin base de datos persistente (mock data)
- ℹ️ **Prototipo = funcional, no producción**
- ℹ️ Sin tests unitarios
- ℹ️ CSS básico pero responsive
- ℹ️ Máximo 3 ciclos Developer-QA; sin cuarto ciclo automático

---

## 🎯 PRÓXIMOS PASOS

### **Ahora:**
1. ✅ Tienes 9 MDs en outputs/
2. ✅ Estás en este chat
3. 👉 **Escribe tu requisito aquí**

### **Después:**
1. Yo arrancaré automáticamente
2. Leeré INDEX.md → 01_ORCHESTRATOR.md
3. Dispararé agentes en orden
4. Si falla: usaré feedback para Developer
5. Si QA pasa 3x: LISTO
6. Reportaré resultado final

---

## 💬 EJEMPLO: CÓMO INICIAR

### Tú escribes:

```
Crea un prototipo que separe el documento de autoexclusión 
y revocación en dos apartados independientes dentro del 
perfil del jugador. Actualmente están en un apartado que 
causa confusión. El equipo legal debe aprobar documentos. 
Autoexclusión indefinida no se puede revocar, pero temporal sí.
```

### Yo respondo:

```
✅ Sesión iniciada: PROTO-2026-10-01-001

🔄 Ejecutando Capturador...
[Lee 02_AGENT_CAPTURADOR.md]
[Genera IDEA-2026-10-01-AUTOEX-001.json]
✅ Capturador completado

🔄 Ejecutando Validador...
[Lee 03_AGENT_VALIDADOR.md]
[Valida viabilidad]
✅ Validador: GO (score 0.92)

🔄 Ejecutando Especificador...
[... etc ...]
```

---

## 📞 SOPORTE

Si algo no está claro:

1. **Lee INDEX.md** - 90% de preguntas se resuelven aquí
2. **Lee 01_ORCHESTRATOR.md** - Entiende el flujo completo
3. **Lee el agente específico** - Si quieres saber qué hace cada uno
4. **Pregunta aquí** - Si algo no está cubierto

---

## ✨ RESUMEN

| Aspecto | Estado |
|---------|--------|
| Archivos sistema | ✅ 9 MDs optimizados |
| Flujo definido | ✅ 7 pasos (rápido) |
| Loop QA | ✅ Máximo 3 ciclos; después decide PM |
| Entregables | ✅ 5-6 archivos (consolidados) |
| **Timeline real** | ✅ **2-2.5 horas** |
| Velocidad Developer | ✅ +300% más rápido |
| Enfoque | ✅ Prototipo funcional, no perfecto |
| PM Intervención | ✅ Mínima (casi automático) |

---

## 🚀 LISTO PARA COMENZAR

**Escribe tu requisito aquí y comenzamos:**

```
"Crea un prototipo que..."
```

---

**Última actualización:** 1 Octubre 2026  
**Archivos:** 9 Markdown + Este README  
**Estado:** ✅ LISTO PARA USAR
