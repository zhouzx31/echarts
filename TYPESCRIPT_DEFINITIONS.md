# ECharts TypeScript Definitions Reference

This document provides detailed TypeScript interface definitions for ECharts options and APIs.

## Core Interfaces

### EChartsOption

The main configuration interface for ECharts.

```typescript
interface EChartsOption extends ECBasicOption {
  // Dataset
  dataset?: DatasetComponentOption | DatasetComponentOption[];
  
  // Accessibility
  aria?: AriaComponentOption;
  
  // Title
  title?: TitleComponentOption | TitleComponentOption[];
  
  // Coordinate Systems
  grid?: GridComponentOption | GridComponentOption[];
  radar?: RadarComponentOption | RadarComponentOption[];
  polar?: PolarComponentOption | PolarComponentOption[];
  geo?: GeoComponentOption | GeoComponentOption[];
  parallel?: ParallelComponentOption | ParallelComponentOption[];
  calendar?: CalendarComponentOption | CalendarComponentOption[];
  
  // Axes
  xAxis?: XAXisComponentOption | XAXisComponentOption[];
  yAxis?: YAXisComponentOption | YAXisComponentOption[];
  angleAxis?: AngleAxisComponentOption | AngleAxisComponentOption[];
  radiusAxis?: RadiusAxisComponentOption | RadiusAxisComponentOption[];
  singleAxis?: SingleAxisComponentOption | SingleAxisComponentOption[];
  parallelAxis?: ParallelAxisComponentOption | ParallelAxisComponentOption[];
  
  // Interactive Components
  tooltip?: TooltipComponentOption | TooltipComponentOption[];
  axisPointer?: AxisPointerComponentOption | AxisPointerComponentOption[];
  brush?: BrushComponentOption | BrushComponentOption[];
  toolbox?: ToolboxComponentOption | ToolboxComponentOption[];
  
  // Visual Components
  legend?: LegendComponentOption | LegendComponentOption[];
  dataZoom?: DataZoomComponentOption | DataZoomComponentOption[];
  visualMap?: VisualMapComponentOption | VisualMapComponentOption[];
  graphic?: GraphicComponentOption | GraphicComponentOption[];
  
  // Timeline
  timeline?: TimelineComponentOption | TimelineSliderComponentOption;
  
  // Series
  series?: SeriesOption | SeriesOption[];
  
  // Multi-option support
  options?: EChartsOption[];
  baseOption?: EChartsOption;
}
```

## Series Options

### Base Series Interface

```typescript
interface SeriesOption {
  type: string;
  id?: OptionId;
  name?: string;
  colorBy?: 'series' | 'data';
  legendHoverLink?: boolean;
  hoverLayerThreshold?: number;
  seriesLayoutBy?: 'column' | 'row';
  labelLine?: LabelLineOption;
  labelLayout?: LabelLayoutOption | LabelLayoutOptionCallback;
  emphasis?: {
    focus?: 'none' | 'self' | 'series' | 'adjacency';
    blurScope?: 'coordinateSystem' | 'series' | 'global';
    disabled?: boolean;
  };
  blur?: object;
  select?: object;
  selectedMode?: boolean | 'single' | 'multiple';
  animation?: boolean;
  animationThreshold?: number;
  animationDuration?: number | AnimationDurationCallback;
  animationEasing?: AnimationEasing;
  animationDelay?: number | AnimationDelayCallback;
  animationDurationUpdate?: number | AnimationDurationCallback;
  animationEasingUpdate?: AnimationEasing;
  animationDelayUpdate?: number | AnimationDelayCallback;
  tooltip?: SeriesTooltipOption;
}
```

### Line Series Options

