-- GUI COMPLETO: Main + Mini Chat + Status + Player List (com imagem e copy/paste)
-- Feito para executores / LocalScript; mantém o layout principal e adiciona os recursos pedidos

-- ===== SERVIÇOS =====
local Players = game:GetService("Players")
local UserInputService = game:GetService("UserInputService")
local StarterGui = game:GetService("StarterGui")
local RunService = game:GetService("RunService")
local TweenService = game:GetService("TweenService")

local player = Players.LocalPlayer
local PlayerGui = player:WaitForChild("PlayerGui")

-- Remove GUI anterior se existir
local old = PlayerGui:FindFirstChild("HD_GUI")
if old then old:Destroy() end

-- ===== FUNÇÃO DE NOTIFICAÇÃO RÁPIDA =====
local function notify(msg)
    pcall(function()
        StarterGui:SetCore("SendNotification", {
            Title = "Interface",
            Text = msg,
            Duration = 4
        })
    end)
end

-- ===== CRIA GUI PRINCIPAL =====
local ScreenGui = Instance.new("ScreenGui")
ScreenGui.Name = "HD_GUI"
ScreenGui.Parent = PlayerGui
ScreenGui.ResetOnSpawn = false

local MainFrame = Instance.new("Frame", ScreenGui)
MainFrame.Name = "MainFrame"
MainFrame.Size = UDim2.new(0, 300, 0, 220)
MainFrame.Position = UDim2.new(0.1, 0, 0.1, 0)
MainFrame.BackgroundColor3 = Color3.fromRGB(33,33,33)
MainFrame.Active = true
MainFrame.Draggable = true
Instance.new("UICorner", MainFrame).CornerRadius = UDim.new(0,8)

-- fechar
local CloseButton = Instance.new("TextButton", MainFrame)
CloseButton.Size = UDim2.new(0,30,0,30)
CloseButton.Position = UDim2.new(1,-35,0,5)
CloseButton.Text = "X"
CloseButton.Font = Enum.Font.SourceSansBold
CloseButton.TextSize = 20
CloseButton.TextColor3 = Color3.new(1,1,1)
CloseButton.BackgroundColor3 = Color3.fromRGB(215,50,50)
Instance.new("UICorner", CloseButton).CornerRadius = UDim.new(0,6)

-- fly button (mantive o estilo)
local FlyButton = Instance.new("TextButton", MainFrame)
FlyButton.Size = UDim2.new(0,100,0,30)
FlyButton.Position = UDim2.new(0,10,0,50)
FlyButton.Text = "FLY"
FlyButton.Font = Enum.Font.SourceSansBold
FlyButton.TextSize = 18
FlyButton.TextColor3 = Color3.new(1,1,1)
FlyButton.BackgroundColor3 = Color3.fromRGB(0,170,255)
Instance.new("UICorner", FlyButton).CornerRadius = UDim.new(0,6)

-- players button (abre painel lateral)
local PlayersButton = Instance.new("TextButton", MainFrame)
PlayersButton.Size = UDim2.new(0,100,0,30)
PlayersButton.Position = UDim2.new(0,120,0,50)
PlayersButton.Text = "Players"
PlayersButton.Font = Enum.Font.SourceSansBold
PlayersButton.TextSize = 18
PlayersButton.TextColor3 = Color3.new(1,1,1)
PlayersButton.BackgroundColor3 = Color3.fromRGB(0,200,120)
Instance.new("UICorner", PlayersButton).CornerRadius = UDim.new(0,6)

-- Slider (mantive layout)
local SliderLabel = Instance.new("TextLabel", MainFrame)
SliderLabel.Position = UDim2.new(0,10,0,100)
SliderLabel.Size = UDim2.new(0,200,0,20)
SliderLabel.BackgroundTransparency = 1
SliderLabel.Text = "Velocidade de Andar"
SliderLabel.TextColor3 = Color3.new(1,1,1)
SliderLabel.Font = Enum.Font.SourceSansBold
SliderLabel.TextSize = 18

local Bar = Instance.new("Frame", MainFrame)
Bar.Position = UDim2.new(0,10,0,130)
Bar.Size = UDim2.new(0,200,0,10)
Bar.BackgroundColor3 = Color3.fromRGB(70,70,70)
Instance.new("UICorner", Bar).CornerRadius = UDim.new(0,5)

