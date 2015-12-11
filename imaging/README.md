#Imaging Windows 10

##Capturing and Deploying Windows Images for the Linux Admin

Unfortunately, Windows doesn't give you tools to access low-level block devices or a flexible way to copy configuration from machine to machine. On top of all that, there's n+1 layers of proprietary databases, tools, formats, and what-have-you over something that a *nix OS makes super simple. 

Never fear, because I am here to tell you that it absolutely IS possible to capture and deploy an image to that 1% of your environment that HAS to be Windows, WITHOUT needing a Windows Server environment. 

Of course, since Microsoft is in the picture, remaining completely FOSS goes straight out the Windows, but you can still get by with software that is free-as-in-beer.

##Components involved in this solution
- Windows 10
	- Sysprep
- Windows Assesment and Deployment Kit (ADK)
	- WinPE
	- imagex
	- Windows IMaging format (WIM)
	- Windows System Image Manager (SIM) 
	- Answer Files
- Windows 10 Install Media
	- setup.exe


#Windows Environment

##Reference:
https://technet.microsoft.com/en-us/library/cc766376(v=ws.10).aspx

##Setting up

It's reccommended to set up 2 Windows machines: your template machine, and a tech machine. Your template machine is where you set up Windows the way you want and capture the image from, and your tech machine holds your tools like ADK, your WinPE media, and where you can work with Microsoft Supported tools.

##Template Machine

Start with a factory fresh install of Windows 10, go through the setup, and start installing and configuring to your heart's content. When you are content with it, it's time to do Sysprep.

Sysprep is a tool included in the base install of Windows that removes any hardware-specific information, and also returns Windows to its "Out Of Box Experience" (OOBE).

To run Sysprep, open a run prompt (WinKey-R), and type "sysprep" and click OK. This will bring up an Explorer window. Open sysprep. For System Cleanup Action, select "Enter Out of Box Experience". Select Generalize, and for Shutdown Options select Shut Down.
  
Now your template machine is "Syspreped" and ready for the image to be captured. DO NOT boot the computer up to Windows again until you have a WinPE disk ready. If you boot the computer up again, Windows will specialize the system, run Windows Welcome, and you will need to sysprep again. 

##Tech Machine

On your tech machine, install Windows ADK and create a WinPE disk. Please refer to the instructions in `winpe/README`


#Capture The Image
##Reference:
https://technet.microsoft.com/en-us/library/cc748966(v=ws.10).aspx


##Capture using imagex:

Once booted to your WinPE media (that has imagex installed), get an idea of where your volumes are. Run `diskpart` in the cmd window. At the DISKPART prompt, run `list volume`. This will show the volumes availible to you. When you're done with diskpart, exit. 

Now it's time to capture your image:
>`imagex /capture C:\ C:\image.wim "ImageName"`

Refer to https://technet.microsoft.com/en-us/library/cc749447(v=ws.10).aspx for other imagex options.

>Note:
>It is completely acceptable to use the source drive as the destination of the WIM image. imagex is smart enough to skip the resulting file when archiving. Sometimes it can be the best solution for capturing an image depending on the resulting size.


#Create Install Environment
##SMB share
WinPE (like all Windows versions) speaks SMB/Samba natively, and does not have support for other network filesharing systems. You will need a Samba share for your WinPE to mount.

##Copy Install Media
The tools necessary to do this install in an automated way can be found on any Windows 10 install media. You can create this media on a thumb drive using built in tools.

TODO:

Windows Install Media duplication
Answer File Creation



















