-- UI et bibliothèque
local Library = loadstring(game:HttpGet("https://raw.githubusercontent.com/xHeptc/Kavo-UI-Library/main/source.lua"))()
local Window = Library.CreateLib("Pour Toi Mon BB", "Serpent")

-- Onglet Accueil
local Main = Window:NewTab("Accueil")
local Section = Main:NewSection("Clique ici 👇")

-- Bouton Image
Section:NewButton("J'ai peur", "Clique pour afficher ta peur", function()
    local ScreenGui = Instance.new("ScreenGui", game.Players.LocalPlayer:WaitForChild("PlayerGui"))
    ScreenGui.Name = "ImageDisplayGui"

    local ImageLabel = Instance.new("ImageLabel", ScreenGui)
    ImageLabel.Size = UDim2.new(1, 0, 1, 0)
    ImageLabel.Position = UDim2.new(0, 0, 0, 0)
    ImageLabel.BackgroundTransparency = 1
    ImageLabel.ScaleType = Enum.ScaleType.Fit
    ImageLabel.Name = "ImagePourToi"
    ImageLabel.Image = "rbxassetid://72495273950314"

    local CloseBtn = Instance.new("TextButton", ScreenGui)
    CloseBtn.Size = UDim2.new(0, 120, 0, 50)
    CloseBtn.Position = UDim2.new(1, -130, 1, -60)
    CloseBtn.Text = "Fermer"
    CloseBtn.BackgroundColor3 = Color3.fromRGB(200, 0, 0)
    CloseBtn.TextColor3 = Color3.fromRGB(255, 255, 255)
    CloseBtn.Font = Enum.Font.GothamBold
    CloseBtn.TextSize = 20

    CloseBtn.MouseButton1Click:Connect(function()
        ScreenGui:Destroy()
    end)
end)

Section:NewButton("Tes Bizzare", "Clique pour afficher Un mec bizzare", function()
    local ScreenGui = Instance.new("ScreenGui", game.Players.LocalPlayer:WaitForChild("PlayerGui"))
    ScreenGui.Name = "ImageDisplayGui"

    local ImageLabel = Instance.new("ImageLabel", ScreenGui)
    ImageLabel.Size = UDim2.new(1, 0, 1, 0)
    ImageLabel.Position = UDim2.new(0, 0, 0, 0)
    ImageLabel.BackgroundTransparency = 1
    ImageLabel.ScaleType = Enum.ScaleType.Fit
    ImageLabel.Name = "ImagePourToi"
    ImageLabel.Image = "rbxassetid://139905770361225"

    local CloseBtn = Instance.new("TextButton", ScreenGui)
    CloseBtn.Size = UDim2.new(0, 120, 0, 50)
    CloseBtn.Position = UDim2.new(1, -130, 1, -60)
    CloseBtn.Text = "Fermer"
    CloseBtn.BackgroundColor3 = Color3.fromRGB(200, 0, 0)
    CloseBtn.TextColor3 = Color3.fromRGB(255, 255, 255)
    CloseBtn.Font = Enum.Font.GothamBold
    CloseBtn.TextSize = 20

    CloseBtn.MouseButton1Click:Connect(function()
        ScreenGui:Destroy()
    end)
end)

Section:NewButton("Un Arabe", "Clique pour afficher un Arabe", function()
    local ScreenGui = Instance.new("ScreenGui", game.Players.LocalPlayer:WaitForChild("PlayerGui"))
    ScreenGui.Name = "ImageDisplayGui"

    local ImageLabel = Instance.new("ImageLabel", ScreenGui)
    ImageLabel.Size = UDim2.new(1, 0, 1, 0)
    ImageLabel.Position = UDim2.new(0, 0, 0, 0)
    ImageLabel.BackgroundTransparency = 1
    ImageLabel.ScaleType = Enum.ScaleType.Fit
    ImageLabel.Name = "ImagePourToi"
    ImageLabel.Image = "rbxassetid://89185641663180"

    local CloseBtn = Instance.new("TextButton", ScreenGui)
    CloseBtn.Size = UDim2.new(0, 120, 0, 50)
    CloseBtn.Position = UDim2.new(1, -130, 1, -60)
    CloseBtn.Text = "Fermer"
    CloseBtn.BackgroundColor3 = Color3.fromRGB(200, 0, 0)
    CloseBtn.TextColor3 = Color3.fromRGB(255, 255, 255)
    CloseBtn.Font = Enum.Font.GothamBold
    CloseBtn.TextSize = 20

    CloseBtn.MouseButton1Click:Connect(function()
        ScreenGui:Destroy()
    end)
end)

