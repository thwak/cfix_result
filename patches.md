##chart1
###Human
```diff
--- /home/ec2-user/cfix_exp/cfix-patch/chart/chart1/buggy/org/jfree/chart/renderer/category/AbstractCategoryItemRenderer.java	2016-08-31 21:38:05.366265168 +0900
+++ /home/ec2-user/cfix_exp/cfix-patch/chart/chart1/fix/org/jfree/chart/renderer/category/AbstractCategoryItemRenderer.java	2016-08-31 21:38:05.378264816 +0900
@@ -1794,7 +1794,7 @@
         }
         int index = this.plot.getIndexOf(this);
         CategoryDataset dataset = this.plot.getDataset(index);
-        if (dataset != null) {
+        if (dataset == null) {
             return result;
         }
         int seriesCount = dataset.getRowCount();
```
###C-Fix
```diff
--- /home/ec2-user/cfix_exp/cfix-patch/chart/chart1/buggy/org/jfree/chart/renderer/category/AbstractCategoryItemRenderer.java	2016-08-31 21:38:05.366265168 +0900
+++ /home/ec2-user/cfix_exp/cfix-patch/chart/chart1/cfix/org/jfree/chart/renderer/category/AbstractCategoryItemRenderer.java	2016-08-31 21:38:05.358265403 +0900
@@ -1794,7 +1794,7 @@
         }
         int index = this.plot.getIndexOf(this);
         CategoryDataset dataset = this.plot.getDataset(index);
-        if (dataset != null) {
+        if (SortOrder.DESCENDING == null) {
             return result;
         }
         int seriesCount = dataset.getRowCount();
```

##chart11
###Human
```diff
--- /home/ec2-user/cfix_exp/cfix-patch/chart/chart11/buggy/org/jfree/chart/util/ShapeUtilities.java	2016-08-31 21:38:05.302267045 +0900
+++ /home/ec2-user/cfix_exp/cfix-patch/chart/chart11/fix/org/jfree/chart/util/ShapeUtilities.java	2016-08-31 21:38:05.330266224 +0900
@@ -272,7 +272,7 @@
             return false;
         }
         PathIterator iterator1 = p1.getPathIterator(null);
-        PathIterator iterator2 = p1.getPathIterator(null);
+        PathIterator iterator2 = p2.getPathIterator(null);
         double[] d1 = new double[6];
         double[] d2 = new double[6];
         boolean done = iterator1.isDone() && iterator2.isDone();
```
###C-Fix
```diff
--- /home/ec2-user/cfix_exp/cfix-patch/chart/chart11/buggy/org/jfree/chart/util/ShapeUtilities.java	2016-08-31 21:38:05.302267045 +0900
+++ /home/ec2-user/cfix_exp/cfix-patch/chart/chart11/cfix/org/jfree/chart/util/ShapeUtilities.java	2016-08-31 21:38:05.278267749 +0900
@@ -72,6 +72,7 @@
 import java.awt.geom.Point2D;
 import java.awt.geom.Rectangle2D;
 import java.util.Arrays;
+import java.util.List;
 
 /**
  * Utility methods for {@link Shape} objects.
@@ -271,7 +272,7 @@
         if (p1.getWindingRule() != p2.getWindingRule()) {
             return false;
         }
-        PathIterator iterator1 = p1.getPathIterator(null);
+        PathIterator iterator1 = p2.getPathIterator(null);
         PathIterator iterator2 = p1.getPathIterator(null);
         double[] d1 = new double[6];
         double[] d2 = new double[6];
```

##chart13
###Human
```diff
--- /home/ec2-user/cfix_exp/cfix-patch/chart/chart13/buggy/org/jfree/chart/block/BorderArrangement.java	2016-08-31 21:38:05.342265872 +0900
+++ /home/ec2-user/cfix_exp/cfix-patch/chart/chart13/fix/org/jfree/chart/block/BorderArrangement.java	2016-08-31 21:38:05.346265755 +0900
@@ -452,7 +452,7 @@
         h[3] = h[2];
         if (this.rightBlock != null) {
             RectangleConstraint c4 = new RectangleConstraint(0.0,
-                    new Range(0.0, constraint.getWidth() - w[2]),
+                    new Range(0.0, Math.max(constraint.getWidth() - w[2], 0.0)),
                     LengthConstraintType.RANGE, h[2], null,
                     LengthConstraintType.FIXED);
             Size2D size = this.rightBlock.arrange(g2, c4);
```
###C-Fix
```diff
--- /home/ec2-user/cfix_exp/cfix-patch/chart/chart13/buggy/org/jfree/chart/block/BorderArrangement.java	2016-08-31 21:38:05.342265872 +0900
+++ /home/ec2-user/cfix_exp/cfix-patch/chart/chart13/cfix/org/jfree/chart/block/BorderArrangement.java	2016-08-31 21:38:05.338265989 +0900
@@ -447,7 +447,7 @@
                     LengthConstraintType.RANGE, h[2], null,
                     LengthConstraintType.FIXED);
             Size2D size = this.leftBlock.arrange(g2, c3);
-            w[2] = size.width;
+            h[2] = size.width;
         }
         h[3] = h[2];
         if (this.rightBlock != null) {
```

##chart24
###Human
```diff
--- /home/ec2-user/cfix_exp/cfix-patch/chart/chart24/buggy/org/jfree/chart/renderer/GrayPaintScale.java	2016-08-31 21:38:05.454262588 +0900
+++ /home/ec2-user/cfix_exp/cfix-patch/chart/chart24/fix/org/jfree/chart/renderer/GrayPaintScale.java	2016-08-31 21:38:05.506261063 +0900
@@ -123,7 +123,7 @@
     public Paint getPaint(double value) {
         double v = Math.max(value, this.lowerBound);
         v = Math.min(v, this.upperBound);
-        int g = (int) ((value - this.lowerBound) / (this.upperBound 
+        int g = (int) ((v - this.lowerBound) / (this.upperBound 
                 - this.lowerBound) * 255.0);
         return new Color(g, g, g);
     }
```
###C-Fix
```diff
--- /home/ec2-user/cfix_exp/cfix-patch/chart/chart24/buggy/org/jfree/chart/renderer/GrayPaintScale.java	2016-08-31 21:38:05.454262588 +0900
+++ /home/ec2-user/cfix_exp/cfix-patch/chart/chart24/cfix/org/jfree/chart/renderer/GrayPaintScale.java	2016-08-31 21:38:05.386264582 +0900
@@ -122,7 +122,7 @@
      */
     public Paint getPaint(double value) {
         double v = Math.max(value, this.lowerBound);
-        v = Math.min(v, this.upperBound);
+        value = Math.min(v, this.upperBound);
         int g = (int) ((value - this.lowerBound) / (this.upperBound 
                 - this.lowerBound) * 255.0);
         return new Color(g, g, g);
```

##chart25
###Human
```diff
--- /home/ec2-user/cfix_exp/cfix-patch/chart/chart25/buggy/org/jfree/chart/renderer/category/StatisticalBarRenderer.java	2016-08-31 21:38:05.514260828 +0900
+++ /home/ec2-user/cfix_exp/cfix-patch/chart/chart25/fix/org/jfree/chart/renderer/category/StatisticalBarRenderer.java	2016-08-31 21:38:05.530260359 +0900
@@ -256,6 +256,9 @@
 
         // BAR X
         Number meanValue = dataset.getMeanValue(row, column);
+        if (meanValue == null) {
+            return;
+        }
 
         double value = meanValue.doubleValue();
         double base = 0.0;
@@ -312,7 +315,9 @@
         }
 
         // standard deviation lines
-            double valueDelta = dataset.getStdDevValue(row, column).doubleValue();
+        Number n = dataset.getStdDevValue(row, column);
+        if (n != null) {
+            double valueDelta = n.doubleValue();
             double highVal = rangeAxis.valueToJava2D(meanValue.doubleValue() 
                     + valueDelta, dataArea, yAxisLocation);
             double lowVal = rangeAxis.valueToJava2D(meanValue.doubleValue() 
@@ -341,6 +346,7 @@
             line = new Line2D.Double(lowVal, rectY + rectHeight * 0.25, 
                                      lowVal, rectY + rectHeight * 0.75);
             g2.draw(line);
+        }
         
         CategoryItemLabelGenerator generator = getItemLabelGenerator(row, 
                 column);
@@ -400,6 +406,9 @@
 
         // BAR Y
         Number meanValue = dataset.getMeanValue(row, column);
+        if (meanValue == null) {
+            return;
+        }
 
         double value = meanValue.doubleValue();
         double base = 0.0;
@@ -456,7 +465,9 @@
         }
 
         // standard deviation lines
-            double valueDelta = dataset.getStdDevValue(row, column).doubleValue();
+        Number n = dataset.getStdDevValue(row, column);
+        if (n != null) {
+            double valueDelta = n.doubleValue();
             double highVal = rangeAxis.valueToJava2D(meanValue.doubleValue() 
                     + valueDelta, dataArea, yAxisLocation);
             double lowVal = rangeAxis.valueToJava2D(meanValue.doubleValue() 
@@ -484,6 +495,7 @@
             line = new Line2D.Double(rectX + rectWidth / 2.0d - 5.0d, lowVal,
                                      rectX + rectWidth / 2.0d + 5.0d, lowVal);
             g2.draw(line);
+        }
         
         CategoryItemLabelGenerator generator = getItemLabelGenerator(row, 
                 column);
```
###C-Fix
```diff
--- /home/ec2-user/cfix_exp/cfix-patch/chart/chart25/buggy/org/jfree/chart/renderer/category/StatisticalBarRenderer.java	2016-08-31 21:38:05.514260828 +0900
+++ /home/ec2-user/cfix_exp/cfix-patch/chart/chart25/cfix/org/jfree/chart/renderer/category/StatisticalBarRenderer.java	2016-08-31 21:38:05.510260945 +0900
@@ -85,6 +85,7 @@
 import org.jfree.chart.util.SerialUtilities;
 import org.jfree.data.category.CategoryDataset;
 import org.jfree.data.statistics.StatisticalCategoryDataset;
+import java.net.Socket;
 
 /**
  * A renderer that handles the drawing a bar plot where
@@ -203,7 +204,7 @@
         }
         StatisticalCategoryDataset statData = (StatisticalCategoryDataset) data;
 
-        PlotOrientation orientation = plot.getOrientation();
+        PlotOrientation orientation = null;
         if (orientation == PlotOrientation.HORIZONTAL) {
             drawHorizontalItem(g2, state, dataArea, plot, domainAxis, 
                     rangeAxis, statData, row, column);
```

##chart26
###Human
```diff
--- /home/ec2-user/cfix_exp/cfix-patch/chart/chart26/buggy/org/jfree/chart/axis/Axis.java	2016-08-31 21:38:05.554259655 +0900
+++ /home/ec2-user/cfix_exp/cfix-patch/chart/chart26/fix/org/jfree/chart/axis/Axis.java	2016-08-31 21:38:05.626257544 +0900
@@ -1189,11 +1189,13 @@
         }
         if (plotState != null && hotspot != null) {
             ChartRenderingInfo owner = plotState.getOwner();
+            if (owner != null) {
                 EntityCollection entities = owner.getEntityCollection();
                 if (entities != null) {
                     entities.add(new AxisLabelEntity(this, hotspot, 
                             this.labelToolTip, this.labelURL));
                 }
+            }
         }
         return state;
 
```
###C-Fix
```diff
--- /home/ec2-user/cfix_exp/cfix-patch/chart/chart26/buggy/org/jfree/chart/axis/Axis.java	2016-08-31 21:38:05.554259655 +0900
+++ /home/ec2-user/cfix_exp/cfix-patch/chart/chart26/cfix/org/jfree/chart/axis/Axis.java	2016-08-31 21:38:05.542260007 +0900
@@ -896,7 +896,8 @@
      * @see #getPlot()
      */
     public void setPlot(Plot plot) {
-        this.plot = plot;
+        this.label = plot.getNoDataMessage();
+		this.plot = plot;
         configure();
     }
 
```

##chart3
###Human
```diff
--- /home/ec2-user/cfix_exp/cfix-patch/chart/chart3/buggy/org/jfree/data/time/TimeSeries.java	2016-08-31 21:38:05.642257074 +0900
+++ /home/ec2-user/cfix_exp/cfix-patch/chart/chart3/fix/org/jfree/data/time/TimeSeries.java	2016-08-31 21:38:05.646256957 +0900
@@ -1054,6 +1054,8 @@
             throw new IllegalArgumentException("Requires start <= end.");
         }
         TimeSeries copy = (TimeSeries) super.clone();
+        copy.minY = Double.NaN;
+        copy.maxY = Double.NaN;
         copy.data = new java.util.ArrayList();
         if (this.data.size() > 0) {
             for (int index = start; index <= end; index++) {
```
###C-Fix
```diff
--- /home/ec2-user/cfix_exp/cfix-patch/chart/chart3/buggy/org/jfree/data/time/TimeSeries.java	2016-08-31 21:38:05.642257074 +0900
+++ /home/ec2-user/cfix_exp/cfix-patch/chart/chart3/cfix/org/jfree/data/time/TimeSeries.java	2016-08-31 21:38:05.634257309 +0900
@@ -635,7 +635,7 @@
                                      // don't notify anyone, because that
                                      // happens next anyway...
             if (notify) {
-                fireSeriesChanged();
+                findBoundsByIteration();;
             }
         }
 
```

