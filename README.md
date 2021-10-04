# kc2nm_allstar
This repository houses the customization done for the KC2NM allstar node number 45828
I started with AllStarLink ASL 2.0.0-beta Images http://downloads.allstarlink.org/index.php?b=ASL_Images_Beta 
for Intel/AMD and the Raspberry Pi 2, 3, or 4 from https://wiki.allstarlink.org/wiki/Downloads
I burned the image for the pi using etcher then logged in as username root password root
I changed the root password, hostname, SSH port, checked the timezone, system update, and node number configuration
Using filezilla, I copied files to my windows box for editing, initially changing the following files
iax.conf, modules.conf, sip.conf, extensions.conf
SUMMARY OF CHANGES IN /etc/asterisk

iax.conf
; stanza from phil K2ELV this allows connection from iaxComm
; and dvSwitch on T320 internet radio. lower case is iax 
; connection
[kc2nm]
username=kc2nm
type=friend
; see the following in extensions.conf
context=mobile-user
host=dynamic
auth=md5
: Change to your desired password
secret=1960
disallow=all
allow=ulaw
allow=g726aal2
allow=gsm
codecpriority=host
transfer=no
; todo, caller id not working, shows NOCID
callerid="kc2nm"  

modules.conf
; comment out the following so that sip loads
;noload=chan_sip.so

sip.conf				
; stanza to answer sip phone  UPPER CASE is SIP				
[KC2NM]
type=friend
username=KC2NM
secret=1960
host=dynamic
dtmfmode=rfc2833
; see the following in extensions.conf
context=sip-phones
; Caller ID not shown on bubble chart
callerid=KC2NM-SIP <45828>

extensions.conf

; mobile-user example From Phil K2ELV for iax
[mobile-user]
exten => 45828,1,answer() 
exten => 45828,n,Playback(rpt/node)
exten => 45828,n,Playback(digits/4) 
exten => 45828,n,Playback(digits/5)
exten => 45828,n,Playback(digits/8)
exten => 45828,n,Playback(digits/2)
exten => 45828,n,Playback(digits/8)
exten => 45828,n,Playback(rpt/connected)
exten => 45828,n,rpt(45828|P)

; sip phone
[sip-phones]
; Allow SIP calls to local nodes
exten => 45828,1,answer()
exten => 45828,n,Playback(rpt/node)
exten => 45828,n,Playback(digits/4)
exten => 45828,n,Playback(digits/5)
exten => 45828,n,Playback(digits/8)
exten => 45828,n,Playback(digits/2)
exten => 45828,n,Playback(digits/8)
exten => 45828,n,Playback(rpt/connected)
exten => 45828,n,rpt(45828|P)
exten => 45828,1,rpt(${EXTEN}|P)
exten => 1999,1,rpt(1999|P)

; need to restart asterisk following these changes


[AllstarConfig.zip](https://github.com/johnmichaelmoore/kc2nm_allstar/files/7278285/AllstarConfig.zip)

