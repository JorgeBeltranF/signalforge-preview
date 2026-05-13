# Third-Party Licenses

SignalForge vendors the following third-party JavaScript assets so generated preview reports can render offline as a single self-contained HTML file.

## Included assets

1. `Chart.js`
   - Upstream: [chartjs/Chart.js](https://github.com/chartjs/Chart.js)
   - Vendored asset: `src/report/assets/chart.umd.min.js`
   - Version header observed in vendored file: `v4.5.1`
   - License: MIT

2. `chartjs-adapter-date-fns`
   - Upstream: [chartjs/chartjs-adapter-date-fns](https://github.com/chartjs/chartjs-adapter-date-fns)
   - Vendored asset: `src/report/assets/chartjs-adapter-date-fns.bundle.min.js`
   - Version header observed in vendored file: `v3.0.0`
   - License: MIT

These assets are redistributed only to support offline/local preview rendering of generated SignalForge HTML reports.

## MIT License notice

The following MIT license text applies to the vendored assets above according to their upstream repositories.

```text
The MIT License (MIT)

Copyright (c) 2014-2024 Chart.js Contributors

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.
```

## Source notes

- Chart.js official repository states it is available under the MIT license.
- chartjs-adapter-date-fns official repository states it is available under the MIT license.
