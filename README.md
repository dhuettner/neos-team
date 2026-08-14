# Waterproof.Team

Team and people management for **Neos CMS 9**. People are document nodes and are
maintained in one place. Overviews and contact blocks read from there, so no
data is entered twice. Departments run on
[Sitegeist.Taxonomy](https://github.com/sitegeist/Sitegeist.Taxonomy), lists on
[Flowpack.Listable](https://github.com/Flowpack/Flowpack.Listable).

The package ships **structure, not design**. Markup carries Tailwind class names,
there is no CSS file and no build step of its own.

## Installation

```bash
composer require waterproof/neos-team
./flow neos.flow:package:rescan
./flow flow:cache:flush --force
./flow resource:publish
```

The three commands after the require are mandatory under DDEV. The Composer
plugin writes the package cache to `Data/Temporary/`, while Flow reads it from
`/tmp/Flow/`. Without `package:rescan` the package stays invisible.

### Set up Tailwind, or everything stays unstyled

The Fusion files live outside your site package, and Tailwind only finds class
names in files from its `content` glob. Add this to your `tailwind.config.js`:

```js
content: [
    './Resources/Private/**/*.{fusion,html,js}',
    './NodeTypes/**/*.{yaml,fusion}',
    '../../Packages/Application/Waterproof.Team/Resources/Private/**/*.fusion',
],
```

Without that entry the package works, but arrives without any layout.

## Node types

| Node type | Backend label | Purpose |
|---|---|---|
| `Waterproof.Team:Document.Person` | Person | One person with role, portrait, contact details, optional detail page |
| `Waterproof.Team:Document.TeamIndex` | Teamübersicht | Parent page, lists the people below it |
| `Waterproof.Team:Content.TeamGrid` | Team-Übersicht | Grid, optionally limited to departments |
| `Waterproof.Team:Content.TeamHighlight` | Ansprechpartner | A single person on any page |

## Detail pages

Every person carries the `hasDetailPage` property, switched off by default:

- **off:** cards do not link, a direct call answers with 404 in the frontend.
  The page stays editable in the backend so it can be prepared.
- **on:** the person gets a regular page with its own content area.

## Column choice

`Document.TeamIndex` and `Content.TeamGrid` carry a `columns` property with the
values `cols1` to `cols4`, default `cols3`. The class names are written out in
`Component/PersonCard.fusion`, because composed class names would be invisible
to the Tailwind scanner.

## Apply your own design

| Prototype | Responsible for |
|---|---|
| `Waterproof.Team:Document.Person.Short` | Card in listings |
| `Waterproof.Team:Content.PersonBody` | Detail page |
| `Waterproof.Team:Content.PersonNotFound` | Output when the detail page is switched off |
| `Waterproof.Team:Component.PersonCollection` | Grid around the cards |
| `Waterproof.Team:Component.GridClass` | Mapping of column value to classes |

## Languages

Backend labels ship as XLIFF in `Resources/Private/Translations/` for German and
English.

## License

MIT.

Built and maintained by [waterproof.agency](https://waterproof.agency/).
