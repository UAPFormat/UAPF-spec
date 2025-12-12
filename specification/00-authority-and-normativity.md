# Authority and Normativity

## Normative SSOT

This repository (**UAPFormat/UAPF-spec**) is the **Single Source of Truth (SSOT)** for the UAPF specification.

The following content is **normative**:
- `/specification/**` (specification text)
- `/schemas/**` (JSON Schemas and validation constraints)

All other materials are **informative** (non-normative), including but not limited to:
- Examples, sample packages, tutorials, blog posts, READMEs outside this repository, and any website content.

## Conflict Resolution and Precedence

When contradictions exist, the SSOT prevails in this order:

1. **JSON Schemas** in `/schemas/**` (highest authority)
2. **Specification text** in `/specification/**`
3. **Appendices and examples** (informative only)
4. **Websites and other repositories** (informative only)

## Requirements Keywords

The keywords **MUST**, **MUST NOT**, **SHOULD**, **SHOULD NOT**, and **MAY** are to be interpreted as requirement levels.
