# Home Assistant Blueprint Repository Instructions

## Scope

These instructions apply to this repository and all subdirectories unless a more specific `AGENTS.md` overrides them.

## Project context

This repository contains Home Assistant blueprints. It may include blueprints for automations, scripts, templates, scenes, and other blueprint-supported domains. Every blueprint must be valid Home Assistant YAML and should be safe, predictable, configurable, and understandable to users installing it through the Home Assistant UI.

## Source layout

- Store each blueprint under the directory matching its Home Assistant domain, such as `blueprints/automation/`, `blueprints/script/`, or another supported blueprint directory.
- Keep each blueprint self-contained in its own YAML file unless there is a clear reason to split functionality.
- Keep user-facing documentation in `README.md` or next to the relevant blueprint.
- Use English for blueprint metadata, input labels, descriptions, logs, and documentation. Italian may be used in the conversation, but not in repository user-facing content.

## YAML and blueprint rules

- Use two-space indentation and valid YAML syntax.
- Use Home Assistant's blueprint structure and the current documented syntax for the relevant domain.
- Set the correct blueprint `domain` and use the corresponding top-level structure for automations, scripts, templates, scenes, or other supported blueprint types.
- Include an accurate `author` when publishing a blueprint.
- Set `homeassistant.min_version` when the blueprint relies on version-specific features; quote semantic versions as strings, for example `"2024.6.0"`.
- Do not add `source_url` to the source YAML. Home Assistant records the import URL automatically and uses it for re-importing updates.
- Keep domain-specific sections and actions valid for the selected domain; do not assume automation-only features apply to scripts, templates, or scenes.
- Declare every blueprint input explicitly and reference inputs with `!input` or clearly defined variables.
- For optional inputs, define safe defaults and handle empty or unavailable values explicitly.
- Use selectors that restrict users to compatible domains, devices, entities, or services whenever possible.
- Keep input names stable after release; renaming or removing one can break existing automation configurations.
- Treat entity state, attributes, unavailable entities, and empty selections as untrusted runtime values.
- Avoid unnecessary polling. Prefer event/state triggers, waits, and bounded timeouts.
- Make concurrency behavior explicit with `mode`, `max`, and `max_exceeded` when relevant.

### Domain-specific considerations

- **Automations:** verify triggers, conditions, actions, execution mode, retriggering, waits, and timeout behavior.
- **Scripts:** verify fields, sequence behavior, variables, response handling, and script execution mode.
- **Templates:** verify rendering, entity dependencies, availability behavior, type conversions, and update triggers where applicable.
- **Scenes:** verify supported entity attributes, restore behavior, and the effect of applying the scene to multiple entities.
- **Other domains:** follow the official Home Assistant blueprint schema and the domain's supported configuration keys.

## Jinja and runtime behavior

- Keep templates short and readable; use variables for repeated expressions.
- Guard conversions with checks such as `is_number` before numeric filters.
- Do not assume that an entity exists, is available, or has a particular attribute.
- Preserve user intent: do not overwrite manually changed device state unless documented.
- Consider restarts, reloads, retriggers, simultaneous triggers, and multiple configured entities.
- Ensure waits and loops have sensible exit conditions and cannot run indefinitely.
- Make debug logging optional and never log secrets or unnecessary entity data.

## Changes and compatibility

- Make the smallest focused change that solves the problem.
- Preserve existing behavior unless a bug fix or intentional contract change is documented.
- Update descriptions, defaults, selectors, and documentation whenever behavior or configuration changes.
- Before the first release, input names may be refined for clarity; after release, treat input names as a compatibility contract.
- For pre-release preparation, separate formatting, metadata, documentation, and behavior changes; do not change runtime behavior unless explicitly requested.
- Do not add credentials, tokens, personal entity IDs, private URLs, or installation-specific configuration.

## Documentation and releases

- Keep the README in English and document installation, requirements, inputs, runtime behavior, limitations, troubleshooting, version, and license.
- Use Semantic Versioning for releases. The initial public release should use the `0.1.0` form when appropriate, with a matching Git tag such as `v0.1.0`.
- Ensure the README's documented behavior matches the actual blueprint before tagging a release.

## Review checklist

Before considering a change complete, verify:

1. The YAML parses successfully.
2. Every referenced input is declared and every declared input is used intentionally.
3. Defaults, selectors, and units match the expected Home Assistant types.
4. Empty, unavailable, and simultaneous-trigger cases behave safely.
5. Manual device changes are handled as documented.
6. Delays, waits, loops, and automation modes terminate as intended.
7. User-facing descriptions match the actual behavior.
8. The diff contains no unrelated changes or secrets.

## Git conventions

- Keep commits focused and use clear imperative messages.
- Do not commit generated files, local secrets, editor settings, or machine-specific state.
- Commit this `AGENTS.md`: it is shared repository guidance, not local user state.
