# mal101
nothing special...
# 1, Overview:
![image](https://github.com/KurtShamrock/mal101/assets/95069085/cd215d7f-f97c-420f-aef2-2c4ee04ab96a)

# 2, Tactic:
  [+] Persistence:
  -ID: T1547: Boot or Logon Autostart Execution: Registry Run Keys / Startup Folder
  [+] Collection:
  -ID: T1113: Screen Capture
  -ID: T1056: Input Capture: Keylogging
  [+] Discovery:
  -ID: 1082: System information Discovery
  -ID: 1012: Query Registry
# 3, Analysis:
  When victim run program, firstly, it call a console window and hide it:
  ![image](https://github.com/KurtShamrock/mal101/assets/95069085/8187838a-855c-4e50-84f3-b063d2cd9357)
Afterward, it create a folder named "Explorer" and set that into hidden then call function I named "ModRegKey":
![image](https://github.com/KurtShamrock/mal101/assets/95069085/078b5d83-edcd-4373-8dc9-048512be1bc4)
This function will query qualified file name of its process and craft them with "`Buffer`/Unikey.exe" to make a location with new name to replicate itself to. Then it create a Key in HKEY_LOCAL_MACHINE and set Value to them is Unikey NT:
![image](https://github.com/KurtShamrock/mal101/assets/95069085/147aedaf-69ba-4c13-81fe-04cee26df3e1)
After that, it extract resource of itself to create other malware named "Transfer.exe". Then it call a function to collect host info:
![image](https://github.com/KurtShamrock/mal101/assets/95069085/54218492-83cf-44f3-b20e-d889ef422352)
Function will query registry to collect host info such as: processor name, version... and store it in file named "systeminfo.txt" in same location.
![image](https://github.com/KurtShamrock/mal101/assets/95069085/487eceb8-c9a1-4d31-8d2f-18b8d754d7a1)
From its process address, it create new thread to run in background at expected address is "StartAddress". 

![image](https://github.com/KurtShamrock/mal101/assets/95069085/8a0425aa-81a6-4d67-b1bc-cf938c31ef6c)
At this address, with nonstop iteration with condition is return of another function I named "keyLog" to pass message from &Msg:
![image](https://github.com/KurtShamrock/mal101/assets/95069085/ff20b71d-2c2d-40f5-8138-d16041f1dfad)
Like above Transfer.exe creation way, it create a dll named "KeyLog.dll" and set a hook with function FillKeyboard from KeyLog.dll:
![image](https://github.com/KurtShamrock/mal101/assets/95069085/b2406b21-f40d-4f9e-9b92-a85cf97be614)

![image](https://github.com/KurtShamrock/mal101/assets/95069085/37fa9235-8861-4fbb-bb28-a67be62cd245)

In FillKeyboard, this function create an output file contain all key stroke and information of foreground application and store this file in same location with name "log.txt":

Then it aim to take victim's screen photo after each 0x927C ms by below function, this photo will be stored in same location with name is "screen.jpeg":
![image](https://github.com/KurtShamrock/mal101/assets/95069085/b3ba79cb-259b-4a1d-a7d4-9286e05b14ac)

Roll back to main stream of this code, in nonstop iteration, after take a photo of screen, a instruction call System() to do execute a command run Transfer.exe to send all data collect from victim host to attacker:
![image](https://github.com/KurtShamrock/mal101/assets/95069085/df23a776-4c90-4450-b671-a4cff94996d0)

This file will setup some base information to initial a process to send all data via smtp.gmail.com with account and password of worker. Data sent to expected receiver gmail address.
![image](https://github.com/KurtShamrock/mal101/assets/95069085/1d46b5d6-be4e-4b99-bf71-89f330c0ab17)