```typescript
interface LineSeriesOption extends SeriesOption {
  type: 'line';
  coordinateSystem?: 'cartesian2d' | 'polar';
  xAxisIndex?: number;
  yAxisIndex?: number;
  polarIndex?: number;
  symbol?: string | (value: any, params: any) => string;
  symbolSize?: number | number[] | (value: any, params: any) => number | number[];
  symbolRotate?: number | (value: any, params: any) => number;
  symbolKeepAspect?: boolean;
  symbolOffset?: string | number | (string | number)[];
  showSymbol?: boolean;
  showAllSymbol?: boolean | 'auto';
  hoverAnimation?: boolean;
  legendHoverLink?: boolean;
  stack?: string;
  cursor?: string;
  connectNulls?: boolean;
  clipOverflow?: boolean;
  step?: boolean | 'start' | 'middle' | 'end';
  label?: SeriesLabelOption;
  endLabel?: LineEndLabelOption;
  labelLine?: LabelLineOption;
  labelLayout?: LabelLayoutOption | LabelLayoutOptionCallback;
  itemStyle?: ItemStyleOption;
  lineStyle?: LineStyleOption;
  areaStyle?: AreaStyleOption;
  emphasis?: {
    focus?: DefaultEmphasisFocus;
    scale?: boolean;
    label?: SeriesLabelOption;
    endLabel?: LineEndLabelOption;
    itemStyle?: ItemStyleOption;
    lineStyle?: LineStyleOption;
    areaStyle?: AreaStyleOption;
  };
  blur?: {
    label?: SeriesLabelOption;
    endLabel?: LineEndLabelOption;
    itemStyle?: ItemStyleOption;
    lineStyle?: LineStyleOption;
    areaStyle?: AreaStyleOption;
  };
  select?: {
    label?: SeriesLabelOption;
    endLabel?: LineEndLabelOption;
    itemStyle?: ItemStyleOption;
    lineStyle?: LineStyleOption;
    areaStyle?: AreaStyleOption;
  };
  smooth?: boolean | number;
  smoothMonotone?: 'x' | 'y' | 'none';
  sampling?: 'lttb' | 'average' | 'max' | 'min' | 'sum';
  dimensions?: DimensionDefinitionLoose[];
  encode?: OptionEncode;
  seriesLayoutBy?: 'column' | 'row';
  datasetIndex?: number;
  data?: (LineDataItemOption | OptionDataValueNumeric | OptionDataValueNumeric[])[];
  markPoint?: MarkPointOption;
  markLine?: MarkLineOption;
  markArea?: MarkAreaOption;
  zlevel?: number;
  z?: number;
  silent?: boolean;
  universalTransition?: boolean | { enabled?: boolean; };
}
```

### Bar Series Options

```typescript
interface BarSeriesOption extends SeriesOption {
  type: 'bar';
  coordinateSystem?: 'cartesian2d' | 'polar';
  xAxisIndex?: number;
  yAxisIndex?: number;
  polarIndex?: number;
  roundCap?: boolean;
  realtimeSort?: boolean;
  showBackground?: boolean;
  backgroundStyle?: ItemStyleOption;
  label?: BarSeriesLabelOption;
  labelLine?: LabelLineOption;
  itemStyle?: BarItemStyleOption;
  emphasis?: {
    focus?: DefaultEmphasisFocus;
    label?: BarSeriesLabelOption;
    itemStyle?: BarItemStyleOption;
  };
  blur?: {
    label?: BarSeriesLabelOption;
    itemStyle?: BarItemStyleOption;
  };
  select?: {
    label?: BarSeriesLabelOption;
    itemStyle?: BarItemStyleOption;
  };
  stack?: string;
  cursor?: string;
  barWidth?: number | string;
  barMaxWidth?: number | string;
  barMinWidth?: number | string;
  barMinHeight?: number;
  barGap?: number | string;
  barCategoryGap?: number | string;
  large?: boolean;
  largeThreshold?: number;
  progressive?: number;
  progressiveThreshold?: number;
  progressiveChunkMode?: 'mod';
  dimensions?: DimensionDefinitionLoose[];
  encode?: OptionEncode;
  seriesLayoutBy?: 'column' | 'row';
  datasetIndex?: number;
  data?: (BarDataItemOption | OptionDataValueNumeric | OptionDataValueNumeric[])[];
  markPoint?: MarkPointOption;
  markLine?: MarkLineOption;
  markArea?: MarkAreaOption;
  clip?: boolean;
  zlevel?: number;
  z?: number;
  silent?: boolean;
  universalTransition?: boolean | { enabled?: boolean; };
}
```

