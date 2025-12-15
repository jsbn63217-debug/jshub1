-- JZ Hub GUI - Clean Premium Edition
local UserInputService = game:GetService("UserInputService")
local Players = game:GetService("Players")
local RunService = game:GetService("RunService")
local TweenService = game:GetService("TweenService")
local LocalPlayer = Players.LocalPlayer
local Camera = workspace.CurrentCamera

-- Configurações
local Config = {
    Aimbot = {Enabled = false, Target = "Head", TeamCheck = true},
    ESP = {Enabled = false},
    Tracer = {Enabled = false},
    NoClip = {Enabled = false},
    Hitbox = {Enabled = false, Size = Vector3.new(10, 10, 10)}
}

local isRightMouseDown = false
local espObjects = {}
local tracerObjects = {}

-- Criar ScreenGui
local ScreenGui = Instance.new("ScreenGui")
ScreenGui.Name = "JZHub"
ScreenGui.ResetOnSpawn = false
ScreenGui.ZIndexBehavior = Enum.ZIndexBehavior.Sibling

if LocalPlayer.PlayerGui:FindFirstChild("JZHub") then
    LocalPlayer.PlayerGui:FindFirstChild("JZHub"):Destroy()
end

ScreenGui.Parent = LocalPlayer.PlayerGui

-- Sistema de Key
local KeySystem = Instance.new("Frame")
KeySystem.Size = UDim2.new(0, 400, 0, 250)
KeySystem.Position = UDim2.new(0.5, -200, 0.5, -125)
KeySystem.BackgroundColor3 = Color3.fromRGB(20, 20, 25)
KeySystem.BorderSizePixel = 0
KeySystem.Parent = ScreenGui

local KeyCorner = Instance.new("UICorner")
KeyCorner.CornerRadius = UDim.new(0, 16)
KeyCorner.Parent = KeySystem

local KeyStroke = Instance.new("UIStroke")
KeyStroke.Color = Color3.fromRGB(120, 80, 200)
KeyStroke.Thickness = 2
KeyStroke.Parent = KeySystem

local KeyTitle = Instance.new("TextLabel")
KeyTitle.Size = UDim2.new(1, 0, 0, 60)
KeyTitle.Position = UDim2.new(0, 0, 0, 20)
KeyTitle.BackgroundTransparency = 1
KeyTitle.Text = "JZ HUB"
KeyTitle.TextColor3 = Color3.fromRGB(255, 255, 255)
KeyTitle.TextSize = 32
KeyTitle.Font = Enum.Font.GothamBold
KeyTitle.Parent = KeySystem

local KeySubtitle = Instance.new("TextLabel")
KeySubtitle.Size = UDim2.new(1, 0, 0, 25)
KeySubtitle.Position = UDim2.new(0, 0, 0, 75)
KeySubtitle.BackgroundTransparency = 1
KeySubtitle.Text = "Sistema de Autenticação"
KeySubtitle.TextColor3 = Color3.fromRGB(150, 150, 160)
KeySubtitle.TextSize = 13
KeySubtitle.Font = Enum.Font.Gotham
KeySubtitle.Parent = KeySystem

local KeyInput = Instance.new("TextBox")
KeyInput.Size = UDim2.new(0, 320, 0, 45)
KeyInput.Position = UDim2.new(0.5, -160, 0, 115)
KeyInput.BackgroundColor3 = Color3.fromRGB(30, 30, 38)
KeyInput.BorderSizePixel = 0
KeyInput.PlaceholderText = "Digite a key..."
KeyInput.PlaceholderColor3 = Color3.fromRGB(100, 100, 120)
KeyInput.Text = ""
KeyInput.TextColor3 = Color3.fromRGB(255, 255, 255)
KeyInput.TextSize = 14
KeyInput.Font = Enum.Font.Gotham
KeyInput.Parent = KeySystem

local InputCorner = Instance.new("UICorner")
InputCorner.CornerRadius = UDim.new(0, 10)
InputCorner.Parent = KeyInput

local SubmitButton = Instance.new("TextButton")
SubmitButton.Size = UDim2.new(0, 150, 0, 45)
SubmitButton.Position = UDim2.new(0.5, -75, 0, 175)
SubmitButton.BackgroundColor3 = Color3.fromRGB(120, 80, 200)
SubmitButton.BorderSizePixel = 0
SubmitButton.Text = "VERIFICAR"
SubmitButton.TextColor3 = Color3.fromRGB(255, 255, 255)
SubmitButton.TextSize = 14
SubmitButton.Font = Enum.Font.GothamBold
SubmitButton.AutoButtonColor = false
SubmitButton.Parent = KeySystem

