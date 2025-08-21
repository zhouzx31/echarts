# Advanced ECharts Examples

This document provides comprehensive examples demonstrating advanced features and use cases of Apache ECharts.

## Table of Contents

1. [Real-time Data Updates](#real-time-data-updates)
2. [Custom Interactions](#custom-interactions)
3. [Advanced Styling](#advanced-styling)
4. [Multi-Chart Coordination](#multi-chart-coordination)
5. [Custom Components](#custom-components)
6. [Performance Optimization](#performance-optimization)
7. [Geographic Visualizations](#geographic-visualizations)
8. [Animation and Transitions](#animation-and-transitions)

## Real-time Data Updates

### Live Data Streaming

```javascript
class LiveChart {
  constructor(containerId) {
    this.chart = echarts.init(document.getElementById(containerId));
    this.data = [];
    this.maxDataPoints = 50;
    
    this.initChart();
    this.startDataStream();
  }
  
  initChart() {
    const option = {
      title: {
        text: 'Real-time Data Stream',
        left: 'center'
      },
      tooltip: {
        trigger: 'axis',
        formatter: function(params) {
          const date = new Date(params[0].name);
          return `${date.toLocaleTimeString()}<br/>${params[0].seriesName}: ${params[0].value[1]}`;
        }
      },
      xAxis: {
        type: 'time',
        splitLine: {
          show: false
        },
        axisLabel: {
          formatter: function(value) {
            return new Date(value).toLocaleTimeString();
          }
        }
      },
      yAxis: {
        type: 'value',
        boundaryGap: [0, '100%'],
        splitLine: {
          show: false
        }
      },
      series: [{
        name: 'Value',
        type: 'line',
        showSymbol: false,
        data: this.data,
        smooth: true,
        lineStyle: {
          color: '#5470c6',
          width: 2
        },
        areaStyle: {
          color: new echarts.graphic.LinearGradient(0, 0, 0, 1, [
            { offset: 0, color: 'rgba(84, 112, 198, 0.3)' },
            { offset: 1, color: 'rgba(84, 112, 198, 0.1)' }
          ])
        }
      }]
    };
    
    this.chart.setOption(option);
  }
  
  startDataStream() {
    setInterval(() => {
      const now = new Date();
      const value = Math.random() * 100;
      
      this.data.push([now, value]);
      
      // Keep only recent data points
      if (this.data.length > this.maxDataPoints) {
        this.data.shift();
      }
      
      // Update chart
      this.chart.setOption({
        series: [{
          data: this.data
        }]
      });
    }, 1000);
  }
}

// Usage
const liveChart = new LiveChart('live-chart-container');
```

### Dynamic Data Filtering

```javascript
class DynamicFilterChart {
  constructor(containerId, rawData) {
    this.chart = echarts.init(document.getElementById(containerId));
    this.rawData = rawData;
    this.currentFilter = 'all';
    
    this.initChart();
    this.setupFilters();
  }
  
  initChart() {
    const option = {
      title: {
        text: 'Dynamic Data Filtering',
        left: 'center'
      },
      toolbox: {
        feature: {
          myFilter: {
            show: true,
            title: 'Filter Data',
            icon: 'path://M12,2A10,10 0 0,0 2,12A10,10 0 0,0 12,22A10,10 0 0,0 22,12A10,10 0 0,0 12,2Z',
            onclick: () => this.showFilterDialog()
          }
        }
      },
      tooltip: {
        trigger: 'axis',
        axisPointer: {
          type: 'cross'
        }
      },
      legend: {
        data: ['Category A', 'Category B', 'Category C']
      },
      xAxis: {
        type: 'category',
        data: this.rawData.categories
      },
      yAxis: {
        type: 'value'
      },
      series: this.getFilteredSeries()
    };
    
    this.chart.setOption(option);
  }
  
  getFilteredSeries() {
    const series = [];
    
    ['A', 'B', 'C'].forEach(category => {
      if (this.currentFilter === 'all' || this.currentFilter === category) {
        series.push({
          name: `Category ${category}`,
          type: 'bar',
          data: this.rawData[`category${category}`],
          animationDelay: (idx) => idx * 100
        });
      }
    });
    
    return series;
  }
  
  applyFilter(filter) {
    this.currentFilter = filter;
    
    this.chart.setOption({
      series: this.getFilteredSeries()
    });
  }
  
  showFilterDialog() {
    // Custom filter dialog implementation
    const filters = ['all', 'A', 'B', 'C'];
    const selectedFilter = prompt('Select filter: ' + filters.join(', '), this.currentFilter);
    
    if (selectedFilter && filters.includes(selectedFilter)) {
      this.applyFilter(selectedFilter);
    }
  }
}

// Usage
const filterChart = new DynamicFilterChart('filter-chart', {
  categories: ['Jan', 'Feb', 'Mar', 'Apr', 'May'],
  categoryA: [20, 25, 30, 35, 40],
  categoryB: [15, 20, 25, 30, 35],
  categoryC: [10, 15, 20, 25, 30]
});
```

## Custom Interactions

### Drill-down Charts

```javascript
class DrilldownChart {
  constructor(containerId) {
    this.chart = echarts.init(document.getElementById(containerId));
    this.breadcrumb = [];
    this.data = this.generateHierarchicalData();
    
    this.initChart();
    this.setupInteractions();
  }
  
  generateHierarchicalData() {
    return {
      root: {
        name: 'Total Sales',
        children: {
          'North America': {
            name: 'North America',
            value: 1000,
            children: {
              'USA': { name: 'USA', value: 800 },
              'Canada': { name: 'Canada', value: 200 }
            }
          },
          'Europe': {
            name: 'Europe',
            value: 800,
            children: {
              'Germany': { name: 'Germany', value: 300 },
              'France': { name: 'France', value: 250 },
              'UK': { name: 'UK', value: 250 }
            }
          },
          'Asia': {
            name: 'Asia',
            value: 600,
            children: {
              'China': { name: 'China', value: 300 },
              'Japan': { name: 'Japan', value: 200 },
              'India': { name: 'India', value: 100 }
            }
          }
        }
      }
    };
  }
  
  initChart() {
    this.updateChart('root');
  }
  
  updateChart(level) {
    const currentData = this.getCurrentData(level);
    const canDrillDown = this.canDrillDown(level);
    
    const option = {
      title: {
        text: this.getBreadcrumbText(),
        left: 'center',
        subtext: canDrillDown ? 'Click to drill down' : 'Lowest level'
      },
      tooltip: {
        trigger: 'item',
        formatter: '{a} <br/>{b}: {c} ({d}%)'
      },
      series: [{
        name: 'Sales',
        type: 'pie',
        radius: '50%',
        data: currentData,
        emphasis: {
          itemStyle: {
            shadowBlur: 10,
            shadowOffsetX: 0,
            shadowColor: 'rgba(0, 0, 0, 0.5)'
          }
        },
        label: {
          formatter: canDrillDown ? '{b}\n{c}\n(Click to expand)' : '{b}\n{c}'
        }
      }]
    };
    
    this.chart.setOption(option, true);
  }
  
  getCurrentData(level) {
    const path = level === 'root' ? [] : level.split('.');
    let currentLevel = this.data.root;
    
    for (const segment of path) {
      currentLevel = currentLevel.children[segment];
    }
    
    if (currentLevel.children) {
      return Object.keys(currentLevel.children).map(key => ({
        name: key,
        value: currentLevel.children[key].value,
        drilldownKey: level === 'root' ? key : `${level}.${key}`
      }));
    }
    
    return [{ name: currentLevel.name, value: currentLevel.value }];
  }
  
  canDrillDown(level) {
    const path = level === 'root' ? [] : level.split('.');
    let currentLevel = this.data.root;
    
    for (const segment of path) {
      currentLevel = currentLevel.children[segment];
    }
    
    return !!currentLevel.children;
  }
  
  getBreadcrumbText() {
    if (this.breadcrumb.length === 0) return 'Total Sales';
    return 'Sales > ' + this.breadcrumb.join(' > ');
  }
  
  setupInteractions() {
    this.chart.on('click', (params) => {
      if (params.data.drilldownKey && this.canDrillDown(params.data.drilldownKey)) {
        this.breadcrumb.push(params.data.name);
        this.updateChart(params.data.drilldownKey);
      }
    });
    
    // Add back navigation
    this.chart.on('dblclick', () => {
      if (this.breadcrumb.length > 0) {
        this.breadcrumb.pop();
        const level = this.breadcrumb.length === 0 ? 'root' : this.breadcrumb.join('.');
        this.updateChart(level);
      }
    });
  }
}

// Usage
const drilldownChart = new DrilldownChart('drilldown-chart');
```

### Linked Charts with Brush Selection

```javascript
class LinkedChartsWithBrush {
  constructor(container1Id, container2Id) {
    this.chart1 = echarts.init(document.getElementById(container1Id));
    this.chart2 = echarts.init(document.getElementById(container2Id));
    this.rawData = this.generateData();
    
    this.initCharts();
    this.setupBrushLinking();
  }
  
  generateData() {
    const data = [];
    for (let i = 0; i < 100; i++) {
      data.push([
        Math.random() * 100,
        Math.random() * 100,
        Math.random() * 50 + 10
      ]);
    }
    return data;
  }
  
  initCharts() {
    // Scatter plot
    const scatterOption = {
      title: { text: 'Scatter Plot', left: 'center' },
      brush: {
        xAxisIndex: 0,
        brushStyle: {
          borderWidth: 1,
          color: 'rgba(120,140,180,0.3)',
          borderColor: 'rgba(120,140,180,0.8)'
        }
      },
      tooltip: {
        trigger: 'item',
        formatter: 'X: {c0}<br/>Y: {c1}<br/>Size: {c2}'
      },
      xAxis: { type: 'value', scale: true },
      yAxis: { type: 'value', scale: true },
      series: [{
        type: 'scatter',
        symbolSize: (data) => data[2],
        data: this.rawData,
        itemStyle: {
          color: '#5470c6',
          opacity: 0.7
        }
      }]
    };
    
    // Bar chart
    const barOption = {
      title: { text: 'Distribution', left: 'center' },
      tooltip: { trigger: 'axis' },
      xAxis: {
        type: 'category',
        data: this.getDistributionCategories()
      },
      yAxis: { type: 'value' },
      series: [{
        type: 'bar',
        data: this.getDistributionData(),
        itemStyle: {
          color: '#91cc75'
        }
      }]
    };
    
    this.chart1.setOption(scatterOption);
    this.chart2.setOption(barOption);
  }
  
  getDistributionCategories() {
    return ['0-20', '20-40', '40-60', '60-80', '80-100'];
  }
  
  getDistributionData(filteredData = null) {
    const data = filteredData || this.rawData;
    const bins = [0, 0, 0, 0, 0];
    
    data.forEach(point => {
      const value = point[0]; // Use X value for distribution
      const binIndex = Math.min(Math.floor(value / 20), 4);
      bins[binIndex]++;
    });
    
    return bins;
  }
  
  setupBrushLinking() {
    this.chart1.on('brushSelected', (params) => {
      const brushComponent = params.batch[0];
      
      if (brushComponent && brushComponent.selected.length > 0) {
        const selectedIndices = brushComponent.selected[0].dataIndex;
        const selectedData = selectedIndices.map(index => this.rawData[index]);
        
        // Update bar chart with selected data
        this.chart2.setOption({
          series: [{
            data: this.getDistributionData(selectedData),
            itemStyle: {
              color: '#ee6666' // Highlight selected data
            }
          }]
        });
      } else {
        // Reset bar chart when brush is cleared
        this.chart2.setOption({
          series: [{
            data: this.getDistributionData(),
            itemStyle: {
              color: '#91cc75'
            }
          }]
        });
      }
    });
  }
}

// Usage
const linkedCharts = new LinkedChartsWithBrush('scatter-chart', 'bar-chart');
```

## Advanced Styling

### Custom Themes and Dark Mode

```javascript
// Custom theme definition
const customTheme = {
  color: [
    '#ff6b6b', '#4ecdc4', '#45b7d1', '#96ceb4', '#feca57',
    '#ff9ff3', '#54a0ff', '#5f27cd', '#00d2d3', '#ff9f43'
  ],
  backgroundColor: '#1a1a1a',
  textStyle: {
    color: '#ffffff'
  },
  title: {
    textStyle: {
      color: '#ffffff'
    },
    subtextStyle: {
      color: '#cccccc'
    }
  },
  line: {
    itemStyle: {
      borderWidth: 2
    },
    lineStyle: {
      width: 3
    },
    symbolSize: 8,
    symbol: 'circle',
    smooth: true
  },
  radar: {
    itemStyle: {
      borderWidth: 2
    },
    lineStyle: {
      width: 3
    },
    symbolSize: 8,
    symbol: 'circle',
    smooth: true
  },
  bar: {
    itemStyle: {
      barBorderWidth: 0,
      barBorderColor: '#ccc'
    }
  },
  pie: {
    itemStyle: {
      borderWidth: 0,
      borderColor: '#ccc'
    }
  },
  scatter: {
    itemStyle: {
      borderWidth: 0,
      borderColor: '#ccc'
    }
  },
  boxplot: {
    itemStyle: {
      borderWidth: 0,
      borderColor: '#ccc'
    }
  },
  parallel: {
    itemStyle: {
      borderWidth: 0,
      borderColor: '#ccc'
    }
  },
  sankey: {
    itemStyle: {
      borderWidth: 0,
      borderColor: '#ccc'
    }
  },
  funnel: {
    itemStyle: {
      borderWidth: 0,
      borderColor: '#ccc'
    }
  },
  gauge: {
    itemStyle: {
      borderWidth: 0,
      borderColor: '#ccc'
    }
  },
  candlestick: {
    itemStyle: {
      color: '#e6a0d2',
      color0: '#64b5f6',
      borderColor: '#e6a0d2',
      borderColor0: '#64b5f6',
      borderWidth: 2
    }
  },
  graph: {
    itemStyle: {
      borderWidth: 0,
      borderColor: '#ccc'
    },
    lineStyle: {
      width: '1',
      color: '#cccccc'
    },
    symbolSize: 8,
    symbol: 'circle',
    smooth: true,
    color: [
      '#ff6b6b', '#4ecdc4', '#45b7d1', '#96ceb4', '#feca57'
    ],
    label: {
      color: '#ffffff'
    }
  },
  categoryAxis: {
    axisLine: {
      show: true,
      lineStyle: {
        color: '#666666'
      }
    },
    axisTick: {
      show: false,
      lineStyle: {
        color: '#333'
      }
    },
    axisLabel: {
      show: true,
      color: '#cccccc'
    },
    splitLine: {
      show: true,
      lineStyle: {
        color: ['#333333']
      }
    },
    splitArea: {
      show: false,
      areaStyle: {
        color: ['rgba(250,250,250,0.05)', 'rgba(200,200,200,0.02)']
      }
    }
  },
  valueAxis: {
    axisLine: {
      show: true,
      lineStyle: {
        color: '#666666'
      }
    },
    axisTick: {
      show: false,
      lineStyle: {
        color: '#333'
      }
    },
    axisLabel: {
      show: true,
      color: '#cccccc'
    },
    splitLine: {
      show: true,
      lineStyle: {
        color: ['#333333']
      }
    },
    splitArea: {
      show: false,
      areaStyle: {
        color: ['rgba(250,250,250,0.05)', 'rgba(200,200,200,0.02)']
      }
    }
  },
  logAxis: {
    axisLine: {
      show: true,
      lineStyle: {
        color: '#666666'
      }
    },
    axisTick: {
      show: false,
      lineStyle: {
        color: '#333'
      }
    },
    axisLabel: {
      show: true,
      color: '#cccccc'
    },
    splitLine: {
      show: true,
      lineStyle: {
        color: ['#333333']
      }
    },
    splitArea: {
      show: false,
      areaStyle: {
        color: ['rgba(250,250,250,0.05)', 'rgba(200,200,200,0.02)']
      }
    }
  },
  timeAxis: {
    axisLine: {
      show: true,
      lineStyle: {
        color: '#666666'
      }
    },
    axisTick: {
      show: false,
      lineStyle: {
        color: '#333'
      }
    },
    axisLabel: {
      show: true,
      color: '#cccccc'
    },
    splitLine: {
      show: true,
      lineStyle: {
        color: ['#333333']
      }
    },
    splitArea: {
      show: false,
      areaStyle: {
        color: ['rgba(250,250,250,0.05)', 'rgba(200,200,200,0.02)']
      }
    }
  },
  toolbox: {
    iconStyle: {
      borderColor: '#cccccc'
    },
    emphasis: {
      iconStyle: {
        borderColor: '#ffffff'
      }
    }
  },
  legend: {
    textStyle: {
      color: '#cccccc'
    }
  },
  tooltip: {
    axisPointer: {
      lineStyle: {
        color: '#cccccc',
        width: 1
      },
      crossStyle: {
        color: '#cccccc',
        width: 1
      }
    }
  },
  timeline: {
    lineStyle: {
      color: '#666666',
      width: 1
    },
    itemStyle: {
      color: '#666666',
      borderWidth: 1
    },
    controlStyle: {
      color: '#666666',
      borderColor: '#666666',
      borderWidth: 0.5
    },
    checkpointStyle: {
      color: '#ff6b6b',
      borderColor: '#ff6b6b'
    },
    label: {
      color: '#cccccc'
    },
    emphasis: {
      itemStyle: {
        color: '#ffffff'
      },
      controlStyle: {
        color: '#666666',
        borderColor: '#666666',
        borderWidth: 0.5
      },
      label: {
        color: '#cccccc'
      }
    }
  },
  visualMap: {
    color: ['#ff6b6b', '#feca57', '#4ecdc4']
  },
  dataZoom: {
    backgroundColor: 'rgba(255,255,255,0)',
    dataBackgroundColor: 'rgba(114,204,255,1)',
    fillerColor: 'rgba(114,204,255,0.2)',
    handleColor: '#cccccc',
    handleSize: '100%',
    textStyle: {
      color: '#cccccc'
    }
  },
  markPoint: {
    label: {
      color: '#ffffff'
    },
    emphasis: {
      label: {
        color: '#ffffff'
      }
    }
  }
};

// Register the custom theme
echarts.registerTheme('customDark', customTheme);

// Theme switcher class
class ThemeSwitcher {
  constructor(containerId) {
    this.containerId = containerId;
    this.currentTheme = 'light';
    this.chart = null;
    
    this.initChart();
    this.createThemeToggle();
  }
  
  initChart() {
    if (this.chart) {
      this.chart.dispose();
    }
    
    this.chart = echarts.init(
      document.getElementById(this.containerId),
      this.currentTheme === 'dark' ? 'customDark' : null
    );
    
    const option = {
      title: {
        text: 'Theme Switcher Demo',
        left: 'center'
      },
      tooltip: {
        trigger: 'axis',
        axisPointer: {
          type: 'cross'
        }
      },
      legend: {
        data: ['Line 1', 'Line 2', 'Line 3']
      },
      toolbox: {
        feature: {
          saveAsImage: {}
        }
      },
      grid: {
        left: '3%',
        right: '4%',
        bottom: '3%',
        containLabel: true
      },
      xAxis: {
        type: 'category',
        boundaryGap: false,
        data: ['Mon', 'Tue', 'Wed', 'Thu', 'Fri', 'Sat', 'Sun']
      },
      yAxis: {
        type: 'value'
      },
      series: [
        {
          name: 'Line 1',
          type: 'line',
          stack: 'Total',
          data: [120, 132, 101, 134, 90, 230, 210]
        },
        {
          name: 'Line 2',
          type: 'line',
          stack: 'Total',
          data: [220, 182, 191, 234, 290, 330, 310]
        },
        {
          name: 'Line 3',
          type: 'line',
          stack: 'Total',
          data: [150, 232, 201, 154, 190, 330, 410]
        }
      ]
    };
    
    this.chart.setOption(option);
  }
  
  createThemeToggle() {
    const toggle = document.createElement('button');
    toggle.textContent = 'Switch to Dark Theme';
    toggle.style.marginBottom = '10px';
    
    toggle.onclick = () => {
      this.currentTheme = this.currentTheme === 'light' ? 'dark' : 'light';
      toggle.textContent = `Switch to ${this.currentTheme === 'light' ? 'Dark' : 'Light'} Theme`;
      this.initChart();
    };
    
    const container = document.getElementById(this.containerId);
    container.parentNode.insertBefore(toggle, container);
  }
}

// Usage
const themeSwitcher = new ThemeSwitcher('theme-chart');
```

## Performance Optimization

### Large Dataset Handling

```javascript
class LargeDatasetChart {
  constructor(containerId) {
    this.chart = echarts.init(document.getElementById(containerId));
    this.initChart();
  }
  
  generateLargeDataset(size = 100000) {
    const data = [];
    for (let i = 0; i < size; i++) {
      data.push([
        Math.random() * 1000,
        Math.random() * 1000,
        Math.random() * 50 + 10
      ]);
    }
    return data;
  }
  
  initChart() {
    const largeData = this.generateLargeDataset();
    
    const option = {
      title: {
        text: `Large Dataset (${largeData.length.toLocaleString()} points)`,
        left: 'center'
      },
      tooltip: {
        trigger: 'item',
        formatter: function(params) {
          return `Point ${params.dataIndex}<br/>X: ${params.value[0].toFixed(2)}<br/>Y: ${params.value[1].toFixed(2)}`;
        }
      },
      toolbox: {
        feature: {
          dataZoom: {
            yAxisIndex: false
          },
          brush: {
            type: ['rect', 'polygon', 'clear']
          }
        }
      },
      brush: {
        toolbox: ['rect', 'polygon', 'keep', 'clear'],
        xAxisIndex: 0
      },
      xAxis: {
        type: 'value',
        scale: true
      },
      yAxis: {
        type: 'value',
        scale: true
      },
      series: [{
        type: 'scatter',
        symbolSize: 3,
        data: largeData,
        large: true, // Enable large dataset optimization
        largeThreshold: 5000, // Threshold for large dataset
        progressive: 5000, // Progressive rendering
        progressiveThreshold: 10000,
        itemStyle: {
          opacity: 0.6,
          color: function(params) {
            // Color based on data value
            const value = params.value[2];
            return echarts.color.lift('#5470c6', (value - 10) / 40);
          }
        },
        emphasis: {
          itemStyle: {
            opacity: 1,
            shadowBlur: 10,
            shadowColor: 'rgba(0, 0, 0, 0.5)'
          }
        }
      }]
    };
    
    this.chart.setOption(option);
    
    // Handle brush selection for large datasets
    this.chart.on('brushSelected', (params) => {
      const brushComponent = params.batch[0];
      if (brushComponent && brushComponent.selected.length > 0) {
        const selectedCount = brushComponent.selected[0].dataIndex.length;
        console.log(`Selected ${selectedCount} points`);
      }
    });
  }
}

// Usage
const largeDataChart = new LargeDatasetChart('large-data-chart');
```

### Virtual Scrolling for Time Series

```javascript
class VirtualScrollingChart {
  constructor(containerId) {
    this.chart = echarts.init(document.getElementById(containerId));
    this.allData = this.generateTimeSeriesData();
    this.viewportSize = 1000; // Number of points to show at once
    this.currentStart = 0;
    
    this.initChart();
    this.setupVirtualScrolling();
  }
  
  generateTimeSeriesData() {
    const data = [];
    const startTime = new Date('2023-01-01').getTime();
    const interval = 60000; // 1 minute intervals
    
    for (let i = 0; i < 50000; i++) {
      const time = startTime + i * interval;
      const value = Math.sin(i * 0.01) * 50 + Math.random() * 20 + 100;
      data.push([time, value]);
    }
    
    return data;
  }
  
  initChart() {
    const option = {
      title: {
        text: 'Virtual Scrolling Time Series',
        subtext: `Showing ${this.viewportSize} of ${this.allData.length.toLocaleString()} points`
      },
      tooltip: {
        trigger: 'axis',
        formatter: function(params) {
          const date = new Date(params[0].value[0]);
          return `${date.toLocaleString()}<br/>Value: ${params[0].value[1].toFixed(2)}`;
        }
      },
      dataZoom: [{
        type: 'inside',
        start: 0,
        end: 100,
        filterMode: 'none'
      }, {
        start: 0,
        end: 100,
        handleIcon: 'M10.7,11.9v-1.3H9.3v1.3c-4.9,0.3-8.8,4.4-8.8,9.4c0,5,3.9,9.1,8.8,9.4v1.3h1.3v-1.3c4.9-0.3,8.8-4.4,8.8-9.4C19.5,16.3,15.6,12.2,10.7,11.9z',
        handleSize: '80%',
        handleStyle: {
          color: '#fff',
          shadowBlur: 3,
          shadowColor: 'rgba(0, 0, 0, 0.6)',
          shadowOffsetX: 2,
          shadowOffsetY: 2
        }
      }],
      xAxis: {
        type: 'time',
        axisLabel: {
          formatter: function(value) {
            return echarts.format.formatTime('MM-dd hh:mm', value);
          }
        }
      },
      yAxis: {
        type: 'value'
      },
      series: [{
        type: 'line',
        showSymbol: false,
        data: this.getViewportData(),
        lineStyle: {
          color: '#5470c6',
          width: 1
        },
        areaStyle: {
          color: new echarts.graphic.LinearGradient(0, 0, 0, 1, [
            { offset: 0, color: 'rgba(84, 112, 198, 0.3)' },
            { offset: 1, color: 'rgba(84, 112, 198, 0.1)' }
          ])
        }
      }]
    };
    
    this.chart.setOption(option);
  }
  
  getViewportData() {
    return this.allData.slice(this.currentStart, this.currentStart + this.viewportSize);
  }
  
  setupVirtualScrolling() {
    this.chart.on('dataZoom', (params) => {
      const startPercent = params.start;
      const endPercent = params.end;
      
      const totalPoints = this.allData.length;
      const newStart = Math.floor((startPercent / 100) * totalPoints);
      const newEnd = Math.floor((endPercent / 100) * totalPoints);
      
      // Only update if we need to load new data
      if (Math.abs(newStart - this.currentStart) > this.viewportSize * 0.1) {
        this.currentStart = Math.max(0, newStart - this.viewportSize * 0.2);
        
        this.chart.setOption({
          series: [{
            data: this.getViewportData()
          }]
        });
      }
    });
  }
}

// Usage
const virtualScrollChart = new VirtualScrollingChart('virtual-scroll-chart');
```

This advanced examples documentation demonstrates sophisticated ECharts usage patterns including real-time data updates, custom interactions, advanced styling, performance optimization techniques, and complex visualizations. These examples provide a solid foundation for building professional-grade data visualization applications.