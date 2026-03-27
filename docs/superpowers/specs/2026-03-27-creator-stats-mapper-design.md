# Creator Public Stats Mapper

**Issue:** #41 — Add creator public stats mapper
**Date:** 2026-03-27
**Approach:** Field-map pattern (full alignment with sort mapper)

## Goal

Refactor `creators.stats.ts` so the public stats mapper follows the same const-map + mapper-function pattern established by `creators.sort.ts`. The output shape (`PublicCreatorStats`) does not change — this is a pattern-alignment task, not a new-data-shape task.

## Acceptance Criteria (from issue)

- Creator stats output uses one shared mapping pattern
- Mapper is easy to reuse across endpoints
- Output stays stable for client consumption

## Design

### 1. Mapper module — `src/modules/creators/creators.stats.ts`

Refactor the existing file to introduce:

**`CREATOR_STATS_FIELDS`** — const tuple of public field names. Single source of truth for what the mapper exposes.

```ts
export const CREATOR_STATS_FIELDS = [
  'holderCount',
  'totalSupply',
  'totalVolume',
  'lastActivityAt',
] as const;
```

**`CREATOR_STATS_FIELD_MAP`** — maps each public key to its corresponding `CreatorMetrics` key. Currently 1:1, but the indirection lets internal names change without breaking the API contract.

```ts
const CREATOR_STATS_FIELD_MAP: Record<
  CreatorStatsField,
  keyof CreatorMetrics
> = {
  holderCount: 'holderCount',
  totalSupply: 'totalSupply',
  totalVolume: 'totalVolume',
  lastActivityAt: 'lastActivityAt',
};
```

**`mapPublicCreatorStats(metrics: CreatorMetrics): PublicCreatorStats`** — replaces `serializePublicCreatorStats`. Iterates the field map to build the output. Handles the optional `lastActivityAt` by only including it when defined.

**`PublicCreatorStats`** — interface stays unchanged. Already correct.

### 2. Consumer update — `src/modules/creators/creators.controllers.ts`

- Update import: `serializePublicCreatorStats` -> `mapPublicCreatorStats`
- Update call site in `httpGetCreatorStats` (line 80)

No other consumers exist.

### 3. Out of scope

- No new fields added to `PublicCreatorStats`
- No changes to `CreatorMetrics` type
- No changes to routes, schemas, or caching
- No changes to the sort mapper or other mappers

## Files Changed

| File | Change |
|------|--------|
| `src/modules/creators/creators.stats.ts` | Refactor: add field map, rename function |
| `src/modules/creators/creators.controllers.ts` | Update import and call site |