local Ball = Instance.new("Frame", Bar)
Ball.Size = UDim2.new(0,18,0,18)
Ball.AnchorPoint = Vector2.new(0.5,0.5)
Ball.Position = UDim2.new(0.16,0,0.5,0)
Ball.BackgroundColor3 = Color3.new(1,1,1)
Ball.Active = true
Instance.new("UICorner", Ball).CornerRadius = UDim.new(1,0)

local ValueLabel = Instance.new("TextLabel", MainFrame)
ValueLabel.Position = UDim2.new(0,220,0,125)
ValueLabel.Size = UDim2.new(0,40,0,20)
ValueLabel.BackgroundTransparency = 1
ValueLabel.Text = "16"
ValueLabel.TextColor3 = Color3.new(1,1,1)
ValueLabel.Font = Enum.Font.SourceSansBold
ValueLabel.TextSize = 16

-- Toggle Chat button (no MainFrame)
local ToggleChatButton = Instance.new("TextButton", MainFrame)
ToggleChatButton.Size = UDim2.new(0,100,0,30)
ToggleChatButton.Position = UDim2.new(0,10,0,170)
ToggleChatButton.Text = "Chat ON/OFF"
ToggleChatButton.Font = Enum.Font.SourceSansBold
ToggleChatButton.TextSize = 14
ToggleChatButton.TextColor3 = Color3.new(1,1,1)
ToggleChatButton.BackgroundColor3 = Color3.fromRGB(0,140,255)
Instance.new("UICorner", ToggleChatButton).CornerRadius = UDim.new(0,6)

-- ===== PLAYER LIST PANEL (LATERAL BONITO, ANIMADO) =====
local PlayerListFrame = Instance.new("Frame", ScreenGui)
PlayerListFrame.Name = "PlayerList"
PlayerListFrame.Size = UDim2.new(0,300,0,350)
-- inicialmente fora da tela (direita)
PlayerListFrame.Position = UDim2.new(1.05, 0, 0.08, 0)
PlayerListFrame.BackgroundColor3 = Color3.fromRGB(25,25,25)
Instance.new("UICorner", PlayerListFrame).CornerRadius = UDim.new(0,8)
PlayerListFrame.Visible = false

local PLTitle = Instance.new("TextLabel", PlayerListFrame)
PLTitle.Size = UDim2.new(1,0,0,40)
PLTitle.Position = UDim2.new(0,0,0,0)
PLTitle.BackgroundTransparency = 1
PLTitle.Text = "Jogadores"
PLTitle.Font = Enum.Font.SourceSansBold
PLTitle.TextSize = 20
PLTitle.TextColor3 = Color3.new(1,1,1)

local PLClose = Instance.new("TextButton", PlayerListFrame)
PLClose.Size = UDim2.new(0,28,0,28)
PLClose.Position = UDim2.new(1,-34,0,6)
PLClose.Text = "X"
PLClose.Font = Enum.Font.SourceSansBold
PLClose.TextSize = 18
PLClose.TextColor3 = Color3.new(1,1,1)
PLClose.BackgroundColor3 = Color3.fromRGB(180,40,40)
Instance.new("UICorner", PLClose).CornerRadius = UDim.new(0,6)

local PlayerScroll = Instance.new("ScrollingFrame", PlayerListFrame)
PlayerScroll.Size = UDim2.new(1,-10,1,-50)
PlayerScroll.Position = UDim2.new(0,5,0,45)
PlayerScroll.BackgroundTransparency = 1
PlayerScroll.ScrollBarThickness = 6

local UIList = Instance.new("UIListLayout", PlayerScroll)
UIList.SortOrder = Enum.SortOrder.LayoutOrder
UIList.Padding = UDim.new(0,8)

