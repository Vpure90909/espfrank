-- Variáveis
local players = game:GetService("Players")
local player = players.LocalPlayer
local camera = game.Workspace.CurrentCamera
local UserInputService = game:GetService("UserInputService")

-- Controla se a GUI de ESP está aberta ou fechada
local guiVisible = false

-- Função para criar a GUI
local function createESP_GUI()
    -- Cria o ScreenGui
    local screenGui = Instance.new("ScreenGui")
    screenGui.Name = "ESP_GUI"
    screenGui.Parent = player:WaitForChild("PlayerGui")
    screenGui.Enabled = guiVisible  -- Inicializa a GUI com a visibilidade controlada

    -- Cria o botão de fechamento da GUI
    local closeButton = Instance.new("TextButton")
    closeButton.Size = UDim2.new(0, 200, 0, 50)
    closeButton.Position = UDim2.new(0.5, -100, 0.05, 0)
    closeButton.BackgroundColor3 = Color3.fromRGB(255, 0, 0)
    closeButton.Text = "Fechar GUI"
    closeButton.TextColor3 = Color3.fromRGB(255, 255, 255)
    closeButton.TextScaled = true
    closeButton.Parent = screenGui
    closeButton.MouseButton1Click:Connect(function()
        guiVisible = false
        screenGui.Enabled = guiVisible
    end)

    -- Cria o TextLabel que exibirá as informações
    local espLabel = Instance.new("TextLabel")
    espLabel.Size = UDim2.new(0, 200, 0, 50)
    espLabel.Position = UDim2.new(0.5, -100, 0.2, 0)  -- Centralizado na tela
    espLabel.BackgroundTransparency = 1
    espLabel.TextColor3 = Color3.fromRGB(255, 255, 255)
    espLabel.TextScaled = true
    espLabel.Text = "ESP Ativado!"
    espLabel.Parent = screenGui

    -- Função para atualizar a posição do ESP baseado nos jogadores visíveis
    local function updateESP()
        while guiVisible do
            -- Percorre todos os jogadores
            for _, targetPlayer in ipairs(players:GetPlayers()) do
                if targetPlayer.Character and targetPlayer.Character:FindFirstChild("HumanoidRootPart") then
                    local rootPart = targetPlayer.Character.HumanoidRootPart
                    local screenPosition, onScreen = camera:WorldToScreenPoint(rootPart.Position)
                    
                    -- Se o jogador estiver na tela, exibe o ESP
                    if onScreen then
                        espLabel.Text = "Jogador: " .. targetPlayer.Name
                        espLabel.Position = UDim2.new(0, screenPosition.X, 0, screenPosition.Y)
                    else
                        espLabel.Text = ""
                    end
                end
            end
            wait(0.1) -- Aguarda um pouco antes de atualizar
        end
    end

    -- Chama a função de atualização
    updateESP()
end

-- Cria a GUI inicialmente
createESP_GUI()

-- Função para alternar a visibilidade da GUI com as teclas H e F
UserInputService.InputBegan:Connect(function(input, gameProcessed)
    if gameProcessed then return end

    if input.KeyCode == Enum.KeyCode.H then
        guiVisible = true
        player.PlayerGui.ESP_GUI.Enabled = guiVisible
    elseif input.KeyCode == Enum.KeyCode.F then
        guiVisible = false
        player.PlayerGui.ESP_GUI.Enabled = guiVisible
    end
end)
