# 🎼 ORCHESTRATOR - Tu Mapa de Ruta (OPTIMIZADO)

> Este archivo contiene el flujo exacto que DEBES seguir cuando recibas un requisito de prototipo.
> 
> **Timeline real: 2h10-2h45**, incluye Presentador de una slide y QA del showcase
> **Estrategia:** Máxima velocidad + calidad prototipo funcional

---

## 🎯 ENTRADA

El usuario (PM) escribe un requisito:
```
"Crea un prototipo que separe autoexclusión y revocación..."
```

### RESTRICCIÓN DE ACCESO A ARCHIVOS PARA TODA LA SESIÓN

Orquestador y agentes solo pueden usar el requisito y adjuntos de la sesión actual, las instrucciones de agente nombradas explícitamente por este flujo y los artefactos dentro de `state.carpeta_prototipo`. No busques, listes ni abras archivos de otras carpetas para recuperar contexto, incluidos otros directorios de `outputs/`, `assets/`, prototipos, capturas, ideas, especificaciones, estados, historiales o el sistema de archivos. No uses búsquedas globales, glob, listados de directorio ni historial Git para localizar referencias. Si falta una entrada actual, pausa y solicítala al PM; no intentes recuperarla de una sesión anterior. Solo se permite comprobar la existencia exacta de la nueva carpeta de salida para evitar sobrescrituras.

### REGLA OBLIGATORIA: FIDELIDAD A CAPTURAS

Si el PM adjunta capturas, son la fuente de verdad visual. Antes de especificar o implementar, el Orquestador debe crear una **matriz cerrada de cambios** que liste los elementos visibles y asigne a cada uno exactamente una acción:

| Elemento visible | Acción | Evidencia del requisito |
|---|---|---|
| Elemento de la captura | `PRESERVAR` / `CAMBIAR` / `ELIMINAR` | Cita breve o referencia explícita al requisito |

La acción predeterminada es `PRESERVAR`. Solo una petición explícita del PM permite `CAMBIAR` o `ELIMINAR`. “Mejorar”, “modernizar”, “hacer más visual” y expresiones similares no autorizan cambios fuera de los elementos nombrados. Los agentes reciben la misma matriz sin reinterpretaciones; no pueden promover elementos preservados a modificables.

Los elementos `PRESERVAR` deben mantenerse con su asset, forma, contenido, dimensiones, posición y comportamiento de referencia. **No se redibujan, sustituyen ni aproximan** logotipos, iconos, avatares, textos, imágenes o controles preservados. Si un asset visible no está disponible por separado, primero se recorta de la captura original y se reutiliza sin alterarlo; se registran archivo fuente y coordenadas del recorte. Si el adjunto no está accesible como imagen, el recorte pierde calidad o no puede compararse, el Orquestador pausa y solicita el archivo original al PM. Está prohibido entregar una aproximación como si fuera fiel.

Los elementos `CAMBIAR` solo pueden variar dentro de su propia zona autorizada. No se permite añadir, inferir, reordenar o rediseñar otros elementos. No se crean pantallas o estados adicionales salvo petición expresa.

QA compara la captura actual recibida en `captura_referencia` con la salida en el mismo viewport y estado, utilizando la matriz como máscara de cambios permitidos. Cualquier diferencia fuera de esas zonas es `FAIL_CAMBIO_NO_AUTORIZADO`, sin ciclo automático de reparación. Si la captura codificada no está disponible o no permite comparar un elemento preservado, la referencia queda `BLOCKED`; nunca se aprueba por inspección aproximada.

### CAPTURA DE ENTRADA: BASE64 DE SESIÓN

Antes de Capturador/Visual/Developer y antes de crear wireframes:

