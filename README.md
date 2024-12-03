-- List of banned user IDs
local bannedIDs = {
    345673454,  -- Replace with actual user IDs
    34567234523,
    456789123
}

-- Function to check if the player is banned
local function checkBan(userId)
    for _, bannedId in ipairs(bannedIDs) do
        if userId == bannedId then
            return true
        end
    end
    return false
end

-- Get Local Player and Check Ban Before Running Script
local localPlayer = game.Players.LocalPlayer

if checkBan(localPlayer.UserId) then
    localPlayer:Kick("You have been banned off the GUI. Reason: ")
    return -- Stop the script from running any further
end

-- Rest of the script only runs if the player is NOT banned
print("SuperNatural Script: Loading..")
print("Loaded!")

-- Load Rayfield library
local Rayfield = loadstring(game:HttpGet('https://sirius.menu/rayfield'))()

-- Create the main window
local Window = Rayfield:CreateWindow({
    Name = "SUPERNATURAL HUB",
    LoadingTitle = "SuperNatural",
    LoadingSubtitle = "By Castiel",
    ConfigurationSaving = {
        Enabled = true,
        FolderName = nil,
        FileName = "SUPERNATURAL HUB"
    },
    Discord = {
        Enabled = false,
        Invite = "H66dDfrS4Q",
        RememberJoins = false 
    },
    KeySystem = false
})

-- Create tabs
local MainTab = Window:CreateTab("Local & Main", nil)
local SurviTab = Window:CreateTab("Species", nil)
local ESTab = Window:CreateTab("ESP", nil)
local ExtrTab = Window:CreateTab("TP", nil)
local SigmaTab = Window:CreateTab("Game", nil)
local GameTab = Window:CreateTab("Trolling", nil)
local EvntTab = Window:CreateTab("Info", nil)

-- Show a notification
Rayfield:Notify({
    Title = "Success!",
    Content = "Welcome!",
    Duration = 6.5,
    Image = nil,
    Actions = {
        Ignore = {
            Name = "Close",
            Callback = function() end
        }
    }
})

local infiniteJumpEnabled = false

game:GetService("UserInputService").JumpRequest:Connect(function()
    if infiniteJumpEnabled then
        game.Players.LocalPlayer.Character:FindFirstChildOfClass("Humanoid"):ChangeState("Jumping")
    end
end)

MainTab:CreateButton({
    Name = "Rejoin",
    Callback = function()
    local TeleportService = game:GetService("TeleportService")
local Players = game:GetService("Players")
local LocalPlayer = Players.LocalPlayer
 
local Rejoin = coroutine.create(function()
    local Success, ErrorMessage = pcall(function()
        TeleportService:Teleport(game.PlaceId, LocalPlayer)
    end)
 
    if ErrorMessage and not Success then
        warn(ErrorMessage)
    end
end)
 
coroutine.resume(Rejoin)
    end
})

MainTab:CreateSection("Local")

local infjToggle = false
local localPlayer = game.Players.LocalPlayer
local infiniteJumpEnabled = false -- Variable to control infinite jump

local infJump = MainTab:CreateToggle({
    Name = "Infinite Jump",
    Default = false,
    Callback = function(state)
    local infiniteJumpEnabled = false

game:GetService("UserInputService").JumpRequest:Connect(function()
    if infiniteJumpEnabled then
        game.Players.LocalPlayer.Character:FindFirstChildOfClass("Humanoid"):ChangeState("Jumping")
    end
end)
        infjToggle = state
        infiniteJumpEnabled = infjToggle -- Set the infiniteJumpEnabled based on the toggle state
        end
})

-- Create the toggle
MainTab:CreateToggle({
    Name = "Ctrl To Sprint (Goes 60% Faster Than Vampires)",
    Callback = function(state)
    local userInputService = game:GetService("UserInputService")
local TweenService = game:GetService("TweenService")
local sprinting = false
local normalWalkSpeed = 16  -- Default walk speed
local sprintWalkSpeed = 160  -- Sprint speed
local defaultFOV = 70       -- Default camera FOV
local sprintFOV = 100       -- Camera FOV when sprinting
local tweenTime = 0.5       -- Time for the FOV transition
local inputBeganConnection, inputEndedConnection

-- Function to smoothly change FOV
local function tweenFOV(camera, targetFOV)
    local tweenInfo = TweenInfo.new(tweenTime, Enum.EasingStyle.Sine, Enum.EasingDirection.Out)
    local goal = {FieldOfView = targetFOV}
    local tween = TweenService:Create(camera, tweenInfo, goal)
    tween:Play()
end
        -- Get player and character
        local player = game.Players.LocalPlayer
        local character = player.Character or player.CharacterAdded:Wait()
        local humanoid = character:WaitForChild("Humanoid")
        local camera = workspace.CurrentCamera

        if state then
            -- Start handling sprinting
            inputBeganConnection = userInputService.InputBegan:Connect(function(input, gameProcessedEvent)
                if not gameProcessedEvent and (input.KeyCode == Enum.KeyCode.LeftControl or input.KeyCode == Enum.KeyCode.RightControl) then
                    humanoid.WalkSpeed = sprintWalkSpeed
                    tweenFOV(camera, sprintFOV)  -- Smooth FOV increase
                    sprinting = true
                end
            end)

            inputEndedConnection = userInputService.InputEnded:Connect(function(input)
                if input.KeyCode == Enum.KeyCode.LeftControl or input.KeyCode == Enum.KeyCode.RightControl then
                    humanoid.WalkSpeed = normalWalkSpeed
                    tweenFOV(camera, defaultFOV)  -- Smooth FOV reset
                    sprinting = false
                end
            end)
        else
            -- Stop handling sprinting
            if inputBeganConnection then
                inputBeganConnection:Disconnect()
                inputBeganConnection = nil
            end
            if inputEndedConnection then
                inputEndedConnection:Disconnect()
                inputEndedConnection = nil
            end

            -- Reset walk speed and FOV if player is sprinting
            if sprinting then
                humanoid.WalkSpeed = normalWalkSpeed
                tweenFOV(camera, defaultFOV)  -- Smooth FOV reset
                sprinting = false
            end
        end
    end
})

