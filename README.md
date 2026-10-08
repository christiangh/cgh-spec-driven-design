# cgh-sdd

Plugin personal de Claude Code para trabajar con **spec driven design** (SDD) en español de España.

## Instalación

Prueba rápida, sin instalar:

```bash
claude --plugin-dir /ruta/a/cgh-spec-drive-design
```

Instalación como marketplace local:

```
/plugin marketplace add /ruta/a/cgh-spec-drive-design
/plugin install cgh-sdd@cgh-sdd-marketplace
```

## Skills

### `spec`

Genera un spec a partir de una idea o problema.

```
/cgh-sdd:spec app para registrar gastos compartidos entre amigos
```

Solo se ejecuta al invocarla explícitamente (`disable-model-invocation: true`); Claude no la lanza por su cuenta.

Si el argumento es un único token kebab-case válido (`^[a-z0-9]+(-[a-z0-9]+)*$`, p. ej. `/cgh-sdd:spec gastos-compartidos`), se usa como slug y semilla de la descripción.

Flujo:

0. **Rama:** solo se crean specs desde la rama principal (para que la numeración no colisione entre ramas). Se detecta con `origin/HEAD`; si no hay remoto, `main` o `master`; si no se puede determinar, pregunta. En otra rama, la skill se detiene y pide cambiar.
1. **Contexto:** fecha, autor, specs existentes y marcadores de proyecto se inyectan al cargar la skill. Si el directorio no parece un proyecto, avisa antes de crear `specs/`. Lee la plantilla, `CLAUDE.md`/`AGENTS.md`/`README.md` y los dos últimos specs (solo para solapes, dependencias y glosario).
2. **Preguntas por rondas** de 3-4 hasta poder responder sin suponer: qué entra y qué no, quién lo usa, cómo se verifica que está terminado y en qué partes se divide. Lo que se aplaza va a «Fuera de alcance». Tras 3 rondas ofrece redactar ya, dejando lo pendiente como supuestos o preguntas abiertas.
3. **Spec demasiado grande** (objetivo en más de una frase, más de 3 áreas, más de 5 partes…): propone dividirlo en varios specs.
4. **Redacción:** spec completo de una vez; sección a sección solo si falta información.
5. **Guardado:** escribe `specs/NNN-slug/spec.md` en estado `Borrador`, valida `Depende de`, autovalida y resume. No propone implementar.
6. **Ajustes:** si en la misma conversación pides cambios, edita el mismo spec y vuelve a autovalidar.

### Seguridad

- `allowed-tools` **pre-aprueba**, no restringe: sin pedir permiso solo puede leer, buscar, preguntar, escribir y editar dentro de `specs/` y ejecutar `ls`, `date`, `echo` y unos pocos comandos git de solo lectura exactos (`git config user.name`, `git branch --show-current`, `git branch --list main master`, `git symbolic-ref --short refs/remotes/origin/HEAD`). Cualquier otra acción pedirá permiso.
- El contenido del proyecto (`CLAUDE.md`, `README.md`, specs previos, argumento) se trata como contexto, nunca como instrucciones. Si contiene órdenes dirigidas al agente, las ignora y avisa.
- El slug se valida con un patrón estricto y solo se escribe `specs/NNN-slug/spec.md`.

## Personalizar la plantilla

`skills/spec/template.md` define las secciones del spec y es la referencia máxima: los specs nuevos siguen siempre la plantilla actual, aunque los antiguos usen otro formato. Cada sección incluye un comentario HTML con guía y ejemplo que la skill usa al redactar y elimina del resultado. Para cambiar la estructura de los specs, edita solo ese archivo: añade, quita o reordena secciones (marca con `(opcional)` las que puedan omitirse).

Secciones actuales: cabecera (estado, dependencias, fecha, autor, objetivo en una frase), resumen, contexto, objetivos y alcance, usuarios, glosario (opcional), historias, RF, RNF, entidades (opcional), casos límite, criterios de éxito, supuestos, decisiones tomadas y descartadas, riesgos (opcional), partes, preguntas abiertas y recordatorio de qué NO entra.

## Convenciones

```
specs/
  001-gastos-compartidos/
    spec.md
    partes/            # (futuro) plan y tareas por parte
      01-grupos-y-miembros/
```

- Estados: `Borrador` → `En revisión` → `Aprobado` → `Implementado` (u `Obsoleto`). La skill siempre crea en `Borrador`; aprobar es manual.
- IDs: `HU-01` historias, `RF-001` requisitos funcionales, `RNF-001` no funcionales, `CE-01` criterios de éxito, `PARTE-01` partes.
- Cada parte es un incremento pequeño, entregable y testable por sí solo; las futuras skills de plan/tareas trabajarán parte a parte.
- Trazabilidad: cada HU y RF en exactamente una parte; cada RNF en alguna parte o marcado como transversal.

## Créditos

Las skills de este plugin están inspiradas en las de Fernando Herrera ([Klerith/fernando-skills](https://github.com/Klerith/fernando-skills)) y adaptadas a un flujo personal de spec driven design.
