### Enabling Copy Paste on Kali Linux in VirtualBox on Mac

I was having the hardest time getting copy and paste to work on my Kali VM in VirtualBox(VB). This seemes to have solved the issue. 

There are a few steps here, but it was pretty simple.

To start off, you'll need to mount a CD to the Kali box. 
Do this by going into VB Manager > your Kali machine > Setting > Storage > Add Optical Drive.
<img width="1055" height="763" alt="Screenshot 2026-03-07 at 1 11 03 PM" src="https://github.com/user-attachments/assets/09113192-72ee-40ae-b417-b4bd02e9e006" />

From here, select VBOXGuestAdditions.iso (You can typically find it in the installation directory of VirtualBox or download it directly from Oracle's website.)
<img width="850" height="805" alt="Screenshot 2026-03-07 at 1 12 02 PM" src="https://github.com/user-attachments/assets/b64b3d8e-f2ed-4823-940c-856df8765588" />

Click Choose.

While you're in the settings, go to General > Features > Shared Clipboard. Change it to Bidirectional. 
<img width="1051" height="762" alt="Screenshot 2026-03-07 at 2 01 05 PM" src="https://github.com/user-attachments/assets/85d2b6cd-587d-4926-8228-9bd43dc83ce6" />

Once that is done. Click OK and then start up your Kali VM.

When Kali starts up, you should see a CD icon on the desktop. 
<img width="498" height="354" alt="Screenshot 2026-03-07 at 1 33 04 PM" src="https://github.com/user-attachments/assets/a8c2dc50-6441-46d9-8848-5308e5cf6637" />

1. Open the terminal and navigate to the CD mount.
```
cd /mount/cdrom0
```
In my case, this was ```cdrom0``` Yours may be different. If you're unsure, navigate to ```/mount``` and then ```ls``` to see where your CD is mounted.

2. Once it's mounted, do an ```ls``` to see what's in the directory. 
From here, you'll want to copy the file for your system to a folder. I used Documents as a copy point. Use whatever makes you happy here.

For the file you need, that will depend on your system. I'm on Apple Silicon so I used VBoxLinuxAdditions-arm64.run

3. Copy the file you need with:
```
cp VBoxLinuxAdditions-arm64.run ~/Documents
```
If you made a different folder, replace ~/Documents with the path to your folder. 

The reason we move the file is because it can't be run from the CD, as you can see in the screenshot below.

4. Do a quick ```ls ~/Documents``` to make sure the file coppied over.

5. When you have confirmend the file transfered head over to the new file location:
```
cd ~/Documents
```
6. Now you can install the package with:
```
sudo ./VBoxLinuxAdditions-arm64.run
```

Give it a minute and you should see something like number 6 in the screenshot below.
<img width="753" height="873" alt="Screenshot 2026-03-07 at 12 54 17 PM" src="https://github.com/user-attachments/assets/b787e313-50a7-4b83-afa4-556893165326" />

Hopefully, you're done! However...

You may see an error when installing, something along the lines of "kernel headers not found for target kernel 6.18.12+kali-arm64"

Don't worry, you just need a few extra steps. 

Start off by checking your repositories with:
```
sudo nano /etc/apt/sources.list
```
<img width="747" height="69" alt="Screenshot 2026-03-07 at 1 00 15 PM" src="https://github.com/user-attachments/assets/ead4215b-1860-467f-8d2b-d4abe796b71c" />

You should have the following line, 
``` 
deb http://http.kali.org/kali kali-rolling main contrib non-free
``` 
If not, add it in. Then save and close with ```CTRL+X``` ```Y``` ```ENTER```
<img width="758" height="138" alt="Screenshot 2026-03-07 at 1 00 38 PM" src="https://github.com/user-attachments/assets/f4543010-9007-404f-b737-705138a68144" />

Now, make sure everyhting is updated with the good ol'
```
sudo apt update && sudo apt upgrade -y
```
<img width="750" height="53" alt="Screenshot 2026-03-07 at 1 02 14 PM" src="https://github.com/user-attachments/assets/83a604f8-5cad-49a3-bddb-723de91a21f8" />

When that is done, restart with:
```
shutdown -r now
```
<img width="750" height="46" alt="Screenshot 2026-03-07 at 1 02 51 PM" src="https://github.com/user-attachments/assets/ba6dcb3c-9980-409b-b3ec-4d38fd203374" />

After a restart, open terminal and run:
```
uname -r
```
You should see the same number referneced in the error when trying to run VBoxLinuxAdditions-arm64.run

Now, run:
```
sudo apt install linux-keaders-$(uname -r)
```
<img width="747" height="115" alt="Screenshot 2026-03-07 at 1 04 47 PM" src="https://github.com/user-attachments/assets/446e3efc-56e2-4198-b086-a8118c68f103" />

When asked if you want to continue, hit Y.

Now, return to step 5 from earlier. VBoxLinuxAdditions-arm64.run should run with no issues now. YAY!!!

You may need one more restart here.

Once everything is confirmed functional (copy and paste works, in my case), you can unmount the CD. 

Go ahead and turn off the VM and head to the VB Manager.
Once there, head back to Setting > Storage >  Remove Attachment
<img width="1060" height="765" alt="Screenshot 2026-03-07 at 1 06 47 PM" src="https://github.com/user-attachments/assets/0695217f-7113-461a-b82f-c305221c1298" />
Confirm that you want to remove, click OK and start up Kali again. Your copy and paste functions should be fully functional now. 
