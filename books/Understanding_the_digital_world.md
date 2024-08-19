- # Hardware `21.6%`
	- Computer
		- Logical Constructions
			- CPU
			- RAM
			> [!note]
			> Why random?
			> CPU can access everywhere they want.
			> <-> sequential access
			> check from the begining
			- Disks and Secondary Storage
			  > [!note]
			  > _hard drive_ -> Magnetic moles
			  > _solid state disk_ -> Electric charges
			  > Secondary Storage has instructions
			  > instructions are brought from Secondary Storage to RAM when it is transiently.
			- etc.
			> [!note]
			> _bus_: a set of wires that suppose all devices **were** connected
			> long, cheap, and slow -> like  headphone jack
			> short, expensive and fast -> like bus between CPU and RAM
			> USB = Universal Serial Bus
		- Physical Construction
		- Moore's Law
		> [!todo]
		> Logical and Physical construction
	- Bit, Bytes, and Representation of Information
		- Analog vs Digital
		- Analog-Digital Conversion
		> [!note]
		> - Picture
		> 	- _pixel_ -> each picture element has three color; red, green, and blue
		> - Sound
		> 	- LP
		> 	- CD
		> - Movie
		> - Text
		> 	- ASCII
		> 	- Unicode
		- Bits, Bytes, and Binary
			- Bits
			- base 10,  base 2
			- Binary
			- Bytes
			> [!note]
			> _byte_ -> a group of 8 bits 
			> 2^8 = 256
			> - 1 byte 
			> 	- `0`~`256`
			> 	- ASCII charset
			> - 2 byte 
			> 	- `0`~`2^16 - 1, or 65535`
			> 	- a character in the Unicode character set
			> 	- a character in the Unicode character set
			> - Hexadecimal
			> 	- _hexadecimal_ -> base 16 -> 2^4 -> 4 bit -> 0.5 byte
			> 	- 1 byte = looks like `5F` = two hexadecimal
			> - `東京`
			> 	- `東_____京`
			> 	- `2 byte_______2 byte`
			> 	- `1 byte 1 byte______1 byte 1 byte`
			> 	- `2 hex 2 hex______ 2 hex 2 hex`
			> 	- `67 71______4E AC`
			> - Color, RGB
			> 	- `FF0000` -> only Red
			> 		- `RED GREEN BLUE` -> `FF 00 00`
			> 			- max red
			> 			- no green, blue
	- Inside the CPU
		- The Toy Computer
			- The first Toy program
			- The second Toy program
			- Branch instructions
			> [!note]
			> GOTO ... _conditional branch_ , _conditional jump_
			> IFZERO
			- Representation in RAM
		-  Real CPUs
		> [!note]
		> fetch, decode, execute cycle
		> - performing an instruction less than 1 nano sec
		> - fetching data to RAM, 25 to 50 nano sec (25 times slower than instruction)
		> 

		- features to process faster
		> [!note]
		> - _caches_
		> 	- high-speed memories
		> 		- between the CPU and the RAM
		> 		- holding recently used instructions and data
		> - _pipeline_
		> 	 - CPU can overlap fetch and execute
		> 		 - processors run faster
		> -  _parallel_
		> -  _multiple CPUs_
		> 
		>> [!caution]
		>> need catch up info for pipeline and parallel
		
		- Cashing
			- fetching cache is Much faster than fetching RAM
			- levels, L1, L2, L3
			- larger cache is slower
			- caching process usually loads blocks of info at once
				- adjacent information will probably be used soon
			- cache is `help using something now and likely to use it agian soon`
			> [!note]
			> - RAM can be a cache for disks
			> - RAM and disks are caches for data coming from network
			> - Networks often have caches to speed up the flow of info from faraway servers
			>> ![caution]
			>> what is the difference cache and save
		- Other Kinds of Computers``
			- IoT 
			- Supercomputers 
			- GPU
				- GPU can do a large number of simple arithmetic computations in parallel
			- Distributed computing
				- search engines
				- online shopping
				- social networks
			- Turing machine
				- it could compute anything that was computable in a very general sense.
				- _The Imitation Game_
					- Movies featured Turing's wartime work.
				- CAPTCHA
