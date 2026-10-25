# F1 Data Contract

**Status:** DRAFT — Mission 2  
**Source:** SYKE VESLA Surface Water Quality API v2.0  
**Scope:** Päijänne, sites 61/100/112, analytes PH/PTOT/NTOT/O2D, 2020-01-01 inclusive to 2026-01-01 exclusive (reporting years 2020–2025)

## Sample depth — Parsing and Validation

### 1. Source evidence

The previously observed `Result_Wide` record contains:

- `SampleDepth_m`: `"1.0"`
- `SampleDepthUpper_m`: `"1.0"`
- `SampleDepthLower_m`: `null`

These are observed field names and values from one API response. Full metadata verification remains pending.

### 2. Proposed parsing rules

Raw values must be preserved without modification.

| Input condition | Classification |
|---|---|
| `None` | `MISSING` |
| Empty string | `MISSING` (proposed policy) |
| Boolean | `INVALID_TYPE` |
| Finite non-negative number | `VALID_NUMERIC` |
| Parseable finite non-negative numeric string | `VALID_NUMERIC` |
| Unparseable non-missing value | `CONVERSION_FAILED` |
| NaN or infinity | `NON_FINITE` |
| Negative finite depth | `INVALID_DEPTH` |

### 3. Scientific interpretation

A successfully parsed numeric value is not automatically a scientifically valid observation.

The project must validate depth values before using them to determine surface eligibility.

For the planned surface view, the depth threshold is 2 m. Missing or invalid depths must not automatically be classified as surface observations.

### 4. Data preservation

The pipeline must preserve the original source fields and create separate derived values.

Invalid data must not be silently replaced with zero or removed from the raw evidence.

### 5. Verification status

- Learning-labs evidence: 8/8 user-run assertions passed on 2026-10-25 for the selected conversion cases. This does not establish full depth-validation coverage or production contract-test PASS.
- Official depth field semantics: pending verification.
- Production contract tests: not yet implemented.
- Final missing-value and depth-interval policy: pending review.