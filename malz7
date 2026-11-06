--=====================================================
-- CAPETINHA HUB | BLUE BLACK EDITION
-- Criador: GOAT VN
--=====================================================

local Players = game:GetService("Players")
local Lighting = game:GetService("Lighting")
local ContentProvider = game:GetService("ContentProvider")
local UIS = game:GetService("UserInputService")
local RunService = game:GetService("RunService")
local TweenService = game:GetService("TweenService")
local Workspace = game:GetService("Workspace")
local LP = Players.LocalPlayer
local Terrain = Workspace:FindFirstChildOfClass("Terrain")

local CoreGui = game:GetService("CoreGui")
pcall(function() if gethui then CoreGui = gethui() end end)

-- Limpa antigo
pcall(function()
    for _, g in ipairs(CoreGui:GetChildren()) do
        if g.Name and g.Name:find("capetinhaHub") then g:Destroy() end
    end
end)

--============ HELPERS ============
local function FixAsset(id)
    id = tostring(id)
    if string.find(id, "rbxasset://", 1, true) then return id end
    return "rbxassetid://" .. id
end

local function IsCharacterPart(obj)
    local c = obj
    while c and c ~= Workspace do
        if c:IsA("Model") and c:FindFirstChildOfClass("Humanoid") then return true end
        c = c.Parent
    end
    return false
end

local function HasBallName(obj)
    if not obj then return false end
    local n = string.lower(obj.Name)
    for _, w in ipairs({"soccerball","football","matchball","gameball","futebol","bola","ball","tps"}) do
        if string.find(n, w, 1, true) then return true end
    end
    return false
end

local function IsBall(part)
    if not part or not part:IsA("BasePart") or IsCharacterPart(part) then return false end
    if string.lower(part.Name) == "tps" then return true end
    if HasBallName(part) or HasBallName(part.Parent) or (part.Parent and HasBallName(part.Parent.Parent)) then return true end
    local b = math.max(part.Size.X, part.Size.Y, part.Size.Z)
    local s = math.min(part.Size.X, part.Size.Y, part.Size.Z)
    if b < 0.5 or b > 8 then return false end
    if part:IsA("Part") and part.Shape == Enum.PartType.Ball and not part.Anchored then return true end
    if part:IsA("MeshPart") and not part.Anchored and s/b >= 0.80 then return true end
    return false
end

-- Cache de bolas: evita percorrer Workspace:GetDescendants() repetidamente.
local KnownBallParts = setmetatable({}, {__mode = "k"})
local function RegisterBallCandidate(obj)
    if obj and obj:IsA("BasePart") and IsBall(obj) then
        KnownBallParts[obj] = true
    end
end

for _, obj in ipairs(Workspace:GetDescendants()) do
    RegisterBallCandidate(obj)
end

Workspace.DescendantAdded:Connect(function(obj)
    if obj:IsA("BasePart") then
        task.defer(RegisterBallCandidate, obj)
    end
end)

Workspace.DescendantRemoving:Connect(function(obj)
    KnownBallParts[obj] = nil
end)

local function GetTrackedBalls()
    local result = {}
    for part in pairs(KnownBallParts) do
        if part and part.Parent and IsBall(part) then
            table.insert(result, part)
        else
            KnownBallParts[part] = nil
        end
    end
    return result
end

local function GetClosestBall()
    local closest, dist = nil, math.huge
    local char = LP.Character
    if not char or not char:FindFirstChild("HumanoidRootPart") then return nil end
    local myPos = char.HumanoidRootPart.Position
    for _, obj in ipairs(GetTrackedBalls()) do
        local d = (obj.Position - myPos).Magnitude
        if d < dist then dist = d; closest = obj end
    end
    return closest
end

local States = {
    AutoBall=false, Reach=false, AutoCatch=false, AutoDive=false,
    AutoHambolo=false, AutoFollow=false, RGBBall=false, BallFire=false
}

--=====================================================
-- UI PRINCIPAL (Capetinha Hub • Pure Black / Active Blue)
--=====================================================
local GUI = Instance.new("ScreenGui")
GUI.Name = "CapetinhaHub_"..tick()
GUI.ResetOnSpawn = false
GUI.IgnoreGuiInset = true
GUI.ZIndexBehavior = Enum.ZIndexBehavior.Sibling
GUI.Parent = CoreGui

-- Botão flutuante: quadrado preto com símbolo geométrico azul
local ToggleBtn = Instance.new("TextButton", GUI)
ToggleBtn.Size = UDim2.new(0, 42, 0, 42)
ToggleBtn.Position = UDim2.new(0, 15, 0.5, -21)
ToggleBtn.BackgroundColor3 = Color3.fromRGB(0, 0, 0)
ToggleBtn.Text = ""
ToggleBtn.AutoButtonColor = false
ToggleBtn.BorderSizePixel = 0
Instance.new("UICorner", ToggleBtn).CornerRadius = UDim.new(0, 10)

local BtnStroke = Instance.new("UIStroke", ToggleBtn)
BtnStroke.Color = Color3.fromRGB(16, 16, 16)
BtnStroke.Thickness = 1

-- Marca angular azul inspirada no símbolo quadrado de referência
local LogoMark = Instance.new("Frame", ToggleBtn)
LogoMark.Name = "BlueLogo"
LogoMark.Size = UDim2.new(1, 0, 1, 0)
LogoMark.BackgroundTransparency = 1
LogoMark.Active = false

local LogoLeft = Instance.new("Frame", LogoMark)
LogoLeft.Size = UDim2.new(0, 5, 0, 21)
LogoLeft.Position = UDim2.new(0, 10, 0, 10)
LogoLeft.BackgroundColor3 = Color3.fromRGB(0, 145, 255)
LogoLeft.BorderSizePixel = 0
LogoLeft.Rotation = 0

local LogoDiagonal = Instance.new("Frame", LogoMark)
LogoDiagonal.Size = UDim2.new(0, 5, 0, 25)
LogoDiagonal.Position = UDim2.new(0, 17, 0, 8)
LogoDiagonal.BackgroundColor3 = Color3.fromRGB(0, 145, 255)
LogoDiagonal.BorderSizePixel = 0
LogoDiagonal.Rotation = -25

local LogoRight = Instance.new("Frame", LogoMark)
LogoRight.Size = UDim2.new(0, 5, 0, 21)
LogoRight.Position = UDim2.new(0, 27, 0, 10)
LogoRight.BackgroundColor3 = Color3.fromRGB(0, 145, 255)
LogoRight.BorderSizePixel = 0

local LogoFoot = Instance.new("Frame", LogoMark)
LogoFoot.Size = UDim2.new(0, 12, 0, 4)
LogoFoot.Position = UDim2.new(0, 17, 0, 28)
LogoFoot.BackgroundColor3 = Color3.fromRGB(0, 145, 255)
LogoFoot.BorderSizePixel = 0

-- Janela Principal
local TargetSize = UDim2.new(0, 560, 0, 390)

local Main = Instance.new("Frame", GUI)
Main.Size = UDim2.new(0, 0, 0, 0)
Main.Position = UDim2.new(0.5, 0, 0.5, 0)
Main.AnchorPoint = Vector2.new(0.5, 0.5)
Main.BackgroundColor3 = Color3.fromRGB(0, 0, 0)
Main.BorderSizePixel = 0
Main.ClipsDescendants = true
Main.Visible = false
Instance.new("UICorner", Main).CornerRadius = UDim.new(0, 10)
local MainStroke = Instance.new("UIStroke", Main)
MainStroke.Color = Color3.fromRGB(18, 18, 18)
MainStroke.Thickness = 1

-- Header
local Header = Instance.new("Frame", Main)
Header.Size = UDim2.new(1, 0, 0, 36)
Header.BackgroundColor3 = Color3.fromRGB(0, 0, 0)
Header.BorderSizePixel = 0
Instance.new("UICorner", Header).CornerRadius = UDim.new(0, 10)
local HeaderFix = Instance.new("Frame", Header)
HeaderFix.Size = UDim2.new(1, 0, 0, 10)
HeaderFix.Position = UDim2.new(0, 0, 1, -10)
HeaderFix.BackgroundColor3 = Color3.fromRGB(0, 0, 0)
HeaderFix.BorderSizePixel = 0