### Pie Series Options

```typescript
interface PieSeriesOption extends SeriesOption {
  type: 'pie';
  legendHoverLink?: boolean;
  hoverAnimation?: boolean;
  hoverOffset?: number;
  selectedMode?: boolean | 'single' | 'multiple';
  selectedOffset?: number;
  clockwise?: boolean;
  startAngle?: number;
  minAngle?: number;
  minShowLabelAngle?: number;
  roseType?: boolean | 'radius' | 'area';
  avoidLabelOverlap?: boolean;
  stillShowZeroSum?: boolean;
  cursor?: string;
  label?: PieLabelOption;
  labelLine?: PieLabelLineOption;
  labelLayout?: LabelLayoutOption | LabelLayoutOptionCallback;
  itemStyle?: PieItemStyleOption;
  emphasis?: {
    focus?: DefaultEmphasisFocus;
    scale?: boolean;
    scaleSize?: number;
    label?: PieLabelOption;
    labelLine?: PieLabelLineOption;
    itemStyle?: PieItemStyleOption;
  };
  blur?: {
    label?: PieLabelOption;
    labelLine?: PieLabelLineOption;
    itemStyle?: PieItemStyleOption;
  };
  select?: {
    label?: PieLabelOption;
    labelLine?: PieLabelLineOption;
    itemStyle?: PieItemStyleOption;
  };
  center?: (number | string)[];
  radius?: (number | string) | (number | string)[];
  seriesLayoutBy?: 'column' | 'row';
  datasetIndex?: number;
  dimensions?: DimensionDefinitionLoose[];
  encode?: OptionEncode;
  data?: (PieDataItemOption | OptionDataValueNumeric)[];
  markPoint?: MarkPointOption;
  markLine?: MarkLineOption;
  markArea?: MarkAreaOption;
  zlevel?: number;
  z?: number;
  silent?: boolean;
  animationType?: 'expansion' | 'scale';
  animationTypeUpdate?: 'transition' | 'expansion';
  universalTransition?: boolean | { enabled?: boolean; };
}
```

## Component Options

### Grid Options

```typescript
interface GridOption extends ComponentOption {
  mainType?: 'grid';
  show?: boolean;
  zlevel?: number;
  z?: number;
  left?: number | string;
  top?: number | string;
  right?: number | string;
  bottom?: number | string;
  width?: number | string;
  height?: number | string;
  containLabel?: boolean;
  backgroundColor?: ColorString;
  borderColor?: ColorString;
  borderWidth?: number;
  shadowBlur?: number;
  shadowColor?: ColorString;
  shadowOffsetX?: number;
  shadowOffsetY?: number;
  tooltip?: ComponentTooltipOption;
}
```

### Axis Options

```typescript
interface XAxisOption extends AxisBaseOption {
  mainType?: 'xAxis';
  position?: 'top' | 'bottom';
  gridIndex?: number;
  offset?: number;
}

interface YAxisOption extends AxisBaseOption {
  mainType?: 'yAxis';
  position?: 'left' | 'right';
  gridIndex?: number;
  offset?: number;
}

interface AxisBaseOption extends ComponentOption {
  type?: 'value' | 'category' | 'time' | 'log';
  id?: OptionId;
  show?: boolean;
  gridIndex?: number;
  alignTicks?: boolean;
  position?: string;
  offset?: number;
  categorySortInfo?: OrdinalSortInfo;
  name?: string;
  nameLocation?: 'start' | 'middle' | 'end';
  nameTextStyle?: AxisNameTextStyleOption;
  nameGap?: number;
  nameRotate?: number;
  inverse?: boolean;
  boundaryGap?: boolean | [number | string, number | string];
  min?: ScaleDataValue | 'dataMin' | ((extent: {min: number, max: number}) => ScaleDataValue);
  max?: ScaleDataValue | 'dataMax' | ((extent: {min: number, max: number}) => ScaleDataValue);
  scale?: boolean;
  splitNumber?: number;
  minInterval?: number;
  maxInterval?: number;
  interval?: number;
  logBase?: number;
  silent?: boolean;
  triggerEvent?: boolean;
  axisLine?: AxisLineOption;
  axisTick?: AxisTickOption;
  axisLabel?: AxisLabelOption;
  splitLine?: SplitLineOption;
  splitArea?: SplitAreaOption;
  data?: (OrdinalRawValue | AxisDataItemOption)[];
  axisPointer?: CommonAxisPointerOption;
  zlevel?: number;
  z?: number;
}
```

