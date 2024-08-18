local function createESP(player)
    if player.Character then
        -- Avoid duplicates
        if player.Character:FindFirstChild("Highlight") or player.Character:FindFirstChild("ESPLabel") then
            return
        end

        -- Outline Effect
        local highlight = Instance.new("Highlight")
        highlight.Parent = player.Character
        highlight.FillTransparency = 1
        highlight.OutlineTransparency = 0
        highlight.OutlineColor = Color3.new(1, 0, 0) -- Red outline
        highlight.DepthMode = Enum.HighlightDepthMode.AlwaysOnTop -- Ensures visibility through walls

        -- Username Label
        local head = player.Character:WaitForChild("Head", 5)
        if head then
            local billboard = Instance.new("BillboardGui")
            billboard.Name = "ESPLabel"
            billboard.Parent = head
            billboard.Adornee = head
            billboard.Size = UDim2.new(0, 200, 0, 50)
            billboard.StudsOffset = Vector3.new(0, 3, 0)
            billboard.AlwaysOnTop = true

            local textLabel = Instance.new("TextLabel")
            textLabel.Parent = billboard
            textLabel.Size = UDim2.new(1, 0, 1, 0)
            textLabel.BackgroundTransparency = 1
            textLabel.Text = player.Name
            textLabel.TextColor3 = Color3.new(1, 1, 1) -- White text
            textLabel.TextScaled = true
        end
    end
end

local function onCharacterAdded(character)
    local player = game.Players:GetPlayerFromCharacter(character)
    if player then
        createESP(player)
    end
end

local function onPlayerAdded(player)
    -- Listen for the character to be added
    player.CharacterAdded:Connect(onCharacterAdded)

    -- Add ESP if the character already exists
    if player.Character then
        onCharacterAdded(player.Character)
    end
end

-- Connect PlayerAdded event
game.Players.PlayerAdded:Connect(onPlayerAdded)

-- Add ESP to existing players
for _, player in ipairs(game.Players:GetPlayers()) do
    onPlayerAdded(player)
end