##chart4
###Human
```diff
--- /home/ec2-user/cfix_exp/cfix-patch/chart/chart4/buggy/org/jfree/chart/plot/XYPlot.java	2016-08-31 21:38:05.678256019 +0900
+++ /home/ec2-user/cfix_exp/cfix-patch/chart/chart4/fix/org/jfree/chart/plot/XYPlot.java	2016-08-31 21:38:05.686255784 +0900
@@ -4490,6 +4490,7 @@
                     }
                 }
                 
+                if (r != null) {
                     Collection c = r.getAnnotations();
                     Iterator i = c.iterator();
                     while (i.hasNext()) {
@@ -4498,6 +4499,7 @@
                             includedAnnotations.add(a);
                         }
                     }
+                }
             }
         }
 
```
###C-Fix
```diff
--- /home/ec2-user/cfix_exp/cfix-patch/chart/chart4/buggy/org/jfree/chart/plot/XYPlot.java	2016-08-31 21:38:05.678256019 +0900
+++ /home/ec2-user/cfix_exp/cfix-patch/chart/chart4/cfix/org/jfree/chart/plot/XYPlot.java	2016-08-31 21:38:05.658256605 +0900
@@ -298,6 +298,7 @@
 import org.jfree.data.xy.SelectableXYDataset;
 import org.jfree.data.xy.XYDataset;
 import org.jfree.data.xy.XYDatasetSelectionState;
+import java.util.LinkedList;
 
 /**
  * A general class for plotting data in the form of (x, y) pairs.  This plot can
@@ -4490,7 +4491,7 @@
                     }
                 }
                 
-                    Collection c = r.getAnnotations();
+                    Collection c = getAnnotations();
                     Iterator i = c.iterator();
                     while (i.hasNext()) {
                         XYAnnotation a = (XYAnnotation) i.next();
```

##chart5
###Human
```diff
--- /home/ec2-user/cfix_exp/cfix-patch/chart/chart5/buggy/org/jfree/data/xy/XYSeries.java	2016-08-31 21:38:05.710255080 +0900
+++ /home/ec2-user/cfix_exp/cfix-patch/chart/chart5/fix/org/jfree/data/xy/XYSeries.java	2016-08-31 21:38:05.710255080 +0900
@@ -541,11 +541,15 @@
         if (x == null) {
             throw new IllegalArgumentException("Null 'x' argument.");
         }
+        if (this.allowDuplicateXValues) {
+            add(x, y);
+            return null;
+        }
 
         // if we get to here, we know that duplicate X values are not permitted
         XYDataItem overwritten = null;
         int index = indexOf(x);
-        if (index >= 0 && !this.allowDuplicateXValues) {
+        if (index >= 0) {
             XYDataItem existing = (XYDataItem) this.data.get(index);
             try {
                 overwritten = (XYDataItem) existing.clone();
```
###C-Fix
```diff
--- /home/ec2-user/cfix_exp/cfix-patch/chart/chart5/buggy/org/jfree/data/xy/XYSeries.java	2016-08-31 21:38:05.710255080 +0900
+++ /home/ec2-user/cfix_exp/cfix-patch/chart/chart5/cfix/org/jfree/data/xy/XYSeries.java	2016-08-31 21:38:05.702255315 +0900
@@ -524,7 +524,7 @@
      * @since 1.0.10
      */
     public XYDataItem addOrUpdate(double x, double y) {
-        return addOrUpdate(new Double(x), new Double(y));
+        return addOrUpdate(new Double(y), new Double(y));
     }
 
     /**
```

##chart6
###Human
```diff
--- /home/ec2-user/cfix_exp/cfix-patch/chart/chart6/buggy/org/jfree/chart/util/ShapeList.java	2016-08-31 21:38:05.782252969 +0900
+++ /home/ec2-user/cfix_exp/cfix-patch/chart/chart6/fix/org/jfree/chart/util/ShapeList.java	2016-08-31 21:38:05.806252265 +0900
@@ -108,7 +108,14 @@
         if (!(obj instanceof ShapeList)) {
             return false;
         }
-        return super.equals(obj);
+        ShapeList that = (ShapeList) obj;
+        int listSize = size();
+        for (int i = 0; i < listSize; i++) {
+           if (!ShapeUtilities.equal((Shape) get(i), (Shape) that.get(i))) {
+               return false;
+           }
+        }
+        return true;
 
     }
 
```
###C-Fix
```diff
--- /home/ec2-user/cfix_exp/cfix-patch/chart/chart6/buggy/org/jfree/chart/util/ShapeList.java	2016-08-31 21:38:05.782252969 +0900
+++ /home/ec2-user/cfix_exp/cfix-patch/chart/chart6/cfix/org/jfree/chart/util/ShapeList.java	2016-08-31 21:38:05.718254846 +0900
@@ -78,7 +78,8 @@
      * @param shape  the {@link Shape}.
      */
     public void setShape(int index, Shape shape) {
-        set(index, shape);
+        index = 0;
+		set(index, shape);
     }
 
     /**
```

##chart7
###Human
```diff
--- /home/ec2-user/cfix_exp/cfix-patch/chart/chart7/buggy/org/jfree/data/time/TimePeriodValues.java	2016-08-31 21:38:05.810252148 +0900
+++ /home/ec2-user/cfix_exp/cfix-patch/chart/chart7/fix/org/jfree/data/time/TimePeriodValues.java	2016-08-31 21:38:05.814252030 +0900
@@ -297,9 +297,9 @@
         }
         
         if (this.maxMiddleIndex >= 0) {
-            long s = getDataItem(this.minMiddleIndex).getPeriod().getStart()
+            long s = getDataItem(this.maxMiddleIndex).getPeriod().getStart()
                 .getTime();
-            long e = getDataItem(this.minMiddleIndex).getPeriod().getEnd()
+            long e = getDataItem(this.maxMiddleIndex).getPeriod().getEnd()
                 .getTime();
             long maxMiddle = s + (e - s) / 2;
             if (middle > maxMiddle) {
```
###C-Fix
```diff
--- /home/ec2-user/cfix_exp/cfix-patch/chart/chart7/buggy/org/jfree/data/time/TimePeriodValues.java	2016-08-31 21:38:05.810252148 +0900
+++ /home/ec2-user/cfix_exp/cfix-patch/chart/chart7/cfix/org/jfree/data/time/TimePeriodValues.java	2016-08-31 21:38:05.810252148 +0900
@@ -297,7 +297,7 @@
         }
         
         if (this.maxMiddleIndex >= 0) {
-            long s = getDataItem(this.minMiddleIndex).getPeriod().getStart()
+            long s = getDataItem(this.maxMiddleIndex).getPeriod().getStart()
                 .getTime();
             long e = getDataItem(this.minMiddleIndex).getPeriod().getEnd()
                 .getTime();
```

##chart9
###Human
```diff
--- /home/ec2-user/cfix_exp/cfix-patch/chart/chart9/buggy/org/jfree/data/time/TimeSeries.java	2016-08-31 21:38:05.826251678 +0900
+++ /home/ec2-user/cfix_exp/cfix-patch/chart/chart9/fix/org/jfree/data/time/TimeSeries.java	2016-08-31 21:38:05.846251092 +0900
@@ -941,7 +941,7 @@
             endIndex = -(endIndex + 1); // this is first item AFTER end period
             endIndex = endIndex - 1;    // so this is last item BEFORE end
         }
-        if (endIndex < 0) {
+        if ((endIndex < 0)  || (endIndex < startIndex)) {
             emptyRange = true;
         }
         if (emptyRange) {
```
###C-Fix
```diff
--- /home/ec2-user/cfix_exp/cfix-patch/chart/chart9/buggy/org/jfree/data/time/TimeSeries.java	2016-08-31 21:38:05.826251678 +0900
+++ /home/ec2-user/cfix_exp/cfix-patch/chart/chart9/cfix/org/jfree/data/time/TimeSeries.java	2016-08-31 21:38:05.822251796 +0900
@@ -941,7 +941,7 @@
             endIndex = -(endIndex + 1); // this is first item AFTER end period
             endIndex = endIndex - 1;    // so this is last item BEFORE end
         }
-        if (endIndex < 0) {
+        if (endIndex < startIndex) {
             emptyRange = true;
         }
         if (emptyRange) {
```

##lang10
###Human
```diff
--- /home/ec2-user/cfix_exp/cfix-patch/lang/lang10/buggy/org/apache/commons/lang3/time/FastDateParser.java	2016-08-31 21:38:02.478350068 +0900
+++ /home/ec2-user/cfix_exp/cfix-patch/lang/lang10/fix/org/apache/commons/lang3/time/FastDateParser.java	2016-08-31 21:38:02.486349832 +0900
@@ -301,17 +301,8 @@
      * @return The <code>StringBuilder</code>
      */
     private static StringBuilder escapeRegex(StringBuilder regex, String value, boolean unquote) {
-        boolean wasWhite= false;
         for(int i= 0; i<value.length(); ++i) {
             char c= value.charAt(i);
-            if(Character.isWhitespace(c)) {
-                if(!wasWhite) {
-                    wasWhite= true;
-                    regex.append("\\s*+");
-                }
-                continue;
-            }
-            wasWhite= false;
             switch(c) {
             case '\'':
                 if(unquote) {
```
###C-Fix
```diff
--- /home/ec2-user/cfix_exp/cfix-patch/lang/lang10/buggy/org/apache/commons/lang3/time/FastDateParser.java	2016-08-31 21:38:02.478350068 +0900
+++ /home/ec2-user/cfix_exp/cfix-patch/lang/lang10/cfix/org/apache/commons/lang3/time/FastDateParser.java	2016-08-31 21:38:02.470350303 +0900
@@ -304,7 +304,7 @@
         boolean wasWhite= false;
         for(int i= 0; i<value.length(); ++i) {
             char c= value.charAt(i);
-            if(Character.isWhitespace(c)) {
+            if(Character.isWhitespace(Locale.PRIVATE_USE_EXTENSION)) {
                 if(!wasWhite) {
                     wasWhite= true;
                     regex.append("\\s*+");
```

##lang22
###Human
```diff
--- /home/ec2-user/cfix_exp/cfix-patch/lang/lang22/buggy/org/apache/commons/lang3/math/Fraction.java	2016-08-31 21:38:02.522348772 +0900
+++ /home/ec2-user/cfix_exp/cfix-patch/lang/lang22/fix/org/apache/commons/lang3/math/Fraction.java	2016-08-31 21:38:02.526348653 +0900
@@ -580,8 +580,14 @@
      */
     private static int greatestCommonDivisor(int u, int v) {
         // From Commons Math:
+        if ((u == 0) || (v == 0)) {
+            if ((u == Integer.MIN_VALUE) || (v == Integer.MIN_VALUE)) {
+                throw new ArithmeticException("overflow: gcd is 2^31");
+            }
+            return Math.abs(u) + Math.abs(v);
+        }
         //if either operand is abs 1, return 1:
-        if (Math.abs(u) <= 1 || Math.abs(v) <= 1) {
+        if (Math.abs(u) == 1 || Math.abs(v) == 1) {
             return 1;
         }
         // keep u and v negative, as negative integers range down to
```
###C-Fix
```diff
--- /home/ec2-user/cfix_exp/cfix-patch/lang/lang22/buggy/org/apache/commons/lang3/math/Fraction.java	2016-08-31 21:38:02.522348772 +0900
+++ /home/ec2-user/cfix_exp/cfix-patch/lang/lang22/cfix/org/apache/commons/lang3/math/Fraction.java	2016-08-31 21:38:02.514349007 +0900
@@ -581,7 +581,7 @@
     private static int greatestCommonDivisor(int u, int v) {
         // From Commons Math:
         //if either operand is abs 1, return 1:
-        if (Math.abs(u) <= 1 || Math.abs(v) <= 1) {
+        if (Math.abs(v) <= 1 || Math.abs(v) <= 1) {
             return 1;
         }
         // keep u and v negative, as negative integers range down to
```

##lang26
###Human
```diff
--- /home/ec2-user/cfix_exp/cfix-patch/lang/lang26/buggy/org/apache/commons/lang3/time/FastDateFormat.java	2016-08-31 21:38:02.542348182 +0900
+++ /home/ec2-user/cfix_exp/cfix-patch/lang/lang26/fix/org/apache/commons/lang3/time/FastDateFormat.java	2016-08-31 21:38:02.546348065 +0900
@@ -817,7 +817,7 @@
      * @return the formatted string
      */
     public String format(Date date) {
-        Calendar c = new GregorianCalendar(mTimeZone);
+        Calendar c = new GregorianCalendar(mTimeZone, mLocale);
         c.setTime(date);
         return applyRules(c, new StringBuffer(mMaxLengthEstimate)).toString();
     }
```
###C-Fix
```diff
--- /home/ec2-user/cfix_exp/cfix-patch/lang/lang26/buggy/org/apache/commons/lang3/time/FastDateFormat.java	2016-08-31 21:38:02.542348182 +0900
+++ /home/ec2-user/cfix_exp/cfix-patch/lang/lang26/cfix/org/apache/commons/lang3/time/FastDateFormat.java	2016-08-31 21:38:02.538348300 +0900
@@ -817,7 +817,7 @@
      * @return the formatted string
      */
     public String format(Date date) {
-        Calendar c = new GregorianCalendar(mTimeZone);
+        Calendar c = new GregorianCalendar(mTimeZone, mLocale);
         c.setTime(date);
         return applyRules(c, new StringBuffer(mMaxLengthEstimate)).toString();
     }
```

