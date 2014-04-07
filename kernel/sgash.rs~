/* kernel::sgash.rs */
#[allow(unused_imports)];
use core::*;
use core::str::*;
use core::option::{Some, Option, None}; 
use core::iter::Iterator;
use kernel::*;
use super::super::platform::*;
use kernel::memory::Allocator;

pub static mut buffer: cstr = cstr {
				p: 0 as *mut u8,
				p_cstr_i: 0,
				max: 0
			      };


pub fn putchar(key: char) {
    unsafe {
	/*
	 * We need to include a blank asm call to prevent rustc
	 * from optimizing this part out
	 */
	asm!("");
	io::write_char(key, io::UART0);
    }
}

fn putstr(msg: &str) {
    for c in slice::iter(as_bytes(msg)) {
	putchar(*c as char);
    }	
}

pub unsafe fn drawstr(msg: &str) {
    let old_fg = super::super::io::FG_COLOR;
    let mut x: u32 = 0xFFFFFFFF;
    for c in slice::iter(as_bytes(msg)) {
	x = (x << 8) + (x >> 24); 
	super::super::io::set_fg(x);
	drawchar(*c as char);
    }
    super::super::io::set_fg(old_fg);
}

unsafe fn drawchar(x: char)
{
    io::restore();
    if x == '\n' {
	io::CURSOR_Y += io::CURSOR_HEIGHT;
	io::CURSOR_X = 0u32;
    } else {
	io::draw_char(x);	
	io::CURSOR_X += io::CURSOR_WIDTH;
    }
    io::backup();
    io::draw_cursor();
}

unsafe fn backspace()
{
    io::restore();
    if (io::CURSOR_X >= io::CURSOR_WIDTH) { 
	io::CURSOR_X -= io::CURSOR_WIDTH;
	io::draw_char(' ');
    }
    io::backup();
    io::draw_cursor();
}

pub unsafe fn parsekey(x: char) {
    let x = x as u8;
    //let string:~[char]=~[];
    // Set this to false to learn the keycodes of various keys!
    // Key codes are printed backwards because life is hard
//keycode(x);
    match x { 
	13		=>	{ 
	    //putstr(&"\n");
	    //drawstr(&"\n");
	    prompt(false);

	}
	127		=>	{ 
	    if(buffer.delete_char())
	    {
		    putchar('');
		    putchar(' ');
		    putchar(''); 
		    backspace();
	    }
	}

	    //if size is not 0 delete from array of chars(for string)
	27		=>	{
}


/*match ((x%10)%10){
	91 => {drawchar('y')}
	_ => {drawchar((((x%10)%10) +0 )as char);}
*/
//drawchar('L');
//drawchar('E');
//drawchar('F');
//drawchar('T');



	   /* putchar((x%10) as char);
	    putchar((x%10)  as char);
putchar((x%10) as char);
putchar((x%10) as char);

}
*/
 	//68 		=>	{
	//    drawchar('L');
//}
	_		=>	{ 
	    if io::CURSOR_X < io::SCREEN_WIDTH-io::CURSOR_WIDTH {
		if(buffer.add_char(x)){		
		putchar(x as char);
		drawchar(x as char);	
		}
		
		//Store to Array of chars

	    }
	}
    }
}

fn keycode(x: u8) {
   let mut x = x;
   while x != 0 {
       putchar((x%10+ ('0' as u8)) as char);
       x= x/10;
   }
   putchar(' ');
}