local SubmitCorner = Instance.new("UICorner")
SubmitCorner.CornerRadius = UDim.new(0, 10)
SubmitCorner.Parent = SubmitButton

-- Frame Principal
local MainFrame = Instance.new("Frame")
MainFrame.Size = UDim2.new(0, 480, 0, 580)
MainFrame.Position = UDim2.new(0.5, -240, 0.5, -290)
MainFrame.BackgroundColor3 = Color3.fromRGB(20, 20, 25)
MainFrame.BorderSizePixel = 0
MainFrame.Active = true
MainFrame.Draggable = true
MainFrame.Visible = false
MainFrame.ClipsDescendants = true
MainFrame.Parent = ScreenGui

local MainCorner = Instance.new("UICorner")
MainCorner.CornerRadius = UDim.new(0, 16)
MainCorner.Parent = MainFrame

local MainStroke = Instance.new("UIStroke")
MainStroke.Color = Color3.fromRGB(120, 80, 200)
MainStroke.Thickness = 2
MainStroke.Parent = MainFrame

-- Header
local Header = Instance.new("Frame")
Header.Size = UDim2.new(1, 0, 0, 60)
Header.BackgroundColor3 = Color3.fromRGB(25, 25, 32)
Header.BorderSizePixel = 0
Header.Parent = MainFrame

local HeaderCorner = Instance.new("UICorner")
HeaderCorner.CornerRadius = UDim.new(0, 16)
HeaderCorner.Parent = Header
local Title = Instance.new("TextLabel")
Title.Size = UDim2.new(1, -120, 1, 0)
Title.Position = UDim2.new(0, 20, 0, 0)
Title.BackgroundTransparency = 1
Title.Text = "JZ HUB"
Title.TextColor3 = Color3.fromRGB(255, 255, 255)
Title.TextSize = 24
Title.Font = Enum.Font.GothamBold
Title.TextXAlignment = Enum.TextXAlignment.Left
Title.Parent = Header

local MinimizeButton = Instance.new("TextButton")
MinimizeButton.Size = UDim2.new(0, 40, 0, 40)
MinimizeButton.Position = UDim2.new(1, -90, 0, 10)
MinimizeButton.BackgroundColor3 = Color3.fromRGB(60, 60, 75)
MinimizeButton.Text = "−"
MinimizeButton.TextColor3 = Color3.fromRGB(255, 255, 255)
MinimizeButton.TextSize = 20
MinimizeButton.Font = Enum.Font.GothamBold
MinimizeButton.AutoButtonColor = false
MinimizeButton.Parent = Header

local MinCorner = Instance.new("UICorner")
MinCorner.CornerRadius = UDim.new(0, 10)
MinCorner.Parent = MinimizeButton

local CloseButton = Instance.new("TextButton")
CloseButton.Size = UDim2.new(0, 40, 0, 40)
CloseButton.Position = UDim2.new(1, -45, 0, 10)
CloseButton.BackgroundColor3 = Color3.fromRGB(220, 60, 60)
CloseButton.Text = "×"
CloseButton.TextColor3 = Color3.fromRGB(255, 255, 255)
CloseButton.TextSize = 24
CloseButton.Font = Enum.Font.GothamBold
CloseButton.AutoButtonColor = false
CloseButton.Parent = Header

local CloseCorner = Instance.new("UICorner")
CloseCorner.CornerRadius = UDim.new(0, 10)
CloseCorner.Parent = CloseButton

-- Content
local ContentFrame = Instance.new("ScrollingFrame")
ContentFrame.Size = UDim2.new(1, -30, 1, -80)
ContentFrame.Position = UDim2.new(0, 15, 0, 70)
ContentFrame.BackgroundTransparency = 1
ContentFrame.BorderSizePixel = 0
ContentFrame.ScrollBarThickness = 4
ContentFrame.ScrollBarImageColor3 = Color3.fromRGB(120, 80, 200)
ContentFrame.CanvasSize = UDim2.new(0, 0, 0, 680)
ContentFrame.Parent = MainFrame