### Tooltip Options

```typescript
interface TooltipOption extends CommonTooltipOption<TopLevelFormatterParams>, ComponentOption {
  mainType?: 'tooltip';
  axisPointer?: AxisPointerOption & {
    axis?: 'auto' | 'x' | 'y' | 'angle' | 'radius';
    crossStyle?: LineStyleOption & {
      textStyle?: LabelOption;
    };
  };
  showContent?: boolean;
  trigger?: 'item' | 'axis' | 'none';
  displayMode?: 'single' | 'multipleByCoordSys';
  renderMode?: 'auto' | TooltipRenderMode;
  appendToBody?: boolean;
  className?: string;
  order?: TooltipOrderMode;
}

interface CommonTooltipOption<TFormatterParams> {
  show?: boolean;
  showContent?: boolean;
  z?: number;
  zlevel?: number;
  trigger?: 'item' | 'axis' | 'none';
  triggerOn?: 'mousemove' | 'click' | 'none' | 'mousemove|click';
  alwaysShowContent?: boolean;
  displayMode?: 'single' | 'multipleByCoordSys';
  renderMode?: 'auto' | 'html' | 'richText';
  confine?: boolean;
  showDelay?: number;
  hideDelay?: number;
  transitionDuration?: number;
  enterable?: boolean;
  backgroundColor?: ColorString;
  borderColor?: ColorString;
  borderRadius?: number;
  borderWidth?: number;
  shadowBlur?: number;
  shadowColor?: string;
  shadowOffsetX?: number;
  shadowOffsetY?: number;
  padding?: number | number[];
  extraCssText?: string;
  textStyle?: LabelOption;
  formatter?: string | TooltipFormatterCallback<TFormatterParams>;
  position?: TooltipPositionCallback | (number | string)[] | 'inside' | 'top' | 'left' | 'right' | 'bottom';
  valueFormatter?: (value: OptionDataValue | OptionDataValue[], dataIndex: number) => string;
  order?: TooltipOrderMode;
}
```

### Legend Options

```typescript
interface LegendOption extends ComponentOption, BoxLayoutOptionMixin, BorderOptionMixin {
  mainType?: 'legend';
  type?: 'plain' | 'scroll';
  id?: OptionId;
  show?: boolean;
  zlevel?: number;
  z?: number;
  left?: number | string;
  top?: number | string;
  right?: number | string;
  bottom?: number | string;
  width?: number | string;
  height?: number | string;
  orient?: LayoutOrient;
  align?: 'auto' | 'left' | 'right';
  padding?: number | number[];
  itemGap?: number;
  itemWidth?: number;
  itemHeight?: number;
  symbolKeepAspect?: boolean;
  formatter?: string | ((name: string) => string);
  textStyle?: LegendTextStyleOption;
  tooltip?: CommonTooltipOption<LegendTooltipFormatterParams>;
  icon?: string;
  data?: (string | LegendDataItemOption)[];
  backgroundColor?: ColorString;
  borderColor?: ColorString;
  borderWidth?: number;
  borderRadius?: number | number[];
  shadowBlur?: number;
  shadowColor?: ColorString;
  shadowOffsetX?: number;
  shadowOffsetY?: number;
  selected?: Dictionary<boolean>;
  selector?: (LegendSelectorButtonOption | string)[] | boolean;
  selectorLabel?: LegendSelectorButtonOption;
  emphasis?: {
    selectorLabel?: LegendSelectorButtonOption;
  };
  selectorPosition?: 'auto' | 'start' | 'end';
  selectorItemGap?: number;
  selectorButtonGap?: number;
  pageButtonItemGap?: number;
  pageButtonGap?: number | string;
  pageButtonPosition?: 'start' | 'end';
  pageFormatter?: string | ((current: number, total: number) => string);
  pageIcons?: {
    horizontal?: string[];
    vertical?: string[];
  };
  pageIconColor?: ColorString;
  pageIconInactiveColor?: ColorString;
  pageIconSize?: number | number[];
  pageTextStyle?: LabelOption;
  animationDurationUpdate?: number;
}
```