local HTitle = Instance.new("TextLabel", Header)
HTitle.Size = UDim2.new(1, -80, 1, 0)
HTitle.Position = UDim2.new(0, 12, 0, 0)
HTitle.BackgroundTransparency = 1
HTitle.Text = "CAPETINHA HUB"
HTitle.TextColor3 = Color3.fromRGB(235, 235, 235)
HTitle.Font = Enum.Font.GothamBold
HTitle.TextSize = 13
HTitle.TextXAlignment = Enum.TextXAlignment.Left

local CloseBtn = Instance.new("TextButton", Header)
CloseBtn.Size = UDim2.new(0, 24, 0, 24)
CloseBtn.Position = UDim2.new(1, -30, 0.5, -12)
CloseBtn.BackgroundColor3 = Color3.fromRGB(8, 8, 8)
CloseBtn.Text = "✕"
CloseBtn.TextColor3 = Color3.fromRGB(225, 240, 255)
CloseBtn.Font = Enum.Font.GothamBold
CloseBtn.TextSize = 12
CloseBtn.BorderSizePixel = 0
CloseBtn.AutoButtonColor = false
Instance.new("UICorner", CloseBtn).CornerRadius = UDim.new(1, 0)

-- Menu lateral no estilo da referência
local SidePanel = Instance.new("Frame", Main)
SidePanel.Name = "Sidebar"
SidePanel.Size = UDim2.new(0, 116, 1, -50)
SidePanel.Position = UDim2.new(0, 8, 0, 44)
SidePanel.BackgroundColor3 = Color3.fromRGB(3, 3, 3)
SidePanel.BorderSizePixel = 0
Instance.new("UICorner", SidePanel).CornerRadius = UDim.new(0, 8)

local SideTitle = Instance.new("TextLabel", SidePanel)
SideTitle.Size = UDim2.new(1, -16, 0, 25)
SideTitle.Position = UDim2.new(0, 8, 0, 8)
SideTitle.BackgroundTransparency = 1
SideTitle.Text = "MENU"
SideTitle.TextColor3 = Color3.fromRGB(125, 125, 125)
SideTitle.Font = Enum.Font.GothamBold
SideTitle.TextSize = 10
SideTitle.TextXAlignment = Enum.TextXAlignment.Left

local TabBar = Instance.new("Frame", SidePanel)
TabBar.Name = "Navigation"
TabBar.Size = UDim2.new(1, -12, 1, -42)
TabBar.Position = UDim2.new(0, 6, 0, 34)
TabBar.BackgroundTransparency = 1
TabBar.BorderSizePixel = 0
local TabList = Instance.new("UIListLayout", TabBar)
TabList.FillDirection = Enum.FillDirection.Vertical
TabList.Padding = UDim.new(0, 5)
TabList.HorizontalAlignment = Enum.HorizontalAlignment.Center
TabList.SortOrder = Enum.SortOrder.LayoutOrder

-- Área de conteúdo
local ContentTitle = Instance.new("TextLabel", Main)
ContentTitle.Name = "ContentTitle"
ContentTitle.Size = UDim2.new(1, -145, 0, 25)
ContentTitle.Position = UDim2.new(0, 135, 0, 47)
ContentTitle.BackgroundTransparency = 1
ContentTitle.Text = "INFORMAÇÕES"
ContentTitle.TextColor3 = Color3.fromRGB(230, 230, 230)
ContentTitle.Font = Enum.Font.GothamBold
ContentTitle.TextSize = 13
ContentTitle.TextXAlignment = Enum.TextXAlignment.Left

local Container = Instance.new("ScrollingFrame", Main)
Container.Size = UDim2.new(1, -145, 1, -86)
Container.Position = UDim2.new(0, 135, 0, 76)
Container.BackgroundColor3 = Color3.fromRGB(2, 2, 2)
Container.BorderSizePixel = 0
Container.ScrollBarThickness = 3
Container.ScrollBarImageColor3 = Color3.fromRGB(35, 35, 35)
Container.CanvasSize = UDim2.new(0, 0, 0, 0)
Container.AutomaticCanvasSize = Enum.AutomaticSize.Y
Instance.new("UICorner", Container).CornerRadius = UDim.new(0, 8)
local CList = Instance.new("UIListLayout", Container)
CList.Padding = UDim.new(0, 7)
CList.SortOrder = Enum.SortOrder.LayoutOrder
local CPad = Instance.new("UIPadding", Container)
CPad.PaddingTop = UDim.new(0, 9)
CPad.PaddingBottom = UDim.new(0, 9)
CPad.PaddingLeft = UDim.new(0, 9)
CPad.PaddingRight = UDim.new(0, 9)

-- Drag Suave
local function MakeDraggable(frame, handle)
    local dragging, dragStart, startPos
    handle.InputBegan:Connect(function(input)
        if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then
            dragging = true; dragStart = input.Position; startPos = frame.Position
        end
    end)
    handle.InputEnded:Connect(function(input)
        if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then
            dragging = false
        end
    end)
    UIS.InputChanged:Connect(function(input)
        if dragging and (input.UserInputType == Enum.UserInputType.MouseMovement or input.UserInputType == Enum.UserInputType.Touch) then
            local d = input.Position - dragStart
            TweenService:Create(frame, TweenInfo.new(0.08, Enum.EasingStyle.Sine, Enum.EasingDirection.Out), {
                Position = UDim2.new(startPos.X.Scale, startPos.X.Offset + d.X, startPos.Y.Scale, startPos.Y.Offset + d.Y)
            }):Play()
        end
    end)
end
MakeDraggable(Main, Header)
MakeDraggable(ToggleBtn, ToggleBtn)

-- ANIMAÇÃO DE ABERTURA E FECHAMENTO DA JANELA
local isOpen = false
local animating = false

local function ToggleWindow(state)
    if animating then return end
    animating = true
    if state == nil then state = not isOpen end
    isOpen = state

    if isOpen then
        Main.Visible = true
        TweenService:Create(Main, TweenInfo.new(0.35, Enum.EasingStyle.Back, Enum.EasingDirection.Out), {Size = TargetSize}):Play()
        TweenService:Create(ToggleBtn, TweenInfo.new(0.3, Enum.EasingStyle.Quad, Enum.EasingDirection.Out), {Rotation = 180}):Play()
        task.wait(0.35)
    else
        TweenService:Create(Main, TweenInfo.new(0.25, Enum.EasingStyle.Quad, Enum.EasingDirection.In), {Size = UDim2.new(0, 0, 0, 0)}):Play()
        TweenService:Create(ToggleBtn, TweenInfo.new(0.3, Enum.EasingStyle.Quad, Enum.EasingDirection.Out), {Rotation = 0}):Play()
        task.wait(0.25)
        Main.Visible = false
    end
    animating = false
end

