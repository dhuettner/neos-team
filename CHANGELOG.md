# Changelog

All notable changes to this package are documented here. The format follows
[Keep a Changelog](https://keepachangelog.com/en/1.1.0/), versioning follows
[Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [1.0.1] - 2026-08-17

### Fixed

- **Backend labels stayed untranslated.** The inspector showed raw translation
  identifiers such as `Waterproof.Team:NodeTypes.Document.Person:properties.jobTitle`
  instead of the field label. Two independent causes:
  - `Settings.yaml` declared only `Neos.Neos.fusion.autoInclude`. Without
    `Neos.Neos.userInterface.translation.autoInclude` the XLIFF catalogues are
    never shipped to the backend JavaScript, so no property label could resolve.
  - Inspector group labels used the identifier `ui.inspector.groups.<name>`.
    Neos generates `groups.<name>` (see `NodeTypeEnrichmentService::getInspectorElementTranslationId()`),
    so every group heading fell back to its raw key.

  Both are corrected for German and English. No configuration change is required
  in projects using the package; a cache flush after the update is enough.

## [1.0.0] - 2026-08-14

### Added

- Initial release: person, team index, team grid and contact highlight node types
  for Neos CMS 9, built on Flowpack.Listable and Sitegeist.Taxonomy.
- Central maintenance of people, with optional detail pages per person
  (`hasDetailPage`); a disabled detail page answers with 404 in the frontend
  while staying editable in the backend.
- Departments through Sitegeist.Taxonomy, with an optional filter on the grid.
- Backend labels as XLIFF for German and English.

[1.0.1]: https://github.com/dhuettner/neos-team/compare/v1.0.0...v1.0.1
[1.0.0]: https://github.com/dhuettner/neos-team/releases/tag/v1.0.0
