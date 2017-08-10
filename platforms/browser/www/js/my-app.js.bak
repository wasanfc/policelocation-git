// Initialize app
var myApp = new Framework7();
var latUser=14.8682338;
var lonUser=103.5147736;
var opendb;
var dataGlobal;
var resultsGlobal;//ค่าการ select สถานีตำรวจ 
var timeSet=4000;
var map, infoWindow;
var markerUser;
var markerStation=[];
var distanceUser=20;
var chkstart=0;
var pageGlobal="home";
// If we need to use custom DOM library, let's save it to $$ variable:
var $$ = Dom7;

// Add view
var mainView = myApp.addView('.view-main', {
    // Because we want to use dynamic navbar, we need to enable it for this view:
    dynamicNavbar: true
});

// Handle Cordova Device Ready Event
$$(document).on('deviceready', function() {
    console.log("Device is ready!");
});


// Now we need to run the code that will be executed only for About page.

// Option 1. Using page callback for page (for "about" page in this case) (recommended way):
myApp.onPageInit('about', function (page) {
    // Do something here for "about" page

})

// Option 2. Using one 'pageInit' event handler for all pages:
$$(document).on('pageInit', function (e) {
    // Get page data from event data
    var page = e.detail.page;

    if (page.name === 'about') {
        // Following code will be executed for page with data-page attribute equal to "about"
        myApp.alert('Here comes About page');
    }
})

// Option 2. Using live 'pageInit' event handlers for each page
$$(document).on('pageInit', '.page[data-page="about"]', function (e) {
    // Following code will be executed for page with data-page attribute equal to "about"
    myApp.alert('Here comes About page');
})

