--// Brainrot Spin GUI
local Players = game:GetService("Players")
local TeleportService = game:GetService("TeleportService")
local player = Players.LocalPlayer

-- Criar ScreenGui
local gui = Instance.new("ScreenGui")
gui.Name = "BrainrotSpin"
gui.ResetOnSpawn = false
gui.Parent = game.CoreGui

-- Janela principal
local main = Instance.new("Frame")
main.Size = UDim2.new(0, 250, 0, 150)
main.Position = UDim2.new(0.3, 0, 0.3, 0)
main.BackgroundColor3 = Color3.fromRGB(0, 0, 0)
main.BorderSizePixel = 2
main.Active = true
main.Draggable = true
main.Parent = gui

-- Título
local title = Instance.new("TextLabel")
title.Size = UDim2.new(1, -30, 0, 30)
title.Position = UDim2.new(0, 5, 0, 0)
title.BackgroundTransparency = 1
title.Text = "Brainrot Spin"
title.TextColor3 = Color3.fromRGB(255,255,255)
title.TextSize = 16
title.Font = Enum.Font.SourceSansBold
title.TextXAlignment = Enum.TextXAlignment.Left
title.Parent = main

-- Botão minimizar [-]
local minimize = Instance.new("TextButton")
minimize.Size = UDim2.new(0, 30, 0, 30)
minimize.Position = UDim2.new(1, -30, 0, 0)
minimize.Text = "-"
minimize.TextColor3 = Color3.fromRGB(255,255,255)
minimize.BackgroundColor3 = Color3.fromRGB(30,30,30)
minimize.Parent = main

-- Conteúdo da GUI
local content = Instance.new("Frame")
content.Size = UDim2.new(1, -10, 1, -40)
content.Position = UDim2.new(0, 5, 0, 35)
content.BackgroundTransparency = 1
content.Parent = main

-- Botão Auto Exec
local autoExec = Instance.new("TextButton")
autoExec.Size = UDim2.new(0, 200, 0, 40)
autoExec.Position = UDim2.new(0, 20, 0, 10)
autoExec.Text = "Auto Exec: OFF"
autoExec.TextColor3 = Color3.fromRGB(255,255,255)
autoExec.BackgroundColor3 = Color3.fromRGB(100,0,0)
autoExec.Parent = content

-- Botão Rejoin
local rejoin = Instance.new("TextButton")
rejoin.Size = UDim2.new(0, 200, 0, 40)
rejoin.Position = UDim2.new(0, 20, 0, 60)
rejoin.Text = "Rejoin"
rejoin.TextColor3 = Color3.fromRGB(255,255,255)
rejoin.BackgroundColor3 = Color3.fromRGB(0,0,100)
rejoin.Parent = content

-- Variáveis
local autoExecEnabled = false

-- Função Auto Exec
autoExec.MouseButton1Click:Connect(function()
    autoExecEnabled = not autoExecEnabled
    if autoExecEnabled then
        autoExec.Text = "Auto Exec: ON"
        autoExec.BackgroundColor3 = Color3.fromRGB(0,100,0)
    else
        autoExec.Text = "Auto Exec: OFF"
        autoExec.BackgroundColor3 = Color3.fromRGB(100,0,0)
    end
end)

-- Função Rejoin
rejoin.MouseButton1Click:Connect(function()
    -- Se auto exec estiver ativado, salva no _G
    if autoExecEnabled then
        _G.BrainrotAutoExec = true
    else
        _G.BrainrotAutoExec = false
    end
    TeleportService:Teleport(game.PlaceId, player)
end)

-- Verificar se deve auto-executar
if _G.BrainrotAutoExec then
    warn("Brainrot Spin Auto Exec ativo! Executando script automaticamente...")
    -- Coloque aqui o script que deseja rodar automaticamente
end

-- Minimizar
local minimized = false
minimize.MouseButton1Click:Connect(function()
    minimized = not minimized
    if minimized then
        content.Visible = false
        main.Size = UDim2.new(0, 250, 0, 30)
    else
        content.Visible = true
        main.Size = UDim2.new(0, 250, 0, 150)
    end
end)