-- Handle infinite jump functionality
game:GetService("UserInputService").JumpRequest:Connect(function()
    if infiniteJumpEnabled then
        local humanoid = localPlayer.Character and localPlayer.Character:FindFirstChildOfClass("Humanoid")
        if humanoid then
            humanoid:ChangeState(Enum.HumanoidStateType.Physics)
            humanoid:ChangeState(Enum.HumanoidStateType.Seated)
            humanoid:ChangeState(Enum.HumanoidStateType.GettingUp)
            humanoid:ChangeState(Enum.HumanoidStateType.Jumping)
        end
    end
end)



local function resetTabs(tab)
    for _, button in ipairs(tab:GetChildren()) do
        if button:IsA("GuiButton") then
            button:Destroy()
        end
    end
end

local walkSpeedSlider = MainTab:CreateSlider({
    Name = "WalkSpeed",
    Range = {16, 500},
    Increment = 1,
    Suffix = "Speed",
    CurrentValue = 16,
    Callback = function(Value)
        game.Players.LocalPlayer.Character.Humanoid.WalkSpeed = Value
        handleSpeedChange(Value)
    end
})

local jumpPowerSlider = MainTab:CreateSlider({
    Name = "JumpPower",
    Range = {50, 550},
    Increment = 1,
    Suffix = "JumpPower",
    CurrentValue = 50,
    Callback = function(Value)
        game.Players.LocalPlayer.Character.Humanoid.JumpPower = Value
    end
})

MainTab:CreateButton({
    Name = "Unalive Yourself (Normal)",
    Callback = function()
    game.Players.LocalPlayer.Character.Humanoid.MaxHealth = 0
    game.Players.LocalPlayer.Character.Humanoid.Health = 0
    end
})

MainTab:CreateButton({
    Name = "Unalive Yourself (Bypasses God-Mode)",
    Callback = function()
game.Players.LocalPlayer.Character.OtherScripts.DiedEffects.Shatter:FireServer()
    end
})

MainTab:CreateButton({
    Name = "Reset Speed & JumpPower",
    Callback = function()
        game.Players.LocalPlayer.Character.Humanoid.WalkSpeed = 16
        game.Players.LocalPlayer.Character.Humanoid.JumpPower = 50
        walkSpeedSlider:Set(16)
        jumpPowerSlider:Set(50)
    end
})

MainTab:CreateSection("Extra")

local antiKickConnection

MainTab:CreateToggle({
    Name = "Idle-Mode",
    Callback = function(state)
        if state then
            -- Activate Anti-Kick
            antiKickConnection = game:GetService("Players").LocalPlayer.Idled:Connect(function()
                local VirtualUser = game:GetService("VirtualUser")
                VirtualUser:Button2Down(Vector2.new(0, 0), workspace.CurrentCamera.CFrame)
                wait(1)
                VirtualUser:Button2Up(Vector2.new(0, 0), workspace.CurrentCamera.CFrame)
            end)
        else
            -- Deactivate Anti-Kick
            if antiKickConnection then
                antiKickConnection:Disconnect()
                antiKickConnection = nil
            end
        end
    end
})

MainTab:CreateParagraph({Title = "", Content = "Idle-Mode: Prevents You From Being Kicked Off The Game After 20 Minutes."}) 

MainTab:CreateButton({
    Name = "Infinite yield",
    Callback = function()
        loadstring(game:HttpGet('https://raw.githubusercontent.com/EdgeIY/infiniteyield/master/source'))()
        end
})

MainTab:CreateButton({
    Name = "Dex Explorer",
    Callback = function()
        loadstring(game:HttpGet("https://pastebin.com/raw/Dq7x5HqJ"))()
        end
})

