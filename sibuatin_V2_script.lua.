-- // Rayfield UI Loader
local Rayfield = loadstring(game:HttpGet('https://sirius.menu/rayfield'))()

-- // Window
local Window = Rayfield:CreateWindow({
    Name = "Sibuatan Hub",
    LoadingTitle = "Sibuatan Script",
    LoadingSubtitle = "By Lynx",
    ConfigurationSaving = {
        Enabled = true,
        FolderName = "SibuatanHub",
        FileName = "Config"
    },
    KeySystem = false
})

-- // Lokasi teleport
local locations = {
    ["Basecamp"]        = Vector3.new(987, 112, -698),
    ["Pos Pendakian"]   = Vector3.new(5200, 4000, 2100),
    ["Puncak Sibuatan"] = Vector3.new(5344, 8113, 2117),
    ["Checkpoint 19"]   = Vector3.new(1667, 4284, 5191),
}

local player = game.Players.LocalPlayer
local char = player.Character or player.CharacterAdded:Wait()

local function teleportTo(pos)
    if char and char:FindFirstChild("HumanoidRootPart") then
        char:MoveTo(pos)
    end
end

-- // Tabs
local teleportTab = Window:CreateTab("Teleport", 4483362458)
local playerTab   = Window:CreateTab("Player", 4483362458)
local utilityTab  = Window:CreateTab("Utility", 4483362458)

-- // Teleport Buttons
for name, pos in pairs(locations) do
    teleportTab:CreateButton({
        Name = name,
        Callback = function()
            teleportTo(pos)
        end,
    })
end

-- // Anti Fall Damage
local antiFall = false
playerTab:CreateToggle({
    Name = "Anti Fall Damage",
    CurrentValue = false,
    Callback = function(state)
        antiFall = state
        if state then
            player.CharacterAdded:Connect(function(c)
                c:WaitForChild("Humanoid").StateChanged:Connect(function(_, new)
                    if new == Enum.HumanoidStateType.Freefall then
                        c.Humanoid:ChangeState(Enum.HumanoidStateType.Jumping)
                    end
                end)
            end)
        end
    end,
})

-- // Fly V2
local flying = false
local flySpeed = 60

playerTab:CreateToggle({
    Name = "Fly V2",
    CurrentValue = false,
    Callback = function(state)
        flying = state
        local hrp = char:WaitForChild("HumanoidRootPart")
        local hum = char:WaitForChild("Humanoid")

        if flying then
            local bv = Instance.new("BodyVelocity", hrp)
            bv.Velocity = Vector3.zero
            bv.MaxForce = Vector3.new(400000, 400000, 400000)

            -- kontrol fly
            while flying and task.wait() do
                local dir = Vector3.zero
                if game.UserInputService:IsKeyDown(Enum.KeyCode.W) then dir += workspace.CurrentCamera.CFrame.LookVector end
                if game.UserInputService:IsKeyDown(Enum.KeyCode.S) then dir -= workspace.CurrentCamera.CFrame.LookVector end
                if game.UserInputService:IsKeyDown(Enum.KeyCode.A) then dir -= workspace.CurrentCamera.CFrame.RightVector end
                if game.UserInputService:IsKeyDown(Enum.KeyCode.D) then dir += workspace.CurrentCamera.CFrame.RightVector end
                if game.UserInputService:IsKeyDown(Enum.KeyCode.Space) then dir += Vector3.new(0,1,0) end
                if game.UserInputService:IsKeyDown(Enum.KeyCode.LeftShift) then dir -= Vector3.new(0,1,0) end

                bv.Velocity = dir * flySpeed
                hum.PlatformStand = true
            end
            bv:Destroy()
            hum.PlatformStand = false
        end
    end,
})

playerTab:CreateSlider({
    Name = "Fly Speed",
    Range = {10, 200},
    Increment = 5,
    Suffix = "Speed",
    CurrentValue = 60,
    Callback = function(value)
        flySpeed = value
    end,
})

-- // Anti Lag / Low Texture
utilityTab:CreateButton({
    Name = "Anti Lag / Low Texture",
    Callback = function()
        -- Hilangkan efek & kurangi lag
        for _,v in pairs(workspace:GetDescendants()) do
            if v:IsA("BasePart") then
                v.Material = Enum.Material.Plastic
                v.Reflectance = 0
            elseif v:IsA("Decal") or v:IsA("Texture") then
                v:Destroy()
            elseif v:IsA("ParticleEmitter") or v:IsA("Trail") then
                v.Enabled = false
            end
        end
        sethiddenproperty(game.Lighting, "Technology", Enum.Technology.Compatibility)
        game.Lighting.GlobalShadows = false
        game.Lighting.FogEnd = 9e9
        settings().Rendering.QualityLevel = Enum.QualityLevel.Level01
    end,
})
Rayfield:LoadConfiguration()
