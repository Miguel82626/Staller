-- Serviços
local Players = game:GetService("Players")
local RunService = game:GetService("RunService")
local localPlayer = Players.LocalPlayer

-- GUI principal
local ScreenGui = Instance.new("ScreenGui")
ScreenGui.Name = "RK_HUB_GUI"
ScreenGui.ResetOnSpawn = false
ScreenGui.Parent = localPlayer:WaitForChild("PlayerGui")

-- Função rainbow color
local function rainbowColor(offset)
    return Color3.fromHSV((tick() * 0.2 + (offset or 0)) % 1, 1, 1)
end

-- Botão Toggle (redondo, rainbow, arrastável)
local ToggleFrame = Instance.new("Frame")
ToggleFrame.Name = "RK_ToggleFrame"
ToggleFrame.Size = UDim2.new(0, 60, 0, 60)
ToggleFrame.Position = UDim2.new(0, 30, 0.5, -30)
ToggleFrame.BackgroundTransparency = 1
ToggleFrame.Active = true
ToggleFrame.Parent = ScreenGui

local ToggleButton = Instance.new("ImageButton")
ToggleButton.Name = "RK_ToggleButton"
ToggleButton.Size = UDim2.new(1, 0, 1, 0)
ToggleButton.Position = UDim2.new(0, 0, 0, 0)
ToggleButton.BackgroundTransparency = 1
ToggleButton.Image = ""
ToggleButton.Parent = ToggleFrame

local UICorner = Instance.new("UICorner")
UICorner.CornerRadius = UDim.new(1, 0)
UICorner.Parent = ToggleButton

local UIStroke = Instance.new("UIStroke")
UIStroke.Thickness = 3
UIStroke.ApplyStrokeMode = Enum.ApplyStrokeMode.Border
UIStroke.Parent = ToggleButton

local ToggleLabel = Instance.new("TextLabel")
ToggleLabel.BackgroundTransparency = 1
ToggleLabel.Size = UDim2.new(1, 0, 1, 0)
ToggleLabel.Position = UDim2.new(0, 0, 0, 0)
ToggleLabel.Text = "RK"
ToggleLabel.Font = Enum.Font.GothamBold
ToggleLabel.TextScaled = true
ToggleLabel.TextColor3 = Color3.fromRGB(255,255,255)
ToggleLabel.TextStrokeTransparency = 0.25
ToggleLabel.Parent = ToggleButton

-- Função para tornar um Frame arrastável (PC e mobile)
local function makeDraggable(frame)
    local UIS = game:GetService("UserInputService")
    local dragging, dragInput, dragStart, startPos

    frame.InputBegan:Connect(function(input)
        if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then
            dragging = true
            dragStart = input.Position
            startPos = frame.Position
            input.Changed:Connect(function()
                if input.UserInputState == Enum.UserInputState.End then
                    dragging = false
                end
            end)
        end
    end)
    frame.InputChanged:Connect(function(input)
        if input.UserInputType == Enum.UserInputType.MouseMovement or input.UserInputType == Enum.UserInputType.Touch then
            dragInput = input
        end
    end)
    game:GetService("UserInputService").InputChanged:Connect(function(input)
        if input == dragInput and dragging then
            local delta = input.Position - dragStart
            frame.Position = UDim2.new(startPos.X.Scale, startPos.X.Offset + delta.X, startPos.Y.Scale, startPos.Y.Offset + delta.Y)
        end
    end)
end

makeDraggable(ToggleFrame)

-- HUB (padrão fechado)
local MainFrame = Instance.new("Frame")
MainFrame.Name = "RK_Hub"
MainFrame.Size = UDim2.new(0, 250, 0, 100)
MainFrame.Position = UDim2.new(0.5, -125, 0.5, -50)
MainFrame.BackgroundColor3 = Color3.fromRGB(25, 25, 25)
MainFrame.BorderSizePixel = 0
MainFrame.Visible = false
MainFrame.Active = true
MainFrame.Parent = ScreenGui

local HubCorner = Instance.new("UICorner")
HubCorner.CornerRadius = UDim.new(0, 12)
HubCorner.Parent = MainFrame

local Title = Instance.new("TextLabel")
Title.Name = "Title"
Title.Size = UDim2.new(1, 0, 0, 32)
Title.BackgroundTransparency = 1
Title.Text = "RK 100% | By RK ACCOUNTS"
Title.TextColor3 = Color3.fromRGB(255,255,255)
Title.TextStrokeTransparency = 0.7
Title.Font = Enum.Font.GothamBold
Title.TextSize = 19
Title.Parent = MainFrame

