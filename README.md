<div align="center">

# ❄️ Neon Climate Card

**A cyberpunk climate card for Home Assistant, with a real fluid simulation for the airflow.**

[![HACS Custom][hacs-badge]][hacs-url]
[![Release][release-badge]][release-url]
[![Validate][validate-badge]][validate-url]
[![License: MIT][license-badge]][license-url]

[![Open your Home Assistant instance and open this repository in HACS.](https://my.home-assistant.io/badges/hacs_repository.svg)](https://my.home-assistant.io/redirect/hacs_repository/?owner=cerealkiller57540&repository=neon-climate-card&category=plugin)

<img src="https://raw.githubusercontent.com/cerealkiller57540/neon-climate-card/main/images/cool.gif" alt="Neon Climate Card in cooling mode, airflow animated by a WebGL fluid solver" width="448">

</div>

The air under the louvres is not a looping animation: it is a small Navier–Stokes solver running on the GPU. One jet per slot, the colour follows the HVAC mode, the strength follows the fan speed, and the slats follow `swing_mode`.

<p>
  <img src="https://raw.githubusercontent.com/cerealkiller57540/neon-climate-card/main/images/heat.png" alt="Neon Climate Card in heating mode" width="400">
  <img src="https://raw.githubusercontent.com/cerealkiller57540/neon-climate-card/main/images/off.png" alt="Neon Climate Card switched off" width="400">
</p>

## ✨ Features

- **Two cards in one install**
  - `neon-climate-card-webgl`: airflow rendered by a WebGL fluid solver (recommended).
  - `neon-climate-card`: lighter version, same look, airflow drawn with CSS/canvas 2D.
- **Every HVAC mode** your entity supports (heat, cool, dry, fan only, auto, off), each with its own colour.
- **Draggable setpoint slider** whose scale follows your entity's `min_temp`, `max_temp` and `target_temp_step`.
- **Fan speed and swing controls** read from `fan_modes` and `swing_modes`.
- **Optional real-power gating**: point `power_entity` at a power sensor and the airflow only shows when the unit actually draws power.
- **Full visual editor**, with no YAML needed. The WebGL card exposes all 19 airflow settings as sliders.
- **Built for phones and tablets**: decorative animations are toned down on mobile, the solver sleeps when the card is off-screen, and it releases its WebGL context when it is removed (Android WebViews cap a page at 8 contexts).
- Respects `prefers-reduced-motion`.

## 📦 Installation

### HACS (recommended)

1. Click the **Open in HACS** button above, or add this repository as a custom repository in HACS (category **Dashboard**): `https://github.com/cerealkiller57540/neon-climate-card`.
2. Download **Neon Climate Card**.
3. Reload your browser.

HACS registers one resource, `neon-climate-card.js`. It loads the WebGL variant on its own, so **do not** add `neon-climate-card-webgl.js` as a second resource.

### Manual

1. Copy both files from [`dist/`](dist) to `config/www/neon-climate-card/`.
2. Add a dashboard resource: URL `/local/neon-climate-card/neon-climate-card.js`, type **JavaScript module**.

## 🚀 Usage

Add a card from the dashboard editor and search for **Neon Climate**, or use YAML:

```yaml
type: custom:neon-climate-card-webgl
entity: climate.living_room
name: Living Room
humidity_entity: sensor.living_room_humidity   # optional
header:
  title: Climate
  icon: mdi:heat-pump
```

## ⚙️ Options

| Option | Type | Default | Description |
|---|---|---|---|
| `entity` | string | **required** | A `climate.*` entity |
| `name` | string | `friendly_name` | Label shown above the display |
| `humidity_entity` | string | — | Sensor used for the humidity readout (otherwise `current_humidity`) |
| `show_wind` | bool | `true` | Show the airflow under the louvres |
| `power_entity` | string | — | Power sensor: the airflow only shows above `power_threshold` |
| `power_threshold` | number | `10` | Watts |
| `color_heat` / `color_cool` / `color_dry` / `color_fan` / `color_off` | colour | `#FF2D6B` / `#5B7CFF` / `#00E0C0` / `#00FFAA` / `#6a7aaa` | Colour per mode |
| `color_pill` | colour | `#B400FF` | Swing pill |
| `color_fan_btn` | colour | `#00FFAA` | Fan button |
| `color_display` | colour | `#00fff9` | Dot-matrix display |
| `neon_display_glow` | bool | `true` | Triple glow on the display digits |
| `header` | object | — | `title`, `subtitle`, `icon`, `icon_color`, `icon_size`, `title_size`, `glow`, `glow_color`, `glow_size`, `gradient`, `gradient_from`, `gradient_to` |

**WebGL card only**

| Option | Default | Description |
|---|---|---|
| `flow_quality` | `auto` | `auto` (1 device pixel per CSS pixel on dense screens), `full` (up to 2×), `light`, `off` |
| `flow_count` | `5` | Number of slots, one jet each |
| `flow_alpha` | `0.39` | Overall exposure |
| `flow_smoke` | `2.32` | Dye rate, multiplied by fan speed |
| `flow_curl` | `19` | Vorticity (turbulence) |
| `flow_hue` | `-1` | `-1` = colour from the HVAC mode, otherwise a fixed hue (0–360) |

The other `flow_*` settings (`fan`, `wobble`, `speed`, `taper`, `glow`, `smoke_scale`, `smoke_speed`, `louver`, `force`, `v_dissip`, `d_dissip`, `sparkle`, `sparkle_size`, `fade`) are easiest to tune from the visual editor, where each slider is documented.

## ❓ FAQ

**The airflow does not show.** It only shows when the unit is not `off`. If you set `power_entity`, it also needs the sensor to read above `power_threshold`.

**Some cards go blank on my Android phone.** Android WebViews keep at most 8 WebGL contexts per page and drop the oldest one. This card uses a single context and only while the airflow is visible. If you run many WebGL cards on one view, use `neon-climate-card` (CSS) or `flow_quality: off` on some of them.

**Which theme is in the screenshots?** Neo Tokyo, from [Home-Assistant-Neon-Cards](https://github.com/cerealkiller57540/Home-Assistant-Neon-Cards). The card works with any theme.

## 🌃 More neon cards

This card is part of a family. See the full collection at [**Home-Assistant-Neon-Cards**](https://github.com/cerealkiller57540/Home-Assistant-Neon-Cards).

---

## 🐾 Support this project

If you enjoy these cards, please consider donating to **Quatre Pattes**, an animal rescue organization.

[![Sauver des animaux](https://img.shields.io/badge/🐾%20Sauver%20des%20animaux-Faire%20un%20don-ff69b4?style=for-the-badge)](https://don.quatre-pattes.org/s/?_jtsuid=70083177244599792679303)

> 💛 No need to support me — just help the animals. Thank you!

---

## 🤝 Contributing

1. Fork the repo
2. Create your branch: `git checkout -b feature/my-card`
3. Commit and push
4. Open a Pull Request

---

## 📄 License

[MIT License][license-url]

[hacs-badge]: https://img.shields.io/badge/HACS-Custom-orange.svg?style=for-the-badge
[hacs-url]: https://hacs.xyz
[release-badge]: https://img.shields.io/github/v/release/cerealkiller57540/neon-climate-card?style=for-the-badge
[release-url]: https://github.com/cerealkiller57540/neon-climate-card/releases
[validate-badge]: https://img.shields.io/github/actions/workflow/status/cerealkiller57540/neon-climate-card/validate.yml?branch=main&label=HACS&style=for-the-badge
[validate-url]: https://github.com/cerealkiller57540/neon-climate-card/actions/workflows/validate.yml
[license-badge]: https://img.shields.io/github/license/cerealkiller57540/neon-climate-card?style=for-the-badge
[license-url]: LICENSE