fn screen() {

   putstr(&"\n  .__                           __.                  "); 
   putstr(&"\n    \\ `\\~~---..---~~~~~~--.---~~| /                  "); 
   putstr(&"\n     `~-.   `                   .~         _____     "); 
   putstr(&"\n         ~.                .--~~    .---~~~    /     "); 
   putstr(&"\n          / .-.      .-.      |  <~~        __/      "); 
   putstr(&"\n         |  |_|      |_|       \\  \\     .--'         "); 
   putstr(&"\n        /-.      -       .-.    |  \\_   \\_           "); 
   putstr(&"\n        \\-'   -..-..-    `-'    |    \\__  \\_         "); 
   putstr(&"\n         `.                     |     _/  _/         "); 
   putstr(&"\n           ~-                .,-\\   _/  _/           "); 
   putstr(&"\n          /                 -~~~~\\ /_  /_            "); 
   putstr(&"\n         |               /   |    \\  \\_  \\_          "); 
   putstr(&"\n         |   /          /   /      | _/  _/          "); 
   putstr(&"\n         |  |          |   /    .,-|/  _/            "); 
   putstr(&"\n         )__/           \\_/    -~~~| _/              "); 
   putstr(&"\n           \\                      /  \\               "); 
   putstr(&"\n            |           |        /_---`              "); 
   putstr(&"\n            \\    .______|      ./                    "); 
   putstr(&"\n            (   /        \\    /                      "); 
   putstr(&"\n            `--'          /__/                       "); 
   putstr(&"\n   ________   ____  _    __        _     ___    _    ");
   putstr(&"\n   \\       \\  \\_  |/ \\  /_/   __  / |   /_  \\  | |   ");
   putstr(&"\n    \\_  \\_  \\ __\\    /  ___  |  \\/  | ___ |  \\ | |   ");
   putstr(&"\n      \\   __//  \\|   \\ / |_\\ |      |/   \\|   \\| |   ");
   putstr(&"\n       \\  \\ | |_ |    \\\\  --|| |\\/| |  |_ | |\\   |   ");
   putstr(&"\n        \\__\\ \\__/|_|\\__|\\__/ |_|  |_|\\___/|_| |__|   ");

}
unsafe fn echo()
{
 let mut x=5;
//drawchar('t');
 while x < buffer.len()
	{
		
	//drawchar('g'); //test to see if working
	let bufferp=*(((buffer.p as uint)+x) as *mut char);	
	//let mut bufferp: char = ((*buffer.p as uint) + x) as char; 
	//putchar(*(bufferp as *char));
	putchar(bufferp);
	drawchar(bufferp);
	//drawchar((bufferp) as char);//GET THIS TO Work	
	x+=1;	
	}
}

unsafe fn pokecho()
{
 let mut x=0;
//drawchar('t');
 while x < buffer.len()
	{
		
	//drawchar('g'); //test to see if working
	let bufferp=*(((buffer.p as uint)+x) as *mut char);	
	//let mut bufferp: char = ((*buffer.p as uint) + x) as char; 
	//putchar(*(bufferp as *char));
	//putstr("\n");
	//putchar(bufferp);
	drawchar(bufferp);
	//drawchar((bufferp) as char);//GET THIS TO Work	
	x+=1;	
	}
}