local ESPButton = Instance.new("TextButton")
ESPButton.Name = "ESPButton"
ESPButton.Size = UDim2.new(0, 200, 0, 40)
ESPButton.Position = UDim2.new(0.5, -100, 0, 44)
ESPButton.BackgroundColor3 = Color3.fromRGB(35, 35, 35)
ESPButton.TextColor3 = Color3.fromRGB(255, 255, 255)
ESPButton.Text = "ESP PLAYER | 🌇"
ESPButton.Font = Enum.Font.GothamBold
ESPButton.TextSize = 18
ESPButton.Parent = MainFrame

local ESPButtonCorner = Instance.new("UICorner")
ESPButtonCorner.CornerRadius = UDim.new(0, 10)
ESPButtonCorner.Parent = ESPButton

makeDraggable(MainFrame)

-- ESP Vars
local espActive = false
local highlights = {}
local adorns = {}

-- Rainbow update (botão)
RunService.RenderStepped:Connect(function()
    local col = rainbowColor()
    ToggleButton.BackgroundColor3 = col
    UIStroke.Color = rainbowColor(0.25)
    ToggleLabel.TextColor3 = rainbowColor(0.5)
end)

-- Rainbow function for ESP
local function rainbowESP(t)
    return Color3.fromHSV((tick() * 0.2 + t)%1, 1, 1)
end

-- Adiciona Highlight e BillboardGui ao player
local function addESP(player)
    if player == localPlayer then return end
    local character = player.Character
    if not character then return end
    if highlights[player] then return end

    -- Highlight
    local highlight = Instance.new("Highlight")
    highlight.Name = "ESP_Highlight"
    highlight.FillTransparency = 0.7
    highlight.OutlineTransparency = 0.1
    highlight.Parent = character
    highlight.Adornee = character
    highlights[player] = highlight

    -- Billboard para o nome
    local head = character:FindFirstChild("Head")
    if head then
        local billboard = Instance.new("BillboardGui")
        billboard.Name = "ESP_Billboard"
        billboard.Adornee = head
        billboard.Size = UDim2.new(0, 120, 0, 40)
        billboard.StudsOffset = Vector3.new(0, 2.5, 0)
        billboard.AlwaysOnTop = true
        billboard.Parent = head

        local label = Instance.new("TextLabel")
        label.Size = UDim2.new(1, 0, 1, 0)
        label.BackgroundTransparency = 1
        label.Text = "ESP | 🌌"
        label.TextColor3 = Color3.new(1,1,1)
        label.TextStrokeTransparency = 0.25
        label.Font = Enum.Font.GothamBold
        label.TextScaled = true
        label.Parent = billboard

        adorns[player] = billboard
    end
end

-- Remove Highlight/Billboard
local function removeESP(player)
    if highlights[player] then
        highlights[player]:Destroy()
        highlights[player] = nil
    end
    if adorns[player] then
        adorns[player]:Destroy()
        adorns[player] = nil
    end
end

-- Aplica/remova ESP a todos os jogadores (exceto local)
local function setESP(state)
    for _, player in ipairs(Players:GetPlayers()) do
        if player ~= localPlayer then
            if state then
                addESP(player)
            else
                removeESP(player)
            end
        end
    end
end

-- Atualiza cores do Rainbow ESP
RunService.RenderStepped:Connect(function()
    if not espActive then return end
    for player, highlight in pairs(highlights) do
        local t = tick() + (player.UserId % 10)
        highlight.FillColor = rainbowESP(t)
        highlight.OutlineColor = rainbowESP(t + 0.5)
        if adorns[player] and adorns[player]:FindFirstChildOfClass("TextLabel") then
            adorns[player].TextLabel.TextColor3 = rainbowESP(t + 0.25)
        end
    end
end)

-- Liga/desliga ESP ao clicar
ESPButton.MouseButton1Click:Connect(function()
    espActive = not espActive
    setESP(espActive)
    ESPButton.Text = espActive and "DESATIVAR ESP PLAYER | 🌇" or "ESP PLAYER | 🌇"
end)

-- Toggle HUB ao clicar no botão redondo
ToggleButton.MouseButton1Click:Connect(function()
    MainFrame.Visible = not MainFrame.Visible
end)

-- Atualiza para novos jogadores
Players.PlayerAdded:Connect(function(player)
    player.CharacterAdded:Connect(function()
        if espActive then
            wait(1)
            addESP(player)
        end
    end)
end)

-- Remove quando alguém sai
Players.PlayerRemoving:Connect(function(player)
    removeESP(player)
end)

-- Atualiza personagens já existentes
for _, player in ipairs(Players:GetPlayers()) do
    player.CharacterAdded:Connect(function()
        if espActive then
            wait(1)
            addESP(player)
        end
    end)
end

Rayfield:LoadConfiguration()
