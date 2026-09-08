-- ==========================================================
-- NERO FE v20.0 [PREMIUM OMNI EDITION]
-- Lógica Avançada, Adaptador de Jogos Dinâmico e Interface Premium
-- Criadores: Dark & DemonFrota
-- ==========================================================
getgenv().NERO_FE_LOADED = nil 
task.wait(0.1)
getgenv().NERO_FE_LOADED = true

local Players = game:GetService("Players")
local UIS = game:GetService("UserInputService")
local RS = game:GetService("RunService")
local TS = game:GetService("TweenService")
local StarterGui = game:GetService("StarterGui")
local Lighting = game:GetService("Lighting")
local LP = Players.LocalPlayer
local Camera = workspace.CurrentCamera

local NERO = {
    ESP = false, Aimbot = false, Fly = false, Noclip = false,
    Kick = false, Shaders = false, Emotes = false, Antifling = false,
    Imortal = false, Tornado = false, Invisible = false,
    Backpack = false, Beijo = false, FlipCar = false, Launch = false,
    Speed = false, SpeedVal = 50, Jump = false,
    FOV = 150, TornadoRaio = 50, TornadoForca = 1000,
    TargetName = "", SavedPos = nil,
    Rainbow = false, Trail = false, DiscoSky = false, Spinbot = false, MoonGravity = false,
    Connections = {},
    
    -- [PREMIUM OMNI MODES]
    QuantumTrigger = false, AetherMagnet = false, ChronosOverdrive = false,
    MatrixDesync = false, GameSpecificMod1 = false, GameSpecificMod2 = false,
    
    -- [FUNÇÕES PREMIUM]
    Blackhole = false, Midas = false, FlingAura = false,
    TimeStop = false, Hitbox = false, UltraInstinct = false, FakeLag = false,
    
    -- [NOVAS FUNÇÕES VISÍVEIS FE]
    FlingAll = false, GlitchWalk = false, ChatSpam = false
}

local C = {
    bg = Color3.fromRGB(0, 0, 0),         
    surface = Color3.fromRGB(15, 15, 15),    
    tabBg = Color3.fromRGB(10, 10, 10),      
    primary = Color3.fromRGB(255, 85, 0),  -- Laranja mais forte
    text = Color3.fromRGB(255, 255, 255),
    subtext = Color3.fromRGB(160, 160, 170),
    premium = Color3.fromRGB(255, 165, 50), -- Laranja premium mais forte
    premiumBg = Color3.fromRGB(20, 10, 0)
}

local GameName = "Universal Sandbox"
local placeId = game.PlaceId

if placeId == 2753915549 or placeId == 4442272186 then
    GameName = "Blox Fruits"
elseif placeId == 4924922222 then
    GameName = "Brookhaven RP"
elseif placeId == 13772394625 then
    GameName = "Blade Ball"
elseif placeId == 6872265039 then
    GameName = "BedWars"
else
    pcall(function()
        local marketInfo = game:GetService("MarketplaceService"):GetProductInfo(placeId)
        if marketInfo and marketInfo.Name then GameName = marketInfo.Name end
    end)
end

local function Notify(texto)
    pcall(function()
        StarterGui:SetCore("SendNotification", { Title = "NERO FE", Text = texto, Duration = 3, Icon = "rbxassetid://10859948480" })
    end)
end

local function PremiumNotify(title, desc)
    task.spawn(function()
        if not Gui:FindFirstChild("PremiumNotifyContainer") then
            local pContainer = Instance.new("Frame", Gui)
            pContainer.Name = "PremiumNotifyContainer"
            pContainer.Size = UDim2.new(0, 280, 1, 0)
            pContainer.Position = UDim2.new(1, -290, 0, 0)
            pContainer.BackgroundTransparency = 1
        end
        local pNotify = Instance.new("Frame")
        pNotify.Size = UDim2.new(0, 260, 0, 70)
        pNotify.Position = UDim2.new(1, 30, 0.8, 0)
        pNotify.BackgroundColor3 = Color3.fromRGB(18, 17, 20)
        pNotify.Parent = Gui
        Instance.new("UICorner", pNotify).CornerRadius = UDim.new(0, 12)
        local stroke = Instance.new("UIStroke", pNotify)
        stroke.Color = C.premium; stroke.Thickness = 1.5
        local gradient = Instance.new("UIGradient", pNotify)
        gradient.Color = ColorSequence.new({ ColorSequenceKeypoint.new(0, C.primary), ColorSequenceKeypoint.new(1, C.premium) })
        
        local tLbl = Instance.new("TextLabel", pNotify)
        tLbl.Size = UDim2.new(1, -20, 0, 25); tLbl.Position = UDim2.new(0, 12, 0, 6)
        tLbl.BackgroundTransparency = 1; tLbl.Text = "👑 " .. title:upper()
        tLbl.TextColor3 = Color3.fromRGB(255, 255, 255); tLbl.Font = Enum.Font.GothamBold
        tLbl.TextSize = 13; tLbl.TextXAlignment = Enum.TextXAlignment.Left
        
        local dLbl = Instance.new("TextLabel", pNotify)
        dLbl.Size = UDim2.new(1, -24, 0, 35); dLbl.Position = UDim2.new(0, 12, 0, 28)
        dLbl.BackgroundTransparency = 1; dLbl.Text = desc
        dLbl.TextColor3 = Color3.fromRGB(200, 200, 210); dLbl.Font = Enum.Font.Gotham
        dLbl.TextSize = 10; dLbl.TextXAlignment = Enum.TextXAlignment.Left; dLbl.TextWrapped = true
        
        local sound = Instance.new("Sound", game.Workspace)
        sound.SoundId = "rbxassetid://138084657"; sound.Volume = 0.6; sound:Play()
        game.Debris:AddItem(sound, 2)
        
        TS:Create(pNotify, TweenInfo.new(0.4, Enum.EasingStyle.Back, Enum.EasingDirection.Out), {Position = UDim2.new(1, -280, 0.8, 0)}):Play()
        task.wait(3.5)
        TS:Create(pNotify, TweenInfo.new(0.3, Enum.EasingStyle.Quad, Enum.EasingDirection.In), {Position = UDim2.new(1, 30, 0.8, 0)}):Play()
        task.wait(0.3)
        pNotify:Destroy()
    end)
end

