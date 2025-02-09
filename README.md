local SeaIDs = {2753915549, 4442272183, 7449423635}
if table.find(SeaIDs, game.PlaceId) then
    local TweenService = game:GetService("TweenService")
    local RunService = game:GetService("RunService")
    local TeleportService = game:GetService("TeleportService")
    local Rayfield = loadstring(game:HttpGet('https://sirius.menu/rayfield'))()
    _G.GrabFruit = false
    _G.EspFruit = false
    _G.EspPlayer = false
    _G.AutoFarmChest = false

    function EnablePlayerESP()
        for _, player in pairs(game.Players:GetPlayers()) do
            if player ~= game.Players.LocalPlayer then
                if player.Character then
                    if not player.Character:FindFirstChild("PlayerESPHighlight") then
                        local hl = Instance.new("Highlight")
                        hl.Name = "PlayerESPHighlight"
                        hl.FillColor = Color3.new(1, 0, 0)
                        hl.OutlineColor = Color3.new(1, 1, 1)
                        hl.Parent = player.Character
                    end
                    if player.Character:FindFirstChild("Head") and not player.Character.Head:FindFirstChild("PlayerESPText") then
                        local billboard = Instance.new("BillboardGui")
                        billboard.Name = "PlayerESPText"
                        billboard.Adornee = player.Character.Head
                        billboard.Size = UDim2.new(0, 100, 0, 25)
                        billboard.StudsOffset = Vector3.new(0, 2, 0)
                        billboard.AlwaysOnTop = true
                        local textLabel = Instance.new("TextLabel")
                        textLabel.Size = UDim2.new(1, 0, 1, 0)
                        textLabel.BackgroundTransparency = 1
                        textLabel.Text = "@" .. player.Name:sub(1, 8)
                        textLabel.TextColor3 = Color3.new(1, 1, 1)
                        textLabel.TextScaled = false
                        textLabel.TextSize = 12
                        textLabel.Parent = billboard
                        billboard.Parent = player.Character.Head
                    end
                end
                player.CharacterAdded:Connect(function(character)
                    wait(1)
                    if _G.EspPlayer then
                        if not character:FindFirstChild("PlayerESPHighlight") then
                            local hl = Instance.new("Highlight")
                            hl.Name = "PlayerESPHighlight"
                            hl.FillColor = Color3.new(1, 0, 0)
                            hl.OutlineColor = Color3.new(1, 1, 1)
                            hl.Parent = character
                        end
                        if character:FindFirstChild("Head") and not character.Head:FindFirstChild("PlayerESPText") then
                            local billboard = Instance.new("BillboardGui")
                            billboard.Name = "PlayerESPText"
                            billboard.Adornee = character.Head
                            billboard.Size = UDim2.new(0, 100, 0, 25)
                            billboard.StudsOffset = Vector3.new(0, 2, 0)
                            billboard.AlwaysOnTop = true
                            local textLabel = Instance.new("TextLabel")
                            textLabel.Size = UDim2.new(1, 0, 1, 0)
                            textLabel.BackgroundTransparency = 1
                            textLabel.Text = "@" .. player.Name:sub(1, 8)
                            textLabel.TextColor3 = Color3.new(1, 1, 1)
                            textLabel.TextScaled = false
                            textLabel.TextSize = 12
                            textLabel.Parent = billboard
                            billboard.Parent = character.Head
                        end
                    end
                end)
            end
        end
    end

    function DisablePlayerESP()
        for _, player in pairs(game.Players:GetPlayers()) do
            if player ~= game.Players.LocalPlayer and player.Character then
                local hl = player.Character:FindFirstChild("PlayerESPHighlight")
                if hl then hl:Destroy() end
                if player.Character:FindFirstChild("Head") then
                    local billboard = player.Character.Head:FindFirstChild("PlayerESPText")
                    if billboard then billboard:Destroy() end
                end
            end
        end
    end

    function EspFruit()
        while _G.EspFruit do
            for _, fruit in ipairs(game.Workspace:GetChildren()) do
                if fruit.Name == "Fruit" and fruit:IsA("Model") then
                    local targetPart = fruit:FindFirstChild("Handle")
                    if not targetPart then
                        for _, child in ipairs(fruit:GetChildren()) do
                            if child:IsA("BasePart") then
                                targetPart = child
                                break
                            end
                        end
                    end
                    if targetPart then
                        local hl = fruit:FindFirstChild("FruitESPHighlight")
                        if not hl then
                            hl = Instance.new("Highlight")
                            hl.Name = "FruitESPHighlight"
                            hl.FillColor = Color3.new(1, 0, 0)
                            hl.OutlineColor = Color3.new(1, 0, 0)
                            hl.Parent = fruit
                        end
                        local billboard = targetPart:FindFirstChild("FruitESPText")
                        if not billboard then
                            billboard = Instance.new("BillboardGui")
                            billboard.Name = "FruitESPText"
                            billboard.Adornee = targetPart
                            billboard.Size = UDim2.new(0, 200, 0, 50)
                            billboard.StudsOffset = Vector3.new(0, 2, 0)
                            billboard.AlwaysOnTop = true
                            local textLabel = Instance.new("TextLabel")
                            textLabel.Size = UDim2.new(1, 0, 1, 0)
                            textLabel.BackgroundTransparency = 1
                            textLabel.Text = "Fruit"
                            textLabel.TextColor3 = Color3.new(1, 1, 1)
                            textLabel.TextScaled = true
                            textLabel.Parent = billboard
                            billboard.Parent = targetPart
                        end
                    else
                        local hl = fruit:FindFirstChild("FruitESPHighlight")
                        if hl then hl:Destroy() end
                        for _, obj in ipairs(fruit:GetDescendants()) do
                            if obj:IsA("BillboardGui") and obj.Name == "FruitESPText" then
                                obj:Destroy()
                            end
                        end
                    end
                end
            end
            wait(0.5)
        end
        for _, fruit in ipairs(game.Workspace:GetChildren()) do
            if fruit.Name == "Fruit" and fruit:IsA("Model") then
                local hl = fruit:FindFirstChild("FruitESPHighlight")
                if hl then hl:Destroy() end
                for _, obj in ipairs(fruit:GetDescendants()) do
                    if obj:IsA("BillboardGui") and obj.Name == "FruitESPText" then
                        obj:Destroy()
                    end
                end
            end
        end
    end

    local function FlyToFruit(targetPart)
        local player = game.Players.LocalPlayer
        if not (player and player.Character and player.Character:FindFirstChild("HumanoidRootPart")) then return end
        local hrp = player.Character.HumanoidRootPart
        local bodyVelocity = Instance.new("BodyVelocity")
        bodyVelocity.MaxForce = Vector3.new(5000, 5000, 5000)
        bodyVelocity.Parent = hrp
        local speed = 150
        local threshold = 3
        local connection
        connection = RunService.RenderStepped:Connect(function()
            if not _G.GrabFruit or not targetPart or not targetPart.Parent or not game.Workspace:FindFirstChild(targetPart.Parent.Name) then
                bodyVelocity:Destroy()
                connection:Disconnect()
                return
            end
            local direction = (targetPart.Position - hrp.Position)
            local distance = direction.Magnitude
            if distance < threshold then
                bodyVelocity:Destroy()
                connection:Disconnect()
                return
            end
            direction = direction.Unit
            bodyVelocity.Velocity = direction * speed
        end)
    end

    function GrabFruit()
        while _G.GrabFruit do
            wait(0.1)
            local fruitModel = game.Workspace:FindFirstChild("Fruit")
            if fruitModel then
                local targetPart = fruitModel:FindFirstChild("Handle")
                if not targetPart then
                    for _, child in ipairs(fruitModel:GetChildren()) do
                        if child:IsA("BasePart") then
                            targetPart = child
                            break
                        end
                    end
                end
                if targetPart then
                    FlyToFruit(targetPart)
                end
            end
        end
    end

    local function FlyToChest(targetPart, chest)
        local player = game.Players.LocalPlayer
        if not (player and player.Character and player.Character:FindFirstChild("HumanoidRootPart")) then return end
        local hrp = player.Character.HumanoidRootPart
        local bodyVelocity = Instance.new("BodyVelocity")
        bodyVelocity.MaxForce = Vector3.new(5000, 5000, 5000)
        bodyVelocity.Parent = hrp
        local speed = 150
        local threshold = 3
        local connection
        connection = RunService.RenderStepped:Connect(function()
            if not _G.AutoFarmChest or not targetPart or not targetPart.Parent or not chest or not chest.Parent then
                bodyVelocity:Destroy()
                connection:Disconnect()
                return
            end
            local direction = (targetPart.Position - hrp.Position)
            local distance = direction.Magnitude
            if distance < threshold then
                bodyVelocity.Velocity = Vector3.new(0, 0, 0)
                bodyVelocity:Destroy()
                connection:Disconnect()
                hrp.CFrame = CFrame.new(targetPart.Position)
                wait(0.3)
                if chest and chest.Parent then
                    chest:Destroy()
                end
            else
                direction = direction.Unit
                bodyVelocity.Velocity = direction * speed
            end
        end)
    end

    function AutoFarmChest()
        while _G.AutoFarmChest do
            local chestFolder = game.Workspace:FindFirstChild("ChestModels")
            if chestFolder then
                local validChests = {}
                for _, chest in ipairs(chestFolder:GetChildren()) do
                    if chest:IsA("Model") and (chest.Name == "DiamondChest" or chest.Name == "GoldChest" or chest.Name == "SilverChest") then
                        table.insert(validChests, chest)
                    end
                end
                if #validChests > 0 then
                    local chosenChest = validChests[math.random(1, #validChests)]
                    if chosenChest then
                        local targetPart = chosenChest:FindFirstChild("RootPart")
                        if not targetPart then
                            for _, child in ipairs(chosenChest:GetChildren()) do
                                if child:IsA("BasePart") then
                                    targetPart = child
                                    break
                                end
                            end
                        end
                        if targetPart then
                            FlyToChest(targetPart, chosenChest)
                            repeat wait(0.1) until not chosenChest.Parent
                        end
                    end
                end
            end
            wait(0.1)
        end
    end

    local aimlockEnabled = false
    local aimlockGui = nil
    local aimlockConnection = nil

    function SetAimlock(state)
        local player = game.Players.LocalPlayer
        local camera = game.Workspace.CurrentCamera
        if state == true and not aimlockEnabled then
            aimlockEnabled = true
            aimlockGui = Instance.new("ScreenGui")
            aimlockGui.Name = "AimlockGui"
            aimlockGui.Parent = player:WaitForChild("PlayerGui")
            local textButton = Instance.new("TextButton", aimlockGui)
            textButton.Size = UDim2.new(0, 200, 0, 50)
            textButton.Position = UDim2.new(0.5, -100, 0, 50)
            textButton.Text = "Desativar Seguimento"
            textButton.MouseButton1Click:Connect(function()
                SetAimlock(false)
            end)
            local function findClosestPlayer()
                local closestPlayer = nil
                local shortestDistance = math.huge
                if not player.Character or not player.Character:FindFirstChild("HumanoidRootPart") then return nil end
                for _, otherPlayer in pairs(game.Players:GetPlayers()) do
                    if otherPlayer ~= player and otherPlayer.Character then
                        local hrp = otherPlayer.Character:FindFirstChild("HumanoidRootPart")
                        if hrp then
                            local distance = (player.Character.HumanoidRootPart.Position - hrp.Position).Magnitude
                            if distance < shortestDistance then
                                shortestDistance = distance
                                closestPlayer = otherPlayer
                            end
                        end
                    end
                end
                return closestPlayer
            end
            aimlockConnection = RunService.Heartbeat:Connect(function()
                if aimlockEnabled then
                    local targetPlayer = findClosestPlayer()
                    if targetPlayer and targetPlayer.Character then
                        local hrp = targetPlayer.Character:FindFirstChild("HumanoidRootPart")
                        if hrp then
                            camera.CFrame = CFrame.new(camera.CFrame.Position, hrp.Position)
                        end
                    end
                end
            end)
        elseif state == false and aimlockEnabled then
            aimlockEnabled = false
            if aimlockGui then
                aimlockGui:Destroy()
                aimlockGui = nil
            end
            if aimlockConnection then
                aimlockConnection:Disconnect()
                aimlockConnection = nil
            end
        end
    end

    local MainWindow = Rayfield:CreateWindow({
        Name = "zClaw X Hub",
        Icon = 0,
        LoadingTitle = "zClaw X Hub",
        LoadingSubtitle = "by zClaw Team",
        Theme = "Green",
        DisableRayfieldPrompts = false,
        DisableBuildWarnings = false,
        ConfigurationSaving = {
            Enabled = true,
            FolderName = nil,
            FileName = "zClaw X Hub"
        },
        Discord = {
            Enabled = true,
            Invite = "https://discord.gg/esnWj5ddkz",
            RememberJoins = true
        },
        KeySystem = true,
        KeySettings = {
            Title = "Key System",
            Subtitle = "Key System",
            Note = " https://rekonise.com/zclaw-x-hubkey1-x3int ",
            FileName = "Key",
            SaveKey = true,
            GrabKeyFromSite = true,
            Key = {"X7B2F9K3"}
        }
    })

    local FruitTab = MainWindow:CreateTab("Fruit", 4483362458)
    local FruitSection = FruitTab:CreateSection("Fruit")
    local AutoFarmTab = MainWindow:CreateTab("AutoFarm", 4483362458)
    local AutoFarmSection = AutoFarmTab:CreateSection("AutoFarm")
    local PvPTab = MainWindow:CreateTab("PvP", 4483362458)
    local PvPSection = PvPTab:CreateSection("PvP")
    local EspTab = MainWindow:CreateTab("Esp", 4483362458)
    local EspSection = EspTab:CreateSection("Esp")
    local TeleportTab = MainWindow:CreateTab("Teleport", 4483362458)
    local TeleportSection = TeleportTab:CreateSection("Teleports Seas")

    Rayfield:Notify({
        Title = "zClaw X Hub Loading",
        Content = "zClaw X Hub by zClaw Team",
        Duration = 6.5,
        Image = 4483362458,
    })

    local EspPlayerToggle = EspTab:CreateToggle({
        Name = "EspPlayer",
        CurrentValue = false,
        Flag = "EspPlayer",
        Callback = function(Value)
            _G.EspPlayer = Value
            if _G.EspPlayer then
                EnablePlayerESP()
            else
                DisablePlayerESP()
            end
        end,
    })

    local EspFruitToggle = EspTab:CreateToggle({
        Name = "EspFruit",
        CurrentValue = false,
        Flag = "EspFruit",
        Callback = function(Value)
            _G.EspFruit = Value
            if _G.EspFruit then
                EspFruit()
            end
        end,
    })

    local GrabFruitToggle = FruitSection:CreateToggle({
        Name = "GrabFruit",
        CurrentValue = false,
        Flag = "GrabFruit",
        Callback = function(Value)
            _G.GrabFruit = Value
            if _G.GrabFruit then
                GrabFruit()
            end
        end,
    })

    local AutoFarmChestToggle = AutoFarmSection:CreateToggle({
        Name = "AutoFarm Chest",
        CurrentValue = false,
        Flag = "AutoFarmChest",
        Callback = function(Value)
            _G.AutoFarmChest = Value
            if _G.AutoFarmChest then
                AutoFarmChest()
            end
        end,
    })

    local AimlockToggle = PvPTab:CreateToggle({
        Name = "Aimlock",
        CurrentValue = false,
        Flag = "Aimlock",
        Callback = function(Value)
            SetAimlock(Value)
        end,
    })

    local Sea1Button = TeleportSection:CreateButton({
        Name = "Sea1",
        Callback = function()
            TeleportService:Teleport(2753915549, game.Players.LocalPlayer)
        end,
    })

    local Sea2Button = TeleportSection:CreateButton({
        Name = "Sea2",
        Callback = function()
            TeleportService:Teleport(4442272183, game.Players.LocalPlayer)
        end,
    })

    local Sea3Button = TeleportSection:CreateButton({
        Name = "Sea3",
        Callback = function()
            TeleportService:Teleport(7449423635, game.Players.LocalPlayer)
        end,
    })
end
