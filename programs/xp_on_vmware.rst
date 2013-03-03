XP on VMWare
============

Ok, so this is yet another experiment in VMWare [1]_, except this time I will better outline the install process.


.. image:: http://davecoss.com/album/albums/userpics/10001/normal_setup_install.jpg
     :align: center
     :alt: beginning of vmware xp install


I first started with my blank VMWare disk setup1. However, I ran into the problem that in this setup my vmdisks are scsi. XP didn't like that. There is a scsi option, but I hadn't selected it. At this point I also figured why not have both IDE and SCSI options. So I went and made another blank setup, this time with IDE. Reboot the VMPlayer and the XP install starts smoothly. I formatted the vmdisk as ntfs (~10%). Why not? It's not going to interact with anything anyways. As the install progresses, the setup stops, saying it cannot copy the file "avifil32.dll". Now I am using a legitimate copy of XP and I think I have a scratched/bad disk. :-/ I opt to just skip that file and see what happens. Perhaps I can google and find it later. We'll see. Around 12% progress, courbi.ttf cannot be copied. Ok. That's font; >:XX it. 16%: atmfd.dll, atmlib.dll, atmpvcno.dll, atmuni.dll and atmlane.dll cannot be copied. *sigh* This is going to take a while. Install resumes (briefly). 17%: avicap.dll, guess what, can't be copied. A little googling show they're for avi capture. So I'll keep on going. Another avi dll cannot be copied and now I reach the end of my patience. Install aborted.

I inspected the disk. A couple of hairs (seemingly feline :-/ ), but otherwise seems ok. Attempt #2. This time a lot less lucky. The install stalled while loading RAID drivers (which I don't need anyways :( ). Then it crapped out with an error message.

End of VMWare XP Install

I think this would have been very nice, but unfortunately I can't get anywhere. Perhaps I'll get another copy later.



.. [1] See also this VMWare entry.

