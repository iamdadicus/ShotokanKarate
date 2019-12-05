<link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/materialize/0.95.1/css/materialize.min.css">
<link rel="stylesheet" href="nouislider.css">

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
    <div class="row">
	  <div class="col s12">
		<h6>Random Delay</h6>
	  </div>
      <div class="col s12">
		<div id="test-slider"></div>
        <!--<label>Randomise start (s)</label>
        <p class="range-field">
          <input type="range" id="random-start" min="0" max="20" value="5,25" />
        </p>
      </div>-->	  
    </div>
    <div class="row">
      <div class="input-field col s12">
        <textarea id="message" class="materialize-textarea"></textarea>
        <label>Text To Speech</label>
      </div>
      <div class="col s6">	
		<a href="#" id="kata" class="waves-effect waves-light btn">Kata Test</a>
	  </div>
	  <div class="col s6">		
		<a href="#" id="kumite" class="waves-effect waves-light btn">Kumite Test</a>
	  </div>
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

<script src="https://cdnjs.cloudflare.com/ajax/libs/jquery/2.1.3/jquery.min.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/materialize/0.95.1/js/materialize.min.js"></script>
<script src="nouislider.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/wnumb/1.1.0/wNumb.min.js"></script>
<script>
//use Google 粤語（香港）

//https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Math/random
function getRandomInt(p1, p2) {

  var m = p2 ? p2 - p1 : p1;
  console.log(m, p1, p2);
  return Math.floor(Math.random() * Math.floor(m)) + (p2 ? p1 : 0);
}

var grading = {
	"10th Kyu" : {
		belt : "blue",
		kata	: ["Taikyo-Ku-Shodan"]
	},
	"9th Kyu" : {
		belt   : "red",
		kumite : {
			"kihon ippon" : "set 1" 
		},
		kata	: ["Taikyo-Ku-Shodan"]		
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
			"Age-Uke, Uraken, Mae-Geri, Gyaku-Zuki, Gedan-Barai"
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

function getRandomKata(){

	var belts = Object.keys(grading),
		random_no = getRandomInt(belts.length);

	return grading[ belts[random_no] ].kata;
}

function getRandomKumite(){

	var all_kumite = Object.keys(grading["2nd Kyu"].kumite),
		random_no = getRandomInt(all_kumite.length),
		kumite = all_kumite[ random_no ],
		sets = grading["2nd Kyu"].kumite[ kumite ];
		
	if( sets ){
		
		var random_set_no = getRandomInt( sets[0].length ),
		    option_no	  = getRandomInt( sets[1].length ),
		
		kumite = kumite + " " + sets[0][random_set_no] + " " + sets[1][option_no];
	}	

	return kumite;
}

//https://codepen.io/SteveJRobertson/pen/emGWaR
$(function(){

	$('.collapsible').collapsible();

	var slider = document.getElementById('test-slider');
	  noUiSlider.create(slider, {
	   start: [2, 15],
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
			  .html(voice.name + (voice.default ? ' (default)' :''))
			  .attr('data-lang', voice.lang)
			  .attr('data-default_voice', voice.default);

				//is there a Japanese voice?
			   if( voice.lang == 'ja-JP' ) has_jp = true;
			   
			  $voicelist.append($option);
			});

		  //auto select japanese voice
		  if( has_jp ){ 
			$('option[data-lang="ja-JP"]', $voicelist).attr('selected', true);
		  }
		  else {
			//selected default voice
			$('option[data-default_voice="1"]', $voicelist).attr('selected', true);
		  }

			$voicelist.material_select();
		  }		  
		}

		$('#speak').click(function(){
		
			var text = $('#message').val();
			var msg = new SpeechSynthesisUtterance();
			var voices = window.speechSynthesis.getVoices();
			msg.voice = voices[$('#voices').val()];
			msg.rate = $('#rate').val() / 10;
			msg.pitch = $('#pitch').val();
			msg.text = text;

			msg.onend = function(e) {
				console.log('Finished in ' + e.elapsedTime + ' seconds.');
			};

			console.log("speaking: " + text);
			speechSynthesis.speak(msg);
		});
		
		function speak(msg){
		
			$('#message').focus().val( msg );
			$('#speak').trigger('click');
		};
		  
		$('#kata').click(function(){

			var r = getRandomInt.apply(null, 
										slider.noUiSlider.get().map(function(item) {
																		return parseInt(item, 10);
																	})
									  )||0,
				k = getRandomKata();
		
			console.log( k + " in " + r + "(s)");
			window.setTimeout(speak, r * 1000, k);	 	 
		});	
		
		$('#kumite').click(function(){

			var r = getRandomInt.apply(null, 
										slider.noUiSlider.get().map(function(item) {
																		return parseInt(item, 10);
																	})
									  )||0,
				k = getRandomKumite();
		
			console.log( k + " in " + r + "(s)");
			window.setTimeout(speak, r * 1000, k);	
		});	
		
	  } else {
		$('#modal1').openModal();
	  }
});
</script>