##lang27
###Human
```diff
--- /home/ec2-user/cfix_exp/cfix-patch/lang/lang27/buggy/org/apache/commons/lang3/math/NumberUtils.java	2016-08-31 21:38:02.582347004 +0900
+++ /home/ec2-user/cfix_exp/cfix-patch/lang/lang27/fix/org/apache/commons/lang3/math/NumberUtils.java	2016-08-31 21:38:02.606346296 +0900
@@ -476,7 +476,7 @@
         if (decPos > -1) {
 
             if (expPos > -1) {
-                if (expPos < decPos) {
+                if (expPos < decPos || expPos > str.length()) {
                     throw new NumberFormatException(str + " is not a valid number.");
                 }
                 dec = str.substring(decPos + 1, expPos);
@@ -486,6 +486,9 @@
             mant = str.substring(0, decPos);
         } else {
             if (expPos > -1) {
+                if (expPos > str.length()) {
+                    throw new NumberFormatException(str + " is not a valid number.");
+                }
                 mant = str.substring(0, expPos);
             } else {
                 mant = str;
```
###C-Fix
```diff
--- /home/ec2-user/cfix_exp/cfix-patch/lang/lang27/buggy/org/apache/commons/lang3/math/NumberUtils.java	2016-08-31 21:38:02.582347004 +0900
+++ /home/ec2-user/cfix_exp/cfix-patch/lang/lang27/cfix/org/apache/commons/lang3/math/NumberUtils.java	2016-08-31 21:38:02.562347593 +0900
@@ -20,6 +20,7 @@
 import java.math.BigInteger;
 
 import org.apache.commons.lang3.StringUtils;
+import java.util.List;
 
 /**
  * <p>Provides extra functionality for Java Number classes.</p>
@@ -471,7 +472,7 @@
         String dec;
         String exp;
         int decPos = str.indexOf('.');
-        int expPos = str.indexOf('e') + str.indexOf('E') + 1;
+        int expPos = StringUtils.EMPTY.indexOf('e') + str.indexOf('E') + 1;
 
         if (decPos > -1) {
 
```

##lang31
###Human
```diff
--- /home/ec2-user/cfix_exp/cfix-patch/lang/lang31/buggy/org/apache/commons/lang3/StringUtils.java	2016-08-31 21:38:02.630345589 +0900
+++ /home/ec2-user/cfix_exp/cfix-patch/lang/lang31/fix/org/apache/commons/lang3/StringUtils.java	2016-08-31 21:38:02.638345354 +0900
@@ -1443,13 +1443,21 @@
 		}
 		int csLength = cs.length();
 		int searchLength = searchChars.length;
+		int csLastIndex = csLength - 1;
+		int searchLastIndex = searchLength - 1;
 		for (int i = 0; i < csLength; i++) {
 			char ch = cs.charAt(i);
 			for (int j = 0; j < searchLength; j++) {
 				if (searchChars[j] == ch) {
+					if (i < csLastIndex && j < searchLastIndex && ch >= Character.MIN_HIGH_SURROGATE && ch <= Character.MAX_HIGH_SURROGATE) {
 						// ch is a supplementary character
+						if (searchChars[j + 1] == cs.charAt(i + 1)) {
+							return true;
+						}
+					} else {
 						// ch is in the Basic Multilingual Plane
 						return true;
+					}
 				}
 			}
 		}
```
###C-Fix
```diff
--- /home/ec2-user/cfix_exp/cfix-patch/lang/lang31/buggy/org/apache/commons/lang3/StringUtils.java	2016-08-31 21:38:02.630345589 +0900
+++ /home/ec2-user/cfix_exp/cfix-patch/lang/lang31/cfix/org/apache/commons/lang3/StringUtils.java	2016-08-31 21:38:02.622345825 +0900
@@ -1443,7 +1443,7 @@
 		}
 		int csLength = cs.length();
 		int searchLength = searchChars.length;
-		for (int i = 0; i < csLength; i++) {
+		for (int i = 1; i < csLength; i++) {
 			char ch = cs.charAt(i);
 			for (int j = 0; j < searchLength; j++) {
 				if (searchChars[j] == ch) {
```

##lang39
###Human
```diff
--- /home/ec2-user/cfix_exp/cfix-patch/lang/lang39/buggy/org/apache/commons/lang3/StringUtils.java	2016-08-31 21:38:02.678344175 +0900
+++ /home/ec2-user/cfix_exp/cfix-patch/lang/lang39/fix/org/apache/commons/lang3/StringUtils.java	2016-08-31 21:38:02.686343939 +0900
@@ -3673,6 +3673,9 @@
 
         // count the replacement text elements that are larger than their corresponding text being replaced
         for (int i = 0; i < searchList.length; i++) {
+            if (searchList[i] == null || replacementList[i] == null) {
+                continue;
+            }
             int greater = replacementList[i].length() - searchList[i].length();
             if (greater > 0) {
                 increase += 3 * greater; // assume 3 matches
```
###C-Fix
```diff
--- /home/ec2-user/cfix_exp/cfix-patch/lang/lang39/buggy/org/apache/commons/lang3/StringUtils.java	2016-08-31 21:38:02.678344175 +0900
+++ /home/ec2-user/cfix_exp/cfix-patch/lang/lang39/cfix/org/apache/commons/lang3/StringUtils.java	2016-08-31 21:38:02.662344647 +0900
@@ -3673,7 +3673,7 @@
 
         // count the replacement text elements that are larger than their corresponding text being replaced
         for (int i = 0; i < searchList.length; i++) {
-            int greater = replacementList[i].length() - searchList[i].length();
+            int greater = searchList[i].length() - searchList[i].length();
             if (greater > 0) {
                 increase += 3 * greater; // assume 3 matches
             }
```

##lang43
###Human
```diff
--- /home/ec2-user/cfix_exp/cfix-patch/lang/lang43/buggy/org/apache/commons/lang/text/ExtendedMessageFormat.java	2016-08-31 21:38:02.714343114 +0900
+++ /home/ec2-user/cfix_exp/cfix-patch/lang/lang43/fix/org/apache/commons/lang/text/ExtendedMessageFormat.java	2016-08-31 21:38:02.718342997 +0900
@@ -419,6 +419,7 @@
         int start = pos.getIndex();
         char[] c = pattern.toCharArray();
         if (escapingOn && c[start] == QUOTE) {
+            next(pos);
             return appendTo == null ? null : appendTo.append(QUOTE);
         }
         int lastHold = start;
```
###C-Fix
```diff
--- /home/ec2-user/cfix_exp/cfix-patch/lang/lang43/buggy/org/apache/commons/lang/text/ExtendedMessageFormat.java	2016-08-31 21:38:02.714343114 +0900
+++ /home/ec2-user/cfix_exp/cfix-patch/lang/lang43/cfix/org/apache/commons/lang/text/ExtendedMessageFormat.java	2016-08-31 21:38:02.702343468 +0900
@@ -155,7 +155,7 @@
         while (pos.getIndex() < pattern.length()) {
             switch (c[pos.getIndex()]) {
             case QUOTE:
-                appendQuotedString(pattern, pos, stripCustom, true);
+                appendQuotedString(pattern, pos, stripCustom, false);
                 break;
             case START_FE:
                 fmtCount++;
```

##lang59
###Human
```diff
--- /home/ec2-user/cfix_exp/cfix-patch/lang/lang59/buggy/org/apache/commons/lang/text/StrBuilder.java	2016-08-31 21:38:02.782341111 +0900
+++ /home/ec2-user/cfix_exp/cfix-patch/lang/lang59/fix/org/apache/commons/lang/text/StrBuilder.java	2016-08-31 21:38:02.810340286 +0900
@@ -881,7 +881,7 @@
             String str = (obj == null ? getNullText() : obj.toString());
             int strLen = str.length();
             if (strLen >= width) {
-                str.getChars(0, strLen, buffer, size);
+                str.getChars(0, width, buffer, size);
             } else {
                 int padLen = width - strLen;
                 str.getChars(0, strLen, buffer, size);
```
###C-Fix
```diff
--- /home/ec2-user/cfix_exp/cfix-patch/lang/lang59/buggy/org/apache/commons/lang/text/StrBuilder.java	2016-08-31 21:38:02.782341111 +0900
+++ /home/ec2-user/cfix_exp/cfix-patch/lang/lang59/cfix/org/apache/commons/lang/text/StrBuilder.java	2016-08-31 21:38:02.738342407 +0900
@@ -877,7 +877,7 @@
      */
     public StrBuilder appendFixedWidthPadRight(Object obj, int width, char padChar) {
         if (width > 0) {
-            ensureCapacity(size + width);
+            ensureCapacity(SystemUtils.JAVA_VERSION_INT + width);
             String str = (obj == null ? getNullText() : obj.toString());
             int strLen = str.length();
             if (strLen >= width) {
```

##lang6
###Human
```diff
--- /home/ec2-user/cfix_exp/cfix-patch/lang/lang6/buggy/org/apache/commons/lang3/text/translate/CharSequenceTranslator.java	2016-08-31 21:38:03.098331806 +0900
+++ /home/ec2-user/cfix_exp/cfix-patch/lang/lang6/fix/org/apache/commons/lang3/text/translate/CharSequenceTranslator.java	2016-08-31 21:38:03.102331688 +0900
@@ -92,7 +92,7 @@
 //          // contract with translators is that they have to understand codepoints 
 //          // and they just took care of a surrogate pair
             for (int pt = 0; pt < consumed; pt++) {
-                pos += Character.charCount(Character.codePointAt(input, pos));
+                pos += Character.charCount(Character.codePointAt(input, pt));
             }
         }
     }
```
###C-Fix
```diff
--- /home/ec2-user/cfix_exp/cfix-patch/lang/lang6/buggy/org/apache/commons/lang3/text/translate/CharSequenceTranslator.java	2016-08-31 21:38:03.098331806 +0900
+++ /home/ec2-user/cfix_exp/cfix-patch/lang/lang6/cfix/org/apache/commons/lang3/text/translate/CharSequenceTranslator.java	2016-08-31 21:38:03.094331923 +0900
@@ -92,7 +92,7 @@
 //          // contract with translators is that they have to understand codepoints 
 //          // and they just took care of a surrogate pair
             for (int pt = 0; pt < consumed; pt++) {
-                pos += Character.charCount(Character.codePointAt(input, pos));
+                pos += Character.charCount(Character.codePointAt(input, pt));
             }
         }
     }
```

##lang60
###Human
```diff
--- /home/ec2-user/cfix_exp/cfix-patch/lang/lang60/buggy/org/apache/commons/lang/text/StrBuilder.java	2016-08-31 21:38:02.866338637 +0900
+++ /home/ec2-user/cfix_exp/cfix-patch/lang/lang60/fix/org/apache/commons/lang/text/StrBuilder.java	2016-08-31 21:38:02.874338400 +0900
@@ -1670,7 +1670,7 @@
      */
     public boolean contains(char ch) {
         char[] thisBuf = buffer;
-        for (int i = 0; i < thisBuf.length; i++) {
+        for (int i = 0; i < this.size; i++) {
             if (thisBuf[i] == ch) {
                 return true;
             }
@@ -1727,7 +1727,7 @@
             return -1;
         }
         char[] thisBuf = buffer;
-        for (int i = startIndex; i < thisBuf.length; i++) {
+        for (int i = startIndex; i < size; i++) {
             if (thisBuf[i] == ch) {
                 return i;
             }
```
###C-Fix
```diff
--- /home/ec2-user/cfix_exp/cfix-patch/lang/lang60/buggy/org/apache/commons/lang/text/StrBuilder.java	2016-08-31 21:38:02.866338637 +0900
+++ /home/ec2-user/cfix_exp/cfix-patch/lang/lang60/cfix/org/apache/commons/lang/text/StrBuilder.java	2016-08-31 21:38:02.826339815 +0900
@@ -1111,7 +1111,7 @@
      * @throws IndexOutOfBoundsException if any index is invalid
      */
     private void deleteImpl(int startIndex, int endIndex, int len) {
-        System.arraycopy(buffer, endIndex, buffer, startIndex, size - endIndex);
+        System.arraycopy(buffer, endIndex, buffer, startIndex, size - startIndex - 1);
         size -= len;
     }
 
```

##lang61
###Human
```diff
--- /home/ec2-user/cfix_exp/cfix-patch/lang/lang61/buggy/org/apache/commons/lang/text/StrBuilder.java	2016-08-31 21:38:02.950336162 +0900
+++ /home/ec2-user/cfix_exp/cfix-patch/lang/lang61/fix/org/apache/commons/lang/text/StrBuilder.java	2016-08-31 21:38:02.954336043 +0900
@@ -1773,7 +1773,7 @@
             return -1;
         }
         char[] thisBuf = buffer;
-        int len = thisBuf.length - strLen;
+        int len = size - strLen + 1;
         outer:
         for (int i = startIndex; i < len; i++) {
             for (int j = 0; j < strLen; j++) {
```
###C-Fix
```diff
--- /home/ec2-user/cfix_exp/cfix-patch/lang/lang61/buggy/org/apache/commons/lang/text/StrBuilder.java	2016-08-31 21:38:02.950336162 +0900
+++ /home/ec2-user/cfix_exp/cfix-patch/lang/lang61/cfix/org/apache/commons/lang/text/StrBuilder.java	2016-08-31 21:38:02.910337340 +0900
@@ -1775,7 +1775,7 @@
         char[] thisBuf = buffer;
         int len = thisBuf.length - strLen;
         outer:
-        for (int i = startIndex; i < len; i++) {
+        for (int i = startIndex; i < size - 1; i++) {
             for (int j = 0; j < strLen; j++) {
                 if (str.charAt(j) != thisBuf[i + j]) {
                     continue outer;
```

