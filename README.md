if not game:IsLoaded() then
    game.Loaded:Wait()
end

local player = game.Players.LocalPlayer

-- Criar a Tool
local tool = Instance.new("Tool")
tool.Name = "PushTool"
tool.RequiresHandle = true
tool.Parent = player.Backpack

-- Criar o Handle da Tool
local handle = Instance.new("Part")
handle.Name = "Handle"
handle.Size = Vector3.new(1, 5, 1)
handle.Position = player.Character.HumanoidRootPart.Position
handle.Anchored = false
handle.CanCollide = false
handle.Parent = tool
handle.BrickColor = BrickColor.new("Bright blue")

-- Função para empurrar jogadores
local function pushPlayersInRange()
    local hrp = player.Character:WaitForChild("HumanoidRootPart")
    local range = 10  -- Defina o alcance do empurrão

    for _, targetPlayer in pairs(game.Players:GetPlayers()) do
        if targetPlayer ~= player and targetPlayer.Character then
            local targetHRP = targetPlayer.Character:FindFirstChild("HumanoidRootPart")
            if targetHRP then
                local distance = (hrp.Position - targetHRP.Position).Magnitude
                if distance <= range then
                    local direction = (targetHRP.Position - hrp.Position).unit
                    -- Criar e aplicar uma força para empurrar o jogador
                    local bodyVelocity = Instance.new("BodyVelocity")
                    bodyVelocity.MaxForce = Vector3.new(5000, 5000, 5000) -- Garantir que a força seja suficiente
                    bodyVelocity.Velocity = direction * 50 -- Intensidade do empurrão
                    bodyVelocity.Parent = targetHRP
                    -- Remove o BodyVelocity após 0.1 segundos para não deixar ele preso
                    game:GetService("Debris"):AddItem(bodyVelocity, 0.1)
                end
            end
        end
    end
end

-- UI para ativar/desativar o empurrão
local screenGui = Instance.new("ScreenGui")
screenGui.Parent = player.PlayerGui

local frame = Instance.new("Frame")
frame.Size = UDim2.new(0, 200, 0, 100)
frame.Position = UDim2.new(0.8, 0, 0.1, 0)
frame.BackgroundColor3 = Color3.fromRGB(40, 40, 40)
frame.Parent = screenGui
frame.Active = true
frame.Draggable = true

local title = Instance.new("TextLabel")
title.Size = UDim2.new(1, 0, 0, 30)
title.Text = "☄️ PushTool Control"
title.TextColor3 = Color3.fromRGB(255, 255, 255)
title.BackgroundColor3 = Color3.fromRGB(30, 30, 30)
title.Parent = frame

local statusLabel = Instance.new("TextLabel")
statusLabel.Size = UDim2.new(1, 0, 0, 30)
statusLabel.Position = UDim2.new(0, 0, 0, 30)
statusLabel.Text = "Empurrar: Desligado"
statusLabel.TextColor3 = Color3.fromRGB(255, 255, 255)
statusLabel.BackgroundTransparency = 1
statusLabel.Parent = frame

local toggleButton = Instance.new("TextButton")
toggleButton.Size = UDim2.new(1, 0, 0, 30)
toggleButton.Position = UDim2.new(0, 0, 0, 60)
toggleButton.Text = "Ativar Empurrar"
toggleButton.BackgroundColor3 = Color3.fromRGB(60, 60, 60)
toggleButton.TextColor3 = Color3.fromRGB(255, 255, 255)
toggleButton.Parent = frame

local isPushing = false

-- Função para ativar/desativar o empurrão
local function togglePush()
    isPushing = not isPushing
    if isPushing then
        statusLabel.Text = "Empurrar: Ligado"
        toggleButton.Text = "Desativar Empurrar"
    else
        statusLabel.Text = "Empurrar: Desligado"
        toggleButton.Text = "Ativar Empurrar"
    end
end

toggleButton.MouseButton1Click:Connect(togglePush)

-- Função de ativação da Tool
tool.Activated:Connect(function()
    if isPushing then
        pushPlayersInRange()
    end
end)

-- Mensagem no Chat para informar que a Tool foi ativada
game:GetService("StarterGui"):SetCore("ChatMakeSystemMessage", {
    Text = "☄️ PushTool ativada! Use a ferramenta para empurrar jogadores.",
    Color = Color3.fromRGB(255, 165, 0),
    Font = Enum.Font.SourceSansBold,
    TextSize = 18,
})