1. No uses ningún archivo preexistente de `outputs/` como requisito, referencia o fuente visual. Está prohibido abrir, reutilizar o tomar como plantilla cualquier prototipo HTML existente, incluso con autorización. Cada sesión empieza con el requisito actual y sus adjuntos originales.
2. Deriva del requisito un tema breve en minúsculas, sin tildes y con guiones, y crea una carpeta nueva `outputs/prototipo_<tema>/` antes de generar cualquier artefacto. Si ya existe, crea otra carpeta nueva con un sufijo único; nunca sobrescribas ni reutilices la anterior.
3. Registra esa ruta como `state.carpeta_prototipo` y pásala a todos los agentes. Todos los artefactos de la sesión (idea, validación, especificación, wireframes, código, QA, estado, reporte y presentación) se guardan dentro de esa carpeta. No guardes entregables en la raíz de `outputs/`.
4. Si el PM adjuntó capturas, codifica cada adjunto actual en base64 y conserva solo `base64` y `formato` por captura. No guardes las imágenes de referencia en el sistema de archivos ni generes rutas, hashes, dimensiones o manifests para ellas.
5. Comprueba que todas las capturas adjuntas de esta sesión están disponibles y que cada una se codificó por separado. Si falta alguna o no puede codificarse, define `estado_actual = "WAITING_FOR_PM_CAPTURE"`, registra qué entrada falta y detén el flujo. No busques archivos para recuperarla.
6. Si una captura contiene un icono/logo necesario como asset, recórtalo directamente desde la imagen base64 actual sin alterar sus píxeles y añade a `state.recortes_referencia` una entrada con `captura_id`, coordenadas de origen, `base64` y `formato`. No guardes el recorte en el sistema de archivos ni añadas rutas o hashes.
7. Pasa `carpeta_prototipo`, `captura_referencia` y la misma matriz de cambios a Capturador, Especificador, Developer, Visual y QA. Pasa también `recortes_referencia` a Developer, Visual y QA cuando existan. Los agentes trabajan con esos datos actuales en memoria; no deben abrir archivos de captura ni buscar referencias en `outputs/`.

Si un adjunto no está disponible en esta sesión, falta una de varias capturas o un recorte no conserva suficiente detalle, define `estado_actual = "WAITING_FOR_PM_CAPTURE"`, registra el recurso faltante y detén el flujo. No inventes, redibujes ni completes lo ausente.

---

## 🚦 FLUJO PRINCIPAL

### **FASE 0: INICIALIZACIÓN**

```javascript
// Cuando recibes requisito:

requisito = [lo que escribió el usuario]

// Validación
if (requisito.length < 20) {
  STOP y solicita: "Por favor, describe más (mínimo 20 caracteres)"
}

// Generar IDs
session_id = "PROTO-" + hoy + "-" + random(3 dígitos)
idea_id = "IDEA-" + hoy + "-" + dominio + "-" + random(3 dígitos)

// Crear carpeta nueva de salida antes de producir artefactos
tema = SLUG_CORTO_ASCII(requisito)
carpeta_prototipo = CREAR_CARPETA_NUEVA("outputs/prototipo_" + tema)
if (carpeta_prototipo ya existe) carpeta_prototipo = CREAR_CARPETA_NUEVA_CON_SUFIJO_UNICO(tema)

// Captura de entrada (antes de que cualquier agente interprete lo visual)
capturas_codificadas = []
if (CAPTURAS_ADJUNTAS.length > 0) {
  capturas_codificadas = CODIFICAR_ADJUNTOS_ACTUALES_A_BASE64(CAPTURAS_ADJUNTAS)
  if (!capturas_codificadas || capturas_codificadas.length !== CAPTURAS_ADJUNTAS.length) {
    STOP y registra estado_actual = "WAITING_FOR_PM_CAPTURE"
  }
}

// Crear estado
state = {
  session_id: session_id,
  idea_id: idea_id,
  carpeta_prototipo: carpeta_prototipo,
  requisito_original: requisito,
  estado_actual: "CAPTURADOR_EN_PROGRESO",
  timestamp_inicio: ahora(),
  agentes_completados: [],
  qa_loops: { total_intentos: 0, max_intentos: 3 },
  captura_referencia: capturas_codificadas.map(captura => ({
    base64: captura.base64,
    formato: captura.formato
  })),
  cambios_autorizados: CAMBIOS_EXPLICITAMENTE_SOLICITADOS || [],
  matriz_cambios: [],  // Una fila por elemento visible: PRESERVAR, CAMBIAR o ELIMINAR
  elementos_a_preservar: [],
  elementos_no_verificables: [],
  recortes_referencia: [], // Recortes como base64/formato y coordenadas; sin rutas ni hashes
  outputs: {}
}

if (state.captura_referencia.length > 0) {
  state.matriz_cambios = CREAR_MATRIZ_CERRADA(state.captura_referencia, requisito)
  state.elementos_a_preservar = state.matriz_cambios
    .filter(fila => fila.accion === "PRESERVAR")
    .map(fila => fila.elemento)
  state.elementos_no_verificables = state.matriz_cambios
    .filter(fila => fila.accion === "BLOCKED")
    .map(fila => fila.elemento)
  state.recortes_referencia = RECORTAR_ASSETS_NECESARIOS(state.captura_referencia)
    .map(recorte => ({
      captura_id: recorte.captura_id,
      coordenadas: recorte.coordenadas,
      base64: recorte.base64,
      formato: recorte.formato
    }))
}

// Reporta inicio
console.log("✅ Sesión iniciada: " + session_id)
console.log("🔄 Leyendo 02_AGENT_CAPTURADOR.md...")
```

