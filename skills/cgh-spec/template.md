# SPEC NNN — [Título corto y descriptivo]

> **Estado:** Borrador
> **Depende de:** —
> **Fecha:** AAAA-MM-DD
> **Autor:** [Nombre]
> **Objetivo:** [Una sola frase.]

<!--
GUÍA GENERAL (los comentarios y ejemplos se eliminan al generar el spec)
- Esta plantilla manda sobre el formato de specs antiguos. Las secciones (opcional) se omiten si no aplican.
- QUÉ y POR QUÉ, nunca CÓMO: sin lenguajes, frameworks, APIs, archivos ni esquemas de BD. Comprensible para negocio y verificable por QA.
- Estados: Borrador, En revisión, Aprobado, Implementado, Obsoleto. Un spec nuevo nace en Borrador.
- Depende de: "SPEC 001, SPEC 002" o "—". ID = número de la carpeta (specs/001-gastos-compartidos/ → SPEC 001); debe existir.
- Objetivo: una frase legible en 5 segundos. Si necesitas dos, divide en varios specs.
  Ej.: "Permitir que un grupo de amigos registre gastos compartidos y vea en todo momento quién debe a quién."
- Redacción: una idea por frase; nombres concretos, nada de "el módulo de…"; sin TODOs (decide o llévalo a Preguntas abiertas con motivo); Markdown estándar para GitHub.
- IDs: HU-01, RF-001, RNF-001, CE-01, PARTE-01.
-->

## 1. Resumen

<!-- 2-4 frases: qué se construye, para quién y qué valor aporta.
Ej.: "Aplicación para que grupos de amigos registren gastos compartidos y sepan quién debe a quién, sin cuentas manuales al final de un viaje." -->

## 2. Contexto y problema

<!-- Situación actual, dolor concreto, por qué ahora; datos si los hay.
Ej.: "Hoy los gastos se apuntan en un chat y se cuadran a mano al volver. Es lento y genera errores." -->

## 3. Objetivos y alcance

### Objetivos

<!-- Resultados perseguidos. Ej.: "- Registrar un gasto en menos de 30 segundos." -->

### Entra

<!-- Lo que cubre este spec. Ej.: "- Crear grupos e invitar miembros. - Registrar gastos con reparto igualitario. - Ver saldos." -->

### Fuera de alcance (para specs futuros)

<!-- Obligatorio. Lo que podría hacerse pero no ahora y lo que salió en las preguntas y se aplazó; evita colarlo "ya que estamos".
Ej.: "- Pagos reales entre usuarios. - Conversión entre divisas." -->

## 4. Usuarios y actores

<!-- Quién interactúa y qué necesita.
Ej.: "- **Miembro**: registra gastos y consulta saldos. - **Creador del grupo**: además invita y expulsa." -->

## 5. Glosario (opcional)

<!-- Términos del dominio con definición de una frase, usados siempre igual. Reutiliza los de specs previos sin redefinirlos.
Ej.: "- **Saldo**: diferencia entre lo pagado y lo que corresponde pagar. - **Liquidar**: dejar un saldo a 0 € con un pago fuera de la aplicación." -->

## 6. Historias de usuario

<!-- Una subsección por historia, con prioridad (P1 imprescindible, P2 importante, P3 deseable), comprobable de forma independiente.
Ej.:
### HU-01: Registrar un gasto (P1)
**Como** miembro, **quiero** registrar un gasto indicando quién pagó y entre quiénes se reparte, **para** que los saldos se actualicen solos.

**Criterios de aceptación**
1. **Dado** un grupo de 3 miembros, **cuando** registro 30 € pagados por mí a partes iguales, **entonces** los otros dos me deben 10 € cada uno.
2. **Dado** un gasto sin importe, **cuando** intento guardarlo, **entonces** se indica que el importe es obligatorio.
-->

## 7. Requisitos funcionales

<!-- Testables y sin ambigüedad: "El sistema debe…". Si algo no está claro: "… [NECESITA ACLARACIÓN: motivo]" (máximo 3 en el spec).
Ej.: "- **RF-001**: El sistema debe permitir crear un grupo con nombre y al menos un miembro." -->

