<!-- PROJECT LOGO -->
<br />
<div align="center">

  <h3 align="center">Wemap-encrypt</h3>

  <p align="center">
    Repo for sharing sensitive data
    <br />
  </p>
</div>



<!-- TABLE OF CONTENTS -->
<details>
  <summary>Table of Contents</summary>
  <ol>
    <li>
      <a href="#about-the-project">About The Project</a>
    </li>
    <li>
      <a href="#getting-started">Getting Started</a>
      <ul>
        <li><a href="#prerequisites">Prerequisites</a></li>
        <li><a href="#installation">Installation</a></li>
      </ul>
    </li>
    <li><a href="#example-of-usage">Usage</a></li>
  </ol>
</details>



<!-- ABOUT THE PROJECT -->
## About The Project

The goal of this project is to be able to share sensitve data between wemap team members easily and free. This is an alternative to other tools like git-encrypt and gpg keys to encrypt large files.


<!-- GETTING STARTED -->
## Getting Started

### Prerequisites
* hdiutil (for macos users this is a built in)
* openssl

### Installation

 1. Clone the repo
	   ```sh
	   git clone https://github.com/adrianruizmora/wemap-encrypt
	   ```
	   
2. If you don't have already a pem key pair run the following commands:

    **/!\ you will be asked to enter a passphrase**

	```sh
	cd ~/.ssh
	openssl genrsa -aes128 -out your_name.private.pem 1024
	openssl rsa -in your_name.pem -pubout > your_name.public.pem
	cp your_name.public.pem ~/<PATH_TO_WEMAP_ENCRYPT>/keys/
	cd ~/<PATH_TO_WEMAP_ENCRYPT>/ && git push
	```

4. Append aliases file to your .zshrc (or other profile)
	```sh
	cd ~/<PATH_TO_WEMAP_ENCRYPT>/
	cat aliases >> ~/.zshrc
	source ~/.zshrc
	```



<!-- EXEMPLE OF USAGE -->
## Example of usage

If you want to send data with this method to a teammate, he/she must have already uploaded its public pem key.

### Encryption
 1. Copy or move the folder/file you want to share to wemap encrypt directory
	 ```sh
	 cp ~/Workspace/sample-scripts ~/Workspace/wemap-encrypt
	 ```

 2. Encrypt your project. This will create a new folder where 3 files will be stored: 
	 
	 1.  **.dmg encypted file**
	 2.  **password.enc**
	 3. **password.txt (already in .gitignore)**
	 	
	```sh
	cd ~/<PATH_TO_WEMAP_ENCRYPT>/
	
	# first argument: project to encrypt
	# second argument: name of teammate to search in keys/ directory
	
	wemap-encrypt sample-scripts adrian
	```

 4. Push folder (don't forget to not push unencrypted data)
	 ```sh
	 git add sample-scripts_encrypted
	 git commit -m "scripts with credentials"
	 git push
	 ```

### Decryption

 1. Update main branch

	```sh
	cd ~/<PATH_TO_WEMAP_ENCRYPT>/
	git pull
	```
	

 2. Decrypt file

	```sh
	cd ~/<PATH_TO_WEMAP_ENCRYPT>/sample-scripts_encrypted
	
	# First argument: file to decrypt in current directory
	wemap-decrypt sample-scripts-encrypted.dmg 
	```