-- Atualiza lista com imagem e animação
local function createPlayerItem(plr)
    local item = Instance.new("Frame")
    item.Size = UDim2.new(1, -10, 0, 60)
    item.BackgroundColor3 = Color3.fromRGB(42,42,42)
    item.ClipsDescendants = true
    Instance.new("UICorner", item).CornerRadius = UDim.new(0,6)

    -- thumbnail
    local thumb = Instance.new("ImageLabel", item)
    thumb.Size = UDim2.new(0,46,0,46)
    thumb.Position = UDim2.new(0,6,0,7)
    thumb.BackgroundTransparency = 1
    thumb.Image = ""
    thumb.ScaleType = Enum.ScaleType.Fit
    -- nomes: DisplayName (verdadeiro) + Username
    local display = Instance.new("TextLabel", item)
    display.Size = UDim2.new(0.6, -10, 0, 26)
    display.Position = UDim2.new(0,60,0,6)
    display.BackgroundTransparency = 1
    display.Text = plr.DisplayName -- nome "verdadeiro"
    display.TextColor3 = Color3.new(1,1,1)
    display.Font = Enum.Font.SourceSansBold
    display.TextSize = 16
    display.TextXAlignment = Enum.TextXAlignment.Left

    local usernameLabel = Instance.new("TextLabel", item)
    usernameLabel.Size = UDim2.new(0.6,-10,0,18)
    usernameLabel.Position = UDim2.new(0,60,0,30)
    usernameLabel.BackgroundTransparency = 1
    usernameLabel.Text = plr.Name
    usernameLabel.TextColor3 = Color3.fromRGB(180,180,180)
    usernameLabel.Font = Enum.Font.SourceSans
    usernameLabel.TextSize = 14
    usernameLabel.TextXAlignment = Enum.TextXAlignment.Left

    -- copy / paste small button
    local copyBtn = Instance.new("TextButton", item)
    copyBtn.Size = UDim2.new(0,56,0,28)
    copyBtn.Position = UDim2.new(1,-66,0,16)
    copyBtn.Text = "Copiar"
    copyBtn.Font = Enum.Font.SourceSansBold
    copyBtn.TextSize = 13
    copyBtn.BackgroundColor3 = Color3.fromRGB(0,170,120)
    copyBtn.TextColor3 = Color3.new(1,1,1)
    Instance.new("UICorner", copyBtn).CornerRadius = UDim.new(0,6)

    -- ao clicar copia e cola no chat box (se existir)
    copyBtn.MouseButton1Click:Connect(function()
        pcall(function() setclipboard(plr.Name) end)
        if ChatBox then
            ChatBox.Text = plr.Name
            ChatBox:CaptureFocus()
        end
        notify("Nick colocado no chat e copiado: "..plr.Name)
    end)

    -- carrega thumbnail com proteção
    spawn(function()
        local ok, img = pcall(function()
            return Players:GetUserThumbnailAsync(plr.UserId, Enum.ThumbnailType.HeadShot, Enum.ThumbnailSize.Size48x48)
        end)
        if ok and img and thumb then
            thumb.Image = img
            thumb.ImageTransparency = 1
            TweenService:Create(thumb, TweenInfo.new(0.35), {ImageTransparency = 0}):Play()
        end
    end)

    return item
end

local function UpdatePlayerList()
    -- remove itens antigos
    for _,c in ipairs(PlayerScroll:GetChildren()) do
        if c:IsA("Frame") then c:Destroy() end
    end
    -- recria
    for i, plr in ipairs(Players:GetPlayers()) do
        local it = createPlayerItem(plr)
        it.Parent = PlayerScroll
        it.BackgroundTransparency = 1
        TweenService:Create(it, TweenInfo.new(0.25, Enum.EasingStyle.Quad), {BackgroundTransparency = 0}):Play()
    end
    -- Atualiza CanvasSize para scroll infinito
    local listLayout = PlayerScroll:FindFirstChildOfClass("UIListLayout")
    if listLayout then
        PlayerScroll.CanvasSize = UDim2.new(0,0,0,listLayout.AbsoluteContentSize.Y + 10)
    end
end

-- também podemos atualizar sempre que o tamanho mudar
PlayerScroll.ChildAdded:Connect(function()
    local listLayout = PlayerScroll:FindFirstChildOfClass("UIListLayout")
    if listLayout then
        PlayerScroll.CanvasSize = UDim2.new(0,0,0,listLayout.AbsoluteContentSize.Y + 10)
    end
end)


Players.PlayerAdded:Connect(function() UpdatePlayerList() end)
Players.PlayerRemoving:Connect(function() UpdatePlayerList() end)