## Style and Formatting Interfaces

### Color Types

```typescript
type ColorString = string;
type ZRColor = ColorString | LinearGradientObject | RadialGradientObject | PatternObject;

interface LinearGradientObject {
  type: 'linear';
  x: number;
  y: number;
  x2: number;
  y2: number;
  colorStops: GradientColorStop[];
  global?: boolean;
}

interface RadialGradientObject {
  type: 'radial';
  x: number;
  y: number;
  r: number;
  colorStops: GradientColorStop[];
  global?: boolean;
}

interface GradientColorStop {
  offset: number;
  color: string;
}
```

### Style Options

```typescript
interface ItemStyleOption<TCbParams = never> {
  color?: ZRColor | (TCbParams extends never ? never : (params: TCbParams) => ZRColor);
  borderColor?: ZRColor | (TCbParams extends never ? never : (params: TCbParams) => ZRColor);
  borderWidth?: number | (TCbParams extends never ? never : (params: TCbParams) => number);
  borderType?: ZRLineType | (TCbParams extends never ? never : (params: TCbParams) => ZRLineType);
  borderDashOffset?: number | (TCbParams extends never ? never : (params: TCbParams) => number);
  borderCap?: CanvasLineCap | (TCbParams extends never ? never : (params: TCbParams) => CanvasLineCap);
  borderJoin?: CanvasLineJoin | (TCbParams extends never ? never : (params: TCbParams) => CanvasLineJoin);
  borderMiterLimit?: number | (TCbParams extends never ? never : (params: TCbParams) => number);
  shadowBlur?: number | (TCbParams extends never ? never : (params: TCbParams) => number);
  shadowColor?: ColorString | (TCbParams extends never ? never : (params: TCbParams) => ColorString);
  shadowOffsetX?: number | (TCbParams extends never ? never : (params: TCbParams) => number);
  shadowOffsetY?: number | (TCbParams extends never ? never : (params: TCbParams) => number);
  opacity?: number | (TCbParams extends never ? never : (params: TCbParams) => number);
}

interface LineStyleOption<TCbParams = never> {
  color?: ZRColor | (TCbParams extends never ? never : (params: TCbParams) => ZRColor);
  width?: number | (TCbParams extends never ? never : (params: TCbParams) => number);
  type?: ZRLineType | (TCbParams extends never ? never : (params: TCbParams) => ZRLineType);
  dashOffset?: number | (TCbParams extends never ? never : (params: TCbParams) => number);
  cap?: CanvasLineCap | (TCbParams extends never ? never : (params: TCbParams) => CanvasLineCap);
  join?: CanvasLineJoin | (TCbParams extends never ? never : (params: TCbParams) => CanvasLineJoin);
  miterLimit?: number | (TCbParams extends never ? never : (params: TCbParams) => number);
  shadowBlur?: number | (TCbParams extends never ? never : (params: TCbParams) => number);
  shadowColor?: ColorString | (TCbParams extends never ? never : (params: TCbParams) => ColorString);
  shadowOffsetX?: number | (TCbParams extends never ? never : (params: TCbParams) => number);
  shadowOffsetY?: number | (TCbParams extends never ? never : (params: TCbParams) => number);
  opacity?: number | (TCbParams extends never ? never : (params: TCbParams) => number);
}

interface AreaStyleOption<TCbParams = never> {
  color?: ZRColor | (TCbParams extends never ? never : (params: TCbParams) => ZRColor);
  shadowBlur?: number | (TCbParams extends never ? never : (params: TCbParams) => number);
  shadowColor?: ColorString | (TCbParams extends never ? never : (params: TCbParams) => ColorString);
  shadowOffsetX?: number | (TCbParams extends never ? never : (params: TCbParams) => number);
  shadowOffsetY?: number | (TCbParams extends never ? never : (params: TCbParams) => number);
  opacity?: number | (TCbParams extends never ? never : (params: TCbParams) => number);
}
```