-- Função criar seção
local function CreateSection(name, position, height)
    local Section = Instance.new("Frame")
    Section.Size = UDim2.new(1, 0, 0, height or 80)
    Section.Position = position
    Section.BackgroundColor3 = Color3.fromRGB(28, 28, 36)
    Section.BorderSizePixel = 0
    Section.Parent = ContentFrame
    
    local SectionCorner = Instance.new("UICorner")
    SectionCorner.CornerRadius = UDim.new(0, 12)
    SectionCorner.Parent = Section
    
    local SectionTitle = Instance.new("TextLabel")
    SectionTitle.Size = UDim2.new(1, -120, 0, 30)
    SectionTitle.Position = UDim2.new(0, 15, 0, 12)
    SectionTitle.BackgroundTransparency = 1
    SectionTitle.Text = name
    SectionTitle.TextColor3 = Color3.fromRGB(255, 255, 255)
    SectionTitle.TextSize = 16
    SectionTitle.Font = Enum.Font.GothamBold
    SectionTitle.TextXAlignment = Enum.TextXAlignment.Left
    SectionTitle.Parent = Section
    
    return Section
end

-- Função criar toggle
local function CreateToggle(parent, position, callback)
    local Toggle = Instance.new("TextButton")
    Toggle.Size = UDim2.new(0, 80, 0, 32)
    Toggle.Position = position
    Toggle.BackgroundColor3 = Color3.fromRGB(220, 60, 60)
    Toggle.Text = "OFF"
    Toggle.TextColor3 = Color3.fromRGB(255, 255, 255)
    Toggle.TextSize = 13
    Toggle.Font = Enum.Font.GothamBold
    Toggle.AutoButtonColor = false
    Toggle.Parent = parent
    
    local ToggleCorner = Instance.new("UICorner")
    ToggleCorner.CornerRadius = UDim.new(0, 8)
    ToggleCorner.Parent = Toggle
    
    local isEnabled = false
    
    Toggle.MouseButton1Click:Connect(function()
        isEnabled = not isEnabled
        local targetColor = isEnabled and Color3.fromRGB(60, 200, 80) or Color3.fromRGB(220, 60, 60)
        Toggle.Text = isEnabled and "ON" or "OFF"
        TweenService:Create(Toggle, TweenInfo.new(0.2), {BackgroundColor3 = targetColor}):Play()
        if callback then callback(isEnabled) end
    end)
    
    return Toggle
end

-- Seções
local AimbotSection = CreateSection("Aimbot", UDim2.new(0, 0, 0, 0), 180)
local AimbotToggle = CreateToggle(AimbotSection, UDim2.new(1, -95, 0, 12), function(enabled)
    Config.Aimbot.Enabled = enabled
end)

local TargetLabel = Instance.new("TextLabel")
TargetLabel.Size = UDim2.new(1, -20, 0, 22)
TargetLabel.Position = UDim2.new(0, 15, 0, 55)
TargetLabel.BackgroundTransparency = 1
TargetLabel.Text = "Parte do Corpo:"
TargetLabel.TextColor3 = Color3.fromRGB(180, 180, 190)
TargetLabel.TextSize = 13
TargetLabel.Font = Enum.Font.Gotham
TargetLabel.TextXAlignment = Enum.TextXAlignment.Left
TargetLabel.Parent = AimbotSection

local HeadButton = Instance.new("TextButton")
HeadButton.Size = UDim2.new(0, 110, 0, 36)
HeadButton.Position = UDim2.new(0, 15, 0, 80)
HeadButton.BackgroundColor3 = Color3.fromRGB(120, 80, 200)
HeadButton.Text = "Cabeça"
HeadButton.TextColor3 = Color3.fromRGB(255, 255, 255)
HeadButton.TextSize = 13
HeadButton.Font = Enum.Font.GothamBold
HeadButton.AutoButtonColor = false
HeadButton.Parent = AimbotSection

local HeadCorner = Instance.new("UICorner")
HeadCorner.CornerRadius = UDim.new(0, 8)
HeadCorner.Parent = HeadButton

local TorsoButton = Instance.new("TextButton")
TorsoButton.Size = UDim2.new(0, 110, 0, 36)
TorsoButton.Position = UDim2.new(0, 135, 0, 80)
TorsoButton.BackgroundColor3 = Color3.fromRGB(50, 50, 65)
TorsoButton.Text = "Peito"
TorsoButton.TextColor3 = Color3.fromRGB(255, 255, 255)
TorsoButton.TextSize = 13
TorsoButton.Font = Enum.Font.GothamBold
TorsoButton.AutoButtonColor = false
TorsoButton.Parent = AimbotSection

