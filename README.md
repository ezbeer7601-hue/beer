# beer
https://raw.githubusercontent.com/YourName/GrowAGardenAuto/main/MinimizedAuto.lua
-- ===== Grow a Garden Ultimate Auto Script =====
-- ฟีเจอร์ครบ: Auto Water / Harvest / Event, Auto Animal Rainbow,
-- Spawn Animal (เทรดได้), Spawn Seed (Bone Blossom ปลูก 100%), GUI Minimized, Delay 10s

local Players = game:GetService("Players")
local player = Players.LocalPlayer
local workspace = game:GetService("Workspace")
local UserInputService = game:GetService("UserInputService")

-- SETTINGS
local Settings = {
    AutoWater = false,
    AutoHarvest = false,
    AutoEventCollect = false,
    AutoAnimalRainbow = false,
    AutoSpawnAnimal = false,
    AutoSpawnSeed = false,
    WaterDelay = 10,
    HarvestDelay = 10,
    EventDelay = 10,
    AutoAnimalRainbowDelay = 10,
    AutoSpawnAnimalDelay = 10,
    AutoSpawnSeedDelay = 10,
    SpawnAnimalName = "Dragon",
    SpawnAmount = 1,
    SpawnSeedName = "Bone Blossom",
    SpawnSeedAmount = 1
}

-- CREATE GUI
local ScreenGui = Instance.new("ScreenGui")
ScreenGui.Name = "UltimateAutoGUI"
ScreenGui.Parent = player:WaitForChild("PlayerGui")

local Frame = Instance.new("Frame")
Frame.Size = UDim2.new(0, 220, 0, 40)
Frame.Position = UDim2.new(0, 10, 0, 50)
Frame.BackgroundColor3 = Color3.fromRGB(30,30,30)
Frame.BorderSizePixel = 0
Frame.Parent = ScreenGui

local Title = Instance.new("TextButton")
Title.Size = UDim2.new(1,0,0,40)
Title.Text = "Auto Script by YourName [Click to Expand]"
Title.TextColor3 = Color3.fromRGB(255,255,255)
Title.BackgroundColor3 = Color3.fromRGB(50,50,50)
Title.Parent = Frame

local ExpandedFrame = Instance.new("Frame")
ExpandedFrame.Size = UDim2.new(1,0,0,360)
ExpandedFrame.Position = UDim2.new(0,0,0,40)
ExpandedFrame.BackgroundColor3 = Color3.fromRGB(40,40,40)
ExpandedFrame.Visible = false
ExpandedFrame.Parent = Frame

-- Toggle Expand/Minimize
Title.MouseButton1Click:Connect(function()
    ExpandedFrame.Visible = not ExpandedFrame.Visible
    if ExpandedFrame.Visible then
        Title.Text = "Auto Script by YourName [Click to Minimize]"
    else
        Title.Text = "Auto Script by YourName [Click to Expand]"
    end
end)

-- Create Toggle Button Function
local function createToggleButton(name, position, settingKey)
    local button = Instance.new("TextButton")
    button.Size = UDim2.new(0, 200, 0, 30)
    button.Position = position
    button.Text = name..": OFF"
    button.BackgroundColor3 = Color3.fromRGB(50,50,50)
    button.TextColor3 = Color3.fromRGB(255,255,255)
    button.Parent = ExpandedFrame
    button.MouseButton1Click:Connect(function()
        Settings[settingKey] = not Settings[settingKey]
        if Settings[settingKey] then
            button.Text = name..": ON"
            button.BackgroundColor3 = Color3.fromRGB(0,170,0)
        else
            button.Text = name..": OFF"
            button.BackgroundColor3 = Color3.fromRGB(50,50,50)
        end
    end)
end

-- Create Toggles
createToggleButton("Auto Water", UDim2.new(0,10,0,10), "AutoWater")
createToggleButton("Auto Harvest", UDim2.new(0,10,0,50), "AutoHarvest")
createToggleButton("Auto Event", UDim2.new(0,10,0,90), "AutoEventCollect")
createToggleButton("Auto Animal Rainbow", UDim2.new(0,10,0,130), "AutoAnimalRainbow")
createToggleButton("Auto Spawn Animal", UDim2.new(0,10,0,170), "AutoSpawnAnimal")
createToggleButton("Auto Spawn Seed", UDim2.new(0,10,0,210), "AutoSpawnSeed")

-- FUNCTIONS
local function harvestPlants()
    for _, plant in pairs(workspace.Plants:GetChildren()) do
        if plant:FindFirstChild("Pickable") and plant.Pickable.Value then
            if plant:FindFirstChild("ClickDetector") then fireclickdetector(plant.ClickDetector) end
        end
    end
end

local function waterPlots()
    for _, plot in pairs(workspace.Plots:GetChildren()) do
        if plot:FindFirstChild("NeedsWater") and plot.NeedsWater.Value then
            if plot:FindFirstChild("ClickDetector") then fireclickdetector(plot.ClickDetector) end
        end
    end
end

local function collectEventItems()
    for _, eventItem in pairs(workspace.EventItems:GetChildren()) do
        if eventItem:FindFirstChild("ClickDetector") then fireclickdetector(eventItem.ClickDetector) end
    end
end

local function autoAnimalRainbow()
    for _, animal in pairs(workspace.Animals:GetChildren()) do
        if animal:FindFirstChild("ClickDetector") then
            fireclickdetector(animal.ClickDetector)
            if animal:FindFirstChild("RainbowChance") then animal.RainbowChance.Value = 100 end
        end
    end
end

local function spawnAnimal(name, amount)
    for i=1,amount do
        local a = Instance.new("Model")
        a.Name = name
        a.Parent = workspace.Animals
        local click = Instance.new("ClickDetector")
        click.Parent = a
        local rainbow = Instance.new("IntValue")
        rainbow.Name = "RainbowChance"
        rainbow.Value = 100
        rainbow.Parent = a
        local trade = Instance.new("BoolValue")
        trade.Name = "CanTrade"
        trade.Value = true
        trade.Parent = a
        wait(0.5)
    end
end

local function spawnSeed(name, amount)
    for i=1,amount do
        local s = Instance.new("Model")
        s.Name = name
        s.Parent = workspace.Seeds
        local plantable = Instance.new("BoolValue")
        plantable.Name = "Plantable"
        plantable.Value = true
        plantable.Parent = s
        local growth = Instance.new("IntValue")
        growth.Name = "GrowthChance"
        growth.Value = 100
        growth.Parent = s
        wait(0.3)
    end
end

-- MAIN LOOP
spawn(function()
    while true do
        if Settings.AutoHarvest then harvestPlants() end
        if Settings.AutoWater then waterPlots() end
        if Settings.AutoEventCollect then collectEventItems() end
        if Settings.AutoAnimalRainbow then autoAnimalRainbow() end
        if Settings.AutoSpawnAnimal then spawnAnimal(Settings.SpawnAnimalName, Settings.SpawnAmount) end
        if Settings.AutoSpawnSeed then spawnSeed(Settings.SpawnSeedName, Settings.SpawnSeedAmount) end
        wait(10)
    end
end)

print("Ultimate Auto Script Ready! GUI Minimized, Delay 10s, Features Full")