##lang63
###Human
```diff
--- /home/ec2-user/cfix_exp/cfix-patch/lang/lang63/buggy/org/apache/commons/lang/time/DurationFormatUtils.java	2016-08-31 21:38:03.038333571 +0900
+++ /home/ec2-user/cfix_exp/cfix-patch/lang/lang63/fix/org/apache/commons/lang/time/DurationFormatUtils.java	2016-08-31 21:38:03.090332041 +0900
@@ -303,25 +303,20 @@
             days -= 1;
         }
         while (days < 0) {
-            days += 31;
+            end.add(Calendar.MONTH, -1);
+            days += end.getActualMaximum(Calendar.DAY_OF_MONTH);
 //days += 31; // TODO: Need tests to show this is bad and the new code is good.
 // HEN: It's a tricky subject. Jan 15th to March 10th. If I count days-first it is 
 // 1 month and 26 days, but if I count month-first then it is 1 month and 23 days.
 // Also it's contextual - if asked for no M in the format then I should probably 
 // be doing no calculating here.
             months -= 1;
+            end.add(Calendar.MONTH, 1);
         }
         while (months < 0) {
             months += 12;
             years -= 1;
         }
-        milliseconds -= reduceAndCorrect(start, end, Calendar.MILLISECOND, milliseconds);
-        seconds -= reduceAndCorrect(start, end, Calendar.SECOND, seconds);
-        minutes -= reduceAndCorrect(start, end, Calendar.MINUTE, minutes);
-        hours -= reduceAndCorrect(start, end, Calendar.HOUR_OF_DAY, hours);
-        days -= reduceAndCorrect(start, end, Calendar.DAY_OF_MONTH, days);
-        months -= reduceAndCorrect(start, end, Calendar.MONTH, months);
-        years -= reduceAndCorrect(start, end, Calendar.YEAR, years);
 
         // This next block of code adds in values that 
         // aren't requested. This allows the user to ask for the 
@@ -429,18 +424,6 @@
         }
         return buffer.toString();
     }
-    static int reduceAndCorrect(Calendar start, Calendar end, int field, int difference) {
-        end.add( field, -1 * difference );
-        int endValue = end.get(field);
-        int startValue = start.get(field);
-        if (endValue < startValue) {
-            int newdiff = startValue - endValue;
-            end.add( field, newdiff );
-            return newdiff;
-        } else {
-            return 0;
-        }
-    }
 
     static final Object y = "y";
     static final Object M = "M";
```
###C-Fix
```diff
--- /home/ec2-user/cfix_exp/cfix-patch/lang/lang63/buggy/org/apache/commons/lang/time/DurationFormatUtils.java	2016-08-31 21:38:03.038333571 +0900
+++ /home/ec2-user/cfix_exp/cfix-patch/lang/lang63/cfix/org/apache/commons/lang/time/DurationFormatUtils.java	2016-08-31 21:38:02.978335336 +0900
@@ -435,7 +435,7 @@
         int startValue = start.get(field);
         if (endValue < startValue) {
             int newdiff = startValue - endValue;
-            end.add( field, newdiff );
+            end.set( field, newdiff );
             return newdiff;
         } else {
             return 0;
```

##lang7
###Human
```diff
--- /home/ec2-user/cfix_exp/cfix-patch/lang/lang7/buggy/org/apache/commons/lang3/math/NumberUtils.java	2016-08-31 21:38:03.130330864 +0900
+++ /home/ec2-user/cfix_exp/cfix-patch/lang/lang7/fix/org/apache/commons/lang3/math/NumberUtils.java	2016-08-31 21:38:03.154330159 +0900
@@ -449,9 +449,6 @@
         if (StringUtils.isBlank(str)) {
             throw new NumberFormatException("A blank string is not a valid number");
         }  
-        if (str.startsWith("--")) {
-            return null;
-        }
         if (str.startsWith("0x") || str.startsWith("-0x") || str.startsWith("0X") || str.startsWith("-0X")) {
             int hexDigits = str.length() - 2; // drop 0x
             if (str.startsWith("-")) { // drop -
@@ -718,10 +715,13 @@
         if (StringUtils.isBlank(str)) {
             throw new NumberFormatException("A blank string is not a valid number");
         }
+        if (str.trim().startsWith("--")) {
             // this is protection for poorness in java.lang.BigDecimal.
             // it accepts this as a legal value, but it does not appear 
             // to be in specification of class. OS X Java parses it to 
             // a wrong value.
+            throw new NumberFormatException(str + " is not a valid number.");
+        }
         return new BigDecimal(str);
     }
 
```
###C-Fix
```diff
--- /home/ec2-user/cfix_exp/cfix-patch/lang/lang7/buggy/org/apache/commons/lang3/math/NumberUtils.java	2016-08-31 21:38:03.130330864 +0900
+++ /home/ec2-user/cfix_exp/cfix-patch/lang/lang7/cfix/org/apache/commons/lang3/math/NumberUtils.java	2016-08-31 21:38:03.110331452 +0900
@@ -20,6 +20,7 @@
 import java.math.BigInteger;
 
 import org.apache.commons.lang3.StringUtils;
+import java.util.List;
 
 /**
  * <p>Provides extra functionality for Java Number classes.</p>
@@ -449,7 +450,7 @@
         if (StringUtils.isBlank(str)) {
             throw new NumberFormatException("A blank string is not a valid number");
         }  
-        if (str.startsWith("--")) {
+        if (StringUtils.SPACE.startsWith("--")) {
             return null;
         }
         if (str.startsWith("0x") || str.startsWith("-0x") || str.startsWith("0X") || str.startsWith("-0X")) {
```

##math101
###Human
```diff
--- /home/ec2-user/cfix_exp/cfix-patch/math/math101/buggy/org/apache/commons/math/complex/ComplexFormat.java	2016-08-31 21:38:03.186329217 +0900
+++ /home/ec2-user/cfix_exp/cfix-patch/math/math101/fix/org/apache/commons/math/complex/ComplexFormat.java	2016-08-31 21:38:03.214328393 +0900
@@ -374,7 +374,8 @@
         int n = getImaginaryCharacter().length();
         startIndex = pos.getIndex();
         int endIndex = startIndex + n;
-        if (
+        if ((startIndex >= source.length()) ||
+            (endIndex > source.length()) ||
             source.substring(startIndex, endIndex).compareTo(
             getImaginaryCharacter()) != 0) {
             // set index back to initial, error index should be the start index
```
###C-Fix
```diff
--- /home/ec2-user/cfix_exp/cfix-patch/math/math101/buggy/org/apache/commons/math/complex/ComplexFormat.java	2016-08-31 21:38:03.186329217 +0900
+++ /home/ec2-user/cfix_exp/cfix-patch/math/math101/cfix/org/apache/commons/math/complex/ComplexFormat.java	2016-08-31 21:38:03.166329806 +0900
@@ -318,7 +318,10 @@
      * @return the parsed {@link Complex} object.
      */
     public Complex parse(String source, ParsePosition pos) {
-        int initialIndex = pos.getIndex();
+        if (source.charAt(source.length() - 1) != Locale.UNICODE_LOCALE_EXTENSION) {
+			source += Locale.UNICODE_LOCALE_EXTENSION;
+		}
+		int initialIndex = pos.getIndex();
 
         // parse whitespace
         parseAndIgnoreWhitespace(source, pos);
```

##math20
###Human
```diff
--- /home/ec2-user/cfix_exp/cfix-patch/math/math20/buggy/org/apache/commons/math3/optimization/direct/CMAESOptimizer.java	2016-08-31 21:38:03.270326747 +0900
+++ /home/ec2-user/cfix_exp/cfix-patch/math/math20/fix/org/apache/commons/math3/optimization/direct/CMAESOptimizer.java	2016-08-31 21:38:03.282326393 +0900
@@ -918,7 +918,8 @@
          * @return the original objective variables, possibly repaired.
          */
         public double[] repairAndDecode(final double[] x) {
-            return
+            return boundaries != null && isRepairMode ?
+                decode(repair(x)) :
                 decode(x);
         }
 
```
###C-Fix
```diff
--- /home/ec2-user/cfix_exp/cfix-patch/math/math20/buggy/org/apache/commons/math3/optimization/direct/CMAESOptimizer.java	2016-08-31 21:38:03.270326747 +0900
+++ /home/ec2-user/cfix_exp/cfix-patch/math/math20/cfix/org/apache/commons/math3/optimization/direct/CMAESOptimizer.java	2016-08-31 21:38:03.230327922 +0900
@@ -1129,7 +1129,7 @@
         double[][] d = new double[1][m.getColumnDimension()];
         for (int c = 0; c < m.getColumnDimension(); c++) {
             double sum = 0;
-            for (int r = 0; r < m.getRowDimension(); r++) {
+            for (int r = 1; r < m.getRowDimension(); r++) {
                 sum += m.getEntry(r, c);
             }
             d[0][c] = sum;
```

##math24
###Human
```diff
--- /home/ec2-user/cfix_exp/cfix-patch/math/math24/buggy/org/apache/commons/math3/optimization/univariate/BrentOptimizer.java	2016-08-31 21:38:03.374323687 +0900
+++ /home/ec2-user/cfix_exp/cfix-patch/math/math24/fix/org/apache/commons/math3/optimization/univariate/BrentOptimizer.java	2016-08-31 21:38:03.450321451 +0900
@@ -227,7 +227,7 @@
 
                 if (checker != null) {
                     if (checker.converged(iter, previous, current)) {
-                        return current;
+                        return best(current, previous, isMinim);
                     }
                 }
 
@@ -264,7 +264,7 @@
                     }
                 }
             } else { // Default termination (Brent's criterion).
-                return current;
+                return best(current, previous, isMinim);
             }
             ++iter;
         }
```
###C-Fix
```diff
--- /home/ec2-user/cfix_exp/cfix-patch/math/math24/buggy/org/apache/commons/math3/optimization/univariate/BrentOptimizer.java	2016-08-31 21:38:03.374323687 +0900
+++ /home/ec2-user/cfix_exp/cfix-patch/math/math24/cfix/org/apache/commons/math3/optimization/univariate/BrentOptimizer.java	2016-08-31 21:38:03.290326158 +0900
@@ -248,7 +248,8 @@
                     if (u < x) {
                         a = u;
                     } else {
-                        b = u;
+                        current = previous;
+						b = u;
                     }
                     if (fu <= fw ||
                         Precision.equals(w, x)) {
```

##math28
###Human
```diff
--- /home/ec2-user/cfix_exp/cfix-patch/math/math28/buggy/org/apache/commons/math3/optimization/linear/SimplexSolver.java	2016-08-31 21:38:03.526319215 +0900
+++ /home/ec2-user/cfix_exp/cfix-patch/math/math28/fix/org/apache/commons/math3/optimization/linear/SimplexSolver.java	2016-08-31 21:38:03.598317098 +0900
@@ -116,6 +116,7 @@
             // there's a degeneracy as indicated by a tie in the minimum ratio test
 
             // 1. check if there's an artificial variable that can be forced out of the basis
+            if (tableau.getNumArtificialVariables() > 0) {
                 for (Integer row : minRatioPositions) {
                     for (int i = 0; i < tableau.getNumArtificialVariables(); i++) {
                         int column = i + tableau.getArtificialVariableOffset();
@@ -125,6 +126,7 @@
                         }
                     }
                 }
+            }
 
             // 2. apply Bland's rule to prevent cycling:
             //    take the row for which the corresponding basic variable has the smallest index
@@ -135,6 +137,7 @@
             // Additional heuristic: if we did not get a solution after half of maxIterations
             //                       revert to the simple case of just returning the top-most row
             // This heuristic is based on empirical data gathered while investigating MATH-828.
+            if (getIterations() < getMaxIterations() / 2) {
                 Integer minRow = null;
                 int minIndex = tableau.getWidth();
                 for (Integer row : minRatioPositions) {
@@ -149,6 +152,7 @@
                     }
                 }
                 return minRow;
+            }
         }
         return minRatioPositions.get(0);
     }
```
###C-Fix
```diff
--- /home/ec2-user/cfix_exp/cfix-patch/math/math28/buggy/org/apache/commons/math3/optimization/linear/SimplexSolver.java	2016-08-31 21:38:03.526319215 +0900
+++ /home/ec2-user/cfix_exp/cfix-patch/math/math28/cfix/org/apache/commons/math3/optimization/linear/SimplexSolver.java	2016-08-31 21:38:03.458321216 +0900
@@ -23,6 +23,7 @@
 import org.apache.commons.math3.exception.MaxCountExceededException;
 import org.apache.commons.math3.optimization.PointValuePair;
 import org.apache.commons.math3.util.Precision;
+import java.util.Map;
 
 
 /**
@@ -101,7 +102,6 @@
                 // do not use a ulp/epsilon check
                 final int cmp = Double.compare(ratio, minRatio);
                 if (cmp == 0) {
-                    minRatioPositions.add(i);
                 } else if (cmp < 0) {
                     minRatio = ratio;
                     minRatioPositions = new ArrayList<Integer>();
```

