local SeaIDs = {2753915549, 4442272183, 7449423635}
if table.find(SeaIDs, game.PlaceId) then
    local Rayfield = loadstring(game:HttpGet('https://sirius.menu/rayfield'))()
    local RunService = game:GetService("RunService")

    _G.GrabFruit = false
    _G.EspFruit = false

    local function FlyToFruit(targetPart)
        local player = game.Players.LocalPlayer
        if not player or not player.Character or not player.Character:FindFirstChild("HumanoidRootPart") then return end
        
        local hrp = player.Character.HumanoidRootPart

        -- Criar BodyVelocity para levitação
        local bodyVelocity = Instance.new("BodyVelocity")
        bodyVelocity.Velocity = Vector3.new(0, 0, 0)
        bodyVelocity.MaxForce = Vector3.new(4000, 4000, 4000)
        bodyVelocity.Parent = hrp

        -- Voar até a fruta
        local connection
        connection = RunService.RenderStepped:Connect(function()
            if not _G.GrabFruit or not targetPart or not targetPart.Parent then
                bodyVelocity:Destroy()
                connection:Disconnect()
                return
            end

            local direction = (targetPart.Position - hrp.Position).unit
            bodyVelocity.Velocity = direction * 80 -- Velocidade de voo ajustável

            if (hrp.Position - targetPart.Position).Magnitude < 3 then
                bodyVelocity:Destroy()
                connection:Disconnect()
            end
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
            Enabled = false,
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
            GrabKeyFromSite = false,
            Key = {"X7B2F9K3"}
        }
    })

    local FruitTab = MainWindow:CreateTab("Fruit Tab", 4483362458)
    local FruitSection = FruitTab:CreateSection("Fruit")

    Rayfield:Notify({
        Title = "zClaw X Hub Loading",
        Content = "zClaw X Hub Loading",
        Duration = 6.5,
        Image = 4483362458,
    })

    local GrabFruitToggle = FruitTab:CreateToggle({
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

    local EspFruitToggle = FruitTab:CreateToggle({
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
end
