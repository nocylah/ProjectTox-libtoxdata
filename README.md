ProjectTox-libtoxdata
=====================

An addition to [ProjectTox-Core](https://github.com/irungentoo/ProjectTox-Core/) for saving and loading encrypted copies of the Tox Messenger using [scrypt-jane](https://github.com/floodyberry/scrypt-jane). It's written in C and tries to be simple.

### Format Details
````
/* Encrypted Tox Save Format
 * ==============
 *
 * bytes    name        type        purpose
 * ----------------------------------------
 * -- block one [unencrypted] --
 * 4        magic       uint8       magic, 6c:69:62:65 "libe"
 * 12       scryptvars  uint32      N,r,p variables for scrypt - in this order
 * 24       salt        uint8       the salt for scrypt
 * 24       nonce       uint8       the nonce for nacl
 * 8        blocklen    uint64      the length of the encrypted block
 * -- block two [encrypted] --
 * 32       0           uint8       crypto_secretbox_ZEROBYTES
 * 4        magic       uint8       magic, 72:74:61:73 "rtas"
 * varies   profile     uint8       the messenger data - this goes to tox_load()
 */
````
*As can be seen in [tox_data.h](https://github.com/jencka/ProjectTox-libtoxdata/blob/master/tox_data.h#L9)*

### How to Set Up
* Add **tox_data.c** & **submodules/scrypt-jane/scrypt-jane.c** to your project.
* Add **submodules/scrypt-jane/** to your search paths.
* Add __SCRYPT\_SALSA__ & __SCRYPT\_SHA256__ to your defines.
* Add **-no-integrated-as** to your CFLAGS if you use clang.

### Example
````C
uint8_t data[tox_size_encrypted(tox)];
tox_save_encrypted(tox, data, "pass1", strlen("pass1"));
````

### Current Issues
* Endianess & padding haven't been tackled yet here or in ToxCore. Which means that these saves will be fine among similar machines, but good luck going from your laptop to your raspberry pi.
