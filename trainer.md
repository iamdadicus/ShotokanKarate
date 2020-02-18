<link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/materialize/1.0.0/css/materialize.min.css">
<link rel="stylesheet" href="nouislider.css">
<link href="https://fonts.googleapis.com/icon?family=Material+Icons" rel="stylesheet">

<div class="container">
	<div class="row">
		<nav>
			<div class="nav-wrapper">
				<div class="col s12">
					<a href="#" class="brand-logo">Aural Trainer</a>
				</div>
			</div>
		</nav>
	</div>
	<form class="col s8 offset-s2">
<!--	
		<div class="row">
			<label>Choose voice</label>
			<select id="voices"></select>
		</div>
		<div class="row">
			<ul class="collapsible">
				<li>
					<div class="collapsible-header">Speech Rate and Pitch</div>
					<div class="collapsible-body">
						<span>

							<div class="col s6">
								<label>Rate</label>
								<p class="range-field">
									<input type="range" id="rate" min="1" max="100" value="10" />
								</p>
							</div>
							<div class="col s6">
								<label>Pitch</label>
								<p class="range-field">
									<input type="range" id="pitch" min="0" max="2" value="1" />
								</p>
							</div>
							<div class="col s12">
								<p>N.B. Rate and Pitch only work with native voice.</p>
							</div>		
						</span>
					</div>
				</li>
			</ul>	
		</div>
-->		
		<div class="row">
			<div class="col s12">
				<h6>Random Delay</h6>
			</div>
			<div class="col s12">
				<div id="test-slider"></div>
			</div>
		</div>
		<div class="row">
			<div class="input-field col s12">
				<textarea id="message" class="materialize-textarea" readonly="true"></textarea>
				<label></label>
			</div>
			<div class="col s6">	
				<a href="#" id="kata" class="waves-effect waves-light btn">Kata Test</a>
			</div>
			<div class="col s6">		
				<a href="#" id="kumite" class="waves-effect waves-light btn">Kumite Test</a>
			</div>
		</div>	
		<div class="row">
			<div class="col s6">	
				<a href="#" id="kata-next" class="waves-effect waves-light btn"><i class="material-icons left">navigate_next</i>Next</a>
			</div>
			<div class="col s6">		
				<a href="#" id="kumite-next" class="waves-effect waves-light btn"><i class="material-icons left">navigate_next</i>Next</a>
			</div>
		</div>			
		<div class="row">
			<div class="col s12">
				<a href="#" id="speak" class="waves-effect waves-light btn">Speak/Repeat</a>	
			</div>			
		</div>
	</form>
</div>

<div id="modal1" class="modal">
	<h4>Speech Synthesis not supported</h4>
	<p>Your browser does not support speech synthesis.</p>
	<p>We recommend you use Google Chrome.</p>
	<div class="action-bar">
		<a href="#" class="waves-effect waves-green btn-flat modal-action modal-close">Close</a>
	</div>
</div>

<audio id="testAudio" hidden type="audio/mpeg"></audio>

<img src="qr-code.png" height="200px">

<!-- Compiled and minified JavaScript -->
<script
  src="https://code.jquery.com/jquery-3.4.1.min.js"
  integrity="sha256-CSXorXvZcTkaix6Yvo6HppcZGetbYMGWSFlBw8HfCJo="
  crossorigin="anonymous"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/materialize/1.0.0/js/materialize.min.js"></script>
<script src="nouislider.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/wnumb/1.1.0/wNumb.min.js"></script>
<script>
//use Google 粤語（香港）

//https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Math/random
function getRandomInt(p1, p2) {

  var m = p2 ? p2 - p1 : p1;
  return Math.floor(Math.random() * Math.floor(m)) + (p2 ? p1 : 0);
}

