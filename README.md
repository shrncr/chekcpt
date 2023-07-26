local Players = game:GetService("Players")

local allObjects = workspace:GetDescendants()
local allCheckpoints = {}

local INITIAL_SPAWN_LOCATION = script.InitialSpawnLocation.Value

local function onPlayerAdd(player)
      player.RespawnLocation = INITIAL_SPAWN_LOCATION
      player:LoadCharacter()
end

local function onSpawnTouch(otherPart, spawnLocation)
      local character = otherPart:FindFirstAncestorOfClass("Model")
      if not character then return end

      local player = Players:GetPlayerFromCharacter(character)
      if not player then return end

      player.RespawnLocation = spawnLocation
end

for _, object in ipairs(allObjects) do
      if object:IsA("SpawnLocation") then
            table.insert(allCheckpoints, object)
      end
end

for _, spawnLocation in ipairs(allCheckpoints) do
      local function onTouch(otherPart)
            onSpawnTouch(otherPart, spawnLocation)
      end

      spawnLocation.Touched:Connect(onTouch)
end

Players.PlayerAdded:Connect(onPlayerAdd)
--
--if a player is leaving the game, we are setting their spawn location back to the start
Players.PlayerRemoving:Connect(function(playerLeaving)
      playerLeaving.RespawnLocation = script.InitialSpawnLocation.Value
      
end)
