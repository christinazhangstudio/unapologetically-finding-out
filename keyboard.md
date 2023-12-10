# keyboard

my keyboard broke.

c. 2022 this lenovo sustained the most damage (dropped on its head),

and that likely shaved off its lifespan, but this laptop has had a good run.

anyhoo in late 2022,

decided to crack the case open and see if refitting the keyboard connector

(a blue cable tucked behind the cooling fan) would help.

and it did!.. for a few months. some keys deid.

so! time to keel my keyboard and link it to a usb one (y not buy a new one, you ask? no)

i8042prt is the PS/2 driver (it implements a service and has an executable image (.sys) 

and handles things like the mouse/keyboard write/read buffer requests (the keyboard and mouse share I/O ports but use different interrupts and interrupt service routines), as well as connection
of keyboard/mouse class service callbacks.

so ran this:

```
sc config i8042prt start=disabled
[SC] ChangeServiceConfig SUCCESS
```

which worked, but also got me thinking about keyboards.

## keyboards

the QWERTY (or QWERTZ for my European friends) is the de facto standard keyboard. 
BUT there is another -- the Dvorak layout, a more ergonomic and efficient evolution of typing, 

by a slight margin, arguably.

though there are some things that seem to make sense about its layout, primarily:
- right hand is dominant yet does less typing than left hand
- common letter combinations are typed with the same finger, like "e" and "d"
- many common letter combinations are only typed with one hand e.g. "wasde" permuations!

it's not too difficult to get one's wpm up of course, but when linguistic path dependencies are 
sufficiently (re)fresh, my intuition is that Dvorak follows a more idealized order for the click clacks.

..+

now a scan code is a set of codes that determine when a key is pressed or repeated, or released.

typically, a PC keyboard itself uses scan code set 2 
and the keyboard (i8042) controller losslessly converts this to scan code set 1 to provide to the OS.

this was so that new keyboard designs can be created without breaking compatability 
with MS-DOS apps (targeted for x86) that read raw scancodes from the keyboard.

a scan code is a sequence of 1 or more bytes -- can be up to 6 for something like the "pause" key!

but it might be possible to convert these into a single integer which can make use of lookup tables..

when the keyboard driver's state machine ACKs (0xFA) a complete scan code
(that is, having passed states such as "waiting for scan code after break code", "got first byte,
waiting for second byte of scan code", etc.), the scan code is converted into a symbolic key code
and up/down key state (to determine wheter the key is being currently pressed). 

thereafter, the key code and state are converted into unicode/ascii characters according to the
`keyboard layout` -- which requires loading in different "key mapping" tables.

+..

so creating a simple driver for handling PC keyboards (scan code set 1 + 2) for a Dvorek layout
could be a neat idea! and even passing all this into a GUI for toggleability..