unsafe fn prompt(startup: bool){
	//putstr(&"\nsgash > ");
	//if !startup {drawstr(&"\nsgash > ");

	if(buffer.len()!=0)
	{
	if(buffer.matchStr(&"bulbasaur") || buffer.matchStr(&"Bulbasaur"))
	{ drawstr("\n Ivysaur");}
else if(buffer.matchStr(&"Ivysaur") || buffer.matchStr(&"ivysaur"))
	{ drawstr("\n Venusaur");}
else if(buffer.matchStr(&"Charmander") || buffer.matchStr(&"charmander"))
	{ drawstr("\n Charmeleon");}
else if(buffer.matchStr(&"Charmeleon") || buffer.matchStr(&"charmeleon"))
	{ drawstr("\n Charizard");}
else if(buffer.matchStr(&"Squirtle") || buffer.matchStr(&"squirtle"))
	{ drawstr("\n Wartortle");}
else if(buffer.matchStr(&"Wartortle") || buffer.matchStr(&"wartortle"))
	{ drawstr("\n Blastoise");}
else if(buffer.matchStr(&"Caterpie") || buffer.matchStr(&"caterpie"))
	{ drawstr("\n Metapod");}
else if(buffer.matchStr(&"Metapod") || buffer.matchStr(&"metapod"))
	{ drawstr("\n Butterfree");}
else if(buffer.matchStr(&"Weedle") || buffer.matchStr(&"weedle"))
	{ drawstr("\n Kakuna");}
else if(buffer.matchStr(&"Kakuna") || buffer.matchStr(&"kakuna"))
	{ drawstr("\n Beedrill");}
else if(buffer.matchStr(&"Pidgey") || buffer.matchStr(&"pidgey"))
	{ drawstr("\n Pidgeotto");}
else if(buffer.matchStr(&"Pidgeotto") || buffer.matchStr(&"pidgeotto"))
	{ drawstr("\n Pidgeot");}
	else if(buffer.matchStr(&"Rattata") || buffer.matchStr(&"rattata"))
	{ drawstr("\n Raticate");}


else if(buffer.matchStr(&"Spearow") || buffer.matchStr(&"spearow"))
	{ drawstr("\n Fearow");}
else if(buffer.matchStr(&"Ekans") || buffer.matchStr(&"ekans"))
	{ drawstr("\n Arbok");}
else if(buffer.matchStr(&"Pikachu") || buffer.matchStr(&"pikachu"))
	{ drawstr("\n Raichu");}

else if(buffer.matchStr(&"Sandshrew") || buffer.matchStr(&"sandshrew"))
	{ drawstr("\n Sandslash");}
else if(buffer.matchStr(&"Nidoran") || buffer.matchStr(&"nidoran"))
	{ drawstr("\n Nidorina/Nidorino");}
else if(buffer.matchStr(&"Nidorina") || buffer.matchStr(&"nidorina"))
	{ drawstr("\n Nidoqueen");}
else if(buffer.matchStr(&"Nidorino") || buffer.matchStr(&"nidorino"))
	{ drawstr("\n Nidoking");}
else if(buffer.matchStr(&"Clefairy") || buffer.matchStr(&"clefairy"))
	{ drawstr("\n Clefable");}
else if(buffer.matchStr(&"Vulpix") || buffer.matchStr(&"vulpix"))
	{ drawstr("\n Ninetales");}
else if(buffer.matchStr(&"Jigglypuff") || buffer.matchStr(&"jigglypuff"))
	{ drawstr("\n Wigglytuff");}
else if(buffer.matchStr(&"Zubat") || buffer.matchStr(&"zubat"))
	{ drawstr("\n Golbat");}
else if(buffer.matchStr(&"Oddish") || buffer.matchStr(&"oddish"))
	{ drawstr("\n Gloom");}
else if(buffer.matchStr(&"Gloom") || buffer.matchStr(&"gloom"))
	{ drawstr("\n Vileplume");}
else if(buffer.matchStr(&"Paras") || buffer.matchStr(&"paras"))
	{ drawstr("\n Parasect");}
else if(buffer.matchStr(&"Venonat") || buffer.matchStr(&"venonat"))
	{ drawstr("\n Venomoth");}
else if(buffer.matchStr(&"Diglett") || buffer.matchStr(&"diglett"))
	{ drawstr("\n Dugtrio");}
else if(buffer.matchStr(&"Meowth") || buffer.matchStr(&"meowth"))
	{ drawstr("\n Persian");}
else if(buffer.matchStr(&"Psyduck") || buffer.matchStr(&"psyduck"))
	{ drawstr("\n Golduck");}
else if(buffer.matchStr(&"Mankey") || buffer.matchStr(&"mankey"))
	{ drawstr("\n Primeape");}
else if(buffer.matchStr(&"Growlithe") || buffer.matchStr(&"growlithe"))
	{ drawstr("\n Arcanine");}
else if(buffer.matchStr(&"Poliwag") || buffer.matchStr(&"poliwag"))
	{ drawstr("\n Poliwhirl");}
else if(buffer.matchStr(&"Poliwhirl") || buffer.matchStr(&"poliwhirl"))
	{ drawstr("\n Poliwrath");}
else if(buffer.matchStr(&"Abra") || buffer.matchStr(&"abra"))
	{ drawstr("\n Kadabra");}
else if(buffer.matchStr(&"Kadabra") || buffer.matchStr(&"kadabra"))
	{ drawstr("\n Alakazam");}
else if(buffer.matchStr(&"Machop") || buffer.matchStr(&"machop"))
	{ drawstr("\n Machoke");}
else if(buffer.matchStr(&"Machoke") || buffer.matchStr(&"machoke"))
	{ drawstr("\n Machamp");}
else if(buffer.matchStr(&"Bellsprout") || buffer.matchStr(&"bellsprout"))
	{ drawstr("\n Weepinbell");}
else if(buffer.matchStr(&"Weepinbell") || buffer.matchStr(&"weepinbell"))
	{ drawstr("\n Victreebel");}
else if(buffer.matchStr(&"Tentacool") || buffer.matchStr(&"tentacool"))
	{ drawstr("\n Tentacruel");}
else if(buffer.matchStr(&"Geodude") || buffer.matchStr(&"geodude"))
	{ drawstr("\n Graveler");}
else if(buffer.matchStr(&"Graveler") || buffer.matchStr(&"graveler"))
	{ drawstr("\n Golem");}
else if(buffer.matchStr(&"Ponyta") || buffer.matchStr(&"ponyta"))
	{ drawstr("\n Rapidash");}
else if(buffer.matchStr(&"Slowpoke") || buffer.matchStr(&"slowpoke"))
	{ drawstr("\n Slowbro");}


else if(buffer.matchStr(&"Slowbro") || buffer.matchStr(&"slowbro"))
	{ pokecho();}
else if(buffer.matchStr(&"Magnemite") || buffer.matchStr(&"magnemite"))
	{ drawstr("\n Magneton");}
else if(buffer.matchStr(&"Farfetch'd") || buffer.matchStr(&"farfetch'd"))
	{ pokecho();}
else if(buffer.matchStr(&"Doduo") || buffer.matchStr(&"doduo"))
	{ drawstr("\n Dodrio");}
else if(buffer.matchStr(&"Seel") || buffer.matchStr(&"seel"))
	{ drawstr("\n Dewgong");}
else if(buffer.matchStr(&"Grimer") || buffer.matchStr(&"grimer"))
	{ drawstr("\n Muk");}
else if(buffer.matchStr(&"Shellder") || buffer.matchStr(&"shellder"))
	{ drawstr("\n Cloyster");}
else if(buffer.matchStr(&"Gastly") || buffer.matchStr(&"gastly"))
	{ drawstr("\n Haunter");}
else if(buffer.matchStr(&"Haunter") || buffer.matchStr(&"haunter"))
	{ drawstr("\n Gengar");}
else if(buffer.matchStr(&"Onix") || buffer.matchStr(&"onix"))
	{ pokecho();}
else if(buffer.matchStr(&"Drowzee") || buffer.matchStr(&"drowzee"))
	{ drawstr("\n Hypno");}
else if(buffer.matchStr(&"Krabby") || buffer.matchStr(&"krabby"))
	{ drawstr("\n Kingler");}
else if(buffer.matchStr(&"Voltorb") || buffer.matchStr(&"voltorb"))
	{ drawstr("\n Electrode");}
else if(buffer.matchStr(&"Exeggcute") || buffer.matchStr(&"exeggcute"))
	{ drawstr("\n Exeggutor");}
else if(buffer.matchStr(&"Cubone") || buffer.matchStr(&"cubone"))
	{ drawstr("\n Marowak");}
else if(buffer.matchStr(&"Hitmonlee") || buffer.matchStr(&"hitmonlee"))
	{ drawstr("\n Hitmonchan");}
else if(buffer.matchStr(&"Lickitung") || buffer.matchStr(&"lickitung"))
	{ pokecho();}
else if(buffer.matchStr(&"Koffing") || buffer.matchStr(&"koffing"))
	{ drawstr("\n Weezing");}
else if(buffer.matchStr(&"Rhyhorn") || buffer.matchStr(&"rhyhorn"))
	{ drawstr("\n Rhydon");}
else if(buffer.matchStr(&"Chansey") || buffer.matchStr(&"chansey"))
	{ pokecho();}
else if(buffer.matchStr(&"Tangela") || buffer.matchStr(&"tangela"))
	{ pokecho();}
else if(buffer.matchStr(&"Kangaskhan") || buffer.matchStr(&"kangaskhan"))
	{ pokecho();}
else if(buffer.matchStr(&"Horsea") || buffer.matchStr(&"horsea"))
	{ drawstr("\n Seadra");}
else if(buffer.matchStr(&"Goldeen") || buffer.matchStr(&"goldeen"))
	{ drawstr("\n Seaking");}
else if(buffer.matchStr(&"Staryu") || buffer.matchStr(&"staryu"))
	{ drawstr("\n Starmie");}
else if(buffer.matchStr(&"Starmie") || buffer.matchStr(&"starmie"))
	{ pokecho();}
else if(buffer.matchStr(&"Mr. Mime") || buffer.matchStr(&"mr. mime"))
	{ pokecho();}
else if(buffer.matchStr(&"Scyther") || buffer.matchStr(&"scyther"))
	{ pokecho();}
else if(buffer.matchStr(&"Jynx") || buffer.matchStr(&"jynx"))
	{ pokecho();}
else if(buffer.matchStr(&"electabuzz") || buffer.matchStr(&"Electabuzz"))
	{ pokecho();}
else if(buffer.matchStr(&"Magmar") || buffer.matchStr(&"magmar"))
	{ pokecho();}
else if(buffer.matchStr(&"Pinsir") || buffer.matchStr(&"pinsir"))
	{ pokecho();}
else if(buffer.matchStr(&"Tauros") || buffer.matchStr(&"tauros"))
	{ pokecho();}
else if(buffer.matchStr(&"Magikarp") || buffer.matchStr(&"magikarp"))
	{ drawstr("\n Gyarados");}
else if(buffer.matchStr(&"Lapras") || buffer.matchStr(&"lapras"))
	{ pokecho();}
else if(buffer.matchStr(&"ditto") || buffer.matchStr(&"Ditto"))
	{ pokecho();}
else if(buffer.matchStr(&"Eevee") || buffer.matchStr(&"eevee"))
	{ drawstr("\n Vaporeon/Jolteon/Flareon");}
else if(buffer.matchStr(&"Porygon") || buffer.matchStr(&"porygon"))
	{ pokecho();}
else if(buffer.matchStr(&"Omanyte") || buffer.matchStr(&"omanyte"))
	{ drawstr("\n Omastar");}
else if(buffer.matchStr(&"Kabuto") || buffer.matchStr(&"kabuto"))
	{ drawstr("\n Kabutops");}
else if(buffer.matchStr(&"Aerodactyl") || buffer.matchStr(&"aerodactyl"))
	{ pokecho();}
else if(buffer.matchStr(&"Snorlax") || buffer.matchStr(&"snorlax"))
	{ pokecho();}
else if(buffer.matchStr(&"Articuno") || buffer.matchStr(&"articuno"))
	{ pokecho();}
else if(buffer.matchStr(&"Zapdos") || buffer.matchStr(&"zapdos"))
	{ pokecho();}
else if(buffer.matchStr(&"Moltres") || buffer.matchStr(&"moltres"))
	{ pokecho();}
else if(buffer.matchStr(&"Dratini") || buffer.matchStr(&"dratini"))
	{ drawstr("\n Dragonair");}
else if(buffer.matchStr(&"Dragonair") || buffer.matchStr(&"dragonair"))
	{ drawstr("\n Dragonite");}
else if(buffer.matchStr(&"Mewtwo") || buffer.matchStr(&"mewtwo"))
	{ pokecho();}
else if(buffer.matchStr(&"Mew") || buffer.matchStr(&"mew"))
	{ pokecho();}
//else {pokecho();}
	putstr(&"\n"); 
	drawstr(&"\n");
 	if(buffer.matchStr(&"echo "))
	{echo();}
	else if(buffer.matchStr(&"ls "))
	{ drawstr("\nTHIS ls COMMAND DOES NOT CURRENTLY WORK\n");}
	else if(buffer.matchStr(&"cat "))
	{ drawstr("\nTHIS cat COMMAND DOES NOT CURRENTLY WORK\n");}
	else if(buffer.matchStr(&"cd "))
	{ drawstr("\nTHIS cd COMMAND DOES NOT CURRENTLY WORK\n");}
	else if(buffer.matchStr(&"rm "))
	{ drawstr("\nTHIS rm COMMAND DOES NOT CURRENTLY WORK\n");}
	else if(buffer.matchStr(&"mkdir "))
	{ drawstr("\nTHIS mkdir COMMAND DOES NOT CURRENTLY WORK\n");}
	else if(buffer.matchStr(&"pwd "))
	{ drawstr("\nTHIS pwd COMMAND DOES NOT CURRENTLY WORK\n");}
	else if(buffer.matchStr(&"wr "))
	{ drawstr("\nTHIS wr COMMAND DOES NOT CURRENTLY WORK\n");}
else {pokecho();}
	}

	
	//After printing it out we gotta determine if its a command
	//if it is send it to another function to handle it.
	//else dont clear buffer and go on.

	buffer.reset();	
	putstr(&"\nsgash > ");
	if !startup {drawstr(&"\nsgash > ");
	}
	
}