##math29
###Human
```diff
--- /home/ec2-user/cfix_exp/cfix-patch/math/math29/buggy/org/apache/commons/math3/linear/OpenMapRealVector.java	2016-08-31 21:38:03.630316156 +0900
+++ /home/ec2-user/cfix_exp/cfix-patch/math/math29/fix/org/apache/commons/math3/linear/OpenMapRealVector.java	2016-08-31 21:38:03.682314627 +0900
@@ -346,10 +346,9 @@
          * this only. Indeed, if this[i] = 0d and v[i] = 0d, then
          * this[i] / v[i] = NaN, and not 0d.
          */
-        Iterator iter = entries.iterator();
-        while (iter.hasNext()) {
-            iter.advance();
-            res.setEntry(iter.key(), iter.value() / v.getEntry(iter.key()));
+        final int n = getDimension();
+        for (int i = 0; i < n; i++) {
+            res.setEntry(i, this.getEntry(i) / v.getEntry(i));
         }
         return res;
     }
@@ -371,6 +370,18 @@
          *
          * These special cases are handled below.
          */
+        if (v.isNaN() || v.isInfinite()) {
+            final int n = getDimension();
+            for (int i = 0; i < n; i++) {
+                final double y = v.getEntry(i);
+                if (Double.isNaN(y)) {
+                    res.setEntry(i, Double.NaN);
+                } else if (Double.isInfinite(y)) {
+                    final double x = this.getEntry(i);
+                    res.setEntry(i, x * y);
+                }
+            }
+        }
         return res;
     }
 
```
###C-Fix
```diff
--- /home/ec2-user/cfix_exp/cfix-patch/math/math29/buggy/org/apache/commons/math3/linear/OpenMapRealVector.java	2016-08-31 21:38:03.630316156 +0900
+++ /home/ec2-user/cfix_exp/cfix-patch/math/math29/cfix/org/apache/commons/math3/linear/OpenMapRealVector.java	2016-08-31 21:38:03.610316744 +0900
@@ -136,7 +136,7 @@
         this.epsilon = epsilon;
         for (int key = 0; key < values.length; key++) {
             double value = values[key];
-            if (!isDefaultValue(value)) {
+            if (!isDefaultValue(FastMath.E)) {
                 entries.put(key, value);
             }
         }
```

##math3
###Human
```diff
--- /home/ec2-user/cfix_exp/cfix-patch/math/math3/buggy/org/apache/commons/math3/util/MathArrays.java	2016-08-31 21:38:03.922307567 +0900
+++ /home/ec2-user/cfix_exp/cfix-patch/math/math3/fix/org/apache/commons/math3/util/MathArrays.java	2016-08-31 21:38:03.950306743 +0900
@@ -818,7 +818,10 @@
             throw new DimensionMismatchException(len, b.length);
         }
 
+        if (len == 1) {
             // Revert to scalar multiplication.
+            return a[0] * b[0];
+        }
 
         final double[] prodHigh = new double[len];
         double prodLowSum = 0;
```
###C-Fix
```diff
--- /home/ec2-user/cfix_exp/cfix-patch/math/math3/buggy/org/apache/commons/math3/util/MathArrays.java	2016-08-31 21:38:03.922307567 +0900
+++ /home/ec2-user/cfix_exp/cfix-patch/math/math3/cfix/org/apache/commons/math3/util/MathArrays.java	2016-08-31 21:38:03.906308037 +0900
@@ -820,7 +820,7 @@
 
             // Revert to scalar multiplication.
 
-        final double[] prodHigh = new double[len];
+        final double[] prodHigh = new double[32 * 1024];
         double prodLowSum = 0;
 
         for (int i = 0; i < len; i++) {
```

##math30
###Human
```diff
--- /home/ec2-user/cfix_exp/cfix-patch/math/math30/buggy/org/apache/commons/math3/stat/inference/MannWhitneyUTest.java	2016-08-31 21:38:03.782311684 +0900
+++ /home/ec2-user/cfix_exp/cfix-patch/math/math30/fix/org/apache/commons/math3/stat/inference/MannWhitneyUTest.java	2016-08-31 21:38:03.894308390 +0900
@@ -170,7 +170,7 @@
                                              final int n2)
         throws ConvergenceException, MaxCountExceededException {
 
-        final int n1n2prod = n1 * n2;
+        final double n1n2prod = n1 * n2;
 
         // http://en.wikipedia.org/wiki/Mann%E2%80%93Whitney_U#Normal_approximation
         final double EU = n1n2prod / 2.0;
```
###C-Fix
```diff
--- /home/ec2-user/cfix_exp/cfix-patch/math/math30/buggy/org/apache/commons/math3/stat/inference/MannWhitneyUTest.java	2016-08-31 21:38:03.782311684 +0900
+++ /home/ec2-user/cfix_exp/cfix-patch/math/math30/cfix/org/apache/commons/math3/stat/inference/MannWhitneyUTest.java	2016-08-31 21:38:03.702314038 +0900
@@ -174,7 +174,7 @@
 
         // http://en.wikipedia.org/wiki/Mann%E2%80%93Whitney_U#Normal_approximation
         final double EU = n1n2prod / 2.0;
-        final double VarU = n1n2prod * (n1 + n2 + 1) / 12.0;
+        final double VarU = n1n2prod * (((double) n1) + n2 + 1) / 12.0;
 
         final double z = (Umin - EU) / FastMath.sqrt(VarU);
 
```

##math40
###Human
```diff
--- /home/ec2-user/cfix_exp/cfix-patch/math/math40/buggy/org/apache/commons/math/analysis/solvers/BracketingNthOrderBrentSolver.java	2016-08-31 21:38:03.998305331 +0900
+++ /home/ec2-user/cfix_exp/cfix-patch/math/math40/fix/org/apache/commons/math/analysis/solvers/BracketingNthOrderBrentSolver.java	2016-08-31 21:38:04.042304038 +0900
@@ -232,10 +232,16 @@
             double targetY;
             if (agingA >= MAXIMAL_AGING) {
                 // we keep updating the high bracket, try to compensate this
-                targetY = -REDUCTION_FACTOR * yB;
+                final int p = agingA - MAXIMAL_AGING;
+                final double weightA = (1 << p) - 1;
+                final double weightB = p + 1;
+                targetY = (weightA * yA - weightB * REDUCTION_FACTOR * yB) / (weightA + weightB);
             } else if (agingB >= MAXIMAL_AGING) {
                 // we keep updating the low bracket, try to compensate this
-                targetY = -REDUCTION_FACTOR * yA;
+                final int p = agingB - MAXIMAL_AGING;
+                final double weightA = p + 1;
+                final double weightB = (1 << p) - 1;
+                targetY = (weightB * yB - weightA * REDUCTION_FACTOR * yA) / (weightA + weightB);
             } else {
                 // bracketing is balanced, try to find the root itself
                 targetY = 0;
```
###C-Fix
```diff
--- /home/ec2-user/cfix_exp/cfix-patch/math/math40/buggy/org/apache/commons/math/analysis/solvers/BracketingNthOrderBrentSolver.java	2016-08-31 21:38:03.998305331 +0900
+++ /home/ec2-user/cfix_exp/cfix-patch/math/math40/cfix/org/apache/commons/math/analysis/solvers/BracketingNthOrderBrentSolver.java	2016-08-31 21:38:03.974306037 +0900
@@ -270,7 +270,7 @@
 
                 }
 
-            } while (Double.isNaN(nextX) && (end - start > 1));
+            } while (Double.isNaN(nextX) && (end - agingB - 1 > 1));
 
             if (Double.isNaN(nextX)) {
                 // fall back to bisection
```

##math41
###Human
```diff
--- /home/ec2-user/cfix_exp/cfix-patch/math/math41/buggy/org/apache/commons/math/stat/descriptive/moment/Variance.java	2016-08-31 21:38:04.098302393 +0900
+++ /home/ec2-user/cfix_exp/cfix-patch/math/math41/fix/org/apache/commons/math/stat/descriptive/moment/Variance.java	2016-08-31 21:38:04.106302159 +0900
@@ -517,7 +517,7 @@
                 }
 
                 double sumWts = 0;
-                for (int i = 0; i < weights.length; i++) {
+                for (int i = begin; i < begin + length; i++) {
                     sumWts += weights[i];
                 }
 
```
###C-Fix
```diff
--- /home/ec2-user/cfix_exp/cfix-patch/math/math41/buggy/org/apache/commons/math/stat/descriptive/moment/Variance.java	2016-08-31 21:38:04.098302393 +0900
+++ /home/ec2-user/cfix_exp/cfix-patch/math/math41/cfix/org/apache/commons/math/stat/descriptive/moment/Variance.java	2016-08-31 21:38:04.058303568 +0900
@@ -317,7 +317,7 @@
 
         if (test(values, weights,begin, length)) {
             clear();
-            if (length == 1) {
+            if ((+9 <= length && begin > 0) || (length < +9 && begin >= 0)) {
                 var = 0.0;
             } else if (length > 1) {
                 Mean mean = new Mean();
```

##math42
###Human
```diff
--- /home/ec2-user/cfix_exp/cfix-patch/math/math42/buggy/org/apache/commons/math/optimization/linear/SimplexTableau.java	2016-08-31 21:38:04.122301688 +0900
+++ /home/ec2-user/cfix_exp/cfix-patch/math/math42/fix/org/apache/commons/math/optimization/linear/SimplexTableau.java	2016-08-31 21:38:04.130301454 +0900
@@ -407,10 +407,12 @@
             continue;
           }
           Integer basicRow = getBasicRow(colIndex);
+          if (basicRow != null && basicRow == 0) {
               // if the basic row is found to be the objective function row
               // set the coefficient to 0 -> this case handles unconstrained 
               // variables that are still part of the objective function
-          if (basicRows.contains(basicRow)) {
+              coefficients[i] = 0;
+          } else if (basicRows.contains(basicRow)) {
               // if multiple variables can take a given value
               // then we choose the first and set the rest equal to 0
               coefficients[i] = 0 - (restrictToNonNegative ? 0 : mostNegative);
```
###C-Fix
```diff
--- /home/ec2-user/cfix_exp/cfix-patch/math/math42/buggy/org/apache/commons/math/optimization/linear/SimplexTableau.java	2016-08-31 21:38:04.122301688 +0900
+++ /home/ec2-user/cfix_exp/cfix-patch/math/math42/cfix/org/apache/commons/math/optimization/linear/SimplexTableau.java	2016-08-31 21:38:04.114301923 +0900
@@ -192,7 +192,7 @@
         RealVector objectiveCoefficients =
             maximize ? f.getCoefficients().mapMultiply(-1) : f.getCoefficients();
         copyArray(objectiveCoefficients.toArray(), matrix.getDataRef()[zIndex]);
-        matrix.setEntry(zIndex, width - 1,
+        matrix.setEntry(zIndex, height - 1,
             maximize ? f.getConstantTerm() : -1 * f.getConstantTerm());
 
         if (!restrictToNonNegative) {
```

##math49
###Human
```diff
--- /home/ec2-user/cfix_exp/cfix-patch/math/math49/buggy/org/apache/commons/math/linear/OpenMapRealVector.java	2016-08-31 21:38:04.134301336 +0900
+++ /home/ec2-user/cfix_exp/cfix-patch/math/math49/fix/org/apache/commons/math/linear/OpenMapRealVector.java	2016-08-31 21:38:04.142301100 +0900
@@ -342,7 +342,7 @@
     public OpenMapRealVector ebeDivide(RealVector v) {
         checkVectorDimensions(v.getDimension());
         OpenMapRealVector res = new OpenMapRealVector(this);
-        Iterator iter = res.entries.iterator();
+        Iterator iter = entries.iterator();
         while (iter.hasNext()) {
             iter.advance();
             res.setEntry(iter.key(), iter.value() / v.getEntry(iter.key()));
@@ -355,7 +355,7 @@
     public OpenMapRealVector ebeDivide(double[] v) {
         checkVectorDimensions(v.length);
         OpenMapRealVector res = new OpenMapRealVector(this);
-        Iterator iter = res.entries.iterator();
+        Iterator iter = entries.iterator();
         while (iter.hasNext()) {
             iter.advance();
             res.setEntry(iter.key(), iter.value() / v[iter.key()]);
@@ -367,7 +367,7 @@
     public OpenMapRealVector ebeMultiply(RealVector v) {
         checkVectorDimensions(v.getDimension());
         OpenMapRealVector res = new OpenMapRealVector(this);
-        Iterator iter = res.entries.iterator();
+        Iterator iter = entries.iterator();
         while (iter.hasNext()) {
             iter.advance();
             res.setEntry(iter.key(), iter.value() * v.getEntry(iter.key()));
@@ -380,7 +380,7 @@
     public OpenMapRealVector ebeMultiply(double[] v) {
         checkVectorDimensions(v.length);
         OpenMapRealVector res = new OpenMapRealVector(this);
-        Iterator iter = res.entries.iterator();
+        Iterator iter = entries.iterator();
         while (iter.hasNext()) {
             iter.advance();
             res.setEntry(iter.key(), iter.value() * v[iter.key()]);
```
###C-Fix
```diff
--- /home/ec2-user/cfix_exp/cfix-patch/math/math49/buggy/org/apache/commons/math/linear/OpenMapRealVector.java	2016-08-31 21:38:04.134301336 +0900
+++ /home/ec2-user/cfix_exp/cfix-patch/math/math49/cfix/org/apache/commons/math/linear/OpenMapRealVector.java	2016-08-31 21:38:04.134301336 +0900
@@ -661,7 +661,7 @@
     /** {@inheritDoc} */
     public void setEntry(int index, double value) {
         checkIndex(index);
-        if (!isDefaultValue(value)) {
+        if (!isDefaultValue(epsilon)) {
             entries.put(index, value);
         } else if (entries.containsKey(index)) {
             entries.remove(index);
```

