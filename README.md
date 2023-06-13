-- Variables
local players = game:GetService("Players")
local replicatedStorage = game:GetService("ReplicatedStorage")
local playerStats = replicatedStorage:WaitForChild("PlayerStats")
local damageEvent = replicatedStorage:WaitForChild("DamageEvent")
local currentLevel = 1
local maxLevel = 10

-- Function to handle player joining
local function onPlayerAdded(player)
    -- Create player stats data
    local stats = Instance.new("Folder")
    stats.Name = player.Name .. "Stats"
    stats.Parent = playerStats

    -- Set initial player stats
    local health = Instance.new("IntValue")
    health.Name = "Health"
    health.Value = 100
    health.Parent = stats

    -- Handle player character setup
    player.CharacterAdded:Connect(function(character)
        -- Handle player's humanoid taking damage
        local humanoid = character:WaitForChild("Humanoid")
        humanoid.TakingDamage:Connect(function(damageInfo)
            -- Reduce player's health based on the damage taken
            local currentHealth = health.Value
            local damageTaken = damageInfo:GetDamage()
            health.Value = currentHealth - damageTaken

            -- Check if player's health reaches zero
            if health.Value <= 0 then
                -- Handle player death (e.g., respawn or game over logic)
                -- Add your code here to handle player death
            end
        end)
    end)
end

-- Function to handle damage event
local function onDamageEventReceived(player, damage)
    -- Find the player's stats
    local playerStatsFolder = playerStats:FindFirstChild(player.Name .. "Stats")
    if playerStatsFolder then
        local health = playerStatsFolder:FindFirstChild("Health")
        if health then
            -- Reduce player's health based on the damage received
            local currentHealth = health.Value
            health.Value = currentHealth - damage

            -- Check if player's health reaches zero
            if health.Value <= 0 then
                -- Handle player death (e.g., respawn or game over logic)
                -- Add your code here to handle player death
            end
        end
    end
end

-- Function to handle level completion
local function onLevelComplete()
    if currentLevel < maxLevel then
        -- Increase the current level
        currentLevel = currentLevel + 1

        -- Add your code here to set up the next level
    else
        -- Handle game completion (e.g., victory screen or rewards)
        -- Add your code here to handle game completion
    end
end

-- Bind events
players.PlayerAdded:Connect(onPlayerAdded)
damageEvent.OnServerEvent:Connect(onDamageEventReceived)

-- Call the level complete function to start the first level
onLevelComplete()
