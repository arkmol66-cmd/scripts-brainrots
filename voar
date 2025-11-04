-- Este é um LocalScript. Coloque-o em StarterPlayer.StarterPlayerScripts

local UserInputService = game:GetService("UserInputService")
local RunService = game:GetService("RunService")
local Players = game:GetService("Players")

-- Variáveis de Estado
local player = Players.LocalPlayer
local character = player.Character or player.CharacterAdded:Wait()
local humanoid = character:WaitForChild("Humanoid")

local isFlying = false
local currentFlightSpeed = 50 -- Velocidade inicial (studs por frame)

-- ==================================================
-- 1. CRIAÇÃO DOS ELEMENTOS DA GUI
-- ==================================================

local function createGUI()
    local screenGui = Instance.new("ScreenGui")
    screenGui.Name = "FlightGUI"
    screenGui.Parent = player:WaitForChild("PlayerGui")

    -- Botão de Ativação
    local toggleButton = Instance.new("TextButton")
    toggleButton.Name = "ToggleFlightButton"
    toggleButton.Text = "ATIVAR VOO"
    toggleButton.Size = UDim2.new(0.15, 0, 0.05, 0) -- Tamanho: 15% largura, 5% altura
    toggleButton.Position = UDim2.new(0.8, 0, 0.9, 0) -- Posição: Canto inferior direito
    toggleButton.Font = Enum.Font.SourceSansBold
    toggleButton.TextSize = 16
    toggleButton.BackgroundColor3 = Color3.fromRGB(60, 179, 113) -- Verde médio
    toggleButton.TextColor3 = Color3.fromRGB(255, 255, 255)
    toggleButton.Parent = screenGui

    -- Frame de Controle de Velocidade (Inicialmente invisível)
    local speedControlFrame = Instance.new("Frame")
    speedControlFrame.Name = "SpeedControlFrame"
    speedControlFrame.Size = UDim2.new(0.2, 0, 0.15, 0)
    speedControlFrame.Position = UDim2.new(0.75, 0, 0.75, 0) -- Um pouco acima e à esquerda do botão principal
    speedControlFrame.BackgroundColor3 = Color3.fromRGB(40, 40, 40)
    speedControlFrame.BorderSizePixel = 0
    speedControlFrame.Visible = false -- IMPORTANTE: Invisível até ativar o voo
    speedControlFrame.Parent = screenGui

    -- Rótulo de Velocidade
    local speedLabel = Instance.new("TextLabel")
    speedLabel.Name = "SpeedLabel"
    speedLabel.Size = UDim2.new(1, 0, 0.3, 0)
    speedLabel.Position = UDim2.new(0, 0, 0, 0)
    speedLabel.Text = "Velocidade: 50"
    speedLabel.Font = Enum.Font.SourceSans
    speedLabel.TextSize = 14
    speedLabel.TextColor3 = Color3.fromRGB(255, 255, 255)
    speedLabel.BackgroundTransparency = 1 -- Transparente
    speedLabel.Parent = speedControlFrame

    -- Barra Deslizante (ScrollBar)
    local speedSlider = Instance.new("ScrollBar")
    speedSlider.Name = "SpeedSlider"
    speedSlider.Min = 0
    speedSlider.Max = 1
    speedSlider.Value = 0.3 -- Padrão inicial (30% da barra)
    speedSlider.Size = UDim2.new(0.9, 0, 0.3, 0)
    speedSlider.Position = UDim2.new(0.05, 0, 0.55, 0)
    speedSlider.Parent = speedControlFrame

    return toggleButton, speedControlFrame, speedSlider
end

-- ==================================================
-- 2. LÓGICA DE CONTROLE
-- ==================================================

local function updateFlightSpeed(slider)
    -- Mapeamento do Slider (0 a 1) para Velocidade (20 a 150)
    local minSpeed = 20
    local maxSpeed = 150
    currentFlightSpeed = minSpeed + (maxSpeed - minSpeed) * slider.Value
    
    speedLabel.Text = "Velocidade: " .. math.floor(currentFlightSpeed)
end

local function toggleFlight()
    if not isFlying then
        -- ATIVAR VOO
        isFlying = true
        humanoid.PlatformStand = true -- Impede a gravidade/pulo padrão
        toggleButton.Text = "PARAR VOO"
        toggleButton.BackgroundColor3 = Color3.fromRGB(255, 80, 80) -- Vermelho
        speedControlFrame.Visible = true -- Mostra o painel
        
    else
        -- DESATIVAR VOO
        isFlying = false
        humanoid.PlatformStand = false
        humanoid.Jump = false -- Garante que ele comece a cair
        toggleButton.Text = "ATIVAR VOO"
        toggleButton.BackgroundColor3 = Color3.fromRGB(60, 179, 113) -- Verde
        speedControlFrame.Visible = false -- Esconde o painel
    end
end

-- ==================================================
-- 3. CONEXÃO DOS EVENTOS
-- ==================================================

local toggleButton, speedControlFrame, speedSlider = createGUI()

-- Conecta o clique do botão
toggleButton.MouseButton1Click:Connect(toggleFlight)

-- Conecta a mudança no Slider (só funciona se estiver voando)
speedSlider.Changed:Connect(function(property)
    if property == "Value" and isFlying then
        updateFlightSpeed(speedSlider)
    end
end)

-- Loop de Voo (Executado a cada frame)
RunService.Heartbeat:Connect(function()
    if isFlying and character and character.PrimaryPart then
        -- Aplica o movimento para cima. O fator de divisão 100 é para ajuste fino.
        local movementAmount = (currentFlightSpeed / 100) * 0.2 
        character.PrimaryPart.CFrame = character.PrimaryPart.CFrame * CFrame.new(0, movementAmount, 0)
    end
end)

-- Configurações iniciais
updateFlightSpeed(speedSlider)