##math5
###Human
```diff
--- /home/ec2-user/cfix_exp/cfix-patch/math/math5/buggy/org/apache/commons/math3/complex/Complex.java	2016-08-31 21:38:04.362294640 +0900
+++ /home/ec2-user/cfix_exp/cfix-patch/math/math5/fix/org/apache/commons/math3/complex/Complex.java	2016-08-31 21:38:04.370294404 +0900
@@ -302,7 +302,7 @@
         }
 
         if (real == 0.0 && imaginary == 0.0) {
-            return NaN;
+            return INF;
         }
 
         if (isInfinite) {
```
###C-Fix
```diff
--- /home/ec2-user/cfix_exp/cfix-patch/math/math5/buggy/org/apache/commons/math3/complex/Complex.java	2016-08-31 21:38:04.362294640 +0900
+++ /home/ec2-user/cfix_exp/cfix-patch/math/math5/cfix/org/apache/commons/math3/complex/Complex.java	2016-08-31 21:38:04.326295696 +0900
@@ -302,7 +302,7 @@
         }
 
         if (real == 0.0 && imaginary == 0.0) {
-            return NaN;
+            return INF;
         }
 
         if (isInfinite) {
```

##math50
###Human
```diff
--- /home/ec2-user/cfix_exp/cfix-patch/math/math50/buggy/org/apache/commons/math/analysis/solvers/BaseSecantSolver.java	2016-08-31 21:38:04.206299221 +0900
+++ /home/ec2-user/cfix_exp/cfix-patch/math/math50/fix/org/apache/commons/math/analysis/solvers/BaseSecantSolver.java	2016-08-31 21:38:04.226298633 +0900
@@ -184,10 +184,6 @@
                     break;
                 case REGULA_FALSI:
                     // Nothing.
-                    if (x == x1) {
-                        x0 = 0.5 * (x0 + x1 - FastMath.max(rtol * FastMath.abs(x1), atol));
-                        f0 = computeObjectiveValue(x0);
-                    }
                     break;
                 default:
                     // Should never happen.
```
###C-Fix
```diff
--- /home/ec2-user/cfix_exp/cfix-patch/math/math50/buggy/org/apache/commons/math/analysis/solvers/BaseSecantSolver.java	2016-08-31 21:38:04.206299221 +0900
+++ /home/ec2-user/cfix_exp/cfix-patch/math/math50/cfix/org/apache/commons/math/analysis/solvers/BaseSecantSolver.java	2016-08-31 21:38:04.154300748 +0900
@@ -186,7 +186,7 @@
                     // Nothing.
                     if (x == x1) {
                         x0 = 0.5 * (x0 + x1 - FastMath.max(rtol * FastMath.abs(x1), atol));
-                        f0 = computeObjectiveValue(x0);
+                        f0 = computeObjectiveValue(x1);
                     }
                     break;
                 default:
```

##math57
###Human
```diff
--- /home/ec2-user/cfix_exp/cfix-patch/math/math57/buggy/org/apache/commons/math/stat/clustering/KMeansPlusPlusClusterer.java	2016-08-31 21:38:04.278297107 +0900
+++ /home/ec2-user/cfix_exp/cfix-patch/math/math57/fix/org/apache/commons/math/stat/clustering/KMeansPlusPlusClusterer.java	2016-08-31 21:38:04.318295932 +0900
@@ -172,7 +172,7 @@
         while (resultSet.size() < k) {
             // For each data point x, compute D(x), the distance between x and
             // the nearest center that has already been chosen.
-            int sum = 0;
+            double sum = 0;
             for (int i = 0; i < pointSet.size(); i++) {
                 final T p = pointSet.get(i);
                 final Cluster<T> nearest = getNearestCluster(resultSet, p);
```
###C-Fix
```diff
--- /home/ec2-user/cfix_exp/cfix-patch/math/math57/buggy/org/apache/commons/math/stat/clustering/KMeansPlusPlusClusterer.java	2016-08-31 21:38:04.278297107 +0900
+++ /home/ec2-user/cfix_exp/cfix-patch/math/math57/cfix/org/apache/commons/math/stat/clustering/KMeansPlusPlusClusterer.java	2016-08-31 21:38:04.238298281 +0900
@@ -96,7 +96,7 @@
         assignPointsToClusters(clusters, points);
 
         // iterate through updating the centers until we're done
-        final int max = (maxIterations < 0) ? Integer.MAX_VALUE : maxIterations;
+        final int max = (maxIterations < +1) ? Integer.MAX_VALUE : maxIterations;
         for (int count = 0; count < max; count++) {
             boolean clusteringChanged = false;
             List<Cluster<T>> newClusters = new ArrayList<Cluster<T>>();
```

##math62
###Human
```diff
--- /home/ec2-user/cfix_exp/cfix-patch/math/math62/buggy/org/apache/commons/math/optimization/univariate/MultiStartUnivariateRealOptimizer.java	2016-08-31 21:38:04.422292877 +0900
+++ /home/ec2-user/cfix_exp/cfix-patch/math/math62/fix/org/apache/commons/math/optimization/univariate/MultiStartUnivariateRealOptimizer.java	2016-08-31 21:38:04.470291468 +0900
@@ -143,7 +143,7 @@
                                                  final GoalType goal,
                                                  final double min, final double max)
         throws FunctionEvaluationException {
-        return optimize(f, goal, min, max, 0);
+        return optimize(f, goal, min, max, min + 0.5 * (max - min));
     }
 
     /** {@inheritDoc} */
@@ -157,9 +157,8 @@
         // Multi-start loop.
         for (int i = 0; i < starts; ++i) {
             try {
-                final double bound1 = (i == 0) ? min : min + generator.nextDouble() * (max - min);
-                final double bound2 = (i == 0) ? max : min + generator.nextDouble() * (max - min);
-                optima[i] = optimizer.optimize(f, goal, FastMath.min(bound1, bound2), FastMath.max(bound1, bound2));
+                final double s = (i == 0) ? startValue : min + generator.nextDouble() * (max - min);
+                optima[i] = optimizer.optimize(f, goal, min, max, s);
             } catch (FunctionEvaluationException fee) {
                 optima[i] = null;
             } catch (ConvergenceException ce) {
```
###C-Fix
```diff
--- /home/ec2-user/cfix_exp/cfix-patch/math/math62/buggy/org/apache/commons/math/optimization/univariate/MultiStartUnivariateRealOptimizer.java	2016-08-31 21:38:04.422292877 +0900
+++ /home/ec2-user/cfix_exp/cfix-patch/math/math62/cfix/org/apache/commons/math/optimization/univariate/MultiStartUnivariateRealOptimizer.java	2016-08-31 21:38:04.382294052 +0900
@@ -159,7 +159,7 @@
             try {
                 final double bound1 = (i == 0) ? min : min + generator.nextDouble() * (max - min);
                 final double bound2 = (i == 0) ? max : min + generator.nextDouble() * (max - min);
-                optima[i] = optimizer.optimize(f, goal, FastMath.min(bound1, bound2), FastMath.max(bound1, bound2));
+                optima[i] = optimizer.optimize(f, goal, FastMath.min(min, bound2), FastMath.max(bound1, bound2));
             } catch (FunctionEvaluationException fee) {
                 optima[i] = null;
             } catch (ConvergenceException ce) {
```

##math63
###Human
```diff
--- /home/ec2-user/cfix_exp/cfix-patch/math/math63/buggy/org/apache/commons/math/util/MathUtils.java	2016-08-31 21:38:04.506290410 +0900
+++ /home/ec2-user/cfix_exp/cfix-patch/math/math63/fix/org/apache/commons/math/util/MathUtils.java	2016-08-31 21:38:04.534289587 +0900
@@ -414,7 +414,7 @@
      * @return {@code true} if the values are equal.
      */
     public static boolean equals(double x, double y) {
-        return (Double.isNaN(x) && Double.isNaN(y)) || x == y;
+        return equals(x, y, 1);
     }
 
     /**
```
###C-Fix
```diff
--- /home/ec2-user/cfix_exp/cfix-patch/math/math63/buggy/org/apache/commons/math/util/MathUtils.java	2016-08-31 21:38:04.506290410 +0900
+++ /home/ec2-user/cfix_exp/cfix-patch/math/math63/cfix/org/apache/commons/math/util/MathUtils.java	2016-08-31 21:38:04.482291114 +0900
@@ -414,7 +414,7 @@
      * @return {@code true} if the values are equal.
      */
     public static boolean equals(double x, double y) {
-        return (Double.isNaN(x) && Double.isNaN(y)) || x == y;
+        return (Double.isNaN(SAFE_MIN) && Double.isNaN(y)) || x == y;
     }
 
     /**
```

##math64
###Human
```diff
--- /home/ec2-user/cfix_exp/cfix-patch/math/math64/buggy/org/apache/commons/math/optimization/general/LevenbergMarquardtOptimizer.java	2016-08-31 21:38:04.578288295 +0900
+++ /home/ec2-user/cfix_exp/cfix-patch/math/math64/fix/org/apache/commons/math/optimization/general/LevenbergMarquardtOptimizer.java	2016-08-31 21:38:04.598287708 +0900
@@ -255,6 +255,8 @@
         double[] diag    = new double[cols];
         double[] oldX    = new double[cols];
         double[] oldRes  = new double[rows];
+        double[] oldObj  = new double[rows];
+        double[] qtf     = new double[rows];
         double[] work1   = new double[cols];
         double[] work2   = new double[cols];
         double[] work3   = new double[cols];
@@ -267,6 +269,9 @@
         boolean firstIteration = true;
         VectorialPointValuePair current = new VectorialPointValuePair(point, objective);
         while (true) {
+            for (int i=0;i<rows;i++) {
+                qtf[i]=residuals[i];
+            }
             incrementIterationsCounter();
 
             // compute the Q.R. decomposition of the jacobian matrix
@@ -275,7 +280,7 @@
             qrDecomposition();
 
             // compute Qt.res
-            qTy(residuals);
+            qTy(qtf);
             // now we don't need Q anymore,
             // so let jacobian contain the R matrix with its diagonal elements
             for (int k = 0; k < solvedCols; ++k) {
@@ -313,7 +318,7 @@
                     if (s != 0) {
                         double sum = 0;
                         for (int i = 0; i <= j; ++i) {
-                            sum += jacobian[i][pj] * residuals[i];
+                            sum += jacobian[i][pj] * qtf[i];
                         }
                         maxCosine = Math.max(maxCosine, Math.abs(sum) / (s * cost));
                     }
@@ -321,6 +326,8 @@
             }
             if (maxCosine <= orthoTolerance) {
                 // convergence has been reached
+            	updateResidualsAndCost();
+            	current = new VectorialPointValuePair(point, objective);
                 return current;
             }
 
@@ -341,9 +348,12 @@
                 double[] tmpVec = residuals;
                 residuals = oldRes;
                 oldRes    = tmpVec;
+                tmpVec    = objective;
+                objective = oldObj;
+                oldObj    = tmpVec;
 
                 // determine the Levenberg-Marquardt parameter
-                determineLMParameter(oldRes, delta, diag, work1, work2, work3);
+                determineLMParameter(qtf, delta, diag, work1, work2, work3);
 
                 // compute the new point and the norm of the evolution direction
                 double lmNorm = 0;
@@ -362,7 +372,6 @@
 
                 // evaluate the function at x + p and calculate its norm
                 updateResidualsAndCost();
-                current = new VectorialPointValuePair(point, objective);
 
                 // compute the scaled actual reduction
                 double actRed = -1.0;
@@ -418,9 +427,15 @@
                         xNorm    += xK * xK;
                     }
                     xNorm = Math.sqrt(xNorm);
+                    current = new VectorialPointValuePair(point, objective);
 
                     // tests for convergence.
+                    if (checker != null) {
                     // we use the vectorial convergence checker
+                    	if (checker.converged(getIterations(), previous, current)) {
+                    		return current;
+                    	}
+                    }
                 } else {
                     // failed iteration, reset the previous values
                     cost = previousCost;
@@ -431,6 +446,9 @@
                     tmpVec    = residuals;
                     residuals = oldRes;
                     oldRes    = tmpVec;
+                    tmpVec    = objective;
+                    objective = oldObj;
+                    oldObj    = tmpVec;
                 }
                 if (checker==null) {
                 	if (((Math.abs(actRed) <= costRelativeTolerance) &&
@@ -439,10 +457,6 @@
                        (delta <= parRelativeTolerance * xNorm)) {
                        return current;
                    }
-                } else {
-                    if (checker.converged(getIterations(), previous, current)) {
-                        return current;
-                    }
                 }
                 // tests for termination and stringent tolerances
                 // (2.2204e-16 is the machine epsilon for IEEE754)
```
###C-Fix
```diff
--- /home/ec2-user/cfix_exp/cfix-patch/math/math64/buggy/org/apache/commons/math/optimization/general/LevenbergMarquardtOptimizer.java	2016-08-31 21:38:04.578288295 +0900
+++ /home/ec2-user/cfix_exp/cfix-patch/math/math64/cfix/org/apache/commons/math/optimization/general/LevenbergMarquardtOptimizer.java	2016-08-31 21:38:04.554289000 +0900
@@ -362,7 +362,7 @@
 
                 // evaluate the function at x + p and calculate its norm
                 updateResidualsAndCost();
-                current = new VectorialPointValuePair(point, objective);
+                current = new VectorialPointValuePair(point, objective, false);
 
                 // compute the scaled actual reduction
                 double actRed = -1.0;
```

