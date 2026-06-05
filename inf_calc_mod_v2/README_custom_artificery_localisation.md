# Custom Artificery Localisation

This pattern allows an Artificery UI slot to display a different name for a
specific country while keeping the original name for everyone else.

For example, Y01 replaces the displayed name of Black Damestear Bullets with
Korashi Bullets.

## 1. Define Conditional Localisation

Create a file under the mod-root folder:

`customizable_localization/`

Do not place it under `common/customizable_localization/`. EU4 will not load
customizable localisation from there.

Example:

```txt
defined_text = {
	name = GetInfCalcMilitaryInvention31Name
	random = no
	text = {
		trigger = {
			tag = Y01
		}
		localisation_key = inf_calc_military_invention_3_1_korashi
	}
	text = {
		trigger = {
			always = yes
		}
		localisation_key = inf_calc_military_invention_3_1_black_damestear
	}
}
```

The final `always = yes` entry provides a fallback for every other country.

## 2. Replace Existing Artificery Localisation

Existing Anbennar localisation keys should be overridden through:

`localisation/replace/`

Example:

```yml
﻿l_english:
 military_invention_3_1_tt:0 "[Root.GetInfCalcMilitaryInvention31Name]"
 artifice_research.3.m31:0 "[Root.GetInfCalcMilitaryInvention31Name]"
```

Using `localisation/replace` ensures the parent mod's original localisation
does not win a duplicate-key load-order collision.

## 3. Add Resulting Text

Define the localisation keys returned by the conditional function in a normal
localisation file:

```yml
﻿l_english:
 inf_calc_military_invention_3_1_korashi:0 "Korashi Bullets"
 inf_calc_military_invention_3_1_black_damestear:0 "Black Damestear Bullets"
```

Localisation files must use UTF-8 with BOM.

## Troubleshooting

- Original text remains: use `localisation/replace` for the existing key.
- Text becomes empty: ensure the `defined_text` file is in the mod-root
  `customizable_localization` folder and the called function name matches.
- Generated privilege effects show the correct name but the heading does not:
  the privilege wiring works, but the static Artificery tooltip key still
  needs the conditional localisation override.
- Fully restart EU4 after changing customizable localisation.