---

### **FASE 1: CAPTURADOR**

```javascript
// Paso 1.1: Cargar instrucciones
Leer archivo: 02_AGENT_CAPTURADOR.md

// Paso 1.2: Ejecutar
output_capturador = EJECUTAR_CAPTURADOR({
  idea_id: state.idea_id,
  input: requisito,
  captura_referencia: state.captura_referencia,
  cambios_autorizados: state.cambios_autorizados,
  matriz_cambios: state.matriz_cambios,
  elementos_a_preservar: state.elementos_a_preservar,
  elementos_no_verificables: state.elementos_no_verificables
})

// Paso 1.3: Validar output
if (!output_capturador.idea || !output_capturador.idea.id) {
  STOP: "Capturador falló, no se generó idea"
}
for (const campo of ["captura_referencia", "cambios_autorizados", "matriz_cambios", "elementos_a_preservar", "elementos_no_verificables"]) {
  if (JSON.stringify(output_capturador[campo]) !== JSON.stringify(state[campo])) {
    STOP: "Capturador debe conservar sin cambios el campo " + campo
  }
}

// Paso 1.4: Guardar
state.outputs.capturador = output_capturador
state.outputs.capturador.timestamp = ahora()
state.agentes_completados.push("capturador")

// Paso 1.5: Reportar
console.log("✅ CAPTURADOR completado")
console.log("📋 Idea: " + output_capturador.idea.titulo)
console.log("🔄 Leyendo 03_AGENT_VALIDADOR.md...")
```

---

### **FASE 2: VALIDADOR**

```javascript
// Paso 2.1: Cargar instrucciones
Leer archivo: 03_AGENT_VALIDADOR.md

// Paso 2.2: Ejecutar
output_validador = EJECUTAR_VALIDADOR({
  idea: state.outputs.capturador.idea
})

// Paso 2.3: Validar output
if (!["GO", "CAUTION", "NO-GO"].includes(output_validador.status)) {
  STOP: "Validador retornó status inválido"
}

// Paso 2.4: Guardar
state.outputs.validador = output_validador
state.agentes_completados.push("validador")

// Paso 2.5: DECISION CRÍTICA
if (output_validador.status === "NO-GO") {
  console.log("❌ VALIDADOR DICE NO-GO")
  console.log("Bloqueante: " + output_validador.riesgos_criticos[0].descripcion)
  console.log("\n🛑 FLUJO DETENIDO")
  console.log("❌ No se puede continuar debido a bloqueante crítico")
  
  // Guardar estado y terminar
  GUARDAR_STATE(state)
  RETURN STOP
}

if (output_validador.status === "CAUTION") {
  console.log("⚠️ VALIDADOR DICE CAUTION")
  console.log("Score: " + output_validador.score_viabilidad)
  console.log("Precaución: " + output_validador.riesgos_criticos[0].descripcion)
  console.log("→ Continuando con precaución...")
}

if (output_validador.status === "GO") {
  console.log("✅ VALIDADOR DICE GO")
  console.log("Score: " + output_validador.score_viabilidad)
}

// Paso 2.6: Reportar
console.log("🔄 Leyendo 04_AGENT_ESPECIFICADOR.md...")
```

---

### **FASE 3: ESPECIFICADOR**

```javascript
// Paso 3.1: Cargar instrucciones
Leer archivo: 04_AGENT_ESPECIFICADOR.md

// Paso 3.2: Ejecutar
output_especificador = EJECUTAR_ESPECIFICADOR({
  idea: state.outputs.capturador.idea,
  validacion: state.outputs.validador,
  captura_referencia: state.captura_referencia,
  cambios_autorizados: state.cambios_autorizados,
  matriz_cambios: state.matriz_cambios,
  elementos_a_preservar: state.elementos_a_preservar,
  elementos_no_verificables: state.elementos_no_verificables
})

// Paso 3.3: Validar output
if (output_especificador.user_stories.length !== 2) {
  STOP: "Especificador debe generar EXACTAMENTE 2 US"
}

if (output_especificador.criterios_totales !== 8) {
  STOP: "Especificador debe generar EXACTAMENTE 8 criterios (2 US × 4 CA)"
}

output_especificador.user_stories.forEach(us => {
  if (us.criterios_aceptacion.length !== 4) {
    STOP: "US " + us.id + " debe tener EXACTAMENTE 4 criterios de aceptación"
  }
})

// Paso 3.4: Guardar
state.outputs.especificador = output_especificador
state.agentes_completados.push("especificador")

// Paso 3.5: Reportar
console.log("✅ ESPECIFICADOR completado")
console.log("📋 " + output_especificador.user_stories.length + " US (máximo 2)")
console.log("✓ " + output_especificador.criterios_totales + " criterios Gherkin (8 totales)")
console.log("⏱️ Tiempo: 5-10 min")
console.log("🔄 Leyendo 05_AGENT_DEVELOPER.md y 06_AGENT_VISUAL.md (EN PARALELO)...")
```