-- GERADORES DE COMPONENTES COM INTERAÇÕES ANIMADAS
local function CreateToggle(name, callback)
    local Btn = Instance.new("TextButton", Container)
    Btn.Size = UDim2.new(1, 0, 0, 34)
    Btn.BackgroundColor3 = Color3.fromRGB(7, 7, 7)
    Btn.BorderSizePixel = 0
    Btn.Text = ""
    Btn.AutoButtonColor = false
    Instance.new("UICorner", Btn).CornerRadius = UDim.new(0, 6)
    local St = Instance.new("UIStroke", Btn)
    St.Color = Color3.fromRGB(14, 14, 14)

    local L = Instance.new("TextLabel", Btn)
    L.Size = UDim2.new(1, -60, 1, 0)
    L.Position = UDim2.new(0, 12, 0, 0)
    L.BackgroundTransparency = 1
    L.Text = name
    L.TextColor3 = Color3.fromRGB(225, 225, 225)
    L.Font = Enum.Font.GothamMedium
    L.TextSize = 12
    L.TextXAlignment = Enum.TextXAlignment.Left

    local Sw = Instance.new("Frame", Btn)
    Sw.Size = UDim2.new(0, 34, 0, 18)
    Sw.Position = UDim2.new(1, -42, 0.5, -9)
    Sw.BackgroundColor3 = Color3.fromRGB(28, 28, 28)
    Sw.BorderSizePixel = 0
    Instance.new("UICorner", Sw).CornerRadius = UDim.new(1, 0)

    local Kn = Instance.new("Frame", Sw)
    Kn.Size = UDim2.new(0, 12, 0, 12)
    Kn.Position = UDim2.new(0, 3, 0.5, -6)
    Kn.BackgroundColor3 = Color3.fromRGB(170, 170, 170)
    Kn.BorderSizePixel = 0
    Instance.new("UICorner", Kn).CornerRadius = UDim.new(1, 0)

    local st = false

    Btn.MouseEnter:Connect(function()
        TweenService:Create(Btn, TweenInfo.new(0.15), {BackgroundColor3 = Color3.fromRGB(16, 16, 16)}):Play()
    end)
    Btn.MouseLeave:Connect(function()
        TweenService:Create(Btn, TweenInfo.new(0.15), {BackgroundColor3 = Color3.fromRGB(7, 7, 7)}):Play()
    end)

    Btn.MouseButton1Click:Connect(function()
        st = not st
        if st then
            TweenService:Create(Sw, TweenInfo.new(0.2, Enum.EasingStyle.Quad, Enum.EasingDirection.Out), {BackgroundColor3 = Color3.fromRGB(0, 130, 235)}):Play()
            TweenService:Create(Kn, TweenInfo.new(0.2, Enum.EasingStyle.Back, Enum.EasingDirection.Out), {Position = UDim2.new(1, -15, 0.5, -6), BackgroundColor3 = Color3.fromRGB(255, 255, 255)}):Play()
        else
            TweenService:Create(Sw, TweenInfo.new(0.2, Enum.EasingStyle.Quad, Enum.EasingDirection.Out), {BackgroundColor3 = Color3.fromRGB(28, 28, 28)}):Play()
            TweenService:Create(Kn, TweenInfo.new(0.2, Enum.EasingStyle.Quad, Enum.EasingDirection.Out), {Position = UDim2.new(0, 3, 0.5, -6), BackgroundColor3 = Color3.fromRGB(210, 210, 210)}):Play()
        end
        callback(st)
    end)
    return Btn
end

local function CreateButton(name, callback)
    local Btn = Instance.new("TextButton", Container)
    Btn.Size = UDim2.new(1, 0, 0, 30)
    Btn.BackgroundColor3 = Color3.fromRGB(7, 7, 7)
    Btn.BorderSizePixel = 0
    Btn.Text = "      " .. name
    Btn.TextColor3 = Color3.fromRGB(225, 225, 225)
    Btn.Font = Enum.Font.GothamMedium
    Btn.TextSize = 12
    Btn.TextXAlignment = Enum.TextXAlignment.Left
    Btn.AutoButtonColor = false
    Instance.new("UICorner", Btn).CornerRadius = UDim.new(0, 6)
    local St = Instance.new("UIStroke", Btn)
    St.Color = Color3.fromRGB(14, 14, 14)

    Btn.MouseEnter:Connect(function()
        TweenService:Create(Btn, TweenInfo.new(0.15), {BackgroundColor3 = Color3.fromRGB(16, 16, 16)}):Play()
    end)
    Btn.MouseLeave:Connect(function()
        TweenService:Create(Btn, TweenInfo.new(0.15), {BackgroundColor3 = Color3.fromRGB(7, 7, 7)}):Play()
    end)
    Btn.MouseButton1Down:Connect(function()
        TweenService:Create(Btn, TweenInfo.new(0.08), {Size = UDim2.new(0.98, 0, 0, 28)}):Play()
    end)
    Btn.MouseButton1Up:Connect(function()
        TweenService:Create(Btn, TweenInfo.new(0.08), {Size = UDim2.new(1, 0, 0, 30)}):Play()
    end)

    Btn.MouseButton1Click:Connect(callback)
    return Btn
end

local function CreateLabel(text)
    local L = Instance.new("TextLabel", Container)
    L.Size = UDim2.new(1, 0, 0, 20)
    L.BackgroundTransparency = 1
    L.Text = "▸ " .. text
    L.TextColor3 = Color3.fromRGB(145, 145, 145)
    L.Font = Enum.Font.GothamBold
    L.TextSize = 11
    L.TextXAlignment = Enum.TextXAlignment.Left
    return L
end

local function ClearContainer()
    for _, v in ipairs(Container:GetChildren()) do
        if not v:IsA("UIListLayout") then v:Destroy() end
    end
end

--=====================================================
-- LÓGICAS DE JOGO
--=====================================================

local AutoFollowConnection
local CurrentFollowBall
local FollowScanTimer = 0
local FollowStopDistance = 0.20
local ManualSteerStrength = 1.25
local PlayerControls = nil
local FollowBallBind = Enum.KeyCode.K
local WaitingFollowBind = false

-- Shift Lock: alterna entre cursor preso e cursor livre
local ShiftLockEnabled = false
local ShiftLockConnection
ShiftLockConnection = UIS.InputBegan:Connect(function(input)
    if input.KeyCode ~= Enum.KeyCode.LeftShift and input.KeyCode ~= Enum.KeyCode.RightShift then return end
    if UIS:GetFocusedTextBox() then return end

    ShiftLockEnabled = not ShiftLockEnabled
    if ShiftLockEnabled then
        UIS.MouseBehavior = Enum.MouseBehavior.LockCenter
        UIS.MouseIconEnabled = false
    else
        UIS.MouseBehavior = Enum.MouseBehavior.Default
        UIS.MouseIconEnabled = true
    end
end)

pcall(function()
    local PlayerModule = require(LP:WaitForChild("PlayerScripts"):WaitForChild("PlayerModule"))
    PlayerControls = PlayerModule:GetControls()
end)

local function GetCharacterData()
    local c = LP.Character
    if not c then return nil, nil, nil end
    return c, c:FindFirstChildOfClass("Humanoid"), c:FindFirstChild("HumanoidRootPart")
end

local function IsValidTPSBall(part)
    if not part or not part.Parent or not part:IsA("BasePart") then return false end
    if string.lower(part.Name) ~= "tps" then return false end
    if IsCharacterPart(part) or part.Anchored then return false end
    local b = math.max(part.Size.X, part.Size.Y, part.Size.Z)
    local s = math.min(part.Size.X, part.Size.Y, part.Size.Z)
    if b < 0.5 or b > 6 then return false end
    if s/b < 0.60 then return false end
    return true
end

local function FindClosestBall()
    local _, _, root = GetCharacterData()
    if not root then return nil end
    local closest, cd = nil, math.huge
    local tracked = GetTrackedBalls()
    for _, p in ipairs(tracked) do
        if IsValidTPSBall(p) then
            local off = Vector3.new(p.Position.X-root.Position.X, 0, p.Position.Z-root.Position.Z)
            if off.Magnitude < cd then cd = off.Magnitude; closest = p end
        end
    end
    if closest then return closest end
    for _, p in ipairs(tracked) do
        local off = Vector3.new(p.Position.X-root.Position.X, 0, p.Position.Z-root.Position.Z)
        if off.Magnitude < cd then cd = off.Magnitude; closest = p end
    end
    return closest
end

