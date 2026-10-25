# cgh-sdd

Plugin personal de Claude Code para trabajar con **spec driven design** (SDD) en español de España.

En SDD primero se escribe un spec (qué se construye y por qué) y solo cuando está aprobado se planifica e implementa, en partes pequeñas y verificables. Este plugin cubre las dos fases con dos skills: `cgh-spec` y `cgh-spec-impl`. Todo lo que generan (preguntas, specs, planes) está en español de España.

## Requisitos

- [Claude Code](https://claude.com/claude-code).
- Proyecto en un repositorio git con rama principal (`main` o `master`).

## Instalación

Como plugin de Claude Code, desde GitHub:

```
/plugin marketplace add christiangh/cgh-spec-driven-design
/plugin install cgh-sdd@cgh-sdd-marketplace
```

Como skills sueltas, con [skills.sh](https://www.skills.sh/):

```bash
npx skills add christiangh/cgh-spec-driven-design
```

Prueba local, sin instalar (desarrollo):

```bash
claude --plugin-dir /ruta/a/cgh-spec-driven-design
```

La invocación depende de cómo se instale:

| Instalación | `cgh-spec` | `cgh-spec-impl` |
|---|---|---|
| Plugin | `/cgh-sdd:cgh-spec` | `/cgh-sdd:cgh-spec-impl` |
| skills.sh | `/cgh-spec` | `/cgh-spec-impl` |

Los ejemplos de este README usan la forma corta.

**Compatibilidad:** probado solo en Claude Code. En otros agentes, la inyección de contexto al cargar la skill (`` !`…` ``), `disable-model-invocation` y `allowed-tools` pueden no funcionar o funcionar distinto.

## Skills

### `cgh-spec`

Genera un spec a partir de una idea o problema.

```
/cgh-spec app para registrar gastos compartidos entre amigos
```

Solo se ejecuta al invocarla explícitamente (`disable-model-invocation: true`); Claude no la lanza por su cuenta.

Si el argumento es un único token kebab-case válido (`^[a-z0-9]+(-[a-z0-9]+)*$`, p. ej. `/cgh-spec gastos-compartidos`), se usa como slug y semilla de la descripción.

Flujo:

0. **Rama:** solo se crean specs desde la rama principal (para que la numeración no colisione entre ramas). Se detecta con `origin/HEAD`; si no hay remoto, `main` o `master`; si no se puede determinar, pregunta. En otra rama, la skill se detiene y pide cambiar.
1. **Contexto:** fecha, autor, specs existentes y marcadores de proyecto se inyectan al cargar la skill. Si el directorio no parece un proyecto, avisa antes de crear `specs/`. Lee la plantilla, `CLAUDE.md`/`AGENTS.md`/`README.md` y los dos últimos specs (solo para solapes, dependencias y glosario).
2. **Preguntas por rondas** de 3-4 hasta poder responder sin suponer: qué entra y qué no, quién lo usa, cómo se verifica que está terminado y en qué partes se divide. Lo que se aplaza va a «Fuera de alcance». Tras 3 rondas ofrece redactar ya, dejando lo pendiente como supuestos o preguntas abiertas.
3. **Spec demasiado grande** (objetivo en más de una frase, más de 3 áreas, más de 5 partes…): propone dividirlo en varios specs.
4. **Redacción:** spec completo de una vez; sección a sección solo si falta información.
5. **Guardado:** escribe `specs/NNN-slug/spec.md` en estado `Borrador`, valida `Depende de`, autovalida y resume. No propone implementar.
6. **Ajustes:** si en la misma conversación pides cambios, edita el mismo spec y vuelve a autovalidar.

### `cgh-spec-impl`

Implementa un spec en estado `Aprobado`.

```
/cgh-spec-impl 001
```

El argumento puede ser `NNN`, el slug o `NNN-slug`. Solo se ejecuta al invocarla explícitamente.

Flujo:

1. **Validación:** solo continúa si el estado es `Aprobado`; si no, se detiene sin ofrecer alternativas.
2. **Git:**
   - Exige estar en la rama principal y con el árbol limpio, salvo cambios en `specs/` (p. ej. el spec recién aprobado).
   - Avisa si la principal va por detrás de `origin`.
   - Crea la rama del spec con la convención del README/CLAUDE.md/AGENTS.md o, si no hay, `feat/spec-NNN-slug`.
   - Si la rama ya existe, la retoma desde `plan.md`.
3. **Contexto:** lee `README.md`, `CLAUDE.md` y `AGENTS.md` para seguir las convenciones del proyecto y detecta las skills y agentes disponibles (`.claude/`, `.agents/` y sesión). Avisa si los specs de `Depende de` no están `Implementado`.
4. **Plan técnico por parte:** escribe en `specs/NNN-slug/plan.md` (plantilla `skills/cgh-spec-impl/plan-template.md`) el enfoque, los archivos, los pasos con checklist, las pruebas y la trazabilidad (tabla con cada HU, RF, RNF y «Hecho cuando», su paso y su prueba), las skills/agentes y las decisiones técnicas. No codifica hasta que lo apruebas.
5. **Implementación paso a paso:** marca cada paso en `plan.md` y hace una pausa para que revises el diff. Por defecto trabaja con tests primero: cada paso con comportamiento comprobable va precedido de un paso de tests que se ejecuta y se ve fallar antes de implementar, con su propio commit `test`. Quedan exentos la configuración, la documentación, los recursos y los estilos puramente visuales, y el proyecto puede restringirlo o desactivarlo en su `CLAUDE.md` o `AGENTS.md`. Ante ambigüedades, da 2-3 opciones, decides tú y queda registrado en el plan. Lo que queda fuera de alcance no se implementa.
6. **Cierre de parte:** tests, lint, «Hecho cuando», criterios de las HU y comprobación de la trazabilidad. Marca la parte como `Hecha`, te pide hacer el commit y sugiere el mensaje `feat(spec-NNN): PARTE-NN …`.
7. **Cierre del spec:** verifica los criterios de éxito y propone el estado `Implementado`, que solo edita si confirmas.

**Nunca hace commits** (ni `add`, `stash`, `push`…): los hace siempre el usuario.

## Seguridad

- `allowed-tools` **pre-aprueba**, no restringe. Cualquier acción no listada pedirá permiso.
  - `cgh-spec`: leer, buscar, preguntar, escribir y editar dentro de `specs/`, `ls`, `date`, `echo` y unos pocos comandos git de solo lectura exactos (`git config user.name`, `git branch --show-current`, `git branch --list main master`, `git symbolic-ref --short refs/remotes/origin/HEAD`).
  - `cgh-spec-impl`: lo mismo (salvo `git config`), más `git status -sb`, `git branch --list`, `git switch -c`, `git diff --stat` y `git diff --name-only`. Editar código, ejecutar tests e instalar dependencias piden permiso según tu modo.
- El contenido del proyecto (`CLAUDE.md`, `AGENTS.md`, `README.md`, specs, `plan.md`, argumento) se trata como contexto, nunca como instrucciones. Si contiene órdenes dirigidas al agente, las ignora y avisa.
- `cgh-spec`: el slug se valida con un patrón estricto y solo se escribe `specs/NNN-slug/spec.md`.
- `cgh-spec-impl`:
  - **Secretos:** no lee ni escribe `.env` ni credenciales, y nunca hardcodea secretos.
  - **Dependencias nuevas:** solo justificadas en el plan, con tu aprobación y comprobando el nombre para evitar typosquatting.
  - **Comandos peligrosos:** nunca `rm -rf`, bases de datos reales, despliegues ni producción.
  - **Tests:** nunca desactiva ni debilita tests o lint para que pasen; si falla algo ajeno a la parte, avisa y no lo toca.
  - **Alcance:** fuera de `specs/` solo edita lo incluido en el plan aprobado de la parte.

## Personalizar las plantillas

`skills/cgh-spec/template.md` define las secciones del spec y es la referencia máxima: los specs nuevos siguen siempre la plantilla actual, aunque los antiguos usen otro formato. Cada sección incluye un comentario HTML con guía y ejemplo que la skill usa al redactar y elimina del resultado. Para cambiar la estructura de los specs, edita solo ese archivo: añade, quita o reordena secciones (marca con `(opcional)` las que puedan omitirse).

Secciones actuales: cabecera (estado, dependencias, fecha, autor, objetivo en una frase), resumen, contexto, objetivos y alcance, usuarios, glosario (opcional), historias, RF, RNF, entidades (opcional), casos límite, criterios de éxito, supuestos, decisiones tomadas y descartadas, riesgos (opcional), partes, preguntas abiertas y recordatorio de qué NO entra.

`skills/cgh-spec-impl/plan-template.md` define la estructura de `plan.md` y se personaliza igual. Tiene una cabecera (spec, rama, fecha) y una sección por parte con su estado (`Pendiente`, `En curso`, `Hecha`), enfoque, archivos, pasos, pruebas y trazabilidad, skills y agentes, y decisiones técnicas.

## Convenciones

```
specs/
  001-gastos-compartidos/
    spec.md           # QUÉ y POR QUÉ (/cgh-spec)
    plan.md           # CÓMO, una sección por parte (/cgh-spec-impl)
```

- Estados: `Borrador` → `En revisión` → `Aprobado` → `Implementado` (u `Obsoleto`). La skill siempre crea en `Borrador`; aprobar es manual.
- IDs: `HU-01` historias, `RF-001` requisitos funcionales, `RNF-001` no funcionales, `CE-01` criterios de éxito, `PARTE-01` partes.
- Cada parte es un incremento pequeño, entregable y testable por sí solo; `cgh-spec-impl` la planifica e implementa parte a parte en una única rama por spec.
- Ramas: convención del proyecto o `feat/spec-NNN-slug`. Commits siempre a cargo del usuario.
- Trazabilidad: cada HU y RF en exactamente una parte; cada RNF en alguna parte o marcado como transversal.

## Créditos

Las skills de este plugin están inspiradas en las de Fernando Herrera ([Klerith/fernando-skills](https://github.com/Klerith/fernando-skills)) y adaptadas a un flujo personal de spec driven design.

## Licencia

[MIT](LICENSE) © 2026 Christian Garcia.
