-- scripter_box - Ultra Seguro
local RS = game:GetService("RunService")
local Players = game:GetService("Players")

local plr = Players.LocalPlayer
local vooOn, espOn = false, false
local posSalva = nil
local flutuando = false
local flutuarConn = nil
local vooConn = nil

-- velocidades controláveis
local velocidadeVoo = 26
local velocidadeFlutuar = 27
local velocidadePlayer = 16 -- WalkSpeed padrão

-----------------------------------------------------
-- LOADING
-----------------------------------------------------
task.spawn(function()
    local sg = Instance.new("ScreenGui", plr.PlayerGui)
    local txt = Instance.new("TextLabel", sg)
    txt.Size = UDim2.new(0,200,0,40)
    txt.Position = UDim2.new(0.5,-100,0.5,-20)
    txt.BackgroundTransparency = 1
    txt.Text = "scripter_box"
    txt.TextColor3 = Color3.fromRGB(0,200,255)
    txt.TextSize = 24
    txt.Font = Enum.Font.GothamBold
    txt.TextStrokeTransparency = 0.5
    task.wait(1.5)
    sg:Destroy()
end)
task.wait(1.5)

-----------------------------------------------------
-- KEY
-----------------------------------------------------
local function key()
    local sg = Instance.new("ScreenGui", plr.PlayerGui)
    local bg = Instance.new("Frame", sg)
    bg.Size = UDim2.new(1,0,1,0)
    bg.BackgroundColor3 = Color3.new(0,0,0)
    bg.BackgroundTransparency = 0.7
    
    local fr = Instance.new("Frame", bg)
    fr.Size = UDim2.new(0,280,0,140)
    fr.Position = UDim2.new(0.5,-140,0.5,-70)
    fr.BackgroundColor3 = Color3.fromRGB(25,25,30)
    Instance.new("UICorner", fr).CornerRadius = UDim.new(0,8)
    
    local t = Instance.new("TextLabel", fr)
    t.Size = UDim2.new(1,0,0,35)
    t.Position = UDim2.new(0,0,0,8)
    t.BackgroundTransparency = 1
    t.Text = "KEY"
    t.TextColor3 = Color3.fromRGB(0,200,255)
    t.TextSize = 18
    t.Font = Enum.Font.GothamBold
    
    local inp = Instance.new("TextBox", fr)
    inp.Size = UDim2.new(0.85,0,0,32)
    inp.Position = UDim2.new(0.075,0,0,50)
    inp.BackgroundColor3 = Color3.fromRGB(35,35,40)
    inp.PlaceholderText = "Edward1k"
    inp.TextColor3 = Color3.new(1,1,1)
    Instance.new("UICorner", inp).CornerRadius = UDim.new(0,5)
    
    local btn = Instance.new("TextButton", fr)
    btn.Size = UDim2.new(0.85,0,0,32)
    btn.Position = UDim2.new(0.075,0,0,95)
    btn.BackgroundColor3 = Color3.fromRGB(0,200,255)
    btn.Text = "OK"
    btn.TextColor3 = Color3.new(1,1,1)
    Instance.new("UICorner", btn).CornerRadius = UDim.new(0,5)

    local ok = false
    local function check()
        if inp.Text == "Edward1k" then
            ok = true
            sg:Destroy()
        else
            inp.Text = ""
        end
    end

    btn.MouseButton1Click:Connect(check)
    inp.FocusLost:Connect(function(e) if e then check() end end)

    repeat task.wait() until ok
end

key()

-----------------------------------------------------
-- GUI PRINCIPAL
-----------------------------------------------------
local sg = Instance.new("ScreenGui", plr.PlayerGui)
sg.ResetOnSpawn = false

local main = Instance.new("Frame", sg)
main.Size = UDim2.new(0,170,0,200)
main.Position = UDim2.new(0.5,-85,0.5,-100)
main.BackgroundColor3 = Color3.fromRGB(22,22,28)
main.Active = true
main.Draggable = true
Instance.new("UICorner", main).CornerRadius = UDim.new(0,8)

local top = Instance.new("TextLabel", main)
top.Size = UDim2.new(1,0,0,22)
top.BackgroundTransparency = 1
top.Text = "scripter_box"
top.TextColor3 = Color3.fromRGB(0,200,255)
top.TextSize = 12
top.Font = Enum.Font.GothamBold

