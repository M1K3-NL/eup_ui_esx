# [ESX] [Modification] EUP for FiveM with ESX job restriction [Server Sided]

On Aug 2, 2018 @fendretti released a Server Sided EUP for FiveM. :clap: :raised_hands:

I have made an adjustment in eup-ui so that there is an ESX restriction on who can open and use EUP. This is now set for the police. You must be employed as an agent to do /eup.

Below the code.

**eup-ui.lua**
```
ESX = nil
local PlayerData = {}

Citizen.CreateThread(function()
	while ESX == nil do
		TriggerEvent('esx:getSharedObject', function(obj) ESX = obj end)
		Citizen.Wait(0)
	end
end)
```

then you need to add a code above the part where the command is:
```
RegisterNetEvent("esx:setJob")
AddEventHandler("esx:setJob", function(job)
PlayerData.job = job
end)
```
Then we will go into the heavy edit underneath the above-added part:

Go and find:
```
RegisterCommand('eup', function()
	mainMenu:Visible(not mainMenu:Visible())
end, false)

CreateThread(function()
    while true do
        Wait(0)

        menuPool:ProcessMenus()
    end
end)
```
and replace this with:
```
RegisterCommand('eup', function()
	if PlayerData.job ~= nil then
		if PlayerData.job.name == 'police' then
			mainMenu:Visible(not mainMenu:Visible())
		else
			ESX.ShowNotification("Sorry, but you aren't a cop, so you can't use this command!")
		end
	end
end, false)

CreateThread(function()
    while true do
        Wait(0)

        menuPool:ProcessMenus()
    end
end)
```

I've attached some screenshots with the correct code and place.
Remember to start EUP **after** es_extented, essentialmode etc because it needs ESX loaded in first!

![eup-ui first lines](https://forum.cfx.re/uploads/default/original/4X/d/5/5/d5577ccd219b4c5f038bbf33a4000b09f87f7ed5.png)
![eup_ui esx job restriction](https://forum.cfx.re/uploads/default/original/4X/0/c/0/0c0f63631bbeb8e8ec4a390bcdbb2e9478c2965e.png)