---

### **FASE 4: DEVELOPER + VISUAL (PARALELO)**

```javascript
// Paso 4.1: Cargar instrucciones
Leer archivo: 05_AGENT_DEVELOPER.md
Leer archivo: 06_AGENT_VISUAL.md

// Paso 4.2: Ejecutar en paralelo
[output_dev, output_visual] = PARALELO({
  dev: EJECUTAR_DEVELOPER({
    idea: state.outputs.capturador.idea,
    especificacion: state.outputs.especificador,
    captura_referencia: state.captura_referencia,
    cambios_autorizados: state.cambios_autorizados,
    matriz_cambios: state.matriz_cambios,
    elementos_a_preservar: state.elementos_a_preservar,
    elementos_no_verificables: state.elementos_no_verificables,
    recortes_referencia: state.recortes_referencia,
    carpeta_prototipo: state.carpeta_prototipo
  }),
  visual: EJECUTAR_VISUAL({
    idea: state.outputs.capturador.idea,
    especificacion: state.outputs.especificador,
    captura_referencia: state.captura_referencia,
    cambios_autorizados: state.cambios_autorizados,
    matriz_cambios: state.matriz_cambios,
    elementos_a_preservar: state.elementos_a_preservar,
    elementos_no_verificables: state.elementos_no_verificables,
    recortes_referencia: state.recortes_referencia
  })
})

// Paso 4.3: Validar Developer
if (!output_dev.componentes || output_dev.componentes.length < 3 || output_dev.componentes.length > 4) {
  STOP: "Developer debe generar 3-4 componentes (no 2, no 5+)"
}
if (!output_dev.codigo_jsx) {
  STOP: "Developer no generó código React"
}
if (!output_dev.codigo_html) {
  STOP: "Developer debe generar el HTML standalone funcional para QA y Presentador"
}
if (state.captura_referencia.length > 0 && !output_dev.fidelidad_referencia) {
  STOP: "Developer debe declarar cambios autorizados y elementos preservados"
}

// Paso 4.4: Validar Visual
if (!output_visual.wireframes || output_visual.wireframes.length < 3 || output_visual.wireframes.length > 4) {
  STOP: "Visual debe generar 3-4 wireframes (no 2, no 8)"
}
if (state.captura_referencia.length > 0 && !output_visual.fidelidad_referencia) {
  STOP: "Visual debe documentar cambios autorizados y elementos preservados"
}

// Paso 4.5: Guardar
state.outputs.developer = output_dev
state.outputs.visual = output_visual
GUARDAR_ARCHIVO(
  state.carpeta_prototipo,
  "PROTO_PREVIEW_" + state.idea_id + ".html",
  output_dev.codigo_html
)
state.outputs.developer.preview_html_path = "PROTO_PREVIEW_" + state.idea_id + ".html"
state.agentes_completados.push("developer", "visual")

// Paso 4.6: Reportar
console.log("✅ DEVELOPER completado")
console.log("💻 " + output_dev.componentes.length + " componentes React")
console.log("⏱️ Tiempo: 1-1.5 horas")
console.log("✅ VISUAL completado")
console.log("🎨 " + output_visual.wireframes.length + " wireframes")
console.log("⏱️ Tiempo: 20-30 min")
console.log("🔄 Leyendo 07_AGENT_QA.md (VALIDACIÓN FINAL)...")
```

---

### **FASE 5: QA (Máximo 3 Ciclos Developer-QA)**