pub unsafe fn init() {
 	buffer = cstr::new(256);
	//drawchar((((*echoComm.p as uint) + 0) as u8) as char);
	//drawchar((((*echoComm.p as uint) + 1) as u8) as char);    
	screen();
	prompt(true);
}
//CREATED THE C STRING HERE
struct cstr {
	p: *mut u8,
	p_cstr_i: uint,
	max: uint 
}

impl cstr {
	pub unsafe fn new(size: uint) -> cstr {
		// Sometimes this doesn't allocate enough memory and gets stuck...
		let (x, y) = heap.alloc(size);
		let this = cstr {
			p: x,
			p_cstr_i: 0,
			max: y
		};
		*(((this.p as uint)+this.p_cstr_i) as *mut char) = '\0';
		this
	}

#[allow(dead_code)]
	fn len(&self) -> uint { return self.p_cstr_i }

	// HELP THIS DOESN'T WORK THERE IS NO GARBAGE COLLECTION!!!
	// -- TODO: exchange_malloc, exchange_free
	unsafe fn add_char(&mut self, x: u8) -> bool{
		if (self.p_cstr_i == self.max) { return false; }
		*(((self.p as uint)+self.p_cstr_i) as *mut u8) = x;
		self.p_cstr_i += 1;
		*(((self.p as uint)+self.p_cstr_i) as *mut char) = '\0';
		return true
	}