local function btn(parent, txt, y, col)
    local b = Instance.new("TextButton", parent)
    b.Size = UDim2.new(0.88,0,0,28)
    b.Position = UDim2.new(0.06,0,y,0)
    b.BackgroundColor3 = col
    b.Text = txt
    b.TextColor3 = Color3.new(1,1,1)
    b.Font = Enum.Font.GothamBold
    b.TextSize = 11
    b.BackgroundTransparency = 0.15
    Instance.new("UICorner", b).CornerRadius = UDim.new(0,5)
    return b
end

-----------------------------------------------------
-- ABA PRINCIPAL
-----------------------------------------------------
local bVoo = btn(main, "🚀 VOO", 0.15, Color3.fromRGB(220,50,50))
local bEsp = btn(main, "👁 ESP", 0.35, Color3.fromRGB(220,50,50))
local bSave = btn(main, "📍 SALVAR POSIÇÃO", 0.55, Color3.fromRGB(50,100,220))
local bFlutuar = btn(main, "☁ FLUTUAR ATÉ", 0.75, Color3.fromRGB(120,60,220))

-- botão abrir aba config
local bConfig = btn(main, "⚙ CONFIGURAÇÕES", 0.87, Color3.fromRGB(0,150,255))

-----------------------------------------------------
-- ABA DE CONFIGURAÇÕES
-----------------------------------------------------
local config = Instance.new("Frame", sg)
config.Size = UDim2.new(0,170,0,180)
config.Position = UDim2.new(0.5,-85,0.5,-100)
config.BackgroundColor3 = Color3.fromRGB(18,18,24)
Instance.new("UICorner", config).CornerRadius = UDim.new(0,8)
config.Visible = false
config.Active = true
config.Draggable = true

local ct = Instance.new("TextLabel", config)
ct.Size = UDim2.new(1,0,0,22)
ct.BackgroundTransparency = 1
ct.Text = "CONFIG"
ct.TextColor3 = Color3.fromRGB(0,200,255)
ct.TextSize = 12
ct.Font = Enum.Font.GothamBold

local bSpeedVoo = btn(config, "✏ VELOCIDADE DO VOO", 0.18, Color3.fromRGB(0,150,255))
local bSpeedFlutuar = btn(config, "✏ VELOCIDADE FLUTUAR", 0.39, Color3.fromRGB(0,150,255))
local bSpeedPlayer = btn(config, "✏ SPEED PLAYER", 0.60, Color3.fromRGB(0,150,255))

local bVoltar = btn(config, "⬅ VOLTAR", 0.80, Color3.fromRGB(200,60,60))

-----------------------------------------------------
-- MOSTRAR/OCULTAR ABA CONFIG
-----------------------------------------------------
bConfig.MouseButton1Click:Connect(function()
    config.Visible = true
    main.Visible = false
end)

bVoltar.MouseButton1Click:Connect(function()
    config.Visible = false
    main.Visible = true
end)

-----------------------------------------------------
-- POPUP PARA DIGITAR NÚMERO
-----------------------------------------------------
local function pedirNumero(titulo, callback)
    local sg2 = Instance.new("ScreenGui", plr.PlayerGui)

    local fr = Instance.new("Frame", sg2)
    fr.Size = UDim2.new(0,230,0,130)
    fr.Position = UDim2.new(0.5,-115,0.5,-65)
    fr.BackgroundColor3 = Color3.fromRGB(25,25,30)
    Instance.new("UICorner", fr).CornerRadius = UDim.new(0,8)

    local t = Instance.new("TextLabel", fr)
    t.Size = UDim2.new(1,0,0,30)
    t.Position = UDim2.new(0,0,0,5)
    t.Text = titulo
    t.TextColor3 = Color3.fromRGB(0,200,255)
    t.BackgroundTransparency = 1

    local inp = Instance.new("TextBox", fr)
    inp.Size = UDim2.new(0.8,0,0,32)
    inp.Position = UDim2.new(0.1,0,0,45)
    inp.BackgroundColor3 = Color3.fromRGB(35,35,40)
    inp.PlaceholderText = "Digite..."
    inp.TextColor3 = Color3.new(1,1,1)
    Instance.new("UICorner", inp).CornerRadius = UDim.new(0,5)

    local ok = Instance.new("TextButton", fr)
    ok.Size = UDim2.new(0.8,0,0,30)
    ok.Position = UDim2.new(0.1,0,0,85)
    ok.Text = "OK"
    ok.BackgroundColor3 = Color3.fromRGB(0,200,255)
    ok.TextColor3 = Color3.new(1,1,1)
    Instance.new("UICorner", ok).CornerRadius = UDim.new(0,5)

    ok.MouseButton1Click:Connect(function()
        local n = tonumber(inp.Text)
        if n then
            callback(n)
            sg2:Destroy()
        else
            inp.Text = ""
        end
    end)