MainTab:CreateToggle({
    Name = "Spy Private Messages",
    Default = false,
    Callback = function(value)
        enabled = value --chat "/spy" to toggle!
        spyOnMyself = true --if true will check your messages too
        public = false --if true will chat the logs publicly (fun, risky)
        publicItalics = true --if true will use /me to stand out
        privateProperties = { --customize private logs
	Color = Color3.fromRGB(0,255,255); 
	Font = Enum.Font.SourceSansBold;
	TextSize = 20;
}

        local StarterGui = game:GetService("StarterGui")
        local Players = game:GetService("Players")
        local player = Players.LocalPlayer or Players:GetPropertyChangedSignal("LocalPlayer"):Wait() or Players.LocalPlayer
        local saymsg = game:GetService("ReplicatedStorage"):WaitForChild("DefaultChatSystemChatEvents"):WaitForChild("SayMessageRequest")
        local getmsg = game:GetService("ReplicatedStorage"):WaitForChild("DefaultChatSystemChatEvents"):WaitForChild("OnMessageDoneFiltering")
        local instance = (_G.chatSpyInstance or 0) + 1
        _G.chatSpyInstance = instance

        local function onChatted(p, msg)
            if _G.chatSpyInstance == instance then
                if p == player and msg:lower():sub(1, 4) == "/spy" then
                    enabled = not enabled
                    wait(0.3)
                    privateProperties.Text = "[SPY " .. (enabled and "EN" or "DIS") .. "ABLED]"
                    StarterGui:SetCore("ChatMakeSystemMessage", privateProperties)
                elseif enabled and (spyOnMyself == true or p ~= player) then
                    msg = msg:gsub("[\n\r]", ''):gsub("\t", ' '):gsub("[ ]+", ' ')
                    local hidden = true
                    local conn = getmsg.OnClientEvent:Connect(function(packet, channel)
                        if packet.SpeakerUserId == p.UserId and packet.Message == msg:sub(#msg - #packet.Message + 1) and (channel == "All" or (channel == "Team" and public == false and Players[packet.FromSpeaker].Team == player.Team)) then
                            hidden = false
                        end
                    end)
                    wait(1)
                    conn:Disconnect()
                    if hidden and enabled then
                        if public then
                            saymsg:FireServer((publicItalics and "/me " or '') .. "[SPY] [" .. p.Name .. "]: " .. msg, "All")
                        else
                            privateProperties.Text = "[SPY] [" .. p.Name .. "]: " .. msg
                            StarterGui:SetCore("ChatMakeSystemMessage", privateProperties)
                        end
                    end
                end
            end
        end

        for _, p in ipairs(Players:GetPlayers()) do
            p.Chatted:Connect(function(msg) onChatted(p, msg) end)
        end
        Players.PlayerAdded:Connect(function(p)
            p.Chatted:Connect(function(msg) onChatted(p, msg) end)
        end)
        privateProperties.Text = "[SPY " .. (enabled and "EN" or "DIS") .. "ABLED]"
        StarterGui:SetCore("ChatMakeSystemMessage", privateProperties)
        if not player.PlayerGui:FindFirstChild("Chat") then wait(3) end
        local chatFrame = player.PlayerGui.Chat.Frame
        chatFrame.ChatChannelParentFrame.Visible = true
        chatFrame.ChatBarParentFrame.Position = chatFrame.ChatChannelParentFrame.Position + UDim2.new(0, 0, chatFrame.ChatChannelParentFrame.Size.Y.Scale, chatFrame.ChatChannelParentFrame.Size.Y.Offset)
    end
})

MainTab:CreateSection("Protection")

local staffProtectionToggle = false
local staffProtectionConnection
local localPlayer = game.Players.LocalPlayer

-- Main toggle for staff protection
MainTab:CreateToggle({
    Name = "Leave If Admin Joins",
    CurrentValue = false,
    Callback = function(state)
        staffProtectionToggle = state

        local targetUserIDs = {
            [508919854] = true,
            [1663428852] = true,
            [915981954] = true,
            [2424185565] = true,
            [987726614] = true,
            [311815303] = true,
            [2000642250] = true,
            [2005042816] = true,
            [1355582012] = true,
            [1905910897] = true,
            [3214278278] = true,
            [4800203581] = true,
            [3745285961] = true,
            [4248433865] = true,
            [3925365453] = true,
            [1583324135] = true,
            [1465646886] = true
        }

        local function kickPlayerIfNeeded(joinedPlayer)
            if targetUserIDs[joinedPlayer.UserId] then
                localPlayer:Kick("An Admin From This Game Has Joined Your Server, You Have Been Kicked For Your Safety.")
            end
        end

        if state then
            -- Check existing players
            for _, existingPlayer in ipairs(game.Players:GetPlayers()) do
                kickPlayerIfNeeded(existingPlayer)
            end

            -- Check new players
            staffProtectionConnection = game.Players.PlayerAdded:Connect(kickPlayerIfNeeded)
        else
            if staffProtectionConnection then
                staffProtectionConnection:Disconnect()
                staffProtectionConnection = nil
            end
        end
    end
})

MainTab:CreateParagraph({Title = "", Content = "Kick If Admin Joins: Automatically Kicks You If Admin Joins."})

local staffProtectionToggle = false
local staffProtectionConnection
local localPlayer = game.Players.LocalPlayer

-- Main toggle for staff protection
MainTab:CreateToggle({
    Name = "Leave If Admin Joins (OPTIONAL)",
    CurrentValue = false,
    Callback = function(state)
        staffProtectionToggle = state

        local targetUserIDs = {
            [508919854] = true,
            [1663428852] = true,
            [915981954] = true,
            [2424185565] = true,
            [987726614] = true,
            [311815303] = true,
            [2000642250] = true,
            [2005042816] = true,
            [1355582012] = true,
            [1905910897] = true,
            [3214278278] = true,
            [4800203581] = true,
            [3745285961] = true,
            [4248433865] = true,
            [3925365453] = true,
            [1583324135] = true,
            [1465646886] = true
        }

        local function handlePlayer(joinedPlayer)
            if targetUserIDs[joinedPlayer.UserId] then
                -- Show a notification with Yes and No buttons
                Rayfield:Notify({
                    Title = "Admin Joined Alert!",
                    Content = "An Admin has joined your game. Do you want to leave?",
                    Duration = 3600,
                    Image = nil,
                    Actions = {
                        Yes = {
                            Name = "Yes",
                            Callback = function()
                                localPlayer:Kick("An Admin From This Game Has Joined Your Server, You Choose To Be Kicked For Your Safety.")
                            end
                        },
                        No = {
                            Name = "No",
                            Callback = function()
                            end
                        }
                    }
                })
            end
        end

        if state then
            -- Check existing players
            for _, existingPlayer in ipairs(game.Players:GetPlayers()) do
                handlePlayer(existingPlayer)
            end

            -- Check new players
            staffProtectionConnection = game.Players.PlayerAdded:Connect(handlePlayer)
        else
            if staffProtectionConnection then
                staffProtectionConnection:Disconnect()
                staffProtectionConnection = nil
            end
        end
    end
})

MainTab:CreateParagraph({Title = "", Content = "Kick If Admin Joins 2: Alerts You And Gives Option To Leave If Admin Joins."})

ESTab:CreateSection("ESP")

local espToggle = false
local espConnection

ESTab:CreateToggle({
    Name = "ESP Everyone",
    CurrentValue = false,
    Callback = function(state)
        local Players = game:GetService("Players")
        local player = Players.LocalPlayer

        espToggle = state

        if espToggle then
            local RunService = game:GetService("RunService")
            local Camera = game:GetService("Workspace").CurrentCamera
            local LocalPlayer = Players.LocalPlayer

            local function createESP(player)
                local character = player.Character
                if character and character:FindFirstChild("HumanoidRootPart") then
                    local highlight = Instance.new("Highlight", character)
                    highlight.Name = "ESP"
                    highlight.Adornee = character
                    highlight.FillTransparency = 0.5
                    highlight.OutlineTransparency = 0
                    highlight.FillColor = Color3.fromRGB(255, 94, 19) -- Orange-reddish color
                    highlight.OutlineColor = Color3.fromRGB(0, 0, 0) -- Black outline

                    -- Create name tag
                    local head = character:FindFirstChild("Head")
                    if head then
                        local billboardGui = Instance.new("BillboardGui", head)
                        billboardGui.Name = "NameTag"
                        billboardGui.AlwaysOnTop = true
                        billboardGui.Size = UDim2.new(0, 200, 0, 50)
                        billboardGui.StudsOffset = Vector3.new(0, 2, 0)

                        local textLabel = Instance.new("TextLabel", billboardGui)
                        textLabel.Text = player.Name
                        textLabel.Font = Enum.Font.SourceSansBold
                        textLabel.TextSize = 14
                        textLabel.TextColor3 = Color3.fromRGB(255, 255, 255) -- White
                        textLabel.BackgroundTransparency = 1
                        textLabel.Size = UDim2.new(1, 0, 1, 0)
                    end
                end
            end

            local function updateESP()
                for _, player in pairs(Players:GetPlayers()) do
                    if player ~= LocalPlayer and player.Character and player.Character:FindFirstChild("Humanoid") then
                        local esp = player.Character:FindFirstChild("ESP")
                        if not esp then
                            createESP(player)
                        end
                    end
                end
            end

            espConnection = RunService.Heartbeat:Connect(updateESP)
        else
            if espConnection then
                espConnection:Disconnect()
            end

            -- Remove all ESP elements
            for _, player in pairs(Players:GetPlayers()) do
                if player.Character then
                    local espOutline = player.Character:FindFirstChild("ESP")
                    local nameTag = player.Character:FindFirstChild("Head"):FindFirstChild("NameTag")
                    if espOutline then
                        espOutline:Destroy()
                    end
                    if nameTag then
                        nameTag:Destroy()
                    end
                end
            end
        end
    end
})

local espToggle = false
local espConnection

ESTab:CreateToggle({
    Name = "ESP By Specie",
    CurrentValue = false,
    Callback = function(state)
        local Players = game:GetService("Players")
        local player = Players.LocalPlayer

        espToggle = state

        if espToggle then
            local RunService = game:GetService("RunService")
            local Camera = game:GetService("Workspace").CurrentCamera
            local LocalPlayer = Players.LocalPlayer

            local speciesColors = {
                WITCH = Color3.fromRGB(128, 0, 128),       -- Purple
                WEREWOLF = Color3.fromRGB(255, 255, 0),    -- Yellow
                VAMPIRE = Color3.fromRGB(255, 0, 0),       -- Red
                SIPHONER = Color3.fromRGB(255,105,180),    -- Pink
                CHAMELEON = Color3.fromRGB(0, 255, 0),     -- Green
                PHOENIX = Color3.fromRGB(255, 165, 0),     -- Orange
                ORIGINAL = Color3.fromRGB(139, 0, 0),      -- Dark Red
                DEMON = Color3.fromRGB(75, 0, 130),        -- Dark Purple
                HYBRID = Color3.fromRGB(0, 255, 255),      -- Cyan
                HERETIC = Color3.fromRGB(219, 112, 147),   -- Mix of pink and purple
                MORTAL = Color3.fromRGB(255, 255, 255),    --  White
            }

            local function createESP(player, species, color)
                local character = player.Character
                if character and character:FindFirstChild("HumanoidRootPart") then
                    local highlight = Instance.new("Highlight", character)
                    highlight.Name = "ESP"
                    highlight.Adornee = character
                    highlight.FillTransparency = 0.5
                    highlight.OutlineTransparency = 0
                    highlight.FillColor = color
                    highlight.OutlineColor = Color3.fromRGB(0, 0, 0) -- Black outline

                    -- Create name tag
                    local head = character:FindFirstChild("Head")
                    if head then
                        local billboardGui = Instance.new("BillboardGui", head)
                        billboardGui.Name = "NameTag"
                        billboardGui.AlwaysOnTop = true
                        billboardGui.Size = UDim2.new(0, 200, 0, 50)
                        billboardGui.StudsOffset = Vector3.new(0, 2, 0)

                        local textLabel = Instance.new("TextLabel", billboardGui)
                        textLabel.Text = species
                        textLabel.Font = Enum.Font.SourceSansBold
                        textLabel.TextSize = 14
                        textLabel.TextColor3 = Color3.fromRGB(255, 255, 255) -- White
                        textLabel.BackgroundTransparency = 1
                        textLabel.Size = UDim2.new(1, 0, 1, 0)
                    end
                end
            end

            local function updateESP()
                for _, player in pairs(Players:GetPlayers()) do
                    if player ~= LocalPlayer and player.Character and player.Character:FindFirstChild("Humanoid") then
                        local overhead = player.Character:FindFirstChild("Head"):FindFirstChild("OverHead")
                        if overhead then
                            local speciesLabel = overhead:FindFirstChild("Species")
                            if speciesLabel and speciesLabel:IsA("TextLabel") then
                                local species = speciesLabel.Text
                                local color = speciesColors[species]
                                if color then
                                    local esp = player.Character:FindFirstChild("ESP")
                                    if esp then
                                        -- Update existing ESP color and text
                                        esp.FillColor = color
                                    else
                                        createESP(player, species, color)
                                    end
                                    
                                    local nameTag = player.Character:FindFirstChild("Head"):FindFirstChild("NameTag")
                                    if nameTag and nameTag:FindFirstChild("TextLabel") then
                                        nameTag.TextLabel.Text = species
                                        nameTag.TextLabel.TextColor3 = color
                                    end
                                end
                            end
                        end
                    end
                end
            end

            espConnection = RunService.Heartbeat:Connect(updateESP)
        else
            if espConnection then
                espConnection:Disconnect()
            end

            -- Remove all ESP elements
            for _, player in pairs(Players:GetPlayers()) do
                if player.Character then
                    local espOutline = player.Character:FindFirstChild("ESP")
                    local nameTag = player.Character:FindFirstChild("Head"):FindFirstChild("NameTag")
                    if espOutline then
                        espOutline:Destroy()
                    end
                    if nameTag then
                        nameTag:Destroy()
                    end
                end
            end
        end
    end
})

local espToggle = false
local espConnection

ESTab:CreateToggle({
    Name = "ESP Friends",
    CurrentValue = false,
    Callback = function(state)
        local Players = game:GetService("Players")
        local LocalPlayer = Players.LocalPlayer

        espToggle = state

        if espToggle then
            local RunService = game:GetService("RunService")

            local function createESPForFriend(friend)
                local character = friend.Character
                if character and character:FindFirstChild("HumanoidRootPart") then
                    -- Highlight
                    local highlight = Instance.new("Highlight", character)
                    highlight.Name = "ESP"
                    highlight.Adornee = character
                    highlight.FillTransparency = 0.5
                    highlight.OutlineTransparency = 0
                    highlight.FillColor = Color3.fromRGB(0, 255, 0) -- Green
                    highlight.OutlineColor = Color3.fromRGB(0, 0, 0) -- Black outline

                    -- Name Tag
                    local head = character:FindFirstChild("Head")
                    if head then
                        local billboardGui = Instance.new("BillboardGui", head)
                        billboardGui.Name = "NameTag"
                        billboardGui.AlwaysOnTop = true
                        billboardGui.Size = UDim2.new(0, 200, 0, 50)
                        billboardGui.StudsOffset = Vector3.new(0, 2, 0)

                        local textLabel = Instance.new("TextLabel", billboardGui)
                        textLabel.Text = friend.Name
                        textLabel.Font = Enum.Font.SourceSansBold
                        textLabel.TextSize = 14
                        textLabel.TextColor3 = Color3.fromRGB(255, 255, 255) -- White
                        textLabel.BackgroundTransparency = 1
                        textLabel.Size = UDim2.new(1, 0, 1, 0)
                    end
                end
            end

            local function updateESP()
                for _, player in ipairs(Players:GetPlayers()) do
                    if player ~= LocalPlayer and LocalPlayer:IsFriendsWith(player.UserId) and player.Character and player.Character:FindFirstChild("Humanoid") then
                        local esp = player.Character:FindFirstChild("ESP")
                        local nameTag = player.Character:FindFirstChild("Head"):FindFirstChild("NameTag")
                        if not esp then
                            createESPForFriend(player)
                        end
                    end
                end
            end

            espConnection = RunService.Heartbeat:Connect(updateESP)
        else
            if espConnection then
                espConnection:Disconnect()
            end

            -- Remove all ESP elements from friends
            for _, player in ipairs(Players:GetPlayers()) do
                if player ~= LocalPlayer and LocalPlayer:IsFriendsWith(player.UserId) and player.Character then
                    local espOutline = player.Character:FindFirstChild("ESP")
                    local nameTag = player.Character:FindFirstChild("Head"):FindFirstChild("NameTag")
                    if espOutline then
                        espOutline:Destroy()
                    end
                    if nameTag then
                        nameTag:Destroy()
                    end
                end
            end
        end
    end
})

SurviTab:CreateParagraph({Title = "Note", Content = "All Species Unlocked By This Tab Will Disappear From Inventory Once You Leave The Game!"})

SurviTab:CreateSection("Shuffle Species")

-- Show a notification
Rayfield:Notify({
    Title = "Success!",
    Content = "Welcome!",
    Duration = 6.5,
    Image = nil,
    Actions = {
        Ignore = {
            Name = "Close",
            Callback = function() end
        }
    }
})

-- Shuffle Species Section
SurviTab:CreateButton({
    Name = "Get All Shuffle Species",
    Callback = function()
        local player = game:GetService("Players").LocalPlayer
        local speciesFolder = player:WaitForChild("OwnedSpecies")
        local speciesList = {"Witch", "Werewolf", "Vampire", "Siphoner", "Chameleon"}

        for _, speciesName in ipairs(speciesList) do
            local species = speciesFolder:FindFirstChild(speciesName)
            if species and species:IsA("BoolValue") then
                species.Value = true
            end
        end
    end
})

SurviTab:CreateButton({
    Name = "Remove All Shuffle Species",
    Callback = function()
        local player = game:GetService("Players").LocalPlayer
        local speciesFolder = player:WaitForChild("OwnedSpecies")
        local speciesList = {"Witch", "Werewolf", "Vampire", "Siphoner", "Chameleon"}

        for _, speciesName in ipairs(speciesList) do
            local species = speciesFolder:FindFirstChild(speciesName)
            if species and species:IsA("BoolValue") then
                species.Value = false
            end
        end
    end
})

local shuffleSpecies = {"Witch", "Werewolf", "Vampire", "Siphoner", "Chameleon"}
for _, speciesName in ipairs(shuffleSpecies) do
    SurviTab:CreateButton({
        Name = "Unlock " .. speciesName,
        Callback = function()
            local player = game:GetService("Players").LocalPlayer
            local speciesFolder = player:WaitForChild("OwnedSpecies")
            local species = speciesFolder:FindFirstChild(speciesName)
            if species and species:IsA("BoolValue") then
                species.Value = true
            end
        end
    })
end

-- Gamepass & Other Species Section
SurviTab:CreateSection("Gamepass & Other Species")

SurviTab:CreateButton({
    Name = "Get All Gamepass & Other Species",
    Callback = function()
        local player = game:GetService("Players").LocalPlayer
        local speciesFolder = player:WaitForChild("OwnedSpecies")
        local speciesList = {"Phoenix", "Original", "Demon"}

        for _, speciesName in ipairs(speciesList) do
            local species = speciesFolder:FindFirstChild(speciesName)
            if species and species:IsA("BoolValue") then
                species.Value = true
            end
        end
    end
})

SurviTab:CreateButton({
    Name = "Remove All Gamepass & Other Species",
    Callback = function()
        local player = game:GetService("Players").LocalPlayer
        local speciesFolder = player:WaitForChild("OwnedSpecies")
        local speciesList = {"Phoenix", "Original"}

        for _, speciesName in ipairs(speciesList) do
            local species = speciesFolder:FindFirstChild(speciesName)
            if species and species:IsA("BoolValue") then
                species.Value = false
            end
        end
    end
})

local gamepassSpecies = {"Phoenix", "Original"}
for _, speciesName in ipairs(gamepassSpecies) do
    SurviTab:CreateButton({
        Name = "Unlock " .. speciesName,
        Callback = function()
            local player = game:GetService("Players").LocalPlayer
            local speciesFolder = player:WaitForChild("OwnedSpecies")
            local species = speciesFolder:FindFirstChild(speciesName)
            if species and species:IsA("BoolValue") then
                species.Value = true
            end
        end
    })
end

-- Unreleased Species Section
SurviTab:CreateSection("Unreleased Species")

SurviTab:CreateButton({
    Name = "Unequip Selected Specie",
    Callback = function()
        local player = game:GetService("Players").LocalPlayer
        local speciesDataFolder = player:WaitForChild("SpeciesData")
        local species = speciesDataFolder:FindFirstChild("Species")

        if species and species:IsA("StringValue") then
            species.Value = "ThisSpecieDoesNotExist"
        end
    end
})

local unreleasedSpecies = {
    {Name = "Demon", Value = "Demon"},
    {Name = "Angel", Value = "Angel"},
    {Name = "Siren", Value = "Siren"},
    {Name = "Kitsune", Value = "Harvest Witch"},
    {Name = "Kanima", Value = "Original Hybrid"},
    {Name = "Hunter", Value = "Hunter"},
    {Name = "Hellhound", Value = "Dark Siphoner"},
    {Name = "Banshee", Value = "Banshee"}
}

for _, species in ipairs(unreleasedSpecies) do
    SurviTab:CreateButton({
        Name = "Equip " .. species.Name,
        Callback = function()
            local player = game:GetService("Players").LocalPlayer
            local speciesDataFolder = player:WaitForChild("SpeciesData")
            local speciesValue = speciesDataFolder:FindFirstChild("Species")

            if speciesValue and speciesValue:IsA("StringValue") then
                speciesValue.Value = species.Value
            end
        end
    })
end

ExtrTab:CreateSection("Main TPS")

ExtrTab:CreateButton({
    Name = "Teleport To Dark Spawn",
    Callback = function()
        local mapFolder = workspace:FindFirstChild("Map")
        local spawnRoom = mapFolder and mapFolder:FindFirstChild("SpawnRoom")
        local player = game.Players.LocalPlayer

        if spawnRoom then
            -- Filter only SpawnLocations within SpawnRoom
            local spawnLocations = {}
            for _, child in ipairs(spawnRoom:GetChildren()) do
                if child:IsA("SpawnLocation") then
                    table.insert(spawnLocations, child)
                end
            end

            -- Check if there are any SpawnLocations and teleport the player
            if #spawnLocations > 0 and player.Character then
                local randomSpawn = spawnLocations[math.random(1, #spawnLocations)]
                
                -- Teleport above the SpawnLocation
                local abovePosition = randomSpawn.CFrame * CFrame.new(0, 5, 0) -- Adjust Y offset as needed
                player.Character:SetPrimaryPartCFrame(abovePosition)
            end
        end
    end
})

ExtrTab:CreateButton({
    Name = "Teleport To City Spawn",
    Callback = function()
local player = game.Players.LocalPlayer
local targetPosition = Vector3.new(-240.2942657470703, 9.570392608642578, -146.6822509765625)

player.Character.HumanoidRootPart.CFrame = CFrame.new(targetPosition)
    end
})

ExtrTab:CreateSection("City TPS")

ExtrTab:CreateButton({
    Name = "Teleport To MysticGrill",
    Callback = function()
local player = game.Players.LocalPlayer
local targetPosition = Vector3.new(-191.7035675048828, 10.64358901977539, 185.60047912597656)

player.Character.HumanoidRootPart.CFrame = CFrame.new(targetPosition)
    end
})

ExtrTab:CreateButton({
    Name = "Teleport To Mai's Store",
    Callback = function()
local player = game.Players.LocalPlayer
local targetPosition = Vector3.new(-268.9493564453125, 9.028172492980957, 199.57943725585938)

player.Character.HumanoidRootPart.CFrame = CFrame.new(targetPosition)
    end
})

ExtrTab:CreateButton({
    Name = "Teleport To Library",
    Callback = function()
local player = game.Players.LocalPlayer
local targetPosition = Vector3.new(-236.8478240966797, 9.37256908416748, 353.8052062988281)

player.Character.HumanoidRootPart.CFrame = CFrame.new(targetPosition)
    end
})

ExtrTab:CreateButton({
    Name = "Teleport Inside Tomb",
    Callback = function()
        local targetPosition = Vector3.new(-294.5696, -7.6207, 122.6822)
        local player = game.Players.LocalPlayer

        if player.Character and player.Character.PrimaryPart then
            player.Character:SetPrimaryPartCFrame(CFrame.new(targetPosition))
        end
    end
})

ExtrTab:CreateButton({
    Name = "Teleport Outside Tomb",
    Callback = function()
local player = game.Players.LocalPlayer
local targetPosition = Vector3.new(-315.47467041015625, -4.335916042327881, 123.9755859375)

player.Character.HumanoidRootPart.CFrame = CFrame.new(targetPosition)
    end
})

ExtrTab:CreateButton({
    Name = "Teleport Inside Roof Of Mystic Grill",
    Callback = function()
local player = game.Players.LocalPlayer
local targetPosition = Vector3.new(-165.95254516601562, 25.194843292236328, 184.7862548828125)

player.Character.HumanoidRootPart.CFrame = CFrame.new(targetPosition)
    end
})

ExtrTab:CreateButton({
    Name = "Teleport Inside Roof Of Mai's Store",
    Callback = function()
local player = game.Players.LocalPlayer
local targetPosition = Vector3.new(-272.27288818359375, 22.349416732788086, 199.7329559326172)

player.Character.HumanoidRootPart.CFrame = CFrame.new(targetPosition)
    end
})
ExtrTab:CreateSection("Player TPS")

ExtrTab:CreateButton({
    Name = "Teleport To Random Player",
    Callback = function()
        local players = game:GetService("Players")
        local localPlayer = players.LocalPlayer

        local function teleportToPlayer(player)
            if player and player.Character and player.Character:FindFirstChild("HumanoidRootPart") then
                localPlayer.Character:SetPrimaryPartCFrame(player.Character.HumanoidRootPart.CFrame)
            end
        end

        -- Find players with "OverHead" in their Head part
        local validPlayers = {}
        for _, player in pairs(players:GetPlayers()) do
            if player ~= localPlayer 
                and player.Character 
                and player.Character:FindFirstChild("Head") 
                and player.Character.Head:FindFirstChild("OverHead") then
                table.insert(validPlayers, player)
            end
        end

        -- Teleport to a random valid player
        if #validPlayers > 0 then
            local randomPlayer = validPlayers[math.random(1, #validPlayers)]
            teleportToPlayer(randomPlayer)
        else
            print("No valid players found with OverHead.")
        end
    end
})

-- Teleport to Random Player Button
ExtrTab:CreateButton({
    Name = "Teleport To Random Player (Dark Spawn)",
    Callback = function()
        local players = game:GetService("Players")
        local localPlayer = players.LocalPlayer

        local function teleportToPlayer(player)
            if player and player.Character and player.Character:FindFirstChild("HumanoidRootPart") then
                localPlayer.Character:SetPrimaryPartCFrame(player.Character.HumanoidRootPart.CFrame)
            end
        end

        -- Find players without "OverHead" in their Head part
        local validPlayers = {}
        for _, player in pairs(players:GetPlayers()) do
            if player ~= localPlayer 
                and player.Character 
                and player.Character:FindFirstChild("Head") 
                and not player.Character.Head:FindFirstChild("OverHead") then
                table.insert(validPlayers, player)
            end
        end

        -- Teleport to a random valid player
        if #validPlayers > 0 then
            local randomPlayer = validPlayers[math.random(1, #validPlayers)]
            teleportToPlayer(randomPlayer)
        else
            print("No valid players found without OverHead.")
        end
    end
})

-- Teleport Information Paragraph
ExtrTab:CreateParagraph({Title = "Info", Content = "Teleport To Random Player (Dark Spawn): Teleports You To Players That Are JUST In The Main Spawn Area."})

-- Section for Other
ExtrTab:CreateSection("Other")

-- Click To Teleport Toggle
local clickDetector = Instance.new("ClickDetector") -- Create a click detector
local indicatorActive = false
local teleportConnection

ExtrTab:CreateToggle({
    Name = "Click To Teleport",
    Callback = function(state)
        if state then
            -- Activate click detector
            clickDetector.Parent = workspace

            local player = game.Players.LocalPlayer
            local mouse = player:GetMouse()

            -- Disconnect any existing teleport connection
            if teleportConnection then
                teleportConnection:Disconnect()
            end

            -- Teleport to clicked position
            teleportConnection = mouse.Button1Down:Connect(function()
                player.Character:SetPrimaryPartCFrame(CFrame.new(mouse.Hit.p))
            end)

            indicatorActive = true
        else
            -- Deactivate click detector and disconnect teleport connection
            if clickDetector then
                clickDetector.Parent = nil
            end
            if teleportConnection then
                teleportConnection:Disconnect()
            end
            indicatorActive = false
        end
    end
})

SigmaTab:CreateSection("Useful Stuff")

SigmaTab:CreateToggle({
    Name = "GodMode/Immortality",
    Callback = function(toggleState)
        local player = game:GetService("Players").LocalPlayer
        local diedEffects = player.Character.OtherScripts.DiedEffects
        if toggleState then
            diedEffects.Enabled = false  -- Toggle ON
        else
            diedEffects.Enabled = true   -- Toggle OFF
        end
    end
})

SigmaTab:CreateButton({
    Name = "Open Witch Book/Grimoire",
    Callback = function()
    game:GetService("ReplicatedStorage").Events.Grimoire:FireServer()
    end
})

SigmaTab:CreateParagraph({Title = "Note:", Content = "Must be Witch, Siphoner or Heretic To Use Buttons Below."})

SigmaTab:CreateButton({
    Name = "Self Revive (Must Be On Otherside)",
    Callback = function()
        local ReplicatedStorage = game:GetService("ReplicatedStorage")
        local Players = game:GetService("Players")
        local player = Players.LocalPlayer  -- Get the local player
        local corpseName = player.Name .. " Corpse"  -- Define the corpse name

        -- Look for a model named "MyUsername Corpse" in the workspace
        local corpse = game.Workspace:FindFirstChild(corpseName)
        
        if corpse and corpse:IsA("Model") and corpse:FindFirstChild("HumanoidRootPart") then
            -- Teleport to the corpse's position
            player.Character.HumanoidRootPart.CFrame = corpse.HumanoidRootPart.CFrame

            -- Fire the event only for the local player
            ReplicatedStorage.Events.ResWitch:FireServer(player)
        else
            print("Corpse not found.")
        end
    end
})

SigmaTab:CreateButton({
    Name = "Revive Player (Must Be Dear Dead Body)",
    Callback = function()
        game:GetService("ReplicatedStorage").Events.ResWitch:FireServer()
    end
})

MainTab:CreateInput({
   Name = "Strangle Specific Player",
   CurrentValue = "",
   PlaceholderText = "Username",
   RemoveTextAfterFocusLost = false,
   Flag = "Input1",
   Callback = function(Text)
   -- The function that takes place when the input is changed
   -- The variable (Text) is a string for the value in the text box
   end,
})

SigmaTab:CreateButton({
    Name = "Create A LightBall That Follows You",
    Callback = function()
for _, player in ipairs(game.Players:GetPlayers()) do
    local character = player.Character
    if character and player.Name ~= game.Players.LocalPlayer.Name then
        game:GetService("ReplicatedStorage").Events.Post:FireServer(character)
    end
end
    end
})

SigmaTab:CreateButton({
    Name = "Cover Your Skin In Vervain",
    Callback = function()
for _, player in ipairs(game.Players:GetPlayers()) do
    local character = player.Character
    if character and player.Name ~= game.Players.LocalPlayer.Name then
        game:GetService("ReplicatedStorage").Events.Cutis:FireServer(character)
    end
end
    end
})

SigmaTab:CreateButton({
    Name = "Turn Invisible For A Few Seconds",
    Callback = function()
game.ReplicatedStorage.Events.Invisique:FireServer()
    end
})

SigmaTab:CreateButton({
    Name = "Regenerate Blood",
    Callback = function()
        for i = 1, 10 do
    game:GetService("ReplicatedStorage").Events.BloodBag:FireServer()
end
    end
})

SigmaTab:CreateParagraph({Title = "Note:", Content = "Must be Vampire, Original, Heretic Or Hybrid To Use Buttons Below."})

SigmaTab:CreateButton({
    Name = "Remove Fire From Your Body",
    Callback = function()
    game:GetService("ReplicatedStorage").Events.Extinguo:FireServer()
    end
})

SigmaTab:CreateButton({
    Name = "Turn Humanity Off",
    Callback = function()
game.Players.LocalPlayer.Character.Abilities.Vampire.NoHumanity.Remote:FireServer()
    end
})

-- GameTab Section
GameTab:CreateSection("ServerSide & Trolls")

GameTab:CreateButton({
    Name = "Generate Angels NPC In Mystic Grill",
    Callback = function()
for _, player in ipairs(game.Players:GetPlayers()) do
    local character = player.Character
    if character and player.Name ~= game.Players.LocalPlayer.Name then
        game:GetService("ReplicatedStorage").Events.FriendSpawner:FireServer(character)
    end
end
    end
})

GameTab:CreateParagraph({Title = "Generate Angels NPC In Mystic Grill:", Content = "Don't spam this button because you might be kicked off the game and it will get very laggy."})

GameTab:CreateButton({
    Name = "Crash/Lag Server (Affects Everyone)",
    Callback = function()
        for i = 1, 1000 do
    game:GetService("ReplicatedStorage").Events.BloodBag:FireServer()
end
    end
})

GameTab:CreateParagraph({Title = "Note:", Content = "Must be Witch, Siphoner or Heretic To Use Buttons Below."})

GameTab:CreateButton({
    Name = "Cast Whole Server In Fire",
    Callback = function()
local ReplicatedStorage = game:GetService("ReplicatedStorage")
local Players = game:GetService("Players")

local function fireEventForAllPlayers()
    for _, player in pairs(Players:GetPlayers()) do
        ReplicatedStorage.Events.Incendia:FireServer(player.Character)
    end
end

fireEventForAllPlayers()
    end
})

GameTab:CreateButton({
    Name = "Cast Whole Server In Fire (MASSIVE)",
    Callback = function()
local ReplicatedStorage = game:GetService("ReplicatedStorage")
local Players = game:GetService("Players")

local function fireEventForAllPlayers()
    for _, player in pairs(Players:GetPlayers()) do
        ReplicatedStorage.Events.Incendia:FireServer(player.Character)
    end
end

for i = 1, 15 do
    fireEventForAllPlayers()
    wait(0.5)
end
    end
})

SigmaTab:CreateButton({
    Name = "Wut",
    Callback = function()
for _, player in ipairs(game.Players:GetPlayers()) do
    local character = player.Character
    if character and player.Name ~= game.Players.LocalPlayer.Name then
        game:GetService("ReplicatedStorage").Events.Motus:FireServer(character)
    end
end
    end
})

GameTab:CreateButton({
    Name = "Make Whole Server To Go To Sleep",
    Callback = function()
local ReplicatedStorage = game:GetService("ReplicatedStorage")
local Players = game:GetService("Players")

local function fireEventForAllPlayers()
    for _, player in pairs(Players:GetPlayers()) do
        ReplicatedStorage.Events.AdSomnum:FireServer(player.Character)
    end
end

fireEventForAllPlayers()
    end
})

GameTab:CreateButton({
    Name = "Crack Whole Servers Bones",
    Callback = function()
for _, player in ipairs(game.Players:GetPlayers()) do
    local character = player.Character
    if character and player.Name ~= game.Players.LocalPlayer.Name then
        game:GetService("ReplicatedStorage").Events.Bone:FireServer(character)
    end
end
    end
})

GameTab:CreateButton({
    Name = "Annoy Whole Server",
    Callback = function()
for _, player in ipairs(game.Players:GetPlayers()) do
    local character = player.Character
    if character and player.Name ~= game.Players.LocalPlayer.Name then
        game:GetService("ReplicatedStorage").Events.Exousia:FireServer(character)
    end
end
    end
})

GameTab:CreateButton({
    Name = "Push Everyone Away From You",
    Callback = function()
for _, player in ipairs(game.Players:GetPlayers()) do
    local character = player.Character
    if character and player.Name ~= game.Players.LocalPlayer.Name then
        game:GetService("ReplicatedStorage").Events.Ictus:FireServer(character)
    end
end
    end
})

GameTab:CreateButton({
    Name = "Immobile Whole Server For A Few Seconds",
    Callback = function()
for _, player in ipairs(game.Players:GetPlayers()) do
    local character = player.Character
    if character and player.Name ~= game.Players.LocalPlayer.Name then
        game:GetService("ReplicatedStorage").Events.Immobilus:FireServer(character)
    end
end
    end
})

GameTab:CreateButton({
    Name = "Trap Every Single User In A Fire Circle",
    Callback = function()
for _, player in ipairs(game.Players:GetPlayers()) do
    local character = player.Character
    if character and player.Name ~= game.Players.LocalPlayer.Name then
        game:GetService("ReplicatedStorage").Events.Incendiamos:FireServer(character)
    end
end
    end
})

GameTab:CreateButton({
    Name = "Strike Whole Server With Lightning",
    Callback = function()
for _, player in ipairs(game.Players:GetPlayers()) do
    local character = player.Character
    if character and player.Name ~= game.Players.LocalPlayer.Name then
        game:GetService("ReplicatedStorage").Events.Lecutio:FireServer(character)
    end
end
    end
})

GameTab:CreateButton({
    Name = "Give Pain To The Whole Server",
    Callback = function()
for _, player in ipairs(game.Players:GetPlayers()) do
    local character = player.Character
    if character and player.Name ~= game.Players.LocalPlayer.Name then
        game:GetService("ReplicatedStorage").Events.Poena:FireServer(character)
    end
end
    end
})

GameTab:CreateButton({
    Name = "Mute The Whole Server For A Few Seconds",
    Callback = function()
for _, player in ipairs(game.Players:GetPlayers()) do
    local character = player.Character
    if character and player.Name ~= game.Players.LocalPlayer.Name then
        game:GetService("ReplicatedStorage").Events.Silencio:FireServer(character)
    end
end
    end
})

GameTab:CreateButton({
    Name = "Strangle The Whole Server",
    Callback = function()
for _, player in ipairs(game.Players:GetPlayers()) do
    local character = player.Character
    if character and player.Name ~= game.Players.LocalPlayer.Name then
        game:GetService("ReplicatedStorage").Events.Strangle:FireServer(character)
    end
end
    end
})

GameTab:CreateParagraph({Title = "Note:", Content = "Must be Vampire, Original, Hybrid Or Heretic To Use Buttons Below."})

GameTab:CreateButton({
    Name = "Give Whole Server Blood",
    Callback = function()
for _, player in ipairs(game.Players:GetPlayers()) do
    local character = player.Character
    if character and player.Name ~= game.Players.LocalPlayer.Name then
        game:GetService("ReplicatedStorage").Events.BloodOffer:FireServer(character)
    end
end
    end
})

GameTab:CreateButton({
    Name = "Compulse Whole Server",
    Callback = function()
for _, player in ipairs(game.Players:GetPlayers()) do
    local character = player.Character
    if character and player.Name ~= game.Players.LocalPlayer.Name then
        game:GetService("ReplicatedStorage").Events.Compulsion:FireServer(character)
    end
end
    end
})

GameTab:CreateButton({
    Name = "Annoy Whole Server",
    Callback = function()
for _, player in ipairs(game.Players:GetPlayers()) do
    local character = player.Character
    if character and player.Name ~= game.Players.LocalPlayer.Name then
        game:GetService("ReplicatedStorage").Events.EyeGorge:FireServer(character)
    end
end
    end
})

GameTab:CreateButton({
    Name = "Feast From Whole Server",
    Callback = function()
for _, player in ipairs(game.Players:GetPlayers()) do
    local character = player.Character
    if character and player.Name ~= game.Players.LocalPlayer.Name then
        game:GetService("ReplicatedStorage").Events.Feed:FireServer(character)
    end
end
    end
})

-- EventTab Sections
EvntTab:CreateSection("Info")
EvntTab:CreateParagraph({Title = "Supernatural Hub Developer", Content = "Castiel Aka Angelus."})
EvntTab:CreateParagraph({Title = "Support & Discord", Content = "https://discord.gg/ZHpN6hAFnu"})
EvntTab:CreateParagraph({Title = "Ban Risk", Content = "75%"})
EvntTab:CreateParagraph({Title = "Exploit Patches", Content = "0!"})
EvntTab:CreateParagraph({Title = "Note From Developer", Content = "Admins Are Watching, Be careful When You Exploit Supernatural!"})
