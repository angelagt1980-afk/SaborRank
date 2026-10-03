# SaborRank

Ranking multicriterio de restaurantes basado en **reseñas verificadas**.
Proyecto de la materia **Ingeniería de Software para Sistemas Computacionales** (7.º cuatrimestre) — **Taller 5: Construcción de Software** (TDD, calidad e integración).

El documento del taller está en [`docs/Taller5_SaborRank.docx`](docs/Taller5_SaborRank.docx).

## Funcionalidad implementada

> **HU-03.** Como comensal, quiero consultar el ranking global actualizado de un restaurante, para decidir si vale la pena visitarlo basándome en la experiencia verificada de otros comensales.

**Regla de negocio:** el ranking global de un restaurante es el promedio de las calificaciones globales de sus reseñas verificadas y publicadas. Las reseñas en disputa, retiradas o pendientes no cuentan. Si no hay reseñas verificadas, el ranking es `0` (nunca `NaN`).

Además: ranking por criterio (Precio, Calidad, Atención, Distancia, Promoción), máquina de estados de la reseña y una aplicación de consola de demostración.

## Requisitos

- Node.js 20 o superior (recomendado 22)
- npm

## Uso

```bash
npm install        # instala dependencias
npm test           # ejecuta las pruebas (vitest)
npm run coverage   # pruebas con cobertura
npm run typecheck  # verifica tipos con TypeScript estricto
npm start          # ejecuta la aplicación de demostración
```

Salida esperada de `npm start`:

```
=== Ranking inicial ===
1. Mariscos La Playa    4.50  (2 reseñas verificadas)  ...
2. Tacos El Güero       4.00  (3 reseñas verificadas)  ...
3. Cafetería Nueva      0.00  (0 reseñas verificadas)
```

## Arquitectura

```
src/
├── domain/
│   ├── enums/            CriterioCalificacion, EstadoResena
│   ├── valueobjects/     Calificacion
│   ├── entities/         Resena (invariantes + estados), Restaurante
│   └── services/         CalculadorRanking  <- servicio de dominio puro
├── application/          ActualizarRankingRestaurante (caso de uso)
├── infrastructure/       RepositorioResenas (memoria), datosDemo
└── main.ts               CLI de demostración
test/                     Pruebas unitarias e integración (patrón AAA)
```

### Trazabilidad

| Taller | Artefacto | Código |
| --- | --- | --- |
| 1 | HU-03: consultar ranking | `Restaurante.rankingGlobal` |
| 2 | CU "Dejar reseña verificada", paso 8 | `ActualizarRankingRestaurante` |
| 3 | DCD: `CalculadorRanking.calcularGlobal()` (Information Expert) | `src/domain/services/CalculadorRanking.ts` |
| 4 | Secuencia `recalcular(restaurante, reseñas)` y máquina de estados | `Restaurante.actualizarRanking()`, `Resena` |
| 5 | TDD, refactor, commit y Pull Request | historial Git + `test/` |

## Flujo de trabajo (feature branching)

- `main`: código compartido.
- `feature/calculo-ranking-restaurante`: rama de la funcionalidad, integrada a `main` mediante Pull Request.
- Commits convencionales (`feat`, `test`, `refactor`, `docs`, `chore`).
- El historial de la rama muestra el ciclo **RED → GREEN → REFACTOR**:
  1. `test(calculador-ranking): ... (RED)`: la prueba falla porque `CalculadorRanking` no existe.
  2. `feat(calculador-ranking): ... (GREEN)`: implementación mínima.
  3. `refactor(calculador-ranking): ... (REFACTOR)`: extrae `promedio()`.
  4. `docs(calculador-ranking): ...`: cambio pedido en la revisión por pares.

## Criterios de entrega del taller

| Criterio | Evidencia |
| --- | --- |
| 1. Repositorio | Este repositorio, con `.gitignore`, README y `tsconfig.json` |
| 2. Rama de trabajo | `feature/calculo-ranking-restaurante` |
| 3. Código | `src/domain/services/CalculadorRanking.ts` |
| 4. Pruebas | `test/calculador-ranking.test.ts` y el resto de `test/` |
| 5. TDD | Commits RED, GREEN y REFACTOR en el historial |
| 6. Commit | Conventional commits |
| 7. Pull Request | PR con la plantilla de `.github/pull_request_template.md` |
