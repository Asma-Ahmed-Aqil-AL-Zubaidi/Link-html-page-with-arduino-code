# Link-html-page-with-arduino-code
Link html page with arduino code "full build code"


<!doctype html>
	<head>
		<title>Yanbu</title>
		<meta charset="UTF-8">
		<script src="https://ajax.googleapis.com/ajax/libs/jquery/3.6.0/jquery.min.js"></script>
		<style>
			/* CSS comes here */
			
			body {
			    font-family: arial;
			}
			button {
			    padding:10px;
			    background-color:#6a67ce;
			    color: #FFFFFF;
			    border: 0px;
			    cursor:pointer;
			    border-radius: 5px;
				font-size: 20px;
			}
			#output {
			    background-color:#F9F9F9;
			    padding:10px;
			    width: 100%;
			    margin-top:20px;
			    line-height:30px;
				font-size: 20px;
			}
			.hide {
			    display:none;
			}
			.show {
			    display:block;
			}
			
			

			/* CSS */
			.button-71 {
			  background-color: #ff8517;
			  border: 0;
			  border-radius: 56px;
			  color: #fff;
			  cursor: pointer;
			  display: inline-block;
			  font-size: 24px;
			  font-weight: 900;
			  outline: 0;
			  padding: 13px 17px;
			  position: relative;
			  text-align: center;
			  text-decoration: none;
			  transition: all .3s;
			  user-select: none;
			  -webkit-user-select: none;
			  touch-action: manipulation;
			}

			/*.button-71:before {
			  background-color: initial;
			  background-image: linear-gradient(#fff 0, rgba(255, 255, 255, 0) 100%);
			  border-radius: 125px;
			  content: "";
			  height: 45%;
			  left: 4%;
			  opacity: .5;
			  position: absolute;
			  top: 0;
			  transition: all .3s;
			  width: 85%;
			}*/

			.button-71:hover {
			  box-shadow: rgba(255, 255, 255, .2) 0 3px 15px inset, rgba(0, 0, 0, .1) 0 3px 5px, rgba(0, 0, 0, .1) 0 10px 13px;
			  transform: scale(1.05);
			}

			@media (min-width: 768px) {
			  .button-71 {
				padding: 16px 48px;
			  }
			}
		</style>
	</head>
	<body >
	<center>
	
		<audio id="audio1">
			<source  id="sound" src="1.mp3" type="audio/mp3">
		</audio>
		<audio id="audio2">
			<source  id="sound" src="2.mp3" type="audio/mp3">
		</audio>
		<audio id="audio3">
			<source  id="sound" src="3.mp3" type="audio/mp3">
		</audio>
		<audio id="audio4">
			<source  id="sound" src="4.mp3" type="audio/mp3">
		</audio>
		<audio id="audio6">
			<source  id="sound" src="6.mp3" type="audio/mp3">
		</audio>
		<audio id="audio7">
			<source  id="sound" src="7.mp3" type="audio/mp3">
		</audio>
		<audio id="audio8">
			<source  id="sound" src="8.mp3" type="audio/mp3">
		</audio>
		<audio id="audio9">
			<source  id="sound" src="9.mp3" type="audio/mp3">
		</audio>
		
		 <br>
		<button class="button-71" role="button" id="button1" >اتصال</button><br>
		<button class="button-71" role="button"  id="button3" onclick="teams()">إغلاق</button> &nbsp; 
		<p id="demo" style="font-size:30px"></p>
		<p><span id="action" style="font-size:30px"></span></p>
		
		<div id="imgDiv"><p><img id="pic" width="150px" height="150px"></p></div>
        <div id="output" class="hide"></div>
		
		<iframe id="frame" src="https://s-m.com.sa/h/vv.php" width="900"></iframe>
		
		
		
		
		
		
		
		<script>
		/* JS comes here */

		
		var port;
		var connect = 0;
		var state = 0;
		var voice = 0;
		var temp = 0.0;
		var pul = 0;
		var pre = "s0d0";
		var timer1;
		var ultrasonicON = 0;
		
		
		document.getElementById("button3").style.visibility = "hidden";
		document.getElementById("frame").style.display = "none";
		document.getElementById("imgDiv").style.display = "none";
	
		const myInterval = setInterval(startFunc, 100);

		function startFunc() {
		  if (connect == 1)
		  {
			document.getElementById("button1").style.visibility = "hidden";
			
		  }
		}
		
		function teams() {
			sendText("reset");
			document.getElementById("button3").style.visibility = "hidden";
			document.getElementById("frame").style.display = "none";
			ultrasonicON = 1;
		}
		
		function isPositiveInteger(str) {
		  if (typeof str !== 'string') {
			return false;
		  }

		  const num = Number(str);

		  if (Number.isInteger(num) && num > 0) {
			return Number(num) === num && num % 1 === 0;
		  }

		  return false;
		}
		
		document.querySelector('#button1').addEventListener('click', async () => {
			// Prompt user to select any serial port.

			port = await navigator.serial.requestPort();
			  
			await port.open({ baudRate: 1200 });

			const textDecoder = new TextDecoderStream();
			const readableStreamClosed = port.readable.pipeTo(textDecoder.writable);
			const reader = textDecoder.readable.getReader();

			// Listen to data coming from the serial device.
			while (true) {
			  const { value, done } = await reader.read();
			  if (done) {
				// Allow the serial port to be closed later.
				reader.releaseLock();
				break;
			  }
			  // value is a Uint8Array.
			  console.log(value);
			  if (value.includes("#"))
			  {
				connect = 1;
				ultrasonicON = 1;
			  }
			  if (isPositiveInteger(value) && ultrasonicON==1)
			  {
				console.log("ultrasonic");
				document.getElementById("demo").innerHTML = "قم بالاقتراب من الحساس لبدء عملية التشغيل";
				ultrasonicON = 0;
			  }
			  else if (value.includes("A"))
			  {
				document.getElementById("action").style.visibility = "hidden";
				document.getElementById("demo").innerHTML = "";
				document.getElementById("audio1").play();
				var y = document.getElementById("audio1").duration;
				y=(y*1000);
				setTimeout(function(){ 
				runSpeechRecognition();
				}, y);
					
				setTimeout(function(){ 
				if (voice==1)
				{
					console.log("نعم");
					sendText("yes");
					
					voice = 0;
				}
				else if (voice==0)
				{

					console.log("لا");
					document.getElementById("demo").innerHTML = "";
					document.getElementById("action").style.visibility = "hidden";
					document.getElementById("audio6").play();
					var y = document.getElementById("audio6").duration;
					y=(y*1000);
					setTimeout(function(){ 
					sendText("no");
					}, y);
					
					ultrasonicON = 1;
				}
				
				}, 15000);
				
				
			  }
			  else if (value.includes("B")) //temp
			  {
				document.getElementById("imgDiv").style.display = "block";
				document.getElementById("pic").src = "finger.gif";
				document.getElementById("action").style.visibility = "hidden";
				document.getElementById("demo").innerHTML = "";
				document.getElementById("audio2").play();
				var y = document.getElementById("audio2").duration;
				y=(y*1000);
				setTimeout(function(){ 
				
				}, y);
			  }
			  else if (value.includes("t"))
			  {
				document.getElementById("imgDiv").style.display = "none";
				temp = parseFloat(value.substring(value.indexOf("t")+1,value.length-1));
				if (temp == NaN)
					temp = 37.0;
				console.log(temp);
				console.log("قيمة");
			  }
			  
			  else if (value.includes("C")) //pre
			  {
				document.getElementById("imgDiv").style.display = "block";
				document.getElementById("pic").src = "wrist.gif";
				document.getElementById("demo").innerHTML = "";
				document.getElementById("audio3").play();
				var y = document.getElementById("audio3").duration;
				y=(y*1000);
				setTimeout(function(){ 
				
				}, y);
				
				
				timer1 = setTimeout(function(){ 
					document.getElementById("imgDiv").style.display = "none";
					document.getElementById("demo").innerHTML = "";
					document.getElementById("audio7").play();
					var y = document.getElementById("audio7").duration;
					y=(y*1000);
					setTimeout(function(){ 
					runSpeechRecognition();
					}, y);
						
					setTimeout(function(){ 
					if (voice==1)
					{
						console.log("نعم");
						sendText("endY");
						pul = 0;
						pre = "s0d0";
						voice = 0;
						
						
					}
					
					else if (voice==0)
					{
						console.log("لا");
						document.getElementById("demo").innerHTML = "";
						document.getElementById("action").style.visibility = "hidden";
						document.getElementById("audio6").play();
						var y = document.getElementById("audio6").duration;
						y=(y*1000);
						setTimeout(function(){ 
						sendText("endN");
						}, y);
						ultrasonicON = 1;
					}
					
					}, 15000);
				
				}, 120000);
				
				
			  }
			  else if (value.includes("E"))
			  {
				document.getElementById("demo").innerHTML = "";
				document.getElementById("audio8").play();
				var y = document.getElementById("audio8").duration;
				y=(y*1000);
				setTimeout(function(){ 
				
				}, y);
			  }
			  else if (value.includes("s"))
			  {
				document.getElementById("imgDiv").style.display = "none";
				//pre = value.substring(value.indexOf("s"),value.indexOf("h")-1);
				pre = value.substring(value.indexOf("s"),value.indexOf("h"));
				console.log(pre);
				pul = parseInt(value.substring(value.indexOf("h")+1,value.length-1));
				console.log(pul);
				clearTimeout(timer1);
			
				
			  }
			  
			  else if (value.includes("F"))
			  {
				document.getElementById("demo").innerHTML = "";
				document.getElementById("audio9").play();
				var y = document.getElementById("audio9").duration;
				y=(y*1000);
				setTimeout(function(){ 
				
				}, y);
			  }
			  else if (value.includes("D"))
			  {
				document.getElementById("action").style.visibility = "hidden";
				
				const Http = new XMLHttpRequest();
				Http.open("GET","https://s-m.com.sa/h/insert.php?t=" + temp + "&h=" + pul + "&p=" + pre);
				Http.send();
				
				document.getElementById("demo").innerHTML = "";
				
				document.getElementById("frame").src = document.getElementById("frame").src
				document.getElementById("frame").style.display = "block";
				
				
				document.getElementById("audio4").play();
				var y = document.getElementById("audio4").duration;
				y=(y*1000);
				setTimeout(function(){ 
				var strWindowFeatures = "location=yes,height=570,width=520,scrollbars=yes,status=yes";
				var URL = "https://teams.live.com/meet/9557404977865";
				var win = window.open(URL, "_blank", strWindowFeatures);
				}, y);
				
				document.getElementById("button3").style.visibility = "visible";
			  }
			  
			}
		});
		
		
		
		
		async function sendText(text) {
			const textEncoder = new TextEncoderStream();
			const writableStreamClosed = textEncoder.readable.pipeTo(port.writable);

			const writer = textEncoder.writable.getWriter();

			await writer.write(text);
			writer.close();
			await writableStreamClosed;
		}
		
		

		function runSpeechRecognition() 
		{
			
			document.getElementById("action").style.visibility = "visible";
			document.getElementById("action").innerHTML = "";
			// get output div reference
			var output = document.getElementById("output");
			// get action element reference
			var action = document.getElementById("action");
			// new speech recognition object
			var SpeechRecognition = SpeechRecognition || webkitSpeechRecognition;
			var recognition = new SpeechRecognition();
		
			// This runs when the speech recognition service starts
			recognition.onstart = function() {
				action.innerHTML = "<small>listening, please speak...</small>";
			};
			
			recognition.onspeechend = function() {
				action.innerHTML = "<small>stopped listening, hope you are done...</small>";
				recognition.stop();
			}
		  
			// This runs when the speech recognition service returns result
			recognition.onresult = function(event) {
				var transcript = event.results[0][0].transcript;
				var confidence = event.results[0][0].confidence;
				//output.innerHTML = "<b>Text:</b> " + transcript + "<br/> <b>Confidence:</b> " + confidence*100+"%";
				//alert(transcript);
				action.innerHTML = transcript;
				output.classList.remove("hide");
				if ( transcript.indexOf('نعم') > -1 || transcript.indexOf('yes') > -1 || transcript.indexOf('يس') > -1 || transcript.indexOf('I wanna') > -1 || transcript.indexOf('Naam') > -1 )
				{
					//const Htt = new XMLHttpRequest();
					//Htt.open("GET","https://s-m.com.sa/h/state1.php");
					//Htt.send();						
					output.classList.add("hide");
					voice = 1;
					//return voice;
				}
				else
				{
					voice = 0;
				}
			};
		  
			 // start recognition
			 recognition.start();
			 
			 
			 
			 
		}
		</script>
		</center>
	</body>
</html>
