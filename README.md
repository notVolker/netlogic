# netlogic

# chart.html

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


# netlogic

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