// https://itinerarium.github.io/phoneme-synthesis/
var phonetics = [
		["sanbon", "sanbonn"],
		["gohon", "go honn"],
		["kihon", "keyhon"],
		["jiyu", "jeeyu"],
		["migi", "migy"],
		["mae", "may"],
		["geri", "gehry"],		
		["age", "aggi"],
		["uke", "ookey"],
		["uraken", "youracken"], //uoracken earachen
		["gyaku", "guyackhu"], //Guycku 
		["geden", "Gehd dan"], 
		["barai", "bharrai"],
		["uchi", "oochi"],
		["kizami", "kizzahmi"],
		["keage", "keeahgi"],
		["usherio", "ohwsherro"], //yousherrow
		["heian", "hean"],
		["hidari", "hid areye"],
		["nidan", "kneedan"],
		["bassai", "bhass eye"],
		["shodan", "showdan"]
	],
	mp3 = [
		"age uki", 
		"bassai dai",
		"gedan barrai",       
		"gohon",              
		"gyaku zuki",
		"heian godan",        
		"heian nidan",        
		"heian sandan",
		"heian shodan",       
		"heian yondan",       
		"hidari",
		"jiyu ippon",               
		"kihon ippon",        
		"mai geri",
		"migi",               
		"sanbon zuki",
		"sanbon",             
		"set five",           
		"set four",
		"set one",            
		"set three",          
		"set two",
		"taikyo-ku-shodan",   
		"tekki shodan",       
		"uraken"
		],
	syllabus = {
/*
	"10th Kyu" : {
		belt : "blue",
		kata	: ["Taikyo-Ku-Shodan"]
	},
*/	
	"9th Kyu" : {
		belt   : "red",
		kumite : {
			"kihon ippon" : "set 1" 
		},
		kata	: ["taikyo-ku-shodan"]		
	},
	"8th Kyu" : {
		belt   : "orange",
		kumite : {
			"gohon" : "set 1" 
		},
		kata	: ["heian shodan"]
	},
	"7th Kyu" : {
		belt   : "yellow",
		kumite : {
			"sanbon" : "set 1" 
		},
		kata	: ["heian nidan"]
	},
	"6th Kyu" : {
		belt   : "green",
		kumite : {
			"kihon ippon" : "set 2" 
		},
		kata	: ["heian sandan"]	
	},
	"5th Kyu" : {
		belt   : "purple",
		kumite : {
			"kihon ippon" : "set 3" 
		},
		kata	: ["heian yondan"]
	},
	"4th Kyu" : {
		belt   : "purple-white",
		kumite : {
			"kihon ippon" : "set 4" 
		},
		kata	: ["heian godan"]	
	},
	"3rd Kyu" : {
		belt   : "brown",
		kumite : {
			"kihon ippon" : "set 5",
			"jiyu ippon"  : "set 1"
		},
		kata	: ["tekki shodan"]
	},
	"2nd Kyu" : {
		belt	: "brown-white",
		kihon	: [
			"Mae-Geri, Sanbon-Zuki",
			"Age-Uke, Uraken, Mae-Geri, Gyaku-Zuki, Geda-Barai",
			"Soto-Uke, Yoko-Empi, Uraken, Gyaku-Zuki, Gedan-Barai",
			"Uchi-Uke, Kizami-Zuki, Gyaku-Zuki",
			"Shuto-Uke, Kizami-Mawashi-Geri, Gyaku-Zuki, Gedan-Barai",
			"Mae-Geri, Mawashi-Geri, Uraken, Gyaku-Zuki, Gedan-Barai",
			"Mae-Geri, Kekomi, Shuto-Uchi, Gyaku-Zuki, Gedan-Barai",
			"Yoko-Geri-Keage & Kekomi, Gyaku-Zuki, Gedan-Barai",
			"Ushiro-Geri, Uraken, Gyaku-Zuki"
		],
		kumite : {
			"kihon ippon" : [["set five", "set four", "set three", "set two", "set one"],["", "hidari", "migi"]],
			"jiyu ippon"  : [["set two", "set one"],["", "hidari", "migi"]],
			"gohon": false,
			"sanbon" : false
		},	
		kata	: ["bassai dai"]
	}	
};

function getRandomBelt(data){

	var belts = Object.keys(data),
		random_no = getRandomInt(belts.length);

	return belts[random_no];
}

function getRandomBeltKumite(data, belt){

	var all_kumite = Object.keys(data[belt].kumite),
		random_no  = getRandomInt(all_kumite.length),
		kumite     = all_kumite[ random_no ],
		sets       = data[belt].kumite[kumite];

	if( !sets ){
		return [kumite];
	}
			
	var random_set_no = getRandomInt( sets[0].length ),
		option_no	  = getRandomInt( sets[1].length );
		
	return [kumite, sets[0][random_set_no], sets[1][option_no]];	
}

