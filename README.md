--[[
    ISCZ HUB - Performance Edition
    Versão: KEY TESTE
    Key: PRGV

    Coloque este LocalScript em StarterPlayer > StarterPlayerScripts
]]

local Players = game:GetService("Players")
local Lighting = game:GetService("Lighting")
local UserInputService = game:GetService("UserInputService")
local RunService = game:GetService("RunService")

local player = Players.LocalPlayer
local playerGui = player:WaitForChild("PlayerGui")

local KEY = "PRGV"

--// GUI
local gui = Instance.new("ScreenGui")
gui.Name = "ISCZ_HUB"
gui.ResetOnSpawn = false
gui.ZIndexBehavior = Enum.ZIndexBehavior.Sibling
gui.Parent = playerGui

local function corner(obj, radius)
    local c = Instance.new("UICorner")
    c.CornerRadius = UDim.new(0, radius or 10)
    c.Parent = obj
end

local function stroke(obj, color, transparency)
    local s = Instance.new("UIStroke")
    s.Color = color or Color3.fromRGB(90, 90, 105)
    s.Transparency = transparency or 0.35
    s.Thickness = 1
    s.Parent = obj
end

local function label(parent, text, size, pos, fontSize)
    local l = Instance.new("TextLabel")
    l.BackgroundTransparency = 1
    l.Text = text
    l.TextColor3 = Color3.fromRGB(235,235,240)
    l.Font = Enum.Font.GothamMedium
    l.TextSize = fontSize or 13
    l.Size = size
    l.Position = pos
    l.TextXAlignment = Enum.TextXAlignment.Left
    l.Parent = parent
    return l
end

--// Key screen
local keyFrame = Instance.new("Frame")
keyFrame.Size = UDim2.fromOffset(330, 215)
keyFrame.Position = UDim2.fromScale(0.5, 0.5)
keyFrame.AnchorPoint = Vector2.new(0.5, 0.5)
keyFrame.BackgroundColor3 = Color3.fromRGB(20,20,24)
keyFrame.Parent = gui
corner(keyFrame, 16)
stroke(keyFrame, Color3.fromRGB(115,115,135), 0.25)

label(keyFrame, "ISCZ HUB", UDim2.new(1,-40,0,32), UDim2.fromOffset(20,20), 22)
local sub = label(keyFrame, "Performance Edition", UDim2.new(1,-40,0,22), UDim2.fromOffset(20,51), 12)
sub.TextColor3 = Color3.fromRGB(155,155,165)

local box = Instance.new("TextBox")
box.Size = UDim2.new(1,-40,0,42)
box.Position = UDim2.fromOffset(20,87)
box.BackgroundColor3 = Color3.fromRGB(30,30,35)
box.PlaceholderText = "Digite sua Key"
box.PlaceholderColor3 = Color3.fromRGB(120,120,130)
box.TextColor3 = Color3.fromRGB(240,240,245)
box.Font = Enum.Font.GothamMedium
box.TextSize = 14
box.ClearTextOnFocus = false
box.Parent = keyFrame
corner(box, 10)
stroke(box)

local activate = Instance.new("TextButton")
activate.Size = UDim2.new(1,-40,0,40)
activate.Position = UDim2.fromOffset(20,137)
activate.BackgroundColor3 = Color3.fromRGB(75,75,90)
activate.Text = "ATIVAR"
activate.TextColor3 = Color3.new(1,1,1)
activate.Font = Enum.Font.GothamBold
activate.TextSize = 13
activate.Parent = keyFrame
corner(activate, 10)

local errorLabel = label(keyFrame, "", UDim2.new(1,-40,0,20), UDim2.fromOffset(20,180), 11)
errorLabel.TextColor3 = Color3.fromRGB(255,115,115)

--// Main UI
local main = Instance.new("Frame")
main.Size = UDim2.fromOffset(390, 430)
main.Position = UDim2.fromScale(0.5, 0.5)
main.AnchorPoint = Vector2.new(0.5,0.5)
main.BackgroundColor3 = Color3.fromRGB(20,20,24)
main.BackgroundTransparency = 0.04
main.Visible = false
main.Parent = gui
corner(main, 16)
stroke(main, Color3.fromRGB(100,100,115), 0.25)

