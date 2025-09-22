# Governance Flow (Governor + Timelock + Token + Box)

This project demonstrates a simple DAO governance system using OpenZeppelin contracts.  
The flow is:

1. **Deploy Governance Token (`GovToken`)**
   - Mint governance tokens to voters.
   - Delegate voting power to addresses.

2. **Deploy Timelock (`Timelock`)**
   - Acts as the executor for all governance proposals.
   - Has roles:
     - `PROPOSER_ROLE` → Assigned to Governor contract.
     - `EXECUTOR_ROLE` → Can be open (`address(0)`).
     - `DEFAULT_ADMIN_ROLE` → Revoked for security.

3. **Deploy Governor (`MyGovernor`)**
   - Connects governance token and timelock.
   - Handles:
     - Proposal creation
     - Voting
     - Queuing (via Timelock)
     - Execution (via Timelock)

4. **Deploy Box (the governed contract)**
   - Ownable contract where only the Timelock can call `store()`.

---

### Governance Lifecycle

1. **Propose**  
   - A voter proposes an action (e.g., store value in Box).
   - Governor creates a `proposalId`.

2. **Voting**  
   - Token holders vote (`castVoteWithReason`).
   - After `votingPeriod`, proposal succeeds or fails.

3. **Queue (Timelock)**  
   - Successful proposals are scheduled with a delay (`minDelay`).

4. **Execute**  
   - After delay, the proposal can be executed.
   - Timelock calls the Box contract and updates state.

---

### Example
- Propose: `Box.store(777)`
- Vote: voters approve
- Queue: scheduled after voting
- Execute: Box value becomes `777`