local TorsoCorner = Instance.new("UICorner")
TorsoCorner.CornerRadius = UDim.new(0, 8)
TorsoCorner.Parent = TorsoButton

local TeamCheckButton = Instance.new("TextButton")
TeamCheckButton.Size = UDim2.new(0, 230, 0, 36)
TeamCheckButton.Position = UDim2.new(0, 15, 0, 128)
TeamCheckButton.BackgroundColor3 = Color3.fromRGB(60, 200, 80)
TeamCheckButton.Text = "Filtrar Time: ON"
TeamCheckButton.TextColor3 = Color3.fromRGB(255, 255, 255)
TeamCheckButton.TextSize = 13
TeamCheckButton.Font = Enum.Font.GothamBold
TeamCheckButton.AutoButtonColor = false
TeamCheckButton.Parent = AimbotSection

local TeamCorner = Instance.new("UICorner")
TeamCorner.CornerRadius = UDim.new(0, 8)
TeamCorner.Parent = TeamCheckButton

-- ESP Section
local ESPSection = CreateSection("ESP (Wallhack)", UDim2.new(0, 0, 0, 190), 80)
local ESPToggle = CreateToggle(ESPSection, UDim2.new(1, -95, 0, 12), function(enabled)
    Config.ESP.Enabled = enabled
    if not enabled then
        for _, v in pairs(espObjects) do if v then v:Destroy() end end
        espObjects = {}
    end
end)

local ESPDesc = Instance.new("TextLabel")
ESPDesc.Size = UDim2.new(1, -20, 0, 30)
ESPDesc.Position = UDim2.new(0, 15, 0, 48)
ESPDesc.BackgroundTransparency = 1
ESPDesc.Text = "Ver jogadores através das paredes"
ESPDesc.TextColor3 = Color3.fromRGB(140, 140, 150)
ESPDesc.TextSize = 12
ESPDesc.Font = Enum.Font.Gotham
ESPDesc.TextXAlignment = Enum.TextXAlignment.Left
ESPDesc.TextWrapped = true
ESPDesc.Parent = ESPSection

-- Tracer Section
local TracerSection = CreateSection("Tracer (Linhas)", UDim2.new(0, 0, 0, 280), 80)
local TracerToggle = CreateToggle(TracerSection, UDim2.new(1, -95, 0, 12), function(enabled)
    Config.Tracer.Enabled = enabled
    if not enabled then
        for _, v in pairs(tracerObjects) do if v then v:Remove() end end
        tracerObjects = {}
    end
end)

local TracerDesc = Instance.new("TextLabel")
TracerDesc.Size = UDim2.new(1, -20, 0, 30)
TracerDesc.Position = UDim2.new(0, 15, 0, 48)
TracerDesc.BackgroundTransparency = 1
TracerDesc.Text = "Linhas apontando para os jogadores"
TracerDesc.TextColor3 = Color3.fromRGB(140, 140, 150)
TracerDesc.TextSize = 12
TracerDesc.Font = Enum.Font.Gotham
TracerDesc.TextXAlignment = Enum.TextXAlignment.Left
TracerDesc.TextWrapped = true
TracerDesc.Parent = TracerSection

-- NoClip Section
local NoClipSection = CreateSection("NoClip", UDim2.new(0, 0, 0, 370), 80)
local NoClipToggle = CreateToggle(NoClipSection, UDim2.new(1, -95, 0, 12), function(enabled)
    Config.NoClip.Enabled = enabled
end)

local NoClipDesc = Instance.new("TextLabel")
NoClipDesc.Size = UDim2.new(1, -20, 0, 30)
NoClipDesc.Position = UDim2.new(0, 15, 0, 48)
NoClipDesc.BackgroundTransparency = 1
NoClipDesc.Text = "Atravessar paredes e objetos sólidos"
NoClipDesc.TextColor3 = Color3.fromRGB(140, 140, 150)
NoClipDesc.TextSize = 12
NoClipDesc.Font = Enum.Font.Gotham
NoClipDesc.TextXAlignment = Enum.TextXAlignment.Left
NoClipDesc.TextWrapped = true
NoClipDesc.Parent = NoClipSection