-- Header
local header = Instance.new("Frame")
header.Size = UDim2.new(1,0,0,68)
header.BackgroundTransparency = 1
header.Parent = main

local title = label(header, "ISCZ HUB", UDim2.new(1,-100,0,27), UDim2.fromOffset(18,12), 20)
local version = label(header, "KEY TESTE  •  PRGV", UDim2.new(1,-100,0,20), UDim2.fromOffset(18,37), 10)
version.TextColor3 = Color3.fromRGB(150,150,160)

local minimize = Instance.new("TextButton")
minimize.Size = UDim2.fromOffset(34,34)
minimize.Position = UDim2.new(1,-48,0,17)
minimize.BackgroundColor3 = Color3.fromRGB(32,32,38)
minimize.Text = "—"
minimize.TextColor3 = Color3.fromRGB(220,220,225)
minimize.Font = Enum.Font.GothamBold
minimize.TextSize = 18
minimize.Parent = header
corner(minimize, 9)

-- Tabs
local tabs = Instance.new("Frame")
tabs.Size = UDim2.new(1,-28,0,36)
tabs.Position = UDim2.fromOffset(14,65)
tabs.BackgroundTransparency = 1
tabs.Parent = main

local tabNames = {"HOME","ANTI-LAG","TELA","CONFIG"}
local tabButtons = {}
for i,name in ipairs(tabNames) do
    local b = Instance.new("TextButton")
    b.Size = UDim2.new(0.25,-4,1,0)
    b.Position = UDim2.new((i-1)*0.25, i==1 and 0 or 4, 0, 0)
    b.BackgroundColor3 = i == 1 and Color3.fromRGB(58,58,70) or Color3.fromRGB(31,31,37)
    b.Text = name
    b.TextColor3 = Color3.fromRGB(220,220,225)
    b.Font = Enum.Font.GothamBold
    b.TextSize = 10
    b.Parent = tabs
    corner(b, 8)
    tabButtons[name] = b
end

local content = Instance.new("ScrollingFrame")
content.Size = UDim2.new(1,-28,1,-118)
content.Position = UDim2.fromOffset(14,110)
content.BackgroundTransparency = 1
content.BorderSizePixel = 0
content.ScrollBarThickness = 3
content.CanvasSize = UDim2.new()
content.Parent = main

local layout = Instance.new("UIListLayout")
layout.Padding = UDim.new(0,8)
layout.Parent = content

local padding = Instance.new("UIPadding")
padding.PaddingBottom = UDim.new(0,12)
padding.Parent = content

local function clearContent()
    for _,v in ipairs(content:GetChildren()) do
        if not v:IsA("UIListLayout") and not v:IsA("UIPadding") then
            v:Destroy()
        end
    end
end

local function section(text)
    local l = label(content, text, UDim2.new(1,-4,0,22), UDim2.new(), 12)
    l.TextColor3 = Color3.fromRGB(160,160,170)
    return l
end

local function button(text, callback)
    local b = Instance.new("TextButton")
    b.Size = UDim2.new(1,-4,0,42)
    b.BackgroundColor3 = Color3.fromRGB(31,31,37)
    b.Text = text
    b.TextColor3 = Color3.fromRGB(230,230,235)
    b.Font = Enum.Font.GothamMedium
    b.TextSize = 12
    b.Parent = content
    corner(b, 10)
    stroke(b, Color3.fromRGB(70,70,82), 0.55)
    b.MouseButton1Click:Connect(callback)
    return b
end

local function toggle(text, callback)
    local state = false
    local b
    b = button(text .. "  [ OFF ]", function()
        state = not state
        b.Text = text .. (state and "  [ ON ]" or "  [ OFF ]")
        b.BackgroundColor3 = state and Color3.fromRGB(48,65,53) or Color3.fromRGB(31,31,37)
        callback(state)
    end)
    return b
end

