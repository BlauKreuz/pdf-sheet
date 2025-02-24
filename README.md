# PDF Sheet

![Version](https://img.shields.io/github/v/tag/arcanistzed/pdf-sheet?label=Version&style=flat-square&color=2577a1) ![Latest Release Download Count](https://img.shields.io/github/downloads/arcanistzed/pdf-sheet/latest/module.zip?label=Downloads&style=flat-square&color=9b43a8) ![Supported Foundry Versions](https://img.shields.io/endpoint?url=https://foundryshields.com/version?url=https://raw.githubusercontent.com/arcanistzed/pdf-sheet/main/module.json&style=flat-square&color=ff6400) [![Discord Server](https://img.shields.io/badge/-Discord-%232c2f33?style=flat-square&logo=discord)](https://discord.gg/AAkZWWqVav) [![Patreon](https://img.shields.io/badge/-Patreon-%23141518?style=flat-square&logo=patreon)](https://www.patreon.com/bePatron?u=15896855)

A system agnostic tool to export your Foundry character sheet to a PDF!

## Installation

In the setup screen, use the URL `https://github.com/arcanistzed/pdf-sheet/releases/latest/download/module.json` to install the module.

## Usage

Just click on the button in the header of a character sheet and then it will open the configuration window.
Once you upload a PDF, you'll see a list of each of the PDF's fields alongside an input.
You can either fill in the fields manually with what is on your character sheet or you can use a "mapping" (described later) to fill them in automatically with the values from your character sheet.

### Get a PDF

If you need a copy of the PDF for D&D 5th edition, there is a button to take you to the official one.

You can theoretically use this module with the PDF for any system, but some fields used on other PDFs may not be supported. File a bug if you encounter a PDF that doesn't work.

### Mapping

In order to automatically fill out the sheet with values, you will need a JS Object mapping of the PDF fields to the values on your character sheet. This is known as a "mapping" and can be edited in the module settings.

If a mapping has been created for your system, you can select it with the dropdown to load it in.

**TIP:** Install [Ace Library](https://foundryvtt.com/packages/acelib) if you want to use a proper editor to work on creating your mapping.

The mapping is formatted like this:

```js
[
    {
        "pdf": "CharacterName",
        "foundry": @name
    },
    {
        "pdf": "Check Box 11",
        "foundry": @data.abilities.str.proficient
    },
    {
        "pdf": "Race",
        "foundry": @data.details.race
    },
    {
        pdf: "Speed",
        foundry: Object.entries(actor.data.data.attributes.movement).filter(val => val[1]).map(val => val[0] === "hover" ? Object.entries(actor.data.data.attributes.movement)[6][0] : "" + val[0] !== "units" && val[0] !== "hover" ? val.join(": ") + Object.entries(actor.data.data.attributes.movement)[5][1] : "").filter(String).join(", ")
    },
    {
        pdf: "Backstory",
        foundry: @data.details.biography.value?.replaceAll(/<[^>]*>/g, "").trim()
    },
    { "pdf": "PlayerName", foundry: Object.entries(@permission).filter(entry => entry[1] === 3).map(entry => entry[0]).map(id => !game.users.get(id)?.isGM ? game.users.get(id)?.name : null).filter(x => x).join(", ") }
]
```

This will take care of filling out the character name, strength save proficiency, race, speed, and backstory on a D&D 5e character sheet.

As you can see, the `@` is used to access properties of the Actor data, rather than a fixed value.
You may use any valid JavaScript functions or formulas in the mapping, but it should return a String or [coerce](https://developer.mozilla.org/en-US/docs/Glossary/Type_coercion) into one.

While creating a mapping, it's very helpful to work in the browser console (F12):

https://user-images.githubusercontent.com/82790112/131598024-cfb300d9-57d3-4768-bd01-28b8b93c89be.mp4

Please share any mappings you create with me and I will include them in the module for the benefit of the community. [See the mappings here](https://github.com/arcanistzed/pdf-sheet/blob/main/mappings/README.md).

## License

Copyright © 2021 arcanist

This package is under an [MIT license](LICENSE) and the [Foundry Virtual Tabletop Limited License Agreement for module development](https://foundryvtt.com/article/license/).

This uses some code from [pdfform.js](https://github.com/phihag/pdfform.js) which is under the [Apache 2.0 License](https://github.com/phihag/pdfform.js/blob/master/LICENSE) and heavily modified by me. You may obtain a copy of the license on the [Apache website](http://www.apache.org/licenses/LICENSE-2.0).

## Bugs

You can submit bugs via [Github Issues](https://github.com/arcanistzed/pdf-sheet/issues/new/choose) or on [my Discord server](https://discord.gg/AAkZWWqVav).

## Contact me

Come hang out on my [my Discord server](https://discord.gg/AAkZWWqVav) or [click here to send me an email](mailto:arcanistzed@gmail.com?subject=Export%20Sheet%20to%20PDF%20module).

## TODO

- Use PDFlib library instead of pdfform.js (since it's not maintained and can't support images)
- Change the mapping to JSON, evaluating each value instead of the whole file
- Store the JSON as an Object rather than serialized text
- Add a field to the JSON mapping for a link to the PDF which can be put in the App instead of only the D&D link