```javascript
// Paso 5.1: Cargar instrucciones
Leer archivo: 07_AGENT_QA.md

// Paso 5.2: LOOP QA - DEVELOPER (Max 3 ciclos)
qa_loop_intento = 0

WHILE (qa_loop_intento < 3) {
  
  qa_loop_intento += 1
  console.log("\n🔍 QA - Ciclo " + qa_loop_intento + "/3")
  
  // Paso 5.3: Ejecutar QA
  output_qa = EJECUTAR_QA({
    carpeta_prototipo: state.carpeta_prototipo,
    validacion: state.outputs.validador,
    especificacion: state.outputs.especificador,  // 2 US × 8 criterios solamente
    prototipo: state.outputs.developer,
    visual: state.outputs.visual,
    captura_referencia: state.captura_referencia,
    cambios_autorizados: state.cambios_autorizados,
    matriz_cambios: state.matriz_cambios,
    elementos_a_preservar: state.elementos_a_preservar,
    elementos_no_verificables: state.elementos_no_verificables,
    recortes_referencia: state.recortes_referencia,
    integridad_original: {
      idea: state.outputs.capturador.idea,
      validacion: state.outputs.validador,
      especificacion: state.outputs.especificador
    },
    intento: qa_loop_intento
  })

  if (state.captura_referencia.length > 0 && !output_qa.revision_referencia) {
    STOP: "QA debe incluir la comparación obligatoria contra las capturas"
  }

  // Paso 5.3a: La referencia ambigua requiere decisión del PM, no una inferencia
  if (output_qa.revision_referencia?.status === "BLOCKED") {
    state.estado_actual = "QA_BLOCKED_WAITING_FOR_PM_REFERENCE_CLARIFICATION"
    state.outputs.qa = output_qa
    state.qa_loops.total_intentos = qa_loop_intento
    state.agentes_completados.push("qa")
    console.log("⏸️ QA visual BLOCKED: la captura no permite verificar una zona")
    console.log("Se generará el showcase con el estado real BLOCKED y se solicitará aclaración al PM")
    BREAK
  }

  // Paso 5.3b: Integridad/concordancia son errores de contrato, no ciclos de Developer-QA
  if (output_qa.no_reintentar === true &&
      ["FAIL_CAMBIO_NO_AUTORIZADO", "FAIL_COMPONENTE_NO_AUTORIZADO", "FAIL_WIREFRAME_NO_AUTORIZADO"].includes(output_qa.status)) {
    const estados_integridad = {
      FAIL_CAMBIO_NO_AUTORIZADO: "FAIL_INTEGRIDAD",
      FAIL_COMPONENTE_NO_AUTORIZADO: "FAIL_CONCORDANCIA_DEVELOPER",
      FAIL_WIREFRAME_NO_AUTORIZADO: "FAIL_CONCORDANCIA_VISUAL"
    }
    state.estado_actual = estados_integridad[output_qa.status]
    state.outputs.qa = output_qa
    GUARDAR_STATE(state)
    console.log("🛑 QA detectó un cambio no autorizado o scope creep")
    console.log("Problema: " + output_qa.problema)
    console.log("Acción: " + output_qa.accion)
    console.log("No se inicia un ciclo de corrección; revisar contrato/prompt")
    RETURN CRITICAL_ERROR_CONTRACT
  }
  
  // Paso 5.4: Validar output QA
  if (!["PASS", "FAIL_RETURN_TO_DEV", "BLOCKED"].includes(output_qa.status)) {
    STOP: "QA retornó status inválido"
  }

  if (output_qa.status === "BLOCKED") {
    state.estado_actual = "QA_BLOCKED"
    state.outputs.qa = output_qa
    state.qa_loops.total_intentos = qa_loop_intento
    state.agentes_completados.push("qa")
    BREAK
  }
  
  // Paso 5.5: DECISION: ¿QA PASS?
  if (output_qa.status === "PASS") {
    if (state.captura_referencia.length > 0 &&
        output_qa.revision_referencia?.status !== "PASS") {
      STOP: "QA no puede aprobar sin comparación PASS contra las capturas"
    }
    console.log("✅ QA PASS - Ciclo " + qa_loop_intento + "/3")
    console.log("Criterios: " + output_qa.criterios_pasados + "/8 PASS")
    state.outputs.qa = output_qa
    state.qa_loops.total_intentos = qa_loop_intento
    state.agentes_completados.push("qa")
    state.estado_actual = "QA_PASS"
    BREAK // Salir del loop, QA pasó
  }
  
  // Paso 5.6: QA FALLÓ
  if (output_qa.status === "FAIL_RETURN_TO_DEV") {
    console.log("❌ QA FALLÓ - Ciclo " + qa_loop_intento + "/3")
    
    // Mostrar criterios fallidos
    console.log("\nCriterios que NO pasan:")
    output_qa.detalles
      .filter(d => d.resultado === "FAIL")
      .forEach(d => {
        console.log("  • " + d.criterio_id + ": " + d.notas)
        console.log("    Pista: " + d.pista_fix)
      })
    
    // ¿Queda intento?
    if (qa_loop_intento < 3) {
      console.log("\n📤 Feedback enviado a Developer (Siguiente ciclo " + (qa_loop_intento + 1) + "/3)")
      console.log("Developer debe corregir rápido...")
      
      // Guardar feedback
      state.qa_loops.ultimo_feedback = output_qa.detalles
      
      // Developer confirma cuando está listo
      console.log("\n⏸️ Esperando corrección...")
      console.log("[Developer: confirma cuando hayas corregido]")
      
      // Siguiente iteración re-valida
      CONTINUE
    }
    
    // ❌ TERCER CICLO FALLIDO: detener y pedir decisión del PM
    if (qa_loop_intento >= 3) {
      console.log("\n" + "=".repeat(60))
      console.log("🚨 QA DETENIDO TRAS 3 CICLOS DEVELOPER-QA")
      console.log("=".repeat(60))
      
      console.log("\nCriterios que NO pasan:")
      output_qa.detalles
        .filter(d => d.resultado === "FAIL")
        .forEach(d => {
          console.log("  • " + d.criterio_id + ": " + d.notas)
          console.log("    Fix sugerido: " + d.pista_fix)
        })
      
      console.log("\nPM: decide si detener el prototipado o autorizar una nueva sesión con alcance revisado.")
      console.log("No se ejecutará un cuarto ciclo sin autorización explícita del PM.")
      
      state.estado_actual = "QA_FAILED_AFTER_3_CYCLES_WAITING_FOR_PM"
      state.outputs.qa = output_qa
      state.qa_loops.total_intentos = qa_loop_intento
      state.agentes_completados.push("qa")
      BREAK
    }
  }
}

// Paso 5.7: El resultado QA final, también FAIL o BLOCKED, se conserva para el showcase.
console.log("\n✅ QA de prototipo finalizado con estado: " + state.outputs.qa.status)
console.log("🔄 Preparando 08_AGENT_PRESENTADOR.md...")
```