$$('#tab-1').on('show', function () {  
	$('#menu1').html('<i class="f7-icons" style="color:#e02c2c;">collection_fill</i></a>');
	$('#menu2').html('<i class="f7-icons color-gray">world_fill</i>');
	timeSet=10000;// ตั้งค่าเวลาวนลูป หน้าแผนที่
});
$$('#tab-2').on('show', function () {  
	chkstart=0;//ตั้งเชค ให้เหมือนเริ่มแอพ
	$('#menu1').html('<i class="f7-icons color-gray">collection_fill</i></a>');
	$('#menu2').html('<i class="f7-icons" style="color:#e02c2c;">world_fill</i>');
	$('#mapmain').css('height',$(window).height());
	if(markerUser==null){// เชคว่าเข้าหน้าแผนที่ครั้งแรกจาก markerUser
		initMap();	
		
	}
	timeSet=2000;// ตั้งค่าเวลาวนลูป หน้าแผนที่
	drawmarkstation();
});


	
	var openpopupaddress;
	var openpopupname;
	var openpopuptel;
	var openpopuptels;
	var openpopupdis;
	var openpopuplat;
	var openpopuplon;
	function opendevelop(){ 
		pageGlobal="popupde";
	}
	function openpopup(address,name,tel,tels,dis,lat,lon) {	 //console.log(lat);
		pageGlobal="popup";
		var openpopupaddress=address;
		var openpopupname=name;
		var openpopuptel=tel;
		var openpopuptels=tels;
		var openpopupdis=dis;
		var openpopuplat=lat;
		var openpopuplon=lon;
		$('#mapdetail').css('height',$(window).height()-250);				
		$('#bottomdetail').css('height',$(window).height()-$('#mapdetail').height()-$('.navbar').height());
		$('#t').html('<div class="center">รายละเอียด</div>');
		setTimeout(function(){
			//$('#mapdetail').css('height',$(window).height()-$('.toolbar').height());				
			initMapDetail(openpopupaddress,openpopupname,openpopuptel,openpopuptels,openpopupdis,openpopuplat,openpopuplon);

		$('#mapdetail').css('height',$(window).height()-$('#detailstation').height()-60);				
		$('#bottomdetail').css('height',$(window).height()-$('#mapdetail').height()-$('.navbar').height());
		$('#t').html('<div class="center">รายละเอียด</div>');
		},10);
			var xx="'"+tel+"'";
			var x="";
				x+='<a href="#" onclick="callp('+xx+')" class="button button-big button-fill color-orange" style="font-size:20px;">โทรด่วน</a>';
				x+='<div class="list-block" style="padding:0px;margin:0px;">';
				x+='<ul>';
				x+='<li class="item-content">';
				x+='<div class="item-media"><img src="img/job-royal-thai-police.png" width="50" height="50"></div>';
				x+='<div class="item-inner">';
				x+='<div>';																	
				x+='<div style="font-size:16px;color:#000;">'+name+'</div>';
				x+='<div style="font-size:12px;color:#707070;">'+address+'</div>';
				x+='<div style="font-size:12px;color:#707070;"><i class="f7-icons color-orange" style="font-size:20px;">phone_round_fill</i> '+tel+'&nbsp;&nbsp; โทรสาร : '+tels+'<br>ระยะทาง '+dis+' <span class="list1">กม.</span></div>';
				x+='</div>';
				x+='</div>';
				x+='</li>	';
				x+='</ul>';
				x+='</div>';
			$('#detailstation').html(x);

	}

	function callp(tel){  
		window.open('tel:'+ tel, '_system');
	}

	function closepopup(){
		pageGlobal="home";
		myApp.closeModal(".popup-about");
	}
	function closepopupdevelop(){
		pageGlobal="home";
		myApp.closeModal(".popup-develop");
	}
	$('#myForm input').on('change', function() { // เชคตั้งค่าระยะการหาสถานีตำรวจ
	   chkstart=0;
	   var v=$('input[name=my-radio]:checked', '#myForm').val();    
	   distanceUser=v;
	   drawliststation(resultsGlobal);	 	   
	});


	function initMapDetail(address,name,tel,tels,dis,lat,lon) {// แผนที่ popup	 
		//console.log(address+"-"+name+"-"+tel+"-"+tels+"-"+dis+"-"+lat+"-"+lon);
		//var m='<img src="https://maps.googleapis.com/maps/api/directions/json?origin=Chicago,IL&destination=Los+Angeles,CA&waypoints=Joplin,MO|Oklahoma+City,OK&key=AIzaSyBN3bpGYSZ7rLkgRcrw4LzLlf13SdOGu_0">';

	$.getJSON('https://maps.googleapis.com/maps/api/directions/json?origin='+lat+','+lon+'&destination='+latUser+','+lonUser+'&mode=driving&key=AIzaSyBN3bpGYSZ7rLkgRcrw4LzLlf13SdOGu_0&callback', function(data) {
	//console.log(data.routes[0].overview_polyline.points);
    //data is the JSON string
var x=$(window).width()+'x'+($(window).height()-$('#detailstation').height()-60);
	var m='<img src="https://maps.googleapis.com/maps/api/staticmap?size='+x+'&path=enc%3A'+data.routes[0].overview_polyline.points+'&markers=icon:https://maps.gstatic.com/mapfiles/ms2/micons/man.png%7Clabel:P%7C'+latUser+','+lonUser+'&markers=icon:https://maps.gstatic.com/mapfiles/ms2/micons/police.png%7Clabel:U%7C'+lat+','+lon+'">';
	$('#mapdetail').html(m);
});
						//&markers=size:mid%7Ccolor:red%7CSan+Francisco,CA%7COakland,CA%7CSan+Jose,CA


		//$('#mapdetail').html(m);



		/*
        var directionsService = new google.maps.DirectionsService;
        var directionsDisplay = new google.maps.DirectionsRenderer({suppressMarkers: true});


		var mapdetail = new google.maps.Map(document.getElementById('mapdetail'), {
			center: {lat: 14.8681302, lng: 103.5141084},
				disableDefaultUI: true,
				zoom: 15
		});
		directionsDisplay.setMap(mapdetail);		

        directionsService.route({
          origin: new google.maps.LatLng(latUser,lonUser),
          destination: new google.maps.LatLng(lat, lon),
          travelMode: 'DRIVING'
        }, function(response, status) {
          if (status === 'OK') {
            directionsDisplay.setDirections(response);



          } else {
            
          }
        });


		var image = {
		  url: "img/markerUser.png",
		  scaledSize: new google.maps.Size(25, 25)
		};

		var m = new google.maps.Marker({
			position: new google.maps.LatLng(latUser, lonUser),
			animation: google.maps.Animation.BOUNCE,
			icon:image,
			map: mapdetail
		});
		var bounds = new google.maps.LatLngBounds();// ตั้งค่า zoom ทุกจุด
		bounds.extend(new google.maps.LatLng(latUser, lonUser));
		bounds.extend(new google.maps.LatLng(lat, lon));
		mapdetail.fitBounds(bounds);		

		var image2 = {
		  url: "img/map-marker-2-xxl (1).png"
		};										
		var m2 = new google.maps.Marker({
			position: new google.maps.LatLng(lat, lon),
			icon:image2,
			map: mapdetail
		});	
		var infowindow2 = new google.maps.InfoWindow({
			content: name
		});
		infowindow2.open(mapdetail, m2);
		*/
	}
      

		function getDistanceFromLatLonInKm(lat1,lon1,lat2,lon2) {
		  var R = 6371; // Radius of the earth in km
		  var dLat = deg2rad(lat2-lat1);  // deg2rad below
		  var dLon = deg2rad(lon2-lon1); 
		  var a = 
			Math.sin(dLat/2) * Math.sin(dLat/2) +
			Math.cos(deg2rad(lat1)) * Math.cos(deg2rad(lat2)) * 
			Math.sin(dLon/2) * Math.sin(dLon/2)
			; 
		  var c = 2 * Math.atan2(Math.sqrt(a), Math.sqrt(1-a)); 
		  var d = R * c; // Distance in km
		  return d;
		}

		function deg2rad(deg) {
		  return deg * (Math.PI/180)
		}

	var varibleloop;

	  $(function(){
		document.addEventListener("backbutton", 
		function(e){ 
			var page=myApp.getCurrentView().activePage; 
			myApp.hidePreloader(); 	
			if(pageGlobal=="popup"){ 
				e.preventDefault(); 
				pageGlobal="home";
				myApp.closeModal(".popup-about");				
			}else if(pageGlobal=="popupde"){		
				pageGlobal="home";
				myApp.closeModal(".popup-develop");
			}else if(pageGlobal=="home") { 
				navigator.app.backHistory();
				navigator.app.exitApp();
			}
			
		});

			$('#menu1').html('<i class="f7-icons" style="color:#e02c2c;">collection_fill</i></a>');
			$('#menu2').html('<i class="f7-icons color-gray">world_fill</i>');
			// แสดงไอคอนเมนูบน
			loopgetlocationA(); // ตั้งวนลูป หาตำแหน่ง

			//opendb= window.openDatabase({name: "js/policelocation.db"});
			opendb = window.openDatabase("policelocation", "1.0", "policelocation_db", 10000);
			opendb.transaction(function(tx){					 
				 //tx.executeSql('DROP TABLE policelocation');
				 //tx.executeSql('DROP TABLE locationold');
				 tx.executeSql('CREATE TABLE IF NOT EXISTS policelocation (id integer NOT NULL PRIMARY KEY AUTOINCREMENT,address TEXT,name TEXT,lat TEXT)');
				 tx.executeSql('CREATE TABLE IF NOT EXISTS locationold (id integer NOT NULL PRIMARY KEY AUTOINCREMENT,lat TEXT,lon TEXT)');
				 tx.executeSql('SELECT * FROM policelocation', [], function(txs, results){ 
					 
					if (results.rows.length!=0)
					{ 	resultsGlobal=results;
						
						opendb.transaction(function(tx){	
							 tx.executeSql('SELECT * FROM locationold', [], function(txs, results){ 	// select ตำแหน่งเดิม เมื่อเริ่มแอพ
								if (results.rows.length!=0)
								{ 	
									for (var i=0;i<results.rows.length ;i++ )
									{
										latUser=results.rows.item(i).lat;
										lonUser=results.rows.item(i).lon;
									}																		
									drawliststation(resultsGlobal); // สร้าง list สถานีตำรวจ
									
								}
							 });
						});


						getLocationUser();// หาตำแหน่งปัจจุบัน
					}else{					
						// สร้าง insert list สถานีตำรวจ ครั้งแรก
						$.ajax({
							url: 'http://policelocation.tomatogood.com/data/select_policestation.php',
							type: "POST",
							data:{id:1},
							dataType: 'jsonp',
							jsonp: 'callback',
							success: function(data){	 
								dataGlobal=data;
								opendb.transaction(function(tx){	
										tx.executeSql('INSERT INTO locationold VALUES (null, "'+latUser+'","'+lonUser+'")');	

									for(var i=0;i<dataGlobal.length;i++){
										tx.executeSql('INSERT INTO policelocation VALUES (null, "'+dataGlobal[i].address+'","'+dataGlobal[i].name+'","'+dataGlobal[i].lat+'")');	

										if(i+1==dataGlobal.length){ 
											 tx.executeSql('SELECT * FROM policelocation', [], function(txs, results){ 		
												if (results.rows.length!=0)
												{ 	resultsGlobal=results;
													drawliststation(resultsGlobal); // สร้าง list สถานีตำรวจ
													getLocationUser();// หาตำแหน่งปัจจุบัน
												}										
											});
										}

									}														
								});
							}
						});

					}
			
			}, null, null);	
		});


			$('#searchstation').keyup(function() {
				var data = this.value; 
				if(data!=null){ 
					clearInterval(varibleloop);
					distanceUser=5000;//เซตระยะค้นหา เมื่อค้นหาสถานีทั้งหมด
					searchstation(data);
				}
				
				if(data==""){ 
					var v=$('input[name=my-radio]:checked', '#myForm').val();    
					distanceUser=v;
					drawliststation(resultsGlobal);
				}
				
				
			});

	  });
		
		var datasearch;
		function searchstation(data){ //ค้นหาสถานีตำรวจ 
			datasearch=data; 
			opendb.transaction(function(tx){ 
			tx.executeSql("SELECT * FROM policelocation WHERE name LIKE '%"+datasearch+"%'", [], function(txs, results){ 	// select ตำแหน่งเดิม เมื่อเริ่มแอพ				
				if (results.rows.length!=0)
				{ 	
					resultsGlobal=results;
					drawliststation(resultsGlobal); // สร้าง list สถานีตำรวจ
					if($('#searchstation').val()==""){
					   var v=$('input[name=my-radio]:checked', '#myForm').val();    //เซตระยะทางค้นหาสถานี เป็นตัวที่ตั้งไว้
					   distanceUser=v;
						loopgetlocationB();
						loopgetlocationA();// เริ่มวนลูป
					}
					drawmarkstation();
				}else{
					$('#liststation').html("<li><div style='font-size:14px;color:#707070;padding:10px;'>ไม่พบสถานีตำรวจที่ค้นหา</div></li>");
				}
			});	
			});
		}


		  function loopgetlocationA(){
			  //setTimeout(function(){loopgetlocationB();},timeSet);
				varibleloop = setInterval(function(){loopgetlocationB();},timeSet);
		  }
		  function loopgetlocationB(){ 
						opendb.transaction(function(tx){
							 tx.executeSql('SELECT * FROM locationold', [], function(txs, results){ 	// select ตำแหน่งเดิม เมื่อหาตำแหน่งไม่ได้	
								if (results.rows.length!=0)
								{ 	 
									for (var i=0;i<results.rows.length ;i++ )
									{
										if(latUser!=results.rows.item(i).lat){ // เชคว่าตรงกับตำแหน่งเดิมหรือไม่
											drawmarkstation();
											drawliststation(resultsGlobal);

										}
										if(chkstart==0){ // เชคว่าเข้าแอพเป็นครั้งแรกใช่หรือไม่
											drawmarkstation();
											drawliststation(resultsGlobal);

										}
									}																		
								}										
										chkstart=1; //เชคว่าเคยเข้าแผนที่แล้ว
										//loopgetlocationA();
										getLocationUser();																		

							});
						});
		  }

		  function drawmarkstation(){  // มาคจุดสถานีตำรวจ

				if(timeSet==2000){ // เชคว่าเปิดหน้าแผนที่อยู่ เพื่อมาคจุดปัจจุบัน
					var image = {
					  url: "img/markerUser.png",
					  scaledSize: new google.maps.Size(25, 25)
					};
					 if(markerUser!=null){
						markerUser.setMap(null);
					 }
					var m = new google.maps.Marker({
						position: new google.maps.LatLng(latUser, lonUser),
						animation: google.maps.Animation.BOUNCE,
						icon:image,
						map: map
					});	
					//map.panTo(new google.maps.LatLng(latUser, lonUser));

					markerUser=m;
					for (var i=0;i<markerStation.length ;i++ )
					{
							markerStation[i].setMap(null);
					}

					opendb.transaction(function(tx){ // หาตำแหน่งสถานีตำรวจ	
						var sql=""; 
						if($('#searchstation').val()==""){
							sql="SELECT * FROM policelocation";
						}else{
							sql="SELECT * FROM policelocation WHERE name LIKE '%"+$('#searchstation').val()+"%'";
						}
						 tx.executeSql(sql, [], function(txs, results){ 		
							if (results.rows.length!=0)
							{ 	resultsGlobal=results;

								var bounds = new google.maps.LatLngBounds();// ตั้งค่า zoom ทุกจุด
								bounds.extend(new google.maps.LatLng(latUser, lonUser));

								for (var i=0;i<results.rows.length ;i++ )
								{
									var x=results.rows.item(i).address;
									var y = x.split("<br>");
									var address=(y[1].replace("ที่อยู่", "")).trim();
									var tel=(y[2].replace("เบอร์โทรศัพท์", "")).trim();
									var tels=(y[3].replace("เบอร์โทรสาร", "")).trim();
									var name=results.rows.item(i).name;
									var a=results.rows.item(i).lat;
									var b = a.split(",");
									var lat=b[1];
									var lon=b[0];									
									if(parseInt(getDistanceFromLatLonInKm(lat,lon,latUser,lonUser))<parseInt(distanceUser)){// เชคระยะว่าจะแสดงภายในกี่ กม.
										var image = {
										  url: "img/x.png"
										};			
										var m = new google.maps.Marker({
											position: new google.maps.LatLng(lat, lon),
											icon:image,
											map: map
										});	
										bounds.extend(new google.maps.LatLng(lat, lon));
										markerStation.push(m);
										//console.log(getDistanceFromLatLonInKm(lat,lon,latUser,lonUser)+"---"+i);

									var addresssend="'"+address+"'";
									var namesend="'"+name+"'";
									var telsend="'"+tel+"'";
									var telssend="'"+tels+"'";
									var dissend="'"+parseInt(getDistanceFromLatLonInKm(lat,lon,latUser,lonUser)).toFixed(2)+"'";
									var latsend="'"+lat+"'";
									var lonsend="'"+lon+"'";
									
									var content='<a href="#detail" data-popup=".popup-about" class="item-link item-content open-popup" onclick="openpopup('+addresssend+','+namesend+','+telsend+','+telssend+','+dissend+','+latsend+','+lonsend+')">'+name+'</a>';
									var infowindows = new google.maps.InfoWindow();			
									google.maps.event.addListener(m,'click', (function(marker,content,infowindow){ // show info หน้า mapmain
										return function() {
											infowindow.setContent(content);
											infowindow.open(map,marker);
										};
									})(m,content,infowindows)); 									

									}
									//console.log(getDistanceFromLatLonInKm(lat,lon,latUser,lonUser));

								}
								
								map.fitBounds(bounds);					

							}										
						});
					});
				}			  
		  }
		

			function drawliststation(results){ // สร้าง list สถานีตำรวจ
						var textdraw="";

						var obList = new Array(results.rows.length);
						for (var i=0;i<results.rows.length ;i++ )// sort ระยะห่างของสถานีตำรวจ
						{
							obList[i]=new Array(1);
							var x=results.rows.item(i).address;
							var y = x.split("<br>");
							//console.log(y);
							var address=(y[1].replace("ที่อยู่", "")).trim();
							var tel=(y[2].replace("เบอร์โทรศัพท์", "")).trim();
							var tels=(y[3].replace("เบอร์โทรสาร", "")).trim();
							var name=results.rows.item(i).name;
							var a=results.rows.item(i).lat;
							var b = a.split(",");
							var lat=b[1];
							var lon=b[0];
							var dis=(getDistanceFromLatLonInKm(lat,lon,latUser,lonUser)).toFixed(2);
							//console.log(dis+"--"+name+"-"+lat+"-"+lon+"-"+latUser+"-"+lonUser);
							obList[i]['dis']=dis;
							obList[i]['name']=name;
							obList[i]['address']=address;
							obList[i]['tel']=tel;
							obList[i]['tels']=tels;
							obList[i]['name']=name;
							obList[i]['lat']=lat;
							obList[i]['lon']=lon;
							if(i+1==results.rows.length){ //รอบสุดท้าย
								obList.sort(function (a, b) {
								  return a.dis - b.dis;
								});

								for (var ii=0;ii<obList.length ;ii++ )// สร้าง list สถานีตำรวจ
								{
									if(parseInt(obList[ii]['dis'])<parseInt(distanceUser)){// เชคระยะว่าจะแสดงภายในกี่ กม.

											var address2="'"+obList[ii]['address']+"'";
											var name2="'"+obList[ii]['name']+"'";
											var tel2="'"+obList[ii]['tel']+"'";
											var tels2="'"+obList[ii]['tels']+"'";
											var dis2="'"+parseInt(obList[ii]['dis']).toFixed(2)+"'";
											var lat2="'"+obList[ii]['lat']+"'";
											var lon2="'"+obList[ii]['lon']+"'";
										
											textdraw+='<li>';
											textdraw+='<a href="#detail" data-popup=".popup-about" class="item-link item-content open-popup" onclick="openpopup('+address2+','+name2+','+tel2+','+tels2+','+dis2+','+lat2+','+lon2+')">';
											textdraw+='<div class="item-media">';
											textdraw+='<img src="img/job-royal-thai-police.png" width="50" height="50">';
											textdraw+='</div>';
											textdraw+='<div class="item-inner"> ';
											textdraw+='<div class="item-title">';
											textdraw+='<div style="font-size:16px;">'+obList[ii]['name']+'</div>';
											textdraw+='<div style="font-size:12px;color:#707070;">'+obList[ii]['address']+'</div>';
											textdraw+='<span style="font-size:12px;color:#707070;"><i class="f7-icons color-orange" style="font-size:20px;">phone_round_fill</i> '+obList[ii]['tel']+'</span>';
											textdraw+='</div>';
											textdraw+='<div class="item-after" style="font-size:12px;padding-left:10px;">'+obList[ii]['dis']+'<span class="list1">กม.</span>';
											textdraw+='</div></div>';
											textdraw+='</a>';
											textdraw+='</li>';
										
									}
								}
								$('#liststation').html(textdraw);														
							}
						}




			
			
			}

			function getLocationUser(){ // หาตำแหน่งปัจจุบัน
				navigator.geolocation.getCurrentPosition(
					
					function onSuccess(position) {  
						latUser=position.coords.latitude;
						lonUser=position.coords.longitude;
						//alert(latUser+"--"+lonUser);
						opendb.transaction(function(tx){	// update ตำแหน่งล่าสุดลง database						
							tx.executeSql("update locationold set lat='"+latUser+"', lon='"+lonUser+"'");
						});
						

/*
						var element = document.getElementById('geolocation');
						element.innerHTML = 'Latitude: '           + position.coords.latitude              + '<br />' +
											'Longitude: '          + position.coords.longitude             + '<br />' +
											'Altitude: '           + position.coords.altitude              + '<br />' +
											'Accuracy: '           + position.coords.accuracy              + '<br />' +
											'Altitude Accuracy: '  + position.coords.altitudeAccuracy      + '<br />' +
											'Heading: '            + position.coords.heading               + '<br />' +
											'Speed: '              + position.coords.speed                 + '<br />' +
											'Timestamp: '          + new Date(position.timestamp)          + '<br />';
*/											
					}				
				, 					
				    function onError(error) {
					alert("error");
						opendb.transaction(function(tx){
							 tx.executeSql('SELECT * FROM locationold', [], function(txs, results){ 	// select ตำแหน่งเดิม เมื่อหาตำแหน่งไม่ได้	
								if (results.rows.length!=0)
								{ 	
									for (var i=0;i<results.rows.length ;i++ )
									{
										latUser=results.rows.item(i).lat;
										lonUser=results.rows.item(i).lon;
									}									
//console.log("onError gps");
									//drawliststation(resultsGlobal); // สร้าง list สถานีตำรวจ
									
								}										
							});
						});
					});
			
			}

			function initMap() {			// สร้าง map หลัก	
				map = new google.maps.Map(document.getElementById('mapmain'), {
					center: {lat: 14.8681302, lng: 103.5141084},
    disableDefaultUI: true,
						zoom: 15
				});
			}