-- close do painel lateral
PLClose.MouseButton1Click:Connect(function()
    if PlayerListFrame.Visible then
        TweenService:Create(PlayerListFrame, TweenInfo.new(0.25, Enum.EasingStyle.Quad), {Position = UDim2.new(1.05,0,0.08,0)}):Play()
        wait(0.25)
        PlayerListFrame.Visible = false
    end
end)

PlayersButton.MouseButton1Click:Connect(function()
    if not PlayerListFrame.Visible then
        UpdatePlayerList()
        PlayerListFrame.Position = UDim2.new(1.05,0,0.08,0)
        PlayerListFrame.Visible = true
        TweenService:Create(PlayerListFrame, TweenInfo.new(0.3, Enum.EasingStyle.Quad), {Position = UDim2.new(0.58,0,0.08,0)}):Play()
    else
        TweenService:Create(PlayerListFrame, TweenInfo.new(0.25, Enum.EasingStyle.Quad), {Position = UDim2.new(1.05,0,0.08,0)}):Play()
        wait(0.25)
        PlayerListFrame.Visible = false
    end
end)

-- ===== MINI CHAT (ABAIXO DO MENU) + STATUS (COMANDO EXECUTADO) =====
local ChatFrame = Instance.new("Frame", ScreenGui)
ChatFrame.Name = "MiniChat"
ChatFrame.Size = UDim2.new(0, 300, 0, 150)
ChatFrame.Position = UDim2.new(0.1, 0, 0.35, 0) -- abaixo do MainFrame
ChatFrame.BackgroundColor3 = Color3.fromRGB(38,38,38)
Instance.new("UICorner", ChatFrame).CornerRadius = UDim.new(0,8)
ChatFrame.Visible = false

-- status bar (mostra resultado do último comando)
local StatusFrame = Instance.new("Frame", ChatFrame)
StatusFrame.Size = UDim2.new(1, -20, 0, 28)
StatusFrame.Position = UDim2.new(0, 10, 0, 8)
StatusFrame.BackgroundColor3 = Color3.fromRGB(30,30,30)
Instance.new("UICorner", StatusFrame).CornerRadius = UDim.new(0,6)

local StatusLabel = Instance.new("TextLabel", StatusFrame)
StatusLabel.Size = UDim2.new(1, -10, 1, 0)
StatusLabel.Position = UDim2.new(0,8,0,0)
StatusLabel.BackgroundTransparency = 1
StatusLabel.Text = "Status: pronto"
StatusLabel.TextColor3 = Color3.fromRGB(200,200,200)
StatusLabel.Font = Enum.Font.SourceSans
StatusLabel.TextSize = 14
StatusLabel.TextXAlignment = Enum.TextXAlignment.Left

-- chat log
local ChatLog = Instance.new("ScrollingFrame", ChatFrame)
ChatLog.Size = UDim2.new(1, -20, 0, 80)
ChatLog.Position = UDim2.new(0,10,0,44)
ChatLog.BackgroundTransparency = 1
ChatLog.ScrollBarThickness = 6
local ChatListLayout = Instance.new("UIListLayout", ChatLog)
ChatListLayout.Padding = UDim.new(0,6)
ChatListLayout.SortOrder = Enum.SortOrder.LayoutOrder

-- input
local ChatBox = Instance.new("TextBox", ChatFrame)
ChatBox.Size = UDim2.new(1, -100, 0, 28)
ChatBox.Position = UDim2.new(0,10,1,-38)
ChatBox.PlaceholderText = "Digite comando (ex: kill nome /tp nome /tpall)..."
ChatBox.BackgroundColor3 = Color3.fromRGB(55,55,55)
ChatBox.TextColor3 = Color3.new(1,1,1)
ChatBox.Font = Enum.Font.SourceSans
ChatBox.TextSize = 15
Instance.new("UICorner", ChatBox).CornerRadius = UDim.new(0,6)

local SendBtn = Instance.new("TextButton", ChatFrame)
SendBtn.Size = UDim2.new(0,72,0,28)
SendBtn.Position = UDim2.new(1,-82,1,-38)
SendBtn.Text = "Enviar"
SendBtn.Font = Enum.Font.SourceSansBold
SendBtn.TextSize = 14
SendBtn.BackgroundColor3 = Color3.fromRGB(0,170,120)
SendBtn.TextColor3 = Color3.new(1,1,1)
Instance.new("UICorner", SendBtn).CornerRadius = UDim.new(0,6)