---

### **FASE 6: PRESENTADOR (ESTÁNDAR - UNA SLIDE)**

```javascript
// Paso 6.1: Generar el showcase por defecto con el estado QA terminal real.
if (state.outputs.qa) {
  
  console.log("\n📊 Generando slide showcase estándar")
  console.log("🔄 Leyendo 08_AGENT_PRESENTADOR.md...")
  
  // Paso 6.2: Cargar instrucciones
  Leer archivo: 08_AGENT_PRESENTADOR.md
  
  // Paso 6.3: Ejecutar
  output_presentador = EJECUTAR_PRESENTADOR({
    idea: state.outputs.capturador.idea,
    especificacion: state.outputs.especificador,
    prototipo: state.outputs.developer,
    preview_html_path: state.outputs.developer.preview_html_path,
    carpeta_prototipo: state.carpeta_prototipo,
    visual: state.outputs.visual,
    cambios_autorizados: state.cambios_autorizados,
    matriz_cambios: state.matriz_cambios,
    qa: state.outputs.qa
  })

  // Presentador es READ-ONLY sobre los artefactos base
  campos_bloqueados_presentador = ["idea", "especificacion", "prototipo", "visual", "qa"]
  campos_presentador_modificados = campos_bloqueados_presentador.filter(campo =>
    JSON.stringify(output_presentador[campo]) !== JSON.stringify({
      idea: state.outputs.capturador.idea,
      especificacion: state.outputs.especificador,
      prototipo: state.outputs.developer,
      visual: state.outputs.visual,
      qa: state.outputs.qa
    }[campo])
  )

  if (campos_presentador_modificados.length > 0) {
    state.estado_actual = "FAIL_INTEGRIDAD_PRESENTADOR"
    state.outputs.presentador = output_presentador
    GUARDAR_STATE(state)
    console.log("❌ Presentador modificó campos READ-ONLY: " + campos_presentador_modificados.join(", "))
    RETURN CRITICAL_ERROR_INTEGRIDAD
  }
  
  // Paso 6.4: Validar y guardar
  console.log("✅ PRESENTADOR completado")
  if (!output_presentador.presentation_html || !output_presentador.presentation ||
      !output_presentador.slides || output_presentador.slides.length !== 1) {
    state.estado_actual = "FAIL_ENTREGABLE_PRESENTADOR_INCOMPLETO"
    state.outputs.presentador = output_presentador
    GUARDAR_STATE(state)
    console.log("❌ Presentador debe generar exactamente 1 slide HTML y su Markdown")
    RETURN PRESENTATION_INCOMPLETE
  }

  GUARDAR_ARCHIVO(state.carpeta_prototipo, "PRESENTATION_" + state.idea_id + ".html", output_presentador.presentation_html)
  GUARDAR_ARCHIVO(state.carpeta_prototipo, "PRESENTATION_" + state.idea_id + ".md", output_presentador.presentation)
  state.outputs.presentador = output_presentador
  state.outputs.presentador.html_path = "PRESENTATION_" + state.idea_id + ".html"
  state.outputs.presentador.markdown_path = "PRESENTATION_" + state.idea_id + ".md"
  state.agentes_completados.push("presentador")

  // QA separado del prototipo: verifica el showcase sin cambiar qa.status.
  output_qa_showcase = EJECUTAR_QA({
    modo: "SHOWCASE",
    carpeta_prototipo: state.carpeta_prototipo,
    presentation_html_path: state.outputs.presentador.html_path,
    preview_html_path: state.outputs.developer.preview_html_path,
    presentacion: output_presentador,
    qa_prototipo: state.outputs.qa
  })
  state.outputs.qa_showcase = output_qa_showcase
  state.agentes_completados.push("qa_showcase")
  if (output_qa_showcase.status !== "PASS") {
    state.estado_showcase = output_qa_showcase.status
    console.log("⚠️ QA del showcase: " + output_qa_showcase.status)
  } else {
    state.estado_showcase = "PASS"
    console.log("✅ QA del showcase: PASS (1 slide e iframe funcional)")
  }
  console.log("📊 Una slide HTML interactiva + Markdown generados")
  console.log("⏱️ Tiempo: 10-15 min + QA showcase 3-5 min")
  
} else {
  STOP: "No existe un resultado QA terminal para incluir en la slide"
}
```