end

-----------------------------------------------------
-- DEFINIR VELOCIDADES
-----------------------------------------------------
bSpeedVoo.MouseButton1Click:Connect(function()
    pedirNumero("Velocidade do Voo", function(v)
        velocidadeVoo = v
    end)
end)

bSpeedFlutuar.MouseButton1Click:Connect(function()
    pedirNumero("Velocidade Flutuar", function(v)
        velocidadeFlutuar = v
    end)
end)

bSpeedPlayer.MouseButton1Click:Connect(function()
    pedirNumero("Speed do Player", function(v)
        velocidadePlayer = v
        local char = plr.Character
        if char and char:FindFirstChild("Humanoid") then
            char.Humanoid.WalkSpeed = v
        end
    end)
end)

-----------------------------------------------------
-- VOO
-----------------------------------------------------
bVoo.MouseButton1Click:Connect(function()
    vooOn = not vooOn

    if vooOn then
        bVoo.Text = "✅ VOO"
        bVoo.BackgroundColor3 = Color3.fromRGB(50,200,50)

        vooConn = RS.Heartbeat:Connect(function()
            local char = plr.Character
            if not char or not vooOn then return end

            local r = char:FindFirstChild("HumanoidRootPart")
            if not r then return end

            local cam = workspace.CurrentCamera
            r.Velocity = cam.CFrame.LookVector * velocidadeVoo
        end)

    else
        bVoo.Text = "🚀 VOO"
        bVoo.BackgroundColor3 = Color3.fromRGB(220,50,50)
        if vooConn then vooConn:Disconnect() end
    end
end)

-----------------------------------------------------
-- ESP
-----------------------------------------------------
bEsp.MouseButton1Click:Connect(function()
    espOn = not espOn

    if espOn then
        bEsp.Text = "✅ ESP"
        bEsp.BackgroundColor3 = Color3.fromRGB(50,200,50)
    else
        bEsp.Text = "👁 ESP"
        bEsp.BackgroundColor3 = Color3.fromRGB(220,50,50)
        for _,p in pairs(Players:GetPlayers()) do
            if p~=plr and p.Character then
                for _,v in pairs(p.Character:GetDescendants()) do
                    if v:IsA("Highlight") then v:Destroy() end
                end
            end
        end
    end
end)

task.spawn(function()
    while task.wait(0.5) do
        if espOn then
            for _,p in pairs(Players:GetPlayers()) do
                if p~=plr and p.Character and not p.Character:FindFirstChild("Highlight") then
                    local h = Instance.new("Highlight", p.Character)
                    h.FillColor = Color3.fromRGB(255,0,0)
                    h.FillTransparency = 0.6
                end
            end
        end
    end
end)

-----------------------------------------------------
-- SAVE POSIÇÃO
-----------------------------------------------------
bSave.MouseButton1Click:Connect(function()
    local char = plr.Character
    if not char then return end
    local r = char:FindFirstChild("HumanoidRootPart")
    if not r then return end

    posSalva = r.Position
    bSave.Text = "✔ SALVO"
    task.wait(1)
    bSave.Text = "📍 SALVAR POSIÇÃO"
end)

-----------------------------------------------------
-- FLUTUAR ATÉ
-----------------------------------------------------
bFlutuar.MouseButton1Click:Connect(function()
    flutuando = not flutuando

    if flutuando then
        bFlutuar.Text = "⏳ FLUTUANDO..."

        flutuarConn = RS.Heartbeat:Connect(function()
            if not posSalva then return end
            
            local char = plr.Character
            if not char or not flutuando then return end
            local r = char:FindFirstChild("HumanoidRootPart")
            if not r then return end

            local alvo = posSalva
            local atual = r.Position
            local dist = (alvo - atual).Magnitude

            if dist < 3 then
                flutuando = false
                bFlutuar.Text = "☁ FLUTUAR ATÉ"
                return
            end

            local dir = (alvo - atual).Unit
            r.Velocity = dir * velocidadeFlutuar
        end)

    else
        bFlutuar.Text = "☁ FLUTUAR ATÉ"
        if flutuarConn then flutuarConn:Disconnect() end
    end
end)

print("OK")