//https://codepen.io/SteveJRobertson/pen/emGWaR
$(function(){

	M.AutoInit();
	
	$('.collapsible').collapsible();
	$('#kumite-next').hide();
	$('#kata-next').hide();
	
	//process mp3 list
	for( var i = 0; i < mp3.length; i++) {

		mp3[i] = [new RegExp( '\\b' + mp3[i].replace(/\s+/i, '\\s+') + '\\b(?![^{]*})'), mp3[i] + ".mp3"];
		
		$('<audio controls preload="auto">')
			.attr('type', 'audio/mpeg')
			.attr('src', 'mp3/' + mp3[i][1])
			.appendTo('body')
			.hide();
	}	

	var slider = document.getElementById('test-slider');
		noUiSlider.create(slider, {
			start: [2, 10],
			connect: true,
			step: 1,
			orientation: 'horizontal', // 'horizontal' or 'vertical'
			range: {
				'min': 0,
				'max': 60
			},
			format: wNumb({
				decimals: 0
			})
		});
	  
	if ('speechSynthesis' in window) {
	  
		speechSynthesis.onvoiceschanged = function() {
		
			var $voicelist = $('#voices'),
				has_jp	 = false;

			if($voicelist.find('option').length == 0) {
			
				speechSynthesis.getVoices().forEach(function(voice, index) {
			
					//console.log(voice);
					var $option = $('<option>')
						.val(index)
						.html((voice.name || voice.voiceURI || voice.lang)+ (voice.default ? ' (default)' :''))
						.attr('data-lang', voice.lang)
						.attr('data-default_voice', voice.default);

					//M.toast({html: 'Found ' + voice.lang});
				
					//is there a Japanese voice?
					if( voice.lang == 'ja-JP' ) has_jp = true;
			   
					$voicelist.append($option);
				});

				//auto select japanese voice
				if( false && has_jp ){ 
					$('option[data-lang="ja-JP"]', $voicelist).attr('selected', true);
				}
				else {
					//selected default voice
					$('option[data-default_voice="1"]', $voicelist).attr('selected', true);
				}

				$voicelist.formSelect();
			}		  
		}
	}
/*	else {
		$('#modal1').openModal();
	}
*/	
	
	$('#speak').click(function(){
	
		var text = $('#message').val().toLowerCase(),
			playlist = [];
		
		for( var i = 0; i < mp3.length; i++) {

			//can we perform a replace?				
			if( mp3[i][0].test(text) ) {
			
				//playlist.push('mp3/' + mp3[i][1]);				
				text = text.replace(mp3[i][0], '{mp3/' + mp3[i][1] + '}').trim();
			}
		}			

		const regex = /{([^}]+)}/gm;
		let m;

		while ((m = regex.exec(text)) !== null) {
		
			// This is necessary to avoid infinite loops with zero-width matches
			if (m.index === regex.lastIndex) {
				regex.lastIndex++;
			}

			// The result can be accessed through the `m`-variable.
			playlist.push(m[1]);//group 1
		}
		
		console.log(playlist);
		
		// https://stackoverflow.com/questions/44366485/how-to-make-multiple-mp3-files-play-one-after-another-on-the-click-of-a-button-u
		var currentTrackIndex = 0;    
		var delayBetweenTracks = 0;
		
		var audio = document.getElementById('testAudio'); 
		
		audio.src = playlist[currentTrackIndex];
		audio.play();
			  
		document.getElementById("testAudio").addEventListener("ended",function(e) {
			   
			$('#kata, #kata-next, #kumite, #kumite-next').attr('disabled', false);
		  setTimeout(function() { 
			currentTrackIndex++;
			if (currentTrackIndex < playlist.length) { 
			  audio.src = playlist[currentTrackIndex];
			  audio.play();
			}
		  }, delayBetweenTracks);
		});			
	});
	
	$('#synthesize-speech').click(function(){
	
		var text = $('#message').val().toLowerCase();
		var msg = new SpeechSynthesisUtterance();
		var voices = window.speechSynthesis.getVoices();
		msg.voice = voices[$('#voices').val()];
		msg.rate = $('#rate').val() / 10;
		msg.pitch = $('#pitch').val();
		
		for( var i = 0; i < phonetics.length; i++) {
			console.log(phonetics[i][0]);
			text = text.replace(phonetics[i][0], phonetics[i][1]);
		}
		
		msg.text = text;

		msg.onend = function(e) {
			console.log('Finished in ' + e.elapsedTime + ' seconds.');
		};

		console.log("speaking: " + text);
		speechSynthesis.speak(msg);
	});
	
	function speak(msg){
	
		//$('#message').focus().val( msg );
		$('#message').val( msg );
		$('#speak').trigger('click');
	};
	  
	$('#kata').click(function(){

		var r = getRandomInt.apply(null, 
									slider.noUiSlider.get().map(function(item) {
																	return parseInt(item, 10);
																})
								  )||0,
			b = getRandomBelt(syllabus),
			k = syllabus[b],
			c = $.extend(true, {}, syllabus); //copy grading syllabus
	
		delete c[b]; //remove selected belt from copy
		
		//assign copy to next button
		$('#kata-next')
			.data(c)
			.show();
		
		console.log( k.kata + " in " + r + "(s)");
		window.setTimeout(speak, r * 1000, k.kata);	 	 
		$(this).attr('disabled', true);
	});	
	
	$('#kata-next').click(function(){

		var r = getRandomInt.apply(null, 
									slider.noUiSlider.get().map(function(item) {
																	return parseInt(item, 10);
																})
								  )||0,
			s = $('#kata-next').data(),	//syllabus stored in next button
			b = getRandomBelt(s),
			k = syllabus[b];
	
		delete s[b]; //remove selected belt from copy
		
		//re-assign syllabus next button
		$('#kata-next').data(s);
		
		//hide next button if this is the last
		if( !Object.keys(s).length ){
		
			$('#kata-next').hide();
		}
		
		console.log( k.kata + " in " + r + "(s)");
		window.setTimeout(speak, r * 1000, k.kata);	 	
		$(this).attr('disabled', true);		
	});		
	
	$('#kumite').click(function(){

		var r = getRandomInt.apply(null, 
									slider.noUiSlider.get().map(function(item) {
																	return parseInt(item, 10);
																})
								  )||0,
			b = "2nd Kyu",				
			k = getRandomBeltKumite(syllabus, b),				
			s = k.join(' '),
			c = $.extend(true, {}, syllabus); //copy grading syllabus
			
		if( k.length > 1 ){
		
			var kumite_name = k[0],
				set_name    = k[1],
				set_array 	= c[b].kumite[kumite_name];

			//remove selected kumite set
			var index = set_array[0].indexOf(set_name);
			
			if (index > -1) {
			  set_array[0].splice(index, 1);
			}				
			
			console.log(set_array);
			c[b].kumite[ kumite_name ][ set_name ] = set_array;
		}
		else {
		
			delete c[b].kumite[ k[0] ]; //remove selected kumite from belt
		}
	
		//re-assign syllabus next button
		$('#kumite-next')
			.data(c)
			.show();
		
		console.log( s + " in " + r + "(s)");
		window.setTimeout(speak, r * 1000, s);	
		$(this).attr('disabled', true);
	});	
	
	$('#kumite-next').click(function(){

		var r = getRandomInt.apply(null, 
									slider.noUiSlider.get().map(function(item) {
																	return parseInt(item, 10);
																})
								  )||0,
			c = $('#kumite-next').data(),	//syllabus stored in next button
			b = "2nd Kyu",				
			k = getRandomBeltKumite(c, b),				
			s = k.join(' ');
			
		if( k.length > 1 ){
		
			var kumite_name = k[0],
				set_name    = k[1],
				set_array 	= c[b].kumite[kumite_name];

			//remove selected kumite set
			var index = set_array[0].indexOf(set_name);
			
			if (index > -1) {
			  set_array[0].splice(index, 1);
			}				
			
			if( !set_array[0].length ){
			
				delete c[b].kumite[ kumite_name ];
			}
		}
		else {
		
			delete c[b].kumite[ k[0] ]; //remove selected kumite from belt
		}
		
		//re-assign syllabus next button
		$('#kumite-next').data(c);
		
		//hide next button if this is the last
		if( !Object.keys(c[b].kumite).length ){
		
			$('#kumite-next').hide();
		}
		
		console.log( s + " in " + r + "(s)");
		window.setTimeout(speak, r * 1000, s);		 
		$(this).attr('disabled', true);
	});			
});
</script>
