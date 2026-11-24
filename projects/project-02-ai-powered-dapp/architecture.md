
# Project 02 — System Architecture

## 1. User Interface

The user submits:

- Product name
- Product claim
- Supporting evidence text

## 2. Intelligent Contract

The contract:

- Validates user input
- Processes the submitted claim
- Evaluates the evidence using an AI prompt
- Produces a structured result
- Stores the accepted result after consensus

## 3. AI Evaluation

The AI evaluates:

- Whether the evidence is relevant
- Whether the evidence supports the claim
- Whether the evidence contradicts the claim
- Whether the evidence is sufficient

## 4. Validator Consensus

The leader produces a candidate result.

Validators independently evaluate the same claim and evidence.

The contract compares the important decision fields.

A result must not be accepted merely because its JSON format is valid.

## 5. Result Storage

After successful consensus, the contract stores:

- Product name
- Claim
- Decision
- Short explanation
- Evidence summary

## 6. Application Flow

User Input
    |
    v
Input Validation
    |
    v
AI Evaluation
    |
    v
Validator Consensus
    |
    v
Accepted Result
    |
    v
Contract Storage
    |
    v
Frontend Result

## 7. Initial Version Scope

The first version will use user-submitted evidence text.

Automatic external website research and QR verification are outside the initial scope.