-- Auto Farm Tab
local AutoFarmTab = Window:NewTab("Auto Farm")
local AutoFarmSection = AutoFarmTab:NewSection("Collecte automatique")

local Players = game:GetService("Players")
local player = Players.LocalPlayer
local maxCoins = 50
local coinName = "Coin_Server"
local speed = 30 -- studs/sec
local autoFarmEnabled = false
local farmCoroutine = nil

local function getCharacter()
    return player.Character or player.CharacterAdded:Wait()
end

local function getRootPart(char)
    return char:WaitForChild("HumanoidRootPart")
end

local function findNearestCoin(rootPart, radius)
    local closestCoin = nil
    local closestDist = radius

    for _, descendant in ipairs(workspace:GetDescendants()) do
        if descendant:IsA("TouchTransmitter") and descendant.Parent and descendant.Parent.Name == coinName then
            local coinPart = descendant.Parent
            local dist = (rootPart.Position - coinPart.Position).Magnitude
            if dist < closestDist then
                closestDist = dist
                closestCoin = coinPart
            end
        end
    end

    return closestCoin
end

local function moveToPosition(character, targetPos, speed)
    local rootPart = getRootPart(character)
    local startPos = rootPart.Position
    local distance = (targetPos - startPos).Magnitude
    local duration = distance / speed
    local startTime = tick()

    while true do
        if not autoFarmEnabled then return false end -- stop if toggle off

        local elapsed = tick() - startTime
        local alpha = math.min(elapsed / duration, 1)
        local newPos = startPos:Lerp(targetPos, alpha)
        character:PivotTo(CFrame.new(newPos))

        if alpha >= 1 then break end
        task.wait()
    end
    return true
end

local function resetCharacter(char)
    local humanoid = char:FindFirstChildOfClass("Humanoid")
    if humanoid then
        humanoid.Health = 0
    end
end

local function waitForNextGame()
    local gui = player:WaitForChild("PlayerGui")
    repeat
        task.wait(1)
    until gui:FindFirstChild("MainGUI") and gui.MainGUI:FindFirstChild("Game") and gui.MainGUI.Game.Visible
end

local function goToCoinAndWaitPickup(char, coin)
    local rootPart = getRootPart(char)
    local touched = false

    local touchConn
    touchConn = coin.Touched:Connect(function(hit)
        if hit:IsDescendantOf(char) then
            touched = true
            if touchConn then
                touchConn:Disconnect()
                touchConn = nil
            end
        end
    end)

    local moved = moveToPosition(char, coin.Position, speed)
    if not moved then
        if touchConn then touchConn:Disconnect() end
        return false
    end

    local startWait = tick()
    while not touched and tick() - startWait < 5 do
        if not autoFarmEnabled then
            if touchConn then touchConn:Disconnect() end
            return false
        end
        task.wait(0.1)
        if not coin.Parent then
            break
        end
    end

    if touchConn then
        touchConn:Disconnect()
    end

    return touched
end

local function autoFarm()
    while autoFarmEnabled do
        local char = getCharacter()
        local rootPart = getRootPart(char)
        local coinsCollected = 0

        while autoFarmEnabled and coinsCollected < maxCoins do
            local coin = findNearestCoin(rootPart, 200)
            if coin then
                local success = goToCoinAndWaitPickup(char, coin)
                if success then
                    coinsCollected += 1
                    print("Pièces collectées : " .. coinsCollected)
                    task.wait(0.2)
                else
                    print("Pièce prise par un autre joueur avant d'arriver.")
                    task.wait(0.5)
                end
            else
                task.wait(1)
            end
        end

        if not autoFarmEnabled then break end

        print("40 pièces collectées, reset du personnage...")
        resetCharacter(char)
        waitForNextGame()
    end