local function GetManualDirection()
    local cam = Workspace.CurrentCamera
    if not cam then return Vector3.zero end
    local fwd = Vector3.new(cam.CFrame.LookVector.X, 0, cam.CFrame.LookVector.Z)
    local rgt = Vector3.new(cam.CFrame.RightVector.X, 0, cam.CFrame.RightVector.Z)
    if fwd.Magnitude > 0 then fwd = fwd.Unit end
    if rgt.Magnitude > 0 then rgt = rgt.Unit end
    if PlayerControls then
        local ok, mv = pcall(function() return PlayerControls:GetMoveVector() end)
        if ok and mv then
            local d = rgt * mv.X + fwd * -mv.Z
            if d.Magnitude > 1 then d = d.Unit end
            if d.Magnitude > 0.01 then return d end
        end
    end
    local d = Vector3.zero
    if UIS:IsKeyDown(Enum.KeyCode.W) then d += fwd end
    if UIS:IsKeyDown(Enum.KeyCode.S) then d -= fwd end
    if UIS:IsKeyDown(Enum.KeyCode.D) then d += rgt end
    if UIS:IsKeyDown(Enum.KeyCode.A) then d -= rgt end
    if d.Magnitude > 1 then d = d.Unit end
    return d
end

local function StartAutoFollowBall()
    if States.AutoFollow then return end
    States.AutoFollow = true
    CurrentFollowBall = nil
    FollowScanTimer = 1
    if AutoFollowConnection then AutoFollowConnection:Disconnect() end
    AutoFollowConnection = RunService.RenderStepped:Connect(function(dt)
        if not States.AutoFollow then return end
        local _, hum, root = GetCharacterData()
        if not hum or not root or hum.Health <= 0 then return end
        hum.AutoRotate = true
        FollowScanTimer += dt
        local inv = not CurrentFollowBall or not CurrentFollowBall.Parent
        if CurrentFollowBall and CurrentFollowBall.Parent and string.lower(CurrentFollowBall.Name) == "tps" and not IsValidTPSBall(CurrentFollowBall) then inv = true end
        local far = false
        if CurrentFollowBall and CurrentFollowBall.Parent then
            far = (Vector3.new(CurrentFollowBall.Position.X-root.Position.X,0,CurrentFollowBall.Position.Z-root.Position.Z)).Magnitude > 250
        end
        if inv or far then FollowScanTimer = 0; CurrentFollowBall = FindClosestBall()
        elseif FollowScanTimer >= 0.25 then FollowScanTimer = 0 end
        local ball = CurrentFollowBall
        if not ball or not ball.Parent then return end


        local toBall = Vector3.new(ball.Position.X-root.Position.X, 0, ball.Position.Z-root.Position.Z)
        local dist = toBall.Magnitude
        local manual = GetManualDirection()
        if dist > FollowStopDistance then
            local fl = toBall.Unit
            local fin = fl
            if manual.Magnitude > 0.01 then
                local amt = math.clamp(manual.Magnitude, 0, 1)
                fin = fl * (1 - amt * 0.45) + manual * ManualSteerStrength
                if fin.Magnitude > 0 then fin = fin.Unit end
            end
            hum:Move(fin, false)
        elseif manual.Magnitude > 0.01 then
            hum:Move(manual, false)
        end
    end)
end

local function StopAutoFollowBall()
    States.AutoFollow = false
    CurrentFollowBall = nil
    FollowScanTimer = 0
    if AutoFollowConnection then AutoFollowConnection:Disconnect() AutoFollowConnection = nil end
    local _, hum = GetCharacterData()
    if hum then hum.AutoRotate = true end
end

UIS.InputBegan:Connect(function(input, processed)
    if input.UserInputType ~= Enum.UserInputType.Keyboard then return end
    if WaitingFollowBind then
        if input.KeyCode == Enum.KeyCode.Escape then WaitingFollowBind = false return end
        if input.KeyCode == Enum.KeyCode.Unknown then return end
        FollowBallBind = input.KeyCode
        WaitingFollowBind = false
        return
    end
    if processed or UIS:GetFocusedTextBox() then return end
    if input.KeyCode == FollowBallBind then
        if States.AutoFollow then StopAutoFollowBall() else StartAutoFollowBall() end
    end
end)

-- Skyboxes: presets com seis faces, carregamento antes da troca e restauração segura
local OriginalSkies = {}
for _, skyObject in ipairs(Lighting:GetChildren()) do
    if skyObject:IsA("Sky") then
        table.insert(OriginalSkies, skyObject:Clone())
    end
end

local SKYBOXES = {
    {
        C = "PADRÃO", N = "Roblox Clássico",
        Bk = "rbxasset://textures/sky/sky512_bk.tex",
        Dn = "rbxasset://textures/sky/sky512_dn.tex",
        Ft = "rbxasset://textures/sky/sky512_ft.tex",
        Lf = "rbxasset://textures/sky/sky512_lf.tex",
        Rt = "rbxasset://textures/sky/sky512_rt.tex",
        Up = "rbxasset://textures/sky/sky512_up.tex",
        BuiltIn = true
    },
    {
        C = "AZUL", N = "Blue Sky",
        Bk = "135483466", Dn = "135483484",
        Ft = "135483461", Lf = "135483495",
        Rt = "135483499", Up = "135483475"
    },
    {
        C = "ESPAÇO", N = "Galaxy",
        Bk = "159454299", Dn = "159454296",
        Ft = "159454293", Lf = "159454286",
        Rt = "159454300", Up = "159454288"
    }
}

local SkyStatusLabel
local SkyRequestId = 0

local function SetSkyStatus(message)
    if SkyStatusLabel and SkyStatusLabel.Parent then
        SkyStatusLabel.Text = "▸ STATUS: " .. tostring(message)
    end
end

local function FixSkyAsset(asset)
    local value = tostring(asset or "")
    if value == "" then
        error("Uma das seis faces da Skybox está vazia.")
    end
    if string.find(value, "rbxasset://", 1, true)
        or string.find(value, "rbxassetid://", 1, true)
        or string.find(value, "http", 1, true) then
        return value
    end
    return "rbxassetid://" .. value
end

local function ApplySkybox(data)
    if type(data) ~= "table" then
        SetSkyStatus("preset inválido")
        return
    end

    SkyRequestId += 1
    local requestId = SkyRequestId

    local candidate = Instance.new("Sky")
    candidate.Name = "CapetinhaHub_Sky_Loading"

    local propertyMap = {
        SkyboxBk = data.Bk,
        SkyboxDn = data.Dn,
        SkyboxFt = data.Ft,
        SkyboxLf = data.Lf,
        SkyboxRt = data.Rt,
        SkyboxUp = data.Up
    }

    local assigned, assignError = pcall(function()
        for propertyName, assetId in pairs(propertyMap) do
            candidate[propertyName] = FixSkyAsset(assetId)
        end
        candidate.CelestialBodiesShown = true
        candidate.StarCount = 3000
    end)

    if not assigned then
        candidate:Destroy()
        SetSkyStatus("não foi possível configurar: " .. tostring(assignError))
        return
    end

    -- O céu atual permanece enquanto as seis imagens são carregadas.
    candidate.Parent = Lighting
    SetSkyStatus("carregando " .. tostring(data.N) .. "...")

    task.spawn(function()
        local failedAsset = false
        local preloadOk, preloadError = pcall(function()
            ContentProvider:PreloadAsync({candidate}, function(_, fetchStatus)
                if fetchStatus ~= Enum.AssetFetchStatus.Success then
                    failedAsset = true
                end
            end)
        end)

        if requestId ~= SkyRequestId then
            if candidate.Parent then candidate:Destroy() end
            return
        end

        if not preloadOk or failedAsset then
            if candidate.Parent then candidate:Destroy() end
            SetSkyStatus("falha ao carregar imagens; céu anterior mantido")
            return
        end

        -- Só troca depois do carregamento terminar.
        for _, existingSky in ipairs(Lighting:GetChildren()) do
            if existingSky:IsA("Sky") and existingSky ~= candidate then
                existingSky:Destroy()
            end
        end
        candidate.Name = "CapetinhaHub_Sky"
        SetSkyStatus(tostring(data.N) .. " aplicado")
    end)
end