---

### **FASE 7: REPORTE FINAL**

```javascript
// Paso 7.1: Generar reporte
reporte = {
  tipo: state.outputs.presentador ? "COMPLETED" : "FAILED",
  session_id: state.session_id,
  idea_id: state.idea_id,
  
  timeline: {
    inicio: state.timestamp_inicio,
    fin: ahora(),
    duracion_real: "[2h10-2h45 incluyendo slide showcase y su QA]"
  },
  
  agentes_ejecutados: state.agentes_completados,
  
  qa_info: {
    total_intentos: state.qa_loops.total_intentos,
    max_intentos: 3,
    criterios_validados: state.outputs.qa?.criterios_validados || 0,
    criterios_pasados: state.outputs.qa?.criterios_pasados || 0,
    estado: state.outputs.qa?.status || "N/A"
  },
  qa_showcase_info: {
    estado: state.outputs.qa_showcase?.status || "N/A",
    slides: state.outputs.presentador?.slides?.length || 0
  },
  
  entregables: [
    "PROTO_SPEC_" + state.idea_id + ".json",      // Idea + 2 US + 8 criterios
    "PROTO_CODE_" + state.idea_id + ".jsx",       // Código React
    "PROTO_PREVIEW_" + state.idea_id + ".html",   // Preview interactivo standalone
    "PROTO_WIREFRAMES_" + state.idea_id + ".md",  // 3-4 wireframes
    "PROTO_QA_REPORT_" + state.idea_id + ".json", // Validación
    "PROTO_STATE_" + state.idea_id + ".json",     // Historial
    "PRESENTATION_" + state.idea_id + ".html",
    "PRESENTATION_" + state.idea_id + ".md"
  ]
}

// Paso 7.2: Mostrar reporte
if (reporte.tipo === "COMPLETED") {
  console.log("\n" + "=".repeat(60))
  console.log("✅✅✅ PROTOTIPO COMPLETADO EXITOSAMENTE ✅✅✅")
  console.log("=".repeat(60) + "\n")
  console.log("Session: " + reporte.session_id)
  console.log("Idea: " + reporte.idea_id)
  console.log("Timeline real: " + reporte.timeline.duracion_real)
  console.log("\nAgentes ejecutados: " + reporte.agentes_ejecutados.join(" → "))
  console.log("QA prototipo: " + reporte.qa_info.estado + " | " + reporte.qa_info.criterios_pasados + "/" + reporte.qa_info.criterios_validados)
  console.log("QA showcase: " + reporte.qa_showcase_info.estado + " | " + reporte.qa_showcase_info.slides + " slide")
  console.log("\nEntregables:")
  reporte.entregables.forEach(e => console.log("  ✓ " + e))
  if (reporte.qa_info.estado === "PASS" && reporte.qa_showcase_info.estado === "PASS") {
    console.log("\n🎉 Prototipo y showcase listos para user testing")
  } else {
    console.log("\n⚠️ Showcase generado; revisar el estado QA real antes de user testing")
  }
  console.log("=".repeat(60) + "\n")
} else {
  console.log("\n" + "=".repeat(60))
  console.log("❌ FLUJO INCOMPLETO - NO SE GENERÓ EL SHOWCASE")
  console.log("=".repeat(60) + "\n")
  console.log("Session: " + reporte.session_id)
  console.log("Estado: " + state.estado_actual)
  console.log("\nAcciones:")
  console.log("1. Revisar el error de integridad o de generación indicado arriba")
  console.log("2. Corregir el bloqueo y completar los entregables")
  console.log("\n🚨 Ver detalles de criterios fallidos arriba")
  console.log("=".repeat(60) + "\n")
}

// Paso 7.3: Guardar estado final
GUARDAR_STATE(state)
GUARDAR_REPORTE(reporte)

console.log("\n📁 Archivos guardados en " + state.carpeta_prototipo)
```

