# Guwahati Flood Investigation

An interactive, browser‑based investigation exploring the devastating floods of 2022 in Guwahati using satellite imagery, soil analysis, and a regional impact map to guide resilience planning.

## Overview

This project offers a step‑by‑step experience where users act as scientists to analyze pre/post‑monsoon conditions, relate soil composition to flood behavior, and assess localized impacts via an interactive map, ending with an actionable resilience roadmap.

## Features

- Catlogue of Synthetic Aperture Radio (SAR) working and functioning
- Soil composition overlays and legend covering clayey, alluvial, sandy‑loam, and vegetation with risk context.
- Regional impact map with clickable markers; right‑hand details panel updates soil type, humidity, cloud cover, impact, and image.
- Resilience roadmap detailing drainage upgrades, flood‑resistant infrastructure, riverbank reinforcement, and risk‑zone mapping.

## Why It Matters

Clayey and alluvial soils influence retention and waterlogging; combining soil, basic weather indicators, and on‑map impacts helps target interventions to reduce human, agricultural, and infrastructure losses effectively.

## Tech Stack

- HTML/CSS for layout
- JavaScript powering the image slider, marker interactions, and sidebar updates.
- Leaflet with OpenStreetMap tiles for the interactive map and custom markers.

## Project Structure

- Step 1: Satellite Imagery Analysis — hover/drag to compare before/after conditions.
- Step 2: Soil Composition Analysis — overlays with color‑coded legend and risk notes.
- Step 3: Regional Impact Assessment — click markers to populate details in the sidebar.
- Step 4: Roadmap to Resilience — milestones for drainage, infrastructure, riverbanks, and risk‑zone policy.

## Getting Started

1. Clone the repository and open the main HTML file in a modern browser; no backend required.
2. Keep internet access for Leaflet/OSM tiles or configure local tiles if offline.
3. Click map markers to view location‑specific impact details in the dialog panel.

## Customization

- Edit the locations array to add districts, attributes, and images.
- Replace placeholder images and adjust soil overlay colors/labels to match domain classification schemes.
- Extend the roadmap with organization‑specific milestones and timelines.

## Use Cases

- Rapid impact briefings for officials and disaster response teams.
- Educational demos linking land composition and flood dynamics.
- Planning workshops to prioritize mitigation with stakeholders.

## License

Use, modify, and adapt for research, education, and planning contexts; please attribute project structure and interaction design in derivatives.

## Acknowledgments[1]

Built on Leaflet and OpenStreetMap data with an emphasis on storytelling from imagery to soil to on‑the‑ground impacts for resilient planning in Assam’s flood‑prone corridor.