local function RestoreSky()
    SkyRequestId += 1 -- cancela qualquer carregamento ainda em andamento

    for _, skyObject in ipairs(Lighting:GetChildren()) do
        if skyObject:IsA("Sky") then
            pcall(function() skyObject:Destroy() end)
        end
    end

    if #OriginalSkies > 0 then
        for _, savedSky in ipairs(OriginalSkies) do
            pcall(function()
                savedSky:Clone().Parent = Lighting
            end)
        end
    else
        -- Caso o jogo não tivesse um objeto Sky no início, usa o céu padrão do Roblox.
        local defaultSky = Instance.new("Sky")
        defaultSky.Name = "CapetinhaHub_DefaultSky"
        defaultSky.SkyboxBk = "rbxasset://textures/sky/sky512_bk.tex"
        defaultSky.SkyboxDn = "rbxasset://textures/sky/sky512_dn.tex"
        defaultSky.SkyboxFt = "rbxasset://textures/sky/sky512_ft.tex"
        defaultSky.SkyboxLf = "rbxasset://textures/sky/sky512_lf.tex"
        defaultSky.SkyboxRt = "rbxasset://textures/sky/sky512_rt.tex"
        defaultSky.SkyboxUp = "rbxasset://textures/sky/sky512_up.tex"
        defaultSky.Parent = Lighting
    end

    SetSkyStatus("céu original restaurado")
end

-- Gramados
local OriginalTerrainGrassColor
if Terrain then
    pcall(function() OriginalTerrainGrassColor = Terrain:GetMaterialColor(Enum.Material.Grass) end)
end
local OriginalGrassParts = setmetatable({}, {__mode="k"})

local GRASS_PRESETS = {
    {"🔵 Azul",Color3.fromRGB(20,90,255)},
    {"🧊 Azul Gelo",Color3.fromRGB(70,190,255)},
    {"🟣 Roxo",Color3.fromRGB(120,40,200)},
    {"🔴 Vermelho",Color3.fromRGB(210,35,35)},
    {"🌸 Rosa",Color3.fromRGB(255,75,170)},
    {"⚪ Branco",Color3.fromRGB(225,225,225)},
    {"⚫ Preto",Color3.fromRGB(20,20,20)},
    {"🟢 Lime",Color3.fromRGB(120,255,25)}
}

local function IsGrassPart(part)
    if not part or not part:IsA("BasePart") or IsBall(part) or IsCharacterPart(part) then return false end
    if part.Size.X < 15 or part.Size.Z < 15 or not part.Anchored then return false end
    if part.Material == Enum.Material.Grass then return true end
    local n = string.lower(part.Name)
    for _, w in ipairs({"grass","grama","campo","field","pitch","turf","ground","lawn"}) do
        if string.find(n, w, 1, true) then return true end
    end
    local c = part.Color
    return part.Size.X >= 30 and part.Size.Z >= 30 and c.G > c.R * 1.10 and c.G > c.B * 1.10
end

local function ApplyGrass(color)
    if Terrain then pcall(function() Terrain:SetMaterialColor(Enum.Material.Grass, color) end) end
    for _, v in ipairs(Workspace:GetDescendants()) do
        if IsGrassPart(v) then
            if OriginalGrassParts[v] == nil then OriginalGrassParts[v] = v.Color end
            pcall(function() v.Color = color end)
        end
    end
end

local function RestoreGrass()
    if Terrain and OriginalTerrainGrassColor then
        pcall(function() Terrain:SetMaterialColor(Enum.Material.Grass, OriginalTerrainGrassColor) end)
    end
    for part, color in pairs(OriginalGrassParts) do
        if part and part.Parent then pcall(function() part.Color = color end) end
    end
    for p in pairs(OriginalGrassParts) do OriginalGrassParts[p] = nil end
end

-- Visuals da Bola (RGB/Fogo)
local OriginalBallVisuals = setmetatable({}, {__mode="k"})
local RGBBallConnection

local function SaveBallVisual(obj)
    if OriginalBallVisuals[obj] then return end
    if obj:IsA("BasePart") then
        OriginalBallVisuals[obj] = {T="Part",Color=obj.Color}
    elseif obj:IsA("Decal") or obj:IsA("Texture") then
        OriginalBallVisuals[obj] = {T="Texture",Color=obj.Color3,Transparency=obj.Transparency}
    elseif obj:IsA("SpecialMesh") then
        OriginalBallVisuals[obj] = {T="Mesh",VertexColor=obj.VertexColor}
    end
end

local function ColorBallObject(obj, color)
    SaveBallVisual(obj)
    pcall(function()
        if obj:IsA("BasePart") then obj.Color = color
        elseif obj:IsA("Decal") or obj:IsA("Texture") then obj.Color3 = color
        elseif obj:IsA("SpecialMesh") then obj.VertexColor = Vector3.new(color.R, color.G, color.B) end
    end)
end

local function ApplyColorToBall(ball, color)
    if not ball or not ball.Parent or not ball:IsA("BasePart") or not IsBall(ball) then return end
    ColorBallObject(ball, color)
    for _, v in ipairs(ball:GetDescendants()) do
        if v:IsA("Decal") or v:IsA("Texture") or v:IsA("SpecialMesh") then
            ColorBallObject(v, color)
        end
    end
end

local function GetRGBBalls()
    local tps, otherBalls = {}, {}
    for _, part in ipairs(GetTrackedBalls()) do
        local largest = math.max(part.Size.X, part.Size.Y, part.Size.Z)
        if largest <= 8 then
            if string.lower(part.Name) == "tps" then
                table.insert(tps, part)
            else
                table.insert(otherBalls, part)
            end
        end
    end
    return #tps > 0 and tps or otherBalls
end

local function StopRGBBall()
    States.RGBBall = false
    if RGBBallConnection then RGBBallConnection:Disconnect() RGBBallConnection = nil end
end

local function StartRGBBall()
    if States.RGBBall then return end
    States.RGBBall = true
    local hue = 0
    local scanTimer = 0
    local paintTimer = 0
    local cachedBalls = {}

    -- Nao varre o mapa a cada frame: atualiza a lista a cada 1 segundo.
    RGBBallConnection = RunService.Heartbeat:Connect(function(dt)
        if not States.RGBBall then return end
        scanTimer += dt
        paintTimer += dt

        if scanTimer >= 1 then
            scanTimer = 0
            cachedBalls = GetRGBBalls()
        end
        -- 12 atualizacoes por segundo sao suficientes para o efeito RGB.
        if paintTimer >= (1/12) then
            paintTimer = 0
            hue = (hue + dt * 1.8) % 1
            local pulse = (math.sin(hue * math.pi * 2) + 1) / 2
            -- Pulso entre azul profundo e azul elétrico; sem ciclo RGB.
            local color = Color3.fromRGB(0, math.floor(35 + pulse * 95), math.floor(95 + pulse * 160))
            for i = #cachedBalls, 1, -1 do
                local ball = cachedBalls[i]
                if ball and ball.Parent then
                    ApplyColorToBall(ball, color)
                else
                    table.remove(cachedBalls, i)
                end
            end
        end
    end)
end

local BallFireConnection
local BallFireObjects = {}
local function StartBallFire()
    if States.BallFire then return end
    States.BallFire = true
    local scanTimer = 1
    BallFireConnection = RunService.Heartbeat:Connect(function(dt)
        if not States.BallFire then return end
        scanTimer += dt
        if scanTimer < 1 then return end
        scanTimer = 0
        -- Procura bolas uma vez por segundo, em vez de percorrer o mapa a cada frame.
        for _, ball in ipairs(GetRGBBalls()) do
            if ball and ball.Parent and not ball:FindFirstChild("vnFire") then
                local fire = Instance.new("Fire")
                fire.Name = "vnFire"
                fire.Size = 5
                fire.Heat = 10
                fire.Color = Color3.fromRGB(0, 145, 255)
                fire.SecondaryColor = Color3.fromRGB(0, 8, 24)
                fire.Parent = ball
                table.insert(BallFireObjects, fire)
            end
        end
    end)
end

local function StopBallFire()
    States.BallFire = false
    if BallFireConnection then BallFireConnection:Disconnect() BallFireConnection = nil end
    for i = #BallFireObjects, 1, -1 do
        local obj = BallFireObjects[i]
        if obj then pcall(function() obj:Destroy() end) end
        BallFireObjects[i] = nil
    end
end