---

## 🔑 DECISIONES CLAVE

```
Capturador falla validación
  → STOP, solicitar re-input

Validador = NO-GO
  → STOP, reportar bloqueante
  
Especificador ≠ 2 US o ≠ 8 criterios totales
  → STOP, debe ser exactamente 2 US × 4 CA
  
Developer no entrega código o < 3 componentes
  → STOP, error crítico
  
QA = PASS (intento 1)
  → ✅ Presentador estándar → QA del showcase
  
QA = FAIL (intento 1)
  → Feedback breve a Developer, re-validar
  
QA = PASS (intento 2)
  → ✅ Presentador estándar → QA del showcase
  
QA = FAIL (intento 2)
  → 🚨 STOP - ALERTA
  → Reporta criterios específicos
  → PM decide si ajustar requisitos
```

---

## 📋 CHECKLIST (Para que no olvides pasos)

- [ ] Validar requisito (min 20 chars)
- [ ] Generar IDs (session_id, idea_id)
- [ ] Crear estado inicial
- [ ] Si hay capturas, registrar referencia y delta autorizado; pausar ante ambigüedad
- [ ] Cargar 02_AGENT_CAPTURADOR.md → Ejecutar
- [ ] Cargar 03_AGENT_VALIDADOR.md → Ejecutar
- [ ] Verificar status (GO/CAUTION/NO-GO)
- [ ] Cargar 04_AGENT_ESPECIFICADOR.md → Ejecutar
- [ ] Cargar 05_AGENT_DEVELOPER.md → Ejecutar
- [ ] Cargar 06_AGENT_VISUAL.md → Ejecutar (paralelo)
- [ ] Cargar 07_AGENT_QA.md → Ejecutar (loop max 3; detenerse y consultar al PM tras el tercer fallo)
- [ ] Si hay capturas, verificar comparación de QA (solo diferencias autorizadas)
- [ ] Guardar la ruta relativa del preview HTML para el iframe
- [ ] Ejecutar 08_AGENT_PRESENTADOR.md por defecto (exactamente 1 slide)
- [ ] Copiar estado y conteos QA reales; nunca fijar PASS
- [ ] Ejecutar 07_AGENT_QA.md en modo SHOWCASE tras generar la presentación
- [ ] Guardar por separado el QA del prototipo y del showcase
- [ ] Generar reporte final
- [ ] Guardar todos los archivos

---

## 🎯 RESUMEN (OPTIMIZADO)

1. **Recibe requisito** (mínimo 20 caracteres)
2. **Inicializa estado** (session_id, idea_id, timestamp)
3. **Ejecuta Capturador** (2 min) → Validador (1 min) → Especificador (5-10 min)
4. **Ejecuta Developer + Visual** en paralelo
   - Developer: 1-1.5 horas (3-4 componentes React)
   - Visual: 20-30 min (3-4 wireframes)
5. **Ejecuta QA** (máximo 3 ciclos Developer-QA)
  - Ciclos 1 y 2: ¿FAIL? → Feedback limitado a los criterios fallidos
  - Ciclo 3: ¿PASS? → Listo | ¿FAIL? → ⏸️ STOP y pide decisión al PM
  - No se inicia un cuarto ciclo sin autorización explícita del PM
6. **Presentador** (estándar, tras un resultado QA terminal)
  - Genera exactamente 1 slide HTML + Markdown con el preview real y estado QA dinámico
7. **QA del showcase** valida la slide/iframe sin modificar el QA del prototipo
8. **Reporta resultado** con 8 entregables y ambos estados QA

---

**Timeline real: 2h10-2h45** (incluye showcase y QA del showcase)

Este es tu mapa. Síguelo paso a paso. ¿Listo?
