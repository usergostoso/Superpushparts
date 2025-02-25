-- Serviços
local Players = game:GetService("Players")
local ReplicatedStorage = game:GetService("ReplicatedStorage")
local Player = Players.LocalPlayer
local UIS = game:GetService("UserInputService")
local mouse = Player:GetMouse()

-- Criando o ScreenGui
local screenGui = Instance.new("ScreenGui")
screenGui.Parent = Player:WaitForChild("PlayerGui")
screenGui.Name = "PlayerSelectionUI"

-- Estilo do frame principal
local frame = Instance.new("Frame")
frame.Size = UDim2.new(0, 350, 0, 500)
frame.Position = UDim2.new(0, 50, 0, 50)
frame.BackgroundColor3 = Color3.fromRGB(50, 50, 50)
frame.BackgroundTransparency = 0.2
frame.BorderSizePixel = 0
frame.Parent = screenGui

-- Título
local titleLabel = Instance.new("TextLabel")
titleLabel.Size = UDim2.new(0, 350, 0, 50)
titleLabel.Position = UDim2.new(0, 0, 0, 0)
titleLabel.BackgroundTransparency = 1
titleLabel.Text = "Jogadores Online"
titleLabel.TextColor3 = Color3.fromRGB(255, 255, 255)
titleLabel.TextSize = 24
titleLabel.TextAlign = Enum.TextXAlignment.Center
titleLabel.Font = Enum.Font.GothamBold
titleLabel.Parent = frame

-- Frame que contém a lista de jogadores
local playerListFrame = Instance.new("ScrollingFrame")
playerListFrame.Size = UDim2.new(1, 0, 0, 400)
playerListFrame.Position = UDim2.new(0, 0, 0, 50)
playerListFrame.BackgroundTransparency = 1
playerListFrame.ScrollBarThickness = 8
playerListFrame.Parent = frame

local UIListLayout = Instance.new("UIListLayout")
UIListLayout.Parent = playerListFrame
UIListLayout.Padding = UDim.new(0, 10)
UIListLayout.HorizontalAlignment = Enum.HorizontalAlignment.Center

-- Função para empurrar o jogador
local function pushPlayer(targetPlayer)
    local character = targetPlayer.Character
    if character then
        local humanoidRootPart = character:FindFirstChild("HumanoidRootPart")
        if humanoidRootPart then
            -- Empurrar o jogador
            local direction = humanoidRootPart.CFrame.LookVector * 20
            humanoidRootPart.Velocity = direction
        end
    end
end

-- Criar botões para cada jogador na lista
local function createPlayerButton(player)
    local button = Instance.new("TextButton")
    button.Size = UDim2.new(0.9, 0, 0, 40)
    button.Text = player.Name
    button.BackgroundColor3 = Color3.fromRGB(75, 75, 75)
    button.TextColor3 = Color3.fromRGB(255, 255, 255)
    button.TextSize = 20
    button.BorderSizePixel = 0
    button.Font = Enum.Font.Gotham
    button.TextTransparency = 0
    button.TextButtonMode = Enum.TextButtonMode.Button
    button.Parent = playerListFrame

    -- Efeito de Hover para o botão
    button.MouseEnter:Connect(function()
        button.BackgroundColor3 = Color3.fromRGB(100, 100, 100)
    end)

    button.MouseLeave:Connect(function()
        button.BackgroundColor3 = Color3.fromRGB(75, 75, 75)
    end)

    -- Ação de empurrar o jogador
    button.MouseButton1Click:Connect(function()
        pushPlayer(player)
        -- Feedback visual de ação
        button.BackgroundColor3 = Color3.fromRGB(255, 100, 100)
        wait(0.2)
        button.BackgroundColor3 = Color3.fromRGB(75, 75, 75)
    end)
end

-- Atualizar a lista de jogadores na UI
local function updatePlayerList()
    -- Limpar a lista de jogadores (em caso de atualização)
    for _, child in pairs(playerListFrame:GetChildren()) do
        if child:IsA("TextButton") then
            child:Destroy()
        end
    end

    -- Criar botão para cada jogador na lista
    for _, player in pairs(Players:GetPlayers()) do
        if player ~= Player then  -- Não listar o jogador atual
            createPlayerButton(player)
        end
    end
end

-- Atualizar a lista de jogadores sempre que alguém entrar ou sair
Players.PlayerAdded:Connect(updatePlayerList)
Players.PlayerRemoving:Connect(updatePlayerList)

-- Atualizar a lista inicialmente
updatePlayerList()

-- Adicionar função de fechar a UI (botão de fechar)
local closeButton = Instance.new("TextButton")
closeButton.Size = UDim2.new(0, 100, 0, 50)
closeButton.Position = UDim2.new(0, 125, 0, 460)
closeButton.Text = "Fechar"
closeButton.BackgroundColor3 = Color3.fromRGB(255, 0, 0)
closeButton.TextColor3 = Color3.fromRGB(255, 255, 255)
closeButton.TextSize = 20
closeButton.BorderSizePixel = 0
closeButton.Font = Enum.Font.Gotham
closeButton.Parent = frame

closeButton.MouseButton1Click:Connect(function()
    screenGui:Destroy() -- Fecha a UI
end)

-- Animações suaves na entrada da UI
frame.Position = UDim2.new(0, 50, 0, -500)
frame:TweenPosition(UDim2.new(0, 50, 0, 50), Enum.EasingDirection.Out, Enum.EasingStyle.Quad, 0.5, true)
