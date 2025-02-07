local players = game:GetService("Players")
local localPlayer = players.LocalPlayer
local runService = game:GetService("RunService")

local espEnabled = true -- Variável para ativar/desativar o ESP

-- Função para criar o ESP (caixa ao redor dos jogadores)
local function createESP(character, color)
    if character and character:FindFirstChild("HumanoidRootPart") then
        local highlight = Instance.new("Highlight")
        highlight.Parent = character
        highlight.Adornee = character
        highlight.FillColor = color
        highlight.OutlineColor = Color3.new(1, 1, 1) -- Cor da borda branca
        highlight.FillTransparency = 0.5
        highlight.OutlineTransparency = 0.1
    end
end

-- Função para atualizar ESP
local function updateESP()
    if not espEnabled then
        for _, player in pairs(players:GetPlayers()) do
            if player.Character and player.Character:FindFirstChild("Highlight") then
                player.Character.Highlight:Destroy()
            end
        end
        return
    end

    for _, player in pairs(players:GetPlayers()) do
        if player ~= localPlayer and player.Team and localPlayer.Team then
            if player.Character and not player.Character:FindFirstChild("Highlight") then
                local color = player.Team == localPlayer.Team and Color3.new(0, 0, 1) or Color3.new(1, 0, 0)
                createESP(player.Character, color)
            end
        end
    end
end

-- Loop para atualizar o ESP constantemente
runService.RenderStepped:Connect(updateESP)

-- Tecla para ativar/desativar o ESP
game:GetService("UserInputService").InputBegan:Connect(function(input, gameProcessed)
    if gameProcessed then return end
    if input.KeyCode == Enum.KeyCode.F4 then -- Pressione F4 para ativar/desativar
        espEnabled = not espEnabled
        updateESP()
    end
end)