### Label Options

```typescript
interface SeriesLabelOption {
  show?: boolean;
  position?: 'top' | 'left' | 'right' | 'bottom' | 'inside' | 'insideLeft' | 'insideRight' | 'insideTop' | 'insideBottom' | 'insideTopLeft' | 'insideBottomLeft' | 'insideTopRight' | 'insideBottomRight' | (number | string)[] | ((params: LabelFormatterCallbackParams) => (number | string)[]);
  distance?: number;
  rotate?: number | boolean;
  offset?: number[];
  minMargin?: number;
  overflow?: 'break' | 'breakAll' | 'truncate' | 'none';
  silent?: boolean;
  precision?: number | 'auto';
  valueAnimation?: boolean;
  rich?: Dictionary<TextCommonOption>;
  formatter?: string | LabelFormatterCallback;
  color?: ColorString;
  fontStyle?: ZRFontStyle;
  fontWeight?: ZRFontWeight;
  fontFamily?: string;
  fontSize?: number;
  align?: ZRTextAlign;
  verticalAlign?: ZRTextVerticalAlign;
  lineHeight?: number;
  backgroundColor?: ColorString | {
    image: ImageLike | string;
  };
  borderColor?: ColorString;
  borderWidth?: number;
  borderType?: ZRLineType;
  borderDashOffset?: number;
  borderRadius?: number | number[];
  padding?: number | number[];
  shadowColor?: ColorString;
  shadowBlur?: number;
  shadowOffsetX?: number;
  shadowOffsetY?: number;
  width?: number | string;
  height?: number | string;
  textBorderColor?: ColorString;
  textBorderWidth?: number;
  textBorderType?: ZRLineType;
  textBorderDashOffset?: number;
  textShadowColor?: ColorString;
  textShadowBlur?: number;
  textShadowOffsetX?: number;
  textShadowOffsetY?: number;
}
```

## Animation Interfaces

```typescript
interface AnimationOption {
  animation?: boolean;
  animationThreshold?: number;
  animationDuration?: number | AnimationDurationCallback;
  animationEasing?: AnimationEasing;
  animationDelay?: number | AnimationDelayCallback;
  animationDurationUpdate?: number | AnimationDurationCallback;
  animationEasingUpdate?: AnimationEasing;
  animationDelayUpdate?: number | AnimationDelayCallback;
}

type AnimationDurationCallback = (idx: number) => number;
type AnimationDelayCallback = (idx: number, params?: AnimationDelayCallbackParam) => number;

interface AnimationDelayCallbackParam {
  count: number;
  index: number;
}

type AnimationEasing = 
  | 'linear'
  | 'quadraticIn' | 'quadraticOut' | 'quadraticInOut'
  | 'cubicIn' | 'cubicOut' | 'cubicInOut'
  | 'quarticIn' | 'quarticOut' | 'quarticInOut'
  | 'quinticIn' | 'quinticOut' | 'quinticInOut'
  | 'sinusoidalIn' | 'sinusoidalOut' | 'sinusoidalInOut'
  | 'exponentialIn' | 'exponentialOut' | 'exponentialInOut'
  | 'circularIn' | 'circularOut' | 'circularInOut'
  | 'elasticIn' | 'elasticOut' | 'elasticInOut'
  | 'backIn' | 'backOut' | 'backInOut'
  | 'bounceIn' | 'bounceOut' | 'bounceInOut';
```

## Event Interfaces

