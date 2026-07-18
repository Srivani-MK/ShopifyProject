# Exporting screenshots

The README embeds images from this folder. Export each report page from Power BI Desktop
(**File → Export → Export to PDF**, then screenshot each page, or simpler: resize the window
to a clean 16:9 and use **Win+Shift+S** per page) and save with these exact filenames so the
README links resolve automatically:

| Filename | Report page |
|---|---|
| `01-navigation-summary.png` | Navigation & Summary |
| `02-executive-overview.png` | Executive Overview |
| `03-orders.png` | Orders |
| `04-product-customer-insights.png` | Product & Customer Insights |
| `05-top5-products-pareto.png` | Top 5 Products Revenue contribution |
| `06-marketing-ad-performance.png` | Marketing / Ad Performance |

Tips for clean captures:
- Use a consistent browser/app zoom (100%) so all screenshots are the same scale.
- Crop out the Power BI Desktop chrome (ribbon, page tabs) — image should start at the report canvas.
- PNG, ~1600px wide is plenty; keep each file under ~500KB (use a tool like Squoosh/TinyPNG if larger) so the repo stays fast to clone.
- Once added, delete this NOTES.md — it's a checklist, not part of the portfolio presentation.