local function AddChatMessage(text)
    local label = Instance.new("TextLabel", ChatLog)
    label.Size = UDim2.new(1, -10, 0, 20)
    label.BackgroundTransparency = 1
    label.Text = text
    label.TextColor3 = Color3.new(1,1,1)
    label.Font = Enum.Font.SourceSans
    label.TextSize = 14
    label.TextXAlignment = Enum.TextXAlignment.Left
    -- update canvas
    ChatLog.CanvasSize = UDim2.new(0,0,0, ChatListLayout.AbsoluteContentSize + 10)
    -- scroll to bottom
    ChatLog.CanvasPosition = Vector2.new(0, math.max(0, ChatListLayout.AbsoluteContentSize - ChatLog.AbsoluteSize.Y))
end

local function showStatus(msg)
    StatusLabel.Text = "Status: "..msg
    StatusFrame.BackgroundColor3 = Color3.fromRGB(30,30,30)
    -- pequena animação de destaque
    local t1 = TweenService:Create(StatusFrame, TweenInfo.new(0.18, Enum.EasingStyle.Quad), {BackgroundColor3 = Color3.fromRGB(0,150,80)})
    local t2 = TweenService:Create(StatusFrame, TweenInfo.new(0.35, Enum.EasingStyle.Quad), {BackgroundColor3 = Color3.fromRGB(30,30,30)})
    t1:Play()
    t1.Completed:Connect(function() wait(0.6); t2:Play() end)
end

-- ToggleChat liga/desliga o MiniChat
ToggleChatButton.MouseButton1Click:Connect(function()
    if MainFrame.Visible == false then
        MainFrame.Visible = true
    end
    ChatFrame.Visible = not ChatFrame.Visible
end)

-- quando fechar o main, fecha o chat e a lista
CloseButton.MouseButton1Click:Connect(function()
    MainFrame.Visible = false
    ChatFrame.Visible = false
    if PlayerListFrame.Visible then
        TweenService:Create(PlayerListFrame, TweenInfo.new(0.18), {Position = UDim2.new(1.05,0,0.08,0)}):Play()
        wait(0.18)
        PlayerListFrame.Visible = false
    end
    notify("Menu fechado. Pressione [M] para abrir.")
end)