end

AutoFarmSection:NewToggle("Auto Farm", "Active/Désactive la collecte automatique", function(state)
    autoFarmEnabled = state
    if state then
        farmCoroutine = coroutine.create(autoFarm)
        coroutine.resume(farmCoroutine)
    else
        if farmCoroutine then
            coroutine.close(farmCoroutine)
            farmCoroutine = nil
        end
    end
end)
-- ESP Tab
local ESPTab = Window:NewTab("ESP")
local ESPSection = ESPTab:NewSection("ESP Joueurs")

-- Variables ESP
local ESPParts = {}
local NameTags = {}
local connections = {}
local activeESP = false
local showNames = false

-- Créer un ESP + name tag sur un joueur
local function addESPToCharacter(player, char)
    if player == game.Players.LocalPlayer then return end

    -- Détection rôle (gun ou knife dans le backpack)
    local color = Color3.new(1, 1, 1) -- Blanc par défaut
    local backpack = player:FindFirstChild("Backpack")
    if backpack then
        for _, tool in pairs(backpack:GetChildren()) do
            if tool.Name:lower():find("knife") then
                color = Color3.new(1, 0, 0) -- Rouge = meurtrier
            elseif tool.Name:lower():find("gun") then
                color = Color3.new(0, 0.5, 1) -- Bleu = shérif
            end
        end
    end

    if activeESP then
        -- Box ESP
        for _, part in pairs(char:GetDescendants()) do
            if part:IsA("BasePart") and not part:FindFirstChild("BodyESP") then
                local box = Instance.new("BoxHandleAdornment")
                box.Name = "BodyESP"
                box.Adornee = part
                box.AlwaysOnTop = true
                box.ZIndex = 10
                box.Size = part.Size
                box.Color3 = color
                box.Transparency = 0.4
                box.Parent = part
                table.insert(ESPParts, box)
            end
        end
    end

    -- Pseudo
    if showNames and char:FindFirstChild("HumanoidRootPart") then
        local billboard = Instance.new("BillboardGui", char.HumanoidRootPart)
        billboard.Name = "NameDisplay"
        billboard.Size = UDim2.new(0, 200, 0, 50)
        billboard.Adornee = char.HumanoidRootPart
        billboard.AlwaysOnTop = true

        local label = Instance.new("TextLabel", billboard)
        label.Size = UDim2.new(1, 0, 1, 0)
        label.BackgroundTransparency = 1
        label.Text = player.Name
        label.TextColor3 = color
        label.TextStrokeTransparency = 0.5
        label.Font = Enum.Font.GothamBold
        label.TextScaled = true

        table.insert(NameTags, billboard)
    end
end

local function applyESP(player)
    if player == game.Players.LocalPlayer then return end

    if player.Character then
        addESPToCharacter(player, player.Character)
    end

    local charConn = player.CharacterAdded:Connect(function(char)
        wait(1)
        addESPToCharacter(player, char)
    end)
    table.insert(connections, charConn)
end

local function clearESP()
    for _, esp in ipairs(ESPParts) do
        if esp and esp.Parent then esp:Destroy() end
    end
    ESPParts = {}

    for _, tag in ipairs(NameTags) do
        if tag and tag.Parent then tag:Destroy() end
    end
    NameTags = {}

    for _, conn in pairs(connections) do
        if conn.Disconnect then conn:Disconnect() end
    end
    connections = {}
end

ESPSection:NewToggle("Activer ESP", "Cadre sur les joueurs", function(state)
    activeESP = state
    clearESP()
    for _, player in pairs(game.Players:GetPlayers()) do
        applyESP(player)
    end
    local addConn = game.Players.PlayerAdded:Connect(function(player)
        applyESP(player)
    end)
    table.insert(connections, addConn)
end)

ESPSection:NewToggle("Name Player", "Affiche les noms des joueurs", function(state)
    showNames = state
    clearESP()
    for _, player in pairs(game.Players:GetPlayers()) do
        applyESP(player)
    end
end)


