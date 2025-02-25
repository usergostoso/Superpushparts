local Players = game:GetService("Players")

-- Criar a Interface Gráfica (UI)
local screenGui = Instance.new("ScreenGui")
screenGui.Parent = game.Players.LocalPlayer:WaitForChild("PlayerGui")

-- Criar o Frame da UI
local frame = Instance.new("Frame")
frame.Size = UDim2.new(0, 320, 0, 230)
frame.Position = UDim2.new(0.5, -160, 0.4, 0)
frame.BackgroundColor3 = Color3.fromRGB(25, 25, 25)
frame.BorderSizePixel = 2
frame.Parent = screenGui

-- Criar um título
local title = Instance.new("TextLabel")
title.Size = UDim2.new(1, 0, 0.2, 0)
title.Position = UDim2.new(0, 0, 0, 0)
title.Text = "🔥 Sarrada Supreme 🔥"
title.TextScaled = true
title.BackgroundColor3 = Color3.fromRGB(50, 50, 50)
title.TextColor3 = Color3.fromRGB(255, 215, 0)
title.Font = Enum.Font.GothamBold
title.Parent = frame

-- Criar um campo de entrada de nome
local nameBox = Instance.new("TextBox")
nameBox.Size = UDim2.new(0.8, 0, 0.2, 0)
nameBox.Position = UDim2.new(0.1, 0, 0.3, 0)
nameBox.PlaceholderText = "📝 Digite o nome do alvo..."
nameBox.TextScaled = true
nameBox.BackgroundColor3 = Color3.fromRGB(50, 50, 50)
nameBox.TextColor3 = Color3.fromRGB(255, 255, 255)
nameBox.Font = Enum.Font.GothamBold
nameBox.Parent = frame

-- Criar um botão de ativar/desativar
local toggleButton = Instance.new("TextButton")
toggleButton.Size = UDim2.new(0.8, 0, 0.2, 0)
toggleButton.Position = UDim2.new(0.1, 0, 0.55, 0)
toggleButton.Text = "✅ Ativar Sarrada"
toggleButton.TextScaled = true
toggleButton.TextColor3 = Color3.fromRGB(255, 255, 255)
toggleButton.Font = Enum.Font.GothamBold
toggleButton.Parent = frame

-- Criar um botão de fechar
local closeButton = Instance.new("TextButton")
closeButton.Size = UDim2.new(0.2, 0, 0.15, 0)
closeButton.Position = UDim2.new(0.75, 0, 0.8, 0)
closeButton.Text = "❌"
closeButton.TextScaled = true
closeButton.BackgroundColor3 = Color3.fromRGB(255, 0, 0)
closeButton.TextColor3 = Color3.fromRGB(255, 255, 255)
closeButton.Font = Enum.Font.GothamBold
closeButton.Parent = frame

local active = false -- Estado inicial desativado

-- Função para encontrar o jogador alvo
local function findPlayer(name)
    for _, p in pairs(Players:GetPlayers()) do
        if p.Name:lower():match(name:lower()) then
            return p
        end
    end
    return nil
end

-- Função de sarrar
local function sarrar(player, targetName)
    local targetPlayer = findPlayer(targetName)
    if targetPlayer and targetPlayer.Character then
        local char = player.Character
        local targetChar = targetPlayer.Character

        if char and char:FindFirstChild("HumanoidRootPart") and targetChar:FindFirstChild("HumanoidRootPart") then
            -- Mover o jogador perto do alvo
            char.HumanoidRootPart.CFrame = targetChar.HumanoidRootPart.CFrame * CFrame.new(0, 0, -1)

            -- Criar a animação
            local humanoid = char:FindFirstChildOfClass("Humanoid")
            if humanoid then
                local animation = Instance.new("Animation")
                animation.AnimationId = "rbxassetid://2510196951" -- ID da animação
                local animator = humanoid:FindFirstChildOfClass("Animator") or humanoid:WaitForChild("Animator")
                local track = animator:LoadAnimation(animation)
                track:Play()
            end
        end
    end
end

-- Efeito RGB nos botões
local function applyRGBEffect(button)
    spawn(function()
        while true do
            for i = 0, 255, 5 do
                button.BackgroundColor3 = Color3.fromRGB(i, 0, 255 - i)
                wait(0.05)
            end
            for i = 0, 255, 5 do
                button.BackgroundColor3 = Color3.fromRGB(255 - i, i, 0)
                wait(0.05)
            end
            for i = 0, 255, 5 do
                button.BackgroundColor3 = Color3.fromRGB(0, 255 - i, i)
                wait(0.05)
            end
        end
    end)
end

-- Aplicar RGB aos botões
applyRGBEffect(toggleButton)
applyRGBEffect(closeButton)

-- Evento do botão de ativar/desativar
toggleButton.MouseButton1Click:Connect(function()
    active = not active -- Alternar estado

    if active then
        toggleButton.Text = "🚀 Sarrando!"
    else
        toggleButton.Text = "✅ Ativar Sarrada"
    end
end)

-- Evento do botão de fechar
closeButton.MouseButton1Click:Connect(function()
    screenGui:Destroy()
end)