-- ===== FUNÇÕES DE COMANDO (KILL, FREEZE, UNFREEZE, TP, TPALL) =====
local function findPlayerByPartial(name)
    if not name then return nil end
    name = string.lower(name)
    for _, plr in ipairs(Players:GetPlayers()) do
        if string.sub(string.lower(plr.Name),1,#name) == name then
            return plr
        end
    end
    return nil
end

local function executeCommand(raw)
    if not raw then return end
    local parts = {}
    for part in string.gmatch(raw, "%S+") do table.insert(parts, part) end
    local cmd = string.lower(parts[1] or "")
    local targetArg = parts[2]

    if cmd == "/tpall" or cmd == "tpall" then
        if player.Character and player.Character:FindFirstChild("HumanoidRootPart") then
            local myHRP = player.Character.HumanoidRootPart
            for _,plr in ipairs(Players:GetPlayers()) do
                if plr ~= player and plr.Character and plr.Character:FindFirstChild("HumanoidRootPart") then
                    plr.Character.HumanoidRootPart.CFrame = myHRP.CFrame + Vector3.new(math.random(-3,3),0,math.random(-3,3))
                end
            end
            AddChatMessage("Você: "..raw)
            showStatus("TPALL executado")
        else
            showStatus("Seu personagem não disponível")
        end
        return
    end

    if cmd == "kill" then
        if not targetArg then showStatus("Use: kill <nome>"); return end
        local target = findPlayerByPartial(targetArg)
        if target and target.Character and target.Character:FindFirstChildOfClass("Humanoid") then
            target.Character:FindFirstChildOfClass("Humanoid").Health = 0
            AddChatMessage("Você: "..raw)
            showStatus("kill executado em "..target.Name)
        else
            showStatus("Jogador não encontrado")
        end
        return
    end

    if cmd == "freeze" then
        if not targetArg then showStatus("Use: freeze <nome>"); return end
        local target = findPlayerByPartial(targetArg)
        if target and target.Character and target.Character:FindFirstChild("HumanoidRootPart") then
            target.Character.HumanoidRootPart.Anchored = true
            AddChatMessage("Você: "..raw)
            showStatus("freeze em "..target.Name)
        else
            showStatus("Jogador não encontrado")
        end
        return
    end

    if cmd == "unfreeze" then
        if not targetArg then showStatus("Use: unfreeze <nome>"); return end
        local target = findPlayerByPartial(targetArg)
        if target and target.Character and target.Character:FindFirstChild("HumanoidRootPart") then
            target.Character.HumanoidRootPart.Anchored = false
            AddChatMessage("Você: "..raw)
            showStatus("unfreeze em "..target.Name)
        else
            showStatus("Jogador não encontrado")
        end
        return
    end

    if cmd == "tp" then
        if not targetArg then showStatus("Use: tp <nome>"); return end
        local target = findPlayerByPartial(targetArg)
        if target and target.Character and target.Character:FindFirstChild("HumanoidRootPart") and player.Character and player.Character:FindFirstChild("HumanoidRootPart") then
            player.Character.HumanoidRootPart.CFrame = target.Character.HumanoidRootPart.CFrame + Vector3.new(2,0,2)
            AddChatMessage("Você: "..raw)
            showStatus("Teleportado até "..target.Name)
        else
            showStatus("Jogador não encontrado / seu personagem indisponível")
        end
        return
    end

    -- caso seja só mensagem livre:
    AddChatMessage(player.Name..": "..raw)
    showStatus("Mensagem enviada")
end

-- enviar via botão ou enter
SendBtn.MouseButton1Click:Connect(function()
    local text = ChatBox.Text
    if text and text:match("%S") then
        executeCommand(text)
        ChatBox.Text = ""
    end
end)

ChatBox.FocusLost:Connect(function(enterPressed)
    if enterPressed then
        local text = ChatBox.Text
        if text and text:match("%S") then
            executeCommand(text)
            ChatBox.Text = ""
        end
    end
end)

-- ===== SLIDER LÓGICA (velocidade de andar) =====
local dragging = false
local defaultSpeed = 16
-- posição inicial do ball de acordo com defaultSpeed
Ball.Position = UDim2.new(defaultSpeed/100, 0, 0.5, 0)
ValueLabel.Text = tostring(defaultSpeed)
if player.Character and player.Character:FindFirstChildOfClass("Humanoid") then
    pcall(function() player.Character:FindFirstChildOfClass("Humanoid").WalkSpeed = defaultSpeed end)
end

Ball.InputBegan:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.MouseButton1 then
        dragging = true
    end
end)
Bar.InputBegan:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.MouseButton1 then
        dragging = true
    end
end)
UserInputService.InputEnded:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.MouseButton1 then dragging = false end
end)

RunService.RenderStepped:Connect(function()
    if dragging then
        local mouseX = UserInputService:GetMouseLocation().X
        local startX = Bar.AbsolutePosition.X
        local sizeX = Bar.AbsoluteSize.X
        local pos = math.clamp((mouseX - startX) / math.max(sizeX,1), 0, 1)
        Ball.Position = UDim2.new(pos, 0, 0.5, 0)
        local speed = math.floor(pos * 100)
        ValueLabel.Text = tostring(speed)
        if player.Character and player.Character:FindFirstChildOfClass("Humanoid") then
            pcall(function() player.Character:FindFirstChildOfClass("Humanoid").WalkSpeed = speed end)
        end
    end
end)

-- ===== FLY COM BOTÃO (igual no Studio) =====
local flying = false
local bodyGyro, bodyVelocity
local flySpeed = 120

local function startFly()
    local char = player.Character or player.CharacterAdded:Wait()
    local hrp = char:WaitForChild("HumanoidRootPart")
    local hum = char:FindFirstChildOfClass("Humanoid")
    if not hrp or not hum then return end

    bodyGyro = Instance.new("BodyGyro")
    bodyGyro.P = 9e4
    bodyGyro.MaxTorque = Vector3.new(9e9,9e9,9e9)
    bodyGyro.CFrame = hrp.CFrame
    bodyGyro.Parent = hrp

    bodyVelocity = Instance.new("BodyVelocity")
    bodyVelocity.MaxForce = Vector3.new(9e9,9e9,9e9)
    bodyVelocity.Velocity = Vector3.new(0,0,0)
    bodyVelocity.Parent = hrp

    char.Humanoid.PlatformStand = true -- impede movimentos normais
    flying = true
    notify("Fly ativado")
