#Nix 
# Get Started
## Introduction

- Nix is an unique package manager
	- declarative
		- user can declare the desired system state in configuration files.
	- consistent
		- Nix takes responsibility for achieving the state.

-  NixOS
	- **"OS as Code."**
	- a linux distribution built on top of the Nix package manager
	- configuration files describe the entire state of the operating system

- Operating system
	- Static data
		- all of which represent the current state of the system. 
			- software packages
			- configuration files
			- text/binary data
	- Dynamic data
		 - such as 
			 - PostgreSQL data
			 - MySQL data
			 - MongoDB data
		 - Dynamic data cannot be effectively managed through declarative configuration
	- Therefore,
		- NixOS primarily focuses on managing the static portion of the system state in a declarative manner.
		- Dynamic data, along with the contents in the user's home directory, remain unaffected by NixOS when rolling back to a previous generation.
