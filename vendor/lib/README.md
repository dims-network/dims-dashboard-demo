# Third-party libraries

Served from this repository rather than from a CDN, so the dashboard opens
without reaching any outside host and nothing off-site can change what it
loads. Each file is the unmodified published build.

| file | library | version | licence |
|---|---|---|---|
| `plotly.min.js` | [Plotly.js](https://github.com/plotly/plotly.js) | 2.26.0 | MIT |
| `lodash.min.js` | [Lodash](https://github.com/lodash/lodash) | 4.17.21 | MIT |
| `papaparse.min.js` | [Papa Parse](https://github.com/mholt/PapaParse) | 5.4.1 | MIT |
| `react.production.min.js` | [React](https://github.com/facebook/react) | 18.2.0 | MIT |
| `react-dom.production.min.js` | [React DOM](https://github.com/facebook/react) | 18.2.0 | MIT |

Plotly carries basemap URLs for its geographic chart types. This dashboard
draws none of them, so nothing is fetched at runtime — the offline check in
`ReadMe.MD` is what confirms it.
