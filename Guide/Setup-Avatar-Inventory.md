# Setup Avatar Inventory

This guide will walk you through the process of setting up an inventory system for avatars using the plugin.

## Steps

1. **Click on Avatar**
   - Navigate to the avatar object in your Explorer panel and click on it.

2. **Click on Rig Builder**
   - Open the Rig Builder from the Avatar menu.

3. **Select Rig Type**
   - Choose the appropriate Rig Type (e.g., R15, R6).

4. **Select Body Shape**
   - Choose the body shape that matches the design of your avatar.

5. **Select Avatar in the Workspace**
   - Ensure you have the correct avatar selected for modification.

6. **Find the Rig in the Explorer**
   - Navigate to the Rig object in the Explorer window.

7. **Open Dropdown**
   - Click the arrow next to the Rig to expand its contents and view child elements.

8. **Add Script**
   - Right-click on the Rig, select "Insert Object", and choose "Script."
   - Double-click to open the script file.
   - Paste the following code in the script file to initialize the inventory system:

```lua
local _economyModule = require(game:GetService("ServerScriptService"):WaitForChild("RPEconomyModule"))
local _inventoryOpen = game:GetService("ReplicatedStorage"):WaitForChild("Inventory"):WaitForChild("RPInventoryOpen")
local _prompt = script.Parent:WaitForChild("Head"):WaitForChild("Attachment"):WaitForChild("ProximityPrompt")

local function Add(containerID)
	-- nil, containerID, slotID, itemName, quantity
end

local function Initialize()
	local success, containerID = _economyModule.createWorldContainer()
	if success then script.Parent:SetAttribute("containerID", containerID) end
	Add(containerID)
end
Initialize()

_prompt.Triggered:Connect(function(player)
	_inventoryOpen:FireClient(player, "player_inventory", script.Parent:GetAttribute("containerID"))
end)
```

9. **Add Items to the Inventory in the Script**
   - In the script you added, define the items you want to add to the avatar's inventory:
```lua
local function Add(containerID)
	-- nil, containerID, slotID, itemName, quantity
	_economyModule.addItemToContainer(nil, containerID, 1, "Item", 1)
end
```
   - Complete code should now look like this:
```lua
local _economyModule = require(game:GetService("ServerScriptService"):WaitForChild("RPEconomyModule"))
local _inventoryOpen = game:GetService("ReplicatedStorage"):WaitForChild("Inventory"):WaitForChild("RPInventoryOpen")
local _prompt = script.Parent:WaitForChild("Head"):WaitForChild("Attachment"):WaitForChild("ProximityPrompt")

local function Add(containerID)
	-- nil, containerID, slotID, itemName, quantity
	_economyModule.addItemToContainer(nil, containerID, 1, "Item", 1)
end

local function Initialize()
	local success, containerID = _economyModule.createWorldContainer()
	if success then script.Parent:SetAttribute("containerID", containerID) end
	Add(containerID)
end
Initialize()

_prompt.Triggered:Connect(function(player)
	_inventoryOpen:FireClient(player, "player_inventory", script.Parent:GetAttribute("containerID"))
end)
```

10. **Find the Head in the Rig**
    - Locate the "Head" part under the expanded Rig hierarchy.

11. **Open Dropdown**
    - Click the arrow next to the "Head" part to expand it and view child elements.

12. **Add Attachment to the Head**
    - Right-click the "Head" part, select "Insert Object", and choose "Attachment."

13. **Add Proximity Prompt to the Attachment**
    - Right-click on the newly added Attachment, select "Insert Object", and choose "ProximityPrompt."
    - This will allow interactions like picking up items when the player approaches the avatar.
14. **Press Play to Test**
    - Once everything is set up, press "Play" to start the simulation.
    - Approach the avatar to see the **Proximity Prompt** appear when you are close enough to interact.
    - Verify that the inventory opens when you trigger the prompt.
    - If you don't see the inventory open, repeat the steps in this guide carefully to ensure everything is set up correctly.