-- ================= REACH REDONDA (BUSCA OTIMIZADA) =================
local ReachConnection
local ReachDistance = 6
local ReachVisible = true
local ReachAccumulator = 0
local ReachScanAccumulator = 0
local ReachCachedBall
local ReachSphere
local ReachOverlap = OverlapParams.new()
ReachOverlap.FilterType = Enum.RaycastFilterType.Exclude

local function CreateReachSphere()
    if ReachSphere and ReachSphere.Parent then return end
    ReachSphere = Instance.new("Part")
    ReachSphere.Name = "CapetinhaHub_ReachVisual"
    ReachSphere.Shape = Enum.PartType.Ball
    ReachSphere.Anchored = true
    ReachSphere.CanCollide = false
    ReachSphere.CanTouch = false
    ReachSphere.CanQuery = false
    ReachSphere.Material = Enum.Material.ForceField
    ReachSphere.Color = Color3.fromRGB(0, 145, 255)
    ReachSphere.Transparency = ReachVisible and 0.82 or 1
    ReachSphere.Size = Vector3.new(ReachDistance * 2, ReachDistance * 2, ReachDistance * 2)
    ReachSphere.Parent = Workspace
end

local function UpdateReachSphere()
    if not ReachSphere or not ReachSphere.Parent then return end
    local char = LP.Character
    local root = char and char:FindFirstChild("HumanoidRootPart")
    if root then
        ReachSphere.Position = root.Position
        ReachSphere.Size = Vector3.new(ReachDistance * 2, ReachDistance * 2, ReachDistance * 2)
        ReachSphere.Transparency = ReachVisible and 0.82 or 1
    else
        ReachSphere.Transparency = 1
    end
end

local function SetReachDistance(value)
    ReachDistance = math.clamp(math.floor(tonumber(value) or 6), 1, 15)
    UpdateReachSphere()
end

local function SetReachVisible(value)
    ReachVisible = value
    if ReachSphere then ReachSphere.Transparency = value and 0.82 or 1 end
end

local function FindReachBall(root, character)
    ReachOverlap.FilterDescendantsInstances = {character}
    local nearby = Workspace:GetPartBoundsInRadius(root.Position, ReachDistance + 3, ReachOverlap)
    local best, bestDistance = nil, math.huge
    for _, part in ipairs(nearby) do
        if IsBall(part) then
            local distance = (part.Position - root.Position).Magnitude
            if distance < bestDistance then
                best, bestDistance = part, distance
            end
        end
    end
    return best
end

local function StartReach()
    if States.Reach then return end
    States.Reach = true
    ReachAccumulator = 0
    ReachScanAccumulator = 0
    ReachCachedBall = nil
    CreateReachSphere()
    UpdateReachSphere()

    -- Atualiza o alvo em intervalo curto e aplica o toque sem varrer o mapa a cada frame.
    ReachConnection = RunService.Heartbeat:Connect(function(dt)
        if not States.Reach then return end
        ReachAccumulator += dt
        ReachScanAccumulator += dt
        UpdateReachSphere()

        local char = LP.Character
        local root = char and char:FindFirstChild("HumanoidRootPart")
        if not root then
            ReachCachedBall = nil
            return
        end

        if ReachScanAccumulator >= 0.12 or not ReachCachedBall or not ReachCachedBall.Parent then
            ReachScanAccumulator = 0
            ReachCachedBall = FindReachBall(root, char)
        end

        if ReachAccumulator >= 0.045 then
            ReachAccumulator = 0
            local ball = ReachCachedBall
            if ball and ball.Parent and (ball.Position - root.Position).Magnitude <= ReachDistance then
                local rightFoot = char:FindFirstChild("RightFoot") or char:FindFirstChild("Right Leg")
                local leftFoot = char:FindFirstChild("LeftFoot") or char:FindFirstChild("Left Leg")
                pcall(function()
                    if firetouchinterest then
                        firetouchinterest(root, ball, 0)
                        firetouchinterest(root, ball, 1)
                        if rightFoot then
                            firetouchinterest(rightFoot, ball, 0)
                            firetouchinterest(rightFoot, ball, 1)
                        end
                        if leftFoot then
                            firetouchinterest(leftFoot, ball, 0)
                            firetouchinterest(leftFoot, ball, 1)
                        end
                    end
                end)
            end
        end
    end)
end

local function StopReach()
    States.Reach = false
    ReachCachedBall = nil
    if ReachConnection then
        ReachConnection:Disconnect()
        ReachConnection = nil
    end
    if ReachSphere then
        ReachSphere:Destroy()
        ReachSphere = nil
    end
end

-- Navegação lateral com estado ativo amarelo
local ActiveTabButton
local function CreateTab(name, callback)
    local TabBtn = Instance.new("TextButton", TabBar)
    TabBtn.Size = UDim2.new(1, 0, 0, 31)
    TabBtn.BackgroundColor3 = Color3.fromRGB(8, 8, 8)
    TabBtn.Text = "   " .. name
    TabBtn.TextColor3 = Color3.fromRGB(155, 155, 155)
    TabBtn.Font = Enum.Font.GothamSemibold
    TabBtn.TextSize = 11
    TabBtn.TextXAlignment = Enum.TextXAlignment.Left
    TabBtn.AutoButtonColor = false
    TabBtn.LayoutOrder = #TabBar:GetChildren()
    Instance.new("UICorner", TabBtn).CornerRadius = UDim.new(0, 6)
    local marker = Instance.new("Frame", TabBtn)
    marker.Size = UDim2.new(0, 3, 1, -10)
    marker.Position = UDim2.new(0, 3, 0, 5)
    marker.BackgroundColor3 = Color3.fromRGB(0, 145, 255)
    marker.Visible = false
    marker.BorderSizePixel = 0
    Instance.new("UICorner", marker).CornerRadius = UDim.new(1, 0)
    TabBtn.MouseButton1Click:Connect(function()
        if ActiveTabButton then
            ActiveTabButton.BackgroundColor3 = Color3.fromRGB(8, 8, 8)
            ActiveTabButton.TextColor3 = Color3.fromRGB(155, 155, 155)
            local oldMarker = ActiveTabButton:FindFirstChild("ActiveMarker")
            if oldMarker then oldMarker.Visible = false end
        end
        ActiveTabButton = TabBtn
        TabBtn.BackgroundColor3 = Color3.fromRGB(5, 20, 34)
        TabBtn.TextColor3 = Color3.fromRGB(0, 165, 255)
        marker.Visible = true
        ContentTitle.Text = string.upper(name)
        ClearContainer()
        callback()
    end)
    marker.Name = "ActiveMarker"
    return TabBtn
end

-- Painel independente de ajuste do Reach
local ReachPanel = Instance.new("Frame")
ReachPanel.Name = "CapetinhaHub_ReachPanel"
ReachPanel.Size = UDim2.new(0, 190, 0, 126)
ReachPanel.Position = UDim2.new(1, -205, 0.5, -63)
ReachPanel.BackgroundColor3 = Color3.fromRGB(2, 2, 2)
ReachPanel.BorderSizePixel = 0
ReachPanel.Visible = false
ReachPanel.Parent = GUI
Instance.new("UICorner", ReachPanel).CornerRadius = UDim.new(0, 9)
local ReachPanelStroke = Instance.new("UIStroke", ReachPanel)
ReachPanelStroke.Color = Color3.fromRGB(18, 18, 18)
ReachPanelStroke.Thickness = 1

local ReachPanelTitle = Instance.new("TextLabel", ReachPanel)
ReachPanelTitle.Size = UDim2.new(1, -12, 0, 26)
ReachPanelTitle.Position = UDim2.new(0, 8, 0, 4)
ReachPanelTitle.BackgroundTransparency = 1
ReachPanelTitle.Text = "CAPETINHA • REACH"
ReachPanelTitle.TextColor3 = Color3.fromRGB(225, 225, 225)
ReachPanelTitle.Font = Enum.Font.GothamBold
ReachPanelTitle.TextSize = 12
ReachPanelTitle.TextXAlignment = Enum.TextXAlignment.Left

