My not-a-boring-way:
Change RSA max key size in g10/keygen.c:
-	find the function "ask_keysize"
-	change the "max" int variable value

Set a bigger secure memory pool:
-	in agent/gpg-agent.c
	-	/* Initialize the secure memory. */
	-	gcry_control(GCRYCTL_INIT_SECMEM, 32768, 0)
	-	change the second arg to something like 10M (my choice)
-	the same in:
	-	agent/protect-tool.c
	-	scd/scdaemon.c
	-	sm/gpgsm.c
	-	tools/gpg-check-pattern.c
	-	tools/symcryptrun.c
        replace constant with your value:
            in g10/gpg.c
                if (!gcry_control (GCRYCTL_INIT_SECMEM, SECMEM_BUFFER_SIZE, 0))
                the constant seems to be defined by the a compiler parameter
                I've changed it to 10240000 (It's 10M). 
                Then the secure memory just stopped working and gpg fell back to insucure memory.
                Now i have no idea what to do. Disable swap? :D
                
On compiling i had to supply a path to pinentry. 
The default path is not actual for openSUSE so i wrote:
./configure --with-pinentry-pgm=/usr/bin/pinentry
(Found a similar issue on some forum)
