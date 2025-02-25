local player = game.Players.LocalPlayer
local screenGui = Instance.new("ScreenGui")
screenGui.Parent = player.PlayerGui

-- Criar a interface
local frame = Instance.new("Frame")
frame.Size = UDim2.new(0, 300, 0, 400)
frame.Position = UDim2.new(0.7, 0, 0.2, 0)
frame.BackgroundColor3 = Color3.fromRGB(40, 40, 40)
frame.Parent = screenGui
frame.Active = true
frame.Draggable = true

local title = Instance.new("TextLabel")
title.Size = UDim2.new(1, 0, 0, 30)
title.Text = "Escolher Jogador para Fling"
title.TextColor3 = Color3.fromRGB(255, 255, 255)
title.BackgroundColor3 = Color3.fromRGB(30, 30, 30)
title.Parent = frame

-- Função para criar um botão para cada jogador online
local function createPlayerButton(targetPlayer)
    local button = Instance.new("TextButton")
    button.Size = UDim2.new(1, 0, 0, 30)
    button.Text = targetPlayer.Name
    button.BackgroundColor3 = Color3.fromRGB(60, 60, 60)
    button.TextColor3 = Color3.fromRGB(255, 255, 255)
    button.Parent = frame
    button.MouseButton1Click:Connect(function()
        applyFling(targetPlayer)
    end)
end

-- Função para aplicar o fling
local function applyFling(targetPlayer)
    if targetPlayer.Character then
        local targetHRP = targetPlayer.Character:FindFirstChild("HumanoidRootPart")
        if targetHRP then
            local originalPosition = targetHRP.Position
            local direction = (targetHRP.Position - player.Character.HumanoidRootPart.Position).unit
            local bodyVelocity = Instance.new("BodyVelocity")
            bodyVelocity.MaxForce = Vector3.new(10000, 10000, 10000)
            bodyVelocity.Velocity = direction * 200 -- Força do fling
            bodyVelocity.Parent = targetHRP
            game:GetService("Debris"):AddItem(bodyVelocity, 0.1)
            
            -- Volta à posição original após 1 segundo
            wait(1)
            targetHRP.CFrame = CFrame.new(originalPosition)
        end
    end
end

-- Cria os botões para todos os jogadores online
for _, targetPlayer in pairs(game.Players:GetPlayers()) do
    if targetPlayer ~= player then
        createPlayerButton(targetPlayer)
    end
end
