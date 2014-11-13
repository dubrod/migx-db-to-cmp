###Specific details on creating the actual Interactive Map

If you have any questions just email me - wayne@modx.com

##The XML Schema

```
<?xml version="1.0" encoding="UTF-8"?>
<model package="interactivemap" baseClass="xPDOObject" platform="mysql" defaultEngine="MyISAM" version="1.1">
    <object class="interactivemap" table="interactivemap" extends="xPDOSimpleObject">
	    <field key="photo" dbtype="text" phptype="string" null="false" default="" />
		<field key="simplename" dbtype="varchar" precision="255" phptype="string" null="false" default=""/>
		<field key="fullname" dbtype="varchar" precision="255" phptype="string" null="false" default=""/>
        <field key="category" dbtype="varchar" precision="255" phptype="string" null="false" default=""/>
		<field key="location" dbtype="varchar" precision="255" phptype="string" null="false" default=""/>        
		<field key="quip" dbtype="varchar" precision="255" phptype="string" null="false" default=""/>
		<field key="link" dbtype="varchar" precision="255" phptype="string" null="false" default=""/>		
		<field key="centergps" dbtype="text" phptype="string" null="false" default=""/>
		<field key="startgps" dbtype="text" phptype="string" null="false" default=""/>
		<field key="points" dbtype="text" phptype="string" null="false" default=""/>
    </object>
</model>
```

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

##The Template HTML

```
<div id="map-wrapper">
	<div id="map-filters">
	<ul>
	<li> &nbsp;&nbsp;VIEW PROPERTIES BY:</li>
	<li>
		<a href="javascript:void(0);" class="commercial-link" onclick="boxclick('commercial')"> &lt; Commercial</a>
	</li>
	<li>
		<a href="javascript:void(0);" class="single-link" onclick="boxclick('single-family')"> &lt; Single Family</a>
	</li>
	<li>
		<a href="javascript:void(0);" class="multi-link" onclick="boxclick('multi-family')"> &lt; Multi-Family</a>
	</li>
	<li>
		<a href="javascript:void(0);" class="legacy-link" onclick="boxclick('legacy')"> &lt; Legacy</a>
	</li>
	</ul>
	</div> <!-- /filters -->
				
	<div id="map-canvas"></div>	
</div>
```

##The interactivemap-row chunk

```
// START [[+simplename]]
	
	  var [[+simplename]]Content = '<div class="overlay-cell">'+
      '<div class="overlay-pic"><img src="[[+photo]]"></div>'+
      '<div class="overlay-meta">'+
      '<p><strong>[[+fullname]]</strong><br><em>[[+location]]</em><br>[[+quip]]</p> ' +
      '</div>'+
      '<div class="overlay-link"><a href="[[+link]]">more info &nbsp; <i>&gt;</i></a></div>'+
      '</div>';
	
	  var [[+simplename]]Window = new google.maps.InfoWindow({
	      content: [[+simplename]]Content
	  });
	  
	  //setup marker
	  var [[+simplename]]Marker = new google.maps.Marker({
	      position: new google.maps.LatLng([[+centergps]]),
	      map: map,
	      title: '[[+fullname]]',
	      category: '[[+category]]'
	  });
	  
	  //add marker to array
	  all_markers.push([[+simplename]]Marker);
	  
	  google.maps.event.addListener([[+simplename]]Marker, 'click', function() {
	    [[+simplename]]Window.open(map,[[+simplename]]Marker);
	  });    
	
	  // Define the LatLng coordinates for the polygon's path.
	  var [[+simplename]]Points = [
	    [[+points]]
	  ];
	  
	  // Construct the [[+simplename]] polygon.
	  // single family colors
	  [[+simplename]]Plot = new google.maps.Polygon({
	    paths: [[+simplename]]Points,
	    strokeColor: '[[+category:is=`single-family`:then=`#4a9bce`]][[+category:is=`multi-family`:then=`#cc7a17`]][[+category:is=`commercial`:then=`#66953f`]][[+category:is=`legacy`:then=`#985323`]]',
	    strokeOpacity: 0.8,
	    strokeWeight: 2,
	    fillColor: '[[+category:is=`single-family`:then=`#4a9bce`]][[+category:is=`multi-family`:then=`#cc7a17`]][[+category:is=`commercial`:then=`#66953f`]][[+category:is=`legacy`:then=`#985323`]]',
	    fillOpacity: 0.7,
	    category: '[[+category]]'
	  });
	  [[+simplename]]Plot.setMap(map);
	  
	  google.maps.event.addListener([[+simplename]]Plot, 'click', function() {
	    [[+simplename]]Window.open(map,[[+simplename]]Marker);
	  });
	  
	  //add polygon to array
	  all_polygons.push([[+simplename]]Plot);
	
	// EOF [[+simplename]]
```

##_map.scss

```
//Interactive Map Styles
#map-wrapper{
	position: relative;
	height: 500px;
	margin: 40px 0;
}

#map-canvas {
    height: 500px;
    width: 100%;
    position: absolute;
    top:0;
    left:0;
    z-index: 1;
}

#map-filters{
    width: 180px;
    background: #494c4e;
    position: absolute;
    top: 40px;
    right: 7px;
    z-index: 10;
    border: 1px solid #999;
    border-radius: 2px;
    
    ul{
	    li{
		    @include RegularFont;
		    color: $white;
		    font-size: .9rem;
		    margin: .2rem 0;
		    
		    a{
			    display: block;
			    color: $white;
			    padding: .25rem .5rem;
		    }
	    }
    }
    
    a.commercial-link{
	    background: #66953f;
    }
    
    a.single-link{
	    background: #4d9bce;
    }
    
    a.multi-link{
	    background: #cc7a17;
    }
    a.legacy-link{
	    background: #985323;
    }
}

.overlay-cell{
	width: 300px;
	
		.overlay-pic{
			width: 100%;
			height: 125px;
			overflow: hidden;
			background: #c7eafb;
			img{max-width: 100%;}
		}
		
		.overlay-meta{
			background: #f1f1f2;
			width: 100%;
			
			p{
				padding: .5rem;
				font-size: .8rem;
			}
		}
		
		.overlay-link{
			background: #5a5955;
			width: 100%;
			text-align: right;
			
			a{
				font-style: italic;
				color: $white;
				font-size: .8rem;
				padding-right: .5rem;
				display: block;
				letter-spacing: 1px;
				vertical-align: middle;
				
				i{
					width: 15px;
					height: 15px;
					border-radius: 50%;
					background: $white;
					color: #5a5955;
					text-align: center;
					display: inline-block;
					vertical-align: middle;
					line-height: .9rem;
					text-indent: 2px;
					font-size: .6rem;
					margin-top: -2px;
				}
			}
		}
}
```

##Final Map

![map](https://dl.dropboxusercontent.com/u/4277345/MODX/migx-to-cmp/final-map.png)