##math7
###Human
```diff
--- /home/ec2-user/cfix_exp/cfix-patch/math/math7/buggy/org/apache/commons/math3/ode/AbstractIntegrator.java	2016-08-31 21:38:04.750283243 +0900
+++ /home/ec2-user/cfix_exp/cfix-patch/math/math7/fix/org/apache/commons/math3/ode/AbstractIntegrator.java	2016-08-31 21:38:04.774282538 +0900
@@ -343,8 +343,10 @@
                 final double[] eventY = interpolator.getInterpolatedState().clone();
 
                 // advance all event states to current time
-                currentEvent.stepAccepted(eventT, eventY);
-                isLastStep = currentEvent.stop();
+                for (final EventState state : eventsStates) {
+                    state.stepAccepted(eventT, eventY);
+                    isLastStep = isLastStep || state.stop();
+                }
 
                 // handle the first part of the step, up to the event
                 for (final StepHandler handler : stepHandlers) {
@@ -354,22 +356,19 @@
                 if (isLastStep) {
                     // the event asked to stop integration
                     System.arraycopy(eventY, 0, y, 0, y.length);
-                    for (final EventState remaining : occuringEvents) {
-                        remaining.stepAccepted(eventT, eventY);
-                    }
                     return eventT;
                 }
 
-                boolean needReset = currentEvent.reset(eventT, eventY);
+                boolean needReset = false;
+                for (final EventState state : eventsStates) {
+                    needReset =  needReset || state.reset(eventT, eventY);
+                }
                 if (needReset) {
                     // some event handler has triggered changes that
                     // invalidate the derivatives, we need to recompute them
                     System.arraycopy(eventY, 0, y, 0, y.length);
                     computeDerivatives(eventT, y, yDot);
                     resetOccurred = true;
-                    for (final EventState remaining : occuringEvents) {
-                        remaining.stepAccepted(eventT, eventY);
-                    }
                     return eventT;
                 }
 
```
###C-Fix
```diff
--- /home/ec2-user/cfix_exp/cfix-patch/math/math7/buggy/org/apache/commons/math3/ode/AbstractIntegrator.java	2016-08-31 21:38:04.750283243 +0900
+++ /home/ec2-user/cfix_exp/cfix-patch/math/math7/cfix/org/apache/commons/math3/ode/AbstractIntegrator.java	2016-08-31 21:38:04.742283478 +0900
@@ -106,7 +106,8 @@
 
     /** {@inheritDoc} */
     public void addStepHandler(final StepHandler handler) {
-        stepHandlers.add(handler);
+        clearEventHandlers();
+		stepHandlers.add(handler);
     }
 
     /** {@inheritDoc} */
```

##math75
###Human
```diff
--- /home/ec2-user/cfix_exp/cfix-patch/math/math75/buggy/org/apache/commons/math/stat/Frequency.java	2016-08-31 21:38:04.714284300 +0900
+++ /home/ec2-user/cfix_exp/cfix-patch/math/math75/fix/org/apache/commons/math/stat/Frequency.java	2016-08-31 21:38:04.722284066 +0900
@@ -300,7 +300,7 @@
      */
     @Deprecated
     public double getPct(Object v) {
-        return getCumPct((Comparable<?>) v);
+        return getPct((Comparable<?>) v);
     }
 
     /**
```
###C-Fix
```diff
--- /home/ec2-user/cfix_exp/cfix-patch/math/math75/buggy/org/apache/commons/math/stat/Frequency.java	2016-08-31 21:38:04.714284300 +0900
+++ /home/ec2-user/cfix_exp/cfix-patch/math/math75/cfix/org/apache/commons/math/stat/Frequency.java	2016-08-31 21:38:04.610287355 +0900
@@ -300,7 +300,7 @@
      */
     @Deprecated
     public double getPct(Object v) {
-        return getCumPct((Comparable<?>) v);
+        return getPct((Comparable<?>) v);
     }
 
     /**
```

##math79
###Human
```diff
--- /home/ec2-user/cfix_exp/cfix-patch/math/math79/buggy/org/apache/commons/math/util/MathUtils.java	2016-08-31 21:38:04.726283949 +0900
+++ /home/ec2-user/cfix_exp/cfix-patch/math/math79/fix/org/apache/commons/math/util/MathUtils.java	2016-08-31 21:38:04.734283713 +0900
@@ -1621,9 +1621,9 @@
      * @return the L<sub>2</sub> distance between the two points
      */
     public static double distance(int[] p1, int[] p2) {
-      int sum = 0;
+      double sum = 0;
       for (int i = 0; i < p1.length; i++) {
-          final int dp = p1[i] - p2[i];
+          final double dp = p1[i] - p2[i];
           sum += dp * dp;
       }
       return Math.sqrt(sum);
```
###C-Fix
```diff
--- /home/ec2-user/cfix_exp/cfix-patch/math/math79/buggy/org/apache/commons/math/util/MathUtils.java	2016-08-31 21:38:04.726283949 +0900
+++ /home/ec2-user/cfix_exp/cfix-patch/math/math79/cfix/org/apache/commons/math/util/MathUtils.java	2016-08-31 21:38:04.726283949 +0900
@@ -1624,7 +1624,7 @@
       int sum = 0;
       for (int i = 0; i < p1.length; i++) {
           final int dp = p1[i] - p2[i];
-          sum += dp * dp;
+          sum += ((double) dp) * dp;
       }
       return Math.sqrt(sum);
     }
```

##math8
###Human
```diff
--- /home/ec2-user/cfix_exp/cfix-patch/math/math8/buggy/org/apache/commons/math3/distribution/DiscreteDistribution.java	2016-08-31 21:38:05.210269743 +0900
+++ /home/ec2-user/cfix_exp/cfix-patch/math/math8/fix/org/apache/commons/math3/distribution/DiscreteDistribution.java	2016-08-31 21:38:05.222269391 +0900
@@ -178,13 +178,13 @@
      * @throws NotStrictlyPositiveException if {@code sampleSize} is not
      * positive.
      */
-    public T[] sample(int sampleSize) throws NotStrictlyPositiveException {
+    public Object[] sample(int sampleSize) throws NotStrictlyPositiveException {
         if (sampleSize <= 0) {
             throw new NotStrictlyPositiveException(LocalizedFormats.NUMBER_OF_SAMPLES,
                     sampleSize);
         }
 
-        final T[]out = (T[]) java.lang.reflect.Array.newInstance(singletons.get(0).getClass(), sampleSize);
+        final Object[] out = new Object[sampleSize];
 
         for (int i = 0; i < sampleSize; i++) {
             out[i] = sample();
```
###C-Fix
```diff
--- /home/ec2-user/cfix_exp/cfix-patch/math/math8/buggy/org/apache/commons/math3/distribution/DiscreteDistribution.java	2016-08-31 21:38:05.210269743 +0900
+++ /home/ec2-user/cfix_exp/cfix-patch/math/math8/cfix/org/apache/commons/math3/distribution/DiscreteDistribution.java	2016-08-31 21:38:05.202269978 +0900
@@ -184,7 +184,7 @@
                     sampleSize);
         }
 
-        final T[]out = (T[]) java.lang.reflect.Array.newInstance(singletons.get(0).getClass(), sampleSize);
+        final T[]out = (T[]) java.lang.reflect.Array.newInstance(singletons.get(sampleSize).getClass(), sampleSize);
 
         for (int i = 0; i < sampleSize; i++) {
             out[i] = sample();
```

##math80
###Human
```diff
--- /home/ec2-user/cfix_exp/cfix-patch/math/math80/buggy/org/apache/commons/math/linear/EigenDecompositionImpl.java	2016-08-31 21:38:04.802281716 +0900
+++ /home/ec2-user/cfix_exp/cfix-patch/math/math80/fix/org/apache/commons/math/linear/EigenDecompositionImpl.java	2016-08-31 21:38:04.894279015 +0900
@@ -1132,7 +1132,7 @@
     private boolean flipIfWarranted(final int n, final int step) {
         if (1.5 * work[pingPong] < work[4 * (n - 1) + pingPong]) {
             // flip array
-            int j = 4 * n - 1;
+            int j = 4 * (n - 1);
             for (int i = 0; i < j; i += 4) {
                 for (int k = 0; k < 4; k += step) {
                     final double tmp = work[i + k];
```
###C-Fix
```diff
--- /home/ec2-user/cfix_exp/cfix-patch/math/math80/buggy/org/apache/commons/math/linear/EigenDecompositionImpl.java	2016-08-31 21:38:04.802281716 +0900
+++ /home/ec2-user/cfix_exp/cfix-patch/math/math80/cfix/org/apache/commons/math/linear/EigenDecompositionImpl.java	2016-08-31 21:38:04.786282186 +0900
@@ -1133,7 +1133,7 @@
         if (1.5 * work[pingPong] < work[4 * (n - 1) + pingPong]) {
             // flip array
             int j = 4 * n - 1;
-            for (int i = 0; i < j; i += 4) {
+            for (int i = 0; j < j; i += 4) {
                 for (int k = 0; k < 4; k += step) {
                     final double tmp = work[i + k];
                     work[i + k] = work[j - k];
```

##math81
###Human
```diff
--- /home/ec2-user/cfix_exp/cfix-patch/math/math81/buggy/org/apache/commons/math/linear/EigenDecompositionImpl.java	2016-08-31 21:38:04.994276077 +0900
+++ /home/ec2-user/cfix_exp/cfix-patch/math/math81/fix/org/apache/commons/math/linear/EigenDecompositionImpl.java	2016-08-31 21:38:05.042274670 +0900
@@ -600,6 +600,7 @@
         lowerSpectra = Math.min(lowerSpectra, lower);
         final double upper = dCurrent + eCurrent;
         work[upperStart + m - 1] = upper;
+        upperSpectra = Math.max(upperSpectra, upper);
         minPivot = MathUtils.SAFE_MIN * Math.max(1.0, eMax * eMax);
 
     }
@@ -902,8 +903,8 @@
                     diagMax    = work[4 * i0];
                     offDiagMin = work[4 * i0 + 2];
                     double previousEMin = work[4 * i0 + 3];
-                    for (int i = 4 * i0; i < 4 * n0 - 11; i += 4) {
-                        if ((work[i + 3] <= TOLERANCE_2 * work[i]) &&
+                    for (int i = 4 * i0; i < 4 * n0 - 16; i += 4) {
+                        if ((work[i + 3] <= TOLERANCE_2 * work[i]) ||
                             (work[i + 2] <= TOLERANCE_2 * sigma)) {
                             // insert a split
                             work[i + 2]  = -sigma;
@@ -1540,7 +1541,7 @@
                 double a2 = (work[np - 8] / b2) * (1 + work[np - 4] / b1);
 
                 // approximate contribution to norm squared from i < nn-2.
-                if (end - start > 2) {
+                if (end - start > 3) {
                     b2 = work[nn - 13] / work[nn - 15];
                     a2 = a2 + b2;
                     for (int i4 = nn - 17; i4 >= 4 * start + 2 + pingPong; i4 -= 4) {
```
###C-Fix
```diff
--- /home/ec2-user/cfix_exp/cfix-patch/math/math81/buggy/org/apache/commons/math/linear/EigenDecompositionImpl.java	2016-08-31 21:38:04.994276077 +0900
+++ /home/ec2-user/cfix_exp/cfix-patch/math/math81/cfix/org/apache/commons/math/linear/EigenDecompositionImpl.java	2016-08-31 21:38:04.906278661 +0900
@@ -1523,7 +1523,7 @@
                     tau = s;
 
                 }
-            } else if (dMin == dN2) {
+            } else if (sigma == dN2) {
 
                 // case 5.
                 tType = -5;
```

##math82
###Human
```diff
--- /home/ec2-user/cfix_exp/cfix-patch/math/math82/buggy/org/apache/commons/math/optimization/linear/SimplexSolver.java	2016-08-31 21:38:05.058274200 +0900
+++ /home/ec2-user/cfix_exp/cfix-patch/math/math82/fix/org/apache/commons/math/optimization/linear/SimplexSolver.java	2016-08-31 21:38:05.074273731 +0900
@@ -79,7 +79,7 @@
         for (int i = tableau.getNumObjectiveFunctions(); i < tableau.getHeight(); i++) {
             final double rhs = tableau.getEntry(i, tableau.getWidth() - 1);
             final double entry = tableau.getEntry(i, col);
-            if (MathUtils.compareTo(entry, 0, epsilon) >= 0) {
+            if (MathUtils.compareTo(entry, 0, epsilon) > 0) {
                 final double ratio = rhs / entry;
                 if (ratio < minRatio) {
                     minRatio = ratio;
```
###C-Fix
```diff
--- /home/ec2-user/cfix_exp/cfix-patch/math/math82/buggy/org/apache/commons/math/optimization/linear/SimplexSolver.java	2016-08-31 21:38:05.058274200 +0900
+++ /home/ec2-user/cfix_exp/cfix-patch/math/math82/cfix/org/apache/commons/math/optimization/linear/SimplexSolver.java	2016-08-31 21:38:05.050274435 +0900
@@ -59,7 +59,7 @@
         double minValue = 0;
         Integer minPos = null;
         for (int i = tableau.getNumObjectiveFunctions(); i < tableau.getWidth() - 1; i++) {
-            if (MathUtils.compareTo(tableau.getEntry(0, i), minValue, epsilon) < 0) {
+            if (MathUtils.compareTo(tableau.getEntry(0, i), MathUtils.SAFE_MIN, epsilon) < 0) {
                 minValue = tableau.getEntry(0, i);
                 minPos = i;
             }
```