local ReachValue = Instance.new("TextLabel", ReachPanel)
ReachValue.Size = UDim2.new(0, 70, 0, 28)
ReachValue.Position = UDim2.new(0.5, -35, 0, 36)
ReachValue.BackgroundColor3 = Color3.fromRGB(5, 5, 5)
ReachValue.Text = tostring(ReachDistance)
ReachValue.TextColor3 = Color3.fromRGB(255, 255, 255)
ReachValue.Font = Enum.Font.GothamBold
ReachValue.TextSize = 14
ReachValue.BorderSizePixel = 0
Instance.new("UICorner", ReachValue).CornerRadius = UDim.new(0, 6)

local function MakeReachAdjustButton(label, x, callback)
    local b = Instance.new("TextButton", ReachPanel)
    b.Size = UDim2.new(0, 34, 0, 28)
    b.Position = UDim2.new(0, x, 0, 36)
    b.BackgroundColor3 = Color3.fromRGB(10, 10, 10)
    b.Text = label
    b.TextColor3 = Color3.fromRGB(220, 220, 220)
    b.Font = Enum.Font.GothamBold
    b.TextSize = 16
    b.BorderSizePixel = 0
    Instance.new("UICorner", b).CornerRadius = UDim.new(0, 6)
    b.MouseButton1Click:Connect(callback)
end

MakeReachAdjustButton("−", 10, function()
    SetReachDistance(ReachDistance - 1)
    ReachValue.Text = tostring(ReachDistance)
end)
MakeReachAdjustButton("+", 146, function()
    SetReachDistance(ReachDistance + 1)
    ReachValue.Text = tostring(ReachDistance)
end)

local ReachVisualButton = Instance.new("TextButton", ReachPanel)
ReachVisualButton.Size = UDim2.new(1, -20, 0, 28)
ReachVisualButton.Position = UDim2.new(0, 10, 0, 82)
ReachVisualButton.BackgroundColor3 = ReachVisible and Color3.fromRGB(0, 145, 255) or Color3.fromRGB(10, 10, 10)
ReachVisualButton.Text = "Visualização: ligada"
ReachVisualButton.TextColor3 = Color3.fromRGB(255, 255, 255)
ReachVisualButton.Font = Enum.Font.GothamSemibold
ReachVisualButton.TextSize = 11
ReachVisualButton.BorderSizePixel = 0
Instance.new("UICorner", ReachVisualButton).CornerRadius = UDim.new(0, 6)
ReachVisualButton.MouseButton1Click:Connect(function()
    SetReachVisible(not ReachVisible)
    ReachVisualButton.Text = ReachVisible and "Visualização: ligada" or "Visualização: desligada"
    ReachVisualButton.BackgroundColor3 = ReachVisible and Color3.fromRGB(0, 145, 255) or Color3.fromRGB(10, 10, 10)
end)
MakeDraggable(ReachPanel, ReachPanelTitle)

local function LoadMainTab()
    CreateLabel("LÓGICAS DE JOGO")
    CreateToggle("Auto Seguir Bola (K)", function(v)
        if v then StartAutoFollowBall() else StopAutoFollowBall() end
    end)
    CreateToggle("Reach", function(v)
        if v then
            StartReach()
            ReachPanel.Visible = true
        else
            StopReach()
            ReachPanel.Visible = false
        end
    end)
    CreateToggle("Auto Catch / Defesa", function(v)
        if v then StartAutoCatch() else StopAutoCatch() end
    end)
end

local function LoadVisualsTab()
    CreateLabel("EFEITOS AZUIS")
    CreateToggle("Bola Azul Neon", function(v)
        if v then StartRGBBall() else StopRGBBall() end
    end)
    CreateToggle("Chama Azul na Bola", function(v)
        if v then StartBallFire() else StopBallFire() end
    end)
end

local function LoadGrassTab()
    CreateLabel("CORES DE GRAMADO")
    CreateButton("Restaurar Gramado Padrão", function() RestoreGrass() end)
    for _, preset in ipairs(GRASS_PRESETS) do
        CreateButton(preset[1], function() ApplyGrass(preset[2]) end)
    end
end

local function LoadSkyTab()
    CreateLabel("SKYBOXES • MUDANÇA LOCAL")
    CreateLabel("Aguarde o carregamento das seis imagens antes da troca.")
    SkyStatusLabel = CreateLabel("STATUS: pronto")
    CreateButton("Restaurar Céu Padrão", function() RestoreSky() end)
    for _, sky in ipairs(SKYBOXES) do
        CreateButton(sky.N .. " (" .. sky.C .. ")", function()
            ApplySkybox(sky)
        end)
    end
end


--=====================================================
-- ANTI-LAG LOCAL (ALTERAÇÕES REVERSÍVEIS)
--=====================================================
local function NewWeakBackup()
    return setmetatable({}, {__mode = "k"})
end

local GeneralBackup = {}
local ShadowBackup = NewWeakBackup()
local ParticleBackup = NewWeakBackup()
local PostEffectBackup = NewWeakBackup()
local MaterialBackup = NewWeakBackup()
local BallTextureBackup = NewWeakBackup()
local FogBackup = {FogStart = nil, FogEnd = nil, FogColor = nil, Atmospheres = NewWeakBackup()}

local function SaveAndSet(cache, object, property, value)
    if not object then return end
    local saved = cache[object]
    if not saved then
        saved = {}
        cache[object] = saved
    end
    if saved[property] == nil then
        local ok, oldValue = pcall(function() return object[property] end)
        if not ok then return end
        saved[property] = oldValue
    end
    pcall(function() object[property] = value end)
end

local function RestoreProperties(cache)
    for object, properties in pairs(cache) do
        if object and object.Parent then
            for property, oldValue in pairs(properties) do
                pcall(function() object[property] = oldValue end)
            end
        end
        cache[object] = nil
    end
end

local function SetGeneralOptimization(enabled)
    if enabled then
        if GeneralBackup.Active then return end
        GeneralBackup.Active = true
        local function saveLighting(property, reducedValue)
            if GeneralBackup[property] == nil then
                local ok, oldValue = pcall(function() return Lighting[property] end)
                if ok then GeneralBackup[property] = oldValue end
            end
            pcall(function() Lighting[property] = reducedValue end)
        end
        saveLighting("GlobalShadows", false)
        saveLighting("EnvironmentDiffuseScale", 0)
        saveLighting("EnvironmentSpecularScale", 0)

        if Terrain then
            for _, property in ipairs({"Decoration", "WaterWaveSize", "WaterWaveSpeed"}) do
                local ok, oldValue = pcall(function() return Terrain[property] end)
                if ok then
                    GeneralBackup["Terrain_" .. property] = oldValue
                    local reducedValue = 0
                    if property == "Decoration" then reducedValue = false end
                    pcall(function() Terrain[property] = reducedValue end)
                end
            end
        end
    else
        if not GeneralBackup.Active then return end
        for property, oldValue in pairs(GeneralBackup) do
            if property ~= "Active" and not string.find(property, "^Terrain_") then
                pcall(function() Lighting[property] = oldValue end)
            elseif string.find(property, "^Terrain_") and Terrain then
                local terrainProperty = string.gsub(property, "^Terrain_", "")
                pcall(function() Terrain[terrainProperty] = oldValue end)
            end
            GeneralBackup[property] = nil
        end
        GeneralBackup.Active = nil
    end
end

local function SetShadows(enabled)
    if enabled then
        for _, object in ipairs(Workspace:GetDescendants()) do
            if object:IsA("BasePart") then
                SaveAndSet(ShadowBackup, object, "CastShadow", false)
            end
        end
    else
        RestoreProperties(ShadowBackup)
    end
end

local function SetParticles(enabled)
    local visualClasses = {
        ParticleEmitter = true, Trail = true, Beam = true,
        Smoke = true, Fire = true, Sparkles = true
    }
    if enabled then
        for _, object in ipairs(Workspace:GetDescendants()) do
            if visualClasses[object.ClassName] then
                SaveAndSet(ParticleBackup, object, "Enabled", false)
            end
        end
    else
        RestoreProperties(ParticleBackup)
    end
