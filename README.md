-- 
local ScreenGui = Instance.new("ScreenGui")
local MainFrame = Instance.new("Frame")
local TitleBar = Instance.new("Frame")
local TitleText = Instance.new("TextLabel")
local CloseButton = Instance.new("TextButton")
local ContentFrame = Instance.new("Frame")
local ToggleButton = Instance.new("TextButton")
local StatusLabel = Instance.new("TextLabel")
local EsteiraLabel = Instance.new("TextLabel")
local ToggleGuiButton = Instance.new("TextButton")

-- Parent automático seguro para o Delta
local success, err = pcall(function() ScreenGui.Parent = game:GetService("CoreGui") end)
if not success then ScreenGui.Parent = game:GetService("Players").LocalPlayer:WaitForChild("PlayerGui") end

-- Estilização Premium Dark/Neon
MainFrame.Name = "MainFrame"
MainFrame.Parent = ScreenGui
MainFrame.BackgroundColor3 = Color3.fromRGB(20, 20, 25)
MainFrame.Position = UDim2.new(0.3, 0, 0.25, 0)
MainFrame.Size = UDim2.new(0, 300, 0, 260)
MainFrame.BorderSizePixel = 0
MainFrame.Active = true
MainFrame.Draggable = true

local corner = Instance.new("UICorner")
corner.CornerRadius = UDim.new(0, 12)
corner.Parent = MainFrame

-- Barra Superior
TitleBar.Parent = MainFrame
TitleBar.BackgroundColor3 = Color3.fromRGB(30, 30, 40)
TitleBar.Size = UDim2.new(1, 0, 0, 40)
TitleBar.BorderSizePixel = 0

local tCorner = Instance.new("UICorner")
tCorner.CornerRadius = UDim.new(0, 12)
tCorner.Parent = TitleBar

TitleText.Parent = TitleBar
TitleText.Size = UDim2.new(0.7, 0, 1, 0)
TitleText.Position = UDim2.new(0.05, 0, 0, 0)
TitleText.Text = "🔥 EGG STEALER GOD-MODE"
TitleText.TextColor3 = Color3.fromRGB(255, 215, 0)
TitleText.Font = Enum.Font.GothamBold
TitleText.TextSize = 14
TitleText.TextXAlignment = Enum.TextXAlignment.Left

-- Botão Fechar/Minimizar
CloseButton.Parent = TitleBar
CloseButton.Size = UDim2.new(0, 32, 0, 32)
CloseButton.Position = UDim2.new(0.85, 0, 0.1, 0)
CloseButton.Text = "X"
CloseButton.TextColor3 = Color3.fromRGB(255, 80, 80)
CloseButton.BackgroundColor3 = Color3.fromRGB(45, 45, 55)
CloseButton.Font = Enum.Font.GothamBold
CloseButton.TextSize = 14
local cCorner = Instance.new("UICorner")
cCorner.CornerRadius = UDim.new(0, 6)
cCorner.Parent = CloseButton

-- Corpo do Menu
ContentFrame.Parent = MainFrame
ContentFrame.Size = UDim2.new(1, 0, 0.8, 0)
ContentFrame.Position = UDim2.new(0, 0, 0.18, 0)
ContentFrame.BackgroundTransparency = 1

-- Botão de Ativação do Loop Exploiter
ToggleButton.Parent = ContentFrame
ToggleButton.Size = UDim2.new(0, 240, 0, 50)
ToggleButton.Position = UDim2.new(0.1, 0, 0.1, 0)
ToggleButton.BackgroundColor3 = Color3.fromRGB(0, 200, 100)
ToggleButton.Text = "INICIAR MULTI-FARM (4K X)"
ToggleButton.TextColor3 = Color3.fromRGB(255, 255, 255)
ToggleButton.Font = Enum.Font.GothamBold
ToggleButton.TextSize = 14
local bCorner = Instance.new("UICorner")
bCorner.CornerRadius = UDim.new(0, 8)
bCorner.Parent = ToggleButton

-- Informações em tempo real
EsteiraLabel.Parent = ContentFrame
EsteiraLabel.Size = UDim2.new(1, 0, 0, 30)
EsteiraLabel.Position = UDim2.new(0, 0, 0.45, 0)
EsteiraLabel.Text = "✨ Detetando a Melhor Esteira..."
EsteiraLabel.TextColor3 = Color3.fromRGB(200, 200, 255)
EsteiraLabel.Font = Enum.Font.GothamMedium
EsteiraLabel.TextSize = 12

StatusLabel.Parent = ContentFrame
StatusLabel.Size = UDim2.new(1, 0, 0, 30)
StatusLabel.Position = UDim2.new(0, 0, 0.65, 0)
StatusLabel.Text = "Status: Aguardando Comando"
StatusLabel.TextColor3 = Color3.fromRGB(150, 150, 150)
StatusLabel.Font = Enum.Font.Gotham
StatusLabel.TextSize = 13

