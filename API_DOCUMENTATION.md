# Apache ECharts API Documentation

A comprehensive guide to all public APIs, functions, and components in Apache ECharts.

## Table of Contents

1. [Core APIs](#core-apis)
2. [Chart Types](#chart-types)
3. [Components](#components)
4. [Utility Functions](#utility-functions)
5. [Configuration Options](#configuration-options)
6. [Examples](#examples)

## Core APIs

### ECharts Instance Creation and Management

#### `echarts.init(dom, theme?, opts?)`

Creates an ECharts instance.

**Parameters:**
- `dom` (HTMLElement): The DOM container for the chart
- `theme` (string | object, optional): Theme name or theme object
- `opts` (object, optional): Initialization options
  - `devicePixelRatio` (number): Device pixel ratio
  - `renderer` ('canvas' | 'svg'): Renderer type
  - `useDirtyRect` (boolean): Enable dirty rectangle optimization
  - `width` (number | 'auto'): Canvas width
  - `height` (number | 'auto'): Canvas height
  - `locale` (string): Locale for internationalization

**Returns:** ECharts instance

**Example:**
```javascript
// Basic initialization
const chart = echarts.init(document.getElementById('main'));

// With theme and options
const chart = echarts.init(
  document.getElementById('main'),
  'dark',
  { renderer: 'svg' }
);
```

#### `echarts.dispose(chart)`

Disposes an ECharts instance to free memory.

**Parameters:**
- `chart` (ECharts | HTMLElement | string): ECharts instance, DOM element, or instance ID

**Example:**
```javascript
echarts.dispose(chart);
```

#### `echarts.getInstanceByDom(dom)`

Gets an ECharts instance by DOM element.

**Parameters:**
- `dom` (HTMLElement): DOM container

**Returns:** ECharts instance or undefined

#### `echarts.getInstanceById(id)`

Gets an ECharts instance by ID.

**Parameters:**
- `id` (string): Instance ID

**Returns:** ECharts instance or undefined

### ECharts Instance Methods

#### `chart.setOption(option, opts?)`

Sets chart configuration options.

**Parameters:**
- `option` (EChartsOption): Chart configuration
- `opts` (object, optional): Update options
  - `notMerge` (boolean): Replace instead of merge
  - `lazyUpdate` (boolean): Lazy update for performance
  - `silent` (boolean): Suppress events

**Example:**
```javascript
chart.setOption({
  title: { text: 'My Chart' },
  xAxis: { type: 'category', data: ['A', 'B', 'C'] },
  yAxis: { type: 'value' },
  series: [{
    type: 'bar',
    data: [10, 20, 30]
  }]
});
```

#### `chart.getOption()`

Gets current chart configuration.

**Returns:** Current EChartsOption

#### `chart.resize(opts?)`

Resizes the chart.

**Parameters:**
- `opts` (object, optional): Resize options
  - `width` (number | 'auto'): New width
  - `height` (number | 'auto'): New height
  - `silent` (boolean): Suppress resize event

**Example:**
```javascript
// Auto resize
chart.resize();

// Specific dimensions
chart.resize({ width: 800, height: 400 });
```

#### `chart.dispatchAction(payload)`

Dispatches an action to trigger chart behavior.

**Parameters:**
- `payload` (object): Action payload with type and parameters

**Example:**
```javascript
// Highlight data
chart.dispatchAction({
  type: 'highlight',
  seriesIndex: 0,
  dataIndex: 1
});

// Show tooltip
chart.dispatchAction({
  type: 'showTip',
  seriesIndex: 0,
  dataIndex: 1
});
```

#### Event Handling

#### `chart.on(eventName, handler, context?)`

Registers event handler.

**Parameters:**
- `eventName` (string): Event name
- `handler` (function): Event handler
- `context` (object, optional): Handler context

**Example:**
```javascript
chart.on('click', function(params) {
  console.log('Clicked:', params);
});

chart.on('legendselectchanged', function(params) {
  console.log('Legend selection changed:', params);
});
```

#### `chart.off(eventName, handler?)`

Removes event handler.

#### `chart.getDataURL(opts?)`

Gets chart as data URL.

**Parameters:**
- `opts` (object, optional): Export options
  - `type` ('png' | 'jpeg' | 'svg'): Image format
  - `pixelRatio` (number): Pixel ratio
  - `backgroundColor` (string): Background color

**Returns:** Data URL string

### Global Registration Functions

#### `echarts.registerTheme(name, theme)`

Registers a theme.

**Parameters:**
- `name` (string): Theme name
- `theme` (object): Theme configuration

**Example:**
```javascript
echarts.registerTheme('myTheme', {
  color: ['#ff7f50', '#87cefa', '#da70d6'],
  backgroundColor: '#f5f5f5'
});
```

#### `echarts.registerMap(name, geoJson, specialAreas?)`

Registers map data for geo/map charts.

**Parameters:**
- `name` (string): Map name
- `geoJson` (object): GeoJSON data
- `specialAreas` (object, optional): Special area configurations

#### `echarts.use(extension)`

Uses an extension or component.

**Parameters:**
- `extension` (function | object | array): Extension to install

**Example:**
```javascript
import { BarChart, GridComponent } from 'echarts/charts';
echarts.use([BarChart, GridComponent]);
```

## Chart Types

### Line Chart

Line charts display data as connected points along a line.

**Type:** `'line'`

**Key Options:**
- `symbol` (string | function): Point symbol type
- `symbolSize` (number | array | function): Symbol size
- `lineStyle` (object): Line style configuration
- `areaStyle` (object): Area fill style (for area charts)
- `smooth` (boolean | number): Smooth curves
- `step` (boolean | string): Step line style

**Example:**
```javascript
{
  type: 'line',
  data: [120, 132, 101, 134, 90, 230, 210],
  smooth: true,
  lineStyle: {
    color: '#5470c6',
    width: 3
  },
  areaStyle: {
    opacity: 0.3
  }
}
```

### Bar Chart

Bar charts display data as rectangular bars.

**Type:** `'bar'`

**Key Options:**
- `barWidth` (number | string): Bar width
- `barMaxWidth` (number | string): Maximum bar width
- `barGap` (number | string): Gap between bars
- `barCategoryGap` (number | string): Gap between categories
- `stack` (string): Stack name for stacked bars

**Example:**
```javascript
{
  type: 'bar',
  data: [10, 20, 30, 40, 50],
  barWidth: '60%',
  itemStyle: {
    color: '#91cc75',
    borderRadius: [4, 4, 0, 0]
  }
}
```

### Pie Chart

Pie charts display data as circular sectors.

**Type:** `'pie'`

**Key Options:**
- `radius` (number | string | array): Inner and outer radius
- `center` (array): Center position [x, y]
- `roseType` (boolean | 'radius' | 'area'): Rose chart type
- `startAngle` (number): Start angle
- `minAngle` (number): Minimum sector angle

**Example:**
```javascript
{
  type: 'pie',
  radius: ['40%', '70%'],
  center: ['50%', '50%'],
  data: [
    { value: 335, name: 'Direct' },
    { value: 310, name: 'Email' },
    { value: 234, name: 'Union Ads' }
  ],
  emphasis: {
    itemStyle: {
      shadowBlur: 10,
      shadowOffsetX: 0,
      shadowColor: 'rgba(0, 0, 0, 0.5)'
    }
  }
}
```

### Scatter Chart

Scatter charts display data as points in a coordinate system.

**Type:** `'scatter'`

**Key Options:**
- `symbol` (string | function): Point symbol
- `symbolSize` (number | array | function): Symbol size
- `large` (boolean): Large dataset optimization
- `largeThreshold` (number): Threshold for large dataset

**Example:**
```javascript
{
  type: 'scatter',
  data: [[10, 20], [15, 25], [20, 30]],
  symbolSize: function(data) {
    return Math.sqrt(data[2]) * 5;
  }
}
```

### Radar Chart

Radar charts display multivariate data on a polar coordinate system.

**Type:** `'radar'`

**Key Options:**
- `symbol` (string): Point symbol
- `areaStyle` (object): Area fill style
- `lineStyle` (object): Line style

**Example:**
```javascript
{
  type: 'radar',
  data: [
    {
      value: [4300, 10000, 28000, 35000, 50000, 19000],
      name: 'Allocated Budget'
    }
  ],
  areaStyle: {
    opacity: 0.6
  }
}
```

### Additional Chart Types

- **Gauge Chart** (`'gauge'`): Circular gauge displays
- **Funnel Chart** (`'funnel'`): Funnel visualization
- **Graph Chart** (`'graph'`): Network/relationship visualization  
- **Tree Chart** (`'tree'`): Hierarchical tree structure
- **Treemap Chart** (`'treemap'`): Nested rectangles
- **Sankey Chart** (`'sankey'`): Flow diagrams
- **Parallel Chart** (`'parallel'`): Parallel coordinates
- **Sunburst Chart** (`'sunburst'`): Hierarchical pie chart
- **Boxplot Chart** (`'boxplot'`): Statistical box plots
- **Candlestick Chart** (`'candlestick'`): Financial candlestick
- **Heatmap Chart** (`'heatmap'`): Data density visualization
- **Map Chart** (`'map'`): Geographical maps
- **Lines Chart** (`'lines'`): Flight routes/connections
- **Effect Scatter** (`'effectScatter'`): Animated scatter points
- **Pictorial Bar** (`'pictorialBar'`): Custom shaped bars
- **Theme River** (`'themeRiver'`): Flowing timeline
- **Custom Chart** (`'custom'`): Custom rendered charts

## Components

### Grid

Defines the rectangular coordinate system area.

**Configuration:**
```javascript
grid: {
  left: '10%',
  right: '10%',
  top: '15%',
  bottom: '10%',
  containLabel: true
}
```

**Key Options:**
- `left/right/top/bottom` (number | string): Grid positioning
- `width/height` (number | string): Grid dimensions
- `containLabel` (boolean): Include axis labels in grid area
- `backgroundColor` (string): Grid background color
- `borderColor` (string): Grid border color

### Axes (xAxis/yAxis)

Defines coordinate system axes.

**Configuration:**
```javascript
xAxis: {
  type: 'category',
  data: ['Mon', 'Tue', 'Wed', 'Thu', 'Fri'],
  axisLabel: {
    rotate: 45
  }
},
yAxis: {
  type: 'value',
  min: 0,
  max: 100
}
```

**Key Options:**
- `type` ('value' | 'category' | 'time' | 'log'): Axis type
- `data` (array): Category data for category axis
- `min/max` (number | string | function): Axis range
- `interval` (number): Tick interval
- `axisLabel` (object): Label styling
- `axisTick` (object): Tick mark styling
- `axisLine` (object): Axis line styling
- `splitLine` (object): Grid line styling

### Title

Chart title component.

**Configuration:**
```javascript
title: {
  text: 'Main Title',
  subtext: 'Subtitle',
  left: 'center',
  textStyle: {
    fontSize: 18,
    fontWeight: 'bold'
  }
}
```

**Key Options:**
- `text` (string): Main title text
- `subtext` (string): Subtitle text
- `left/right/top/bottom` (number | string): Positioning
- `textStyle/subtextStyle` (object): Text styling
- `itemGap` (number): Gap between title and subtitle

### Legend

Legend component for series identification.

**Configuration:**
```javascript
legend: {
  type: 'plain',
  orient: 'horizontal',
  left: 'center',
  top: 'bottom',
  data: ['Series 1', 'Series 2']
}
```

**Key Options:**
- `type` ('plain' | 'scroll'): Legend type
- `orient` ('horizontal' | 'vertical'): Orientation
- `left/right/top/bottom` (number | string): Positioning
- `data` (array): Legend items
- `selected` (object): Initial selection state
- `itemWidth/itemHeight` (number): Legend item dimensions

### Tooltip

Interactive tooltip component.

**Configuration:**
```javascript
tooltip: {
  trigger: 'item',
  formatter: '{a} <br/>{b}: {c} ({d}%)',
  backgroundColor: 'rgba(0,0,0,0.7)',
  borderColor: '#ccc',
  textStyle: {
    color: '#fff'
  }
}
```

**Key Options:**
- `trigger` ('item' | 'axis' | 'none'): Trigger type
- `formatter` (string | function): Content formatter
- `backgroundColor` (string): Background color
- `borderColor/borderWidth` (string/number): Border styling
- `position` (array | string | function): Tooltip position
- `showDelay/hideDelay` (number): Show/hide delays

### DataZoom

Data zoom component for axis range selection.

**Configuration:**
```javascript
dataZoom: [
  {
    type: 'slider',
    xAxisIndex: 0,
    start: 10,
    end: 90
  },
  {
    type: 'inside',
    xAxisIndex: 0
  }
]
```

**Types:**
- `'slider'`: Slider-based zoom control
- `'inside'`: Mouse/touch-based zoom

**Key Options:**
- `xAxisIndex/yAxisIndex` (number | array): Target axes
- `start/end` (number): Initial zoom range (percentage)
- `minSpan/maxSpan` (number): Zoom limits
- `orient` ('horizontal' | 'vertical'): Orientation

### VisualMap

Visual mapping component for data visualization.

**Configuration:**
```javascript
visualMap: {
  type: 'continuous',
  min: 0,
  max: 100,
  inRange: {
    color: ['#50a3ba', '#eac736', '#d94e5d']
  },
  orient: 'vertical',
  left: 'left'
}
```

**Types:**
- `'continuous'`: Continuous color mapping
- `'piecewise'`: Discrete color segments

### Toolbox

Interactive toolbox with utility functions.

**Configuration:**
```javascript
toolbox: {
  feature: {
    saveAsImage: {},
    dataView: { readOnly: false },
    magicType: { type: ['line', 'bar'] },
    restore: {},
    dataZoom: {}
  }
}
```

**Features:**
- `saveAsImage`: Save chart as image
- `dataView`: View/edit raw data
- `magicType`: Switch chart types
- `restore`: Reset chart
- `dataZoom`: Data zoom controls
- `brush`: Data brush selection

### Brush

Data brush selection component.

**Configuration:**
```javascript
brush: {
  toolbox: ['rect', 'polygon', 'clear'],
  xAxisIndex: 0,
  brushStyle: {
    borderWidth: 1,
    color: 'rgba(120,140,180,0.3)'
  }
}
```

### AxisPointer

Axis indicator component.

**Configuration:**
```javascript
axisPointer: {
  type: 'cross',
  crossStyle: {
    color: '#999'
  }
}
```

**Types:**
- `'line'`: Line indicator
- `'shadow'`: Shadow indicator  
- `'cross'`: Cross indicator

## Utility Functions

### echarts.graphic

Graphics utilities for custom drawing.

**Functions:**
- `echarts.graphic.clipPointsByRect(points, rect)`: Clip points by rectangle
- `echarts.graphic.clipRectByRect(targetRect, rect)`: Clip rectangle by rectangle

### echarts.number

Number formatting utilities.

**Functions:**
- `echarts.number.addCommas(num)`: Add thousand separators
- `echarts.number.toCamelCase(str)`: Convert to camel case
- `echarts.number.normalizeCssArray(val)`: Normalize CSS array values

### echarts.format

Text formatting utilities.

**Functions:**
- `echarts.format.formatTime(template, value)`: Format time values
- `echarts.format.formatTpl(tpl, paramsList)`: Template formatting
- `echarts.format.getTooltipMarker(color)`: Generate tooltip color marker

### echarts.util

General utilities.

**Functions:**
- `echarts.util.map(arr, callback)`: Array mapping
- `echarts.util.filter(arr, callback)`: Array filtering  
- `echarts.util.reduce(arr, callback, initial)`: Array reduction
- `echarts.util.createHashMap(obj)`: Create hash map
- `echarts.util.clone(obj)`: Deep clone object
- `echarts.util.merge(target, source)`: Merge objects

### echarts.color

Color utilities.

**Functions:**
- `echarts.color.parse(colorStr)`: Parse color string
- `echarts.color.lift(color, level)`: Lighten/darken color
- `echarts.color.toHex(color)`: Convert to hex format
- `echarts.color.stringify(color)`: Convert color to string

### echarts.matrix

Matrix transformation utilities.

**Functions:**
- `echarts.matrix.create()`: Create identity matrix
- `echarts.matrix.translate(matrix, x, y)`: Apply translation
- `echarts.matrix.rotate(matrix, angle)`: Apply rotation
- `echarts.matrix.scale(matrix, x, y)`: Apply scaling

## Configuration Options

### Animation Options

Global animation configuration.

```javascript
{
  animation: true,
  animationDuration: 1000,
  animationEasing: 'cubicOut',
  animationDelay: 0,
  animationDurationUpdate: 300,
  animationEasingUpdate: 'cubicOut',
  animationDelayUpdate: 0
}
```

### Global Style Options

```javascript
{
  color: ['#5470c6', '#91cc75', '#fac858'],
  backgroundColor: 'transparent',
  textStyle: {
    fontFamily: 'sans-serif',
    fontSize: 12,
    fontWeight: 'normal',
    color: '#333'
  }
}
```

### Responsive Options

```javascript
{
  media: [
    {
      query: { maxWidth: 500 },
      option: {
        legend: { orient: 'vertical' },
        grid: { left: '10%', right: '10%' }
      }
    }
  ]
}
```

## Examples

### Basic Line Chart

```javascript
const option = {
  title: { text: 'Line Chart Example' },
  tooltip: { trigger: 'axis' },
  legend: { data: ['Sales', 'Profit'] },
  xAxis: {
    type: 'category',
    data: ['Jan', 'Feb', 'Mar', 'Apr', 'May', 'Jun']
  },
  yAxis: { type: 'value' },
  series: [
    {
      name: 'Sales',
      type: 'line',
      data: [120, 132, 101, 134, 90, 230]
    },
    {
      name: 'Profit', 
      type: 'line',
      data: [220, 182, 191, 234, 290, 330]
    }
  ]
};

chart.setOption(option);
```

### Interactive Bar Chart

```javascript
const option = {
  title: { text: 'Interactive Bar Chart' },
  tooltip: {
    trigger: 'axis',
    axisPointer: { type: 'shadow' }
  },
  toolbox: {
    feature: {
      dataView: { show: true, readOnly: false },
      magicType: { show: true, type: ['line', 'bar'] },
      restore: { show: true },
      saveAsImage: { show: true }
    }
  },
  xAxis: {
    type: 'category',
    data: ['Mon', 'Tue', 'Wed', 'Thu', 'Fri', 'Sat', 'Sun']
  },
  yAxis: { type: 'value' },
  series: [{
    type: 'bar',
    data: [120, 200, 150, 80, 70, 110, 130],
    markPoint: {
      data: [
        { type: 'max', name: 'Max' },
        { type: 'min', name: 'Min' }
      ]
    },
    markLine: {
      data: [{ type: 'average', name: 'Average' }]
    }
  }]
};

chart.setOption(option);
```

### Multi-Series Pie Chart

```javascript
const option = {
  title: {
    text: 'Multi-Series Pie Chart',
    left: 'center'
  },
  tooltip: {
    trigger: 'item',
    formatter: '{a} <br/>{b}: {c} ({d}%)'
  },
  legend: {
    orient: 'vertical',
    left: 'left',
    data: ['Direct', 'Email', 'Union Ads', 'Video Ads', 'Search Engine']
  },
  series: [
    {
      name: 'Access From',
      type: 'pie',
      radius: '55%',
      center: ['50%', '60%'],
      data: [
        { value: 335, name: 'Direct' },
        { value: 310, name: 'Email' },
        { value: 234, name: 'Union Ads' },
        { value: 135, name: 'Video Ads' },
        { value: 1548, name: 'Search Engine' }
      ],
      emphasis: {
        itemStyle: {
          shadowBlur: 10,
          shadowOffsetX: 0,
          shadowColor: 'rgba(0, 0, 0, 0.5)'
        }
      }
    }
  ]
};

chart.setOption(option);
```

### Dynamic Data Updates

```javascript
// Initial setup
let data = [5, 20, 36, 10, 10, 20];
const option = {
  xAxis: {
    type: 'category',
    data: ['A', 'B', 'C', 'D', 'E', 'F']
  },
  yAxis: { type: 'value' },
  series: [{
    type: 'bar',
    data: data
  }]
};

chart.setOption(option);

// Update data periodically
setInterval(() => {
  data = data.map(() => Math.round(Math.random() * 100));
  chart.setOption({
    series: [{ data: data }]
  });
}, 2000);
```

### Custom Styling Example

```javascript
const option = {
  backgroundColor: '#2c343c',
  title: {
    text: 'Custom Styled Chart',
    left: 'center',
    top: 20,
    textStyle: {
      color: '#ccc'
    }
  },
  tooltip: {
    trigger: 'item',
    backgroundColor: 'rgba(0,0,0,0.7)',
    borderColor: '#ccc',
    textStyle: { color: '#fff' }
  },
  visualMap: {
    show: false,
    min: 80,
    max: 600,
    inRange: {
      colorLightness: [0, 1]
    }
  },
  series: [{
    type: 'pie',
    radius: '55%',
    center: ['50%', '50%'],
    data: [
      { value: 335, name: 'Direct' },
      { value: 310, name: 'Email' },
      { value: 274, name: 'Union Ads' },
      { value: 235, name: 'Video Ads' },
      { value: 400, name: 'Search Engine' }
    ].sort((a, b) => a.value - b.value),
    roseType: 'radius',
    label: {
      color: 'rgba(255, 255, 255, 0.3)'
    },
    labelLine: {
      lineStyle: {
        color: 'rgba(255, 255, 255, 0.3)'
      },
      smooth: 0.2,
      length: 10,
      length2: 20
    },
    itemStyle: {
      color: '#c23531',
      shadowBlur: 200,
      shadowColor: 'rgba(0, 0, 0, 0.5)'
    },
    animationType: 'scale',
    animationEasing: 'elasticOut',
    animationDelay: function (idx) {
      return Math.random() * 200;
    }
  }]
};

chart.setOption(option);
```

---

## Additional Resources

- **Official Website:** https://echarts.apache.org
- **API Reference:** https://echarts.apache.org/api.html
- **Option Manual:** https://echarts.apache.org/option.html
- **Examples:** https://echarts.apache.org/examples
- **GitHub Repository:** https://github.com/apache/echarts

This documentation covers the most commonly used APIs and features. For advanced usage and specific chart configurations, refer to the official documentation and examples.