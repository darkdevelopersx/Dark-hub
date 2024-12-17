local player = game.Players.LocalPlayer
local camera = game.Workspace.CurrentCamera
local mouse = player:GetMouse()
local RunService = game:GetService("RunService")

-- Aumentando a distância máxima para puxar a mira
local maxDistance = 1000  -- Aumenta a distância para até 1000 studs
local aimSpeed = 5.0      -- Aumenta a velocidade com a qual a mira se ajusta (0.5 é um valor bem alto)

-- Função para pegar o jogador ou NPC mais próximo
local function getClosestTarget()
    local closestPlayer = nil
    local shortestDistance = maxDistance  -- Usando a distância máxima de 1000 studs

    -- Checar todos os jogadores e NPCs
    for _, otherPlayer in pairs(game.Players:GetPlayers()) do
        if otherPlayer ~= player and otherPlayer.Character and otherPlayer.Character:FindFirstChild("HumanoidRootPart") then
            local charPart = otherPlayer.Character.HumanoidRootPart
            local screenPoint = camera:WorldToScreenPoint(charPart.Position)
            local mousePos = Vector2.new(mouse.X, mouse.Y)
            local distance = (Vector2.new(screenPoint.X, screenPoint.Y) - mousePos).Magnitude

            if distance < shortestDistance then
                closestPlayer = charPart
                shortestDistance = distance
            end
        end
    end

    return closestPlayer
end

-- Função para ajustar suavemente a mira
RunService.RenderStepped:Connect(function()
    local target = getClosestTarget()
    if target then
        local targetPosition = camera:WorldToScreenPoint(target.Position)
        local mousePos = Vector2.new(mouse.X, mouse.Y)
        local moveDirection = Vector2.new(targetPosition.X, targetPosition.Y) - mousePo
        
        mouse.X = mouse.X + moveDirection.X * aimSpeed
        mouse.Y = mouse.Y + moveDirection.Y * aimSpeed
    end
end)
