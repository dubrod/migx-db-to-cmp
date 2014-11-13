###Specific details on creating the actual Interactive Map

If you have any questions just email me - wayne@modx.com

##The `<head>` script

```
<script src="https://maps.googleapis.com/maps/api/js?v=3.exp"></script>

<script>
//create a marker array
var all_markers = [];
//create a polygon array
var all_polygons = [];

function boxclick(cate_filter) {show(cate_filter);}
	
function show(cate_filter) {
    for (var i=0; i<all_markers.length; i++) {
      if (all_markers[i].category == cate_filter) {
        all_markers[i].setVisible(true);
        all_polygons[i].setVisible(true);
      } else {
        all_markers[i].setVisible(false);
        all_polygons[i].setVisible(false);
      }
    }
}

function initialize() {
	var mapOptions = {
	zoom: 8,
	center: new google.maps.LatLng(32.922974, -97.153101),
	mapTypeId: google.maps.MapTypeId.ROADMAP,
};
	  
var map = new google.maps.Map(document.getElementById('map-canvas'),mapOptions);

//start migx loop
[[migxLoopCollection? &packageName=`interactivemap` &classname=`interactivemap` &tpl=`interactivemap-row`]]

} //eof function intialize
google.maps.event.addDomListener(window, 'load', initialize);

</script>
```

##Final Map

![map](https://dl.dropboxusercontent.com/u/4277345/MODX/migx-to-cmp/final-map.png)
