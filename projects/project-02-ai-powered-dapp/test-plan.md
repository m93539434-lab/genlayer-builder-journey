
# Project 02 — Initial Test Plan

## Input Validation

- Empty product name
- Empty claim
- Empty evidence
- Excessively long input

## Decision Tests

### Test 1 — Supported

Evidence clearly supports the claim.

Expected: SUPPORTED

### Test 2 — Unsupported

Evidence directly contradicts the claim.

Expected: UNSUPPORTED

### Test 3 — Insufficient Evidence

Evidence is missing or inconclusive.

Expected: INSUFFICIENT EVIDENCE

## Security Tests

- Prompt injection inside evidence
- Malformed AI output
- Invalid decision value
- Empty explanation
- Validator disagreement

## Storage Tests

- Successful consensus stores the result.
- Failed consensus does not update verification state.

## Current Status

NOT STARTED — Architecture phase.
