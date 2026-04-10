# India Energy Scenario Simulator

## Concept & Vision

A clean, educational simulator that models India's energy market dynamics. The tool feels like a professional analyst's dashboard—minimal, data-focused, and insight-driven. It helps users understand how renewable growth and demand changes impact coal dependency, emissions, and energy security through clear visualizations and automatically generated insights.

## Design Language

**Aesthetic**: Professional data dashboard with energy-sector color coding (green for renewables, dark gray for coal, blue for demand)

**Color Palette**:
- Primary: `#1a365d` (deep navy)
- Secondary: `#2d3748` (slate gray)
- Accent Green: `#38a169` (renewables)
- Accent Coal: `#4a5568` (coal)
- Accent Blue: `#3182ce` (demand/info)
- Warning: `#d69e2e` (surplus warning)
- Danger: `#c53030` (deficit/emissions)
- Background: `#f7fafc` (light gray)
- Card Background: `#ffffff`
- Text Primary: `#1a202c`
- Text Secondary: `#718096`

**Typography**:
- Headings: Inter (600 weight), fallback: system-ui, sans-serif
- Body: Inter (400 weight), fallback: system-ui, sans-serif
- Numbers/Data: JetBrains Mono (monospace for alignment)

**Spatial System**:
- Base unit: 8px
- Card padding: 24px
- Section gaps: 32px
- Max content width: 1200px

**Motion Philosophy**:
- Subtle transitions on input changes (200ms ease)
- Smooth chart animations (400ms)
- No flashy animations—professional and understated

## Layout & Structure

```
┌─────────────────────────────────────────────────────────────┐
│  Header: Title + Brief Description                          │
├─────────────────────────────────────────────────────────────┤
│  ┌─────────────────┐  ┌─────────────────────────────────┐   │
│  │  INPUT PANEL    │  │  RESULTS PANEL                  │   │
│  │                 │  │                                 │   │
│  │  • Renewables %  │  │  Energy Mix Chart (Bar)         │   │
│  │  • Demand Growth │  │                                 │   │
│  │                 │  │  Emissions Indicator            │   │
│  │  [Simulate]     │  │  Supply-Demand Indicator        │   │
│  └─────────────────┘  │                                 │   │
│                       │  Key Metrics Cards               │   │
│                       └─────────────────────────────────┘   │
├─────────────────────────────────────────────────────────────┤
│  KEY INSIGHTS SECTION                                       │
│  3-5 auto-generated analytical insights                     │
├─────────────────────────────────────────────────────────────┤
│  DOCUMENTATION / ASSUMPTIONS                                │
│  • Model explanation                                        │
│  • Assumptions table                                        │
│  • Interview talking points                                 │
└─────────────────────────────────────────────────────────────┘
```

## Features & Interactions

### Input Controls
- **Renewable Share Slider**: Range 5-60%, step 1%, default 25%
- **Demand Growth Slider**: Range -5% to +15%, step 0.5%, default 5%
- **Simulate Button**: Triggers calculation and update
- Real-time update on slider change (debounced 300ms)

### Core Calculations
```
Base Demand = 1500 TWh (India FY2024)
Total Demand = Base × (1 + Growth %)
Renewable Generation = Total Demand × Renewable %
Coal Generation = Total Demand - Renewable Generation
Carbon Emissions = Coal Generation × 0.82 kg CO2/kWh
Supply Capacity = Base Capacity × (1 + Growth %) [assume 80% utilization baseline]
Surplus/Deficit = Supply - Total Demand
```

### Output Indicators
- **Energy Mix**: Horizontal bar chart showing renewable vs coal split
- **Emissions Level**: Color-coded indicator (Green < 500 Mt, Yellow 500-800 Mt, Red > 800 Mt)
- **Supply-Demand Status**: Color-coded indicator (Green = surplus, Yellow = balanced, Red = deficit)
- **Metric Cards**: Total Demand (TWh), Renewable (TWh), Coal (TWh), Emissions (Mt CO2)

### Key Insights Engine
Auto-generates 3-5 insights based on:
1. Coal dependency despite renewable growth
2. Demand growth impact on supply gap
3. Renewable intermittency limitations
4. Emissions trajectory comparison
5. Supply security concerns

### Documentation Section
- Expandable/collapsible sections
- Assumptions table with values and rationale
- "How It Works" explanation in simple terms
- Interview explanation section

## Component Inventory

### Input Card
- White background, subtle shadow
- Label above each slider
- Current value displayed in large text
- Slider with custom styling matching color palette
- States: default, focused (blue ring)

### Metric Card
- White background with left color border
- Icon, label, large value, unit
- States: default only (static display)

### Indicator Component
- Rounded rectangle with icon
- Background color based on status
- Text showing status and value
- States: positive (green), neutral (yellow), negative (red)

### Insight Card
- Light background with left accent border
- Numbered badge
- Title in bold, description in regular weight
- States: default, highlighted on hover

### Chart Container
- White card with title
- Chart.js canvas element
- Legend below chart
- Responsive sizing

## Technical Approach

**Framework**: Vanilla HTML/CSS/JavaScript (no build step needed)

**Libraries**:
- Chart.js 4.x for visualizations (CDN)
- Inter font from Google Fonts
- JetBrains Mono from Google Fonts

**Architecture**:
- Single HTML file with embedded CSS and JS
- Module pattern for JS organization
- Event-driven updates on input changes

**Data Flow**:
```
User Input → Validation → Calculation Engine → Update State → Render UI → Generate Insights
```

**Assumptions (India FY2024 baseline)**:
- Total electricity demand: 1,500 TWh
- Renewable share: ~25% (125 GW installed capacity)
- Coal share: ~75% (~215 GW installed)
- Emission factor: 0.82 kg CO2/kWh (India grid average)
- Supply capacity utilization: ~80%
- Base installed capacity: ~450 GW

**Key Insights Logic**:
- Rules-based system evaluating calculated metrics
- Thresholds for generating specific insights
- Always includes at least one positive and one concern