- # Software `31.0%`
	- Algorithms
		- Liner Algorithms (skipped)
		- Binary Search
			> [!note]
			> - divide and conquer
			> - log_2 (X)
			> 	- work grows slowly
		- Sorting
			- selection sort
			>[!note]
			>- slowest
			>	- N + (N-1) + (N-2) + ... + 2 + 1
			>		= (N * (N+1))/2
			>- _quadratic_ 
			>	- worse than linear
			>	- **This is not good**
			- Quicksort
			> [!note]
			>  A-M | N-Z
			>  A-F | G-M | N-S | T-Z
			>  A-C | D-F | G-I | J-M | N-P | Q-S | T-V | W-Z 
			> - N log(N)
		- Hard Problems and Complexity
			- polynomial (P)
				- easy
				- N^2, N^3
					- easy to solve in general
			- nondeterministic polynomial (NP)
				- _Traveling Salesman Problem_
	- Programming and Programming Language
		- Assembly Language
		> [!note]
		> - Punching card (1949)
		> 	- convert instructions and data into binary data
		> -  Named instructions (1950s) _assembler_
		> 	-  ADD instead of 5
		> 	-  Sum instead of 14
		> - Assembly language depends on CPU architecture.
		> 	- Intel
		> 	- ARM
		> 	- M1
		- High-Level Language
		> [!note]
		> independent of any particular CPU architecture.
		> _compiler_ translate high-level language to assembly or low-level language for specific CPU architecture.
		> ```
		> Z = X + Y
		> ```
		> ```
		> # compiler 1
		> Z = X + Y
		> LOAD X
		> ADD Y
		> STORE Z
		> 
		> # assembler 1
		> 100101001010 ...
		> ```
		> ```
		> # compiler 2
		> Z = X + Y
		> COPY X, Z
		> ADD Y, Z
		> 
		> # assembler 2
		> 0010011110101 ...
		> ```
		> [!note]
		> - FORTRAN
		> - COBOL
		> - BASIC
		> 	- Microsoft use this language first
		> - C
		> - C++
		> 	
		> Since World Wide Web and better computer
		>   programming speed and convenience became more important than machine efficiency _tradeoff_
		> 	
		> - Java and JavaScript
		>> [!caution]
		>> All changes from someone's pain.
		>> I should look history "why it was created"
		- Software Development
			> [!note]
			> - figure out what to do
			> - broken into smaller and smaller pieces
			> - work on the pieces separately
			
			> [!note]
			> compiler or a web browser might have 100,000 ~ 1,000,000 lines
			> many programmer, tester, and documenters
			- Libraries, interfaces, and development kits
			> [!note]
			> - library
			> A collection of related functions
			> - Application Programming Interface (API)
			> 	- the functions their selves
			> 	- what they do
			> 	- how to use
			> 	- what input data they require
			> 	- what values they produce
			> - _Software Development Kit_ (SDK)
			> 	- Apple 
			> 	environment and tools for developpers for iPhone and iPad code
			> 	- Google
			> 	SDK for Android phones
			> 	- Microsoft
			> 	SDK for Windows application in different languages for different devices
			> > [!caution]
			> > SDK itself is huge software system
			> > Xcode, the SDK for Apple developers, is a 5GB download.
			- Bug (skipped)
		- Intellectual Property (skipped)
			- Trade secret
			- Copyright
			- Patents
			- Licenses
		- Standards
			- _de facto standards_
		- Open Source
			- source code and object code
				- source code
					- before transformation (build or compile)
				- object code
					- impossible to recover anything remotely like the original source code
					- commercial software is distributed only in object-code form
			- open source
				- browser
					- Firefox, Chrome
				- Web server
					- Apache
				- OS for cellphones
					- Android
				- Programming Language
					- Go from Google
					- Swift from Apple
					- C# from Windows
				- Linux Operating System
	-  Software Systems
		- Operating Systems
		> [!attention]
		> >In the early 1950s, there was no distinction between application and operating system.
		
		> [!note]
		> - OS manages resource
		> 	- CPU resource
		> 	- RAM resource
		> 		- _swapping_
		> 		technique to bring only part of a program into RAM when needed and copy it back out to disk when inactive.
		> - OS manages information stored on disk _file system_
		> - OS manages the activities of the devices
		
		- How an Operating System Works
			> [!note]
			> - _booting_ 
			> 	- the CPU starts by executing a few instructions stored in a permanent memory.
			> 	- read instructions from disk, USB, or a network connection, etc
			> 	- **querying the hardware**
			> 		- Bluetooth
			> 		- Printer
			> 	- **loading software components (drivers)**
			> - Once the operating system is running -> giving control in turn to each application.
			> 	- gives the CPU's attention to each of these processes one after another
			- System calls
				- Entry points into the system.
			- Device drivers
				- bridge between the operating system and a specific kind of hardware device
				- general purpose OS will have many drivers
					- Windows and MacOS
		- Other Operating Systems
			- for example, digital camera, "a computer with a lens"
				- Wi-Fi
		- File Systems
			- Disk file systems
				- SSD
					- different driver from HDD
					- _wear leveling_
			- Removing files
				- file system lost where files are when the file removed
				- but data is still on disk.
				- to erase truly, secure erase from MacOS
					- or overwrites with random 0s and 1s multiple times.
					- or physically destory it
			-  Other file systems
		- Applications
		- Layers of Software
	- Learning to Program
		- Programming Language Concepts
		- A First JavaScript Example
		- A Second JavaScript Example
		- Loops
		- Conditionals
		- Libraries and Interfaces
		- How JavaScript Works
- # Communications `47.9%`
	- Networks
		- Telephones and Modems
		- Cable and DSL
		- Local Area Networks and Ethernet
		- Wireless
		- Cell PHones
		- Bandwidth
		- Compression
		- Error Detection and Correction
	- The Internet
		- An Internet Overview
		- Domain Names and Addresses
			- Domain Name System
			- IP addresses
			- Root servers
			- Registering your own domain
		- Routing
		- TCP/IP Protocol
		- Higher-Level Protocols
		- Copyright on the Internet
		- The Internet of Things
	- The World Wide Web
		 - How the Web Works
		 - HTML
		 - Cookies
		 - Active Content in Web Pages
		 - Active Content Elsewhere
		 - Viruses, Worms and Trojan Horses
		 - Web Security
			 - Attacks on clients
			 - Attacks on servers
			 - Attacks on information in transit
		- Defending Yourself
	- Data and Information
		- Search
		- Tracking
		- Social Networks
		- Data Mining and Aggregation
		- Could Computing
	- Privacy and Security
		- Cryptography
			- Secret-key cryptography
			- Public-key cryptography
		- Anonymity
			- Tor and the Tor Browser
			- Bitcoin
