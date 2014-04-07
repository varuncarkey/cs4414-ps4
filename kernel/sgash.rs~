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
	    //if size is not 0 delete from array of chars(for string)
	}
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

fn screen() {
    putstr(&"\n                                                               "); 
    putstr(&"\n                                                               ");
    putstr(&"\n                       7=..~$=..:7                             "); 
    putstr(&"\n                  +$: =$$$+$$$?$$$+ ,7?                        "); 
    putstr(&"\n                  $$$$$$$$$$$$$$$$$$Z$$                        ");
    putstr(&"\n              7$$$$$$$$$$$$. .Z$$$$$Z$$$$$$                    ");
    putstr(&"\n           ~..7$$Z$$$$$7+7$+.?Z7=7$$Z$$Z$$$..:                 ");
    putstr(&"\n          ~$$$$$$$$7:     :ZZZ,     :7ZZZZ$$$$=                ");
    putstr(&"\n           Z$$$$$?                    .+ZZZZ$$                 ");
    putstr(&"\n       +$ZZ$$$Z7                         7ZZZ$Z$$I.            "); 
    putstr(&"\n        $$$$ZZZZZZZZZZZZZZZZZZZZZZZZI,    ,ZZZ$$Z              "); 
    putstr(&"\n      :+$$$$ZZZZZZZZZZZZZZZZZZZZZZZZZZZ=    $ZZ$$+~,           "); 
    putstr(&"\n     ?$Z$$$$ZZZZZZZZZZZZZZZZZZZZZZZZZZZZI   7ZZZ$ZZI           "); 
    putstr(&"\n      =Z$$+7Z$$7ZZZZZZZZ$$$$$$$ZZZZZZZZZZ  ~Z$?$ZZ?            ");	 
    putstr(&"\n    :$Z$Z...$Z  $ZZZZZZZ~       ~ZZZZZZZZ,.ZZ...Z$Z$~          "); 
    putstr(&"\n    7ZZZZZI$ZZ  $ZZZZZZZ~       =ZZZZZZZ7..ZZ$?$ZZZZ$          "); 
    putstr(&"\n      ZZZZ$:    $ZZZZZZZZZZZZZZZZZZZZZZ=     ~$ZZZ$:           "); 
    putstr(&"\n    7Z$ZZ$,     $ZZZZZZZZZZZZZZZZZZZZ7         ZZZ$Z$          "); 
    putstr(&"\n   =ZZZZZZ,     $ZZZZZZZZZZZZZZZZZZZZZZ,       ZZZ$ZZ+         "); 
    putstr(&"\n     ,ZZZZ,     $ZZZZZZZ:     =ZZZZZZZZZ     ZZZZZ$:           "); 
    putstr(&"\n    =$ZZZZ+     ZZZZZZZZ~       ZZZZZZZZ~   =ZZZZZZZI          "); 
    putstr(&"\n    $ZZ$ZZZ$$Z$$ZZZZZZZZZ$$$$   IZZZZZZZZZ$ZZZZZZZZZ$          "); 
    putstr(&"\n      :ZZZZZZZZZZZZZZZZZZZZZZ   ~ZZZZZZZZZZZZZZZZZ~            "); 
    putstr(&"\n     ,Z$$ZZZZZZZZZZZZZZZZZZZZ    ZZZZZZZZZZZZZZZZZZ~           "); 
    putstr(&"\n     =$ZZZZZZZZZZZZZZZZZZZZZZ     $ZZZZZZZZZZZZZZZ$+           "); 
    putstr(&"\n        IZZZZZ:.                        . ,ZZZZZ$              "); 
    putstr(&"\n       ~$ZZZZZZZZZZZ                 ZZZZ$ZZZZZZZ+             "); 
    putstr(&"\n           Z$ZZZ. ,Z~               =Z:.,ZZZ$Z                 "); 
    putstr(&"\n          ,ZZZZZ..~Z$.             .7Z:..ZZZZZ:                ");
    putstr(&"\n          ~7+:$ZZZZZZZZI=:.   .,=IZZZZZZZ$Z:=7=                ");
    putstr(&"\n              $$ZZZZZZZZZZZZZZZZZZZZZZ$ZZZZ                    ");
    putstr(&"\n              ==..$ZZZ$ZZZZZZZZZZZ$ZZZZ .~+                    ");
    putstr(&"\n                  I$?.?ZZZ$ZZZ$ZZZI =$7                        ");
    putstr(&"\n                       $7..I$7..I$,                            ");
    putstr(&"\n"); 
    putstr(&"\n _                     _     _                         _  ");
    putstr(&"\n| |                   (_)   | |                       | | ");
    putstr(&"\n| | ____ ___  ____     _____| |_____  ____ ____  _____| | ");
    putstr(&"\n| |/ ___) _ \\|  _ \\   |  _   _) ___ |/ ___)  _ \\| ___ | | ");
    putstr(&"\n| | |  | |_| | | | |  | |  \\ \\| ____| |   | | | | ____| | ");
    putstr(&"\n|_|_|  \\____/|_| |_|  |_|   \\_\\_____)_|   |_| |_|_____)__)\n\n");
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

unsafe fn prompt(startup: bool){
	//putstr(&"\nsgash > ");
	//if !startup {drawstr(&"\nsgash > ");
	if(buffer.len()!=0)
	{
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