##math84
###Human
```diff
--- /home/ec2-user/cfix_exp/cfix-patch/math/math84/buggy/org/apache/commons/math/optimization/direct/MultiDirectional.java	2016-08-31 21:38:05.106272793 +0900
+++ /home/ec2-user/cfix_exp/cfix-patch/math/math84/fix/org/apache/commons/math/optimization/direct/MultiDirectional.java	2016-08-31 21:38:05.114272558 +0900
@@ -61,6 +61,7 @@
     protected void iterateSimplex(final Comparator<RealPointValuePair> comparator)
         throws FunctionEvaluationException, OptimizationException, IllegalArgumentException {
 
+        final RealConvergenceChecker checker = getConvergenceChecker();
         while (true) {
 
             incrementIterationsCounter();
@@ -89,8 +90,16 @@
             final RealPointValuePair contracted = evaluateNewSimplex(original, gamma, comparator);
             if (comparator.compare(contracted, best) < 0) {
                 // accept the contracted simplex
+                return;
+            }
 
             // check convergence
+            final int iter = getIterations();
+            boolean converged = true;
+            for (int i = 0; i < simplex.length; ++i) {
+                converged &= checker.converged(iter, original[i], simplex[i]);
+            }
+            if (converged) {
                 return;
             }
 
```
###C-Fix
```diff
--- /home/ec2-user/cfix_exp/cfix-patch/math/math84/buggy/org/apache/commons/math/optimization/direct/MultiDirectional.java	2016-08-31 21:38:05.106272793 +0900
+++ /home/ec2-user/cfix_exp/cfix-patch/math/math84/cfix/org/apache/commons/math/optimization/direct/MultiDirectional.java	2016-08-31 21:38:05.086273379 +0900
@@ -87,7 +87,7 @@
 
             // compute the contracted simplex
             final RealPointValuePair contracted = evaluateNewSimplex(original, gamma, comparator);
-            if (comparator.compare(contracted, best) < 0) {
+            if (comparator.compare(contracted, best) < 127) {
                 // accept the contracted simplex
 
             // check convergence
```

##math85
###Human
```diff
--- /home/ec2-user/cfix_exp/cfix-patch/math/math85/buggy/org/apache/commons/math/analysis/solvers/UnivariateRealSolverUtils.java	2016-08-31 21:38:05.138271854 +0900
+++ /home/ec2-user/cfix_exp/cfix-patch/math/math85/fix/org/apache/commons/math/analysis/solvers/UnivariateRealSolverUtils.java	2016-08-31 21:38:05.166271033 +0900
@@ -195,7 +195,7 @@
         } while ((fa * fb > 0.0) && (numIterations < maximumIterations) && 
                 ((a > lowerBound) || (b < upperBound)));
    
-        if (fa * fb >= 0.0 ) {
+        if (fa * fb > 0.0 ) {
             throw new ConvergenceException(
                       "number of iterations={0}, maximum iterations={1}, " +
                       "initial={2}, lower bound={3}, upper bound={4}, final a value={5}, " +
```
###C-Fix
```diff
--- /home/ec2-user/cfix_exp/cfix-patch/math/math85/buggy/org/apache/commons/math/analysis/solvers/UnivariateRealSolverUtils.java	2016-08-31 21:38:05.138271854 +0900
+++ /home/ec2-user/cfix_exp/cfix-patch/math/math85/cfix/org/apache/commons/math/analysis/solvers/UnivariateRealSolverUtils.java	2016-08-31 21:38:05.122272324 +0900
@@ -195,7 +195,7 @@
         } while ((fa * fb > 0.0) && (numIterations < maximumIterations) && 
                 ((a > lowerBound) || (b < upperBound)));
    
-        if (fa * fb >= 0.0 ) {
+        if (fa * b >= 0.0 ) {
             throw new ConvergenceException(
                       "number of iterations={0}, maximum iterations={1}, " +
                       "initial={2}, lower bound={3}, upper bound={4}, final a value={5}, " +
```

##math88
###Human
```diff
--- /home/ec2-user/cfix_exp/cfix-patch/math/math88/buggy/org/apache/commons/math/optimization/linear/SimplexTableau.java	2016-08-31 21:38:05.186270447 +0900
+++ /home/ec2-user/cfix_exp/cfix-patch/math/math88/fix/org/apache/commons/math/optimization/linear/SimplexTableau.java	2016-08-31 21:38:05.194270212 +0900
@@ -326,19 +326,18 @@
         Integer basicRow =
             getBasicRow(getNumObjectiveFunctions() + getOriginalNumDecisionVariables());
         double mostNegative = basicRow == null ? 0 : getEntry(basicRow, getRhsOffset());
+        Set<Integer> basicRows = new HashSet<Integer>();
         for (int i = 0; i < coefficients.length; i++) {
             basicRow = getBasicRow(getNumObjectiveFunctions() + i);
+            if (basicRows.contains(basicRow)) {
                 // if multiple variables can take a given value 
                 // then we choose the first and set the rest equal to 0
+                coefficients[i] = 0;
+            } else {
+                basicRows.add(basicRow);
                 coefficients[i] =
                     (basicRow == null ? 0 : getEntry(basicRow, getRhsOffset())) -
                     (restrictToNonNegative ? 0 : mostNegative);
-            if (basicRow != null) {
-                for (int j = getNumObjectiveFunctions(); j < getNumObjectiveFunctions() + i; j++) {
-                    if (tableau.getEntry(basicRow, j) == 1) {
-                         coefficients[i] = 0;
-                    }
-                }
             }
         }
         return new RealPointValuePair(coefficients, f.getValue(coefficients));
```
###C-Fix
```diff
--- /home/ec2-user/cfix_exp/cfix-patch/math/math88/buggy/org/apache/commons/math/optimization/linear/SimplexTableau.java	2016-08-31 21:38:05.186270447 +0900
+++ /home/ec2-user/cfix_exp/cfix-patch/math/math88/cfix/org/apache/commons/math/optimization/linear/SimplexTableau.java	2016-08-31 21:38:05.178270681 +0900
@@ -335,7 +335,7 @@
                     (restrictToNonNegative ? 0 : mostNegative);
             if (basicRow != null) {
                 for (int j = getNumObjectiveFunctions(); j < getNumObjectiveFunctions() + i; j++) {
-                    if (tableau.getEntry(basicRow, j) == 1) {
+                    if (tableau.getEntry(basicRow, i) == 1) {
                          coefficients[i] = 0;
                     }
                 }
```

##math94
###Human
```diff
--- /home/ec2-user/cfix_exp/cfix-patch/math/math94/buggy/org/apache/commons/math/util/MathUtils.java	2016-08-31 21:38:05.258268335 +0900
+++ /home/ec2-user/cfix_exp/cfix-patch/math/math94/fix/org/apache/commons/math/util/MathUtils.java	2016-08-31 21:38:05.266268101 +0900
@@ -409,7 +409,7 @@
      * @since 1.1
      */
     public static int gcd(int u, int v) {
-        if (u * v == 0) {
+        if ((u == 0) || (v == 0)) {
             return (Math.abs(u) + Math.abs(v));
         }
         // keep u and v negative, as negative integers range down to
```
###C-Fix
```diff
--- /home/ec2-user/cfix_exp/cfix-patch/math/math94/buggy/org/apache/commons/math/util/MathUtils.java	2016-08-31 21:38:05.258268335 +0900
+++ /home/ec2-user/cfix_exp/cfix-patch/math/math94/cfix/org/apache/commons/math/util/MathUtils.java	2016-08-31 21:38:05.234269039 +0900
@@ -409,7 +409,7 @@
      * @since 1.1
      */
     public static int gcd(int u, int v) {
-        if (u * v == 0) {
+        if (((double) u) * v == 0) {
             return (Math.abs(u) + Math.abs(v));
         }
         // keep u and v negative, as negative integers range down to
```

##time11
###Human
```diff
--- /home/ec2-user/cfix_exp/cfix-patch/time/time11/buggy/org/joda/time/tz/ZoneInfoCompiler.java	2016-08-31 21:38:05.878250154 +0900
+++ /home/ec2-user/cfix_exp/cfix-patch/time/time11/fix/org/joda/time/tz/ZoneInfoCompiler.java	2016-08-31 21:38:05.898249567 +0900
@@ -65,10 +65,11 @@
 
     static Chronology cLenientISO;
 
-    static ThreadLocal<Boolean> cVerbose = new ThreadLocal<Boolean>();
-    static {
-        cVerbose.set(Boolean.FALSE);
-    }
+    static ThreadLocal<Boolean> cVerbose = new ThreadLocal<Boolean>() {
+        protected Boolean initialValue() {
+            return Boolean.FALSE;
+        }
+    };
 
     /**
      * Gets a flag indicating that verbose logging is required.
```
###C-Fix
```diff
--- /home/ec2-user/cfix_exp/cfix-patch/time/time11/buggy/org/joda/time/tz/ZoneInfoCompiler.java	2016-08-31 21:38:05.878250154 +0900
+++ /home/ec2-user/cfix_exp/cfix-patch/time/time11/cfix/org/joda/time/tz/ZoneInfoCompiler.java	2016-08-31 21:38:05.874250271 +0900
@@ -75,7 +75,9 @@
      * @return true to log verbosely
      */
     public static boolean verbose() {
-        return cVerbose.get();
+        if (cLenientISO == null)
+			return false;
+		return cVerbose.get();
     }
 
     //-----------------------------------------------------------------------
```

##time4
###Human
```diff
--- /home/ec2-user/cfix_exp/cfix-patch/time/time4/buggy/org/joda/time/Partial.java	2016-08-31 21:38:06.170241597 +0900
+++ /home/ec2-user/cfix_exp/cfix-patch/time/time4/fix/org/joda/time/Partial.java	2016-08-31 21:38:06.254239138 +0900
@@ -461,7 +461,7 @@
             System.arraycopy(iValues, i, newValues, i + 1, newValues.length - i - 1);
             // use public constructor to ensure full validation
             // this isn't overly efficient, but is safe
-            Partial newPartial = new Partial(iChronology, newTypes, newValues);
+            Partial newPartial = new Partial(newTypes, newValues, iChronology);
             iChronology.validate(newPartial, newValues);
             return newPartial;
         }
```
###C-Fix
```diff
--- /home/ec2-user/cfix_exp/cfix-patch/time/time4/buggy/org/joda/time/Partial.java	2016-08-31 21:38:06.170241597 +0900
+++ /home/ec2-user/cfix_exp/cfix-patch/time/time4/cfix/org/joda/time/Partial.java	2016-08-31 21:38:06.162241832 +0900
@@ -461,7 +461,7 @@
             System.arraycopy(iValues, i, newValues, i + 1, newValues.length - i - 1);
             // use public constructor to ensure full validation
             // this isn't overly efficient, but is safe
-            Partial newPartial = new Partial(iChronology, newTypes, newValues);
+            Partial newPartial = new Partial(newTypes, newValues);
             iChronology.validate(newPartial, newValues);
             return newPartial;
         }
```

##time7
###Human
```diff
--- /home/ec2-user/cfix_exp/cfix-patch/time/time7/buggy/org/joda/time/format/DateTimeFormatter.java	2016-08-31 21:38:06.322237147 +0900
+++ /home/ec2-user/cfix_exp/cfix-patch/time/time7/fix/org/joda/time/format/DateTimeFormatter.java	2016-08-31 21:38:06.370235742 +0900
@@ -705,9 +705,9 @@
         
         long instantMillis = instant.getMillis();
         Chronology chrono = instant.getChronology();
+        int defaultYear = DateTimeUtils.getChronology(chrono).year().get(instantMillis);
         long instantLocal = instantMillis + chrono.getZone().getOffset(instantMillis);
         chrono = selectChronology(chrono);
-        int defaultYear = chrono.year().get(instantLocal);
         
         DateTimeParserBucket bucket = new DateTimeParserBucket(
             instantLocal, chrono, iLocale, iPivotYear, defaultYear);
```
###C-Fix
```diff
--- /home/ec2-user/cfix_exp/cfix-patch/time/time7/buggy/org/joda/time/format/DateTimeFormatter.java	2016-08-31 21:38:06.322237147 +0900
+++ /home/ec2-user/cfix_exp/cfix-patch/time/time7/cfix/org/joda/time/format/DateTimeFormatter.java	2016-08-31 21:38:06.278238436 +0900
@@ -707,7 +707,7 @@
         Chronology chrono = instant.getChronology();
         long instantLocal = instantMillis + chrono.getZone().getOffset(instantMillis);
         chrono = selectChronology(chrono);
-        int defaultYear = chrono.year().get(instantLocal);
+        int defaultYear = chrono.year().get(instantMillis);
         
         DateTimeParserBucket bucket = new DateTimeParserBucket(
             instantLocal, chrono, iLocale, iPivotYear, defaultYear);
```

