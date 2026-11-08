# netlogic

# chart.html

```html
<!DOCTYPE html>
<html>
<head><script src="echarts.min.js"></script></head>
<body style="margin:0">
<div id="c" style="width:100vw;height:100vh"></div>
<script>
var chart = echarts.init(document.getElementById('c'));
chart.setOption({
  xAxis: { type: 'category', data: ['Mon','Tue','Wed','Thu','Fri'] },
  yAxis: { type: 'value' },
  series: [{ type: 'line', data: [12, 18, 9, 22, 15] }]
});
window.addEventListener('resize', () => chart.resize());
</script>
</body>
</html>
```


# netlogic

```csharp
using System.IO;
using FTOptix.NetLogic;
using FTOptix.WebUI;
using UAManagedCore;

public class LoadChart : BaseNetLogic
{
    public override void Start()
    {
        var path = new ResourceUri(
            ResourceUri.FromProjectRelativePath("chart.html")).Uri;
        var browser = (WebBrowser)Owner;
        browser.URL = "file:///" + path.Replace("\\", "/");
    }
}
```


# echarts

```echarts
var years   = [1950, 1960, 1970, 1980, 1990, 2000, 2010];
var germany = [7000, 15000, 22000, 28000, 33000, 38000, 43000];
var france  = [8000, 13000, 20000, 26000, 31000, 35000, 37000];
var japan   = [3000, 9000, 24000, 30000, 36000, 39000, 41000];
var brazil  = [2000, 4000, 8000, 12000, 15000, 18000, 24000];
var output  = [0, 10000, 0, 10000, 10000, 10000, 0];

option = {
  title: { text: 'Income by Country' },
  tooltip: { trigger: 'axis' },
  legend: { data: ['Output', 'Germany', 'France', 'Japan', 'Brazil'], right: 10 },
  xAxis: { type: 'category', data: years },
  yAxis: {
    name: 'Income',
    min: 0,
    max: 50000,
    interval: 10000,
    axisLabel: { formatter: function (v) { return v / 1000 + 'k'; } }
  },
  series: [
    { name: 'Output',  type: 'bar',  data: output, itemStyle: { opacity: 0.5 } },
    { name: 'Germany', type: 'line', data: germany },
    { name: 'France',  type: 'line', data: france },
    { name: 'Japan',   type: 'line', data: japan },
    { name: 'Brazil',  type: 'line', data: brazil }
  ]
};
```

# sample netlogic
```netlogic
using System.IO;
using System.Text;
using FTOptix.NetLogic;
using FTOptix.WebUI;
using UAManagedCore;

public class EChartsLogic : BaseNetLogic
{
    public override void Start()
    {
        // ---- data (replace later with SQL / tag values) ----
        string years   = "[1950,1960,1970,1980,1990,2000,2010]";
        string germany = "[7000,15000,22000,28000,33000,38000,43000]";
        string france  = "[8000,13000,20000,26000,31000,35000,37000]";
        string japan   = "[3000,9000,24000,30000,36000,39000,41000]";
        string brazil  = "[2000,4000,8000,12000,15000,18000,24000]";
        string output  = "[0,10000,0,10000,10000,10000,0]";

        var sb = new StringBuilder();
        sb.AppendLine("<!DOCTYPE html>");
        sb.AppendLine("<html><head>");
        sb.AppendLine("<script src='echarts.min.js'></script>");
        sb.AppendLine("</head><body style='margin:0'>");
        sb.AppendLine("<div id='c' style='width:100vw;height:100vh'></div>");
        sb.AppendLine("<script>");
        sb.AppendLine("var myChart = echarts.init(document.getElementById('c'));");
        sb.AppendLine("var option = {");
        sb.AppendLine("  title: { text: 'Income by Country' },");
        sb.AppendLine("  tooltip: { trigger: 'axis' },");
        sb.AppendLine("  legend: { data: ['Output','Germany','France','Japan','Brazil'], right: 10 },");
        sb.AppendLine("  xAxis: { type: 'category', data: " + years + " },");
        sb.AppendLine("  yAxis: { name: 'Income', min: 0, max: 50000, interval: 10000,");
        sb.AppendLine("    axisLabel: { formatter: function (v) { return v / 1000 + 'k'; } } },");
        sb.AppendLine("  series: [");
        sb.AppendLine("    { name: 'Output',  type: 'bar',  data: " + output + ", itemStyle: { opacity: 0.5 } },");
        sb.AppendLine("    { name: 'Germany', type: 'line', data: " + germany + " },");
        sb.AppendLine("    { name: 'France',  type: 'line', data: " + france + " },");
        sb.AppendLine("    { name: 'Japan',   type: 'line', data: " + japan + " },");
        sb.AppendLine("    { name: 'Brazil',  type: 'line', data: " + brazil + " }");
        sb.AppendLine("  ]");
        sb.AppendLine("};");
        sb.AppendLine("myChart.setOption(option);");
        sb.AppendLine("window.addEventListener('resize', function () { myChart.resize(); });");
        sb.AppendLine("</script></body></html>");

        // ---- write the html next to echarts.min.js ----
        string folder = new ResourceUri(
            ResourceUri.FromProjectRelativePath("ProjectFiles")).Uri;
        string file = Path.Combine(folder, "chart.html");
        File.WriteAllText(file, sb.ToString());

        // ---- point the WebBrowser at it ----
        var browser = (WebBrowser)Owner;
        browser.URL = "file:///" + file.Replace("\\", "/");
    }
}
```