-- Hitbox Section
local HitboxSection = CreateSection("Hitbox Expander", UDim2.new(0, 0, 0, 460), 110)
local HitboxToggle = CreateToggle(HitboxSection, UDim2.new(1, -95, 0, 12), function(enabled)
    Config.Hitbox.Enabled = enabled
end)

local HitboxDesc = Instance.new("TextLabel")
HitboxDesc.Size = UDim2.new(1, -20, 0, 30)
HitboxDesc.Position = UDim2.new(0, 15, 0, 48)
HitboxDesc.BackgroundTransparency = 1
HitboxDesc.Text = "Aumenta a área de acerto dos inimigos"
HitboxDesc.TextColor3 = Color3.fromRGB(140, 140, 150)
HitboxDesc.TextSize = 12
HitboxDesc.Font = Enum.Font.Gotham
HitboxDesc.TextXAlignment = Enum.TextXAlignment.Left
HitboxDesc.TextWrapped = true
HitboxDesc.Parent = HitboxSection

local SizeLabel = Instance.new("TextLabel")
SizeLabel.Size = UDim2.new(1, -20, 0, 22)
SizeLabel.Position = UDim2.new(0, 15, 0, 80)
SizeLabel.BackgroundTransparency = 1
SizeLabel.Text = "Tamanho: 10 studs"
SizeLabel.TextColor3 = Color3.fromRGB(180, 180, 190)
SizeLabel.TextSize = 13
SizeLabel.Font = Enum.Font.Gotham
SizeLabel.TextXAlignment = Enum.TextXAlignment.Left
SizeLabel.Parent = HitboxSection

-- Detectar botão direito
UserInputService.InputBegan:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.MouseButton2 then
        isRightMouseDown = true
    end
end)

UserInputService.InputEnded:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.MouseButton2 then
        isRightMouseDown = false
    end
end)

-- Função Aimbot
local function GetClosestPlayer()
    local closestPlayer = nil
    local shortestDistance = math.huge
    
    for _, player in pairs(Players:GetPlayers()) do
        if player ~= LocalPlayer and player.Character and player.Character:FindFirstChild("HumanoidRootPart") then
            local humanoid = player.Character:FindFirstChild("Humanoid")
            if not humanoid or humanoid.Health <= 0 then continue end
            if Config.Aimbot.TeamCheck and player.Team == LocalPlayer.Team then continue end
            
            local targetPart = player.Character:FindFirstChild(Config.Aimbot.Target)
            if targetPart then
                local vector, onScreen = Camera:WorldToViewportPoint(targetPart.Position)
                if onScreen then
                    local distance = (Vector2.new(vector.X, vector.Y) - Vector2.new(Camera.ViewportSize.X / 2, Camera.ViewportSize.Y / 2)).Magnitude
                    if distance < shortestDistance then
                        closestPlayer = player
                        shortestDistance = distance
                    end
                end
            end
        end
    end
    
    return closestPlayer
end

-- Loop Principal
RunService.RenderStepped:Connect(function()
    -- Aimbot
    if Config.Aimbot.Enabled and isRightMouseDown and LocalPlayer.Character then
        local target = GetClosestPlayer()
        if target and target.Character then
            local targetPart = target.Character:FindFirstChild(Config.Aimbot.Target)
            if targetPart then
                Camera.CFrame = CFrame.new(Camera.CFrame.Position, targetPart.Position)
            end
        end
    end
    
    -- ESP
    if Config.ESP.Enabled then
        for _, player in pairs(Players:GetPlayers()) do
            if player ~= LocalPlayer and player.Character then
                local humanoid = player.Character:FindFirstChild("Humanoid")
                if humanoid and humanoid.Health > 0 then
                    if not espObjects[player] then
                        local highlight = Instance.new("Highlight")
                        highlight.Adornee = player.Character
                        highlight.FillColor = Color3.fromRGB(255, 100, 100)
                        highlight.OutlineColor = Color3.fromRGB(255, 255, 255)
                        highlight.FillTransparency = 0.5
                        highlight.OutlineTransparency = 0
                        highlight.Parent = player.Character
                        espObjects[player] = highlight
                    end
                end
            end
        end
    end
    
    -- Tracer
    if Config.Tracer.Enabled then
        for _, v in pairs(tracerObjects) do if v then v:Remove() end end
        tracerObjects = {}
        
        for _, player in pairs(Players:GetPlayers()) do
            if player ~= LocalPlayer and player.Character and player.Character:FindFirstChild("HumanoidRootPart") then
                local humanoid = player.Character:FindFirstChild("Humanoid")
                if humanoid and humanoid.Health > 0 then
                    local hrp = player.Character.HumanoidRootPart
                    local vector, onScreen = Camera:WorldToViewportPoint(hrp.Position)
                    if onScreen then
                        local line = Drawing.new("Line")
                        line.From = Vector2.new(Camera.ViewportSize.X / 2, Camera.ViewportSize.Y)
                        line.To = Vector2.new(vector.X, vector.Y)
                        line.Color = Color3.fromRGB(255, 100, 100)
                        line.Thickness = 2
                        line.Visible = true
                        tracerObjects[player] = line
                    end
                end
            end
        end
    end
end)