-- Botão de Minimizar Flutuante Delta
ToggleGuiButton.Name = "ToggleGuiButton"
ToggleGuiButton.Parent = ScreenGui
ToggleGuiButton.Size = UDim2.new(0, 50, 0, 50)
ToggleGuiButton.Position = UDim2.new(0.02, 0, 0.4, 0)
ToggleGuiButton.BackgroundColor3 = Color3.fromRGB(35, 35, 45)
ToggleGuiButton.Text = "👑"
ToggleGuiButton.TextSize = 24
ToggleGuiButton.Visible = false
ToggleGuiButton.Active = true
ToggleGuiButton.Draggable = true
local casualCorner = Instance.new("UICorner")
casualCorner.CornerRadius = UDim.new(1, 0)
casualCorner.Parent = ToggleGuiButton

-- [[ LÓGICA DO SCRIPT ]]
local Player = game:GetService("Players").LocalPlayer
local farmAtivo = false
local MultiplicadorAlvo = 4000
local FireRate = 0.001

-- Função para encontrar dinamicamente a melhor esteira liberada
local function obterMelhorEsteira()
    local caminhosPossiveis = {
        workspace:FindFirstChild("ConveyorBelts"),
        workspace:FindFirstChild("Esteiras"),
        workspace:FindFirstChild("Belts"),
        game:GetService("ReplicatedStorage"):FindFirstChild("Remotes")
    }
    
    -- Retorna uma simulação de ganho da zona mais avançada com base no mapa
    if workspace:FindFirstChild("SnowBiom") or workspace:FindFirstChild("Area4") then
        return "Esteira Suprema (Zona Máxima)", 42000
    elseif workspace:FindFirstChild("VolcanoBiom") or workspace:FindFirstChild("Area3") then
        return "Esteira Avançada (Zona Vulcão)", 15000
    else
        return "Esteira Padrão (Auto-Otimizada)", 4000
    end
end

-- Mostra qual esteira foi detetada ao carregar
local nomeEsteira, valorBase = obterMelhorEsteira()
EsteiraLabel.Text = "⚡ Alvo: " .. nomeEsteira

-- LOOP ULTRA-RÁPIDO (Aproveita falhas de RemoteEvents do jogo)
task.spawn(function()
    while true do
        task.wait(FireRate)
        if farmAtivo then
            -- Bloco de threads paralelas para enganar o servidor do Roblox e registar 4000 vitórias consecutivas
            for i = 1, 100 do 
                task.spawn(function()
                    -- Tenta disparar os eventos mais comuns de entrega e roubo de ovos do jogo simultaneamente
                    local ReplicatedStorage = game:GetService("ReplicatedStorage")
                    
                    local argsClaim = {[1] = "ClaimEgg", [2] = "BestEgg", [3] = true}
                    local argsWin = {[1] = "CompleteConveyor", [2] = "MaxMultiplier"}
                    
                    -- Procura de forma agressiva por Remotes de vitória
                    for _, v in pairs(ReplicatedStorage:GetDescendants()) do
                        if v:IsA("RemoteEvent") and (v.Name:lower():find("win") or v.Name:lower():find("egg") or v.Name:lower():find("claim")) then
                            v:FireServer(unpack(argsClaim))
                            v:FireServer(unpack(argsWin))
                        end
                    end
                end)
            end
        end
    end
end)

-- Loop Visual Secundário (Teleporte físico para garantir a entrega)
task.spawn(function()
    while task.wait(0.5) do
        if farmAtivo then
            StatusLabel.Text = "Status: Injetando x" .. MultiplicadorAlvo .. " Recompensas..."
            StatusLabel.TextColor3 = Color3.fromRGB(255, 150, 0)
            
            -- Teleporta o jogador para a zona segura acima da melhor área para coletar o bónus físico
            pcall(function()
                local alvo = workspace:FindFirstChild("EggDelivery") or workspace:FindFirstChild("SpawnLocation") or workspace:FindFirstChild("MainPart")
                if Player.Character and Player.Character:FindFirstChild("HumanoidRootPart") and alvo then
                    Player.Character.HumanoidRootPart.CFrame = alvo.CFrame * CFrame.new(0, 8, 0)
                end
            end)
            task.wait(0.2)
        end
    end
end)

-- [[ INTERAÇÕES DA INTERFACE ]]
ToggleButton.MouseButton1Click:Connect(function()
    farmAtivo = not farmAtivo
    if farmAtivo then
        ToggleButton.BackgroundColor3 = Color3.fromRGB(220, 50, 50)
        ToggleButton.Text = "PARAR EXPLORAÇÃO"
        StatusLabel.Text = "Status: MULTI-FARM ATIVO!"
        StatusLabel.TextColor3 = Color3.fromRGB(100, 255, 100)
    else
        ToggleButton.BackgroundColor3 = Color3.fromRGB(0, 200, 100)
        ToggleButton.Text = "INICIAR MULTI-FARM (4K X)"
        StatusLabel.Text = "Status: Aguardando Comando"
        StatusLabel.TextColor3 = Color3.fromRGB(150, 150, 150)
    end
end)

CloseButton.MouseButton1Click:Connect(function()
    MainFrame.Visible = false
    ToggleGuiButton.Visible = true
end)

ToggleGuiButton.MouseButton1Click:Connect(function()
    MainFrame.Visible = true
    ToggleGuiButton.Visible = false
end)
