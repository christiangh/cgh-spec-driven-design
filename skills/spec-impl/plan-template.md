# Plan técnico — SPEC NNN — [Título del spec]

> **Spec:** `specs/NNN-slug/spec.md`
> **Rama:** `feat/spec-NNN-slug`
> **Fecha:** AAAA-MM-DD

<!--
GUÍA GENERAL (los comentarios y ejemplos se eliminan al generar el plan)
- El spec dice QUÉ y POR QUÉ; el plan, CÓMO. Nunca lo contradice ni amplía: nada fuera de «Entra».
- Una sección por parte, en el orden sugerido, añadida al planificar esa parte. Estados: Pendiente, En curso, Hecha.
- Convenciones del proyecto (README, CLAUDE.md, AGENTS.md): estructura, estilo, comandos de build/test/lint.
- Pasos pequeños: cada uno deja el proyecto compilando y revisable en un diff corto. Markdown estándar para GitHub.
-->

## PARTE-NN: [Título de la parte]

**Estado:** Pendiente

### Enfoque

<!-- 2-5 frases: cómo se resuelve y por qué así.
Ej.: "Nuevo módulo de grupos con repositorio en memoria, siguiendo la estructura por funcionalidades del proyecto." -->

### Archivos

<!-- A crear o modificar, con motivo. Ej.: "- `src/grupos/grupo.ts` (nuevo): entidad Grupo. - `src/app.ts`: registrar rutas de grupos." -->

### Pasos

<!-- Checklist numerada; [x] al completar cada paso.
Ej.:
- [ ] 1. Crear entidad Grupo con validación de nombre (RF-001).
- [ ] 2. Invitar y listar miembros (RF-005).
- [ ] 3. Pruebas de HU-03.
-->

### Pruebas y trazabilidad

<!-- Cada HU, RF y RNF de «Cubre» y el «Hecho cuando», con el paso que lo implementa y la prueba que lo demuestra (incluidos criterios Dado/Cuando/Entonces). Ninguno sin paso ni prueba; se revisa al cerrar la parte. Después, comandos del proyecto.
Ej.:
| Elemento | Paso | Prueba |
|----------|------|--------|
| RF-001 | 1 | Test unitario de creación de grupo |
| HU-03 / Hecho cuando | 2, 3 | Test de integración con 3 miembros |

Comandos: `npm test`, `npm run lint`
-->

### Skills y agentes

<!-- Del proyecto o la sesión, y para qué. Si no hay: "Ninguno". Ej.: "- `code-reviewer`: revisión al cerrar la parte." -->

### Decisiones técnicas

<!-- Ambigüedades resueltas, con opción y motivo. Si no hay: "Ninguna".
Ej.: "- **Sí:** identificadores UUID. Evita colisiones al sincronizar. - **No:** autoincremental. Expone el volumen de datos." -->
