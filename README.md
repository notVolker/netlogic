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


# logic

That’s the same idea as the template approach from my first message, just written differently. The code doesn’t convert JavaScript into C#. Each line of HTML and JavaScript is still JavaScript, wrapped in a C# string, and AppendLine just stacks those strings into one big HTML text.

csharp
var sb = new StringBuilder();
sb.AppendLine("<!DOCTYPE html>");
sb.AppendLine("<html><head>");
sb.AppendLine("<script src='echarts.min.js'></script>");
sb.AppendLine("</head><body style='margin:0'>");
sb.AppendLine("<div id='c' style='width:100vw;height:100vh'></div>");
sb.AppendLine("<script>");
sb.AppendLine("var chart = echarts.init(document.getElementById('c'));");
sb.AppendLine("chart.setOption({");
sb.AppendLine("  xAxis:{type:'category',data:['Mon','Tue','Wed']},");
sb.AppendLine("  yAxis:{type:'value'},");
sb.AppendLine("  series:[{type:'line',data:[12,18,9]}]");
sb.AppendLine("});");
sb.AppendLine("</script></body></html>");

File.WriteAllText(htmlPath, sb.ToString());

Then the NetLogic points the WebBrowser at that file, or in some projects sets the HTML straight into the widget.

Why people do it this way

The NetLogic can insert live values into the chart (for a Pareto chart, the defect names and counts from SQL) with AppendLine("data:" + json + ",").
No separate chart.html has to be copied around with the project.

Converting your own ECharts code

Take each line of the HTML/JS.
Wrap it in sb.AppendLine("...").
Use single quotes inside the JavaScript so you don’t have to escape double quotes.
Replace the hardcoded data arrays with C# variables.