-- NoClip
RunService.Stepped:Connect(function()
    if Config.NoClip.Enabled and LocalPlayer.Character then
        for _, part in pairs(LocalPlayer.Character:GetDescendants()) do
            if part:IsA("BasePart") then
                part.CanCollide = false
            end
        end
    end
end)

-- Hitbox
RunService.Heartbeat:Connect(function()
    if Config.Hitbox.Enabled then
        for _, player in pairs(Players:GetPlayers()) do
            if player ~= LocalPlayer and player.Character then
                local hrp = player.Character:FindFirstChild("HumanoidRootPart")
                if hrp then
                    hrp.Size = Config.Hitbox.Size
                    hrp.Transparency = 0.8
                    hrp.CanCollide = false
                end
            end
        end
    end
end)

-- Eventos botões
HeadButton.MouseButton1Click:Connect(function()
    Config.Aimbot.Target = "Head"
    TweenService:Create(HeadButton, TweenInfo.new(0.2), {BackgroundColor3 = Color3.fromRGB(120, 80, 200)}):Play()
    TweenService:Create(TorsoButton, TweenInfo.new(0.2), {BackgroundColor3 = Color3.fromRGB(50, 50, 65)}):Play()
end)

TorsoButton.MouseButton1Click:Connect(function()
    Config.Aimbot.Target = "Torso"
    TweenService:Create(TorsoButton, TweenInfo.new(0.2), {BackgroundColor3 = Color3.fromRGB(120, 80, 200)}):Play()
    TweenService:Create(HeadButton, TweenInfo.new(0.2), {BackgroundColor3 = Color3.fromRGB(50, 50, 65)}):Play()
end)

TeamCheckButton.MouseButton1Click:Connect(function()
    Config.Aimbot.TeamCheck = not Config.Aimbot.TeamCheck
    TeamCheckButton.Text = Config.Aimbot.TeamCheck and "Filtrar Time: ON" or "Filtrar Time: OFF"
    local targetColor = Config.Aimbot.TeamCheck and Color3.fromRGB(60, 200, 80) or Color3.fromRGB(220, 60, 60)
    TweenService:Create(TeamCheckButton, TweenInfo.new(0.2), {BackgroundColor3 = targetColor}):Play()
end)

MinimizeButton.MouseButton1Click:Connect(function()
    MainFrame.Visible = false
end)

CloseButton.MouseButton1Click:Connect(function()
    MainFrame.Visible = false
end)

-- Key System
SubmitButton.MouseButton1Click:Connect(function()
    if KeyInput.Text == "jzlindo" then
        KeySystem.Visible = false
        MainFrame.Visible = true
    else
        KeyInput.Text = ""
        KeyInput.PlaceholderText = "❌ Key incorreta!"
        wait(1.5)
        KeyInput.PlaceholderText = "Digite a key..."
    end
end)

-- CTRL para abrir
local ctrlPressed = false
UserInputService.InputBegan:Connect(function(input)
    if input.KeyCode == Enum.KeyCode.LeftControl or input.KeyCode == Enum.KeyCode.RightControl then
        ctrlPressed = true
    end
end)

UserInputService.InputEnded:Connect(function(input)
    if input.KeyCode == Enum.KeyCode.LeftControl or input.KeyCode == Enum.KeyCode.RightControl then
        if ctrlPressed then
            MainFrame.Visible = not MainFrame.Visible
        end
        ctrlPressed = false
    end
end)

print("✨ JZ HUB CARREGADO!")
print("🔑 Key: jzlindo")
print("⌨️ CTRL para abrir/fechar")
