local Players = game:GetService("Players")
local TweenService = game:GetService("TweenService")
local Lighting = game:GetService("Lighting")
local player = Players.LocalPlayer

local blur = Instance.new("BlurEffect", Lighting)
blur.Size = 0
TweenService:Create(blur, TweenInfo.new(0.5), {Size = 24}):Play()

local screenGui = Instance.new("ScreenGui", player:WaitForChild("PlayerGui"))
screenGui.Name = "StellarLoader"
screenGui.ResetOnSpawn = false
screenGui.IgnoreGuiInset = true

local frame = Instance.new("Frame", screenGui)
frame.Size = UDim2.new(1, 0, 1, 0)
frame.BackgroundTransparency = 1

local bg = Instance.new("Frame", frame)
bg.Size = UDim2.new(1, 0, 1, 0)
bg.BackgroundColor3 = Color3.fromRGB(10, 10, 20)
bg.BackgroundTransparency = 1
bg.ZIndex = 0
TweenService:Create(bg, TweenInfo.new(0.5), {BackgroundTransparency = 0.3}):Play()

local word = "法兰西"
local letters = {}

local function tweenOutAndDestroy()
    for _, label in ipairs(letters) do
        TweenService:Create(label, TweenInfo.new(0.3), {TextTransparency = 1, TextSize = 20}):Play()
    end
    TweenService:Create(bg, TweenInfo.new(0.5), {BackgroundTransparency = 1}):Play()
    TweenService:Create(blur, TweenInfo.new(0.5), {Size = 0}):Play()
    wait(0.6)
    screenGui:Destroy()
    blur:Destroy()
end

for i = 1, #word do
    local char = word:sub(i, i)
    local label = Instance.new("TextLabel")
    label.Text = char
    label.Font = Enum.Font.GothamBlack
    label.TextColor3 = Color3.new(1, 1, 1)
    label.TextStrokeTransparency = 1
    label.TextTransparency = 1
    label.TextScaled = false
    label.TextSize = 30
    label.Size = UDim2.new(0, 60, 0, 60)
    label.AnchorPoint = Vector2.new(0.5, 0.5)
    label.Position = UDim2.new(0.5, (i - (#word / 2 + 0.5)) * 65, 0.5, 0)
    label.BackgroundTransparency = 1
    label.Parent = frame

    local gradient = Instance.new("UIGradient")
    gradient.Color = ColorSequence.new({
        ColorSequenceKeypoint.new(0, Color3.fromRGB(100, 170, 255)),
        ColorSequenceKeypoint.new(1, Color3.fromRGB(50, 100, 160))
    })
    gradient.Rotation = 90
    gradient.Parent = label

    local tweenIn = TweenService:Create(label, TweenInfo.new(0.3), {TextTransparency = 0, TextSize = 60})
    tweenIn:Play()
    table.insert(letters, label)
    wait(0.25)
end

wait(2)
tweenOutAndDestroy()

local StellarLibrary = (loadstring(game:HttpGet("https://raw.githubusercontent.com/Potato5466794/GC-WTB/refs/heads/main/UI/WTB-UI.luau")))()
if StellarLibrary:LoadAnimation() then
    StellarLibrary:StartLoad()
end
if StellarLibrary:LoadAnimation() then
    StellarLibrary:Loaded()
end

local UserInputService = game:GetService("UserInputService")
local Window = StellarLibrary:Window({
    SubTitle = "by 法兰西",
    Size = UserInputService.TouchEnabled and UDim2.new(0, 500, 0, 350) or UDim2.new(0, 500, 0, 320),
    TabWidth = 140
})

local InfoTab = Window:Tab("信息", "rbxassetid://128891143813807")
local UniversalTab = Window:Tab("通用", "rbxassetid://10723407389")
local TeleportTab = Window:Tab("传送", "rbxassetid://10734950309")
local PartUniversalTab = Window:Tab("部分通用加载器", "rbxassetid://10734950309")
local BattleTab = Window:Tab("最强战场", "rbxassetid://10723415335")
local UltimateBattleTab = Window:Tab("终极战场", "rbxassetid://10734984606")
local PlantTab = Window:Tab("植物大战机器人", "rbxassetid://10709782497")
local SlapTab = Window:Tab("巴掌大战", "rbxassetid://10747373176")

InfoTab:Seperator("声明")
InfoTab:Label("此脚本完全免费，请勿用于商业用途")

InfoTab:Seperator("基本信息")
if identifyexecutor then
    InfoTab:Label("执行器: " .. identifyexecutor())
else
    InfoTab:Label("执行器: 未知")
end
InfoTab:Label("昵称: " .. player.DisplayName)
InfoTab:Label("用户名: " .. player.Name)

local TimeLabel = InfoTab:Label("游戏时间: 00:00:00")
spawn(function()
    local startTime = tick()
    while true do
        local elapsed = math.floor(tick() - startTime)
        local h = math.floor(elapsed / 3600)
        local m = math.floor((elapsed % 3600) / 60)
        local s = elapsed % 60
        TimeLabel:Set(string.format("游戏时间: %02d:%02d:%02d", h, m, s))
        wait(1)
    end
end)

local BeijingTimeLabel = InfoTab:Label("北京时间: 加载中...")
spawn(function()
    while true do
        local t = os.date("*t", os.time())
        BeijingTimeLabel:Set(string.format("北京时间: %04d/%02d/%02d %02d:%02d:%02d", t.year, t.month, t.day, t.hour, t.min, t.sec))
        wait(1)
    end
end)

UniversalTab:Seperator("数值修改")
local speedEnabled = false
local currentSpeed = 16
local jumpEnabled = false
local currentJump = 50

local function setSpeed(v)
    local char = player.Character
    if char and char:FindFirstChildOfClass("Humanoid") then
        char.Humanoid.WalkSpeed = v
    end
end

local function setJump(v)
    local char = player.Character
    if char and char:FindFirstChildOfClass("Humanoid") then
        char.Humanoid.JumpPower = v
    end
end

spawn(function()
    while true do
        if speedEnabled then setSpeed(currentSpeed) end
        if jumpEnabled then setJump(currentJump) end
        wait()
    end
end)
