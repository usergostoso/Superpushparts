if not game:IsLoaded() then
    game.Loaded:Wait()
end

local player = game.Players.LocalPlayer
local char = player.Character or player.CharacterAdded:Wait()
local hrp = char:FindFirstChild("HumanoidRootPart")

-- Criando GUI Exploit
local screenGui = Instance.new("ScreenGui")
screenGui.Parent = game.CoreGui

local frame = Instance.new("Frame")
frame.Size = UDim2.new(0, 220, 0, 150)
frame.Position = UDim2.new(0.8, 0, 0.2, 0)
frame.BackgroundColor3 = Color3.fromRGB(40, 40, 40)
frame.Parent = screenGui

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

-- Função para Empurrar as Parts Não Ancoradas
local pushing = false
local function pushParts()
    while pushing do
        for _, part in pairs(workspace:GetDescendants()) do
            if part:IsA("BasePart") and not part.Anchored then
                local distance = (part.Position - hrp.Position).Magnitude
                if distance <= 15 then
                    local bodyVelocity = Instance.new("BodyVelocity", part)
                    bodyVelocity.Velocity = (part.Position - hrp.Position).unit * 50
                    bodyVelocity.MaxForce = Vector3.new(4000, 4000, 4000)
                    game:GetService("Debris"):AddItem(bodyVelocity, 0.2)
                end
            end
        end
        wait(0.5)
    end
end

-- Ativar/Desativar Push
createButton("Ativar Push", 70, function()
    pushing = not pushing
    if pushing then
        pushStatus.Text = "Push: ON"
        pushParts()
    else
        pushStatus.Text = "Push: OFF"
    end
end)

-- Mensagem no chat
game:GetService("StarterGui"):SetCore("ChatMakeSystemMessage", {
    Text = "☄️ Super Push Parts V1 Ativado! Teste no Delta Executor.";
    Color = Color3.fromRGB(255, 165, 0);
    Font = Enum.Font.SourceSansBold;
    TextSize = 18;
})

