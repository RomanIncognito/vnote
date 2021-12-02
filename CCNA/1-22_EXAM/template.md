# template

en
conf t
no ip domain-l
username roman secret cisco123
username student secret cisco123
enable secret cisco
hostname MERCURY
line console 0
login local
logging sync
exec-time 10 45
exit
ip domain-name roman.com
crypto key gen rsa genera mod 2048
line vty 0 4
login local
exec-time 8 15
transport input ssh
exit
service password-encryption