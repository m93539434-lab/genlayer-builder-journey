# Project 02 — AI Intelligent Contract Specification

**Version:** 1.0 Draft  
**Project:** AI-Powered Product Review & Verification DApp  
**Contract:** ProductReviewVerifier

## 1. Purpose

The contract evaluates user-submitted product claims against user-provided textual evidence using an AI model and GenLayer's Equivalence Principle.

This is an educational evidence-based claim evaluation prototype. It does not certify product quality, authenticity, safety, or regulatory compliance.

## 2. Contract Inputs

The public submission function will accept:

- `product_name`: Product name.
- `claim`: A specific statement about the product.
- `evidence`: Textual evidence supplied by the user.

Initial version scope:
- User-submitted text only.
- No automatic external website research.
- No QR-code verification.
- No image verification.
- No payment or reward functionality.

## 3. Input Validation

Deterministic validation must run before AI evaluation.

- Product name must not be empty.
- Claim must not be empty.
- Inputs must be strings and must respect defined maximum lengths.
- Empty evidence is allowed and must result in `INSUFFICIENT EVIDENCE`.
- Invalid product name or claim must raise a clear input-validation error.
- Input length limits must be defined as contract constants and tested.

## 4. AI Decision Outcomes

The AI must return exactly one of these decisions:

### SUPPORTED
The submitted evidence is relevant and provides sufficient support for the claim.

### UNSUPPORTED
The submitted evidence directly contradicts the claim.

### INSUFFICIENT EVIDENCE
The evidence is empty, irrelevant, unclear, incomplete, or insufficient to support or contradict the claim.

The AI must not invent facts, sources, product specifications, or evidence.

## 5. Structured AI Output

The AI evaluation must request JSON using `response_format="json"`.

Required fields:

- `decision`: One of the three permitted outcomes.
- `explanation`: A concise explanation grounded in the submitted evidence.
- `evidence_summary`: A short summary of the evidence actually supplied.

Malformed output, missing fields, invalid decision values, or empty explanations must be rejected.

## 6. AI Prompt Security

The contract must:

- Treat product name, claim, and evidence as untrusted user input.
- Explicitly instruct the AI to treat submitted evidence as data, not instructions.
- Ignore instructions embedded inside evidence that attempt to override the evaluation rules.
- Never allow user input to change the permitted decision labels or contract rules.
- Never invent missing facts.
- Avoid claims of authenticity, safety, or guaranteed product quality.

## 7. Equivalence Principle

Use GenLayer's custom leader/validator pattern through `gl.vm.run_nondet_unsafe()`.

Leader:
- Executes the AI evaluation.
- Returns the structured decision and explanation.

Validator:
- Independently repeats the evaluation using the same submitted inputs and rules.
- Verifies the leader result against its own independently generated result.
- Requires exact agreement on the `decision` field.
- Does not require identical wording in `explanation` or `evidence_summary`.
- Rejects malformed or invalid results.
- Rejects when the decision cannot be independently verified.

The validator must not merely check the leader's JSON format or permitted decision label.

## 8. Persistent Storage

Only the accepted leader result may be stored after successful consensus.

Each accepted record should contain:

- Product name
- Claim
- Decision
- Explanation
- Evidence summary

Use GenLayer-supported persistent storage types. A typed `DynArray` of an appropriately declared storage dataclass is the proposed record structure, subject to compilation and serialization tests.

Do not write persistent state inside a non-deterministic block.

## 9. Public Contract Methods

Proposed methods:

- `submit_review(product_name, claim, evidence)`: Validate, evaluate, verify consensus, and store the accepted result.
- `get_review(index)`: Return one stored review.
- `get_review_count()`: Return the number of accepted reviews.

Exact method signatures will be finalized during implementation.

## 10. Error Handling

The contract must reject:

- Empty product name.
- Empty claim.
- Inputs exceeding defined limits.
- Invalid AI JSON.
- Missing required output fields.
- Invalid decision values.
- Empty explanations.
- Validator disagreement.
- AI or execution failures that prevent reliable verification.

No review record may be stored when consensus fails.

## 11. Testing Requirements

Tests must cover:

- Valid SUPPORTED result.
- Valid UNSUPPORTED result.
- Valid INSUFFICIENT EVIDENCE result.
- Empty evidence.
- Empty product name.
- Empty claim.
- Maximum input lengths and oversized inputs.
- Prompt-injection attempts in evidence.
- Malformed AI output.
- Missing JSON fields.
- Invalid decision labels.
- Empty explanation.
- Validator agreement.
- Validator disagreement.
- Failed evaluation must not modify storage.
- Successful evaluation must store exactly one accepted record.
- Read methods must return the correct stored data.

## 12. Implementation and Release Gates

1. Approve this specification.
2. Create the Python Intelligent Contract.
3. Run syntax and GenLayer compatibility checks.
4. Create and run Direct Mode tests.
5. Review all failures and fix them.
6. Run Studio integration and consensus tests when appropriate.
7. Lock the contract and test files only after successful verification.

**Deployment is excluded from this stage until implementation and testing are approved.**