## 8. Requisitos no funcionales

<!-- Rendimiento, seguridad, privacidad, accesibilidad, disponibilidad, legal (RGPD). Medibles y sin tecnología.
Ej.: "- **RNF-001**: Los saldos se muestran actualizados en menos de 2 segundos. - **RNF-002**: Solo los miembros ven los gastos del grupo." -->

## 9. Entidades clave (opcional)

<!-- Solo si hay datos. Conceptos de negocio y relaciones, sin campos ni tipos.
Ej.: "- **Grupo**: miembros que comparten gastos. - **Gasto**: importe pagado por un miembro y repartido entre varios." -->

## 10. Casos límite

<!-- Situaciones extremas, errores y fronteras con su comportamiento esperado.
Ej.: "- Expulsar a un miembro con saldo pendiente → no se permite hasta que sea 0 €." -->

## 11. Criterios de éxito

<!-- Medibles, agnósticos a la tecnología, verificables con sí o no. Antipatrones: "que funcione bien", "buena experiencia", "sin errores".
Ej.: "- **CE-01**: El 90 % registra su primer gasto sin ayuda. - **CE-02**: La suma de saldos de un grupo es siempre 0 €." -->

## 12. Supuestos y dependencias

<!-- Valores decididos por defecto ante falta de información y dependencias externas o de otros specs.
Ej.: "- Una única moneda (EUR) por grupo. - Requiere un sistema de cuentas de usuario." -->

## 13. Decisiones tomadas y descartadas

<!-- La sección de más valor a largo plazo: lo que se consideró, no solo lo elegido. Decisiones de producto y alcance, no de implementación, cada una con motivo.
Si el usuario se saltó las preguntas: "**Sí:** definición rápida sin aclaración detallada. Decisión del usuario."
Ej.: "- **Sí:** reparto igualitario en la v1. Cubre la mayoría de casos. - **No:** reparto por porcentajes. Complica la interfaz; otro spec si hace falta." -->

## 14. Riesgos (opcional)

<!-- Solo riesgos no evidentes; omitir en specs pequeños.
Ej.:
| Riesgo | Mitigación |
|--------|------------|
| Gastos registrados tarde y saldos irreales | Recordatorio diario de gastos pendientes |
-->

## 15. Partes / incrementos

<!-- Partes pequeñas, numeradas y entregables por separado (una sola PARTE-01 si la idea es atómica; más de 5 indica spec demasiado grande).
- Campos de cada parte: Objetivo, Alcance, Cubre (HU · RF · RNF), Depende de, Hecho cuando (verificable).
- Cada parte se desarrolla, prueba y entrega sola, y no incluye nada fuera de "Entra".
- Cada HU y RF en exactamente una parte; cada RNF en una o más partes o en "Transversales".
- Orden: respetando dependencias y priorizando P1.
Ej.:
### PARTE-01: Grupos y miembros
- **Objetivo**: crear grupos y gestionar miembros.
- **Alcance**: crear grupo, invitar y listar miembros.
- **Cubre**: HU-03 · RF-001, RF-005
- **Depende de**: —
- **Hecho cuando**: un grupo con 3 miembros aparece en el listado de todos.

### PARTE-02: Gastos y saldos
- **Objetivo**: registrar gastos y ver saldos.
- **Alcance**: alta de gasto con reparto igualitario y vista de saldos.
- **Cubre**: HU-01, HU-02 · RF-002, RF-003, RF-004 · RNF-001
- **Depende de**: PARTE-01
- **Hecho cuando**: se cumplen los criterios de HU-01 y HU-02.

**Transversales**: RNF-002
**Orden sugerido**: PARTE-01 → PARTE-02
-->

## 16. Preguntas abiertas

<!-- Dudas que no bloquean empezar, cada una con motivo. Si no hay: "Ninguna".
Ej.: "- ¿Gastos en otra moneda durante un viaje? Afecta al cálculo de saldos." -->

## 17. Qué NO entra en este spec

<!-- Repetición deliberada de "Fuera de alcance" para quien solo lee el final; debe coincidir con la sección 3. Cerrar con: "Cada uno de estos, si llega, irá en su propio spec." -->
