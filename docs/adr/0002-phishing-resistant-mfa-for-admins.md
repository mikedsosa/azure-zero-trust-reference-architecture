# ADR-0002: Phishing-resistant MFA for administrators first

- **Status:** Accepted
- **Date:** 2026-10-23
- **Deciders:** CISO, identity lead

## Context
Adversary-in-the-middle phishing kits can relay push or OTP MFA and steal the
session. Admin accounts are the highest-impact target (F1).

## Options considered
1. **Number-matching push for everyone, including admins:** cheap and quick,
   but still phishable through AiTM.
2. **Phishing-resistant methods (FIDO2/passkeys, Windows Hello for Business,
   certificate-based auth) for everyone at once:** best end state, but the
   hardware, enrollment, and support load is too much for one phase.
3. **Phishing-resistant for admins first, then expand to all users.**

## Decision
Option 3. CA003 requires the built-in *Phishing-resistant MFA*
authentication strength for privileged directory roles, with a 4-hour
sign-in frequency and no persistent browser. Standard users move to passkeys
in Phase 3-4.

## Consequences
- Admins need two FIDO2 keys each (primary + backup). Budget line in Phase 1.
- Temporary Access Pass is needed for bootstrap and recovery.
- Standard users stay on phishable MFA until later phases (accepted risk,
  tracked as residual risk S2 in the lab threat model).

## Related
MCSB IM-6, PA-1 · Lab: CA003