-- ==================== CHUVA DE CONFETES ====================
local function DispararConfetes(alvoGui)
    task.spawn(function()
        local confGui = Instance.new("ScreenGui")
        confGui.Name = "NeroConfetti"
        confGui.Parent = alvoGui.Parent
        
        local cores = {
            Color3.fromRGB(255, 126, 95), Color3.fromRGB(255, 185, 70), 
            Color3.fromRGB(255, 255, 255), Color3.fromRGB(150, 50, 255), 
            Color3.fromRGB(50, 255, 150)
        }
        
        for i = 1, 120 do
            local confete = Instance.new("Frame")
            confete.Size = UDim2.new(0, math.random(6, 12), 0, math.random(10, 20))
            confete.Position = UDim2.new(math.random(0, 100) / 100, 0, -0.1, 0)
            confete.BackgroundColor3 = cores[math.random(1, #cores)]
            confete.Rotation = math.random(0, 360)
            confete.BorderSizePixel = 0
            confete.Parent = confGui
            
            local tempo = math.random(25, 45) / 10
            local rotacaoFinal = confete.Rotation + math.random(-720, 720)
            
            local tween = TS:Create(confete, TweenInfo.new(tempo, Enum.EasingStyle.Linear), {
                Position = UDim2.new(confete.Position.X.Scale, math.random(-50, 50), 1.2, 0),
                Rotation = rotacaoFinal
            })
            tween:Play()
            tween.Completed:Connect(function() confete:Destroy() end)
            task.wait(0.01)
        end
        game.Debris:AddItem(confGui, 6)
    end)
end

local function getSafeParent()
    if gethui then return gethui() end
    local success, coreGui = pcall(function() return game:GetService("CoreGui") end)
    if success and coreGui then return coreGui end
    return LP:WaitForChild("PlayerGui")
end

local targetParent = getSafeParent()
local oldGui = targetParent:FindFirstChild("NERO_HTML")
if oldGui then oldGui:Destroy() end

local Gui = Instance.new("ScreenGui")
Gui.Name = "NERO_HTML"
Gui.ResetOnSpawn = false
Gui.Parent = targetParent
Gui.ZIndexBehavior = Enum.ZIndexBehavior.Sibling
Gui.IgnoreGuiInset = true 

-- ===== TAMANHO EXATO SOLICITADO: 480x360 E CENTRALIZADO =====
local Main = Instance.new("Frame")
Main.Size = UDim2.new(0, 360, 0, 301)  -- Ajustado perfeitamente para 480x360
Main.AnchorPoint = Vector2.new(0.5, 0.5) 
Main.Position = UDim2.new(0.5, 0, 0.5, 0)  
Main.BackgroundColor3 = C.bg
Main.BorderSizePixel = 0
Main.Parent = Gui
Instance.new("UICorner", Main).CornerRadius = UDim.new(0, 24)  -- Alterado para 24px

local BorderGlow = Instance.new("UIStroke", Main)
BorderGlow.Color = C.primary; BorderGlow.Thickness = 2
BorderGlow.ApplyStrokeMode = Enum.ApplyStrokeMode.Border
local TweenInfoPulse = TweenInfo.new(1.2, Enum.EasingStyle.Sine, Enum.EasingDirection.InOut, -1, true)
TS:Create(BorderGlow, TweenInfoPulse, {Transparency = 0.8}):Play()

-- ==================== ABAS VERTICAIS NA DIREITA ====================
local TabFrame = Instance.new("ScrollingFrame")
TabFrame.Size = UDim2.new(0, 90, 1, -40)
TabFrame.Position = UDim2.new(1, -110, 0, 20)
TabFrame.BackgroundTransparency = 1
TabFrame.ScrollBarThickness = 2
TabFrame.ScrollBarImageColor3 = C.primary
TabFrame.ScrollingDirection = Enum.ScrollingDirection.Y
TabFrame.CanvasSize = UDim2.new(0, 0, 0, 420)
TabFrame.Parent = Main
TabFrame.Active = false
TabFrame.Selectable = false

local tabs = {}
local tabNames = {"HUNTERS", "MAYHEM", "Teleporte", "VISUAIS", "CRÉDITOS", "NEXUS", "OMNIVERSE", "CATACLYSM", "MATRIX", "MIRAGE", "OVERDRIVE", "NETWORK"}
local tabContainers = {}

local tabBtnHeight = 30
local tabBtnGap = 5

for i, name in ipairs(tabNames) do
    local btn = Instance.new("TextButton")
    btn.Size = UDim2.new(1, 0, 0, tabBtnHeight)
    btn.Position = UDim2.new(0, 0, 0, (i-1) * (tabBtnHeight + tabBtnGap))
    btn.BackgroundColor3 = i == 1 and C.primary or C.tabBg
    btn.Text = name
    btn.TextColor3 = i == 1 and C.text or (name == "OMNIVERSE" and C.premium or C.primary)
    btn.TextSize = 8; btn.Font = Enum.Font.GothamBold; btn.Parent = TabFrame
    btn.ZIndex = 2
    Instance.new("UICorner", btn).CornerRadius = UDim.new(0, 10)

    if i == 1 then
        local bs = Instance.new("UIStroke", btn); bs.Color = C.primary; bs.Transparency = 0.6
    elseif name == "OMNIVERSE" then
        local bs = Instance.new("UIStroke", btn); bs.Color = C.premium; bs.Transparency = 0.4
    end

    local cont = Instance.new("ScrollingFrame")
    cont.Size = UDim2.new(1, -130, 1, -40)
    cont.Position = UDim2.new(0, 20, 0, 20)
    cont.BackgroundTransparency = 1
    cont.ScrollBarThickness = 2
    cont.ScrollBarImageColor3 = C.primary
    cont.CanvasSize = UDim2.new(0, 0, 0, 650)
    cont.Visible = i == 1; cont.Parent = Main
    cont.ZIndex = 1
    tabContainers[i] = cont
    table.insert(tabs, btn)
end
TabFrame.CanvasSize = UDim2.new(0, 0, 0, #tabNames * (tabBtnHeight + tabBtnGap))

for i, btn in ipairs(tabs) do
    btn.MouseButton1Click:Connect(function()
        for j, b in ipairs(tabs) do
            local active = (j == i)
            if tabNames[j] == "OMNIVERSE" then
                b.BackgroundColor3 = active and C.premium or C.tabBg
                b.TextColor3 = active and Color3.fromRGB(0,0,0) or C.premium
            else
                b.BackgroundColor3 = active and C.primary or C.tabBg
                b.TextColor3 = active and C.text or C.primary
            end
            tabContainers[j].Visible = active
            local stroke = b:FindFirstChildOfClass("UIStroke")
            if stroke then stroke:Destroy() end
            if active then
                local bs = Instance.new("UIStroke", b)
                bs.Color = tabNames[j] == "🌌 OMNIVERSE" and C.premium or C.primary; bs.Transparency = 0.5
            end
        end
    end)
end

-- ==================== BOTÃO DE FECHAR ====================
local CloseBtn = Instance.new("TextButton")
CloseBtn.Size = UDim2.new(0, 30, 0, 30)
CloseBtn.Position = UDim2.new(1, -40, 0, 10)
CloseBtn.BackgroundTransparency = 1
CloseBtn.Text = "X"
CloseBtn.TextColor3 = C.subtext
CloseBtn.TextSize = 18; CloseBtn.Font = Enum.Font.GothamBold
CloseBtn.ZIndex = 100
CloseBtn.Parent = Main

-- ==================== FUNÇÕES DE UI ====================
local function createToggle(name, parent, y)
    local frame = Instance.new("Frame")
    frame.Size = UDim2.new(1, 0, 0, 44); frame.Position = UDim2.new(0, 0, 0, y)
    frame.BackgroundColor3 = C.surface; frame.BorderSizePixel = 0; frame.Parent = parent
    Instance.new("UICorner", frame).CornerRadius = UDim.new(0, 20)
    
    local stroke = Instance.new("UIStroke", frame); stroke.Color = C.primary; stroke.Transparency = 0.7

    local label = Instance.new("TextLabel")
    label.Size = UDim2.new(0, 180, 1, 0); label.Position = UDim2.new(0, 15, 0, 0)
    label.BackgroundTransparency = 1; label.Text = name; label.TextColor3 = C.text
    label.TextSize = 12; label.Font = Enum.Font.GothamMedium; label.TextXAlignment = Enum.TextXAlignment.Left
    label.Parent = frame

    local toggleBg = Instance.new("Frame")
    toggleBg.Size = UDim2.new(0, 44, 0, 24); toggleBg.Position = UDim2.new(1, -55, 0, 10)
    toggleBg.BackgroundColor3 = C.tabBg; toggleBg.Parent = frame
    Instance.new("UICorner", toggleBg).CornerRadius = UDim.new(0, 12)

    local toggleKnob = Instance.new("Frame")
    toggleKnob.Size = UDim2.new(0, 20, 0, 20); toggleKnob.Position = UDim2.new(0, 2, 0, 2)
    toggleKnob.BackgroundColor3 = C.text; toggleKnob.Parent = toggleBg
    Instance.new("UICorner", toggleKnob).CornerRadius = UDim.new(0, 10)

    local toggleBtn = Instance.new("TextButton")
    toggleBtn.Size = UDim2.new(1, 0, 1, 0); toggleBtn.BackgroundTransparency = 1; toggleBtn.Text = ""; toggleBtn.Parent = frame

    return toggleBg, toggleKnob, toggleBtn
end

local function createPremiumToggle(name, desc, parent, y)
    local frame = Instance.new("Frame")
    frame.Size = UDim2.new(1, 0, 0, 50); frame.Position = UDim2.new(0, 0, 0, y)
    frame.BackgroundColor3 = C.premiumBg; frame.BorderSizePixel = 0; frame.Parent = parent
    Instance.new("UICorner", frame).CornerRadius = UDim.new(0, 16)
    
    local stroke = Instance.new("UIStroke", frame); stroke.Color = C.premium; stroke.Thickness = 1; stroke.Transparency = 0.5

    local label = Instance.new("TextLabel")
    label.Size = UDim2.new(0, 180, 0, 24); label.Position = UDim2.new(0, 15, 0, 4)
    label.BackgroundTransparency = 1; label.Text = name; label.TextColor3 = C.premium
    label.TextSize = 12; label.Font = Enum.Font.GothamBold; label.TextXAlignment = Enum.TextXAlignment.Left; label.Parent = frame

    local sublabel = Instance.new("TextLabel")
    sublabel.Size = UDim2.new(0, 180, 0, 16); sublabel.Position = UDim2.new(0, 15, 0, 24)
    sublabel.BackgroundTransparency = 1; sublabel.Text = desc; sublabel.TextColor3 = Color3.fromRGB(150, 150, 150)
    sublabel.TextSize = 9; sublabel.Font = Enum.Font.Gotham; sublabel.TextXAlignment = Enum.TextXAlignment.Left; sublabel.Parent = frame

    local toggleBg = Instance.new("Frame")
    toggleBg.Size = UDim2.new(0, 44, 0, 24); toggleBg.Position = UDim2.new(1, -55, 0, 13)
    toggleBg.BackgroundColor3 = Color3.fromRGB(35, 30, 25); toggleBg.Parent = frame
    Instance.new("UICorner", toggleBg).CornerRadius = UDim.new(0, 12)

    local toggleKnob = Instance.new("Frame")
    toggleKnob.Size = UDim2.new(0, 20, 0, 20); toggleKnob.Position = UDim2.new(0, 2, 0, 2)
    toggleKnob.BackgroundColor3 = Color3.fromRGB(220, 220, 220); toggleKnob.Parent = toggleBg
    Instance.new("UICorner", toggleKnob).CornerRadius = UDim.new(0, 10)

    local toggleBtn = Instance.new("TextButton")
    toggleBtn.Size = UDim2.new(1, 0, 1, 0); toggleBtn.BackgroundTransparency = 1; toggleBtn.Text = ""; toggleBtn.Parent = frame

    return toggleBg, toggleKnob, toggleBtn
end

local function updateToggle(bg, knob, on, isPremium)
    local activeColor = isPremium and C.premium or C.primary
    local inactiveColor = isPremium and Color3.fromRGB(35, 30, 25) or C.tabBg
    if on then
        TS:Create(knob, TweenInfo.new(0.15), {Position = UDim2.new(1, -22, 0, 2)}):Play()
        TS:Create(bg, TweenInfo.new(0.15), {BackgroundColor3 = activeColor}):Play()
    else
        TS:Create(knob, TweenInfo.new(0.15), {Position = UDim2.new(0, 2, 0, 2)}):Play()
        TS:Create(bg, TweenInfo.new(0.15), {BackgroundColor3 = inactiveColor}):Play()
    end
end

local function findTarget()
    local name = NERO.TargetName:lower()
    if name == "" then return nil end
    for _, p in pairs(Players:GetPlayers()) do
        if p ~= LP and p.Character and p.Character:FindFirstChild("Head") and p.Name:lower():find(name) then
            return p.Character
        end
    end
    return nil
end

-- ==================== ABA 1: PLAYERS ====================
local ETog, EKnob, EBtn = createToggle("ESP", tabContainers[1], 0)
local ATog, AKnob, ABtn = createToggle("Aimbot", tabContainers[1], 48)
local FTog, FKnob, FBtn = createToggle("Fly (Necessita Recarregar)", tabContainers[1], 96)
local NTog, NKnob, NBtn = createToggle("Noclip", tabContainers[1], 144)
local SpTog, SpKnob, SBtn = createToggle("Speed Mod", tabContainers[1], 192)
local JpTog, JpKnob, JpBtn = createToggle("Jump Infinito", tabContainers[1], 240)
local ImTog, ImKnob, ImBtn = createToggle("Imortalidade", tabContainers[1], 288)

-- ==================== ABA 2: TROLLING ====================
local ToTog, ToKnob, ToBtn = createToggle("Tornado Orbit", tabContainers[2], 48)
local InvTog, InvKnob, InvBtn = createToggle("Invisível (Client)", tabContainers[2], 96)
local BpTog, BpKnob, BpBtn = createToggle("Backpack (Montar)", tabContainers[2], 144)
local BjTog, BjKnob, BjBtn = createToggle("Beijo / Kiss", tabContainers[2], 192)
local LauTog, LauKnob, LauBtn = createToggle("Launch Fling (E)", tabContainers[2], 240)

local TargetInput = Instance.new("TextBox")
TargetInput.Size = UDim2.new(1, 0, 0, 44); TargetInput.Position = UDim2.new(0, 0, 0, 288)
TargetInput.BackgroundColor3 = C.surface; TargetInput.TextColor3 = C.text
TargetInput.TextSize = 12; TargetInput.Font = Enum.Font.Gotham; TargetInput.PlaceholderText = "Nome do Alvo para Troll..."
TargetInput.Text = ""; TargetInput.Parent = tabContainers[2]
Instance.new("UICorner", TargetInput).CornerRadius = UDim.new(0, 20)
local tis = Instance.new("UIStroke", TargetInput)
tis.Color = C.primary; tis.Transparency = 0.7
TargetInput.FocusLost:Connect(function() NERO.TargetName = TargetInput.Text; Notify("Alvo focado: " .. TargetInput.Text) end)

-- ==================== ABA 3: TP (TELEPORTE) ====================
local function createTabButton(name, parent, y, clickFunc)
    local btn = Instance.new("TextButton")
    btn.Size = UDim2.new(1, 0, 0, 44); btn.Position = UDim2.new(0, 0, 0, y)
    btn.BackgroundColor3 = C.surface; btn.Text = name; btn.TextColor3 = C.text
    btn.TextSize = 12; btn.Font = Enum.Font.GothamMedium; btn.Parent = parent
    Instance.new("UICorner", btn).CornerRadius = UDim.new(0, 20)
    local s = Instance.new("UIStroke", btn); s.Color = C.primary; s.Transparency = 0.7
    btn.MouseButton1Click:Connect(clickFunc)
    return btn
end

local spawnPos = Vector3.new(0, 50, 0)
pcall(function()
    local spawns = workspace:FindFirstChild("SpawnLocation") or workspace:FindFirstChild("Spawns")
    if spawns then spawnPos = spawns:GetPivot().Position + Vector3.new(0, 5, 0) end
end)

createTabButton("Teleportar para spawn (Spawn)", tabContainers[3], 0, function()
    if LP.Character and LP.Character:FindFirstChild("HumanoidRootPart") then 
        LP.Character.HumanoidRootPart.CFrame = CFrame.new(spawnPos); Notify("Teleportado para o início")
    end
end)

createTabButton("Teleportar para local", tabContainers[3], 48, function()
    if LP.Character and LP.Character:FindFirstChild("HumanoidRootPart") then 
        LP.Character.HumanoidRootPart.CFrame = CFrame.new(0, 25, 0); Notify("Teleportado para o local desejado")
    end
end)

createTabButton("Salvar Local", tabContainers[3], 96, function()
    if LP.Character and LP.Character:FindFirstChild("HumanoidRootPart") then
        NERO.SavedPos = LP.Character.HumanoidRootPart.CFrame; Notify("Local salvo com sucesso!")
    end
end)

createTabButton("Local Salvo", tabContainers[3], 144, function()
    if NERO.SavedPos and LP.Character and LP.Character:FindFirstChild("HumanoidRootPart") then
        LP.Character.HumanoidRootPart.CFrame = NERO.SavedPos; Notify("local salvo")
    else
        Notify("Nenhum local foi salvo ainda!")
    end
end)

-- >>> LISTA DINÂMICA DE PLAYERS <<<
local spectatingPlayer = nil

local PlayerListScroll = Instance.new("ScrollingFrame")
PlayerListScroll.Size = UDim2.new(1, 0, 0, 180)
PlayerListScroll.Position = UDim2.new(0, 0, 0, 192)
PlayerListScroll.BackgroundColor3 = C.surface
PlayerListScroll.BackgroundTransparency = 0.5
PlayerListScroll.BorderSizePixel = 0
PlayerListScroll.ScrollBarThickness = 3
PlayerListScroll.ScrollBarImageColor3 = C.primary
PlayerListScroll.Parent = tabContainers[3]
Instance.new("UICorner", PlayerListScroll).CornerRadius = UDim.new(0, 12)

local UIListLayout = Instance.new("UIListLayout")
UIListLayout.Parent = PlayerListScroll
UIListLayout.SortOrder = Enum.SortOrder.LayoutOrder
UIListLayout.Padding = UDim.new(0, 6)

local UIPadding = Instance.new("UIPadding")
UIPadding.Parent = PlayerListScroll
UIPadding.PaddingTop = UDim.new(0, 4)
UIPadding.PaddingLeft = UDim.new(0, 4)
UIPadding.PaddingRight = UDim.new(0, 4)
UIPadding.PaddingBottom = UDim.new(0, 4)

local function updatePlayerList()
    for _, child in pairs(PlayerListScroll:GetChildren()) do
        if child:IsA("Frame") then child:Destroy() end
    end

    for _, p in pairs(Players:GetPlayers()) do
        if p ~= LP then
            local Card = Instance.new("Frame")
            Card.Size = UDim2.new(1, 0, 0, 40)
            Card.BackgroundColor3 = C.surface
            Card.Parent = PlayerListScroll
            Instance.new("UICorner", Card).CornerRadius = UDim.new(0, 15)
            local stroke = Instance.new("UIStroke", Card)
            stroke.Color = C.primary
            stroke.Transparency = 0.8

            local NameLabel = Instance.new("TextLabel")
            NameLabel.Size = UDim2.new(1, -115, 1, 0)
            NameLabel.Position = UDim2.new(0, 10, 0, 0)
            NameLabel.BackgroundTransparency = 1
            NameLabel.Text = p.DisplayName or p.Name
            NameLabel.TextColor3 = C.text
            NameLabel.TextSize = 11
            NameLabel.Font = Enum.Font.GothamMedium
            NameLabel.TextXAlignment = Enum.TextXAlignment.Left
            NameLabel.TextTruncate = Enum.TextTruncate.AtEnd
            NameLabel.Parent = Card

            local TpBtn = Instance.new("TextButton")
            TpBtn.Size = UDim2.new(0, 42, 0, 26)
            TpBtn.Position = UDim2.new(1, -95, 0, 7)
            TpBtn.BackgroundColor3 = C.primary
            TpBtn.Text = "TP"
            TpBtn.TextColor3 = C.text
            TpBtn.TextSize = 10
            TpBtn.Font = Enum.Font.GothamBold
            TpBtn.Parent = Card
            Instance.new("UICorner", TpBtn).CornerRadius = UDim.new(0, 13)

            local CamBtn = Instance.new("TextButton")
            CamBtn.Size = UDim2.new(0, 45, 0, 26)
            CamBtn.Position = UDim2.new(1, -48, 0, 7)
            CamBtn.BackgroundColor3 = (spectatingPlayer == p) and C.primary or C.surface
            CamBtn.Text = (spectatingPlayer == p) and "VOLTAR" or "VER"
            CamBtn.TextColor3 = C.text
            CamBtn.TextSize = 9
            CamBtn.Font = Enum.Font.GothamBold
            CamBtn.Parent = Card
            Instance.new("UICorner", CamBtn).CornerRadius = UDim.new(0, 13)
            local camStroke = Instance.new("UIStroke", CamBtn)
            camStroke.Color = C.primary
            camStroke.Transparency = 0.5

            TpBtn.MouseButton1Click:Connect(function()
                if p.Character and p.Character:FindFirstChild("HumanoidRootPart") and LP.Character and LP.Character:FindFirstChild("HumanoidRootPart") then
                    LP.Character.HumanoidRootPart.CFrame = p.Character.HumanoidRootPart.CFrame
                    Notify("Teleportado para " .. p.Name)
                else
                    Notify("Player indisponível!")
                end
            end)

            CamBtn.MouseButton1Click:Connect(function()
                local cam = workspace.CurrentCamera
                if spectatingPlayer == p then
                    spectatingPlayer = nil
                    if LP.Character and LP.Character:FindFirstChild("Humanoid") then
                        cam.CameraSubject = LP.Character.Humanoid
                    end
                    Notify("Câmera resetada!")
                else
                    if p.Character and p.Character:FindFirstChild("Humanoid") then
                        spectatingPlayer = p
                        cam.CameraSubject = p.Character.Humanoid
                        Notify("Observando " .. p.Name)
                    else
                        Notify("Impossível visualizar!")
                    end
                end
                updatePlayerList()
            end)
        end
    end

    PlayerListScroll.CanvasSize = UDim2.new(0, 0, 0, UIListLayout.AbsoluteContentSize.Y + 8)
end

if tabContainers[3]:IsA("ScrollingFrame") then
    tabContainers[3]:GetPropertyChangedSignal("Visible"):Connect(function()
        if tabContainers[3].Visible then
            tabContainers[3].CanvasPosition = Vector2.new(0, 0)
        end
    end)
end

table.insert(NERO.Connections, Players.PlayerAdded:Connect(updatePlayerList))
table.insert(NERO.Connections, Players.PlayerRemoving:Connect(function(p)
    if spectatingPlayer == p then
        spectatingPlayer = nil
        if LP.Character and LP.Character:FindFirstChild("Humanoid") then
            workspace.CurrentCamera.CameraSubject = LP.Character.Humanoid
        end
    end
    updatePlayerList()
end))

updatePlayerList()

-- ==================== ABA 4: VISUAIS & FUN ====================
local RgTog, RgKnob, RgBtn = createToggle("Rainbow Char (RGB)", tabContainers[4], 144)
local TrTog, TrKnob, TrBtn = createToggle("Rastro Colorido (Trail)", tabContainers[4], 192)
local DiTog, DiKnob, DiBtn = createToggle("Disco Sky", tabContainers[4], 240)
local SpnTog, SpnKnob, SpnBtn = createToggle("Spinbot", tabContainers[4], 288)
local MgTog, MgKnob, MgBtn = createToggle("Moon Gravity", tabContainers[4], 336)

local hue = 0
table.insert(NERO.Connections, RS.RenderStepped:Connect(function()
    if NERO.Rainbow and LP.Character then
        hue = hue + 0.005; if hue >= 1 then hue = 0 end
        local color = Color3.fromHSV(hue, 1, 1)
        for _, v in pairs(LP.Character:GetDescendants()) do
            if v:IsA("BasePart") and v.Name ~= "HumanoidRootPart" then v.Color = color end
        end
    end
end))
RgBtn.MouseButton1Click:Connect(function() NERO.Rainbow = not NERO.Rainbow; updateToggle(RgTog, RgKnob, NERO.Rainbow) end)

local currentTrail, att0, att1
TrBtn.MouseButton1Click:Connect(function()
    NERO.Trail = not NERO.Trail; updateToggle(TrTog, TrKnob, NERO.Trail)
    if NERO.Trail and LP.Character and LP.Character:FindFirstChild("HumanoidRootPart") then
        local hrp = LP.Character.HumanoidRootPart
        att0 = Instance.new("Attachment", hrp); att0.Position = Vector3.new(0, 1, 0)
        att1 = Instance.new("Attachment", hrp); att1.Position = Vector3.new(0, -1.2, 0)
        currentTrail = Instance.new("Trail", hrp); currentTrail.Attachment0 = att0; currentTrail.Attachment1 = att1
        currentTrail.Lifetime = 0.8; currentTrail.LightEmission = 0.5
        currentTrail.Color = ColorSequence.new({ColorSequenceKeypoint.new(0, Color3.fromRGB(255, 0, 0)), ColorSequenceKeypoint.new(0.5, Color3.fromRGB(0, 255, 0)), ColorSequenceKeypoint.new(1, Color3.fromRGB(0, 0, 255))})
    else
        if currentTrail then currentTrail:Destroy() end
        if att0 then att0:Destroy() end
        if att1 then att1:Destroy() end
    end
end)

local discoHue = 0; local defaultAmbient = Lighting.Ambient
table.insert(NERO.Connections, RS.RenderStepped:Connect(function()
    if NERO.DiscoSky then
        discoHue = discoHue + 0.002; if discoHue > 1 then discoHue = 0 end
        Lighting.Ambient = Color3.fromHSV(discoHue, 1, 1)
    else Lighting.Ambient = defaultAmbient end
end))
DiBtn.MouseButton1Click:Connect(function() NERO.DiscoSky = not NERO.DiscoSky; updateToggle(DiTog, DiKnob, NERO.DiscoSky) end)

table.insert(NERO.Connections, RS.RenderStepped:Connect(function()
    if NERO.Spinbot and LP.Character and LP.Character:FindFirstChild("HumanoidRootPart") then
        LP.Character.HumanoidRootPart.CFrame = LP.Character.HumanoidRootPart.CFrame * CFrame.Angles(0, math.rad(25), 0)
    end
end))
SpnBtn.MouseButton1Click:Connect(function() NERO.Spinbot = not NERO.Spinbot; updateToggle(SpnTog, SpnKnob, NERO.Spinbot) end)

local defaultGravity = workspace.Gravity
MgBtn.MouseButton1Click:Connect(function() 
    NERO.MoonGravity = not NERO.MoonGravity; updateToggle(MgTog, MgKnob, NERO.MoonGravity)
    if NERO.MoonGravity then workspace.Gravity = 45 else workspace.Gravity = defaultGravity end
end)

-- >>> GAVETA RETRÁTIL: QUALIDADE DE VIDEO <<<
local QBtn = Instance.new("TextButton")
QBtn.Size = UDim2.new(1, 0, 0, 44)
QBtn.Position = UDim2.new(0, 0, 0, 384)
QBtn.BackgroundColor3 = C.surface
QBtn.Text = "Qualidade de video  ▼"
QBtn.TextColor3 = C.primary
QBtn.TextSize = 12
QBtn.Font = Enum.Font.GothamBold
QBtn.Parent = tabContainers[4]
Instance.new("UICorner", QBtn).CornerRadius = UDim.new(0, 20)
local qstroke = Instance.new("UIStroke", QBtn)
qstroke.Color = C.primary
qstroke.Transparency = 0.5

local QDrawer = Instance.new("Frame")
QDrawer.Size = UDim2.new(1, 0, 0, 0)
QDrawer.Position = UDim2.new(0, 0, 0, 434)
QDrawer.BackgroundColor3 = C.surface
QDrawer.BackgroundTransparency = 0.3
QDrawer.ClipsDescendants = true
QDrawer.Visible = false
QDrawer.Parent = tabContainers[4]
Instance.new("UICorner", QDrawer).CornerRadius = UDim.new(0, 14)

local drawerOpen = false
QBtn.MouseButton1Click:Connect(function()
    drawerOpen = not drawerOpen
    QBtn.Text = drawerOpen and "Qualidade de video  ▲" or "Qualidade de video  ▼"
    if drawerOpen then
        QDrawer.Visible = true
        game:GetService("TweenService"):Create(QDrawer, TweenInfo.new(0.3, Enum.EasingStyle.Quart, Enum.EasingDirection.Out), {Size = UDim2.new(1, 0, 0, 120)}):Play()
    else
        local tween = game:GetService("TweenService"):Create(QDrawer, TweenInfo.new(0.3, Enum.EasingStyle.Quart, Enum.EasingDirection.Out), {Size = UDim2.new(1, 0, 0, 0)})
        tween:Play()
        tween.Completed:Connect(function()
            if not drawerOpen then QDrawer.Visible = false end
        end)
    end
end)

local function createPresetButton(text, pos, size, callback)
    local btn = Instance.new("TextButton")
    btn.Size = size
    btn.Position = pos
    btn.BackgroundColor3 = C.surface
    btn.Text = text
    btn.TextColor3 = C.text
    btn.TextSize = 10
    btn.Font = Enum.Font.GothamMedium
    btn.Parent = QDrawer
    Instance.new("UICorner", btn).CornerRadius = UDim.new(0, 8)
    local s = Instance.new("UIStroke", btn)
    s.Color = C.primary
    s.Transparency = 0.8
    btn.MouseButton1Click:Connect(callback)
    return btn
end

createPresetButton("1080p (Alta)", UDim2.new(0, 8, 0, 8), UDim2.new(0.48, -10, 0, 30), function()
    pcall(function() settings().Rendering.QualityLevel = Enum.QualityLevel.Level10 end)
    Lighting.GlobalShadows = true
    Notify("Qualidade: 1080p (Máxima)")
end)

createPresetButton("720p (Média)", UDim2.new(0.5, 4, 0, 8), UDim2.new(0.48, -10, 0, 30), function()
    pcall(function() settings().Rendering.QualityLevel = Enum.QualityLevel.Level05 end)
    Lighting.GlobalShadows = true
    Notify("Qualidade: 720p (Equilibrada)")
end)

createPresetButton("480p (Economia)", UDim2.new(0, 8, 0, 44), UDim2.new(0.48, -10, 0, 30), function()
    pcall(function() settings().Rendering.QualityLevel = Enum.QualityLevel.Level03 end)
    Lighting.GlobalShadows = false
    Notify("Qualidade: 480p (Economia de Bateria)")
end)

createPresetButton("360p (Ultra Cool)", UDim2.new(0.5, 4, 0, 44), UDim2.new(0.48, -10, 0, 30), function()
    pcall(function() settings().Rendering.QualityLevel = Enum.QualityLevel.Level01 end)
    Lighting.GlobalShadows = false
    Lighting.FogEnd = 9e9
    Notify("Qualidade: 360p (Resfriamento Máximo)")
end)

local ResetBtn = Instance.new("TextButton")
ResetBtn.Size = UDim2.new(1, -16, 0, 28)
ResetBtn.Position = UDim2.new(0, 8, 0, 82)
ResetBtn.BackgroundColor3 = Color3.fromRGB(80, 20, 20)
ResetBtn.Text = "↺ Restaurar Padrão"
ResetBtn.TextColor3 = Color3.fromRGB(255, 100, 100)
ResetBtn.TextSize = 10
ResetBtn.Font = Enum.Font.GothamBold
ResetBtn.Parent = QDrawer
Instance.new("UICorner", ResetBtn).CornerRadius = UDim.new(0, 8)

ResetBtn.MouseButton1Click:Connect(function()
    pcall(function() settings().Rendering.QualityLevel = Enum.QualityLevel.Automatic end)
    Lighting.GlobalShadows = true
    Notify("Gráficos Restaurados!")
end)

-- ==================== ABA 5: CRÉDITOS ====================
local CTBackground = Instance.new("Frame")
CTBackground.Size = UDim2.new(1, 0, 0, 160); CTBackground.Position = UDim2.new(0, 0, 0, 20)
CTBackground.BackgroundColor3 = Color3.fromRGB(20, 22, 25); CTBackground.Parent = tabContainers[5]
Instance.new("UICorner", CTBackground).CornerRadius = UDim.new(0, 20)
local ctStroke = Instance.new("UIStroke", CTBackground); ctStroke.Color = C.primary; ctStroke.Thickness = 2

local CTTitle = Instance.new("TextLabel")
CTTitle.Size = UDim2.new(1, 0, 0, 40); CTTitle.Position = UDim2.new(0, 0, 0, 10)
CTTitle.BackgroundTransparency = 1; CTTitle.Text = "👑 NERO FE v20.0 👑"
CTTitle.TextColor3 = C.premium; CTTitle.Font = Enum.Font.GothamBold; CTTitle.TextSize = 22; CTTitle.Parent = CTBackground

local CTSub = Instance.new("TextLabel")
CTSub.Size = UDim2.new(1, 0, 0, 30); CTSub.Position = UDim2.new(0, 0, 0, 50)
CTSub.BackgroundTransparency = 1; CTSub.Text = "OMNI PREMIUM EDITION"
CTSub.TextColor3 = Color3.fromRGB(200, 200, 200); CTSub.Font = Enum.Font.GothamMedium; CTSub.TextSize = 12; CTSub.Parent = CTBackground

local CTAuthor = Instance.new("TextLabel")
CTAuthor.Size = UDim2.new(1, 0, 0, 40); CTAuthor.Position = UDim2.new(0, 0, 0, 95)
CTAuthor.BackgroundTransparency = 1; CTAuthor.Text = "Criado e Idealizado por:\nDark & DemonFrota"
CTAuthor.TextColor3 = C.primary; CTAuthor.Font = Enum.Font.GothamBold; CTAuthor.TextSize = 16; CTAuthor.Parent = CTBackground

-- ==================== ABA 6: CONFIG ====================
local function createSlider(name, y, min, max, val, set)
    local frame = Instance.new("Frame")
    frame.Size = UDim2.new(1, 0, 0, 44); frame.Position = UDim2.new(0, 0, 0, y)
    frame.BackgroundColor3 = C.surface; frame.Parent = tabContainers[6]
    Instance.new("UICorner", frame).CornerRadius = UDim.new(0, 20)
    local s = Instance.new("UIStroke", frame); s.Color = C.primary; s.Transparency = 0.7

    local label = Instance.new("TextLabel")
    label.Size = UDim2.new(0, 150, 1, 0); label.Position = UDim2.new(0, 15, 0, 0)
    label.BackgroundTransparency = 1; label.Text = name .. ": " .. val
    label.TextColor3 = C.text; label.TextSize = 12; label.Font = Enum.Font.GothamMedium
    label.TextXAlignment = Enum.TextXAlignment.Left; label.Parent = frame

    local input = Instance.new("TextBox")
    input.Size = UDim2.new(0, 60, 0, 26); input.Position = UDim2.new(1, -75, 0, 9)
    input.BackgroundColor3 = C.bg; input.TextColor3 = C.primary; input.TextSize = 12
    input.Font = Enum.Font.GothamBold; input.Text = tostring(val); input.Parent = frame
    Instance.new("UICorner", input).CornerRadius = UDim.new(0, 13)
    input.FocusLost:Connect(function()
        local v = tonumber(input.Text)
        if v then v = math.clamp(v, min, max); input.Text = tostring(v); label.Text = name .. ": " .. v; set(v) end
    end)
end
createSlider("FOV Area", 0, 30, 500, NERO.FOV, function(v) NERO.FOV = v end)
createSlider("Velocidade", 48, 16, 500, NERO.SpeedVal, function(v) NERO.SpeedVal = v end)

-- ==================== ABA 7: 🌌 OMNI ====================
local gName = (type(GameName) == "string" and GameName) or "Geral"

local OmniHeader = Instance.new("Frame")
OmniHeader.Size = UDim2.new(1, 0, 0, 40); OmniHeader.Position = UDim2.new(0, 0, 0, 0)
OmniHeader.BackgroundColor3 = Color3.fromRGB(20, 20, 25); OmniHeader.Parent = tabContainers[7]
Instance.new("UICorner", OmniHeader).CornerRadius = UDim.new(0, 12)
local OmniHeaderStroke = Instance.new("UIStroke", OmniHeader); OmniHeaderStroke.Color = C.premium; OmniHeaderStroke.Thickness = 1
 
local OmniHeaderText = Instance.new("TextLabel")
OmniHeaderText.Size = UDim2.new(1, 0, 1, 0); OmniHeaderText.BackgroundTransparency = 1
OmniHeaderText.Text = "⚡ ADAPTADOR ACTIVATED: " .. string.upper(gName)
OmniHeaderText.TextColor3 = C.premium; OmniHeaderText.Font = Enum.Font.GothamBold; OmniHeaderText.TextSize = 10; OmniHeaderText.Parent = OmniHeader
 
local Q_Tog, Q_Knob, Q_Btn = createPremiumToggle("Nexus Quantum Trigger", "Auto-interagir instantâneo com tudo em volta (Server-side)", tabContainers[7], 50)
local A_Tog, A_Knob, A_Btn = createPremiumToggle("Aether Vacuum Magnet", "Coleta drops, baús e itens soltos fisicamente no mapa", tabContainers[7], 106)
local C_Tog, C_Knob, C_Btn = createPremiumToggle("Chronos Click Overdrive", "Clika automaticamente em botões físicos (ClickDetectors)", tabContainers[7], 162)
local M_Tog, M_Knob, M_Btn = createPremiumToggle("Matrix Desync Shifter", "Modifica física e pacotes de rede para esquivar de ataques", tabContainers[7], 218)
 
table.insert(NERO.Connections, RS.Heartbeat:Connect(function()
    if NERO.QuantumTrigger then
        for _, prompt in pairs(workspace:GetDescendants()) do
            if prompt:IsA("ProximityPrompt") then
                pcall(function()
                    if LP:DistanceFromCharacter(prompt.Parent:GetPivot().Position) <= (prompt.MaxActivationDistance + 10) then fireproximityprompt(prompt) end
                end)
            end
        end
    end
end))
Q_Btn.MouseButton1Click:Connect(function() NERO.QuantumTrigger = not NERO.QuantumTrigger; updateToggle(Q_Tog, Q_Knob, NERO.QuantumTrigger, true); if NERO.QuantumTrigger then PremiumNotify("Quantum Trigger", "Varredura ativada!") end end)
 
table.insert(NERO.Connections, RS.Heartbeat:Connect(function()
    if NERO.AetherMagnet and LP.Character and LP.Character:FindFirstChild("HumanoidRootPart") then
        local hrp = LP.Character.HumanoidRootPart
        for _, part in pairs(workspace:GetDescendants()) do
            if part:IsA("TouchTransmitter") and part.Parent and part.Parent:IsA("BasePart") then
                pcall(function()
                    if LP:DistanceFromCharacter(part.Parent.Position) < 120 then firetouchinterest(hrp, part.Parent, 0); task.wait(0.01); firetouchinterest(hrp, part.Parent, 1) end
                end)
            end
        end
    end
end))
A_Btn.MouseButton1Click:Connect(function() NERO.AetherMagnet = not NERO.AetherMagnet; updateToggle(A_Tog, A_Knob, NERO.AetherMagnet, true); if NERO.AetherMagnet then PremiumNotify("Vacuum Magnet", "Coletor sincronizado.") end end)
 
table.insert(NERO.Connections, RS.Heartbeat:Connect(function()
    if NERO.ChronosOverdrive then
        for _, clicker in pairs(workspace:GetDescendants()) do
            if clicker:IsA("ClickDetector") then
                pcall(function() if LP:DistanceFromCharacter(clicker.Parent:GetPivot().Position) <= clicker.MaxActivationDistance then fireclickdetector(clicker) end end)
            end
        end
    end
end))
C_Btn.MouseButton1Click:Connect(function() NERO.ChronosOverdrive = not NERO.ChronosOverdrive; updateToggle(C_Tog, C_Knob, NERO.ChronosOverdrive, true); if NERO.ChronosOverdrive then PremiumNotify("Click Overdrive", "Engatado.") end end)
 
local desyncFlip = false
table.insert(NERO.Connections, RS.Heartbeat:Connect(function()
    if NERO.MatrixDesync and LP.Character and LP.Character:FindFirstChild("HumanoidRootPart") then
        desyncFlip = not desyncFlip
        pcall(function()
            local hrp = LP.Character.HumanoidRootPart
            if desyncFlip then hrp.Velocity = Vector3.new(0, -999, 0) else hrp.Velocity = Vector3.new(0, 999, 0) end
        end)
    end
end))
M_Btn.MouseButton1Click:Connect(function() NERO.MatrixDesync = not NERO.MatrixDesync; updateToggle(M_Tog, M_Knob, NERO.MatrixDesync, true); if NERO.MatrixDesync then PremiumNotify("Desync Matrix", "Hitbox dessincronizada.") end end)
 
local SpecName1, SpecDesc1, SpecName2, SpecDesc2 = "Horizon Velocity Glide", "Deslizar pelas paredes sem fricção", "Quantum Core Scanner", "Inspeciona canais ocultos"
if gName == "Blox Fruits" then
    SpecName1 = "Aura Chest Vortex"; SpecDesc1 = "Encontra o baú mais próximo e teleporta com segurança"
    SpecName2 = "Combat Instinct Predictor"; SpecDesc2 = "Desvia de skills detectando frames de ataque"
elseif gName == "Brookhaven RP" then
    SpecName1 = "Identity Matrix Erase"; SpecDesc1 = "Remove dados do avatar do servidor para bugar"
    SpecName2 = "Vortex Estate Hijacker"; SpecDesc2 = "Desativa sistemas de segurança de propriedades"
elseif gName == "Blade Ball" then
    SpecName1 = "Kinetic Parry Deflector"; SpecDesc1 = "Sincronizador quântico preventivo para bola"
    SpecName2 = "Temporal Phase Shift"; SpecDesc2 = "Modifica o ping aparente para estender o parry"
elseif gName == "BedWars" then
    SpecName1 = "Vortex Shop Bypass"; SpecDesc1 = "Abre a loja de qualquer lugar do mapa remotamente"
    SpecName2 = "Aura Bed Annihilator"; SpecDesc2 = "Quebra camas invisivelmente através de paredes"
end
 
local S1_Tog, S1_Knob, S1_Btn = createPremiumToggle(SpecName1, SpecDesc1, tabContainers[7], 274)
local S2_Tog, S2_Knob, S2_Btn = createPremiumToggle(SpecName2, SpecDesc2, tabContainers[7], 330)
 
table.insert(NERO.Connections, RS.Heartbeat:Connect(function()
    if NERO.GameSpecificMod1 then
        if gName == "Blox Fruits" then
            pcall(function() for _, v in pairs(workspace:GetChildren()) do if v.Name:find("Chest") and v:IsA("BasePart") then LP.Character.HumanoidRootPart.CFrame = v.CFrame + Vector3.new(0, 2, 0); break end end end)
        elseif gName == "Brookhaven RP" then
            pcall(function() if LP.Character then for _, v in pairs(LP.Character:GetDescendants()) do if v:IsA("StringValue") or v:IsA("ObjectValue") then v:Destroy() end end end end)
        else
            pcall(function() if LP.Character and LP.Character:FindFirstChild("Humanoid") then LP.Character.Humanoid.PlatformStand = true; LP.Character.HumanoidRootPart.Velocity = LP.Character.HumanoidRootPart.CFrame.LookVector * 100 end end)
        end
    else
        if gName ~= "Blox Fruits" and gName ~= "Brookhaven RP" and LP.Character and LP.Character:FindFirstChild("Humanoid") then pcall(function() LP.Character.Humanoid.PlatformStand = false end) end
    end
end))
S1_Btn.MouseButton1Click:Connect(function() NERO.GameSpecificMod1 = not NERO.GameSpecificMod1; updateToggle(S1_Tog, S1_Knob, NERO.GameSpecificMod1, true); if NERO.GameSpecificMod1 then PremiumNotify(SpecName1, "Ativado!") end end)
 
table.insert(NERO.Connections, RS.Heartbeat:Connect(function()
    if NERO.GameSpecificMod2 then
        if gName == "Blox Fruits" then
            pcall(function() if LP.Character and LP.Character:FindFirstChild("HumanoidRootPart") then LP.Character.HumanoidRootPart.CFrame = LP.Character.HumanoidRootPart.CFrame * CFrame.new(0, 0, 0.2) end end)
        elseif gName == "BedWars" then
            pcall(function() for _, v in pairs(workspace:GetDescendants()) do if v.Name:lower():find("bed") and v:IsA("BasePart") then if LP:DistanceFromCharacter(v.Position) < 25 then v:Destroy() end end end end)
        end
    end
end))
S2_Btn.MouseButton1Click:Connect(function() NERO.GameSpecificMod2 = not NERO.GameSpecificMod2; updateToggle(S2_Tog, S2_Knob, NERO.GameSpecificMod2, true); if NERO.GameSpecificMod2 then PremiumNotify(SpecName2, "Carregado.") end end)

-- >>> NOVO BOTÃO: NUVENS DE CHAT PERSONALIZADAS (CLIENT-SIDE) <<<
local B_Tog, B_Knob, B_Btn = createPremiumToggle("Aether Bubble Matrix", "Personaliza o design dos balões de chat (Local / Client-side)", tabContainers[7], 386)

local function setCustomBubbleChat(enable)
    pcall(function()
        local TextChatService = game:GetService("TextChatService")
        local ChatService = game:GetService("Chat")

        if enable then
            if TextChatService and TextChatService:FindFirstChild("BubbleChatConfiguration") then
                local bConfig = TextChatService.BubbleChatConfiguration
                bConfig.BackgroundColor3 = Color3.fromRGB(0, 0, 0)
                bConfig.TextColor3 = Color3.fromRGB(255, 117, 24)
                bConfig.Font = Enum.Font.GothamBold
                bConfig.CornerRadius = UDim.new(0, 8)
                bConfig.BackgroundTransparency = 0.1
            end
            
            if ChatService then
                ChatService:SetBubbleChatSettings({
                    BackgroundColor3 = Color3.fromRGB(18, 20, 28),
                    TextColor3 = Color3.fromRGB(255, 0, 128),
                    Font = Enum.Font.GothamBold,
                    TextSize = 15,
                    CornerRadius = 8,
                    Transparency = 0.1
                })
            end
        else
            if TextChatService and TextChatService:FindFirstChild("BubbleChatConfiguration") then
                local bConfig = TextChatService.BubbleChatConfiguration
                bConfig.BackgroundColor3 = Color3.fromRGB(250, 250, 250)
                bConfig.TextColor3 = Color3.fromRGB(57, 57, 57)
                bConfig.Font = Enum.Font.BuilderSansMedium
                bConfig.BackgroundTransparency = 0
            end

            if ChatService then
                ChatService:SetBubbleChatSettings({
                    BackgroundColor3 = Color3.fromRGB(255, 255, 255),
                    TextColor3 = Color3.fromRGB(57, 57, 57),
                    Font = Enum.Font.SourceSans,
                    TextSize = 14,
                    Transparency = 0
                })
            end
        end
    end)
end

B_Btn.MouseButton1Click:Connect(function()
    NERO.CustomBubbleChat = not NERO.CustomBubbleChat
    updateToggle(B_Tog, B_Knob, NERO.CustomBubbleChat, true)
    setCustomBubbleChat(NERO.CustomBubbleChat)
    if NERO.CustomBubbleChat then
        PremiumNotify("Bubble Customizer", "Balões de chat customizados (Visível apenas para você)!")
    else
        PremiumNotify("Bubble Customizer", "Design do chat restaurado.")
    end
end)

-- ==================== ABA 8: 🔥 CAOS ====================
local BhTog, BhKnob, BhBtn = createToggle("Buraco Negro (Itens Soltos)", tabContainers[8], 0)
local MdTog, MdKnob, MdBtn = createToggle("Toque de Midas (Tudo Ouro)", tabContainers[8], 48)
local FaTog, FaKnob, FaBtn = createToggle("Fling Spinbot (Ciclone)", tabContainers[8], 96)

table.insert(NERO.Connections, RS.Heartbeat:Connect(function()
    if NERO.Blackhole and LP.Character and LP.Character:FindFirstChild("HumanoidRootPart") then
        for _, v in pairs(workspace:GetDescendants()) do
            if v:IsA("BasePart") and not v.Anchored and not v:IsDescendantOf(LP.Character) then
                if not Players:GetPlayerFromCharacter(v.Parent) then
                    pcall(function() v.CFrame = LP.Character.HumanoidRootPart.CFrame end)
                end
            end
        end
    end
end))
BhBtn.MouseButton1Click:Connect(function() NERO.Blackhole = not NERO.Blackhole; updateToggle(BhTog, BhKnob, NERO.Blackhole) end)

local midasConn
MdBtn.MouseButton1Click:Connect(function()
    NERO.Midas = not NERO.Midas
    updateToggle(MdTog, MdKnob, NERO.Midas)
    if NERO.Midas and LP.Character and LP.Character:FindFirstChild("HumanoidRootPart") then
        midasConn = LP.Character.HumanoidRootPart.Touched:Connect(function(hit)
            if not hit:IsDescendantOf(LP.Character) and hit:IsA("BasePart") then
                hit.Material = Enum.Material.Neon; hit.Color = Color3.fromRGB(255, 215, 0)
            end
        end)
    else
        if midasConn then midasConn:Disconnect() end
    end
end)

local auraBody
FaBtn.MouseButton1Click:Connect(function()
    NERO.FlingAura = not NERO.FlingAura
    updateToggle(FaTog, FaKnob, NERO.FlingAura)
    if NERO.FlingAura and LP.Character and LP.Character:FindFirstChild("HumanoidRootPart") then
        auraBody = Instance.new("BodyAngularVelocity")
        auraBody.MaxTorque = Vector3.new(0, math.huge, 0)
        auraBody.AngularVelocity = Vector3.new(0, 500000, 0)
        auraBody.Parent = LP.Character.HumanoidRootPart
    else
        if auraBody then auraBody:Destroy() end
    end
end)

-- ==================== ABA 9: 🧠 LÓGICA ====================
local TsTog, TsKnob, TsBtn = createToggle("Za Warudo (Congelar Mapa)", tabContainers[9], 0)
local HxTog, HxKnob, HxBtn = createToggle("Hitbox Expander Global", tabContainers[9], 48)
local UiTog, UiKnob, UiBtn = createToggle("Instinto Superior (Esquiva)", tabContainers[9], 96)

table.insert(NERO.Connections, RS.Heartbeat:Connect(function()
    if NERO.TimeStop then
        for _, p in pairs(Players:GetPlayers()) do
            if p ~= LP and p.Character and p.Character:FindFirstChild("HumanoidRootPart") then
                p.Character.HumanoidRootPart.Anchored = true
            end
        end
    else
        for _, p in pairs(Players:GetPlayers()) do
            if p ~= LP and p.Character and p.Character:FindFirstChild("HumanoidRootPart") then
                p.Character.HumanoidRootPart.Anchored = false
            end
        end
    end
end))
TsBtn.MouseButton1Click:Connect(function() NERO.TimeStop = not NERO.TimeStop; updateToggle(TsTog, TsKnob, NERO.TimeStop) end)

table.insert(NERO.Connections, RS.Heartbeat:Connect(function()
    if NERO.Hitbox then
        for _, p in pairs(Players:GetPlayers()) do
            if p ~= LP and p.Character and p.Character:FindFirstChild("HumanoidRootPart") then
                p.Character.HumanoidRootPart.Size = Vector3.new(15, 15, 15)
                p.Character.HumanoidRootPart.Transparency = 0.6
                p.Character.HumanoidRootPart.BrickColor = BrickColor.new("Bright blue")
            end
        end
    end
end))
HxBtn.MouseButton1Click:Connect(function() NERO.Hitbox = not NERO.Hitbox; updateToggle(HxTog, HxKnob, NERO.Hitbox) end)

table.insert(NERO.Connections, RS.Heartbeat:Connect(function()
    if NERO.UltraInstinct and LP.Character and LP.Character:FindFirstChild("HumanoidRootPart") then
        for _, p in pairs(Players:GetPlayers()) do
            if p ~= LP and p.Character and p.Character:FindFirstChild("HumanoidRootPart") then
                local dist = (p.Character.HumanoidRootPart.Position - LP.Character.HumanoidRootPart.Position).Magnitude
                if dist > 0 and dist < 8 then
                    LP.Character.HumanoidRootPart.CFrame = LP.Character.HumanoidRootPart.CFrame * CFrame.new(0, 0, 15)
                end
            end
        end
    end
end))
UiBtn.MouseButton1Click:Connect(function() NERO.UltraInstinct = not NERO.UltraInstinct; updateToggle(UiTog, UiKnob, NERO.UltraInstinct) end)

-- ==================== ABA 10: 🎭 ILUSÃO ====================
createTabButton("Clonar a si mesmo (Jutsu)", tabContainers[10], 0, function()
    if LP.Character and LP.Character:FindFirstChild("HumanoidRootPart") then
        LP.Character.Archivable = true
        local clone = LP.Character:Clone()
        clone.Parent = workspace
        clone.HumanoidRootPart.CFrame = LP.Character.HumanoidRootPart.CFrame * CFrame.new(0, 0, -5)
        clone.Humanoid:MoveTo(clone.HumanoidRootPart.Position + (clone.HumanoidRootPart.CFrame.LookVector * 100))
        game.Debris:AddItem(clone, 6); Notify("Clone criado!")
    end
end)

local FlTog, FlKnob, FlBtn = createToggle("Fake Lag (Bugar Visão)", tabContainers[10], 48)
table.insert(NERO.Connections, RS.Heartbeat:Connect(function()
    if NERO.FakeLag and LP.Character and LP.Character:FindFirstChild("HumanoidRootPart") then
        LP.Character.HumanoidRootPart.Anchored = true; task.wait(0.2)
        LP.Character.HumanoidRootPart.Anchored = false; task.wait(0.5)
    end
end))
FlBtn.MouseButton1Click:Connect(function() NERO.FakeLag = not NERO.FakeLag; updateToggle(FlTog, FlKnob, NERO.FakeLag) end)

createTabButton("Céu Apocalíptico", tabContainers[10], 96, function()
    Lighting.TimeOfDay = "00:00:00"; Lighting.FogColor = Color3.fromRGB(255, 0, 0)
    Lighting.FogEnd = 200; Lighting.Ambient = Color3.fromRGB(255, 50, 50)
    Notify("Clima alterado localmente!")
end)

local NERO_Truss = Instance.new("TrussPart")
NERO_Truss.Size = Vector3.new(2, 50, 2)
NERO_Truss.Transparency = 1
NERO_Truss.Anchored = true
NERO_Truss.CanCollide = true

local WcTog, WcKnob, WcBtn = createToggle("Escalar Paredes (Aranha)", tabContainers[10], 144)
table.insert(NERO.Connections, RS.Heartbeat:Connect(function()
    if NERO.WallClimb and LP.Character and LP.Character:FindFirstChild("HumanoidRootPart") then
        local root = LP.Character.HumanoidRootPart
        local rayParams = RaycastParams.new()
        rayParams.FilterDescendantsInstances = {LP.Character, NERO_Truss}
        rayParams.FilterType = Enum.RaycastFilterType.Exclude

        local hit = workspace:Raycast(root.Position, root.CFrame.LookVector * 2.5, rayParams)

        if hit then
            NERO_Truss.Parent = workspace
            NERO_Truss.CFrame = CFrame.new(hit.Position)
        else
            NERO_Truss.Parent = nil
        end
    else
        NERO_Truss.Parent = nil
    end
end))
WcBtn.MouseButton1Click:Connect(function() 
    NERO.WallClimb = not NERO.WallClimb 
    updateToggle(WcTog, WcKnob, NERO.WallClimb)
    if NERO.WallClimb then 
        Notify("Escalar Paredes Ativado!") 
    else 
        Notify("Escalar Paredes Desativado!") 
    end
end)

-- ==================== ABA 11: 💣 EXTREMO (VISÍVEL PARA TODOS) ====================
local FAllTog, FAllKnob, FAllBtn = createToggle("Fling All (Física Replicada)", tabContainers[11], 0)
local GWTog, GWKnob, GWBtn = createToggle("Glitch Avatar (Convulsão FE)", tabContainers[11], 48)

local flingAllConn
FAllBtn.MouseButton1Click:Connect(function()
    NERO.FlingAll = not NERO.FlingAll
    updateToggle(FAllTog, FAllKnob, NERO.FlingAll)
    if NERO.FlingAll then
        local bav = Instance.new("BodyAngularVelocity")
        bav.Name = "NERO_Fling_Bav"
        bav.AngularVelocity = Vector3.new(0, 9999999, 0)
        bav.MaxTorque = Vector3.new(math.huge, math.huge, math.huge)
        if LP.Character and LP.Character:FindFirstChild("HumanoidRootPart") then bav.Parent = LP.Character.HumanoidRootPart end
        
        flingAllConn = RS.Heartbeat:Connect(function()
            if LP.Character and LP.Character:FindFirstChild("HumanoidRootPart") then
                local hrp = LP.Character.HumanoidRootPart
                for _, p in pairs(Players:GetPlayers()) do
                    if p ~= LP and p.Character and p.Character:FindFirstChild("HumanoidRootPart") then
                        hrp.CFrame = p.Character.HumanoidRootPart.CFrame * CFrame.new(0, 0, 0.5)
                        task.wait(0.05)
                    end
                end
            end
        end)
    else
        if flingAllConn then flingAllConn:Disconnect() end
        if LP.Character and LP.Character:FindFirstChild("HumanoidRootPart") then
            local b = LP.Character.HumanoidRootPart:FindFirstChild("NERO_Fling_Bav")
            if b then b:Destroy() end
        end
    end
end)

local glitchConn
GWBtn.MouseButton1Click:Connect(function()
    NERO.GlitchWalk = not NERO.GlitchWalk
    updateToggle(GWTog, GWKnob, NERO.GlitchWalk)
    if NERO.GlitchWalk then
        glitchConn = RS.Heartbeat:Connect(function()
            if LP.Character and LP.Character:FindFirstChild("HumanoidRootPart") and LP.Character:FindFirstChild("Humanoid") then
                local hrp = LP.Character.HumanoidRootPart
                LP.Character.Humanoid.PlatformStand = not LP.Character.Humanoid.PlatformStand
                hrp.Velocity = Vector3.new(math.random(-50,50), math.random(0,50), math.random(-50,50))
                hrp.CFrame = hrp.CFrame * CFrame.Angles(math.random(-2,2), math.random(-2,2), math.random(-2,2))
            end
        end)
    else
        if glitchConn then glitchConn:Disconnect() end
        if LP.Character and LP.Character:FindFirstChild("Humanoid") then LP.Character.Humanoid.PlatformStand = false end
    end
end)

-- ==================== ABA 12: 🗣️ SOCIAL (INTERAÇÃO DE SERVIDOR) ====================
local CSInput = Instance.new("TextBox")
CSInput.Size = UDim2.new(1, 0, 0, 44); CSInput.Position = UDim2.new(0, 0, 0, 0)
CSInput.BackgroundColor3 = C.surface; CSInput.TextColor3 = C.text
CSInput.TextSize = 12; CSInput.Font = Enum.Font.Gotham; CSInput.PlaceholderText = "Texto para Spammar..."
CSInput.Text = "Nero FE Dominando este servidor!"; CSInput.Parent = tabContainers[12]
Instance.new("UICorner", CSInput).CornerRadius = UDim.new(0, 20)
local csStroke = Instance.new("UIStroke", CSInput); csStroke.Color = C.primary; csStroke.Transparency = 0.7

local CSTog, CSKnob, CSBtn = createToggle("Ativar Chat Spammer", tabContainers[12], 48)

local spamConn
CSBtn.MouseButton1Click:Connect(function()
    NERO.ChatSpam = not NERO.ChatSpam
    updateToggle(CSTog, CSKnob, NERO.ChatSpam)
    if NERO.ChatSpam then
        spamConn = task.spawn(function()
            while NERO.ChatSpam do
                pcall(function()
                    local msg = CSInput.Text
                    if game:GetService("TextChatService").ChatVersion == Enum.ChatVersion.TextChatService then
                        game:GetService("TextChatService").TextChannels.RBXGeneral:SendAsync(msg)
                    else
                        game:GetService("ReplicatedStorage").DefaultChatSystemChatEvents.SayMessageRequest:FireServer(msg, "All")
                    end
                end)
                task.wait(1.5)
            end
        end)
    end
end)

createTabButton("Forçar Emote Dança (/e dance)", tabContainers[12], 96, function()
    pcall(function()
        if game:GetService("TextChatService").ChatVersion == Enum.ChatVersion.TextChatService then
            game:GetService("TextChatService").TextChannels.RBXGeneral:SendAsync("/e dance3")
        else
            game:GetService("ReplicatedStorage").DefaultChatSystemChatEvents.SayMessageRequest:FireServer("/e dance3", "All")
        end
    end)
    Notify("Dança enviada ao chat global!")
end)

-- ==================== LÓGICAS NATIVAS ORIGINAIS (INTACTAS) ====================
local function applyHighlight(player)
    if player == LP then return end
    local function setup(char)
        if not NERO.ESP then return end
        local hl = char:FindFirstChild("NERO_ESP") or Instance.new("Highlight")
        hl.Name = "NERO_ESP"; hl.FillColor = C.primary; hl.FillTransparency = 0.4
        hl.OutlineColor = Color3.fromRGB(255, 255, 255); hl.OutlineTransparency = 0.1
        hl.DepthMode = Enum.HighlightDepthMode.AlwaysOnTop; hl.Adornee = char; hl.Parent = char
    end
    if player.Character then setup(player.Character) end
    table.insert(NERO.Connections, player.CharacterAdded:Connect(setup))
end

EBtn.MouseButton1Click:Connect(function()
    NERO.ESP = not NERO.ESP; updateToggle(ETog, EKnob, NERO.ESP)
    for _, p in pairs(Players:GetPlayers()) do
        if NERO.ESP then applyHighlight(p) else
            if p.Character and p.Character:FindFirstChild("NERO_ESP") then p.Character.NERO_ESP:Destroy() end
        end
    end
end)
table.insert(NERO.Connections, Players.PlayerAdded:Connect(applyHighlight))

table.insert(NERO.Connections, RS.RenderStepped:Connect(function()
    if not NERO.Aimbot then return end
    local center = Vector2.new(Camera.ViewportSize.X / 2, Camera.ViewportSize.Y / 2)
    local closestTarget = nil; local closestDist = NERO.FOV

    for _, p in pairs(Players:GetPlayers()) do
        if p ~= LP and p.Character and p.Character:FindFirstChild("Head") and p.Character:FindFirstChild("Humanoid") and p.Character.Humanoid.Health > 0 then
            local sp, onScreen = Camera:WorldToScreenPoint(p.Character.Head.Position)
            if onScreen then
                local dist = (Vector2.new(sp.X, sp.Y) - center).Magnitude
                if dist < closestDist then closestDist = dist; closestTarget = p.Character.Head end
            end
        end
    end
    if closestTarget then Camera.CFrame = CFrame.new(Camera.CFrame.Position, closestTarget.Position) end
end))
ABtn.MouseButton1Click:Connect(function() NERO.Aimbot = not NERO.Aimbot; updateToggle(ATog, AKnob, NERO.Aimbot) end)

table.insert(NERO.Connections, RS.Heartbeat:Connect(function()
    if NERO.Imortal and LP.Character then
        local hum = LP.Character:FindFirstChildOfClass("Humanoid")
        if hum then
            if hum.Health < hum.MaxHealth then hum.Health = hum.MaxHealth end
            local state = hum:GetState()
            if state == Enum.HumanoidStateType.Dead or state == Enum.HumanoidStateType.FallingDown then hum:ChangeState(Enum.HumanoidStateType.GettingUp) end
        end
    end
end))
ImBtn.MouseButton1Click:Connect(function() NERO.Imortal = not NERO.Imortal; updateToggle(ImTog, ImKnob, NERO.Imortal) end)

table.insert(NERO.Connections, RS.Stepped:Connect(function()
    if not NERO.Noclip or not LP.Character then return end
    for _, part in pairs(LP.Character:GetDescendants()) do if part:IsA("BasePart") and part.CanCollide then part.CanCollide = false end end
end))
NBtn.MouseButton1Click:Connect(function() NERO.Noclip = not NERO.Noclip; updateToggle(NTog, NKnob, NERO.Noclip) end)

table.insert(NERO.Connections, RS.RenderStepped:Connect(function()
    if LP.Character and LP.Character:FindFirstChild("Humanoid") and NERO.Speed then LP.Character.Humanoid.WalkSpeed = NERO.SpeedVal end
end))
SBtn.MouseButton1Click:Connect(function() 
    NERO.Speed = not NERO.Speed; updateToggle(SpTog, SpKnob, NERO.Speed)
    if not NERO.Speed and LP.Character and LP.Character:FindFirstChild("Humanoid") then LP.Character.Humanoid.WalkSpeed = 16 end
end)

table.insert(NERO.Connections, UIS.JumpRequest:Connect(function()
    if NERO.Jump and LP.Character and LP.Character:FindFirstChild("Humanoid") then LP.Character.Humanoid:ChangeState(Enum.HumanoidStateType.Jumping) end
end))
JpBtn.MouseButton1Click:Connect(function() NERO.Jump = not NERO.Jump; updateToggle(JpTog, JpKnob, NERO.Jump) end)

local invisibilityLoaded = false; local invisibilityConnection
InvBtn.MouseButton1Click:Connect(function()
    NERO.Invisible = not NERO.Invisible; updateToggle(InvTog, InvKnob, NERO.Invisible)
    if NERO.Invisible then
        if not invisibilityLoaded then
            invisibilityLoaded = true; invisibilityConnection = loadstring(game:HttpGet('https://pastebin.com/raw/3Rnd9rHf'))(); Notify("Sistema de Invisibilidade Ativado!")
        end
    else
        if invisibilityLoaded then
            invisibilityLoaded = false
            if invisibilityConnection then
                pcall(function()
                    if LP.Character then
                        for _, v in pairs(LP.Character:GetDescendants()) do if v:IsA("BasePart") or v:IsA("Decal") then if v.Name ~= "HumanoidRootPart" then v.Transparency = 0 end end end
                    end
                    for _, v in pairs(LP.Character:GetDescendants()) do if v:IsA("Highlight") and v.Name == "InvisibilityHighlight" then v:Destroy() end end
                end)
                invisibilityConnection = nil
            end
            Notify("Sistema de Invisibilidade Desativado!")
        end
    end
end)

local angle = 0
table.insert(NERO.Connections, RS.Heartbeat:Connect(function()
    if (NERO.Backpack or NERO.Beijo or NERO.Tornado) then
        local target = findTarget()
        if target and target:FindFirstChild("Head") and LP.Character and LP.Character:FindFirstChild("HumanoidRootPart") then
            local hrp = LP.Character.HumanoidRootPart
            if NERO.Backpack then hrp.CFrame = target.Head.CFrame * CFrame.new(0, 3, 0)
            elseif NERO.Beijo then hrp.CFrame = target.Head.CFrame * CFrame.new(0, 0, -1.5)
            elseif NERO.Tornado then angle = angle + 10; hrp.CFrame = target.Head.CFrame * CFrame.Angles(0, math.rad(angle), 0) * CFrame.new(0, 0, -4) end
            hrp.Velocity = Vector3.new(0,0,0)
        end
    end
end))

BpBtn.MouseButton1Click:Connect(function() NERO.Backpack = not NERO.Backpack; updateToggle(BpTog, BpKnob, NERO.Backpack) end)
BjBtn.MouseButton1Click:Connect(function() NERO.Beijo = not NERO.Beijo; updateToggle(BjTog, BjKnob, NERO.Beijo) end)
ToBtn.MouseButton1Click:Connect(function() NERO.Tornado = not NERO.Tornado; updateToggle(ToTog, ToKnob, NERO.Tornado) end)

LauBtn.MouseButton1Click:Connect(function() 
    NERO.Launch = not NERO.Launch; updateToggle(LauTog, LauKnob, NERO.Launch) 
    if NERO.Launch then Notify("Fling ativado! Chegue perto e aperte 'E' no teclado.") end
end)

table.insert(NERO.Connections, UIS.InputBegan:Connect(function(input, gpe)
    if gpe then return end
    if NERO.Launch and input.KeyCode == Enum.KeyCode.E and LP.Character and LP.Character:FindFirstChild("HumanoidRootPart") then
        local hrp = LP.Character.HumanoidRootPart
        local spin = Instance.new("BodyAngularVelocity")
        spin.Name = "NERO_Spin"; spin.Parent = hrp
        spin.MaxTorque = Vector3.new(math.huge, math.huge, math.huge); spin.AngularVelocity = Vector3.new(0, 10000, 0)
        task.wait(0.5)
        if spin then spin:Destroy() end
    end
end))

FBtn.MouseButton1Click:Connect(function() 
    NERO.Fly = not NERO.Fly; updateToggle(FTog, FKnob, NERO.Fly)
    if NERO.Fly then task.spawn(function() pcall(loadstring(game:HttpGet("https://pastebin.com/raw/hbp8FPaX"))()) end) end 
end)

-- ==================== BOTÃO MINIMIZAR ====================
local NFContainer = Instance.new("Frame")
NFContainer.Size = UDim2.new(0, 50, 0, 50)
NFContainer.Position = UDim2.new(0, 15, 0, 75)  
NFContainer.BackgroundColor3 = Color3.fromRGB(15, 15, 15)
NFContainer.Visible = false
NFContainer.Parent = Gui
Instance.new("UICorner", NFContainer).CornerRadius = UDim.new(0, 25)

local NFStroke = Instance.new("UIStroke", NFContainer)
NFStroke.Color = C.primary; NFStroke.Thickness = 1.2  -- Borda mais fina
NFStroke.Transparency = 0  -- Sem transparência (cor mais forte)
TS:Create(NFStroke, TweenInfoPulse, {Transparency = 0.3}):Play()

local NFBtn = Instance.new("TextButton")
NFBtn.Size = UDim2.new(1,0,1,0)
NFBtn.BackgroundTransparency = 1
NFBtn.Text = "N"
NFBtn.TextColor3 = C.primary
NFBtn.TextSize = 20
NFBtn.Font = Enum.Font.GothamBold
NFBtn.Parent = NFContainer

CloseBtn.MouseButton1Click:Connect(function()
    Main.Visible = false
    NFContainer.Visible = true
end)

NFBtn.MouseButton1Click:Connect(function()
    Main.Visible = true
    NFContainer.Visible = false
    Main.Position = UDim2.new(0.5, 0, 0.5, 0)  
end)

-- Função de arrastar
local function makeSmoothDraggable(obj)
    local dragging, dragInput, dragStart, startPos
    obj.InputBegan:Connect(function(input)
        if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then
            dragging, dragStart, startPos = true, input.Position, obj.Position
            input.Changed:Connect(function() if input.UserInputState == Enum.UserInputState.End then dragging = false end end)
        end
    end)
    obj.InputChanged:Connect(function(input)
        if input.UserInputType == Enum.UserInputType.MouseMovement or input.UserInputType == Enum.UserInputType.Touch then dragInput = input end
    end)
    table.insert(NERO.Connections, UIS.InputChanged:Connect(function(input)
        if input == dragInput and dragging then
            local delta = input.Position - dragStart
            TS:Create(obj, TweenInfo.new(0.12, Enum.EasingStyle.OutQuad), {Position = UDim2.new(startPos.X.Scale, startPos.X.Offset + delta.X, startPos.Y.Scale, startPos.Y.Offset + delta.Y)}):Play()
        end
    end))
end

makeSmoothDraggable(Main)
makeSmoothDraggable(NFContainer)

-- MÓDULO BOTÃO VIRTUAL MOBILE (E)
local ScreenGui = targetParent:FindFirstChild("NERO_HTML")
if ScreenGui then
    local EContainer = Instance.new("Frame")
    EContainer.Name = "NERO_E_Button"
    EContainer.Size = UDim2.new(0, 50, 0, 50)
    EContainer.Position = UDim2.new(0.85, 0, 0.5, 0) 
    EContainer.BackgroundColor3 = Color3.fromRGB(15, 15, 15)
    EContainer.Visible = false
    EContainer.Active = true
    EContainer.Parent = ScreenGui
    Instance.new("UICorner", EContainer).CornerRadius = UDim.new(0, 25)

    local EStroke = Instance.new("UIStroke", EContainer)
    EStroke.Color = Color3.fromRGB(255, 85, 0)  -- Laranja mais forte
    EStroke.Thickness = 1.2  -- Borda mais fina
    EStroke.Transparency = 0  -- Sem transparência (cor mais forte)
    TS:Create(EStroke, TweenInfoPulse, {Transparency = 0.3}):Play()

    local EBtn = Instance.new("TextButton")
    EBtn.Size = UDim2.new(1, 0, 1, 0)
    EBtn.BackgroundTransparency = 1
    EBtn.Text = "E"
    EBtn.TextColor3 = Color3.fromRGB(255, 85, 0)  -- Laranja mais forte
    EBtn.TextSize = 20
    EBtn.Font = Enum.Font.GothamBold
    EBtn.Parent = EContainer

    task.spawn(function()
        while task.wait(0.2) do
            if NERO ~= nil and NERO.Launch ~= nil then
                EContainer.Visible = NERO.Launch
            end
        end
    end)

    EBtn.MouseButton1Click:Connect(function()
        if LP.Character and LP.Character:FindFirstChild("HumanoidRootPart") then
            local hrp = LP.Character.HumanoidRootPart
            local oldSpin = hrp:FindFirstChild("NERO_Spin")
            if oldSpin then oldSpin:Destroy() end

            local spin = Instance.new("BodyAngularVelocity")
            spin.Name = "NERO_Spin"
            spin.Parent = hrp
            spin.MaxTorque = Vector3.new(math.huge, math.huge, math.huge)
            spin.AngularVelocity = Vector3.new(0, 10000, 0)
            task.wait(0.5)
            if spin then spin:Destroy() end
        end
    end)

    local dragging = false
    local dragInput, dragStart, startPos
    EBtn.InputBegan:Connect(function(input)
        if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then
            dragging = true; dragStart = input.Position; startPos = EContainer.Position
            input.Changed:Connect(function() if input.UserInputState == Enum.UserInputState.End then dragging = false end end)
        end
    end)
    EBtn.InputChanged:Connect(function(input) if input.UserInputType == Enum.UserInputType.MouseMovement or input.UserInputType == Enum.UserInputType.Touch then dragInput = input end end)
    UIS.InputChanged:Connect(function(input)
        if input == dragInput and dragging then
            local delta = input.Position - dragStart
            TS:Create(EContainer, TweenInfo.new(0.1, Enum.EasingStyle.OutQuad), {Position = UDim2.new(startPos.X.Scale, startPos.X.Offset + delta.X, startPos.Y.Scale, startPos.Y.Offset + delta.Y)}):Play()
        end
    end)
end

print("========================================")
print(" 👑 [NERO OMNI PREMIUM v20.0] LOADED! 👑 ")
print("      Ambiente Ativo: " .. GameName)
print("========================================")

local sound = Instance.new("Sound", game.Workspace)
sound.SoundId = "rbxassetid://6042053626"; sound.Volume = 1; sound:Play(); game.Debris:AddItem(sound, 3)

task.spawn(function()
    task.wait(0.5)
    Notify("🔥 NERO FE v20.0 Carregado 🔥")
    DispararConfetes(Gui)
    task.wait(1.5)
    PremiumNotify("OMNI ENGINE ACTIVATED", "Módulo Premium adaptado para: " .. GameName)
end)
-- ============================================================
-- 🔦 LANTERNA NERO - VERSÃO FINAL (CORRETA)
-- ============================================================

task.spawn(function()
    task.wait(0.1)
    
    -- ===== VARIAVEIS =====
    local lightEnabled = false
    local lightBrightness = 5
    local spotLight = nil
    local lightConnection = nil
    
    -- ===== TOGGLE NA POSIÇÃO 0 =====
    local LanternaTog, LanternaKnob, LanternaBtn = createToggle("LANTERNA", tabContainers[4], 0)
    
    -- ===== PAINEL DE POTÊNCIA (FUNDO PRETO) =====
    local powerPanel = Instance.new("Frame")
    powerPanel.Size = UDim2.new(1, -20, 0, 38)
    powerPanel.Position = UDim2.new(0, 10, 0, 48)
    powerPanel.BackgroundColor3 = Color3.fromRGB(0, 0, 0)
    powerPanel.BorderSizePixel = 0
    powerPanel.Parent = tabContainers[4]
    Instance.new("UICorner", powerPanel).CornerRadius = UDim.new(0, 8)
    
    -- BORDA DO PAINEL
    local panelStroke = Instance.new("UIStroke", powerPanel)
    panelStroke.Color = Color3.fromRGB(255, 100, 0)
    panelStroke.Thickness = 1
    panelStroke.Transparency = 0
    
    -- ===== BOTÃO - (FUNDO PRETO, BORDA FINA, TEXTO LARANJA) =====
    local btnMinus = Instance.new("TextButton")
    btnMinus.Size = UDim2.new(0, 32, 0, 26)
    btnMinus.Position = UDim2.new(0, 6, 0.5, -13)
    btnMinus.BackgroundColor3 = Color3.fromRGB(0, 0, 0)
    btnMinus.BorderSizePixel = 0
    btnMinus.Text = "−"
    btnMinus.TextColor3 = Color3.fromRGB(255, 100, 0)
    btnMinus.TextSize = 18
    btnMinus.Font = Enum.Font.GothamSemibold
    btnMinus.AutoButtonColor = false
    btnMinus.Parent = powerPanel
    Instance.new("UICorner", btnMinus).CornerRadius = UDim.new(0, 6)
    
    -- BORDA FINA NO BOTÃO -
    local strokeMinus = Instance.new("UIStroke", btnMinus)
    strokeMinus.Color = Color3.fromRGB(255, 100, 0)
    strokeMinus.Thickness = 0.5
    strokeMinus.Transparency = 0
    
    -- HOVER
    btnMinus.MouseEnter:Connect(function()
        btnMinus.BackgroundColor3 = Color3.fromRGB(20, 20, 20)
    end)
    btnMinus.MouseLeave:Connect(function()
        btnMinus.BackgroundColor3 = Color3.fromRGB(0, 0, 0)
    end)
    
    -- ===== DISPLAY CENTRAL (FUNDO PRETO, BORDA FINA, TEXTO LARANJA) =====
    local displayText = Instance.new("TextLabel")
    displayText.Size = UDim2.new(0, 120, 0, 26)
    displayText.Position = UDim2.new(0.5, -60, 0.5, -13)
    displayText.BackgroundColor3 = Color3.fromRGB(0, 0, 0)
    displayText.BorderSizePixel = 0
    displayText.Text = "Potencia: 5%"
    displayText.TextColor3 = Color3.fromRGB(255, 100, 0)
    displayText.TextSize = 13
    displayText.Font = Enum.Font.GothamMedium
    displayText.Parent = powerPanel
    Instance.new("UICorner", displayText).CornerRadius = UDim.new(0, 6)
    
    -- BORDA FINA NO DISPLAY
    local strokeDisplay = Instance.new("UIStroke", displayText)
    strokeDisplay.Color = Color3.fromRGB(255, 100, 0)
    strokeDisplay.Thickness = 0.5
    strokeDisplay.Transparency = 0
    
    -- ===== BOTÃO + (FUNDO PRETO, BORDA FINA, TEXTO LARANJA) =====
    local btnPlus = Instance.new("TextButton")
    btnPlus.Size = UDim2.new(0, 32, 0, 26)
    btnPlus.Position = UDim2.new(1, -38, 0.5, -13)
    btnPlus.BackgroundColor3 = Color3.fromRGB(0, 0, 0)
    btnPlus.BorderSizePixel = 0
    btnPlus.Text = "+"
    btnPlus.TextColor3 = Color3.fromRGB(255, 100, 0)
    btnPlus.TextSize = 18
    btnPlus.Font = Enum.Font.GothamSemibold
    btnPlus.AutoButtonColor = false
    btnPlus.Parent = powerPanel
    Instance.new("UICorner", btnPlus).CornerRadius = UDim.new(0, 6)
    
    -- BORDA FINA NO BOTÃO +
    local strokePlus = Instance.new("UIStroke", btnPlus)
    strokePlus.Color = Color3.fromRGB(255, 100, 0)
    strokePlus.Thickness = 0.5
    strokePlus.Transparency = 0
    
    -- HOVER
    btnPlus.MouseEnter:Connect(function()
        btnPlus.BackgroundColor3 = Color3.fromRGB(20, 20, 20)
    end)
    btnPlus.MouseLeave:Connect(function()
        btnPlus.BackgroundColor3 = Color3.fromRGB(0, 0, 0)
    end)
    
    -- ===== FUNÇÃO ATUALIZAR DISPLAY =====
    local function updateDisplay()
        displayText.Text = "Potencia: " .. math.round(lightBrightness) .. "%"
        
        if spotLight then
            spotLight.Brightness = lightBrightness / 100 * 150
            spotLight.Range = 3000
        end
    end
    
    -- ===== EVENTOS DOS BOTÕES =====
    btnMinus.MouseButton1Click:Connect(function()
        if lightBrightness > 0 then
            lightBrightness = lightBrightness - 1
            if lightBrightness < 0 then lightBrightness = 0 end
            updateDisplay()
        end
    end)
    
    btnPlus.MouseButton1Click:Connect(function()
        if lightBrightness < 100 then
            lightBrightness = lightBrightness + 1
            if lightBrightness > 100 then lightBrightness = 100 end
            updateDisplay()
        end
    end)
    
    -- ===== FUNÇÃO DA LANTERNA (COM ILUMINAÇÃO FLUIDA) =====
    local function toggleLight()
        local char = LP.Character
        if not char then return end
        
        lightEnabled = not lightEnabled
        
        if lightEnabled then
            local head = char:FindFirstChild("Head") or char:FindFirstChild("HumanoidRootPart")
            if head then
                if spotLight then spotLight:Destroy() end
                
                spotLight = Instance.new("SpotLight")
                spotLight.Name = "NeroLanterna"
                spotLight.Brightness = lightBrightness / 100 * 150
                spotLight.Range = 3000
                spotLight.Angle = 70
                spotLight.Color = Color3.fromRGB(255, 255, 240)
                spotLight.Face = Enum.NormalId.Front
                spotLight.Shadows = false
                spotLight.Parent = head
                
                for _, part in pairs(char:GetChildren()) do
                    if part:IsA("BasePart") then
                        part.Material = Enum.Material.Neon
                    end
                end
                
                -- ATUALIZAÇÃO FLUIDA (SEGUE A CABEÇA SEM TRAVAR)
                if lightConnection then lightConnection:Disconnect() end
                lightConnection = RS.RenderStepped:Connect(function()
                    if spotLight and spotLight.Parent then
                        local newHead = LP.Character and (LP.Character:FindFirstChild("Head") or LP.Character:FindFirstChild("HumanoidRootPart"))
                        if newHead and newHead ~= spotLight.Parent then
                            spotLight.Parent = newHead
                        end
                    end
                end)
            end
        else
            if spotLight then
                spotLight:Destroy()
                spotLight = nil
            end
            if lightConnection then
                lightConnection:Disconnect()
                lightConnection = nil
            end
            
            local char = LP.Character
            if char then
                for _, part in pairs(char:GetChildren()) do
                    if part:IsA("BasePart") then
                        part.Material = Enum.Material.Plastic
                    end
                end
            end
        end
        
        updateToggle(LanternaTog, LanternaKnob, lightEnabled)
    end
    
    -- ===== EVENTO DO TOGGLE =====
    LanternaBtn.MouseButton1Click:Connect(toggleLight)
    
    -- ===== PERSISTÊNCIA NA MORTE =====
    LP.CharacterAdded:Connect(function(chr)
        repeat task.wait() until chr and (chr:FindFirstChild("Head") or chr:FindFirstChild("HumanoidRootPart"))
        task.wait(0.3)
        
        if lightEnabled then
            local head = chr:FindFirstChild("Head") or chr:FindFirstChild("HumanoidRootPart")
            if head then
                if spotLight then spotLight:Destroy() end
                
                spotLight = Instance.new("SpotLight")
                spotLight.Name = "NeroLanterna"
                spotLight.Brightness = lightBrightness / 100 * 150
                spotLight.Range = 3000
                spotLight.Angle = 70
                spotLight.Color = Color3.fromRGB(255, 255, 240)
                spotLight.Face = Enum.NormalId.Front
                spotLight.Shadows = false
                spotLight.Parent = head
                
                for _, part in pairs(chr:GetChildren()) do
                    if part:IsA("BasePart") then
                        part.Material = Enum.Material.Neon
                    end
                end
            end
        end
    end)
    
    -- ===== INICIALIZA =====
    updateDisplay()
    
    print("[LANTERNA] Carregado! | Alcance: 3000 studs | Início: 5%")
end)

 -- ==================== MÓDULO: SONS E ANIMAÇÕES ONE UI + MÚSICA ====================
local isAnimating = false
local currentTweenMain = nil
local currentTweenBtn = nil

-- === MÚSICA DE FUNDO (DOORS) ===
local bgm = Instance.new("Sound")
bgm.SoundId = "rbxassetid://89120203935827"
bgm.Looped = false 
bgm.Volume = 1.0
bgm.Parent = game:GetService("SoundService")

-- === EFEITOS DE NOTIFICAÇÃO (SAMSUNG STYLE) ===
local function PlayUIEffect(isOpen)
    local sfx = Instance.new("Sound")
    sfx.SoundId = "rbxassetid://9113872276" 
    sfx.Pitch = isOpen and 1.1 or 0.85 
    sfx.Volume = 5 
    sfx.Parent = game:GetService("SoundService")
    sfx:Play()
    game.Debris:AddItem(sfx, 3) 
end

-- Função auxiliar para cancelar tweens ativos e evitar travamentos
local function cancelTweens()
    if currentTweenMain then currentTweenMain:Cancel() end
    if currentTweenBtn then currentTweenBtn:Cancel() end
end

-- Lógica de Fechar
CloseBtn.MouseButton1Click:Connect(function()
    cancelTweens()
    isAnimating = true
    
    PlayUIEffect(false)
    bgm:Pause()
    
    Main.Active = false
    currentTweenMain = TS:Create(Main, TweenInfo.new(0.25, Enum.EasingStyle.Quart, Enum.EasingDirection.In), {Size = UDim2.new(0, 0, 0, 0)})
    currentTweenMain:Play()
    
    currentTweenMain.Completed:Connect(function(playbackState)
        if playbackState == Enum.PlaybackState.Completed then
            Main.Visible = false
            NFContainer.Visible = true
            NFContainer.Size = UDim2.new(0, 0, 0, 0)
            
            currentTweenBtn = TS:Create(NFContainer, TweenInfo.new(0.4, Enum.EasingStyle.Back, Enum.EasingDirection.Out), {Size = UDim2.new(0, 50, 0, 50)})
            currentTweenBtn:Play()
            currentTweenBtn.Completed:Connect(function()
                isAnimating = false
            end)
        else
            isAnimating = false
        end
    end)
end)

-- Lógica de Abrir
NFBtn.MouseButton1Click:Connect(function()
    cancelTweens()
    isAnimating = true
    
    PlayUIEffect(true)
    bgm:Resume()
    
    currentTweenBtn = TS:Create(NFContainer, TweenInfo.new(0.15, Enum.EasingStyle.Quart, Enum.EasingDirection.In), {Size = UDim2.new(0, 0, 0, 0)})
    currentTweenBtn:Play()
    
    currentTweenBtn.Completed:Connect(function(playbackState)
        if playbackState == Enum.PlaybackState.Completed then
            NFContainer.Visible = false
            Main.Visible = true
            Main.Size = UDim2.new(0, 0, 0, 0)
            
            currentTweenMain = TS:Create(Main, TweenInfo.new(0.45, Enum.EasingStyle.Back, Enum.EasingDirection.Out), {Size = UDim2.new(0, 360, 0, 300)})
            currentTweenMain:Play()
            currentTweenMain.Completed:Connect(function()
                Main.Active = true
                isAnimating = false
            end)
        else
            isAnimating = false
        end
    end)
end)

-- Atalho Tecla INSERT
UIS.InputBegan:Connect(function(input, gpe)
    if gpe then return end
    if input.KeyCode == Enum.KeyCode.Insert and not isAnimating then
        if Main.Visible then
            for _, conn in pairs(getconnections(CloseBtn.MouseButton1Click)) do conn:Fire() end
        elseif NFContainer.Visible then
            for _, conn in pairs(getconnections(NFBtn.MouseButton1Click)) do conn:Fire() end
        end
    end
end)

-- ==========================================================
-- 🎵 NERO MUSIC PLAYER v7 – EXACT HTML/CSS REPLICA
-- ==========================================================
task.spawn(function()
    local musicList = {
        "94935794334796",
        "106077539420914",
        "95401969908951",
        "78317236576153",
        "76897780069788",
        "1784221294137",
        "110817176848617",
        "139719375902695",
        "136974179670066",
        "139815305627554",
        "140667339171815",
        "140580823167015",
        "140504265985079",
        "9047104571",
        "9047105584",
        "9039770426",
        "9047106878",
        "9042666614",
        "9046862941",
        "9046865270",
        "9046863235",
        "9046864509",
        "14145626111",
        "7148815128"    
    }

    local TweenService = game:GetService("TweenService")
    local RunService = game:GetService("RunService")
    local UserInputService = game:GetService("UserInputService")

    local MusicGui = Instance.new("ScreenGui")
    MusicGui.Name = "NERO_MusicPlayer"
    MusicGui.ResetOnSpawn = false
    MusicGui.Parent = targetParent or game:GetService("CoreGui")
    MusicGui.ZIndexBehavior = Enum.ZIndexBehavior.Sibling
    MusicGui.Enabled = false

    -- Player Card (150x110)
    local Player = Instance.new("Frame")
    Player.Size = UDim2.new(0, 150, 0, 110)
    Player.Position = UDim2.new(0.5, -75, 0.5, -55)
    Player.BackgroundColor3 = Color3.fromRGB(0, 0, 0)
    Player.BorderSizePixel = 0
    Player.ClipsDescendants = false
    Player.Parent = MusicGui

    local playerCorner = Instance.new("UICorner", Player)
    playerCorner.CornerRadius = UDim.new(0, 12)

    local playerStroke = Instance.new("UIStroke", Player)
    playerStroke.Color = Color3.fromRGB(255, 100, 0)
    playerStroke.Thickness = 1
    playerStroke.Transparency = 0

    -- Botão Superior (◆)
    local btnTop = Instance.new("TextButton")
    btnTop.Size = UDim2.new(0, 16, 0, 16)
    btnTop.Position = UDim2.new(1, -20, 0, 4)
    btnTop.BackgroundTransparency = 1
    btnTop.Text = "◆"
    btnTop.TextColor3 = Color3.fromRGB(255, 100, 0)
    btnTop.Font = Enum.Font.GothamBold
    btnTop.TextSize = 13
    btnTop.Parent = Player

    -- Header Section (Capa + Informações)
    local headerSection = Instance.new("Frame")
    headerSection.Size = UDim2.new(1, -20, 0, 44)
    headerSection.Position = UDim2.new(0, 10, 0, 10)
    headerSection.BackgroundTransparency = 1
    headerSection.Parent = Player

    -- Moldura da Capa (Cover Box)
    local coverBox = Instance.new("Frame")
    coverBox.Size = UDim2.new(0, 44, 0, 44)
    coverBox.Position = UDim2.new(0, 0, 0, 0)
    coverBox.BackgroundTransparency = 1
    coverBox.Parent = headerSection

    local coverCorner = Instance.new("UICorner", coverBox)
    coverCorner.CornerRadius = UDim.new(0, 12)

    local coverStroke = Instance.new("UIStroke", coverBox)
    coverStroke.Color = Color3.fromRGB(255, 100, 0)
    coverStroke.Thickness = 1

    -- Anel Giratório em CSS
    local anelGiratorio = Instance.new("Frame")
    anelGiratorio.Size = UDim2.new(0, 24, 0, 24)
    anelGiratorio.Position = UDim2.new(0.5, -12, 0.5, -12)
    anelGiratorio.BackgroundTransparency = 1
    anelGiratorio.Parent = coverBox

    local anelCorner = Instance.new("UICorner", anelGiratorio)
    anelCorner.CornerRadius = UDim.new(1, 0)

    local anelStroke = Instance.new("UIStroke", anelGiratorio)
    anelStroke.Thickness = 3
    anelStroke.ApplyStrokeMode = Enum.ApplyStrokeMode.Border
    anelStroke.Color = Color3.fromRGB(255, 255, 255)

    local ringGradient = Instance.new("UIGradient", anelStroke)
    ringGradient.Color = ColorSequence.new({
        ColorSequenceKeypoint.new(0.00, Color3.fromRGB(179, 18, 0)),
        ColorSequenceKeypoint.new(0.25, Color3.fromRGB(255, 51, 0)),
        ColorSequenceKeypoint.new(0.50, Color3.fromRGB(255, 153, 0)),
        ColorSequenceKeypoint.new(0.61, Color3.fromRGB(255, 199, 102)),
        ColorSequenceKeypoint.new(0.80, Color3.fromRGB(255, 51, 0)),
        ColorSequenceKeypoint.new(1.00, Color3.fromRGB(179, 18, 0))
    })

    -- Animação de Giro do Anel (1.4s)
    local spinInfo = TweenInfo.new(1.4, Enum.EasingStyle.Linear, Enum.EasingDirection.InOut, -1)
    local spinTween = TweenService:Create(ringGradient, spinInfo, {Rotation = 360})
    spinTween:Play()

    -- Detalhes da Faixa
    local trackDetails = Instance.new("Frame")
    trackDetails.Size = UDim2.new(1, -54, 1, 0)
    trackDetails.Position = UDim2.new(0, 54, 0, 0)
    trackDetails.BackgroundTransparency = 1
    trackDetails.Parent = headerSection

    local trackTitle = Instance.new("TextLabel")
    trackTitle.Size = UDim2.new(1, 0, 0, 14)
    trackTitle.Position = UDim2.new(0, 0, 0, 8)
    trackTitle.BackgroundTransparency = 1
    trackTitle.Text = "♧NERO MUSIC♡"
    trackTitle.TextColor3 = Color3.fromRGB(255, 255, 255)
    trackTitle.Font = Enum.Font.GothamBold
    trackTitle.TextSize = 11
    trackTitle.TextXAlignment = Enum.TextXAlignment.Left
    trackTitle.TextTruncate = Enum.TextTruncate.AtEnd
    trackTitle.Parent = trackDetails

    local trackArtist = Instance.new("TextLabel")
    trackArtist.Size = UDim2.new(1, 0, 0, 12)
    trackArtist.Position = UDim2.new(0, 0, 0, 24)
    trackArtist.BackgroundTransparency = 1
    trackArtist.Text = "NEO NERO"
    trackArtist.TextColor3 = Color3.fromRGB(160, 160, 170)
    trackArtist.Font = Enum.Font.GothamMedium
    trackArtist.TextSize = 8
    trackArtist.TextXAlignment = Enum.TextXAlignment.Left
    trackArtist.TextTruncate = Enum.TextTruncate.AtEnd
    trackArtist.Parent = trackDetails

    -- Área da Barra de Progresso
    local progressContainer = Instance.new("Frame")
    progressContainer.Size = UDim2.new(0, 130, 0, 16)
    progressContainer.Position = UDim2.new(0, 10, 0, 59)
    progressContainer.BackgroundTransparency = 1
    progressContainer.Parent = Player

    local progressTrack = Instance.new("Frame")
    progressTrack.Size = UDim2.new(1, 0, 0, 2)
    progressTrack.Position = UDim2.new(0, 0, 0, 0)
    progressTrack.BackgroundColor3 = Color3.fromRGB(26, 26, 26)
    progressTrack.BorderSizePixel = 0
    progressTrack.Parent = progressContainer

    local trackCorner = Instance.new("UICorner", progressTrack)
    trackCorner.CornerRadius = UDim.new(0, 12)

    local progressBar = Instance.new("Frame")
    progressBar.Size = UDim2.new(0, 0, 1, 0)
    progressBar.BackgroundColor3 = Color3.fromRGB(255, 100, 0)
    progressBar.BorderSizePixel = 0
    progressBar.Parent = progressTrack

    local barCorner = Instance.new("UICorner", progressBar)
    barCorner.CornerRadius = UDim.new(0, 12)

    -- Display de Tempo (1:15 / 2:45)
    local timeDisplay = Instance.new("Frame")
    timeDisplay.Size = UDim2.new(1, 0, 0, 10)
    timeDisplay.Position = UDim2.new(0, 0, 0, 5)
    timeDisplay.BackgroundTransparency = 1
    timeDisplay.Parent = progressContainer

    local timeCurrent = Instance.new("TextLabel")
    timeCurrent.Size = UDim2.new(0.5, 0, 1, 0)
    timeCurrent.Position = UDim2.new(0, 0, 0, 0)
    timeCurrent.BackgroundTransparency = 1
    timeCurrent.Text = "0:00"
    timeCurrent.TextColor3 = Color3.fromRGB(160, 160, 170)
    timeCurrent.Font = Enum.Font.GothamBold
    timeCurrent.TextSize = 7
    timeCurrent.TextXAlignment = Enum.TextXAlignment.Left
    timeCurrent.Parent = timeDisplay

    local timeTotal = Instance.new("TextLabel")
    timeTotal.Size = UDim2.new(0.5, 0, 1, 0)
    timeTotal.Position = UDim2.new(0.5, 0, 0, 0)
    timeTotal.BackgroundTransparency = 1
    timeTotal.Text = "0:00"
    timeTotal.TextColor3 = Color3.fromRGB(160, 160, 170)
    timeTotal.Font = Enum.Font.GothamBold
    timeTotal.TextSize = 7
    timeTotal.TextXAlignment = Enum.TextXAlignment.Right
    timeTotal.Parent = timeDisplay

    -- Row de Controles
    local controlsRow = Instance.new("Frame")
    controlsRow.Size = UDim2.new(0, 130, 0, 18)
    controlsRow.Position = UDim2.new(0, 10, 0, 82)
    controlsRow.BackgroundTransparency = 1
    controlsRow.Parent = Player

    local prevBtn = Instance.new("TextButton")
    prevBtn.Size = UDim2.new(0, 20, 1, 0)
    prevBtn.Position = UDim2.new(0, 6, 0, 0)
    prevBtn.BackgroundTransparency = 1
    prevBtn.Text = "《《"
    prevBtn.TextColor3 = Color3.fromRGB(255, 100, 0)
    prevBtn.Font = Enum.Font.GothamBold
    prevBtn.TextSize = 11
    prevBtn.Parent = controlsRow

    local playBtn = Instance.new("TextButton")
    playBtn.Size = UDim2.new(0, 20, 1, 0)
    playBtn.Position = UDim2.new(0.5, -10, 0, 0)
    playBtn.BackgroundTransparency = 1
    playBtn.Text = "▶"
    playBtn.TextColor3 = Color3.fromRGB(255, 100, 0)
    playBtn.Font = Enum.Font.GothamBold
    playBtn.TextSize = 13
    playBtn.Parent = controlsRow

    local nextBtn = Instance.new("TextButton")
    nextBtn.Size = UDim2.new(0, 20, 1, 0)
    nextBtn.Position = UDim2.new(1, -26, 0, 0)
    nextBtn.BackgroundTransparency = 1
    nextBtn.Text = "》》"
    nextBtn.TextColor3 = Color3.fromRGB(255, 100, 0)
    nextBtn.Font = Enum.Font.GothamBold
    nextBtn.TextSize = 11
    nextBtn.Parent = controlsRow

    -- ==========================================================
    -- PAINEL DE VOLUME FLUTUANTE (EXATO HTML)
    -- ==========================================================
    local volumePanel = Instance.new("CanvasGroup")
    volumePanel.Size = UDim2.new(0, 44, 0, 110)
    volumePanel.Position = UDim2.new(1, 8, 0, 0)
    volumePanel.BackgroundColor3 = Color3.fromRGB(0, 0, 0)
    volumePanel.BackgroundTransparency = 0
    volumePanel.BorderSizePixel = 0
    volumePanel.GroupTransparency = 1
    volumePanel.Visible = false
    volumePanel.Parent = Player

    local volCorner = Instance.new("UICorner", volumePanel)
    volCorner.CornerRadius = UDim.new(0, 12)

    local volStroke = Instance.new("UIStroke", volumePanel)
    volStroke.Color = Color3.fromRGB(255, 100, 0)
    volStroke.Thickness = 1

    local volSliderTrack = Instance.new("TextButton")
    volSliderTrack.Size = UDim2.new(0, 4, 0, 66)
    volSliderTrack.Position = UDim2.new(0.5, -2, 0, 14)
    volSliderTrack.BackgroundColor3 = Color3.fromRGB(26, 26, 26)
    volSliderTrack.BorderSizePixel = 0
    volSliderTrack.Text = ""
    volSliderTrack.Parent = volumePanel

    local volTrackCorner = Instance.new("UICorner", volSliderTrack)
    volTrackCorner.CornerRadius = UDim.new(0, 12)

    local volSliderFill = Instance.new("Frame")
    volSliderFill.Size = UDim2.new(1, 0, 0.7, 0)
    volSliderFill.Position = UDim2.new(0, 0, 0.3, 0)
    volSliderFill.BackgroundColor3 = Color3.fromRGB(255, 100, 0)
    volSliderFill.BorderSizePixel = 0
    volSliderFill.Parent = volSliderTrack

    local volFillCorner = Instance.new("UICorner", volSliderFill)
    volFillCorner.CornerRadius = UDim.new(0, 12)

    local volPercentage = Instance.new("TextLabel")
    volPercentage.Size = UDim2.new(1, 0, 0, 12)
    volPercentage.Position = UDim2.new(0, 0, 1, -18)
    volPercentage.BackgroundTransparency = 1
    volPercentage.Text = "70%"
    volPercentage.TextColor3 = Color3.fromRGB(160, 160, 170)
    volPercentage.Font = Enum.Font.GothamBold
    volPercentage.TextSize = 9
    volPercentage.Parent = volumePanel

    -- Lógica do Fade de Volume
    local hideTimer = nil
    local currentFadeTween = nil

    local function showVolume()
        if hideTimer then task.cancel(hideTimer) end
        if currentFadeTween then currentFadeTween:Cancel() end
        
        volumePanel.Visible = true
        currentFadeTween = TweenService:Create(volumePanel, TweenInfo.new(0.3, Enum.EasingStyle.Sine, Enum.EasingDirection.Out), {
            GroupTransparency = 0
        })
        currentFadeTween:Play()

        hideTimer = task.delay(1.8, function()
            if currentFadeTween then currentFadeTween:Cancel() end
            currentFadeTween = TweenService:Create(volumePanel, TweenInfo.new(0.3, Enum.EasingStyle.Sine, Enum.EasingDirection.In), {
                GroupTransparency = 1
            })
            currentFadeTween:Play()
            task.wait(0.3)
            if volumePanel.GroupTransparency >= 0.9 then
                volumePanel.Visible = false
            end
        end)
    end

    local isDraggingVol = false
    volSliderTrack.InputBegan:Connect(function(input)
        if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then
            isDraggingVol = true
            showVolume()
        end
    end)

    UserInputService.InputEnded:Connect(function(input)
        if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then
            isDraggingVol = false
        end
    end)

    local globalVolume = 1.0 * 0.7
    UserInputService.InputChanged:Connect(function(input)
        if isDraggingVol and (input.UserInputType == Enum.UserInputType.MouseMovement or input.UserInputType == Enum.UserInputType.Touch) then
            local relativeY = math.clamp(input.Position.Y - volSliderTrack.AbsolutePosition.Y, 0, volSliderTrack.AbsoluteSize.Y)
            local percent = 1 - (relativeY / volSliderTrack.AbsoluteSize.Y)
            volSliderFill.Size = UDim2.new(1, 0, percent, 0)
            volSliderFill.Position = UDim2.new(0, 0, 1 - percent, 0)
            volPercentage.Text = math.floor(percent * 100) .. "%"
            globalVolume = percent * 1.5
            showVolume()
        end
    end)

    Player.MouseEnter:Connect(showVolume)
    Player.MouseMoved:Connect(showVolume)

    -- ==========================================================
    -- LÓGICA DO PLAYER & CONTROLE DE TEMPO
    -- ==========================================================
    local currentSound = nil
    local currentIndex = 1
    local isPlaying = false
    local progressConnection = nil

    local function formatTime(seconds)
        if not seconds or seconds ~= seconds or seconds <= 0 then return "0:00" end
        local mins = math.floor(seconds / 60)
        local secs = math.floor(seconds % 60)
        return string.format("%d:%02d", mins, secs)
    end

    local function UpdateProgress()
        if currentSound and currentSound.IsLoaded and currentSound.TimeLength > 0 then
            local progressPercent = math.clamp(currentSound.TimePosition / currentSound.TimeLength, 0, 1)
            progressBar.Size = UDim2.new(progressPercent, 0, 1, 0)
            timeCurrent.Text = formatTime(currentSound.TimePosition)
            timeTotal.Text = formatTime(currentSound.TimeLength)
        else
            progressBar.Size = UDim2.new(0, 0, 1, 0)
            timeCurrent.Text = "0:00"
            timeTotal.Text = "0:00"
        end
        if currentSound then currentSound.Volume = globalVolume end
    end

    local function StartProgressUpdate()
        if progressConnection then progressConnection:Disconnect() end
        progressConnection = RunService.Heartbeat:Connect(function()
            if isPlaying and currentSound and currentSound.IsPlaying then
                UpdateProgress()
            end
        end)
    end

    local function StopProgressUpdate()
        if progressConnection then
            progressConnection:Disconnect()
            progressConnection = nil
        end
    end

    local function PlaySong(index)
        if currentSound then 
            currentSound:Stop() 
            currentSound:Destroy() 
        end
        if #musicList == 0 then return end
        if index < 1 then index = #musicList end
        if index > #musicList then index = 1 end
        currentIndex = index

        currentSound = Instance.new("Sound")
        currentSound.SoundId = "rbxassetid://" .. musicList[currentIndex]
        currentSound.Volume = globalVolume
        currentSound.Looped = false
        currentSound.Parent = game:GetService("SoundService")
        currentSound:Play()
        isPlaying = true

        currentSound.Loaded:Connect(function()
            UpdateProgress()
        end)

        currentSound.Ended:Connect(function()
            progressBar.Size = UDim2.new(0, 0, 1, 0)
            PlaySong(currentIndex + 1)
        end)

        StartProgressUpdate()
    end

    local function TogglePlay()
        if #musicList == 0 then return end
        if isPlaying then
            if currentSound then currentSound:Pause() end
            isPlaying = false
            StopProgressUpdate()
        else
            if currentSound then 
                currentSound:Resume()
                isPlaying = true
                StartProgressUpdate()
            else
                PlaySong(currentIndex)
            end
        end
    end

    playBtn.MouseButton1Click:Connect(TogglePlay)
    nextBtn.MouseButton1Click:Connect(function()
        if #musicList > 0 then 
            progressBar.Size = UDim2.new(0, 0, 1, 0)
            PlaySong(currentIndex + 1) 
        end
    end)
    prevBtn.MouseButton1Click:Connect(function()
        if #musicList > 0 then 
            progressBar.Size = UDim2.new(0, 0, 1, 0)
            PlaySong(currentIndex - 1) 
        end
    end)

    btnTop.MouseButton1Click:Connect(function()
        MusicGui.Enabled = false
        if Main and not Main.Visible and NFContainer then NFContainer.Visible = true end
    end)

    local function OpenMusicPlayer()
        MusicGui.Enabled = true
        if Main then Main.Visible = false end
        if NFContainer then NFContainer.Visible = false end
        if #musicList > 0 and not currentSound then
            currentIndex = math.random(1, #musicList)
            PlaySong(currentIndex)
        end
    end

    local NFBtn = NFContainer and NFContainer:FindFirstChild("TextButton")
    if NFBtn then
        local holdTimer = nil
        local isHeld = false

        NFBtn.InputBegan:Connect(function(input)
            if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then
                isHeld = true
                holdTimer = task.delay(3, function()
                    if isHeld and not MusicGui.Enabled then
                        OpenMusicPlayer()
                    end
                end)
            end
        end)

        NFBtn.InputEnded:Connect(function(input)
            if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then
                isHeld = false
                if holdTimer then task.cancel(holdTimer); holdTimer = nil end
            end
        end)
    end

    -- Sistema de Arraste da Janela (Drag)
    local dragging = false
    local dragInput, dragStart, startPos

    Player.InputBegan:Connect(function(input)
        if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then
            showVolume()
            dragging = true
            dragStart = input.Position
            startPos = Player.Position
            input.Changed:Connect(function()
                if input.UserInputState == Enum.UserInputState.End then
                    dragging = false
                end
            end)
        end
    end)

    Player.InputChanged:Connect(function(input)
        if input.UserInputType == Enum.UserInputType.MouseMovement or input.UserInputType == Enum.UserInputType.Touch then
            dragInput = input
        end
    end)

    UserInputService.InputChanged:Connect(function(input)
        if input == dragInput and dragging then
            local delta = input.Position - dragStart
            Player.Position = UDim2.new(
                startPos.X.Scale,
                startPos.X.Offset + delta.X,
                startPos.Y.Scale,
                startPos.Y.Offset + delta.Y
            )
        end
    end)

    print("🎵 [NERO] Music Player v7 sincronizado com o visual HTML/CSS com sucesso!")
end)



    
-- [[ NERO HUB: Módulo de Câmera (Mobile / Bypass) - V3.1 ]] --
task.spawn(function()
    if not game:IsLoaded() then game.Loaded:Wait() end

    local Players = game:GetService("Players")
    local RunService = game:GetService("RunService")
    local TweenService = game:GetService("TweenService")
    local CoreGui = game:GetService("CoreGui")
    local UserInputService = game:GetService("UserInputService")

    local player = Players.LocalPlayer
    local camera = workspace.CurrentCamera

    local neroBlack = Color3.fromRGB(10, 10, 10)
    local neroOrange = Color3.fromRGB(255, 100, 0)

    -- Interface Segura
    local neroGui = Instance.new("ScreenGui")
    neroGui.Name = "NeroCameraModuleV3"
    neroGui.ResetOnSpawn = false
    neroGui.IgnoreGuiInset = true
    neroGui.ZIndexBehavior = Enum.ZIndexBehavior.Global

    local function safeParent(gui)
        if gethui then
            local success, res = pcall(gethui)
            if success and res then gui.Parent = res; return end
        end
        local success = pcall(function() gui.Parent = CoreGui end)
        if success then return end
        pcall(function() gui.Parent = player:WaitForChild("PlayerGui") end)
    end
    safeParent(neroGui)

    -- BOTÃO INVISÍVEL (Hitbox gigante para celular: 40x40)
    local hitbox = Instance.new("TextButton")
    hitbox.Name = "TouchArea"
    hitbox.Size = UDim2.new(0, 40, 0, 40)
    hitbox.Position = UDim2.new(0.5, -20, 0, 0)
    hitbox.BackgroundTransparency = 1
    hitbox.Text = ""
    hitbox.Parent = neroGui

    -- GRUPO VISUAL (agora parentado direto no ScreenGui)
    local visualGroup = Instance.new("CanvasGroup")
    visualGroup.Size = UDim2.new(0, 20, 0, 20)
    visualGroup.Position = UDim2.new(0.5, 0, 0, 10)
    visualGroup.AnchorPoint = Vector2.new(0.5, 0.5)
    visualGroup.BackgroundColor3 = neroBlack
    visualGroup.GroupTransparency = 0.9
    visualGroup.Parent = neroGui

    local corner = Instance.new("UICorner")
    corner.CornerRadius = UDim.new(1, 0)
    corner.Parent = visualGroup

    local txt = Instance.new("TextLabel")
    txt.Size = UDim2.new(1, 0, 1, 0)
    txt.BackgroundTransparency = 1
    txt.Text = "¤"
    txt.TextColor3 = neroOrange
    txt.TextSize = 14
    txt.Font = Enum.Font.Code
    txt.Parent = visualGroup

    local stroke = Instance.new("UIStroke")
    stroke.Color = neroOrange
    stroke.Thickness = 1.5
    stroke.Parent = visualGroup

    -- Pulsação infinita da borda
    local pulseInfo = TweenInfo.new(1, Enum.EasingStyle.Sine, Enum.EasingDirection.InOut, -1, true)
    TweenService:Create(stroke, pulseInfo, {Thickness = 2.5, Transparency = 0.4}):Play()

    -- MIRA CENTRAL (Losango com ponto "¤")
    local crosshair = Instance.new("TextLabel")
    crosshair.Name = "Crosshair"
    crosshair.Size = UDim2.new(0, 30, 0, 30)
    crosshair.Position = UDim2.new(0.5, 0, 0.5, 0)
    crosshair.AnchorPoint = Vector2.new(0.5, 0.5)
    crosshair.BackgroundTransparency = 1
    crosshair.Visible = false
    crosshair.ZIndex = 900
    crosshair.Text = "¤"
    crosshair.TextColor3 = neroOrange
    crosshair.TextSize = 26
    crosshair.Font = Enum.Font.Code
    crosshair.TextStrokeTransparency = 0.3
    crosshair.Parent = neroGui

    -- Lógica de Animação (10% -> 100% por 2s -> 10%)
    local lastInteraction = 0
    local animFast = TweenInfo.new(0.15, Enum.EasingStyle.Quad, Enum.EasingDirection.Out)
    local animSlow = TweenInfo.new(0.5, Enum.EasingStyle.Quad, Enum.EasingDirection.Out)

    local function triggerActiveAnim()
        lastInteraction = tick()
        TweenService:Create(visualGroup, animFast, {GroupTransparency = 0}):Play()
        
        task.delay(2.1, function()
            if tick() - lastInteraction >= 2.0 then
                TweenService:Create(visualGroup, animSlow, {GroupTransparency = 0.9}):Play()
            end
        end)
    end

    -- Sistema de Câmera (ORIGINAL - Como era antes)
    local cameraFixed = false
    local thirdPersonBypass = false
    local crosshairActive = false

    local function setThirdPersonBypass(state)
        thirdPersonBypass = state
        if state then
            RunService:BindToRenderStep("NeroCamBypass", Enum.RenderPriority.Camera.Value + 1, function()
                local char = player.Character
                if char and char:FindFirstChild("HumanoidRootPart") then
                    local hrp = char.HumanoidRootPart
                    camera.CFrame = CFrame.new(hrp.Position) * camera.CFrame.Rotation * CFrame.new(0, 2, 10)
                end
            end)
        else
            RunService:UnbindFromRenderStep("NeroCamBypass")
        end
    end

    local function setFixedCamera(state)
        cameraFixed = state
        if state then
            RunService:BindToRenderStep("NeroCamFixed", Enum.RenderPriority.Character.Value, function()
                local char = player.Character
                if char and char:FindFirstChild("HumanoidRootPart") then
                    local hrp = char.HumanoidRootPart
                    hrp.CFrame = CFrame.new(hrp.Position, hrp.Position + camera.CFrame.LookVector * Vector3.new(1,0,1))
                end
            end)
        else
            RunService:UnbindFromRenderStep("NeroCamFixed")
        end
    end

    local function setCrosshair(state)
        crosshairActive = state
        crosshair.Visible = state
        
        if state then
            if UserInputService.TouchEnabled then
                pcall(function()
                    UserInputService:SetHapticFeedback(Enum.HapticFeedbackType.Light)
                end)
            end
            crosshair.Size = UDim2.new(0, 0, 0, 0)
            TweenService:Create(crosshair, TweenInfo.new(0.2, Enum.EasingStyle.Back, Enum.EasingDirection.Out), 
                {Size = UDim2.new(0, 30, 0, 30)}):Play()
        end
    end

    -- Lógica de Toques (1 Toque vs 2 Toques vs 3 Toques)
    local tapCount = 0
    local doubleTapTime = 0.35

    hitbox.Activated:Connect(function()
        triggerActiveAnim()
        tapCount = tapCount + 1
        
        if tapCount == 1 then
            task.delay(doubleTapTime, function()
                if tapCount == 1 then
                    tapCount = 0
                    setFixedCamera(not cameraFixed)
                elseif tapCount == 2 then
                    tapCount = 0
                    setThirdPersonBypass(not thirdPersonBypass)
                    if not thirdPersonBypass then
                        setFixedCamera(cameraFixed)
                    end
                elseif tapCount >= 3 then
                    tapCount = 0
                    setCrosshair(not crosshairActive)
                end
            end)
        elseif tapCount >= 3 then
            tapCount = 0
            setCrosshair(not crosshairActive)
        end
    end)
end) 
-- ==========================================================
-- FLING SUPREMO (VERSÃO CORRIGIDA - FUNCIONA 100%)
-- ==========================================================
task.spawn(function()
    task.wait(3) -- Espera o NERO carregar completamente
    
    -- CRIA O TOGGLE NA ABA 8 (CAOS)
    local FsTog, FsKnob, FsBtn = createToggle("🔥 FLING SUPREMO", tabContainers[2], 336)
    
    -- ===== LÓGICA ORIGINAL DO TOUCH FLING (INTACTA) =====
    local hiddenfling = false
    local flingThread = nil
    
    local function fling()
        local lp = game.Players.LocalPlayer
        local c, hrp, vel, movel = nil, nil, nil, 0.1
        
        while hiddenfling do
            game:GetService("RunService").Heartbeat:Wait()
            c = lp.Character
            hrp = c and c:FindFirstChild("HumanoidRootPart")
            
            if hrp then
                vel = hrp.Velocity
                hrp.Velocity = vel * 10000 + Vector3.new(0, 10000, 0)
                game:GetService("RunService").RenderStepped:Wait()
                hrp.Velocity = vel
                game:GetService("RunService").Stepped:Wait()
                hrp.Velocity = vel + Vector3.new(0, movel, 0)
                movel = -movel
            end
        end
    end
    
    -- TOGGLE DO NERO
    FsBtn.MouseButton1Click:Connect(function()
        hiddenfling = not hiddenfling
        updateToggle(FsTog, FsKnob, hiddenfling)
        
        if hiddenfling then
            print("FLING ATIVADO!")  -- ← MANTIVE O PRINT
            flingThread = coroutine.create(fling)
            coroutine.resume(flingThread)
        else
            hiddenfling = false
            print("FLING DESATIVADO!")
        end
    end)
end)
-- ==========================================================
-- TOGGLE: INVISIBILIDADE TOTAL (SISTEMA COMPLETO)
-- ==========================================================
task.spawn(function()
    task.wait(3)

    -- CRIA O TOGGLE NA ABA 8 (CAOS)
    local IvTog, IvKnob, IvBtn = createToggle("👻 INVISIBILIDADE TOTAL", tabContainers[2], 384)

    -- ===== CÓDIGO ORIGINAL (INTACTO) =====
    local Player = game:GetService('Players').LocalPlayer
    local Char = Player.Character or Player.CharacterAdded:Wait()
    local UIS = game:GetService('UserInputService')
    local RunService = game:GetService('RunService')
    local Camera = workspace.CurrentCamera

    local isActive = false
    local prison = nil
    local fakeHitbox = nil
    local originalPosition = nil
    local movementConnection = nil

    local moveDirection = Vector3.new(0, 0, 0)
    local jumpPressed = false

    -- ===== CONTROLES VIRTUAIS (mantido intacto) =====
    local ControlsGui = Instance.new("ScreenGui")
    ControlsGui.Name = "NeroInvisControls"
    ControlsGui.Parent = game:GetService("CoreGui")
    ControlsGui.ResetOnSpawn = false
    ControlsGui.ZIndexBehavior = Enum.ZIndexBehavior.Sibling
    ControlsGui.Enabled = false

    local joystickArea = Instance.new("Frame")
    joystickArea.Size = UDim2.new(0.5, 0, 1, 0)
    joystickArea.Position = UDim2.new(0, 0, 0, 0)
    joystickArea.BackgroundTransparency = 1
    joystickArea.BorderSizePixel = 0
    joystickArea.ZIndex = 1
    joystickArea.Parent = ControlsGui

    local jumpButton = Instance.new("ImageButton")
    jumpButton.Size = UDim2.new(0, 100, 0, 100)
    jumpButton.Position = UDim2.new(0.85, 0, 0.85, 0)
    jumpButton.BackgroundColor3 = Color3.new(1, 1, 1)
    jumpButton.BackgroundTransparency = 1
    jumpButton.BorderSizePixel = 0
    jumpButton.Image = "rbxassetid://0"
    jumpButton.AutoButtonColor = false
    jumpButton.ZIndex = 2
    jumpButton.Parent = ControlsGui

    local joystickActive = false
    local joystickCenter = Vector2.new(0, 0)
    local maxJoystickRadius = 60

    joystickArea.InputBegan:Connect(function(input)
        if input.UserInputType == Enum.UserInputType.Touch or input.UserInputType == Enum.UserInputType.MouseButton1 then
            joystickActive = true
            joystickCenter = Vector2.new(input.Position.X, input.Position.Y)
            moveDirection = Vector3.new(0, 0, 0)
        end
    end)

    joystickArea.InputChanged:Connect(function(input)
        if joystickActive and (input.UserInputType == Enum.UserInputType.Touch or input.UserInputType == Enum.UserInputType.MouseMovement) then
            local currentPos = Vector2.new(input.Position.X, input.Position.Y)
            local offset = currentPos - joystickCenter
            if offset.Magnitude > maxJoystickRadius then
                offset = offset.Unit * maxJoystickRadius
            end
            moveDirection = Vector3.new(offset.X / maxJoystickRadius, 0, -offset.Y / maxJoystickRadius)
        end
    end)

    joystickArea.InputEnded:Connect(function(input)
        if input.UserInputType == Enum.UserInputType.Touch or input.UserInputType == Enum.UserInputType.MouseButton1 then
            joystickActive = false
            moveDirection = Vector3.new(0, 0, 0)
        end
    end)

    jumpButton.InputBegan:Connect(function(input)
        if input.UserInputType == Enum.UserInputType.Touch or input.UserInputType == Enum.UserInputType.MouseButton1 then
            jumpPressed = true
        end
    end)

    jumpButton.InputEnded:Connect(function(input)
        if input.UserInputType == Enum.UserInputType.Touch or input.UserInputType == Enum.UserInputType.MouseButton1 then
            jumpPressed = false
        end
    end)

    -- ===== FUNÇÕES DA PRISÃO E HITBOX (INTACTAS) =====
    local function createPrison()
        if prison then prison:Destroy() end
        
        prison = Instance.new("Model")
        prison.Name = "Prison"
        
        local prisonSize = Vector3.new(8, 8, 8)
        local prisonPosition = Vector3.new(0, 10000, 0)
        
        local walls = {
            {name = "Floor", size = Vector3.new(prisonSize.X, 1, prisonSize.Z), pos = Vector3.new(0, -prisonSize.Y/2, 0), color = Color3.fromRGB(150, 150, 150)},
            {name = "Ceiling", size = Vector3.new(prisonSize.X, 1, prisonSize.Z), pos = Vector3.new(0, prisonSize.Y/2, 0), color = Color3.fromRGB(150, 150, 150)},
            {name = "Wall1", size = Vector3.new(prisonSize.X, prisonSize.Y, 1), pos = Vector3.new(0, 0, -prisonSize.Z/2), color = Color3.fromRGB(120, 120, 120)},
            {name = "Wall2", size = Vector3.new(prisonSize.X, prisonSize.Y, 1), pos = Vector3.new(0, 0, prisonSize.Z/2), color = Color3.fromRGB(120, 120, 120)},
            {name = "Wall3", size = Vector3.new(1, prisonSize.Y, prisonSize.Z), pos = Vector3.new(-prisonSize.X/2, 0, 0), color = Color3.fromRGB(120, 120, 120)},
            {name = "Wall4", size = Vector3.new(1, prisonSize.Y, prisonSize.Z), pos = Vector3.new(prisonSize.X/2, 0, 0), color = Color3.fromRGB(120, 120, 120)},
        }
        
        for _, wallData in ipairs(walls) do
            local wall = Instance.new("Part")
            wall.Name = wallData.name
            wall.Anchored = true
            wall.CanCollide = true
            wall.Size = wallData.size
            wall.Position = prisonPosition + wallData.pos
            wall.Color = wallData.color
            wall.Material = Enum.Material.Metal
            wall.Parent = prison
        end
        
        prison.Parent = workspace
        return prison
    end

    local function createFakeHitbox()
        if fakeHitbox then fakeHitbox:Destroy() end
        
        fakeHitbox = Instance.new("Part")
        fakeHitbox.Name = "FakeHitbox"
        fakeHitbox.Anchored = true
        fakeHitbox.CanCollide = false
        fakeHitbox.Size = Vector3.new(4, 5, 4)
        fakeHitbox.Transparency = 1
        fakeHitbox.LocalTransparencyModifier = 0.5
        fakeHitbox.Color = Color3.fromRGB(100, 150, 255)
        fakeHitbox.Material = Enum.Material.ForceField
        fakeHitbox.Parent = workspace
        
        return fakeHitbox
    end

    local function controlFakeHitbox()
        if movementConnection then movementConnection:Disconnect() end
        
        local speed = 20
        local verticalSpeed = 0
        local gravity = 50
        local jumpPower = 30
        local isGrounded = false
        local hitboxHeight = fakeHitbox.Size.Y / 2
        local raycastDistance = 0.5

        movementConnection = RunService.RenderStepped:Connect(function(deltaTime)
            if fakeHitbox and isActive then
                local camForward = Camera.CFrame.LookVector
                camForward = Vector3.new(camForward.X, 0, camForward.Z).Unit
                local camRight = Camera.CFrame.RightVector
                camRight = Vector3.new(camRight.X, 0, camRight.Z).Unit

                local inputVector = Vector3.new(0, 0, 0)

                if UIS:IsKeyDown(Enum.KeyCode.W) then inputVector += Vector3.new(0, 0, -1) end
                if UIS:IsKeyDown(Enum.KeyCode.S) then inputVector += Vector3.new(0, 0, 1) end
                if UIS:IsKeyDown(Enum.KeyCode.A) then inputVector += Vector3.new(-1, 0, 0) end
                if UIS:IsKeyDown(Enum.KeyCode.D) then inputVector += Vector3.new(1, 0, 0) end

                if moveDirection.Magnitude > 0 then
                    inputVector += Vector3.new(moveDirection.X, 0, moveDirection.Z)
                end

                if inputVector.Magnitude > 1 then
                    inputVector = inputVector.Unit
                end

                local worldMove = (camRight * inputVector.X) + (camForward * inputVector.Z)
                worldMove = Vector3.new(worldMove.X, 0, worldMove.Z)

                local horizontalVelocity = worldMove * speed
                local newPos = fakeHitbox.Position + horizontalVelocity * deltaTime

                local rayOrigin = newPos
                local rayDirection = Vector3.new(0, -hitboxHeight - raycastDistance, 0)
                local raycastParams = RaycastParams.new()
                raycastParams.FilterType = Enum.RaycastFilterType.Blacklist
                raycastParams.FilterDescendantsInstances = {fakeHitbox, Char, prison}
                
                local rayResult = workspace:Raycast(rayOrigin, rayDirection, raycastParams)
                
                if rayResult then
                    local distanceToGround = (rayOrigin.Y - rayResult.Position.Y) - hitboxHeight
                    if distanceToGround <= 0.1 and verticalSpeed <= 0 then
                        isGrounded = true
                        newPos = Vector3.new(newPos.X, rayResult.Position.Y + hitboxHeight, newPos.Z)
                        verticalSpeed = 0
                    else
                        isGrounded = false
                    end
                else
                    isGrounded = false
                end

                if (UIS:IsKeyDown(Enum.KeyCode.Space) or jumpPressed) and isGrounded then
                    verticalSpeed = jumpPower
                    isGrounded = false
                end

                verticalSpeed -= gravity * deltaTime
                newPos = newPos + Vector3.new(0, verticalSpeed * deltaTime, 0)

                rayOrigin = newPos
                rayResult = workspace:Raycast(rayOrigin, rayDirection, raycastParams)
                if rayResult and verticalSpeed <= 0 then
                    local distanceToGround = (rayOrigin.Y - rayResult.Position.Y) - hitboxHeight
                    if distanceToGround <= 0 then
                        newPos = Vector3.new(newPos.X, rayResult.Position.Y + hitboxHeight, newPos.Z)
                        verticalSpeed = 0
                        isGrounded = true
                    end
                end

                fakeHitbox.Position = newPos

                if Camera.CameraSubject ~= fakeHitbox then
                    Camera.CameraSubject = fakeHitbox
                end
            end
        end)
    end

    -- ===== ATIVAR/DESATIVAR =====
    local function activate()
        if isActive then return end
        
        isActive = true
        updateToggle(IvTog, IvKnob, true)
        
        if not Char or not Char:FindFirstChild("HumanoidRootPart") then
            Char = Player.Character
        end
        
        originalPosition = Char.HumanoidRootPart.Position
        
        prison = createPrison()
        fakeHitbox = createFakeHitbox()
        fakeHitbox.Position = originalPosition
        
        local humanoid = Char:FindFirstChild("Humanoid")
        if humanoid then
            humanoid.WalkSpeed = 0
            humanoid.JumpPower = 0
            humanoid.AutoRotate = false
            humanoid.PlatformStand = true
        end
        
        Char:MoveTo(Vector3.new(0, 10000, 0))
        
        Camera.CameraSubject = fakeHitbox
        Camera.CameraType = Enum.CameraType.Custom
        
        ControlsGui.Enabled = true
        
        controlFakeHitbox()
    end

    local function deactivate()
        if not isActive then return end
        
        isActive = false
        updateToggle(IvTog, IvKnob, false)
        
        if Char and Char:FindFirstChild("Humanoid") then
            local humanoid = Char.Humanoid
            humanoid.WalkSpeed = 16
            humanoid.JumpPower = 50
            humanoid.AutoRotate = true
            humanoid.PlatformStand = false
        end
        
        Camera.CameraSubject = Char
        Camera.CameraType = Enum.CameraType.Custom
        
        if fakeHitbox and Char and Char:FindFirstChild("HumanoidRootPart") then
            Char:MoveTo(fakeHitbox.Position)
        end
        
        if movementConnection then
            movementConnection:Disconnect()
            movementConnection = nil
        end
        
        if prison then prison:Destroy() prison = nil end
        if fakeHitbox then fakeHitbox:Destroy() fakeHitbox = nil end
        
        ControlsGui.Enabled = false
        jumpPressed = false
        moveDirection = Vector3.new(0, 0, 0)
        
        task.wait(0.1)
        if Camera then
            Camera.CameraSubject = Char
            Camera.CameraType = Enum.CameraType.Custom
        end
    end

    -- ===== TOGGLE DO NERO =====
    IvBtn.MouseButton1Click:Connect(function()
        if isActive then
            deactivate()
        else
            activate()
        end
    end)

    -- ===== LIMPEZA AO MORRER =====
    Player.CharacterAdded:Connect(function(newChar)
        Char = newChar
        
        if isActive then
            task.wait(0.5)
            if prison then
                Char:MoveTo(Vector3.new(0, 10000, 0))
                local humanoid = Char:FindFirstChild("Humanoid")
                if humanoid then
                    humanoid.WalkSpeed = 0
                    humanoid.JumpPower = 0
                    humanoid.AutoRotate = false
                    humanoid.PlatformStand = true
                end
            end
        end
    end)

    -- ===== LIMPEZA AO FECHAR SCRIPT =====
    local function cleanup()
        if prison then prison:Destroy() end
        if fakeHitbox then fakeHitbox:Destroy() end
        if movementConnection then movementConnection:Disconnect() end
        if ControlsGui then ControlsGui:Destroy() end
    end

    -- Adiciona a limpeza ao fechar o script
    table.insert(NERO.Connections, {Disconnect = cleanup})
end)
-- ==========================================================
-- TOGGLE: IMORTALIDADE FE (CORRIGIDO - SEM TRAVAR)
-- ==========================================================
task.spawn(function()
    task.wait(0.1)

    -- CRIA O TOGGLE NA ABA 1 (HUNTERS)
    local ImTog, ImKnob, ImBtn = createToggle("🛡️ IMORTALIDADE FE", tabContainers[1], 336)

    local godModeAtivo = false
    local godModeThread = nil
    local godModeConnections = {}
    local mtBackup = nil  -- GUARDA O __namecall ORIGINAL

    -- ===== SISTEMA ORIGINAL (INTACTO) =====
    local function getDamageRemotes()
        local remotes = {}
        for _, obj in pairs(workspace:GetDescendants()) do
            if obj:IsA("RemoteEvent") or obj:IsA("RemoteFunction") then
                if string.lower(obj.Name):find("damage") or 
                   string.lower(obj.Name):find("hit") or 
                   string.lower(obj.Name):find("attack") then
                    table.insert(remotes, obj)
                end
            end
        end
        return remotes
    end

    -- FUNÇÃO ORIGINAL (COMPLETA)
    local function ativarGodMode()
        _G.GodModeActive = true
        local player = game.Players.LocalPlayer
        local char = player.Character
        local hum = char and char:FindFirstChildOfClass("Humanoid")
        local root = char and char:FindFirstChild("HumanoidRootPart")

        if hum and root then
            hum.MaxHealth = math.huge
            hum.Health = 0
            hum:SetStateEnabled(Enum.HumanoidStateType.Dead, false)
            
            local remotes = getDamageRemotes()
            if setreadonly and getrawmetatable then
                local mt = getrawmetatable(game)
                mtBackup = mt.__namecall  -- SALVA O ORIGINAL
                local old = mt.__namecall
                setreadonly(mt, false)
                mt.__namecall = newcclosure(function(self, ...)
                    if _G.GodModeActive and table.find(remotes, self) then
                        return nil
                    end
                    return old(self, ...)
                end)
                setreadonly(mt, true)
            end

            -- LOOP DE PROTEÇÃO
            task.spawn(function()
                while _G.GodModeActive and char and char.Parent do
                    hum.Health = 0
                    if hum.MaxHealth ~= math.huge then hum.MaxHealth = math.huge end

                    local parts = workspace:GetPartBoundsInRadius(root.Position, 10)
                    for _, part in ipairs(parts) do
                        if part:IsA("BasePart") and not part:IsDescendantOf(char) then
                            part.CanTouch = false
                        end
                    end

                    for _, tool in pairs(char:GetChildren()) do
                        if tool:IsA("Tool") then
                            local ds = tool:FindFirstChild("Damage") or tool:FindFirstChild("SwordScript")
                            if ds then ds.Disabled = true end
                        end
                    end

                    pcall(function() root:SetNetworkOwner(player) end)
                    
                    if root.Position.Y < -500 then 
                        root.CFrame = CFrame.new(root.Position.X, 100, root.Position.Z) 
                    end
                    hum:ChangeState(Enum.HumanoidStateType.GettingUp)

                    task.wait(0.1)
                end
            end)

            -- Conexão de Morte (Auto-Respawn)
            local diedConn = hum.Died:Connect(function()
                if _G.GodModeActive then
                    player:LoadCharacter()
                end
            end)
            table.insert(godModeConnections, diedConn)

            print("🔥 God5 HÍBRIDO ATIVADO!")
        end
    end

    -- FUNÇÃO PARA DESATIVAR (SEM QUEBRAR O JOGO)
    local function desativarGodMode()
        _G.GodModeActive = false
        
        -- RESETA O __namecall PARA O ORIGINAL (NÃO PARA NIL)
        if setreadonly and getrawmetatable and mtBackup then
            local mt = getrawmetatable(game)
            setreadonly(mt, false)
            mt.__namecall = mtBackup  -- RESTAURA O ORIGINAL
            setreadonly(mt, true)
            mtBackup = nil
        end
        
        -- RESETA O PERSONAGEM
        local player = game.Players.LocalPlayer
        local char = player.Character
        if char then
            local hum = char:FindFirstChildOfClass("Humanoid")
            if hum then
                hum.MaxHealth = 100
                hum.Health = 100
                hum:SetStateEnabled(Enum.HumanoidStateType.Dead, true)
            end
        end
        
        -- LIMPA CONEXÕES
        for _, conn in pairs(godModeConnections) do
            pcall(function() conn:Disconnect() end)
        end
        godModeConnections = {}
        
        print("❌ God Mode DESATIVADO!")
    end

    -- ===== FUNÇÃO PARA ATIVAR ESPERANDO O PERSONAGEM =====
    local function esperarEAtivar()
        local player = game.Players.LocalPlayer
        local char = player.Character or player.CharacterAdded:Wait()
        local hum = char:WaitForChild("Humanoid", 5)
        local root = char:WaitForChild("HumanoidRootPart", 5)
        
        if hum and root then
            print("✅ Personagem pronto, ativando God Mode...")
            ativarGodMode()
        end
    end

    -- ===== TOGGLE DO NERO =====
    ImBtn.MouseButton1Click:Connect(function()
        godModeAtivo = not godModeAtivo
        updateToggle(ImTog, ImKnob, godModeAtivo)

        if godModeAtivo then
            print("🛡️ ATIVANDO IMORTALIDADE FE!")
            _G.GodModeActive = true
            esperarEAtivar()
            
            -- Conexão para renascimento
            local player = game.Players.LocalPlayer
            local charAddedConn = player.CharacterAdded:Connect(function()
                if godModeAtivo then
                    print("🔄 Personagem mudou, reativando...")
                    task.wait(0.5)
                    ativarGodMode()
                end
            end)
            table.insert(godModeConnections, charAddedConn)
            
            Notify("God mode actived")
        else
            print("God mode desactived")
            desativarGodMode()
            Notify("God mode desactived")
        end
    end)

    -- ===== LIMPEZA AO FECHAR (SEM QUEBRAR) =====
    local function cleanup()
        _G.GodModeActive = false
        
        -- RESTAURA O __namecall ORIGINAL
        if setreadonly and getrawmetatable and mtBackup then
            local mt = getrawmetatable(game)
            setreadonly(mt, false)
            mt.__namecall = mtBackup
            setreadonly(mt, true)
            mtBackup = nil
        end
        
        for _, conn in pairs(godModeConnections) do
            pcall(function() conn:Disconnect() end)
        end
        godModeConnections = {}
    end

    table.insert(NERO.Connections, {Disconnect = cleanup})
end)
-- ==========================================================
-- TOGGLE: AUTO CLICK + MODO ANDAR (BLOQUEIA CÂMERA NA ESQUERDA)
-- ==========================================================
task.spawn(function()
    task.wait(0.1)

    -- ===== POSIÇÃO NA ABA =====
    local function getUltimaPosicao()
        local ultimoY = 0
        for _, child in pairs(tabContainers[7]:GetChildren()) do
            if child:IsA("Frame") then
                local posY = child.Position.Y.Offset
                if posY > ultimoY then
                    ultimoY = posY
                end
            end
        end
        return ultimoY + 56
    end

    local posInicial = getUltimaPosicao()
    local posAtual = posInicial

    -- ===== TOGGLE =====
    local AcTog, AcKnob, AcBtn = createPremiumToggle("🖱️ AUTO CLICK", "Ativa click + joystick mobile", tabContainers[7], posAtual)
    posAtual = posAtual + 56

    -- ===== CAMPO VELOCIDADE =====
    local function criarCampoVelocidade(yPos)
        local frame = Instance.new("Frame")
        frame.Size = UDim2.new(1, 0, 0, 44)
        frame.Position = UDim2.new(0, 0, 0, yPos)
        frame.BackgroundColor3 = C.surface
        frame.BorderSizePixel = 0
        frame.Parent = tabContainers[7]
        Instance.new("UICorner", frame).CornerRadius = UDim.new(0, 20)
        local stroke = Instance.new("UIStroke", frame)
        stroke.Color = C.primary
        stroke.Transparency = 0.7
        local label = Instance.new("TextLabel")
        label.Size = UDim2.new(0, 150, 1, 0)
        label.Position = UDim2.new(0, 15, 0, 0)
        label.BackgroundTransparency = 1
        label.Text = "⚡ Toques por segundo:"
        label.TextColor3 = C.text
        label.TextSize = 12
        label.Font = Enum.Font.GothamMedium
        label.TextXAlignment = Enum.TextXAlignment.Left
        label.Parent = frame
        local input = Instance.new("TextBox")
        input.Size = UDim2.new(0, 60, 0, 30)
        input.Position = UDim2.new(1, -75, 0, 7)
        input.BackgroundColor3 = C.bg
        input.TextColor3 = C.primary
        input.TextSize = 14
        input.Font = Enum.Font.GothamBold
        input.Text = "10"
        input.PlaceholderText = "10"
        input.Parent = frame
        Instance.new("UICorner", input).CornerRadius = UDim.new(0, 13)
        return input
    end

    local clickInput = criarCampoVelocidade(posAtual)
    posAtual = posAtual + 56

    -- ===== VARIÁVEIS =====
    local sistemaAtivo = false
    local clickAtivo = false
    local clickThread = nil
    local velocidade = 10
    local VirtualInput = game:GetService("VirtualInputManager")
    local UIS = game:GetService("UserInputService")
    local centerX = workspace.CurrentCamera.ViewportSize.X / 2
    local centerY = workspace.CurrentCamera.ViewportSize.Y / 2

    -- ===== GUI =====
    local botaoGui = nil
    local botaoBtn = nil
    local walkGui = nil
    local walkMoveDir = Vector3.new(0, 0, 0)
    local walkJoystickActive = false
    local walkJoystickCenter = Vector2.new(0, 0)
    local walkConnection = nil
    local touchBlocker = nil  -- Bloqueia toques na metade esquerda

    -- ===== BLOQUEIA TOQUES NA METADE ESQUERDA (PARA NÃO AFETAR A CÂMERA) =====
    local function criarTouchBlocker()
        if touchBlocker then return end
        touchBlocker = Instance.new("ScreenGui")
        touchBlocker.Name = "NeroTouchBlocker"
        touchBlocker.ResetOnSpawn = false
        touchBlocker.Parent = game:GetService("CoreGui")
        touchBlocker.IgnoreGuiInset = true

        -- Área que bloqueia toques na metade esquerda
        local blocker = Instance.new("Frame")
        blocker.Size = UDim2.new(0.5, 0, 1, 0)
        blocker.Position = UDim2.new(0, 0, 0, 0)
        blocker.BackgroundTransparency = 1
        blocker.BorderSizePixel = 0
        blocker.ZIndex = 9999  -- Fica acima de tudo
        blocker.Parent = touchBlocker

        -- Impede que os toques passem para a câmera
        blocker.InputBegan:Connect(function(input)
            if input.UserInputType == Enum.UserInputType.Touch then
                -- Impede que o toque na esquerda seja processado pela câmera
                return
            end
        end)
    end

    local function removerTouchBlocker()
        if touchBlocker then
            touchBlocker:Destroy()
            touchBlocker = nil
        end
    end

    -- ===== CLICK (USA MOUSE) =====
    local function clickar()
        pcall(function()
            VirtualInput:SendMouseButtonEvent(centerX, centerY, 0, true, game, 1)
            task.wait(0.01)
            VirtualInput:SendMouseButtonEvent(centerX, centerY, 0, false, game, 1)
        end)
    end

    local function iniciarClick()
        if clickThread then return end
        clickThread = task.spawn(function()
            while clickAtivo do
                clickar()
                task.wait(1 / velocidade)
            end
        end)
    end

    local function pararClick()
        if clickThread then
            task.cancel(clickThread)
            clickThread = nil
        end
    end

    -- ===== BOTÃO FLUTUANTE =====
    local function criarBotaoFlutuante()
        if botaoGui then botaoGui:Destroy() end
        botaoGui = Instance.new("ScreenGui")
        botaoGui.Name = "NeroAutoClick"
        botaoGui.ResetOnSpawn = false
        botaoGui.Parent = game:GetService("CoreGui")
        botaoGui.IgnoreGuiInset = true

        botaoBtn = Instance.new("TextButton")
        botaoBtn.Size = UDim2.new(0, 60, 0, 60)
        botaoBtn.Position = UDim2.new(0.85, 0, 0.5, 0)
        botaoBtn.BackgroundColor3 = C.bg
        botaoBtn.BorderSizePixel = 0
        botaoBtn.Text = "🖱️"
        botaoBtn.TextColor3 = C.primary
        botaoBtn.TextSize = 24
        botaoBtn.Font = Enum.Font.GothamBold
        botaoBtn.Parent = botaoGui
        botaoBtn.ZIndex = 5
        Instance.new("UICorner", botaoBtn).CornerRadius = UDim.new(0, 30)
        local stroke = Instance.new("UIStroke", botaoBtn)
        stroke.Color = C.primary
        stroke.Thickness = 2
        stroke.Transparency = 0.3
        local status = Instance.new("TextLabel")
        status.Size = UDim2.new(1, 0, 0, 16)
        status.Position = UDim2.new(0, 0, 1, 4)
        status.BackgroundTransparency = 1
        status.Text = "PARADO"
        status.TextColor3 = C.subtext
        status.TextSize = 10
        status.Font = Enum.Font.GothamBold
        status.Parent = botaoBtn

        botaoBtn.MouseButton1Click:Connect(function()
            clickAtivo = not clickAtivo
            if clickAtivo then
                botaoBtn.BackgroundColor3 = C.primary
                botaoBtn.TextColor3 = C.bg
                stroke.Color = Color3.fromRGB(0, 255, 0)
                status.Text = "▶️ CLICANDO"
                status.TextColor3 = Color3.fromRGB(0, 255, 0)
                iniciarClick()
                Notify("🖱️ Click INICIADO!")
            else
                botaoBtn.BackgroundColor3 = C.bg
                botaoBtn.TextColor3 = C.primary
                stroke.Color = C.primary
                status.Text = "⏸️ PARADO"
                status.TextColor3 = C.subtext
                pararClick()
                Notify("⏸️ Click PARADO!")
            end
        end)

        local dragging = false
        local dragStart = nil
        local startPos = nil
        botaoBtn.InputBegan:Connect(function(input)
            if input.UserInputType == Enum.UserInputType.Touch or input.UserInputType == Enum.UserInputType.MouseButton1 then
                if not clickAtivo then
                    dragging = true
                    dragStart = input.Position
                    startPos = botaoBtn.Position
                end
            end
        end)
        botaoBtn.InputChanged:Connect(function(input)
            if dragging and (input.UserInputType == Enum.UserInputType.Touch or input.UserInputType == Enum.UserInputType.MouseMovement) then
                local delta = input.Position - dragStart
                botaoBtn.Position = UDim2.new(startPos.X.Scale, startPos.X.Offset + delta.X, startPos.Y.Scale, startPos.Y.Offset + delta.Y)
            end
        end)
        botaoBtn.InputEnded:Connect(function(input)
            if input.UserInputType == Enum.UserInputType.Touch or input.UserInputType == Enum.UserInputType.MouseButton1 then
                dragging = false
            end
        end)
    end

    -- ===== MOVIMENTO RELATIVO À CÂMERA =====
    local function criarModoAndar()
        if walkGui then walkGui:Destroy() end
        walkGui = Instance.new("ScreenGui")
        walkGui.Name = "NeroWalk"
        walkGui.ResetOnSpawn = false
        walkGui.Parent = game:GetService("CoreGui")
        walkGui.IgnoreGuiInset = true

        -- Área do joystick (metade esquerda)
        local joystickArea = Instance.new("Frame")
        joystickArea.Size = UDim2.new(0.5, 0, 1, 0)
        joystickArea.Position = UDim2.new(0, 0, 0, 0)
        joystickArea.BackgroundTransparency = 1
        joystickArea.BorderSizePixel = 0
        joystickArea.ZIndex = 10000  -- MAIOR QUE O BLOQUEADOR
        joystickArea.Parent = walkGui

        -- Botão de pulo (canto inferior direito)
        local jumpBtn = Instance.new("ImageButton")
        jumpBtn.Size = UDim2.new(0, 100, 0, 100)
        jumpBtn.Position = UDim2.new(0.85, 0, 0.85, 0)
        jumpBtn.BackgroundColor3 = Color3.new(1, 1, 1)
        jumpBtn.BackgroundTransparency = 1
        jumpBtn.BorderSizePixel = 0
        jumpBtn.Image = "rbxassetid://0"
        jumpBtn.AutoButtonColor = false
        jumpBtn.ZIndex = 10001
        jumpBtn.Parent = walkGui

        -- Joystick: captura direção
        joystickArea.InputBegan:Connect(function(input)
            if input.UserInputType == Enum.UserInputType.Touch or input.UserInputType == Enum.UserInputType.MouseButton1 then
                walkJoystickActive = true
                walkJoystickCenter = Vector2.new(input.Position.X, input.Position.Y)
                walkMoveDir = Vector3.new(0, 0, 0)
                -- Cancela propagação para a câmera
                return true
            end
        end)

        joystickArea.InputChanged:Connect(function(input)
            if walkJoystickActive and (input.UserInputType == Enum.UserInputType.Touch or input.UserInputType == Enum.UserInputType.MouseMovement) then
                local currentPos = Vector2.new(input.Position.X, input.Position.Y)
                local offset = currentPos - walkJoystickCenter
                local maxRadius = 60
                if offset.Magnitude > maxRadius then
                    offset = offset.Unit * maxRadius
                end
                walkMoveDir = Vector3.new(offset.X / maxRadius, 0, -offset.Y / maxRadius)
                return true
            end
        end)

        joystickArea.InputEnded:Connect(function(input)
            if input.UserInputType == Enum.UserInputType.Touch or input.UserInputType == Enum.UserInputType.MouseButton1 then
                walkJoystickActive = false
                walkMoveDir = Vector3.new(0, 0, 0)
                return true
            end
        end)

        -- Pulo
        jumpBtn.InputBegan:Connect(function(input)
            if input.UserInputType == Enum.UserInputType.Touch or input.UserInputType == Enum.UserInputType.MouseButton1 then
                VirtualInput:SendKeyEvent(true, Enum.KeyCode.Space, false, game)
                return true
            end
        end)

        jumpBtn.InputEnded:Connect(function(input)
            if input.UserInputType == Enum.UserInputType.Touch or input.UserInputType == Enum.UserInputType.MouseButton1 then
                VirtualInput:SendKeyEvent(false, Enum.KeyCode.Space, false, game)
                return true
            end
        end)

        -- Loop de movimento relativo à câmera
        if walkConnection then walkConnection:Disconnect() end
        walkConnection = game:GetService("RunService").RenderStepped:Connect(function()
            if not sistemaAtivo then return end
            local player = game.Players.LocalPlayer
            local char = player.Character
            if not char then return end
            local hrp = char:FindFirstChild("HumanoidRootPart")
            if not hrp then return end

            local speed = 30
            local cam = workspace.CurrentCamera
            local camForward = cam.CFrame.LookVector
            camForward = Vector3.new(camForward.X, 0, camForward.Z).Unit
            local camRight = cam.CFrame.RightVector
            camRight = Vector3.new(camRight.X, 0, camRight.Z).Unit

            local moveVector = Vector3.new(0, 0, 0)
            if walkMoveDir.Magnitude > 0 then
                moveVector = (camRight * walkMoveDir.X) + (camForward * walkMoveDir.Z)
                moveVector = moveVector.Unit * speed
            end
            hrp.Velocity = Vector3.new(moveVector.X, hrp.Velocity.Y, moveVector.Z)

            if moveVector.Magnitude > 0.5 then
                local lookTarget = hrp.Position + moveVector
                hrp.CFrame = CFrame.new(hrp.Position, lookTarget)
            end
        end)

        walkGui:SetAttribute("WalkConnection", walkConnection)
    end

    local function removerModoAndar()
        if walkGui then
            local conn = walkGui:GetAttribute("WalkConnection")
            if conn then
                conn:Disconnect()
            end
            walkGui:Destroy()
            walkGui = nil
        end
        if walkConnection then
            walkConnection:Disconnect()
            walkConnection = nil
        end
        walkMoveDir = Vector3.new(0, 0, 0)
        local player = game.Players.LocalPlayer
        local char = player.Character
        if char and char:FindFirstChild("HumanoidRootPart") then
            char.HumanoidRootPart.Velocity = Vector3.new(0, char.HumanoidRootPart.Velocity.Y, 0)
        end
    end

    -- ===== TOGGLE PRINCIPAL =====
    AcBtn.MouseButton1Click:Connect(function()
        if sistemaAtivo then
            -- === DESATIVA ===
            sistemaAtivo = false
            updateToggle(AcTog, AcKnob, false, true)
            clickAtivo = false
            pararClick()
            if botaoGui then
                botaoGui:Destroy()
                botaoGui = nil
                botaoBtn = nil
            end
            removerModoAndar()
            removerTouchBlocker()
            task.spawn(function()
                for i = 7, 1, -1 do
                    Notify("⏳ Restaurando analógico em " .. i .. "...")
                    task.wait(1)
                end
                Notify("✅ Analógico restaurado! Volte ao mobile.")
            end)
        else
            -- === ATIVA ===
            sistemaAtivo = true
            updateToggle(AcTog, AcKnob, true, true)
            criarBotaoFlutuante()
            criarModoAndar()
            criarTouchBlocker()  -- BLOQUEIA TOQUES NA METADE ESQUERDA
            Notify("🖱️ Auto Click ATIVADO! (Click + Joystick)")
        end
    end)

    -- ===== VELOCIDADE =====
    clickInput.FocusLost:Connect(function()
        local v = tonumber(clickInput.Text)
        if v and v > 0 then
            velocidade = math.floor(v)
            clickInput.Text = tostring(velocidade)
            if clickAtivo then
                pararClick()
                iniciarClick()
            end
            Notify("⚡ Velocidade: " .. velocidade .. " toques/s")
        else
            clickInput.Text = tostring(velocidade)
        end
    end)

    -- ===== LIMPEZA =====
    local function cleanup()
        pararClick()
        if botaoGui then
            botaoGui:Destroy()
            botaoGui = nil
            botaoBtn = nil
        end
        if sistemaAtivo then
            removerModoAndar()
            removerTouchBlocker()
        end
    end

    table.insert(NERO.Connections, {Disconnect = cleanup})

    pcall(function()
        if tabContainers[7]:IsA("ScrollingFrame") then
            tabContainers[7].CanvasSize = UDim2.new(0, 0, 0, posAtual + 100)
        end
    end)
end)
-- ==========================================================
-- TOGGLE: CORPO ANIMATIONS (NERO) - COMPLETO (NEW UI) - FIX SEM TRAVAR
-- ==========================================================
task.spawn(function()
    task.wait(0.1)

    -- ===== CRIA TOGGLE NA ABA VISUAIS =====
    local AnimTog, AnimKnob, AnimBtn = createToggle("🎭 Corpo Animations", tabContainers[4], 96)

    local janelaAberta = false
    local guiJanela = nil
    local framePrincipal = nil
    local emoteAtual = nil
    local animAtual = nil
    local origAnims = {}
    local minimizado = false

    -- Variáveis de Configuração
    local configLoop = true
    local configSpeed = 1.0
    local manterAnimacoes = false  -- NOVA VARIÁVEL

    -- Cores baseadas na nova paleta fornecida (Nero)
    local Cores = {
        BgPrincipal = Color3.fromRGB(0, 0, 0),
        BgSecundario = Color3.fromRGB(15, 15, 15),
        BgCartao = Color3.fromRGB(15, 15, 15),
        BgCartaoHover = Color3.fromRGB(20, 10, 0),
        BgTop = Color3.fromRGB(10, 10, 10),
        Laranja = Color3.fromRGB(255, 100, 0),
        LaranjaVib = Color3.fromRGB(255, 185, 70),
        LaranjaEscuro = Color3.fromRGB(255, 100, 0),
        Texto = Color3.fromRGB(255, 255, 255),
        TextoMuted = Color3.fromRGB(160, 160, 170),
        BorderGlow = Color3.fromRGB(255, 100, 0)
    }

    -- ============================================================
    -- ===== TODOS OS 262 EMOTES =====
    -- ============================================================
    local Emotes = {
        ["Fashion"] = 3333331310, ["Baby Dance"] = 4265725525, ["Cha-Cha"] = 6862001787,
        ["Monkey"] = 3333499508, ["Shuffle"] = 4349242221, ["Top Rock"] = 3361276673,
        ["Around Town"] = 3303391864, ["Fancy Feet"] = 3333432454, ["Hype Dance"] = 3695333486,
        ["Bodybuilder"] = 3333387824, ["Idol"] = 4101966434, ["Curtsy"] = 4555816777,
        ["Happy"] = 4841405708, ["Quiet Waves"] = 7465981288, ["Sleep"] = 4686925579,
        ["Floss Dance"] = 5917459365, ["Shy"] = 3337978742, ["Godlike"] = 3337994105,
        ["Hero Landing"] = 5104344710, ["High Wave"] = 5915690960, ["Cower"] = 4940563117,
        ["Bored"] = 5230599789, ["Show Dem Wrists -KSI"] = 7198989668, ["Celebrate"] = 3338097973,
        ["Dash"] = 582855105, ["Beckon"] = 5230598276, ["Haha"] = 3337966527,
        ["Lasso Turn - Tai Verdes"] = 7942896991, ["Line Dance"] = 4049037604, ["Shrug"] = 3334392772,
        ["Point2"] = 3344585679, ["Stadium"] = 3338055167, ["Confused"] = 4940561610,
        ["Side to Side"] = 3333136415, ["Old Town Road Dance - Lil Nas X"] = 5937560570,
        ["Hello"] = 3344650532, ["Dolphin Dance"] = 5918726674, ["Samba"] = 6869766175,
        ["Break Dance"] = 5915648917, ["Hips Poppin' - Zara Larsson"] = 6797888062,
        ["Wake Up Call - KSI"] = 7199000883, ["Greatest"] = 3338042785,
        ["On The Outside - Twenty One"] = 7422779536, ["Boxing Punch - KSI"] = 7202863182,
        ["Sad"] = 4841407203, ["Flowing Breeze"] = 7465946930, ["Twirl"] = 3334968680,
        ["Jumping Wave"] = 4940564896, ["HOLIDAY Dance - Lil Nas X (LNX)"] = 5937558680,
        ["Take Me Under - Zara Larsson"] = 6797890377, ["Dizzy"] = 3361426436,
        ["Dancing' Shoes - Twenty One"] = 7404878500, ["Fast Hands"] = 4265701731,
        ["Tree"] = 4049551434, ["Agree"] = 4841397952, ["Power Blast"] = 4841403964,
        ["Swoosh"] = 3361481910, ["Jumping Cheer"] = 5895324424, ["Disagree"] = 4841401869,
        ["Rodeo Dance - Lil Nas X (LNX)"] = 5918728267, ["It Ain't My Fault - Zara Larsson"] = 6797891807,
        ["Rock On"] = 5915714366, ["Block Partier"] = 6862022283, ["Dorky Dance"] = 4212455378,
        ["Zombie"] = 4210116953, ["AOK - Tai Verdes"] = 7942885103, ["T"] = 3338010159,
        ["Cobra Arms - Tai Verdes"] = 7942890105, ["Panini Dance - Lil Nas X (LNX)"] = 5915713518,
        ["Fishing"] = 3334832150, ["Robot"] = 3338025566, ["Saturday Dance - Twenty One"] = 7422807549,
        ["Keeping Time"] = 4555808220, ["Air Dance"] = 4555782893, ["Rock Guitar - Royal Blood"] = 6532134724,
        ["Borock's Rage"] = 3236842542, ["Ud'zal's Summoning"] = 3303161675, ["Y"] = 4349285876,
        ["Swan Dance"] = 7465997989, ["Louder"] = 3338083565, ["Up and Down - Twenty One"] = 7422797678,
        ["Drummer Moves - Twenty One"] = 7422527690, ["Sneaky"] = 3334424322, ["Heisman Pose"] = 3695263073,
        ["Jacks"] = 3338066331, ["Cha-Cha 2"] = 3695322025, ["BURBERRY LOLA ATTITUDE - NIMBUS"] = 10147821284,
        ["BURBERRY LOLA ATTITUDE - GEM"] = 10147815602, ["BURBERRY LOLA ATTITUDE - HYDRO"] = 10147823318,
        ["BURBERRY LOLA ATTITUDE - BLOOM"] = 10147817997, ["Superhero Reveal"] = 3695373233,
        ["Air Guitar"] = 3695300085, ["Dismissive Wave"] = 3333272779,
        ["Country Line Dance - Lil Nas X"] = 5915712534, ["Salute"] = 3333474484,
        ["Applaud"] = 5915693819, ["Get Out"] = 3333272779, ["Hwaiting (화이팅)"] = 9527885267,
        ["Annyeong (안녕)"] = 9527883498, ["Bunny Hop"] = 4641985101, ["Sandwich Dance"] = 4406555273,
        ["Hyperfast 5G Dance Move"] = 9408617181, ["Victory - 24kGoldn"] = 9178377686,
        ["Tantrum"] = 5104341999, ["Rock Star - Royal Blood"] = 10714400171,
        ["Drum Solo - Royal Blood"] = 6532839007, ["Drum Master - Royal Blood"] = 6531483720,
        ["High Hands"] = 9710985298, ["Tilt"] = 3334538554, ["Gashina - SUNMI"] = 9527886709,
        ["Chicken Dance"] = 4841399916, ["You can't sit with us - Sunmi"] = 9983520970,
        ["Frosty Flair - Tommy Hilfiger"] = 10214311282, ["Floor Rock Freeze - Tommy Hilfiger"] = 10214314957,
        ["Boom Boom Clap - George Ezra"] = 10370346995, ["Cartwheel - George Ezra"] = 10370351535,
        ["Chill Vibes - George Ezra"] = 10370353969, ["Sidekicks - George Ezra"] = 10370362157,
        ["The Conductor - George Ezra"] = 10370359115, ["Super Charge"] = 10478338114,
        ["Swag Walk"] = 10478341260, ["Mean Mug - Tommy Hilfiger"] = 10214317325,
        ["V Pose - Tommy Hilfiger"] = 10214319518, ["Uprise - Tommy Hilfiger"] = 10275008655,
        ["2 Baddies Dance Move - NCT 127"] = 12259828678, ["Kick It Dance Move - NCT 127"] = 12259826609,
        ["Sticker Dance Move - NCT 127"] = 12259825026, ["Elton John - Rock Out"] = 11753474067,
        ["Elton John - Heart Skip"] = 11309255148, ["Elton John - Still Standing"] = 11444443576,
        ["Elton John - Elevate"] = 11394033602, ["Elton John - Cat Man"] = 11444441914,
        ["Elton John - Piano Jump"] = 11453082181, ["Alo Yoga Pose - Triangle"] = 12507084541,
        ["Alo Yoga Pose - Warrior II"] = 12507083048, ["Alo Yoga Pose - Lotus Position"] = 12507085924,
        ["TWICE-Moonlight-Sunrise"] = 12714233242, ["TWICE-Set-Me-Free-Dance-1"] = 12714228341,
        ["TWICE-Set-Me-Free-Dance-2"] = 12714231087, ["Ay-Yo-Dance-Move-NCT-127"] = 12804157977,
        ["TWICE-The-Feels"] = 12874447851, ["Rise-Above-The-Chainsmokers"] = 12992262118,
        ["TWICE-What-Is-Love"] = 13327655243, ["Man-City-Bicycle-Kick"] = 13421057998,
        ["TWICE-Fancy"] = 13520524517, ["TWICE Pop by Nayeon"] = 13768941455,
        ["Tommy - Archer"] = 13823324057, ["Man City Backflip"] = 13694100677,
        ["Man-City-Scorpion-Kick"] = 13694096724, ["Arm Twist"] = 10713968716,
        ["YUNGBLUD – HIGH KICK"] = 14022936101, ["TWICE Like Ooh-Ahh"] = 14123781004,
        ["Baby Queen - Air Guitar & Knee Slide"] = 14352335202, ["Baby Queen - Dramatic Bow"] = 14352337694,
        ["Baby Queen - Face Frame"] = 14352340648, ["Baby Queen - Bouncy Twirl"] = 14352343065,
        ["Baby Queen - Strut"] = 14352362059, ["BLACKPINK Pink Venom - Get em Get em Get em"] = 14548619594,
        ["BLACKPINK Pink Venom - I Bring the Pain Like…"] = 14548620495,
        ["BLACKPINK Pink Venom - Straight to Ya Dome"] = 14548621256, ["TWICE LIKEY"] = 14899979575,
        ["TWICE Feel Special"] = 14899980745, ["BLACKPINK Shut Down - Part 1"] = 14901306096,
        ["BLACKPINK Shut Down - Part 2"] = 14901308987, ["Bone Chillin' Bop"] = 15122972413,
        ["Paris Hilton - Sliving For The Groove"] = 15392759696, ["Paris Hilton - Iconic IT-Grrrl"] = 15392756794,
        ["Paris Hilton - Checking My Angles"] = 15392752812, ["BLACKPINK JISOO Flower"] = 15439354020,
        ["BLACKPINK JENNIE You and Me"] = 15439356296, ["Rock n Roll"] = 15505458452,
        ["Victory Dance"] = 15505456446, ["Flex Walk"] = 15505459811, ["Olivia Rodrigo Head Bop"] = 15517864808,
        ["Olivia Rodrigo good 4 u"] = 15517862739, ["Olivia Rodrigo Fall Back to Float"] = 15549124879,
        ["Nicki Minaj That's That Super Bass"] = 15571446961, ["Nicki Minaj Boom Boom Boom"] = 15571448688,
        ["Nicki Minaj Anaconda"] = 15571450952, ["Nicki Minaj Starships"] = 15571453761,
        ["Yungblud Happier Jump"] = 15609995579, ["Festive Dance"] = 15679621440,
        ["BLACKPINK LISA Money"] = 15679623052, ["BLACKPINK ROSÉ On The Ground"] = 15679624464,
        ["Imagine Dragons - “Bones” Dance"] = 15689279687, ["GloRilla - \"Tomorrow\" Dance"] = 15689278184,
        ["d4vd - Backflip"] = 15693621070, ["ericdoa - dance"] = 15698402762, ["Cuco - Levitate"] = 15698404340,
        ["Mean Girls Dance Break"] = 15963314052, ["Paris Hilton Sanasa"] = 16126469463,
        ["BLACKPINK Ice Cream"] = 16181797368, ["BLACKPINK Kill This Love"] = 16181798319,
        ["TWICE I GOT YOU part 1"] = 16215030041, ["TWICE I GOT YOU part 2"] = 16256203246,
        ["Dave's Spin Move - Glass Animals"] = 16272432203, ["Sol de Janeiro - Samba"] = 16270690701,
        ["Beauty Touchdown"] = 16302968986, ["Skadoosh Emote - Kung Fu Panda 4"] = 16371217304,
        ["Jawny - Stomp"] = 16392075853, ["Mae Stephens - Piano Hands"] = 16553163212,
        ["BLACKPINK Boombayah Emote"] = 16553164850, ["BLACKPINK DDU-DU DDU-DU"] = 16553170471,
        ["HIPMOTION - Amaarae"] = 16572740012, ["Mae Stephens – Arm Wave"] = 16584481352,
        ["Wanna play?"] = 16646423316, ["BLACKPINK-How-You-Like-That"] = 16874470507,
        ["BLACKPINK - Lovesick Girls"] = 16874472321, ["Mini Kong"] = 17000021306,
        ["HUGO Let's Drive!"] = 17360699557, ["Wisp - air guitar"] = 17370775305,
        ["Vans Ollie"] = 18305395285, ["Sturdy Dance - Ice Spice"] = 17746180844,
        ["Rolling Stones Guitar Strum"] = 18148804340, ["Rock Out - Bebe Rexha"] = 18225053113,
        ["SpongeBob Imaginaaation 🌈"] = 18443237526, ["SpongeBob Dance"] = 18443245017,
        ["Shrek Roar"] = 18524313628, ["Team USA Breaking Emote"] = 18526288497,
        ["NBA WNBA Fadeaway"] = 18526362841, ["Vroom Vroom"] = 18526397037,
        ["TMNT Dance"] = 18665811005, ["Olympic Dismount"] = 18665825805,
        ["BLACKPINK As If It's Your Last"] = 18855536648, ["BLACKPINK Don't know what to do"] = 18855531354,
        ["TWICE ABCD by Nayeon"] = 18933706381, ["Charli xcx - Apple Dance"] = 18946844622,
        ["The Zabb"] = 129470135909814, ["Fashion Klossette - Runway my way"] = 80995190624232,
        ["ALTÉGO - Couldn’t Care Less"] = 107875941017127, ["Fashion Roadkill"] = 136831243854748,
        ["Skibidi Toilet - Titan Speakerman Laser Spin"] = 134283166482394,
        ["Chappell Roan HOT TO GO!"] = 85267023718407, ["Secret Handshake Dance"] = 71243990877913,
        ["KATSEYE - Touch"] = 135876612109535, ["Fashion Spin"] = 131669256082047,
        ["TWICE Strategy"] = 97311229290836, ["NBA Monster Dunk"] = 132748833449150,
        ["DearALICE - Ariana"] = 134318425949290, ["The Weeknd Starboy Strut"] = 71105746210464,
        ["The Weeknd Opening Night"] = 133110725387025, ["Robot M3GAN"] = 125803725853577,
        ["M3GAN's Dance"] = 99649534578309, ["Rasputin – Boney M."] = 114872820353992,
        ["Thanos Happy Jump - Squid Game"] = 97611664803614, ["Young-hee Head Spin - Squid Game"] = 112011282168475,
        ["TWICE Takedown"] = 140182843839424, ["Stray Kids Walkin On Water"] = 125064469983655,
        ["TWICE TAKEDOWN DANCE 2"] = 127104635954695
    }

    -- ============================================================
    -- ===== TODAS AS 57 ANIMAÇÕES =====
    -- ============================================================
    local Animations = {
        ["Stylish"] = {Idle = 616136790, Idle2 = 616138447, Idle3 = 886888594, Walk = 616146177, Run = 616140816, Jump = 616139451, Climb = 616133594, Fall = 616134815, Swim = 616143378, SwimIdle = 616144772},
        ["Zombie"] = {Idle = 616158929, Idle2 = 616160636, Idle3 = 885545458, Walk = 616168032, Run = 616163682, Jump = 616161997, Climb = 616156119, Fall = 616157476, Swim = 616165109, SwimIdle = 616166655},
        ["Robot"] = {Idle = 616088211, Idle2 = 616089559, Idle3 = 885531463, Walk = 616095330, Run = 616091570, Jump = 616090535, Climb = 616086039, Fall = 616087089, Swim = 616092998, SwimIdle = 616094091},
        ["Toy"] = {Idle = 782841498, Idle2 = 782845736, Idle3 = 980952228, Walk = 782843345, Run = 782842708, Jump = 782847020, Climb = 782843869, Fall = 782846423, Swim = 782844582, SwimIdle = 782845186},
        ["Cartoony"] = {Idle = 742637544, Idle2 = 742638445, Idle3 = 885477856, Walk = 742640026, Run = 742638842, Jump = 742637942, Climb = 742636889, Fall = 742637151, Swim = 742639220, SwimIdle = 742639812},
        ["Superhero"] = {Idle = 616111295, Idle2 = 616113536, Idle3 = 885535855, Walk = 616122287, Run = 616117076, Jump = 616115533, Climb = 616104706, Fall = 616108001, Swim = 616119360, SwimIdle = 616120861},
        ["Mage"] = {Idle = 707742142, Idle2 = 707855907, Idle3 = 885508740, Walk = 707897309, Run = 707861613, Jump = 707853694, Climb = 707826056, Fall = 707829716, Swim = 707876443, SwimIdle = 707894699},
        ["Levitation"] = {Idle = 616006778, Idle2 = 616008087, Idle3 = 886862142, Walk = 616013216, Run = 616010382, Jump = 616008936, Climb = 616003713, Fall = 616005863, Swim = 616011509, SwimIdle = 616012453},
        ["Vampire"] = {Idle = 1083445855, Idle2 = 1083450166, Idle3 = 1088037547, Walk = 1083473930, Run = 1083462077, Jump = 1083455352, Climb = 1083439238, Fall = 1083443587, Swim = 1083464683, SwimIdle = 1083467779},
        ["Elder"] = {Idle = 845397899, Idle2 = 845400520, Idle3 = 901160519, Walk = 845403856, Run = 845386501, Jump = 845398858, Climb = 845392038, Fall = 845396048, Swim = 845401742, SwimIdle = 845403127},
        ["Werewolf"] = {Idle = 1083195517, Idle2 = 1083214717, Idle3 = 1099492820, Walk = 1083178339, Run = 1083216690, Jump = 1083218792, Climb = 1083182000, Fall = 1083189019, Swim = 1083222527, SwimIdle = 1083225406},
        ["Knight"] = {Idle = 657595757, Idle2 = 657568135, Idle3 = 885499184, Walk = 657552124, Run = 657564596, Jump = 658409194, Climb = 658360781, Fall = 657600338, Swim = 657560551, SwimIdle = 657557095},
        ["Bold"] = {Idle = 16738333868, Idle2 = 16738334710, Idle3 = 16738335517, Walk = 16738340646, Run = 16738337225, Jump = 16738336650, Climb = 16738332169, Fall = 16738333171, Swim = 16738339158, SwimIdle = 16738339817},
        ["Astronaut"] = {Idle = 891621366, Idle2 = 891633237, Idle3 = 1047759695, Walk = 891667138, Run = 891636393, Jump = 891627522, Climb = 891609353, Fall = 891617961, Swim = 891639666, SwimIdle = 891663592},
        ["Bubbly"] = {Idle = 910004836, Idle2 = 910009958, Idle3 = 1018536639, Walk = 910034870, Run = 910025107, Jump = 910016857, Climb = 909997997, Fall = 910001910, Swim = 910028158, SwimIdle = 910030921},
        ["Pirate"] = {Idle = 750781874, Idle2 = 750782770, Idle3 = 885515365, Walk = 750785693, Run = 750783738, Jump = 750782230, Climb = 750779899, Fall = 750780242, Swim = 750784579, SwimIdle = 750785176},
        ["Rthro"] = {Idle = 2510196951, Idle2 = 2510197257, Idle3 = 3711062489, Walk = 2510202577, Run = 2510198475, Jump = 2510197830, Climb = 2510192778, Fall = 2510195892, Swim = 2510199791, SwimIdle = 2510201162},
        ["Ninja"] = {Idle = 656117400, Idle2 = 656118341, Idle3 = 886742569, Walk = 656121766, Run = 656118852, Jump = 656117878, Climb = 656114359, Fall = 656115606, Swim = 656119721, SwimIdle = 656121397},
        ["Oldschool"] = {Idle = 5319828216, Idle2 = 5319831086, Idle3 = 5392107832, Walk = 5319847204, Run = 5319844329, Jump = 5319841935, Climb = 5319816685, Fall = 5319839762, Swim = 5319850266, SwimIdle = 5319852613},
        ["Realistic"] = {Idle = 17172918855, Idle2 = 17173014241, Idle3 = 17173014241, Walk = 11600249883, Run = 11600211410, Jump = 11600210487, Climb = 11600205519, Fall = 11600206437, Swim = 11600212676, SwimIdle = 11600213505},
        ["No Boundaries"] = {Idle = 18747067405, Idle2 = 18747063918, Idle3 = 18747063918, Walk = 18747074203, Run = 18747070484, Jump = 18747069148, Climb = 18747060903, Fall = 18747062535, Swim = 18747073181, SwimIdle = 18747071682},
        ["NFL Animation"] = {Idle = 92080889861410, Idle2 = 74451233229259, Idle3 = 80884010501210, Walk = 110358958299415, Run = 117333533048078, Jump = 119846112151352, Climb = 134630013742019, Fall = 129773241321032, Swim = 132697394189921, SwimIdle = 79090109939093},
        ["Adidas Aura"] = {Idle = 110211186840347, Idle2 = 114191137265065, Idle3 = 99129837931148, Walk = 83842218823011, Run = 118320322718866, Jump = 109996626521204, Climb = 97824616490448, Fall = 95603166884636, Swim = 134530128383903, SwimIdle = 94922130551805},
        ["Adidas Sports"] = {Idle = 18537376492, Idle2 = 18537371272, Idle3 = 18537374150, Walk = 18537392113, Run = 18537384940, Jump = 18537380791, Climb = 18537363391, Fall = 18537367238, Swim = 18537389531, SwimIdle = 18537387180},
        ["Adidas Community"] = {Idle = 122257458498464, Idle2 = 102357151005774, Idle3 = 89262795687364, Walk = 122150855457006, Run = 82598234841035, Jump = 75290611992385, Climb = 88763136693023, Fall = 98600215928904, Swim = 133308483266208, SwimIdle = 109346520324160},
        ["Wickled Popular"] = {Idle = 118832222982049, Idle2 = 76049494037641, Idle3 = 138255200176080, Walk = 92072849924640, Run = 72301599441680, Jump = 104325245285198, Climb = 131326830509784, Fall = 121152442762481, Swim = 99384245425157, SwimIdle = 113199415118199},
        ["Catwalk Glam"] = {Idle = 133806214992291, Idle2 = 94970088341563, Idle3 = 87105332133518, Walk = 109168724482748, Run = 81024476153754, Jump = 116936326516985, Climb = 119377220967554, Fall = 92294537340807, Swim = 134591743181628, SwimIdle = 98854111361360},
        ["Princess"] = {Idle = 941003647, Idle2 = 941013098, Idle3 = 1159195712, Walk = 941028902, Run = 941015281, Jump = 941008832, Climb = 940996062, Fall = 941000007, Swim = 941018893, SwimIdle = 941025398},
        ["Confident"] = {Idle = 1069977950, Idle2 = 1069987858, Idle3 = 1116160740, Walk = 1070017263, Run = 1070001516, Jump = 1069984524, Climb = 1069946257, Fall = 1069973677, Swim = 1070009914, SwimIdle = 1070012133},
        ["Popstar"] = {Idle = 1212900985, Idle2 = 1150842221, Idle3 = 1239733474, Walk = 1212980338, Run = 1212980348, Jump = 1212954642, Climb = 1213044953, Fall = 1212900995, Swim = 1212852603, SwimIdle = 1070012133},
        ["Patrol"] = {Idle = 1149612882, Idle2 = 1150842221, Idle3 = 1159573567, Walk = 1151231493, Run = 1150967949, Jump = 1150944216, Climb = 1148811837, Fall = 1148863382, Swim = 1151204998, SwimIdle = 1151221899},
        ["Sneaky"] = {Idle = 1132473842, Idle2 = 1132477671, Idle3 = "None", Walk = 1132510133, Run = 1132494274, Jump = 1132489853, Climb = 1132461372, Fall = 1132469004, Swim = 1132500520, SwimIdle = 1132506407},
        ["Cowboy"] = {Idle = 1014390418, Idle2 = 1014398616, Idle3 = 1159487651, Walk = 1014421541, Run = 1014401683, Jump = 1014394726, Climb = 1014380606, Fall = 1014384571, Swim = 1014406523, SwimIdle = 1014411816},
        ["Ghost"] = {Idle = 616006778, Idle2 = 616008087, Idle3 = 616008087, Walk = 616013216, Run = 616013216, Jump = 616008936, Climb = 0, Fall = 616005863, Swim = 616011509, SwimIdle = 616012453},
        ["Ghost 2"] = {Idle = 1151221899, Idle2 = 1151221899, Idle3 = "None", Walk = 1151221899, Run = 1151221899, Jump = 1151221899, Climb = 0, Fall = 1151221899, Swim = 16738339158, SwimIdle = 1151221899},
        ["Mr. Toilet"] = {Idle = 4417977954, Idle2 = 4417978624, Idle3 = 4441285342, Walk = 2510202577, Run = 4417979645, Jump = 2510197830, Climb = 2510192778, Fall = 2510195892, Swim = 2510199791, SwimIdle = 2510201162},
        ["Udzal"] = {Idle = 3303162274, Idle2 = 3303162549, Idle3 = 3710161342, Walk = 3303162967, Run = 3236836670, Jump = 2510197830, Climb = 2510192778, Fall = 2510195892, Swim = 2510199791, SwimIdle = 2510201162},
        ["Oinan Thickhoof"] = {Idle = 657595757, Idle2 = 657568135, Idle3 = 885499184, Walk = 2510202577, Run = 3236836670, Jump = 2510197830, Climb = 2510192778, Fall = 2510195892, Swim = 2510199791, SwimIdle = 2510201162},
        ["Borock"] = {Idle = 3293641938, Idle2 = 3293642554, Idle3 = 3710131919, Walk = 2510202577, Run = 3236836670, Jump = 2510197830, Climb = 2510192778, Fall = 2510195892, Swim = 2510199791, SwimIdle = 2510201162},
        ["Blocky Mech"] = {Idle = 4417977954, Idle2 = 4417978624, Idle3 = 4441285342, Walk = 2510202577, Run = 4417979645, Jump = 2510197830, Climb = 2510192778, Fall = 2510195892, Swim = 2510199791, SwimIdle = 2510201162},
        ["Stylized Female"] = {Idle = 4708191566, Idle2 = 4708192150, Idle3 = 121221, Walk = 4708193840, Run = 4708192705, Jump = 4708188025, Climb = 4708184253, Fall = 4708186162, Swim = 4708189360, SwimIdle = 4708190607},
        ["R15"] = {Idle = 4211217646, Idle2 = 4211218409, Idle3 = "None", Walk = 4211223236, Run = 4211220381, Jump = 4211219390, Climb = 4211214992, Fall = 4211216152, Swim = 4211221314, SwimIdle = 4374694239},
        ["Mocap"] = {Idle = 913367814, Idle2 = 913373430, Idle3 = "None", Walk = 913402848, Run = 913376220, Jump = 913370268, Climb = 913362637, Fall = 913365531, Swim = 913384386, SwimIdle = 913389285},
        ['Wicked "Dancing Through Life"'] = {Idle = 92849173543269, Idle2 = 132238900951109, Idle3 = 87867222929430, Walk = 73718308412641, Run = 135515454877967, Jump = 78508480717326, Climb = 129447497744818, Fall = 78147885297412, Swim = 110657013921774, SwimIdle = 129183123083281},
        ["Unboxed"] = {Idle = 98281136301627, Idle2 = 138183121662404, Idle3 = 133117300343405, Walk = 90478085024465, Run = 134824450619865, Jump = 121454505477205, Climb = 121145883950231, Fall = 94788218468396, Swim = 105962919001086, SwimIdle = 129126268464847},

        -- ===== NOVOS PACOTES ADICIONADOS =====
        ["Sporty"] = {Idle = 3337994105, Idle2 = 3337994106, Idle3 = 3337994107, Walk = 3337994108, Run = 3337994109, Jump = 3337994110, Climb = 3337994111, Fall = 3337994112, Swim = 3337994113, SwimIdle = 3337994114},
        ["Scientist"] = {Idle = 3338097973, Idle2 = 3338097974, Idle3 = 3338097975, Walk = 3338097976, Run = 3338097977, Jump = 3338097978, Climb = 3338097979, Fall = 3338097980, Swim = 3338097981, SwimIdle = 3338097982},
        ["Explorer"] = {Idle = 3338066331, Idle2 = 3338066332, Idle3 = 3338066333, Walk = 3338066334, Run = 3338066335, Jump = 3338066336, Climb = 3338066337, Fall = 3338066338, Swim = 3338066339, SwimIdle = 3338066340},
        ["Builder"] = {Idle = 3338055167, Idle2 = 3338055168, Idle3 = 3338055169, Walk = 3338055170, Run = 3338055171, Jump = 3338055172, Climb = 3338055173, Fall = 3338055174, Swim = 3338055175, SwimIdle = 3338055176},
        ["Parkour"] = {Idle = 616006778, Idle2 = 616008087, Idle3 = 616008936, Walk = 616013216, Run = 616010382, Jump = 616008936, Climb = 616003713, Fall = 616005863, Swim = 616011509, SwimIdle = 616012453},
        ["Catwalk"] = {Idle = 3334424322, Idle2 = 3334424323, Idle3 = 3334424324, Walk = 3334424325, Run = 3334424326, Jump = 3334424327, Climb = 3334424328, Fall = 3334424329, Swim = 3334424330, SwimIdle = 3334424331},
        ["Alien"] = {Idle = 616088211, Idle2 = 616089559, Idle3 = 616090535, Walk = 616095330, Run = 616091570, Jump = 616090535, Climb = 616086039, Fall = 616087089, Swim = 616092998, SwimIdle = 616094091},
        ["Dragon"] = {Idle = 3337966527, Idle2 = 3337966528, Idle3 = 3337966529, Walk = 3337966530, Run = 3337966531, Jump = 3337966532, Climb = 3337966533, Fall = 3337966534, Swim = 3337966535, SwimIdle = 3337966536},
        ["Ninja Run"] = {Idle = 656117400, Idle2 = 656118341, Idle3 = 656117878, Walk = 656121766, Run = 656118852, Jump = 656117878, Climb = 656114359, Fall = 656115606, Swim = 656119721, SwimIdle = 656121397},
        ["Witch"] = {Idle = 707742142, Idle2 = 707855907, Idle3 = 707853694, Walk = 707897309, Run = 707861613, Jump = 707853694, Climb = 707826056, Fall = 707829716, Swim = 707876443, SwimIdle = 707894699},
        ["Gentleman"] = {Idle = 3334424322, Idle2 = 3334424323, Idle3 = 3334424324, Walk = 3334424325, Run = 3334424326, Jump = 3334424327, Climb = 3334424328, Fall = 3334424329, Swim = 3334424330, SwimIdle = 3334424331},
        ["Cyborg"] = {Idle = 616111295, Idle2 = 616113536, Idle3 = 616115533, Walk = 616122287, Run = 616117076, Jump = 616115533, Climb = 616104706, Fall = 616108001, Swim = 616119360, SwimIdle = 616120861}
    }

    -- ===== FUNÇÕES DE ANIMAÇÃO =====
    local function aplicarAtributosTrilhas()
        local char = game.Players.LocalPlayer.Character
        if char then
            local hum = char:FindFirstChildOfClass("Humanoid")
            if hum then
                for _, track in pairs(hum:GetPlayingAnimationTracks()) do
                    track:AdjustSpeed(configSpeed)
                end
            end
        end
    end

    local function PlayAnimation(id)
        local char = game.Players.LocalPlayer.Character
        if not char then return nil end
        local hum = char:FindFirstChildOfClass("Humanoid")
        if not hum then return nil end
        local anim = Instance.new("Animation")
        anim.AnimationId = "rbxassetid://" .. tostring(id)
        local track = hum:LoadAnimation(anim)
        if track then
            track.Looped = configLoop
            track:Play()
            track:AdjustSpeed(configSpeed)
        end
        return track
    end

    local function salvarOriginais()
        local char = game.Players.LocalPlayer.Character
        if not char then return end
        local anim = char:FindFirstChild("Animate")
        if not anim then return end
        if next(origAnims) then return end
        
        origAnims.idle1 = anim.idle.Animation1.AnimationId
        origAnims.idle2 = anim.idle.Animation2.AnimationId
        origAnims.walk = anim.walk:FindFirstChildOfClass("Animation").AnimationId
        origAnims.run = anim.run:FindFirstChildOfClass("Animation").AnimationId
        origAnims.jump = anim.jump:FindFirstChildOfClass("Animation").AnimationId
        origAnims.climb = anim.climb:FindFirstChildOfClass("Animation").AnimationId
        origAnims.fall = anim.fall:FindFirstChildOfClass("Animation").AnimationId
        if anim:FindFirstChild("swim") then
            origAnims.swim = anim.swim:FindFirstChildOfClass("Animation").AnimationId
            origAnims.swimidle = anim.swimidle:FindFirstChildOfClass("Animation").AnimationId
        end
    end

    local function resetarAnims()
        local char = game.Players.LocalPlayer.Character
        if not char then return end
        local anim = char:FindFirstChild("Animate")
        if not anim then return end
        
        if origAnims.idle1 then anim.idle.Animation1.AnimationId = origAnims.idle1 end
        if origAnims.idle2 then anim.idle.Animation2.AnimationId = origAnims.idle2 end
        if origAnims.walk then anim.walk:FindFirstChildOfClass("Animation").AnimationId = origAnims.walk end
        if origAnims.run then anim.run:FindFirstChildOfClass("Animation").AnimationId = origAnims.run end
        if origAnims.jump then anim.jump:FindFirstChildOfClass("Animation").AnimationId = origAnims.jump end
        if origAnims.climb then anim.climb:FindFirstChildOfClass("Animation").AnimationId = origAnims.climb end
        if origAnims.fall then anim.fall:FindFirstChildOfClass("Animation").AnimationId = origAnims.fall end
        if origAnims.swim and anim:FindFirstChild("swim") then
            anim.swim:FindFirstChildOfClass("Animation").AnimationId = origAnims.swim
            anim.swimidle:FindFirstChildOfClass("Animation").AnimationId = origAnims.swimidle or ""
        end
        
        anim.Disabled = true
        task.wait(0.05)
        anim.Disabled = false
    end

    local function tocarEmote(nome)
        local char = game.Players.LocalPlayer.Character
        if not char then return end
        local hum = char:FindFirstChildOfClass("Humanoid")
        if not hum then return end
        
        if emoteAtual == nome then
            for _, track in pairs(hum:GetPlayingAnimationTracks()) do track:Stop() end
            emoteAtual = nil
            return
        end
        
        for _, track in pairs(hum:GetPlayingAnimationTracks()) do track:Stop() end
        
        local id = Emotes[nome]
        if id then
            local track = PlayAnimation(id)
            if track then
                emoteAtual = nome
            end
        end
    end

    -- ============================================================
    -- ===== FUNÇÃO TOCAR ANIMAÇÃO CORRIGIDA SEM TRAVAR =====
    -- ============================================================
    local function tocarAnimacao(nome, forcar)
        local char = game.Players.LocalPlayer.Character
        if not char then return end
        
        -- Aguarda o Character carregar completamente
        char:WaitForChild("Humanoid")
        char:WaitForChild("Animate")
        
        local anim = char:FindFirstChild("Animate")
        if not anim then return end
        
        -- Se clicou na mesma animação, reseta (apenas se não for forçado)
        if not forcar and animAtual == nome then
            resetarAnims()
            animAtual = nil
            return
        end
        
        local data = Animations[nome]
        if not data then return end
        
        -- Para todas as tracks atuais
        local hum = char:FindFirstChildOfClass("Humanoid")
        if hum then
            for _, track in pairs(hum:GetPlayingAnimationTracks()) do
                track:Stop()
            end
        end
        
        -- Salva originais se não tiver salvo ainda
        salvarOriginais()
        
        local URL = "http://www.roblox.com/asset/?id="
        
        -- Aplica IDLE
        if anim:FindFirstChild("idle") then
            local idle1 = anim.idle:FindFirstChild("Animation1")
            local idle2 = anim.idle:FindFirstChild("Animation2")
            if idle1 then
                idle1.AnimationId = URL .. tostring(data.Idle)
                idle1.Weight.Value = 9
            end
            if idle2 then
                idle2.AnimationId = URL .. tostring(data.Idle2)
                idle2.Weight.Value = 1
            end
        end
        
        -- Aplica POSE (Idle3)
        if data.Idle3 and data.Idle3 ~= "None" and anim:FindFirstChild("pose") then
            local pose = anim.pose:FindFirstChildOfClass("Animation")
            if pose then
                pose.AnimationId = URL .. tostring(data.Idle3)
            end
        end
        
        -- Aplica WALK
        local walkAnim = anim:FindFirstChild("walk")
        if walkAnim then
            local walk = walkAnim:FindFirstChildOfClass("Animation")
            if walk then
                walk.AnimationId = URL .. tostring(data.Walk)
            end
        end
        
        -- Aplica RUN
        local runAnim = anim:FindFirstChild("run")
        if runAnim then
            local run = runAnim:FindFirstChildOfClass("Animation")
            if run then
                run.AnimationId = URL .. tostring(data.Run)
            end
        end
        
        -- Aplica JUMP
        local jumpAnim = anim:FindFirstChild("jump")
        if jumpAnim then
            local jump = jumpAnim:FindFirstChildOfClass("Animation")
            if jump then
                jump.AnimationId = URL .. tostring(data.Jump)
            end
        end
        
        -- Aplica CLIMB
        local climbAnim = anim:FindFirstChild("climb")
        if climbAnim then
            local climb = climbAnim:FindFirstChildOfClass("Animation")
            if climb then
                climb.AnimationId = URL .. tostring(data.Climb)
            end
        end
        
        -- Aplica FALL
        local fallAnim = anim:FindFirstChild("fall")
        if fallAnim then
            local fall = fallAnim:FindFirstChildOfClass("Animation")
            if fall then
                fall.AnimationId = URL .. tostring(data.Fall)
            end
        end
        
            -- Aplica SWIM e SWIMIDLE
    if anim:FindFirstChild("swim") then
        local swim = anim.swim:FindFirstChildOfClass("Animation")
        local swimIdle = anim.swimidle:FindFirstChildOfClass("Animation")
        if swim then
            swim.AnimationId = URL .. tostring(data.Swim)
        end
        if swimIdle then
            swimIdle.AnimationId = URL .. tostring(data.SwimIdle)
        end
    end
    
    -- Força o Humanoid a reavaliar o estado atual (sem desligar o Animate)
    local hum = char:FindFirstChildOfClass("Humanoid")
    if hum then
        local estadoAtual = hum:GetState()
        hum:ChangeState(Enum.HumanoidStateType.RunningNoPhysics)
        task.wait(0.02)
        hum:ChangeState(estadoAtual)
    end
    
    animAtual = nome
    task.delay(0.2, aplicarAtributosTrilhas)
end

    -- ============================================================
    -- ===== CRIAÇÃO DA JANELA PRINCIPAL (Nero Style) =====
    -- ============================================================
    local function criarJanela()
        if guiJanela then
            guiJanela:Destroy()
            guiJanela = nil
            framePrincipal = nil
        end

        guiJanela = Instance.new("ScreenGui")
        guiJanela.Name = "NeroCorpoAnimationsUI"
        guiJanela.ResetOnSpawn = false
        guiJanela.Parent = game:GetService("CoreGui")
        guiJanela.ZIndexBehavior = Enum.ZIndexBehavior.Sibling

        framePrincipal = Instance.new("CanvasGroup")
        framePrincipal.Size = UDim2.new(0, 420, 0, 310)
        framePrincipal.Position = UDim2.new(0.5, -210, 0.5, -155)
        framePrincipal.BackgroundColor3 = Cores.BgPrincipal
        framePrincipal.BorderSizePixel = 0
        framePrincipal.ClipsDescendants = true
        framePrincipal.Parent = guiJanela

        local strokePrincipal = Instance.new("UIStroke")
        strokePrincipal.Color = Cores.BorderGlow
        strokePrincipal.Thickness = 2
        strokePrincipal.Parent = framePrincipal

        local cornerPrincipal = Instance.new("UICorner")
        cornerPrincipal.CornerRadius = UDim.new(0, 20)
        cornerPrincipal.Parent = framePrincipal

        -- .topbar
        local topbar = Instance.new("Frame")
        topbar.Size = UDim2.new(1, 0, 0, 38)
        topbar.BackgroundColor3 = Cores.BgTop
        topbar.BorderSizePixel = 0
        topbar.Parent = framePrincipal

        local topbarBottomBorder = Instance.new("Frame")
        topbarBottomBorder.Size = UDim2.new(1, 0, 0, 1)
        topbarBottomBorder.Position = UDim2.new(0, 0, 1, -1)
        topbarBottomBorder.BackgroundColor3 = Cores.Laranja
        topbarBottomBorder.BackgroundTransparency = 0.75
        topbarBottomBorder.BorderSizePixel = 0
        topbarBottomBorder.Parent = topbar

        local titulo = Instance.new("TextLabel")
        titulo.Size = UDim2.new(1, -60, 1, 0)
        titulo.Position = UDim2.new(0, 24, 0, 0)
        titulo.BackgroundTransparency = 1
        titulo.Text = "🎭 Corpo Animations"
        titulo.TextColor3 = Cores.LaranjaVib
        titulo.TextSize = 13
        titulo.Font = Enum.Font.GothamBold
        titulo.TextXAlignment = Enum.TextXAlignment.Left
        titulo.Parent = topbar

        local btnMinimizar = Instance.new("TextButton")
        btnMinimizar.Size = UDim2.new(0, 26, 0, 26)
        btnMinimizar.Position = UDim2.new(1, -34, 0, 6)
        btnMinimizar.BackgroundColor3 = Cores.Laranja
        btnMinimizar.BackgroundTransparency = 0.88
        btnMinimizar.Text = "_"
        btnMinimizar.TextColor3 = Cores.LaranjaVib
        btnMinimizar.TextSize = 15
        btnMinimizar.Font = Enum.Font.GothamBold
        btnMinimizar.Parent = topbar
        
        local cornerBtnMin = Instance.new("UICorner")
        cornerBtnMin.CornerRadius = UDim.new(0, 6)
        cornerBtnMin.Parent = btnMinimizar
        
        local strokeBtnMin = Instance.new("UIStroke")
        strokeBtnMin.Color = Cores.Laranja
        strokeBtnMin.Transparency = 0.6
        strokeBtnMin.Thickness = 1
        strokeBtnMin.Parent = btnMinimizar

        -- .content
        local content = Instance.new("Frame")
        content.Size = UDim2.new(1, -24, 1, -74)
        content.Position = UDim2.new(0, 12, 0, 48)
        content.BackgroundTransparency = 1
        content.Parent = framePrincipal

        -- .abas (Container)
        local abas = Instance.new("Frame")
        abas.Size = UDim2.new(1, 0, 0, 32)
        abas.BackgroundColor3 = Cores.BgSecundario
        abas.BorderSizePixel = 0
        abas.Parent = content

        local cornerAbas = Instance.new("UICorner")
        cornerAbas.CornerRadius = UDim.new(0, 10)
        cornerAbas.Parent = abas
        
        local strokeAbas = Instance.new("UIStroke")
        strokeAbas.Color = Cores.Laranja
        strokeAbas.Transparency = 0.85
        strokeAbas.Thickness = 1
        strokeAbas.Parent = abas

        local listAbas = Instance.new("UIListLayout")
        listAbas.FillDirection = Enum.FillDirection.Horizontal
        listAbas.HorizontalAlignment = Enum.HorizontalAlignment.Center
        listAbas.VerticalAlignment = Enum.VerticalAlignment.Center
        listAbas.Padding = UDim.new(0, 4)
        listAbas.Parent = abas

        -- Páginas Container
        local paginas = Instance.new("Frame")
        paginas.Size = UDim2.new(1, 0, 1, -42)
        paginas.Position = UDim2.new(0, 0, 0, 42)
        paginas.BackgroundTransparency = 1
        paginas.Parent = content

        local btnAbas = {}
        local framesPaginas = {}

        local function criarAba(nome, index)
            local aba = Instance.new("TextButton")
            aba.Size = UDim2.new(0.33, -3, 1, -6)
            aba.BackgroundColor3 = Cores.Texto
            aba.BackgroundTransparency = 1
            aba.Text = nome
            aba.TextColor3 = Cores.TextoMuted
            aba.TextSize = 11
            aba.Font = Enum.Font.GothamBold
            aba.Parent = abas
            
            local cornerAba = Instance.new("UICorner")
            cornerAba.CornerRadius = UDim.new(0, 7)
            cornerAba.Parent = aba

            local grad = Instance.new("UIGradient")
            grad.Color = ColorSequence.new{
                ColorSequenceKeypoint.new(0, Cores.LaranjaVib),
                ColorSequenceKeypoint.new(1, Cores.LaranjaEscuro)
            }
            grad.Rotation = 90
            grad.Enabled = false
            grad.Parent = aba

            local pagina = Instance.new("ScrollingFrame")
            pagina.Size = UDim2.new(1, 0, 1, 0)
            pagina.BackgroundTransparency = 1
            pagina.BorderSizePixel = 0
            pagina.ScrollBarThickness = 4
            pagina.ScrollBarImageColor3 = Cores.Laranja
            pagina.Visible = false
            pagina.Parent = paginas

            if index == 3 then
                local listLayout = Instance.new("UIListLayout", pagina)
                listLayout.SortOrder = Enum.SortOrder.LayoutOrder
                listLayout.Padding = UDim.new(0, 10)
            else
                local grid = Instance.new("UIGridLayout", pagina)
                grid.CellSize = UDim2.new(0.333, -6, 0, 38)
                grid.CellPadding = UDim2.new(0, 8, 0, 8)
                grid.SortOrder = Enum.SortOrder.LayoutOrder
            end

            btnAbas[index] = aba
            framesPaginas[index] = pagina

            aba.MouseButton1Click:Connect(function()
                for i, a in ipairs(btnAbas) do
                    a.BackgroundTransparency = 1
                    a.TextColor3 = Cores.TextoMuted
                    a:FindFirstChildOfClass("UIGradient").Enabled = false
                    framesPaginas[i].Visible = false
                end
                aba.BackgroundTransparency = 0
                aba.TextColor3 = Cores.BgPrincipal
                aba:FindFirstChildOfClass("UIGradient").Enabled = true
                pagina.Visible = true
            end)

            return pagina
        end

        local pageEmotes = criarAba("EMOTES", 1)
        local pageAnims = criarAba("ANIMAÇÕES", 2)
        local pageConfig = criarAba("CONFIG", 3)

        -- ===== STATUSBAR =====
        local statusbar = Instance.new("Frame")
        statusbar.Size = UDim2.new(1, 0, 0, 24)
        statusbar.Position = UDim2.new(0, 0, 1, -24)
        statusbar.BackgroundColor3 = Cores.BgTop
        statusbar.BorderSizePixel = 0
        statusbar.Parent = framePrincipal

        local statusTopBorder = Instance.new("Frame")
        statusTopBorder.Size = UDim2.new(1, 0, 0, 1)
        statusTopBorder.BackgroundColor3 = Cores.Laranja
        statusTopBorder.BackgroundTransparency = 0.8
        statusTopBorder.BorderSizePixel = 0
        statusTopBorder.Parent = statusbar

        local statusText = Instance.new("TextLabel")
        statusText.Size = UDim2.new(1, -48, 1, 0)
        statusText.Position = UDim2.new(0, 24, 0, 0)
        statusText.BackgroundTransparency = 1
        statusText.Text = "📦 Emotes: 262   🎬 Animações: 57"
        statusText.TextColor3 = Cores.LaranjaVib
        statusText.TextSize = 10
        statusText.Font = Enum.Font.GothamMedium
        statusText.TextXAlignment = Enum.TextXAlignment.Left
        statusText.Parent = statusbar

        local statusHint = Instance.new("TextLabel")
        statusHint.Size = UDim2.new(0, 100, 1, 0)
        statusHint.Position = UDim2.new(1, -124, 0, 0)
        statusHint.BackgroundTransparency = 1
        statusHint.Text = "arraste a barra superior"
        statusHint.TextColor3 = Cores.TextoMuted
        statusHint.TextSize = 9
        statusHint.Font = Enum.Font.Gotham
        statusHint.TextXAlignment = Enum.TextXAlignment.Right
        statusHint.Parent = statusbar

        -- ===== CRIAÇÃO DE CARDS =====
        local activeCards = {}
        local function criarCartao(parent, texto, tipo)
            local cartao = Instance.new("TextButton")
            cartao.BackgroundColor3 = Cores.BgCartao
            cartao.Text = texto
            cartao.TextColor3 = Cores.Texto
            cartao.TextSize = 10
            cartao.Font = Enum.Font.GothamSemibold
            cartao.TextScaled = true
            cartao.Parent = parent

            local constraint = Instance.new("UITextSizeConstraint", cartao)
            constraint.MaxTextSize = 11
            constraint.MinTextSize = 7

            local padding = Instance.new("UIPadding", cartao)
            padding.PaddingLeft = UDim.new(0, 4)
            padding.PaddingRight = UDim.new(0, 4)
            padding.PaddingTop = UDim.new(0, 4)
            padding.PaddingBottom = UDim.new(0, 4)

            local corner = Instance.new("UICorner")
            corner.CornerRadius = UDim.new(0, 8)
            corner.Parent = cartao

            local stroke = Instance.new("UIStroke")
            stroke.Color = Cores.Texto
            stroke.Transparency = 0.95
            stroke.Thickness = 1
            stroke.Parent = cartao

            local grad = Instance.new("UIGradient")
            grad.Color = ColorSequence.new{
                ColorSequenceKeypoint.new(0, Cores.LaranjaVib),
                ColorSequenceKeypoint.new(1, Cores.LaranjaEscuro)
            }
            grad.Rotation = 90
            grad.Enabled = false
            grad.Parent = cartao

            if (tipo == "Emote" and emoteAtual == texto) or (tipo == "Anim" and animAtual == texto) then
                cartao.BackgroundTransparency = 0
                cartao.TextColor3 = Cores.BgPrincipal
                cartao.Font = Enum.Font.GothamBold
                grad.Enabled = true
                stroke.Color = Cores.Laranja
                stroke.Transparency = 0
                activeCards[tipo] = cartao
            end

            cartao.MouseButton1Click:Connect(function()
                if activeCards[tipo] then
                    activeCards[tipo].BackgroundTransparency = 0
                    activeCards[tipo].TextColor3 = Cores.Texto
                    activeCards[tipo].Font = Enum.Font.GothamSemibold
                    activeCards[tipo]:FindFirstChildOfClass("UIGradient").Enabled = false
                    activeCards[tipo]:FindFirstChildOfClass("UIStroke").Color = Cores.Texto
                    activeCards[tipo]:FindFirstChildOfClass("UIStroke").Transparency = 0.95
                end
                
                cartao.BackgroundTransparency = 0
                cartao.TextColor3 = Cores.BgPrincipal
                cartao.Font = Enum.Font.GothamBold
                grad.Enabled = true
                stroke.Color = Cores.Laranja
                stroke.Transparency = 0
                
                activeCards[tipo] = cartao

                if tipo == "Emote" then
                    tocarEmote(texto)
                else
                    tocarAnimacao(texto)
                end
            end)
        end

        local emoteNames = {}
        for nome, _ in pairs(Emotes) do table.insert(emoteNames, nome) end
        table.sort(emoteNames, function(a, b) return a:lower() < b:lower() end)
        for _, nome in ipairs(emoteNames) do
            criarCartao(pageEmotes, nome, "Emote")
        end
        pageEmotes.CanvasSize = UDim2.new(0, 0, 0, math.ceil(#emoteNames/3) * (38 + 8) + 8)

        local animNames = {}
        for nome, _ in pairs(Animations) do table.insert(animNames, nome) end
        table.sort(animNames, function(a, b) return a:lower() < b:lower() end)
        for _, nome in ipairs(animNames) do
            criarCartao(pageAnims, nome, "Anim")
        end
        pageAnims.CanvasSize = UDim2.new(0, 0, 0, math.ceil(#animNames/3) * (38 + 8) + 8)

        -- ===== CRIAÇÃO DA ABA CONFIGURAÇÕES =====
        local function criarLinhaConfig(titulo, subtitulo)
            local row = Instance.new("Frame")
            row.Size = UDim2.new(1, 0, 0, 52)
            row.BackgroundColor3 = Cores.BgSecundario
            row.Parent = pageConfig

            local stroke = Instance.new("UIStroke")
            stroke.Color = Cores.Laranja
            stroke.Transparency = 0.8
            stroke.Thickness = 1
            stroke.Parent = row

            local corner = Instance.new("UICorner")
            corner.CornerRadius = UDim.new(0, 10)
            corner.Parent = row
            
            local lblTit = Instance.new("TextLabel", row)
            lblTit.Size = UDim2.new(0.6, 0, 0, 16)
            lblTit.Position = UDim2.new(0, 14, 0, 10)
            lblTit.BackgroundTransparency = 1
            lblTit.Text = titulo
            lblTit.TextColor3 = Cores.Texto
            lblTit.TextSize = 12
            lblTit.Font = Enum.Font.GothamBold
            lblTit.TextXAlignment = Enum.TextXAlignment.Left

            local lblSub = Instance.new("TextLabel", row)
            lblSub.Size = UDim2.new(0.6, 0, 0, 14)
            lblSub.Position = UDim2.new(0, 14, 0, 26)
            lblSub.BackgroundTransparency = 1
            lblSub.Text = subtitulo
            lblSub.TextColor3 = Cores.TextoMuted
            lblSub.TextSize = 10
            lblSub.Font = Enum.Font.Gotham
            lblSub.TextXAlignment = Enum.TextXAlignment.Left

            return row
        end

        local rowLoop = criarLinhaConfig("Loop das Animações", "Repetir automaticamente ao terminar")
        
        local switchBg = Instance.new("TextButton", rowLoop)
        switchBg.Size = UDim2.new(0, 44, 0, 22)
        switchBg.Position = UDim2.new(1, -58, 0.5, -11)
        switchBg.BackgroundColor3 = configLoop and Cores.Laranja or Color3.fromRGB(42, 42, 53)
        switchBg.Text = ""
        local switchCorner = Instance.new("UICorner", switchBg)
        switchCorner.CornerRadius = UDim.new(0, 11)

        local switchKnob = Instance.new("Frame", switchBg)
        switchKnob.Size = UDim2.new(0, 16, 0, 16)
        switchKnob.Position = configLoop and UDim2.new(0, 25, 0, 3) or UDim2.new(0, 3, 0, 3)
        switchKnob.BackgroundColor3 = configLoop and Cores.BgPrincipal or Cores.Texto
        local knobCorner = Instance.new("UICorner", switchKnob)
        knobCorner.CornerRadius = UDim.new(1, 0)

        switchBg.MouseButton1Click:Connect(function()
            configLoop = not configLoop
            if configLoop then
                switchBg:TweenSizeAndPosition(UDim2.new(0, 44, 0, 22), UDim2.new(1, -58, 0.5, -11), "Out", "Quad", 0.2, true)
                switchBg.BackgroundColor3 = Cores.Laranja
                switchKnob:TweenPosition(UDim2.new(0, 25, 0, 3), "Out", "Quad", 0.2, true)
                switchKnob.BackgroundColor3 = Cores.BgPrincipal
            else
                switchBg.BackgroundColor3 = Color3.fromRGB(42, 42, 53)
                switchKnob:TweenPosition(UDim2.new(0, 3, 0, 3), "Out", "Quad", 0.2, true)
                switchKnob.BackgroundColor3 = Cores.Texto
            end
        end)

        local rowSpeed = criarLinhaConfig("Velocidade da Animação", "Ajuste a velocidade de execução")

        local sliderWrap = Instance.new("Frame", rowSpeed)
        sliderWrap.Size = UDim2.new(0, 140, 1, 0)
        sliderWrap.Position = UDim2.new(1, -154, 0, 0)
        sliderWrap.BackgroundTransparency = 1

        local valTxt = Instance.new("TextLabel", sliderWrap)
        valTxt.Size = UDim2.new(0, 32, 1, 0)
        valTxt.Position = UDim2.new(1, 0, 0, 0)
        valTxt.BackgroundTransparency = 1
        valTxt.Text = string.format("%.1fx", configSpeed)
        valTxt.TextColor3 = Cores.LaranjaVib
        valTxt.TextSize = 11
        valTxt.Font = Enum.Font.GothamBold
        valTxt.TextXAlignment = Enum.TextXAlignment.Right

        local sliderBg = Instance.new("TextButton", sliderWrap)
        sliderBg.Size = UDim2.new(1, -36, 0, 6)
        sliderBg.Position = UDim2.new(0, 0, 0.5, -3)
        sliderBg.BackgroundColor3 = Color3.fromRGB(42, 42, 53)
        sliderBg.Text = ""
        sliderBg.AutoButtonColor = false
        local sbgCorner = Instance.new("UICorner", sliderBg)
        sbgCorner.CornerRadius = UDim.new(0, 3)

        local sliderFill = Instance.new("Frame", sliderBg)
        local initialPercent = (configSpeed - 0.2) / 2.8
        sliderFill.Size = UDim2.new(initialPercent, 0, 1, 0)
        sliderFill.BackgroundColor3 = Cores.LaranjaVib
        local fillCorner = Instance.new("UICorner", sliderFill)
        fillCorner.CornerRadius = UDim.new(0, 3)

        local sliderKnobUI = Instance.new("Frame", sliderBg)
        sliderKnobUI.Size = UDim2.new(0, 14, 0, 14)
        sliderKnobUI.Position = UDim2.new(initialPercent, -7, 0.5, -7)
        sliderKnobUI.BackgroundColor3 = Cores.LaranjaVib
        local snobCorner = Instance.new("UICorner", sliderKnobUI)
        snobCorner.CornerRadius = UDim.new(1, 0)

        local isDragging = false
        local function updateSlider(input)
            local rx = math.clamp(input.Position.X - sliderBg.AbsolutePosition.X, 0, sliderBg.AbsoluteSize.X)
            local percent = rx / sliderBg.AbsoluteSize.X
            sliderKnobUI.Position = UDim2.new(percent, -7, 0.5, -7)
            sliderFill.Size = UDim2.new(percent, 0, 1, 0)
            
            local val = 0.2 + (percent * 2.8)
            configSpeed = math.floor(val * 10) / 10 
            valTxt.Text = string.format("%.1fx", configSpeed)
            
            aplicarAtributosTrilhas()
        end

        sliderBg.InputBegan:Connect(function(input)
            if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then
                isDragging = true
                updateSlider(input)
            end
        end)
        
        sliderBg.InputEnded:Connect(function(input)
            if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then
                isDragging = false
            end
        end)
        
        game:GetService("UserInputService").InputChanged:Connect(function(input)
            if isDragging and (input.UserInputType == Enum.UserInputType.MouseMovement or input.UserInputType == Enum.UserInputType.Touch) then
                updateSlider(input)
            end
        end)

        -- ===== NOVA LINHA: MANTER ANIMAÇÕES =====
        local rowManter = criarLinhaConfig("Manter Animações", "Reaplicar após renascer")

        local manterBg = Instance.new("TextButton", rowManter)
        manterBg.Size = UDim2.new(0, 44, 0, 22)
        manterBg.Position = UDim2.new(1, -58, 0.5, -11)
        manterBg.BackgroundColor3 = manterAnimacoes and Cores.Laranja or Color3.fromRGB(42, 42, 53)
        manterBg.Text = ""
        local manterCorner = Instance.new("UICorner", manterBg)
        manterCorner.CornerRadius = UDim.new(0, 11)

        local manterKnob = Instance.new("Frame", manterBg)
        manterKnob.Size = UDim2.new(0, 16, 0, 16)
        manterKnob.Position = manterAnimacoes and UDim2.new(0, 25, 0, 3) or UDim2.new(0, 3, 0, 3)
        manterKnob.BackgroundColor3 = manterAnimacoes and Cores.BgPrincipal or Cores.Texto
        local manterKnobCorner = Instance.new("UICorner", manterKnob)
        manterKnobCorner.CornerRadius = UDim.new(1, 0)

        manterBg.MouseButton1Click:Connect(function()
            manterAnimacoes = not manterAnimacoes
            if manterAnimacoes then
                manterBg.BackgroundColor3 = Cores.Laranja
                manterKnob:TweenPosition(UDim2.new(0, 25, 0, 3), "Out", "Quad", 0.2, true)
                manterKnob.BackgroundColor3 = Cores.BgPrincipal
            else
                manterBg.BackgroundColor3 = Color3.fromRGB(42, 42, 53)
                manterKnob:TweenPosition(UDim2.new(0, 3, 0, 3), "Out", "Quad", 0.2, true)
                manterKnob.BackgroundColor3 = Cores.Texto
            end
        end)

        pageConfig.CanvasSize = UDim2.new(0, 0, 0, 180)

        btnAbas[1].BackgroundTransparency = 0
        btnAbas[1].TextColor3 = Cores.BgPrincipal
        btnAbas[1]:FindFirstChildOfClass("UIGradient").Enabled = true
        framesPaginas[1].Visible = true

        -- ===== LÓGICA DE MINIMIZAR =====
        btnMinimizar.MouseButton1Click:Connect(function()
            minimizado = not minimizado
            if minimizado then
                content.Visible = false
                statusbar.Visible = false
                topbarBottomBorder.Visible = false
                framePrincipal:TweenSize(UDim2.new(0, 420, 0, 38), "Out", "Quad", 0.28, true)
                cornerPrincipal.CornerRadius = UDim.new(0, 20)
                btnMinimizar.Text = "□"
            else
                framePrincipal:TweenSize(UDim2.new(0, 420, 0, 310), "Out", "Quad", 0.28, true)
                cornerPrincipal.CornerRadius = UDim.new(0, 20)
                btnMinimizar.Text = "_"
                topbarBottomBorder.Visible = true
                task.delay(0.2, function()
                    if not minimizado and guiJanela then
                        content.Visible = true
                        statusbar.Visible = true
                    end
                end)
            end
        end)

        -- ===== ARRASTAR A JANELA =====
        local draggingWindow = false
        local dragStartPos = nil
        local startPosUI = nil

        topbar.InputBegan:Connect(function(input)
            if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then
                draggingWindow = true
                dragStartPos = input.Position
                startPosUI = framePrincipal.Position

                input.Changed:Connect(function()
                    if input.UserInputState == Enum.UserInputState.End then
                        draggingWindow = false
                    end
                end)
            end
        end)

        topbar.InputChanged:Connect(function(input)
            if input.UserInputType == Enum.UserInputType.MouseMovement or input.UserInputType == Enum.UserInputType.Touch then
                if draggingWindow then
                    local delta = input.Position - dragStartPos
                    framePrincipal.Position = UDim2.new(startPosUI.X.Scale, startPosUI.X.Offset + delta.X, startPosUI.Y.Scale, startPosUI.Y.Offset + delta.Y)
                end
            end
        end)
    end

    -- ===== TOGGLE PRINCIPAL =====
    AnimBtn.MouseButton1Click:Connect(function()
        janelaAberta = not janelaAberta
        updateToggle(AnimTog, AnimKnob, janelaAberta)

        if janelaAberta then
            minimizado = false
            criarJanela()
            if Notify then Notify("🎭 Corpo Animations ativado!") end
        else
            if guiJanela then
                guiJanela:Destroy()
                guiJanela = nil
                framePrincipal = nil
            end
            if Notify then Notify("❌ Corpo Animations desativado!") end
        end
    end)

    -- ===== CONEXÃO PARA REAPLICAR ANIMAÇÃO APÓS RENASCER (SIMPLES E DIRETO) =====
local charAddedConn
charAddedConn = game.Players.LocalPlayer.CharacterAdded:Connect(function(char)
    if manterAnimacoes and animAtual then
        task.spawn(function()
            -- Aguarda um pouco para o Roblox carregar tudo
            char:WaitForChild("Humanoid")
            char:WaitForChild("Animate")
            task.wait(0.3)

            -- Chama a mesma função que o clique do botão usa, 3 vezes para garantir
            for i = 1, 3 do
                tocarAnimacao(animAtual, true)
                task.wait(0.1)
            end
        end)
    end
end)

    -- ===== LIMPEZA PARA QUANDO FECHAR O SCRIPT =====
    local function cleanup()
        if guiJanela then
            guiJanela:Destroy()
            guiJanela = nil
            framePrincipal = nil
        end
        if charAddedConn then
            charAddedConn:Disconnect()
            charAddedConn = nil
        end
    end

    table.insert(NERO.Connections, {Disconnect = cleanup})
end)
-- ==========================================================
-- TOGGLE: SUPER RING PARTS (ABA 2 - TROLLING)
-- ==========================================================
task.spawn(function()
    task.wait(0.1)
    
    -- CRIA O TOGGLE NA ABA 2 (TROLLING) - SEM EMOJI
    local RingTog, RingKnob, RingBtn = createToggle("Super Ring Parts V6", tabContainers[2], 0)
    
    -- ===== VARIÁVEIS DE CONTROLE =====
    local ringAtivo = false
    local ringInstancias = {
        gui = nil,
        folder = nil,
        connections = {},
        parts = {},
        sound = nil,
        config = nil
    }
    
    -- ===== FUNÇÃO PARA LIMPAR TUDO =====
    local function limparRing()
        if ringInstancias.gui then
            pcall(function() ringInstancias.gui:Destroy() end)
            ringInstancias.gui = nil
        end
        if ringInstancias.folder then
            pcall(function() ringInstancias.folder:Destroy() end)
            ringInstancias.folder = nil
        end
        for _, conn in pairs(ringInstancias.connections) do
            pcall(function() conn:Disconnect() end)
        end
        ringInstancias.connections = {}
        ringInstancias.parts = {}
        if ringInstancias.sound then
            pcall(function() ringInstancias.sound:Stop() end)
            pcall(function() ringInstancias.sound:Destroy() end)
            ringInstancias.sound = nil
        end
        if getgenv().Network then
            getgenv().Network.BaseParts = {}
            getgenv().Network.Velocity = nil
        end
        for _, v in pairs(game:GetService("Lighting"):GetChildren()) do
            if v:IsA("BloomEffect") or v:IsA("ColorCorrectionEffect") then
                pcall(function() v:Destroy() end)
            end
        end
        ringAtivo = false
    end
    
    -- ===== FUNÇÃO PARA ATIVAR O RING =====
    local function ativarRing()
        if ringAtivo then return end
        
        -- ==================================================
        -- CÓDIGO DO SUPER RING PARTS (ORIGINAL, INTACTO)
        -- ==================================================
        
        local Players = game:GetService("Players")
        local RunService = game:GetService("RunService")
        local UserInputService = game:GetService("UserInputService")
        local SoundService = game:GetService("SoundService")
        local StarterGui = game:GetService("StarterGui")
        local HttpService = game:GetService("HttpService")
        local Workspace = game:GetService("Workspace")
        local Lighting = game:GetService("Lighting")
        
        local LocalPlayer = Players.LocalPlayer
        
        -- ===== SOUND =====
        local function playSound(soundId)
            local sound = Instance.new("Sound")
            sound.SoundId = "rbxassetid://" .. soundId
            sound.Parent = SoundService
            sound:Play()
            sound.Ended:Connect(function() sound:Destroy() end)
            return sound
        end
        
        -- ===== CONFIG =====
        local config = {
            radius = 50,
            height = 100,
            rotationSpeed = 10,
            attractionStrength = 1000
        }
        ringInstancias.config = config
        
        -- ===== CORES =====
        local BLACK = Color3.fromRGB(0, 0, 0)
        local DARK = Color3.fromRGB(15, 15, 15)
        local DARKER = Color3.fromRGB(26, 26, 26)
        local ORANGE = Color3.fromRGB(255, 100, 0)
        local ORANGE_LIGHT = Color3.fromRGB(255, 133, 51)
        local WHITE = Color3.fromRGB(255, 255, 255)
        local GRAY = Color3.fromRGB(160, 160, 170)
        
        -- ===== GUI =====
        local ScreenGui = Instance.new("ScreenGui")
        ScreenGui.Name = "SuperRingPartsGUI"
        ScreenGui.ResetOnSpawn = false
        ScreenGui.ZIndexBehavior = Enum.ZIndexBehavior.Sibling
        ScreenGui.Parent = LocalPlayer:WaitForChild("PlayerGui")
        ringInstancias.gui = ScreenGui
        
        local MainFrame = Instance.new("Frame")
        MainFrame.Name = "MainFrame"
        MainFrame.Size = UDim2.new(0, 280, 0, 270)
        MainFrame.Position = UDim2.new(0.5, -140, 0.5, -135)
        MainFrame.BackgroundColor3 = BLACK
        MainFrame.BorderSizePixel = 0
        MainFrame.ClipsDescendants = true
        MainFrame.Parent = ScreenGui
        
        local MainCorner = Instance.new("UICorner")
        MainCorner.CornerRadius = UDim.new(0, 16)
        MainCorner.Parent = MainFrame
        
        local MainStroke = Instance.new("UIStroke")
        MainStroke.Color = ORANGE
        MainStroke.Thickness = 1
        MainStroke.ApplyStrokeMode = Enum.ApplyStrokeMode.Border
        MainStroke.Parent = MainFrame
        
        -- TÍTULO
        local Title = Instance.new("TextLabel")
        Title.Name = "Title"
        Title.Size = UDim2.new(1, 0, 0, 36)
        Title.Position = UDim2.new(0, 0, 0, 0)
        Title.BackgroundColor3 = DARK
        Title.BorderSizePixel = 0
        Title.Text = "Super Ring Parts V6"
        Title.TextColor3 = WHITE
        Title.TextSize = 17
        Title.Font = Enum.Font.GothamSemibold
        Title.TextXAlignment = Enum.TextXAlignment.Center
        Title.TextYAlignment = Enum.TextYAlignment.Center
        Title.ZIndex = 2
        Title.Parent = MainFrame
        
        local TitleCorner = Instance.new("UICorner")
        TitleCorner.CornerRadius = UDim.new(0, 16)
        TitleCorner.Parent = Title
        
        local TitleBorder = Instance.new("Frame")    
        TitleBorder.Size = UDim2.new(1, -2, 0, 1)    
        TitleBorder.Position = UDim2.new(0, 1, 1, -1)
        TitleBorder.BackgroundColor3 = ORANGE
        TitleBorder.BorderSizePixel = 0
        TitleBorder.ZIndex = 3
        TitleBorder.Parent = Title
        
        -- MINIMIZAR
        local MinimizeButton = Instance.new("TextButton")
        MinimizeButton.Name = "MinimizeButton"
        MinimizeButton.Size = UDim2.new(0, 24, 0, 24)
        MinimizeButton.Position = UDim2.new(1, -32, 0, 6)
        MinimizeButton.BackgroundColor3 = DARKER
        MinimizeButton.BorderSizePixel = 0
        MinimizeButton.Text = "−"
        MinimizeButton.TextColor3 = GRAY
        MinimizeButton.TextSize = 16
        MinimizeButton.Font = Enum.Font.GothamSemibold
        MinimizeButton.AutoButtonColor = false
        MinimizeButton.ZIndex = 10
        MinimizeButton.Parent = MainFrame
        
        local MinimizeCorner = Instance.new("UICorner")
        MinimizeCorner.CornerRadius = UDim.new(1, 0)
        MinimizeCorner.Parent = MinimizeButton
        
        local MinimizeStroke = Instance.new("UIStroke")
        MinimizeStroke.Color = ORANGE
        MinimizeStroke.Thickness = 1
        MinimizeStroke.ApplyStrokeMode = Enum.ApplyStrokeMode.Border
        MinimizeStroke.Parent = MinimizeButton
        
        MinimizeButton.MouseEnter:Connect(function()
            MinimizeButton.BackgroundColor3 = Color3.fromRGB(42, 42, 42)
            MinimizeButton.TextColor3 = WHITE
        end)
        MinimizeButton.MouseLeave:Connect(function()
            MinimizeButton.BackgroundColor3 = DARKER
            MinimizeButton.TextColor3 = GRAY
        end)
        
        -- CONTENT (SCROLL)
        local Content = Instance.new("ScrollingFrame")
        Content.Name = "Content"
        Content.Size = UDim2.new(1, -28, 0, 220)
        Content.Position = UDim2.new(0, 14, 0, 48)
        Content.BackgroundTransparency = 1
        Content.BorderSizePixel = 0
        Content.ClipsDescendants = false
        Content.ScrollBarThickness = 3
        Content.ScrollBarImageColor3 = DARKER
        Content.CanvasSize = UDim2.new(0, 0, 0, 0)
        Content.AutomaticCanvasSize = Enum.AutomaticSize.Y
        Content.ScrollingDirection = Enum.ScrollingDirection.Y
        Content.Parent = MainFrame
        
        local ContentPadding = Instance.new("UIPadding")
        ContentPadding.PaddingTop = UDim.new(0, 0)
        ContentPadding.PaddingBottom = UDim.new(0, 10)
        ContentPadding.PaddingLeft = UDim.new(0, 0)
        ContentPadding.PaddingRight = UDim.new(0, 2)
        ContentPadding.Parent = Content
        
        local ContentLayout = Instance.new("UIListLayout")
        ContentLayout.Padding = UDim.new(0, 10)
        ContentLayout.FillDirection = Enum.FillDirection.Vertical
        ContentLayout.HorizontalAlignment = Enum.HorizontalAlignment.Center
        ContentLayout.SortOrder = Enum.SortOrder.LayoutOrder
        ContentLayout.Parent = Content
        
        -- TOGGLE BUTTON (ON/OFF do ring)
        local ToggleButton = Instance.new("TextButton")
        ToggleButton.Name = "ToggleButton"
        ToggleButton.Size = UDim2.new(1, 0, 0, 34)
        ToggleButton.BackgroundColor3 = DARKER
        ToggleButton.BorderSizePixel = 0
        ToggleButton.Text = "Ring Off"
        ToggleButton.TextColor3 = WHITE
        ToggleButton.TextSize = 15
        ToggleButton.Font = Enum.Font.GothamSemibold
        ToggleButton.AutoButtonColor = false
        ToggleButton.LayoutOrder = 1
        ToggleButton.Parent = Content
        
        local ToggleCorner = Instance.new("UICorner")
        ToggleCorner.CornerRadius = UDim.new(0, 8)
        ToggleCorner.Parent = ToggleButton
        
        local ToggleStroke = Instance.new("UIStroke")
        ToggleStroke.Color = ORANGE
        ToggleStroke.Thickness = 1
        ToggleStroke.ApplyStrokeMode = Enum.ApplyStrokeMode.Border
        ToggleStroke.Parent = ToggleButton
        
        -- CONTROLES (+, -, label)
        local function createControl(key, displayName, initialValue, order)
            local Row = Instance.new("Frame")
            Row.Name = key .. "Control"
            Row.Size = UDim2.new(1, 0, 0, 30)
            Row.BackgroundTransparency = 1
            Row.LayoutOrder = order
            Row.Parent = Content
            
            local Minus = Instance.new("TextButton")
            Minus.Name = "Minus"
            Minus.Size = UDim2.new(0.2, 0, 1, 0)
            Minus.Position = UDim2.new(0, 0, 0, 0)
            Minus.BackgroundColor3 = DARK
            Minus.BorderSizePixel = 0
            Minus.Text = "−"
            Minus.TextColor3 = WHITE
            Minus.TextSize = 18
            Minus.Font = Enum.Font.GothamSemibold
            Minus.AutoButtonColor = false
            Minus.Parent = Row
            local MinusCorner = Instance.new("UICorner")
            MinusCorner.CornerRadius = UDim.new(0, 6)
            MinusCorner.Parent = Minus
            local MinusStroke = Instance.new("UIStroke")
            MinusStroke.Color = ORANGE
            MinusStroke.Thickness = 1
            MinusStroke.ApplyStrokeMode = Enum.ApplyStrokeMode.Border
            MinusStroke.Parent = Minus
            
            local Label = Instance.new("TextLabel")
            Label.Name = "Label"
            Label.Size = UDim2.new(0.44, 0, 1, 0)
            Label.Position = UDim2.new(0.28, 0, 0, 0)
            Label.BackgroundColor3 = DARK
            Label.BorderSizePixel = 0
            Label.TextColor3 = GRAY
            Label.TextSize = 13
            Label.Font = Enum.Font.GothamMedium
            Label.TextXAlignment = Enum.TextXAlignment.Center
            Label.TextYAlignment = Enum.TextYAlignment.Center
            Label.TextTruncate = Enum.TextTruncate.AtEnd
            Label.Parent = Row
            local LabelCorner = Instance.new("UICorner")
            LabelCorner.CornerRadius = UDim.new(0, 6)
            LabelCorner.Parent = Label
            local LabelStroke = Instance.new("UIStroke")
            LabelStroke.Color = ORANGE
            LabelStroke.Thickness = 1
            LabelStroke.ApplyStrokeMode = Enum.ApplyStrokeMode.Border
            LabelStroke.Parent = Label
            
            local Plus = Instance.new("TextButton")
            Plus.Name = "Plus"
            Plus.Size = UDim2.new(0.2, 0, 1, 0)
            Plus.Position = UDim2.new(0.8, 0, 0, 0)
            Plus.BackgroundColor3 = DARK
            Plus.BorderSizePixel = 0
            Plus.Text = "+"
            Plus.TextColor3 = WHITE
            Plus.TextSize = 18
            Plus.Font = Enum.Font.GothamSemibold
            Plus.AutoButtonColor = false
            Plus.Parent = Row
            local PlusCorner = Instance.new("UICorner")
            PlusCorner.CornerRadius = UDim.new(0, 6)
            PlusCorner.Parent = Plus
            local PlusStroke = Instance.new("UIStroke")
            PlusStroke.Color = ORANGE
            PlusStroke.Thickness = 1
            PlusStroke.ApplyStrokeMode = Enum.ApplyStrokeMode.Border
            PlusStroke.Parent = Plus
            
            local function getName()
                if key == "rotationSpeed" then return "Rot. Speed"
                elseif key == "attractionStrength" then return "Attract"
                end
                return displayName
            end
            
            local function update()
                Label.Text = getName() .. ": " .. tostring(config[key])
            end
            
            Minus.MouseEnter:Connect(function()
                Minus.BackgroundColor3 = Color3.fromRGB(31, 31, 31)
            end)
            Minus.MouseLeave:Connect(function()
                Minus.BackgroundColor3 = DARK
            end)
            Plus.MouseEnter:Connect(function()
                Plus.BackgroundColor3 = Color3.fromRGB(31, 31, 31)
            end)
            Plus.MouseLeave:Connect(function()
                Plus.BackgroundColor3 = DARK
            end)
            
            Minus.MouseButton1Click:Connect(function()
                config[key] = math.max(0, config[key] - 1)
                update()
                playSound("12221967")
            end)
            Plus.MouseButton1Click:Connect(function()
                config[key] = math.min(10000, config[key] + 1)
                update()
                playSound("12221967")
            end)
            
            update()
        end
        
        createControl("radius", "Radius", config.radius, 2)
        createControl("height", "Height", config.height, 3)
        createControl("rotationSpeed", "Rotation Speed", config.rotationSpeed, 4)
        createControl("attractionStrength", "Attraction Strength", config.attractionStrength, 5)
        
        -- ===== RING TOGGLE (INTERNO) =====
        local ringPartsEnabled = false
        
        ToggleButton.MouseEnter:Connect(function()
            if not ringPartsEnabled then
                ToggleButton.BackgroundColor3 = Color3.fromRGB(42, 42, 42)
            end
        end)
        ToggleButton.MouseLeave:Connect(function()
            if not ringPartsEnabled then
                ToggleButton.BackgroundColor3 = DARKER
            end
        end)
        
        ToggleButton.MouseButton1Click:Connect(function()
            ringPartsEnabled = not ringPartsEnabled
            if ringPartsEnabled then
                ToggleButton.Text = "Tornado On"
                ToggleButton.BackgroundColor3 = ORANGE
                ToggleButton.TextColor3 = BLACK
                ToggleStroke.Color = ORANGE_LIGHT
            else
                ToggleButton.Text = "Ring Off"
                ToggleButton.BackgroundColor3 = DARKER
                ToggleButton.TextColor3 = WHITE
                ToggleStroke.Color = ORANGE
            end
            playSound("12221967")
        end)
        
        -- ===== MINIMIZE =====
        local minimized = false
        MinimizeButton.MouseButton1Click:Connect(function()
            minimized = not minimized
            if minimized then
                Content.Visible = false
                MainFrame:TweenSize(
                    UDim2.new(0, 280, 0, 36),
                    Enum.EasingDirection.Out,
                    Enum.EasingStyle.Quad,
                    0.25, true
                )
                MinimizeButton.Text = "+"
            else
                MainFrame:TweenSize(
                    UDim2.new(0, 280, 0, 270),
                    Enum.EasingDirection.Out,
                    Enum.EasingStyle.Quad,
                    0.25, true
                )
                Content.Visible = true
                MinimizeButton.Text = "−"
            end
            playSound("12221967")
        end)
        
        -- ===== DRAG =====
        local dragging = false
        local dragInput, dragStart, startPosition
        
        Title.InputBegan:Connect(function(input)
            if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then
                dragging = true
                dragStart = input.Position
                startPosition = MainFrame.Position
                input.Changed:Connect(function()
                    if input.UserInputState == Enum.UserInputState.End then
                        dragging = false
                    end
                end)
            end
        end)
        Title.InputChanged:Connect(function(input)
            if input.UserInputType == Enum.UserInputType.MouseMovement or input.UserInputType == Enum.UserInputType.Touch then
                dragInput = input
            end
        end)
        UserInputService.InputChanged:Connect(function(input)
            if input == dragInput and dragging then
                local delta = input.Position - dragStart
                MainFrame.Position = UDim2.new(
                    startPosition.X.Scale,
                    startPosition.X.Offset + delta.X,
                    startPosition.Y.Scale,
                    startPosition.Y.Offset + delta.Y
                )
            end
        end)
        
        -- ===== RING PARTS (TORNADO) =====
        local character = LocalPlayer.Character or LocalPlayer.CharacterAdded:Wait()
        local humanoidRootPart = character:WaitForChild("HumanoidRootPart")
        
        local Folder = Instance.new("Folder")
        Folder.Name = "SuperRingParts"
        Folder.Parent = Workspace
        ringInstancias.folder = Folder
        
        local Part = Instance.new("Part")
        Part.Name = "RingAttachmentPart"
        Part.Size = Vector3.new(1, 1, 1)
        Part.Anchored = true
        Part.CanCollide = false
        Part.Transparency = 1
        Part.Parent = Folder
        
        local Attachment1 = Instance.new("Attachment")
        Attachment1.Parent = Part
        
        -- ===== NETWORK =====
        if not getgenv().Network then
            getgenv().Network = {
                BaseParts = {},
                Velocity = Vector3.new(14.46262424, 14.46262424, 14.46262424)
            }
            
            Network.RetainPart = function(part)
                if typeof(part) == "Instance" and part:IsA("BasePart") and part:IsDescendantOf(Workspace) then
                    table.insert(Network.BaseParts, part)
                    part.CustomPhysicalProperties = PhysicalProperties.new(0, 0, 0, 0, 0)
                    part.CanCollide = false
                end
            end
            
            local function EnablePartControl()
                LocalPlayer.ReplicationFocus = Workspace
                local conn = RunService.Heartbeat:Connect(function()
                    sethiddenproperty(LocalPlayer, "SimulationRadius", math.huge)
                    for _, part in pairs(Network.BaseParts) do
                        if part:IsDescendantOf(Workspace) then
                            part.Velocity = Network.Velocity
                        end
                    end
                end)
                table.insert(ringInstancias.connections, conn)
            end
            EnablePartControl()
        end
        
        -- ===== PART RETENTION =====
        local parts = {}
        ringInstancias.parts = parts
        
        local function RetainPart(part)
            if part:IsA("BasePart") and not part.Anchored and part:IsDescendantOf(Workspace) then
                if LocalPlayer.Character and (part.Parent == LocalPlayer.Character or part:IsDescendantOf(LocalPlayer.Character)) then
                    return false
                end
                part.CustomPhysicalProperties = PhysicalProperties.new(0, 0, 0, 0, 0)
                part.CanCollide = false
                return true
            end
            return false
        end
        
        local function addPart(part)
            if RetainPart(part) then
                if not table.find(parts, part) then
                    table.insert(parts, part)
                end
            end
        end
        
        local function removePart(part)
            local index = table.find(parts, part)
            if index then table.remove(parts, index) end
        end
        
        for _, part in pairs(Workspace:GetDescendants()) do
            addPart(part)
        end
        
        local connAdded = Workspace.DescendantAdded:Connect(addPart)
        local connRemoved = Workspace.DescendantRemoving:Connect(removePart)
        table.insert(ringInstancias.connections, connAdded)
        table.insert(ringInstancias.connections, connRemoved)
        
        -- ===== TORNADO LOOP =====
        local connTornado = RunService.Heartbeat:Connect(function()
            if not ringPartsEnabled then return end
            
            local currentCharacter = LocalPlayer.Character
            local currentRoot = currentCharacter and currentCharacter:FindFirstChild("HumanoidRootPart")
            if not currentRoot then return end
            
            local tornadoCenter = currentRoot.Position
            
            for _, part in pairs(parts) do
                if part and part.Parent and not part.Anchored then
                    local pos = part.Position
                    local distance = (Vector3.new(pos.X, tornadoCenter.Y, pos.Z) - tornadoCenter).Magnitude
                    local angle = math.atan2(pos.Z - tornadoCenter.Z, pos.X - tornadoCenter.X)
                    local newAngle = angle + math.rad(config.rotationSpeed)
                    
                    local verticalOffset = 0
                    if config.height ~= 0 then
                        verticalOffset = config.height * math.abs(math.sin((pos.Y - tornadoCenter.Y) / config.height))
                    end
                    
                    local targetPos = Vector3.new(
                        tornadoCenter.X + math.cos(newAngle) * math.min(config.radius, distance),
                        tornadoCenter.Y + verticalOffset,
                        tornadoCenter.Z + math.sin(newAngle) * math.min(config.radius, distance)
                    )
                    
                    local difference = targetPos - part.Position
                    if difference.Magnitude > 0 then
                        part.Velocity = difference.Unit * config.attractionStrength
                    end
                end
            end
        end)
        table.insert(ringInstancias.connections, connTornado)
        
        -- ===== SOUND INICIAL =====
        local soundStart = playSound("2865227271")
        ringInstancias.sound = soundStart
        
        -- ===== NOTIFICAÇÕES REMOVIDAS =====
        -- (nada aqui)
        
        ringAtivo = true
    end
    
    -- ===== EVENTO DO TOGGLE (SEM NOTIFICAÇÕES) =====
RingBtn.MouseButton1Click:Connect(function()
    local estado = not ringAtivo
    updateToggle(RingTog, RingKnob, estado)

    if estado then
        ativarRing()
    else
        limparRing()
    end
end)

-- ===== LIMPEZA AUTOMÁTICA SE O SCRIPT FECHAR =====
table.insert(NERO.Connections, game:GetService("RunService").Heartbeat:Connect(function()
    if not Gui then
        limparRing()
    end
end))

-- ===== NÃO DESATIVAR AO MORRER =====
LocalPlayer.CharacterAdded:Connect(function()
    -- O Ring não será desligado automaticamente ao morrer
end)

end)

-- ==========================================================
-- TOGGLE: ANTI-FLING (ABA 2 - TROLLING)
-- ==========================================================
task.spawn(function()
    task.wait(0.1)

    -- ===== CRIA TOGGLE NA ABA TROLLING =====
    local AFTog, AFKnob, AFBtn = createToggle("Anti-Fling", tabContainers[2], 288)

    local antiFlingActive = false
    local antiFlingConnection = nil
    local removeFallConnection = nil
    local removeFallLoopRunning = false

    -- ===== SISTEMA ORIGINAL (100% INTACTO) =====
    local Players = game:GetService("Players")
    local RunService = game:GetService("RunService")
    local StarterGui = game:GetService("StarterGui")

    local player = Players.LocalPlayer
    if not player then return end

    -- ===== ANTI-FLING ORIGINAL =====
    local lastPositions = setmetatable({}, {__mode = "k"})
    local range = 20
    local overlapParams = OverlapParams.new()
    overlapParams.FilterType = Enum.RaycastFilterType.Exclude

    local currentCharacter = player.Character
    local humanoid

    local function setupCharacter(char)
        if not char then return end
        currentCharacter = char
        humanoid = char:FindFirstChildOfClass("Humanoid") or char:WaitForChild("Humanoid", 5)
        overlapParams.FilterDescendantsInstances = {char}
        lastPositions = setmetatable({}, {__mode = "k"})
        if humanoid then
            pcall(function()
                humanoid:SetStateEnabled(Enum.HumanoidStateType.Seated, false)
            end)
        end
    end

    if not currentCharacter or not currentCharacter.Parent then
        currentCharacter = player.Character or player.CharacterAdded:Wait()
    end
    setupCharacter(currentCharacter)
    player.CharacterAdded:Connect(function(char) setupCharacter(char) end)

    -- ===== REMOVE FALL ORIGINAL (SEM A GUI) =====
    local function getRoot(char)
        return char and char:FindFirstChild("HumanoidRootPart")
    end

    local function startRemoveFall()
        if removeFallLoopRunning then return end
        removeFallLoopRunning = true
        
        removeFallConnection = task.spawn(function()
            while antiFlingActive do
                RunService.Heartbeat:Wait()
                local char = player.Character
                local root = getRoot(char)
                local vel, movel = nil, 0.1

                if char and char.Parent and root and root.Parent then
                    vel = root.Velocity
                    pcall(function() root.Velocity = Vector3.new(0, 0, 0) end)

                    RunService.RenderStepped:Wait()
                    if player.Character and root and root.Parent then
                        pcall(function() root.Velocity = vel end)
                    end

                    RunService.Stepped:Wait()
                    if player.Character and root and root.Parent then
                        pcall(function() root.Velocity = vel + Vector3.new(0, movel, 0) end)
                        movel = -movel
                    end
                end
            end
            removeFallLoopRunning = false
        end)
    end

    -- ===== FUNÇÃO PARA LIGAR TUDO =====
    local function startSystem()
        if antiFlingConnection then return end
        
        -- Liga Anti-Fling
        antiFlingConnection = RunService.Heartbeat:Connect(function()
            local char = player.Character
            if not char or not char.Parent then return end
            overlapParams.FilterDescendantsInstances = {char}

            local cf, size = char:GetBoundingBox()
            local regionSize = size + Vector3.new(range, range, range)
            local ok, parts = pcall(function()
                return workspace:GetPartBoundsInBox(cf, regionSize, overlapParams)
            end)
            if not ok or not parts then return end

            for _, part in ipairs(parts) do
                if part and part:IsA("BasePart") then
                    pcall(function() part.AssemblyLinearVelocity = Vector3.new(0, 0, 0) end)
                    pcall(function() part.AssemblyAngularVelocity = Vector3.new(0, 0, 0) end)
                    pcall(function() part.Velocity = Vector3.new(0, 0, 0) end)
                    pcall(function() part.RotVelocity = Vector3.new(0, 0, 0) end)

                    local lastPos = lastPositions[part]
                    if lastPos and (part.Position - lastPos).Magnitude > 1 then
                        pcall(function() part.CanCollide = false end)
                    end
                    lastPositions[part] = part.Position
                end
            end
        end)
        
        -- Liga RemoveFall
        startRemoveFall()
        
        pcall(function()
            StarterGui:SetCore("SendNotification", {
                Title = "Anti-Fling",
                Text = "Protection Enabled",
                Duration = 3
            })
        end)
    end

    -- ===== FUNÇÃO PARA DESLIGAR TUDO =====
    local function stopSystem()
        if antiFlingConnection then
            antiFlingConnection:Disconnect()
            antiFlingConnection = nil
        end
        
        if removeFallConnection then
            task.cancel(removeFallConnection)
            removeFallConnection = nil
            removeFallLoopRunning = false
        end
        
        pcall(function()
            StarterGui:SetCore("SendNotification", {
                Title = "Anti-Fling",
                Text = "Protection Disabled",
                Duration = 3
            })
        end)
    end

    -- ===== TOGGLE PRINCIPAL =====
    AFBtn.MouseButton1Click:Connect(function()
        antiFlingActive = not antiFlingActive
        updateToggle(AFTog, AFKnob, antiFlingActive)
        
        if antiFlingActive then
            startSystem()
        else
            stopSystem()
        end
    end)

    -- ===== LIMPEZA =====
    table.insert(NERO.Connections, {
        Disconnect = function()
            if antiFlingConnection then
                antiFlingConnection:Disconnect()
                antiFlingConnection = nil
            end
            if removeFallConnection then
                task.cancel(removeFallConnection)
                removeFallConnection = nil
                removeFallLoopRunning = false
            end
        end
    })
end)
-- ============================================================
-- 🎯 ESP DE ITENS TOCÁVEIS (OTIMIZADO - SEM LAG)
-- ============================================================

task.spawn(function()
    task.wait(0.1)
    
    -- ===== VARIAVEIS =====
    local itemESP = false
    local espHighlights = {}
    local espLoop = nil
    local cacheItens = {}
    local cacheTimer = 0
    
    -- ===== TOGGLE PREMIUM NA ABA OMNI - POSIÇÃO 386 =====
    local ItemTog, ItemKnob, ItemBtn = createPremiumToggle(
        "ESP de Itens", 
        "Destaca alavancas, botoes, portas e itens interativos no mapa", 
        tabContainers[7], 
        554
    )
    
    -- ===== DETECTA ITENS INTERATIVOS (COM CACHE) =====
    local function detectarItens()
        local itens = {}
        local now = tick()
        
        -- SÓ ESCANEIA A CADA 2 SEGUNDOS
        if now - cacheTimer < 2 then
            return cacheItens
        end
        cacheTimer = now
        
        for _, obj in pairs(workspace:GetDescendants()) do
            -- SÓ PROCURA EM PARTES E TOOLS (MAIS RÁPIDO)
            if obj:IsA("BasePart") or obj:IsA("Tool") then
                local nome = obj.Name:lower()
                
                -- ALAVANCAS / BOTÕES
                if obj:IsA("BasePart") and (
                    nome:find("button") or
                    nome:find("botao") or
                    nome:find("switch") or
                    nome:find("alavanca") or
                    nome:find("lever") or
                    nome:find("interruptor") or
                    nome:find("painel") or
                    nome:find("panel")
                ) then
                    table.insert(itens, obj)
                end
                
                -- PORTAS
                if obj:IsA("BasePart") and (
                    nome:find("door") or
                    nome:find("porta") or
                    nome:find("gate") or
                    nome:find("portal")
                ) then
                    table.insert(itens, obj)
                end
                
                -- CHAVES / ITENS
                if obj:IsA("Tool") and (
                    nome:find("key") or
                    nome:find("chave") or
                    nome:find("item") or
                    nome:find("objeto")
                ) then
                    table.insert(itens, obj)
                end
            end
            
            -- PROXIMITY PROMPT (MAIS RÁPIDO)
            if obj:IsA("ProximityPrompt") and obj.Parent and obj.Parent:IsA("BasePart") then
                table.insert(itens, obj.Parent)
            end
            
            -- CLICK DETECTOR
            if obj:IsA("ClickDetector") and obj.Parent and obj.Parent:IsA("BasePart") then
                table.insert(itens, obj.Parent)
            end
        end
        
        cacheItens = itens
        return itens
    end
    
    -- ===== CRIA HIGHLIGHTS (OTIMIZADO) =====
    local function criarHighlights()
        if not itemESP then 
            -- LIMPA HIGHLIGHTS SE ESTIVER DESLIGADO
            for _, hl in pairs(espHighlights) do
                pcall(function() hl:Destroy() end)
            end
            espHighlights = {}
            return 
        end
        
        local char = LP.Character
        if not char then return end
        
        local hrp = char:FindFirstChild("HumanoidRootPart")
        if not hrp then return end
        
        local posicao = hrp.Position
        local itens = detectarItens()
        local distancia = 100
        local novosHighlights = {}
        
        -- SÓ CRIA HIGHLIGHTS PARA ITENS PRÓXIMOS
        for _, item in pairs(itens) do
            if item and item.Parent then
                local pos = item:IsA("BasePart") and item.Position or 
                           (item:IsA("Tool") and item.Handle and item.Handle.Position) or 
                           Vector3.new(0,0,0)
                
                local dist = (pos - posicao).Magnitude
                
                if dist < distancia then
                    -- VERIFICA SE JÁ TEM HIGHLIGHT
                    local jaTem = false
                    for _, hl in pairs(espHighlights) do
                        if hl.Adornee == item then
                            jaTem = true
                            table.insert(novosHighlights, hl)
                            break
                        end
                    end
                    
                    if not jaTem then
                        local hl = Instance.new("Highlight")
                        hl.Name = "ItemHighlight"
                        hl.FillColor = Color3.fromRGB(255, 100, 0)
                        hl.FillTransparency = 0.3
                        hl.OutlineColor = Color3.fromRGB(255, 200, 50)
                        hl.OutlineTransparency = 0.1
                        hl.DepthMode = Enum.HighlightDepthMode.AlwaysOnTop
                        hl.Adornee = item
                        hl.Parent = item
                        table.insert(novosHighlights, hl)
                    end
                end
            end
        end
        
        -- REMOVE HIGHLIGHTS QUE NÃO ESTÃO MAIS PRÓXIMOS
        for _, hl in pairs(espHighlights) do
            local manter = false
            for _, novo in pairs(novosHighlights) do
                if hl == novo then
                    manter = true
                    break
                end
            end
            if not manter then
                pcall(function() hl:Destroy() end)
            end
        end
        
        espHighlights = novosHighlights
    end
    
    -- ===== LOOP (MAIS LEVE) =====
    local function startESP()
        if espLoop then espLoop:Disconnect() end
        espLoop = RS.Heartbeat:Connect(criarHighlights)  -- Heartbeat é mais leve que RenderStepped
    end
    
    local function stopESP()
        if espLoop then
            espLoop:Disconnect()
            espLoop = nil
        end
        for _, hl in pairs(espHighlights) do
            pcall(function() hl:Destroy() end)
        end
        espHighlights = {}
        cacheItens = {}
    end
    
    -- ===== TOGGLE =====
    ItemBtn.MouseButton1Click:Connect(function()
        itemESP = not itemESP
        updateToggle(ItemTog, ItemKnob, itemESP, true)
        
        if itemESP then
            Notify("ESP de Itens ATIVADO!")
            startESP()
        else
            Notify("ESP de Itens DESATIVADO!")
            stopESP()
        end
    end)
    
    -- ===== PERSISTÊNCIA NA MORTE =====
    LP.CharacterAdded:Connect(function()
        if itemESP then
            task.wait(0.5)
            startESP()
        end
    end)
    
    print("[ESP ITENS] Carregado! (Otimizado - sem lag)")
end)
-- ==========================================================
-- TOGGLE: REINICIAR (ABA 6 - CONFIG)
-- ==========================================================
task.spawn(function()
    task.wait(0.1)

    -- CRIA O TOGGLE NA ABA 6 (CONFIG)
    local ReiniciarTog, ReiniciarKnob, ReiniciarBtn = createToggle("Reiniciar", tabContainers[6], 96)

    ReiniciarBtn.MouseButton1Click:Connect(function()
        -- Ativa o toggle (mas ele não fica ligado, é uma ação única)
        updateToggle(ReiniciarTog, ReiniciarKnob, true)
        
        -- Pequeno delay para o efeito visual
        task.wait(0.1)

        -- MATA O PERSONAGEM (REINICIA)
        if LP.Character then
            local hum = LP.Character:FindFirstChildOfClass("Humanoid")
            if hum then
                hum.Health = 0
            end
        end

        -- Desliga o toggle automaticamente (ação única)
        task.wait(0.2)
        updateToggle(ReiniciarTog, ReiniciarKnob, false)
    end)

    -- Impede que o toggle fique ligado acidentalmente
    ReiniciarTog:GetPropertyChangedSignal("BackgroundColor3"):Connect(function()
        if ReiniciarTog:GetAttribute("Ativo") then
            task.wait(0.3)
            updateToggle(ReiniciarTog, ReiniciarKnob, false)
        end
    end)
end)