end

local function stopFly()
    flying = false
    if bodyGyro then bodyGyro:Destroy(); bodyGyro = nil end
    if bodyVelocity then bodyVelocity:Destroy(); bodyVelocity = nil end
    player.Character.Humanoid.PlatformStand = false
    notify("Fly desativado")
end

FlyButton.MouseButton1Click:Connect(function()
    if flying then stopFly() else startFly() end
end)

RunService.RenderStepped:Connect(function(delta)
    if flying and player.Character and player.Character:FindFirstChild("HumanoidRootPart") then
        local hrp = player.Character.HumanoidRootPart
        local cam = workspace.CurrentCamera
        local move = Vector3.zero

        if UserInputService:GetFocusedTextBox() then
            if bodyVelocity then bodyVelocity.Velocity = Vector3.new(0,0,0) end
            return
        end

        if not isMobile then
            -- PC: WSAD
            if UserInputService:IsKeyDown(Enum.KeyCode.W) then move = move + cam.CFrame.LookVector end
            if UserInputService:IsKeyDown(Enum.KeyCode.S) then move = move - cam.CFrame.LookVector end
            if UserInputService:IsKeyDown(Enum.KeyCode.A) then move = move - cam.CFrame.RightVector end
            if UserInputService:IsKeyDown(Enum.KeyCode.D) then move = move + cam.CFrame.RightVector end
        else
            -- Mobile: pega direção do Humanoid MoveDirection (virtual joystick)
            local hum = player.Character:FindFirstChildOfClass("Humanoid")
            if hum then
                move = hum.MoveDirection * flySpeed
            end
        end

        if move.Magnitude > 0 then move = move.Unit * flySpeed end

        -- Rotação suave
        if bodyGyro then
            local targetCFrame = CFrame.new(hrp.Position, hrp.Position + cam.CFrame.LookVector)
            bodyGyro.CFrame = bodyGyro.CFrame:Lerp(targetCFrame, 0.2)
        end

        if bodyVelocity then
            bodyVelocity.Velocity = move
        end
    end
end)




-- ===== TECLA M (não ativa se estiver digitando no Chat) e fecha chat se menu fechar =====
local guiVisible = true
UserInputService.InputBegan:Connect(function(input, gameProcessed)
    if gameProcessed then return end
    if UserInputService:GetFocusedTextBox() then return end
    if input.KeyCode == Enum.KeyCode.M then
        guiVisible = not guiVisible
        MainFrame.Visible = guiVisible
        if not guiVisible then
            ChatFrame.Visible = false
            if PlayerListFrame.Visible then
                TweenService:Create(PlayerListFrame, TweenInfo.new(0.18), {Position = UDim2.new(1.05,0,0.08,0)}):Play()
                wait(0.18)
                PlayerListFrame.Visible = false
            end
        end
    end
end)

-- se o usuário fechar manualmente o main com o botão, já tratamos mais acima
CloseButton.MouseButton1Click:Connect(function()
    MainFrame.Visible = false
    ChatFrame.Visible = false
end)

-- ===== INICIALIZAÇÕES =====
UpdatePlayerList()
notify("Interface carregada! Players menu, Chat e Status prontos.")


-- ===== BOLINHA MOBILE PARA ABRIR MENU =====
local isMobile = UserInputService.TouchEnabled and not UserInputService.KeyboardEnabled
if isMobile then
    local OpenButton = Instance.new("ImageButton", PlayerGui)
    OpenButton.Size = UDim2.new(0,50,0,50)
    OpenButton.Position = UDim2.new(0.9,0,0.9,0)
    OpenButton.BackgroundColor3 = Color3.fromRGB(0,170,255)
    OpenButton.Image = "" -- coloque um ícone se quiser
    OpenButton.ZIndex = 10
    Instance.new("UICorner", OpenButton).CornerRadius = UDim.new(1,0)

    OpenButton.MouseButton1Click:Connect(function()
        MainFrame.Visible = not MainFrame.Visible
        ChatFrame.Visible = MainFrame.Visible
    end)
end