```typescript
interface ECActionEvent {
  type: string;
  event?: ElementEvent;
  [key: string]: any;
}

interface SelectChangedPayload {
  type: 'selectchanged';
  escapeConnect: boolean;
  isFromClick: boolean;
  fromAction: 'select' | 'unselect' | 'toggleSelected';
  fromActionPayload: Payload;
  selected: {
    seriesIndex: number;
    dataType?: SeriesDataType;
    dataIndex: number[];
  }[];
}

interface CallbackDataParams {
  componentType: string;
  componentSubType: string;
  componentIndex: number;
  seriesType?: string;
  seriesIndex?: number;
  seriesId?: string;
  seriesName?: string;
  name: string;
  dataIndex: number;
  data: OptionDataItem;
  dataType?: SeriesDataType;
  value: OptionDataValue | OptionDataValue[];
  color?: ZRColor;
  borderColor?: string;
  dimensionNames?: DimensionName[];
  encode?: DimensionUserOuputEncode;
  marker?: TooltipMarker;
  status?: DisplayState;
  dimensionIndex?: number;
  percent?: number;
  $vars: string[];
}
```

## Usage Examples

### Basic TypeScript Usage

```typescript
import * as echarts from 'echarts';
import type { EChartsOption } from 'echarts';

// Create chart instance
const chartDom = document.getElementById('main')!;
const myChart = echarts.init(chartDom);

// Define options with full type safety
const option: EChartsOption = {
  title: {
    text: 'My Chart'
  },
  tooltip: {
    trigger: 'axis'
  },
  xAxis: {
    type: 'category',
    data: ['Mon', 'Tue', 'Wed', 'Thu', 'Fri', 'Sat', 'Sun']
  },
  yAxis: {
    type: 'value'
  },
  series: [
    {
      type: 'bar',
      data: [120, 200, 150, 80, 70, 110, 130]
    }
  ]
};

// Set option with type checking
myChart.setOption(option);
```

### Custom Series Type

```typescript
import type { SeriesOption } from 'echarts';

// Extend RegisteredSeriesOption for custom series
declare module 'echarts/types/dist/echarts' {
  interface RegisteredSeriesOption {
    myCustomSeries: MyCustomSeriesOption;
  }
}

interface MyCustomSeriesOption extends SeriesOption {
  type: 'myCustomSeries';
  customProperty?: string;
  customData?: number[];
}

// Now you can use the custom series with full type support
const option: EChartsOption = {
  series: [
    {
      type: 'myCustomSeries',
      customProperty: 'value',
      customData: [1, 2, 3]
    }
  ]
};
```

### Event Handling with Types

```typescript
import type { ECElementEvent, CallbackDataParams } from 'echarts';

// Click event handler
myChart.on('click', (params: CallbackDataParams) => {
  console.log('Clicked data:', params.data);
  console.log('Series index:', params.seriesIndex);
});

// Mouse events
myChart.on('mouseover', (params: CallbackDataParams) => {
  console.log('Mouse over:', params.name);
});

// Custom events
myChart.on('selectchanged', (params: SelectChangedPayload) => {
  console.log('Selection changed:', params.selected);
});
```

### Custom Formatter Functions

```typescript
import type { TooltipFormatterCallback, LabelFormatterCallback } from 'echarts';

// Tooltip formatter
const tooltipFormatter: TooltipFormatterCallback<CallbackDataParams> = (params) => {
  if (Array.isArray(params)) {
    return params.map(p => `${p.seriesName}: ${p.value}`).join('<br/>');
  }
  return `${params.seriesName}: ${params.value}`;
};

// Label formatter
const labelFormatter: LabelFormatterCallback = (params) => {
  return `${params.value}%`;
};

const option: EChartsOption = {
  tooltip: {
    formatter: tooltipFormatter
  },
  series: [
    {
      type: 'pie',
      label: {
        formatter: labelFormatter
      },
      data: [
        { name: 'A', value: 10 },
        { name: 'B', value: 20 }
      ]
    }
  ]
};
```

This TypeScript definitions reference provides comprehensive type information for building type-safe ECharts applications. Use these interfaces to get full IntelliSense support and compile-time type checking in your TypeScript projects.