end

local function SetPostEffects(enabled)
    if enabled then
        for _, object in ipairs(Lighting:GetDescendants()) do
            if object:IsA("PostEffect") then
                SaveAndSet(PostEffectBackup, object, "Enabled", false)
            end
        end
    else
        RestoreProperties(PostEffectBackup)
    end
end

local function SetMaterials(enabled)
    if enabled then
        for _, object in ipairs(Workspace:GetDescendants()) do
            if object:IsA("BasePart") then
                SaveAndSet(MaterialBackup, object, "Material", Enum.Material.SmoothPlastic)
                SaveAndSet(MaterialBackup, object, "Reflectance", 0)
            end
        end
    else
        RestoreProperties(MaterialBackup)
    end
end

local function SetBallTextures(enabled)
    if enabled then
        local function hideTexture(object)
            if object:IsA("Decal") or object:IsA("Texture") then
                SaveAndSet(BallTextureBackup, object, "Transparency", 1)
            elseif object:IsA("SpecialMesh") then
                SaveAndSet(BallTextureBackup, object, "TextureId", "")
            elseif object:IsA("MeshPart") then
                SaveAndSet(BallTextureBackup, object, "TextureID", "")
            end
        end
        for _, ball in ipairs(GetRGBBalls()) do
            if ball and ball.Parent then
                hideTexture(ball)
                for _, object in ipairs(ball:GetDescendants()) do
                    hideTexture(object)
                end
            end
        end
    else
        RestoreProperties(BallTextureBackup)
    end
end

local function SetFog(enabled)
    if enabled then
        if FogBackup.FogStart == nil then
            FogBackup.FogStart = Lighting.FogStart
            FogBackup.FogEnd = Lighting.FogEnd
            FogBackup.FogColor = Lighting.FogColor
        end
        Lighting.FogStart = 0
        Lighting.FogEnd = 1000000
        for _, object in ipairs(Lighting:GetDescendants()) do
            if object:IsA("Atmosphere") then
                SaveAndSet(FogBackup.Atmospheres, object, "Density", 0)
                SaveAndSet(FogBackup.Atmospheres, object, "Haze", 0)
                SaveAndSet(FogBackup.Atmospheres, object, "Glare", 0)
            end
        end
    else
        if FogBackup.FogStart ~= nil then
            Lighting.FogStart = FogBackup.FogStart
            Lighting.FogEnd = FogBackup.FogEnd
            Lighting.FogColor = FogBackup.FogColor
            FogBackup.FogStart, FogBackup.FogEnd, FogBackup.FogColor = nil, nil, nil
        end
        RestoreProperties(FogBackup.Atmospheres)
    end
end

local function LoadAntiLagTab()
    CreateLabel("OTIMIZACAO DE DESEMPENHO")
    CreateToggle("Anti-Lag geral", function(v)
        SetGeneralOptimization(v)
    end)
    CreateToggle("Remover texturas da bola", function(v)
        SetBallTextures(v)
    end)
    CreateToggle("Desativar sombras das peças", function(v)
        SetShadows(v)
    end)
    CreateToggle("Remover neblina / atmosfera", function(v)
        SetFog(v)
    end)
    CreateToggle("Desativar partículas e trilhas", function(v)
        SetParticles(v)
    end)
    CreateToggle("Desativar pós-processamento", function(v)
        SetPostEffects(v)
    end)
    CreateToggle("Materiais leves", function(v)
        SetMaterials(v)
    end)
    CreateLabel("As opções são locais e podem ser revertidas ao desligar")
end

local function LoadInfoTab()
    CreateLabel("CAPETINHA HUB • BLUE DUCK")
    CreateButton("GOAT VN • CRIADOR / OWNER", function() end)
    CreateButton("Discord: GOAT VN", function() end)
    CreateLabel("TEMA")
    CreateButton("Preto profundo + azul elétrico", function() end)
    CreateLabel("RECURSOS DISPONÍVEIS")
    CreateButton("Auto Seguir Bola • tecla K", function() end)
    CreateButton("Reach • ajuste de 1 a 15", function() end)
    CreateButton("Auto Catch / Defesa", function() end)
    CreateButton("Efeitos, gramado, céu e anti-lag", function() end)
end


--=====================================================
-- ABAS CHAR E SILENT
--=====================================================

local TextChatService = game:GetService("TextChatService")
local ReplicatedStorage = game:GetService("ReplicatedStorage")

local CHARACTERS = {
    "Tudrax0",
    "JGH_JEAN",
    "yurinho_0011",
    "feliiou"
}

local function SendCharCommand(username)
    local command = ":char " .. username

    -- Chat novo do Roblox
    local sent = pcall(function()
        local channels = TextChatService:FindFirstChild("TextChannels")
        local general = channels and channels:FindFirstChild("RBXGeneral")
        if not general then error("Canal geral não encontrado") end
        general:SendAsync(command)
    end)

    -- Compatibilidade com experiências que ainda usam o chat antigo
    if not sent then
        pcall(function()
            local events = ReplicatedStorage:FindFirstChild("DefaultChatSystemChatEvents")
            local say = events and events:FindFirstChild("SayMessageRequest")
            if not say then error("Chat antigo não disponível") end
            say:FireServer(command, "All")
        end)
    end
end

local function LoadCharTab()
    CreateLabel("ESCOLHA O CHAR")
    CreateLabel("Clique em um nome para enviar o comando no chat.")
    for _, username in ipairs(CHARACTERS) do
        CreateButton(":char " .. username, function()
            SendCharCommand(username)
        end)
    end
end

local function LoadSilentTab()
    CreateLabel("SILENT")
    CreateLabel("Silent Pass")
    CreateLabel("A ideia é selecionar como alvo o amigo que sua câmera estiver mirando.")
    CreateLabel("Silent Goal")
    CreateLabel("Assistência de finalização direcionada ao gol.")
    CreateLabel("STATUS: não implementado")
    CreateLabel("Esses recursos dependem da lógica de bola validada pelo servidor do jogo.")
    CreateLabel("A source enviada não contém os eventos ou a API do jogo necessários para implementá-los com segurança.")
end

--=====================================================
-- FORCED: AJUSTE LOCAL DE FORÇA
--=====================================================
local KickForceLevel = 1
local KickForceValueLabel

local function RefreshKickForceLabel()
    if KickForceValueLabel and KickForceValueLabel.Parent then
        KickForceValueLabel.Text = "▸ Força configurada: " .. tostring(KickForceLevel) .. "/10"
    end
end

local function LoadForcedTab()
    CreateLabel("FORÇA DO CHUTE")
    KickForceValueLabel = CreateLabel("Força configurada: " .. tostring(KickForceLevel) .. "/10")
    CreateButton("Diminuir força (−)", function()
        KickForceLevel = math.clamp(KickForceLevel - 1, 1, 10)
        RefreshKickForceLabel()
    end)
    CreateButton("Aumentar força (+)", function()
        KickForceLevel = math.clamp(KickForceLevel + 1, 1, 10)
        RefreshKickForceLabel()
    end)
    CreateLabel("Este valor é uma configuração local. A source não contém a rotina ou o evento de chute para aplicar a força no jogo.")
end

-- EVENTOS DE INTERFACE
CloseBtn.MouseButton1Click:Connect(function() ToggleWindow(false) end)
ToggleBtn.MouseButton1Click:Connect(function() ToggleWindow() end)

-- MONTAGEM DA UI
CreateTab("Info", LoadInfoTab)
CreateTab("Principal", LoadMainTab)
CreateTab("Efeitos", LoadVisualsTab)
CreateTab("Gramado", LoadGrassTab)
CreateTab("Skybox", LoadSkyTab)
CreateTab("Anti-Lag", LoadAntiLagTab)
CreateTab("CHAR", LoadCharTab)
CreateTab("Silent", LoadSilentTab)
CreateTab("forced", LoadForcedTab)

ContentTitle.Text = "INFORMAÇÕES"
LoadInfoTab()
ToggleWindow(true)
