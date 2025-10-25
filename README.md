-- Carrega a biblioteca Rayfield
local Rayfield = loadstring(game:HttpGet('https://sirius.menu/rayfield'))()

-- Cria a janela principal
local Window = Rayfield:CreateWindow({
    Name = "🌱 Plantinha Hub",
    LoadingTitle = "Carregando Plantinha Hub...",
    LoadingSubtitle = "YouTube: pikachuzim_blox",
    ConfigurationSaving = {
        Enabled = true,
        FolderName = nil,
        FileName = "PlantinhaHubConfig"
    },
    Discord = {
        Enabled = false,
        Invite = "",
        RememberJoins = true
    },
    KeySystem = false, -- Sem sistema de key
    IntroText = "Bem-vindo ao Plantinha Hub!"
})

-- Cria uma aba de funções
local MainTab = Window:CreateTab("Funções", 4483362458)

-- Container de sliders
local floatHeight = 3
local floatSpeed = 0.3
local floatD = 25
local floatP = 1000
local floatActive = false
local bodyPos, conn

-- Botão FloatV2 toggle
MainTab:CreateButton({
    Name = "FloatV2",
    Callback = function()
        local player = game.Players.LocalPlayer
        local RunService = game:GetService("RunService")
        local UIS = game:GetService("UserInputService")
        local char = player.Character or player.CharacterAdded:Wait()
        local hrp = char:WaitForChild("HumanoidRootPart")
        
        if not floatActive then
            bodyPos = Instance.new("BodyPosition")
            bodyPos.MaxForce = Vector3.new(1e5,1e5,1e5)
            bodyPos.D = floatD
            bodyPos.P = floatP
            bodyPos.Position = hrp.Position
            bodyPos.Parent = hrp
            
            conn = RunService.Heartbeat:Connect(function()
                local moveDir = Vector3.new(0,0,0)
                if UIS:IsKeyDown(Enum.KeyCode.W) then moveDir = moveDir + workspace.CurrentCamera.CFrame.LookVector end
                if UIS:IsKeyDown(Enum.KeyCode.S) then moveDir = moveDir - workspace.CurrentCamera.CFrame.LookVector end
                if UIS:IsKeyDown(Enum.KeyCode.A) then moveDir = moveDir - workspace.CurrentCamera.CFrame.RightVector end
                if UIS:IsKeyDown(Enum.KeyCode.D) then moveDir = moveDir + workspace.CurrentCamera.CFrame.RightVector end
                
                local targetPos = hrp.Position + moveDir * floatSpeed
                bodyPos.Position = targetPos + Vector3.new(0,floatHeight,0)
            end)
            floatActive = true
        else
            if bodyPos then bodyPos:Destroy() end
            if conn then conn:Disconnect() end
            floatActive = false
        end
    end
})

-- Sliders para altura, velocidade e suavidade
MainTab:CreateSlider({
    Name = "Altura",
    Range = {1, 10},
    Increment = 0.1,
    Suffix = " studs",
    CurrentValue = floatHeight,
    Flag = "AlturaFloat",
    Callback = function(value)
        floatHeight = value
    end
})

MainTab:CreateSlider({
    Name = "Velocidade",
    Range = {0.1, 3},
    Increment = 0.05,
    Suffix = "",
    CurrentValue = floatSpeed,
    Flag = "VelocidadeFloat",
    Callback = function(value)
        floatSpeed = value
    end
})

MainTab:CreateSlider({
    Name = "Suavidade(D)",
    Range = {1, 50},
    Increment = 1,
    Suffix = "",
    CurrentValue = floatD,
    Flag = "SuavidadeFloat",
    Callback = function(value)
        floatD = value
        if bodyPos then bodyPos.D = floatD end
    end
})

MainTab:CreateSlider({
    Name = "Força(P)",
    Range = {100, 5000},
    Increment = 50,
    Suffix = "",
    CurrentValue = floatP,
    Flag = "ForcaFloat",
    Callback = function(value)
        floatP = value
        if bodyPos then bodyPos.P = floatP end
    end
})

-- Adiciona um botão minimizar que vira uma bolinha
local MiniButton = Window:CreateButton({
    Name = "Minimizar",
    Callback = function()
        Window:Toggle()
    end
})# Ffjfjr
