# Unit converter

Single-file web application that converts values between SI units, sub-units, customary units, and US/imperial or regional units. It runs entirely in the browser, with no server, no connection and no dependency.

- **15 quantities**, **249 units**
- **6 languages**: French (default), English, German, Italian, Spanish, Swedish
- **3 values** converted in parallel
- Designed phone-first, also usable on a computer
- Results limited to 3 decimal places
- Single-file application: `convertisseur-unites.html` (this `README.en.md` and the French `README.md` accompany it; a link in the top banner points to them)

## Getting started

Open `convertisseur-unites.html` in a browser (double-click). No installation, no build.

To publish it, simply put the file on a static web server, an intranet or a file share.

**README link**: the “README” link in the top banner, below the language selector, opens `README.md` (French) when the interface language is French, and `README.en.md` (English) for every other language. The paths are relative to the page, so the link works if both README files are in the **same folder** as `convertisseur-unites.html`. Otherwise (page on its own, or hosted elsewhere), change the `README_URL` constant (see [Customization](#readme-link)).

## Usage

**On opening**, the page always shows the **Length** quantity, from inch (`in`) to centimetre (`cm`), with the values **1**, **2** and **3** in the three fields (results: 2.54; 5.08; 7.62). Only the selected language is kept from one visit to the next.

1. Choose a **quantity** in the top banner (length, temperature, force, etc.).
2. Choose the **input unit** and the **output unit** in the two cards. The central button swaps them.
3. Enter up to **three values**: each row shows its result live.
4. Copy a result with the row’s copy button.

Layout:

- **Phone**: each row is a card, with the input above and the result below.
- **Wide screen (700 px and up)**: the input and the result are two blocks of the same width and height, side by side. The quantities wrap onto several lines instead of scrolling.

Other features:

- **“+/−” button** to the left of each input: adds or removes the minus sign (useful on phones, whose numeric keypad does not always have this key).
- **Unit catalogue** at the bottom of the page: lists every unit of the quantity with its usage, region and equivalent. The “input” and “output” pills of each unit select it directly. The catalogue is collapsed by default on small screens.
- **Keyboard**: on the quantity tabs, left and right arrows, Home and End.
- **Dark mode**, automatic, following the system setting.
- **“README” link** in the top banner, below the language selector: opens the README in a new tab, in French if the language is French (label “README (FR)”), in English otherwise (label “README (EN)”). The tooltip and accessible name are translated. On narrow screens (under 480 px), the language selector shows its code (FR, EN, DE…) and the link shows “README” alone, so that the title keeps its room.

### Number entry

- The decimal separator can be `,` or `.` whatever the interface language.
- If both appear, the last one is the decimal separator: `1.234,5` and `1,234.5` both mean 1234.5.
- A lone separator is always read as **decimal**: `1,234` means 1.234 (not 1234).
- Spaces (including non-breaking and thin spaces) and apostrophes are ignored: `1 000` means 1000.
- Scientific notation is accepted: `1e-3`, `2.5e6`.

## Quantities and units

| Quantity | Units | Reference unit (calculations) |
|---|---:|---|
| Length | 20 | metre (m) |
| Temperature | 8 | degree Celsius (°C) |
| Force | 17 | newton (N) |
| Elongation | 16 | metre per metre (m/m) |
| Speed | 15 | metre per second (m/s) |
| Pressure | 19 | pascal (Pa) |
| Flow rate | 18 | cubic metre per second (m³/s) |
| Power | 17 | watt (W) |
| Area | 17 | square metre (m²) |
| Volume | 20 | cubic metre (m³) |
| Angle | 15 | radian (rad) |
| Energy | 18 | joule (J) |
| Mass | 20 | kilogram (kg) |
| Acceleration | 13 | metre per second squared (m/s²) |
| Torque | 16 | newton-metre (N·m) |

Units are grouped by system or region (SI and multiples, United States and United Kingdom, marine, Asia, astronomy, etc.). Each unit in the catalogue states its usage.

## How the calculations work

- **Linear units**: `result = value × input_factor / output_factor`, where the factor is the value of the unit in the reference unit of the table above.
- **Temperature**: the conversion goes through degrees Celsius (the `toC` and `fromC` functions of each scale). A value below absolute zero triggers a warning, but the calculation is still performed. Rounding residues of the order of 10⁻¹³ are brought back to 0: absolute zero entered in one scale gives `0` in another, not `5.684 × 10⁻¹⁴`.
- **Calculation precision**: double-precision floating point (IEEE 754).
- **Result display**: **at most 3 decimal places**, without useless zeros (`200`, not `200.000`). Values below 0.0005, or greater than or equal to 10¹², switch to scientific notation, with at most 3 decimals on the mantissa (for example `1 × 10⁻⁹` for 1 nm in metres), to avoid displaying a false zero.
- **Reference factors**: the equivalence line under the unit cards (“1 ft = 0.3048 m”) and the equivalents column of the catalogue keep their full precision (12 significant digits). They are not limited to 3 decimals.
- **Copy**: the copied value is identical to the displayed value (at most 3 decimals), with the decimal separator of the active language and no thousands separator.

### Definition assumptions

- **Elongation**: relative elongation ε = ΔL / L₀, a dimensionless quantity (tensile tests, strain gauges). It is not a displacement expressed in a length unit.
- **Flow rate**: volumetric flow rate (volume per unit of time). Mass flow rate is not included.
- **Calorie**: international calorie (4.1868 J). The kilocalorie is 4186.8 J.
- **Kilogram-force**: standard gravity g = 9.806 65 m/s².
- **Mach**: speed of sound in the standard atmosphere at sea level, at 15 °C (340.294 m/s). In reality it varies with temperature.
- **Imperial and US units**: exact international definitions (1 in = 25.4 mm, 1 lb = 0.453 592 37 kg, etc.).
- **Regional units** (mu, rai, feddan, tola, dunam, etc.): current legal or conventional value.
- **Ton of TNT equivalent**: thermochemical convention of 4.184 GJ.

## Limitations

- Everyday calculation aid. It is not intended for calibrated metrology, nor for regulatory calculations without independent verification.
- Results are rounded to 3 decimals for display and copy. For higher precision, convert into a smaller unit (for example millimetres rather than metres) or use the factors in the catalogue.
- The translations and the regional usage descriptions were written without review by native speakers. This English README is a translation of the French one: in case of discrepancy, the French version prevails.
- No automated tests are provided with the file.

## Languages

Menu at the top right: Français, English, Deutsch, Italiano, Español, Svenska. French is the default language, whatever the browser language.

Translated: the interface, the messages, the names of the quantities and of all units, the list groups, the usage descriptions and the footnote. The README exists in French (`README.md`) and English (`README.en.md`): the banner link opens the French one when the language is French, and the English one for all other languages. Numbers follow the conventions of the language (for example 4 050,19 in French, 4,050.19 in English, 4.050,19 in German). A few symbols change with the language (horsepower: ch, PS, CV or hk; light-year: al, ly, Lj or ljusår; “per day”: /j, /d or /g).

## Privacy and dependencies

- **No network request**: the file loads no external script, font, image or stylesheet. Fonts are the system’s.
- **No dependency** (no library, no package manager, no build).
- **No cookies, no tracking, no personal data collected.**
- **README link**: it is an ordinary link. It sends no request until it is clicked, and it opens in a new tab (`rel="noopener"`).
- **Local storage**: the browser only remembers the chosen **language**, under the `localStorage` key `convertisseur-unites-v1`. The quantity, units and entered values are not kept: the page always opens in its initial state. If storage is unavailable, the application works normally without remembering the language.
- The code is readable as it stands: it is neither minified nor obfuscated, which makes a security review easier.

## Code structure

Everything is in `convertisseur-unites.html`, in this order:

| Element | Role |
|---|---|
| `<style>` | Light and dark theme (CSS variables), mobile-first layout |
| `<body>` | Banner (title, language, README link, quantities), unit cards, conversion rows, catalogue |
| `README_URL` | Addresses of the two READMEs (French, English) targeted by the banner link |
| `QUANTITIES` | Data: quantities and units, with the conversion factors |
| `defLang(...)` | One entry per language: interface, symbols, translated names and usages |
| `convert`, `fmtResult`, `fmtHTML`, `parseValue` | Calculation, display of results (3 decimals), display of factors, parsing of the input |
| `applyLang`, `setQuantity`, `update` | Rendering and interface updates |

Quantity keys and unit identifiers in the code are French (for example `longueur`, `puissance`, `debit`).

## Customization

### Adding a unit

In `QUANTITIES`, add a line to the desired quantity:

```js
U("id", "name", "symbol", "Group", "Region. Usage", factor)
```

- `id`: identifier, unique within the quantity.
- `factor`: value of the unit in the reference unit (see the quantities table).
- The units of a same group must stay **contiguous**.
- For temperature, use `T(id, name, symbol, group, usage, toC, fromC, formula)`.

**Translations**: in each language, `names` and `uses` are lists separated by `|`, **aligned by position** with the units of the quantity. Insert the translation at the same position in every language, otherwise the labels will be shifted. The groups (`groups`) follow the order in which the groups first appear.

### Adding a quantity

Add an object to `QUANTITIES`:

```js
{ key: "my_quantity", label: "My quantity", base: "reference unit", from: "input-id", to: "output-id",
  desc: "Description shown under the tabs.", units: [ /* U(...) */ ] }
```

Then add an entry `my_quantity: { label, desc, groups, names, uses }` in each `defLang` (otherwise the display falls back to French).

### Adding a language

1. Copy an existing `defLang("xx", { ... })` block and translate `name`, `locale` (BCP 47 code, for example `"pt-PT"`), `ui`, `sym` and `q`.
2. Add `<option value="xx">Name</option>` to the `<select id="lang">` menu.
3. Check that, for each quantity, `names` and `uses` contain as many items as there are units, and `groups` as many as there are groups.
4. Missing `ui` keys (including `readme`, the link tooltip) fall back to French. Any language other than French opens the English README.

Symbols specific to a language are declared in `sym`, with the key `"quantity.id"` (for example `"puissance.ch": "PS"`).

### README link

The banner link is driven by the `README_URL` constant, at the start of the script:

```js
const README_URL = { fr: "README.md", en: "README.en.md" };
```

`fr` is used when the interface language is French, `en` for all other languages. To point elsewhere, replace them with full addresses (for example those of the repository or the intranet):

```js
const README_URL = {
  fr: "https://example.intranet/docs/converter/README.md",
  en: "https://example.intranet/docs/converter/README.en.md"
};
```

The visible text is `README` followed by `(FR)` or `(EN)`: the word is in the HTML, on the `<a id="readme">` tag, and the suffix is computed by the script each time the language changes. The tooltip and accessible name come from the `readme` key of `ui`, to be adapted in each language.

### Changing the default units

For each quantity, `from` and `to` designate the input and output units offered when the page opens and each time the quantity changes (for example `from: "in", to: "cm"` for length).

The quantity shown on opening is the first in the `QUANTITIES` list (Length). The initial values 1, 2 and 3 are set when the three input rows are created, in the `r.input.value = String(i + 1);` line of the script.

## Compatibility

Recent browsers on computers, iOS and Android. The application notably uses `Intl.NumberFormat`, CSS grid, `inset` and `mask-image`. Without JavaScript, the page does not work.

## License

To be defined by the project owner.