--// Performance functions
local originalLighting = {
    GlobalShadows = Lighting.GlobalShadows,
    FogEnd = Lighting.FogEnd,
    Brightness = Lighting.Brightness,
}

local function setLowGraphics()
    pcall(function()
        settings().Rendering.QualityLevel = Enum.QualityLevel.Level01
    end)
end

local function optimizeParticles(enabled)
    for _,obj in ipairs(workspace:GetDescendants()) do
        if obj:IsA("ParticleEmitter") or obj:IsA("Trail") or obj:IsA("Beam") then
            obj.Enabled = not enabled
        end
    end
end

local function optimizeShadows(enabled)
    Lighting.GlobalShadows = not enabled
end

local function optimizeEffects(enabled)
    for _,obj in ipairs(workspace:GetDescendants()) do
        if obj:IsA("BloomEffect") or obj:IsA("BlurEffect") or obj:IsA("ColorCorrectionEffect")
            or obj:IsA("SunRaysEffect") or obj:IsA("DepthOfFieldEffect") then
            obj.Enabled = not enabled
        end
    end
end

local function buildHome()
    clearContent()
    section("STATUS")
    local fpsLabel = label(content, "FPS: calculando...", UDim2.new(1,-4,0,25), UDim2.new(), 13)
    local pingLabel = label(content, "Ping: calculando...", UDim2.new(1,-4,0,25), UDim2.new(), 13)
    local status = label(content, "● Sistema pronto", UDim2.new(1,-4,0,25), UDim2.new(), 11)
    status.TextColor3 = Color3.fromRGB(130,220,150)

    section("OTIMIZAÇÃO RÁPIDA")
    button("⚡ Aplicar otimização recomendada", function()
        setLowGraphics()
        optimizeParticles(true)
        optimizeShadows(true)
        optimizeEffects(true)
        status.Text = "● Otimização aplicada"
    end)
    button("↩ Restaurar iluminação", function()
        Lighting.GlobalShadows = originalLighting.GlobalShadows
        Lighting.FogEnd = originalLighting.FogEnd
        Lighting.Brightness = originalLighting.Brightness
        status.Text = "● Iluminação restaurada"
    end)

    local frames, last = 0, tick()
    local conn
    conn = RunService.RenderStepped:Connect(function()
        frames += 1
        if tick()-last >= 1 then
            fpsLabel.Text = "FPS: " .. frames
            frames = 0
            last = tick()
            if not gui.Parent then conn:Disconnect() end
        end
    end)
end

local function buildAntiLag()
    clearContent()
    section("ANTI-LAG")
    toggle("Gráficos mínimos", function(on) if on then setLowGraphics() end end)
    toggle("Remover partículas", function(on) optimizeParticles(on) end)
    toggle("Reduzir sombras", function(on) optimizeShadows(on) end)
    toggle("Desativar efeitos", function(on) optimizeEffects(on) end)
    toggle("Modo Ultra Performance", function(on)
        if on then
            setLowGraphics()
            optimizeParticles(true)
            optimizeShadows(true)
            optimizeEffects(true)
        end
    end)
    button("🚀 APLICAR TUDO", function()
        setLowGraphics()
        optimizeParticles(true)
        optimizeShadows(true)
        optimizeEffects(true)
    end)
end

local stretchValue = 1
local function buildTela()
    clearContent()
    section("TELA ESTICADA")
    label(content, "Escala atual: 1.00x", UDim2.new(1,-4,0,25), UDim2.new(), 12)
    button("＋ Aumentar escala", function()
        stretchValue = math.clamp(stretchValue + 0.1, 0.8, 1.5)
        local cam = workspace.CurrentCamera
        if cam then cam.FieldOfView = math.clamp(70 + (stretchValue-1)*25, 50, 90) end
    end)
    button("－ Diminuir escala", function()
        stretchValue = math.clamp(stretchValue - 0.1, 0.8, 1.5)
        local cam = workspace.CurrentCamera
        if cam then cam.FieldOfView = math.clamp(70 + (stretchValue-1)*25, 50, 90) end
    end)
    button("↩ Resetar tela", function()
        stretchValue = 1
        local cam = workspace.CurrentCamera
        if cam then cam.FieldOfView = 70 end
    end)
    section("PRESETS")
    button("1.0x  •  Normal", function() stretchValue = 1 end)
    button("1.2x  •  Estendida", function() stretchValue = 1.2 end)
    button("1.4x  •  Forte", function() stretchValue = 1.4 end)
    button("1.5x  •  Máxima", function() stretchValue = 1.5 end)
