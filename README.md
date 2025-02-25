if not game:IsLoaded() then
    game.Loaded:Wait()
end

local player = game.Players.LocalPlayer
local char = player.Character or player.CharacterAdded:Wait()
local hrp = char:FindFirstChild("HumanoidRootPart")
local flying = false
local bodyGyro, bodyVelocity
local isMinimized = false

-- Criando GUI Exploit
local screenGui = Instance.new("ScreenGui")
screenGui.Parent = game.CoreGui

local frame = Instance.new("Frame")
frame.Size = UDim2.new(0, 220, 0, 250)  -- Corrigido o tamanho da Frame
frame.Position = UDim2.new(0.8, 0, 0.2, 0)
frame.BackgroundColor3 = Color3.fromRGB(40, 40, 40)
frame.Parent = screenGui
frame.Active = true  -- Tornando a frame interativa
frame.Draggable = true  -- Permitindo arrastar a frame

local title = Instance.new("TextLabel")
title.Size = UDim2.new(1, 0, 0, 30)
title.Text = "☄️ Super Push Parts V1"
title.TextColor3 = Color3.fromRGB(255, 255, 255)
title.BackgroundColor3 = Color3.fromRGB(30, 30, 30)
title.Parent = frame

local pushStatus = Instance.new("TextLabel")
pushStatus.Size = UDim2.new(1, 0, 0, 30)
pushStatus.Position = UDim2.new(0, 0, 0, 30)
pushStatus.Text = "Push: OFF"
pushStatus.TextColor3 = Color3.fromRGB(255, 255, 255)
pushStatus.BackgroundTransparency = 1
pushStatus.Parent = frame

local rangeLabel = Instance.new("TextLabel")
rangeLabel.Size = UDim2.new(1, 0, 0, 30)
rangeLabel.Position = UDim2.new(0, 0, 0, 60)
rangeLabel.Text = "Alcance: 15"
rangeLabel.TextColor3 = Color3.fromRGB(255, 255, 255)
rangeLabel.BackgroundTransparency = 1
rangeLabel.Parent = frame

-- Barra de alcance
local slider = Instance.new("Slider")
slider.Size = UDim2.new(0.9, 0, 0, 30)
slider.Position = UDim2.new(0.05, 0, 0, 100)
slider.MinValue = 5
slider.MaxValue = 50
slider.Value = 15
slider.Parent = frame

-- Função para criar os botões
local function createButton(name, pos, callback)
    local button = Instance.new("TextButton")
    button.Size = UDim2.new(0.9, 0, 0, 30)
    button.Position = UDim2.new(0.05, 0, 0, pos)
    button.Text = name
    button.BackgroundColor3 = Color3.fromRGB(60, 60, 60)
    button.TextColor3 = Color3.fromRGB(255, 255, 255)
    button.Parent = frame
    button.MouseButton1Click:Connect(callback)
end

-- Função para empurrar as partes
local pushing = false
local function pushParts()
    while pushing do
        local range = slider.Value
        for _, part in pairs(workspace:GetDescendants()) do
            if part:IsA("BasePart") and not part.Anchored then
                local distance = (part.Position - hrp.Position).Magnitude
                if distance <= range then
                    local direction = (part.Position - hrp.Position).unit  -- Direção do empurrão
                    part.Velocity = direction * 50  -- Empurrando a part sem fazer girar
                end
            end
        end
        wait(0.1)
    end
end

-- Função para ativar/desativar o Fly
local function toggleFlyButton(button)
    if flying then
        -- Desativa o fly
        flying = false
        if bodyGyro then bodyGyro:Destroy() end
        if bodyVelocity then bodyVelocity:Destroy() end
        hrp.Anchored = false
        button.Text = "Fly: X"  -- Exibe X quando desativado
    else
        -- Ativa o fly
        flying = true
        bodyGyro = Instance.new("BodyGyro", hrp)
        bodyGyro.MaxTorque = Vector3.new(400000, 400000, 400000)
        bodyGyro.CFrame = hrp.CFrame
        bodyVelocity = Instance.new("BodyVelocity", hrp)
        bodyVelocity.MaxForce = Vector3.new(400000, 400000, 400000)
        bodyVelocity.Velocity = Vector3.new(0, 0, 0)
        hrp.Anchored = true
        button.Text = "Fly: ✓"  -- Exibe ✓ quando ativado
    end
end

-- Função para minimizar a UI
local function toggleMinimize()
    isMinimized = not isMinimized
    if isMinimized then
        frame.Size = UDim2.new(0, 220, 0, 30)  -- Tamanho reduzido
        title.Text = "☄️ Super Push V1"  -- Título minimizado
    else
        frame.Size = UDim2.new(0, 220, 0, 250)  -- Tamanho normal
        title.Text = "☄️ Super Push Parts V1"  -- Título completo
    end
end

-- Ativar/Desativar Push
createButton("Ativar Push", 140, function()
    pushing = not pushing
    if pushing then
        pushStatus.Text = "Push: ON"
        pushParts()
    else
        pushStatus.Text = "Push: OFF"
    end
end)

-- Botão de Fly (✓ / X)
createButton("Fly: X", 180, function()
    toggleFlyButton(frame["Fly: X"])
end)

-- Botão de Minimizar
createButton("Minimizar", 220, toggleMinimize)

-- Enviar mensagem no chat corretamente
game:GetService("StarterGui"):SetCore("ChatMakeSystemMessage", {
    Text = "☄️ Super Push Parts V1 Ativado!";
    Color = Color3.fromRGB(255, 165, 0);
    Font = Enum.Font.SourceSansBold;
    TextSize = 18;
})
