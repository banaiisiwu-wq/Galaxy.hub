local Players = game:GetService("Players")
local TweenService = game:GetService("TweenService")
local RunService = game:GetService("RunService")
local player = Players.LocalPlayer

-- --- CONFIGURAÇÕES DE CORES ---
local AccentColor = Color3.fromRGB(0, 170, 255)
local BGColor = Color3.fromRGB(10, 10, 12)
local CardColor = Color3.fromRGB(15, 20, 25)

-- --- VARIÁVEIS DE CONTROLE ---
local godAtivo = false
local bottomFolder = nil

-- --- INTERFACE PRINCIPAL ---
local screenGui = Instance.new("ScreenGui")
screenGui.Name = "GalaxyHub_Final"
screenGui.ResetOnSpawn = false
screenGui.Parent = player:WaitForChild("PlayerGui")

-- --- TELA DE INTRO ---
local introText = Instance.new("TextLabel", screenGui)
introText.Size = UDim2.new(1, 0, 1, 0)
introText.BackgroundTransparency = 1 
introText.Text = "G A L A X Y  H U B"
introText.TextColor3 = AccentColor
introText.Font = Enum.Font.GothamBlack
introText.TextSize = 2
introText.TextTransparency = 1
introText.ZIndex = 10

task.spawn(function()
    local tweenIn = TweenService:Create(introText, TweenInfo.new(1.2, Enum.EasingStyle.Quint), {TextSize = 55, TextTransparency = 0})
    tweenIn:Play()
    task.wait(2)
    local tweenOut = TweenService:Create(introText, TweenInfo.new(1, Enum.EasingStyle.Quad), {TextTransparency = 1, TextSize = 70})
    tweenOut:Play()
    tweenOut.Completed:Wait()
    introText:Destroy()
    if screenGui:FindFirstChild("OpenBtn") then screenGui.OpenBtn.Visible = true end
end)

-- --- FUNÇÃO ARRASTAR ---
local function makeDraggable(gui)
    local dragging, dragInput, dragStart, startPos
    gui.InputBegan:Connect(function(input)
        if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then
            dragging = true; dragStart = input.Position; startPos = gui.Position
        end
    end)
    gui.InputChanged:Connect(function(input)
        if input.UserInputType == Enum.UserInputType.MouseMovement or input.UserInputType == Enum.UserInputType.Touch then
            dragInput = input
        end
    end)
    RunService.RenderStepped:Connect(function()
        if dragging and dragInput then
            local delta = dragInput.Position - dragStart
            gui.Position = UDim2.new(startPos.X.Scale, startPos.X.Offset + delta.X, startPos.Y.Scale, startPos.Y.Offset + delta.Y)
        end
    end)
end

-- --- BOTÃO ABRIR (70x70) ---
local openButton = Instance.new("TextButton", screenGui)
openButton.Name = "OpenBtn"; openButton.Size = UDim2.new(0, 70, 0, 70)
openButton.Position = UDim2.new(0, 20, 0.5, -35)
openButton.BackgroundColor3 = BGColor; openButton.Text = ""; openButton.Visible = false 
openButton.ZIndex = 5; openButton.ClipsDescendants = true
Instance.new("UICorner", openButton).CornerRadius = UDim.new(1, 0)
Instance.new("UIStroke", openButton).Color = AccentColor
makeDraggable(openButton)

local buttonImage = Instance.new("ImageLabel", openButton)
buttonImage.Size = UDim2.new(1, 0, 1, 0); buttonImage.BackgroundTransparency = 1; buttonImage.ZIndex = 6
buttonImage.Image = "https://www.roblox.com/asset-thumbnail/image?assetId=83833975390682&width=420&height=420&format=png"
Instance.new("UICorner", buttonImage).CornerRadius = UDim.new(1, 0)

-- --- FRAME PRINCIPAL ---
local mainFrame = Instance.new("Frame", screenGui)
mainFrame.Size = UDim2.new(0, 260, 0, 360); mainFrame.Position = UDim2.new(0.5, 0, 0.5, 0)
mainFrame.AnchorPoint = Vector2.new(0.5, 0.5); mainFrame.BackgroundColor3 = BGColor; mainFrame.Visible = false
Instance.new("UICorner", mainFrame); Instance.new("UIStroke", mainFrame).Color = AccentColor
makeDraggable(mainFrame)

local title = Instance.new("TextLabel", mainFrame)
title.Text = "GALAXY HUB"; title.Size = UDim2.new(1, 0, 0, 40); title.Position = UDim2.new(0,0,0,5)
title.BackgroundTransparency = 1; title.TextColor3 = AccentColor; title.Font = Enum.Font.GothamBlack; title.TextSize = 18

local credits = Instance.new("TextLabel", mainFrame)
credits.Text = "feito pelo miguelfipe"; credits.Size = UDim2.new(1, 0, 0, 20); credits.Position = UDim2.new(0, 0, 0, 35)
credits.BackgroundTransparency = 1; credits.TextColor3 = Color3.fromRGB(150, 150, 150); credits.Font = Enum.Font.GothamMedium; credits.TextSize = 12

-- --- MENU COINS (JANELA DE AVISO) ---
local coinsFrame = Instance.new("Frame", screenGui)
coinsFrame.Size = UDim2.new(0, 220, 0, 150); coinsFrame.Position = UDim2.new(0.5, 0, 0.5, 0)
coinsFrame.AnchorPoint = Vector2.new(0.5, 0.5); coinsFrame.BackgroundColor3 = BGColor; coinsFrame.Visible = false
Instance.new("UICorner", coinsFrame); Instance.new("UIStroke", coinsFrame).Color = Color3.fromRGB(255, 0, 0)
makeDraggable(coinsFrame)