end

local function buildConfig()
    clearContent()
    section("APARÊNCIA DO PAINEL")
    button("🔵 Azul", function()
        main.BackgroundColor3 = Color3.fromRGB(20,28,40)
    end)
    button("🟣 Roxo", function()
        main.BackgroundColor3 = Color3.fromRGB(30,22,42)
    end)
    button("🟢 Verde", function()
        main.BackgroundColor3 = Color3.fromRGB(20,38,29)
    end)
    button("🔴 Vermelho", function()
        main.BackgroundColor3 = Color3.fromRGB(42,22,24)
    end)
    button("⚫ Preto", function()
        main.BackgroundColor3 = Color3.fromRGB(15,15,18)
    end)
    button("⚪ Cinza", function()
        main.BackgroundColor3 = Color3.fromRGB(35,35,40)
    end)
    section("CONTROLES")
    button("↔ Tamanho compacto", function()
        main.Size = UDim2.fromOffset(350,390)
    end)
    button("↔ Tamanho padrão", function()
        main.Size = UDim2.fromOffset(390,430)
    end)
end

local function openTab(name)
    for n,b in pairs(tabButtons) do
        b.BackgroundColor3 = n == name and Color3.fromRGB(58,58,70) or Color3.fromRGB(31,31,37)
    end
    if name == "HOME" then buildHome()
    elseif name == "ANTI-LAG" then buildAntiLag()
    elseif name == "TELA" then buildTela()
    elseif name == "CONFIG" then buildConfig() end
end

for name,b in pairs(tabButtons) do
    b.MouseButton1Click:Connect(function() openTab(name) end)
end

--// Drag support
local dragging = false
local dragStart, startPos

header.InputBegan:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.MouseButton1
        or input.UserInputType == Enum.UserInputType.Touch then
        dragging = true
        dragStart = input.Position
        startPos = main.Position
    end
end)

UserInputService.InputChanged:Connect(function(input)
    if dragging and (input.UserInputType == Enum.UserInputType.MouseMovement
        or input.UserInputType == Enum.UserInputType.Touch) then
        local delta = input.Position - dragStart
        main.Position = UDim2.new(
            startPos.X.Scale, startPos.X.Offset + delta.X,
            startPos.Y.Scale, startPos.Y.Offset + delta.Y
        )
    end
end)

UserInputService.InputEnded:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.MouseButton1
        or input.UserInputType == Enum.UserInputType.Touch then
        dragging = false
    end
end)

--// Floating reopen button
local floating = Instance.new("TextButton")
floating.Size = UDim2.fromOffset(52,52)
floating.Position = UDim2.new(1,-70,1,-90)
floating.BackgroundColor3 = Color3.fromRGB(30,30,36)
floating.Text = "ISCZ"
floating.TextColor3 = Color3.fromRGB(235,235,240)
floating.Font = Enum.Font.GothamBold
floating.TextSize = 11
floating.Visible = false
floating.Parent = gui
corner(floating, 18)
stroke(floating, Color3.fromRGB(100,100,115), 0.25)

minimize.MouseButton1Click:Connect(function()
    main.Visible = false
    floating.Visible = true
end)

floating.MouseButton1Click:Connect(function()
    floating.Visible = false
    main.Visible = true
end)

--// Key validation
activate.MouseButton1Click:Connect(function()
    if string.upper(box.Text) == KEY then
        keyFrame.Visible = false
        main.Visible = true
        openTab("HOME")
    else
        errorLabel.Text = "Key inválida."
        box.Text = ""
    end
end)

box.FocusLost:Connect(function(enter)
    if enter then activate:Activate() end
end)