	unsafe fn delete_char(&mut self) -> bool {
		if (self.p_cstr_i == 0) { return false; }
		self.p_cstr_i -= 1;
		*(((self.p as uint)+self.p_cstr_i) as *mut char) = '\0';
		return true
	}

	unsafe fn reset(&mut self) {
		self.p_cstr_i = 0; 
		*(self.p as *mut char) = '\0';
	}

#[allow(dead_code)]
	unsafe fn eq(&self, other: &cstr) -> bool {
		if (self.len() != other.len()) { return false; }
		else {
			let mut x = 0;
			let mut selfp: uint = self.p as uint;
			let mut otherp: uint = other.p as uint;
			while x < self.len() {
				if (*(selfp as *char) != *(otherp as *char)) { return false; }
				selfp += 1;
				otherp += 1;
				x += 1;
			}
			true
		}
	}

	unsafe fn streq(&self, other: &str) -> bool {
		let mut selfp: uint = self.p as uint;
		for c in slice::iter(as_bytes(other)) {
			if( *c != *(selfp as *u8) ) { return false; }
			selfp += 1;
		};
		*(selfp as *char) == '\0'
	}

	unsafe fn getarg(&self, delim: char, mut k: uint) -> Option<cstr> {
		let mut ind: uint = 0;
		let mut found = k == 0;
		let mut selfp: uint = self.p as uint;
		let mut s = cstr::new(256);
		loop {
			if (*(selfp as *char) == '\0') { 
				// End of string
				if (found) { return Some(s); }
				else { return None; }
			};
			if (*(selfp as *u8) == delim as u8) { 
				if (found) { return Some(s); }
				k -= 1;
			};
			if (found) {
				s.add_char(*(selfp as *u8));
			};
			found = k == 0;
			selfp += 1;
			ind += 1;
			if (ind == self.max) { 
				putstr(&"\nSomething broke!");
				return None; 
			}
		}
	}
	unsafe fn matchStr(&self, string: &str) -> bool
	{
		let mut x =0;
		let mut matched=true;
		let mut selfp: uint = self.p as uint;
		//drawstr("\nget\n");		
		for c in slice::iter(as_bytes(string)) {
			 
			if(x<self.len()){
	  		if(*(((self.p as uint)+x) as *mut char) as u8!=(*c as char)as u8)		  
			{
			//drawstr("\n");			
			//drawchar(*c as char);
			//drawstr("\n"); 
			//drawchar(*(((self.p as uint)+x) as *mut char)); 
			//drawstr("\n"); 			
			matched=false;
			break;
			}		
			x+=1;
			//drawstr("\n\n\n");
			}
			else
			{matched=false;}
		}
		return matched;
		
	} 
#[allow(dead_code)]
	unsafe fn split(&self, delim: char) -> (cstr, cstr) {
		let mut selfp: uint = self.p as uint;
		let mut beg = cstr::new(256);
		let mut end = cstr::new(256);
		let mut found = false;
		loop {
			if (*(selfp as *char) == '\0') { 
				return (beg, end);
			}
			else if (*(selfp as *u8) == delim as u8) {
				found = true;
			}
			else if (!found) {
				beg.add_char(*(selfp as *u8));
			}
			else if (found) {
				end.add_char(*(selfp as *u8));
			};
			selfp += 1;
		}
	}	

	/*unsafe fn split(&self, delim: char) -> (cstr, cstr) {
		let mut selfp: uint = self.p as uint;
		
		let mut beg = cstr::new(256);
		let mut end = cstr::new(256);
		let mut found = false;
		let mut x=0;
		loop {
			let current =(*(((self.p as uint)+x) as * mut char));
			//drawchar(current);			
			if (x==self.len()) {	
				return (beg, end);
			}
			else if (found) {
				//drawchar(current);
				end.add_char(current as u8);
			}			
			else if (current==delim) {
				//drawchar('g');
				found = true;
			}
			else if (!found) {
				//drawchar(current);
				beg.add_char(current as u8);
					
			}
			;
			x += 1;
		}
	}*/




}

//ATTEMPT AT FILES
/*
struct Files {
	direc: bool,
	contents: ~str,
	tail: Option<Files>
}

impl Files
{
	pub unsafe fn addDirectory(File: Files, mut root: Files) -> Files
	{
	  root.tail=Some(File);
	}
	pub unsafe fn rmDirectory(File: Files, mut root: Files) -> Files
	{
	  root.tail=None;//assuming this is the directory or find it otherwise.
	}


}*/


