# Swift Platform Skills Brief Template

Use this template when producing an Apple platform domain knowledge brief from the Swift Agent Skills compilation.

## Metadata

- Fetch date: <YYYY-MM-DD>
- Compilation page: <URL>
- Repos fetched: <N of N total>
- Skills counted: <N> (note repos containing multiple sub-skills)
- Warnings: <N> — list any license issues, unavailable repos, or URL changes
- Note: This brief is an in-session working document. Do not persist it as a file in the target project.

## Domain Knowledge

For each domain present in the compilation, add a section:

### <Domain Name>

**Signal strength:** <VERY HIGH | HIGH | MEDIUM | LOW-MEDIUM | LOW> — <N skills from N authors; stars summary>

**Primary sources:**
- `<repo/subdirectory>` (<stars>, <version if versioned>) — <what this skill uniquely contributes>

**Reference files:** <list of references/*.md files if known — helps generator know what depth is available>

**Key topics:**
- <concrete best practice or convention>
- <API or pattern recommendation with specifics>
- <version caveat if applicable (e.g., iOS 17+, Swift 6.2+)>

**Common LLM mistakes flagged:**
- <specific mistake this domain's community skills call out as common LLM errors>

---

## Disagreements

List patterns where independent skills give conflicting guidance. Do not average them — surface the conflict explicitly so the generator can treat it as a choice point rather than a fixed rule.

- **<Topic>**: `<SkillA>` recommends X; `<SkillB>` recommends Y. Reason for disagreement if known.

---

## Unavailable Skills

| Skill | Repo | Reason | Risk |
|-------|------|--------|------|
| <skill name> | <repo URL> | <404 / license incompatible / timeout> | <what domain knowledge is missing as a result> |

---

## Generation Implications

| Domain | Signal | Use in generation |
|--------|--------|-------------------|
| <domain> | <strength> | <when to include; any dependency or precondition for use> |

Mark domains with thin or no community signal explicitly so the generator knows to rely on codebase analysis rather than community conventions for those areas.
