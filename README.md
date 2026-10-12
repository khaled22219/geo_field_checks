# geo_field_checks

An interactive and automated map for **high-frequency checks (HFC)** on the Malengo follow-up survey in Uganda. It compares the GPS point recorded by SurveyCTO during each interview with the home location the respondent reported. This helps spot interviews that may not have happened where they were supposed to, hence supporting likely evidence of fabrications for in-person interviews. 

**🗺️ Live map:** https://khaled22219.github.io/geo_field_checks/

---

## What the map shows

Each dot is one interview. Each dot carries two independent checks:

### 1. Fill color: village-level check
Respondents who report the same town or village should have interview GPS points close together. For each group of respondents who share a reported location, the map takes the group's **median GPS point** as the "expected" location. It then measures how far each interview is from that point.

| Fill | Meaning |
|---|---|
| 🔴 Red | **Flagged**: more than **1 km** from the median point of its location group |
| 🟢 Green | **OK**: within 1 km (acceptable buffer) | 
| ⚪ Grey | **Not comparable**: no one else reported this location, so there is nothing to compare against |

### 2. Border: district-level check
Each GPS point is matched against official Uganda district boundaries (UN OCHA COD-AB). The resulting district is compared with the district the respondent reported. This works even for single respondents with no group.

| Border | Meaning |
|---|---|
| ⚪ Thin / white | **Match**: the GPS point falls inside the reported district |
| 🟠 Orange | **Mismatch, neighboring district**: usually a boundary quirk (e.g. Kampala vs. Wakiso), not a real problem |
| ⚫ Thick black | **Mismatch, non-neighboring district**: a strong signal worth following up |

A **red dot with a black border** is the strongest warning: the interview is far from both its peers and its reported district.

## How to use it

- **Filter by enumerator** with the dropdown to see one person's interviews.
- **Filter by distance** with the slider. Drag the left end to `-1` to include the grey "not comparable" points.
- **Click a dot** to see its details: interview ID, enumerator, date, reported location, distance from the group, and reported vs. actual district.
- The counters above the map update as you filter.
- Use the fullscreen button on the map for a larger view.

## How it's made

The map is produced by `geo_check.R` in the project's HFC folder. The script runs from `enumerators_checks.do`, which exports the interview data. It uses R's `leaflet` and `crosstalk` packages. This repo only hosts the output:

```
index.html   the map (a copy of Graphs/geo_check_map.html)
lib/         JavaScript and CSS libraries the map needs to run
```

## Updating the map

1. Rerun the HFC checks. This rewrites `Graphs/geo_check_map.html`.
2. Copy it into this repo and rename it to `index.html`.
3. Upload or commit it here (on GitHub: **Add file → Upload files → Commit changes**).
4. GitHub Pages refreshes the live map within a minute or two.

You only need to re-upload `lib/` if the R packages are updated.

---

⚠️ **Data note:** this map contains interview GPS locations and respondent identifiers. It is intended for the field team's quality checks only. Please don't share the link beyond the people who need it.
