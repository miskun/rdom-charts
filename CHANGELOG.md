# Changelog

All notable changes to `rdom-charts` are documented here.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [Unreleased]

Ported to `rdom-tui` 0.6. The chart types and their behavior are unchanged, but `rdom-tui`'s types
(`Color`, `TuiDom`, `NodeId`, `RenderContext`) appear in this crate's public API, so this is a
breaking release (0.2.0) for anyone still on `rdom-tui` 0.3.

### Changed

- **Depends on `rdom-tui = "0.6.0"`** (was `0.3.14`); `rust-version` raised to 1.88 to match.
- **Filling the screen:** rdom 0.6 lays the document root out in block flow, so a chart canvas (or
  its container) placed at the top level fills the viewport with `height: 100%`
  (`Size::Percent(100.0)`, plus `box-sizing: border-box` when it has padding). `Size::Flex(1.0)`
  there now gives it no height. The README quick-start and all four examples use the new form; see
  rdom's `UPGRADING-0.6.md` (`sc-root-block`).
- Examples use `Size::Flex(1.0)` (an `f32` weight since rdom 0.6) and `App::with_tick_handler`
  (was `App::on_tick`).

## [0.1.0]

First release: terminal **chart components** for [rdom](https://github.com/miskun/rdom),
built strictly on `rdom-tui`'s public `<canvas>` paint API. Each chart is a pure-logic type plus a
`*View` handle that mounts it on a `<canvas>` and paints through the `RenderContext` — theme-agnostic
(`rdom_tui::Color`/`Style`), with the rasterization math kept independent of the paint layer.

### Added

- **Time-series line chart** — `TimeSeriesChart` / `TimeSeriesView`: braille (2×4 sub-cell) line
  rendering, static + streaming constructors, windowing/follow/zoom/pan, EMA smoothing, nice-tick
  axes, a legend, `Guideline` threshold lines, and an empty state. `install_interaction` wires
  keyboard (`+`/`-`/`h`/`l`/arrows/`0`), wheel-zoom, and drag-pan, each requesting a repaint.
- **Sparkline** — `Sparkline` / `SparklineView`: a compact single-series braille line with no
  chrome (the whole canvas is plot area), auto or pinned vertical range, and `NaN` gaps.
- **Bar chart** — `BarChart` / `BarChartView` (+ `Bar`): horizontal labeled bars with eighth-block
  sub-cell fill, auto or pinned scale, and an optional trailing value readout.
- **Gauge** — `Gauge` / `GaugeView` (+ `GaugeZone`): a linear gauge whose fill color is chosen by
  the value's zone, with an optional label and numeric readout.
- **Palette** — a theme-agnostic default `SERIES_PALETTE` + `series_color()` auto-assignment;
  callers pass explicit `rdom_tui::Color`s for their own theme.
- **Runnable examples** — `timeseries_gallery`, `interactive_chart`, `live_chart`, `dashboard`.

Built on `rdom-tui = "0.3.14"`; no path dependency and no reach into rdom internals.

[Unreleased]: https://github.com/miskun/rdom-charts/compare/v0.1.0...HEAD
[0.1.0]: https://github.com/miskun/rdom-charts/releases/tag/v0.1.0
