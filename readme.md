<!DOCTYPE HTML >
<html>
   <head>
      <title>GazeCloudAPI</title>



 <script src="https://api.gazerecorder.com/GazeCloudAPI.js" ></script>


      <style type="text/css">
         body {
         overflow: hidden;
         }
      </style>
      <script type = "text/javascript" >

            
         function PlotGaze(GazeData) {

/*
	GazeData.state // 0: valid gaze data; -1 : face tracking lost, 1 : gaze uncalibrated
	GazeData.docX // gaze x in document coordinates
	GazeData.docY // gaze y in document cordinates
	GazeData.time // timestamp
*/

         
             document.getElementById("GazeData").innerHTML = "GazeX: " + GazeData.GazeX + " GazeY: " + GazeData.GazeY;
             document.getElementById("HeadPhoseData").innerHTML = " HeadX: " + GazeData.HeadX + " HeadY: " + GazeData.HeadY + " HeadZ: " + GazeData.HeadZ;
             document.getElementById("HeadRotData").innerHTML = " Yaw: " + GazeData.HeadYaw + " Pitch: " + GazeData.HeadPitch + " Roll: " + GazeData.HeadRoll;
         

         var x = GazeData.docX;
         var y = GazeData.docY;
         
         var gaze = document.getElementById("gaze");
         x -= gaze .clientWidth/2;
         y -= gaze .clientHeight/2;
         
         

	gaze.style.left = x + "px";
	gaze.style.top = y + "px";

         
         if(GazeData.state != 0)
         {
         	if( gaze.style.display  == 'block')
         	gaze  .style.display = 'none';
         }
         else
         {
         	if( gaze.style.display  == 'none')
         	gaze  .style.display = 'block';
         }
         
         }
         
         //////set callbacks/////////
   window.addEventListener("load", function() {

         GazeCloudAPI.OnCalibrationComplete =function(){ console.log('Calibración de mirada Completada')  }
         GazeCloudAPI.OnCamDenied =  function(){ console.log('Acceso a cámara denegado')  }
         GazeCloudAPI.OnError =  function(msg){ console.log('Error: ' + msg)  }
         GazeCloudAPI.UseClickRecalibration = true;
      	 GazeCloudAPI.OnResult = PlotGaze; 
          });
     
      </script>
   </head>
   <body >
      <h1>Ejemplo de integración de GazeCloudAPI</h1>
      <center>
         <img src="escalera.png"
         with="500" height="500" 
         alt="imagen de una escalera">
      </center>
      <button  type="button" onclick="GazeCloudAPI.StartEyeTracking();">Start</button>
      <button  type="button" onclick="GazeCloudAPI.StopEyeTracking();">Stop</button>
      <div >
         <p >  
            Real-Time Result:
         <p id = "GazeData" > </p>
         <p id = "HeadPhoseData" > </p>
         <p id = "HeadRotData" > </p>
         </p>
      </div>
      <div id ="gaze" style ='position: absolute;display:none;width: 100px;height: 100px;border-radius: 50%;border: solid 2px  rgba(255, 255,255, .2);	box-shadow: 0 0 100px 3px rgba(125, 125,125, .5);	pointer-events: none;	z-index: 999999'></div>
      ;  
   </body>
</html>