local coinsMsg = Instance.new("TextLabel", coinsFrame)
coinsMsg.Size = UDim2.new(1, 0, 0.7, 0); coinsMsg.Position = UDim2.new(0, 0, 0.1, 0)
coinsMsg.BackgroundTransparency = 1; coinsMsg.TextColor3 = Color3.new(1, 1, 1); coinsMsg.Font = Enum.Font.GothamBold; coinsMsg.TextSize = 14
coinsMsg.Text = "🇧🇷 - não está disponível\n\n🇺🇸 - Not available"

local closeCoins = Instance.new("TextButton", coinsFrame)
closeCoins.Size = UDim2.new(0.8, 0, 0, 30); closeCoins.Position = UDim2.new(0.1, 0, 0.7, 0)
closeCoins.BackgroundColor3 = CardColor; closeCoins.Text = "OK"; closeCoins.TextColor3 = AccentColor
Instance.new("UICorner", closeCoins)
closeCoins.MouseButton1Click:Connect(function() coinsFrame.Visible = false end)

-- --- CONTAINER DE BOTÕES ---
local content = Instance.new("Frame", mainFrame)
content.Size = UDim2.new(0.9, 0, 0.6, 0); content.Position = UDim2.new(0.05, 0, 0.22, 0); content.BackgroundTransparency = 1
local layout = Instance.new("UIListLayout", content); layout.Padding = UDim.new(0, 8); layout.HorizontalAlignment = "Center"

local function createOption(text, callback)
    local btn = Instance.new("TextButton", content)
    btn.Size = UDim2.new(1, 0, 0, 40); btn.BackgroundColor3 = CardColor; btn.Text = text
    btn.TextColor3 = Color3.new(1, 1, 1); btn.Font = Enum.Font.GothamBold; btn.TextSize = 13
    Instance.new("UICorner", btn); local s = Instance.new("UIStroke", btn)
    s.Color = AccentColor; s.Transparency = 0.8; btn.MouseButton1Click:Connect(function() callback(btn) end)
    return btn
end

createOption("Free VIP", function(btn)
    for _, v in pairs(game.Workspace:GetDescendants()) do
        if v:IsA("BasePart") and (v.Name:find("VIP") or v.Name:find("Vip")) then
            v.Size = Vector3.new(50, 50, 50); v.CanCollide = false
            pcall(function() firetouchinterest(player.Character.HumanoidRootPart, v, 0); task.wait(0.1); firetouchinterest(player.Character.HumanoidRootPart, v, 1) end)
        end
    end
end)

createOption("God Mode V2", function(btn)
    godAtivo = not godAtivo
    btn.TextColor3 = godAtivo and AccentColor or Color3.new(1, 1, 1)
    if godAtivo then
        bottomFolder = Instance.new("Folder", workspace); bottomFolder.Name = "GalaxyAssets"
        local posBase = Vector3.new(193, -5, -135)
        for i = -100, 300 do
            local p = Instance.new("Part", bottomFolder); p.Size = Vector3.new(20, 2, 800); p.Anchored = true
            p.Color = Color3.new(0, 1, 1); p.Material = "Neon"; p.CFrame = CFrame.new(posBase) * CFrame.new(20 * i, 0, 0)
        end
        for _, v in pairs(workspace:GetDescendants()) do
            if v:IsA("BasePart") and (math.abs(v.Size.Y - 10) < 1) then v.CanCollide = false; v.Transparency = 0.7 end
        end
    else
        if bottomFolder then bottomFolder:Destroy() end
        for _, v in pairs(workspace:GetDescendants()) do
            if v:IsA("BasePart") and (math.abs(v.Size.Y - 10) < 1) then v.CanCollide = true; v.Transparency = 0 end
        end
    end
end)

createOption("Menu Coins", function() coinsFrame.Visible = true end)

-- --- BOTÕES SOCIAIS ---
local socialFrame = Instance.new("Frame", mainFrame)
socialFrame.Size = UDim2.new(0.9, 0, 0, 35); socialFrame.Position = UDim2.new(0.05, 0, 0.85, 0); socialFrame.BackgroundTransparency = 1
local socialLayout = Instance.new("UIListLayout", socialFrame); socialLayout.FillDirection = Enum.FillDirection.Horizontal
socialLayout.Padding = UDim.new(0, 10); socialLayout.HorizontalAlignment = "Center"

local function createSocial(text, color, link)
    local btn = Instance.new("TextButton", socialFrame); btn.Size = UDim2.new(0.45, 0, 1, 0); btn.BackgroundColor3 = CardColor
    btn.Text = text; btn.TextColor3 = color; btn.Font = Enum.Font.GothamBold; btn.TextSize = 11; Instance.new("UICorner", btn)
    local s = Instance.new("UIStroke", btn); s.Color = color; s.Transparency = 0.5
    btn.MouseButton1Click:Connect(function() setclipboard(link); local old = btn.Text; btn.Text = "Copiado!"; task.wait(1); btn.Text = old end)
end

createSocial("Discord", Color3.fromRGB(114, 137, 218), "https://discord.gg/veajHq65ud")
createSocial("Tiktok", Color3.fromRGB(255, 0, 0), "https://www.tiktok.com/@miguelfipe?_r=1&_t=ZS-93cJ8eueAXk")

openButton.MouseButton1Click:Connect(function() mainFrame.Visible = not mainFrame.Visible end)

