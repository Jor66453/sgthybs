-- Load Rayfield library
local Rayfield = loadstring(game:HttpGet('https://sirius.menu/rayfield'))()

-- Create the main window
local Window = Rayfield:CreateWindow({
    Name = "SURVIVE THE SLASHER HUB V3",
    LoadingTitle = "Survive The Slasher",
    LoadingSubtitle = "By Jor",
    ConfigurationSaving = {
        Enabled = true,
        FolderName = nil,
        FileName = "SURVIVE THE SLASHER HUB V3"
    },
    Discord = {
        Enabled = true,
        Invite = "H66dDfrS4Q",
        RememberJoins = true 
    },
    KeySystem = true,  -- Enable the key system
    KeySettings = {
        Title = "Key System",
        Subtitle = "Please Enter A Key To Access The Script!",
        Note = "The Key is in the discord",
        FileName = "Key",
        SaveKey = true,
        Key = {"NotterPlayjyfoxHACEKA#GSuslrrvi65753eSlsurvielslasherherKey689786549583"}  -- Set your desired key here
    }
})

-- Create tabs
local MainTab = Window:CreateTab("Main", nil)
local SurviTab = Window:CreateTab("Survivor", nil)
local SlrTab = Window:CreateTab("Slasher", nil)
local ExtrTab = Window:CreateTab("Extra", nil)
local CrdTab = Window:CreateTab("Credits", nil)

-- Show a notification
Rayfield:Notify({
    Title = "Success!",
    Content = "Welcome!",
    Duration = 6.5,
    Image = nil,
    Actions = {
        Ignore = {
            Name = "Dismiss",
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

local staffProtectionToggle = false
local staffProtectionConnection
local localPlayer = game.Players.LocalPlayer

-- Main toggle for staff protection
MainTab:CreateToggle({
    Name = "Staff Protection",
    CurrentValue = false,
    Callback = function(state)
        staffProtectionToggle = state

        local targetUserIDs = {
            [362956857] = true,
            [28724905] = true,
            [707723783] = true,
            [331273890] = true,
            [4791037402] = true,
            [311314197] = true,
            [2241157448] = true,
            [3524879643] = true,
            [862638016] = true,
            [316330352] = true,
            [2977642950] = true,
            [3120444159] = true,
            [3047319330] = true
        }

        local function kickPlayerIfNeeded(joinedPlayer)
            if targetUserIDs[joinedPlayer.UserId] then
                localPlayer:Kick("A Developer/Moderator Of This Game Has Joined Your Server, You Have Been Kicked For Your Safety.")
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

MainTab:CreateButton({
    Name = "Infinite yield",
    Callback = function()
        loadstring(game:HttpGet('https://raw.githubusercontent.com/EdgeIY/infiniteyield/master/source'))()
        end
})

local infjToggle = false
local localPlayer = game.Players.LocalPlayer
local infiniteJumpEnabled = false -- Variable to control infinite jump

local infJump = MainTab:CreateToggle({
    Name = "Infinite Jump",
    Default = false,
    Callback = function(state)
        infjToggle = state
        infiniteJumpEnabled = infjToggle -- Set the infiniteJumpEnabled based on the toggle state
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

local function handleSpeedChange(speed)
    local player = game.Players.LocalPlayer
    local humanoid = player.Character and player.Character:FindFirstChildOfClass("Humanoid")

    if humanoid then
        if speed == 20 or speed == 22.5 then
            resetTabs(SurviTab)
            Rayfield:Notify({
                Title = "Warning",
                Content = "You cannot use Survivor scripts while being slasher!",
                Duration = 5,
                Image = nil
            })
        elseif speed == 16 then
            resetTabs(SlrTab)
            Rayfield:Notify({
                Title = "Warning",
                Content = "You cannot use Slasher scripts while being a survivor!",
                Duration = 5,
                Image = nil
            })
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
    Name = "Unalive Yourself",
    Callback = function()
    game.Players.LocalPlayer.Character.Humanoid.MaxHealth = 0
    game.Players.LocalPlayer.Character.Humanoid.Health = 0
    end
})

MainTab:CreateButton({
    Name = "Reset Speed",
    Callback = function()
        game.Players.LocalPlayer.Character.Humanoid.WalkSpeed = 16
        walkSpeedSlider:Set(16)

        local screenGui = Instance.new("ScreenGui", game.Players.LocalPlayer:WaitForChild("PlayerGui"))
        local textLabel = Instance.new("TextLabel", screenGui)
        textLabel.Size = UDim2.new(0, 400, 0, 100)
        textLabel.Position = UDim2.new(-0.5, 0, 0.1, 0) -- Start off-screen to the left
        textLabel.Text = "Success! Current Speed Has Been Reset"
        textLabel.TextScaled = true
        textLabel.BackgroundTransparency = 1
        textLabel.TextColor3 = Color3.fromRGB(204, 0, 0) -- Lighter, llamative purple
        textLabel.Font = Enum.Font.GothamBlack
        textLabel.TextStrokeTransparency = 0
        textLabel.TextStrokeColor3 = Color3.fromRGB(0, 0, 0) -- Black stroke

        -- Slide-in effect
        local targetPosition = UDim2.new(0.5, -200, 0.1, 0) -- Final centered position
        game:GetService("TweenService"):Create(textLabel, TweenInfo.new(0.5, Enum.EasingStyle.Quad, Enum.EasingDirection.Out), {Position = targetPosition}):Play()

        -- Smooth fade out effect after 4 seconds
        task.delay(4, function()
            for i = 0, 1, 0.05 do
                textLabel.TextTransparency = i
                textLabel.TextStrokeTransparency = i
                task.wait(0.05)
            end
            textLabel:Destroy()
        end)
    end
})

MainTab:CreateButton({
    Name = "Reset JumpPower",
    Callback = function()
        game.Players.LocalPlayer.Character.Humanoid.JumpPower = 50
        jumpPowerSlider:Set(50)

        local screenGui = Instance.new("ScreenGui", game.Players.LocalPlayer:WaitForChild("PlayerGui"))
        local textLabel = Instance.new("TextLabel", screenGui)
        textLabel.Size = UDim2.new(0, 400, 0, 100)
        textLabel.Position = UDim2.new(-0.5, 0, 0.1, 0) -- Start off-screen to the left
        textLabel.Text = "Success! Current JumpPower has been Reset"
        textLabel.TextScaled = true
        textLabel.BackgroundTransparency = 1
        textLabel.TextColor3 = Color3.fromRGB(204, 0, 0) -- Lighter, llamative purple
        textLabel.Font = Enum.Font.GothamBlack
        textLabel.TextStrokeTransparency = 0
        textLabel.TextStrokeColor3 = Color3.fromRGB(0, 0, 0) -- Black stroke

        -- Slide-in effect
        local targetPosition = UDim2.new(0.5, -200, 0.1, 0) -- Final centered position
        game:GetService("TweenService"):Create(textLabel, TweenInfo.new(0.5, Enum.EasingStyle.Quad, Enum.EasingDirection.Out), {Position = targetPosition}):Play()

        -- Smooth fade out effect after 4 seconds
        task.delay(4, function()
            for i = 0, 1, 0.05 do
                textLabel.TextTransparency = i
                textLabel.TextStrokeTransparency = i
                task.wait(0.05)
            end
            textLabel:Destroy()
        end)
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

local espToggle = false
local espConnection

SurviTab:CreateToggle({
    Name = "ESP",
    CurrentValue = false,
    Callback = function(state)
        local Players = game:GetService("Players")
        local player = Players.LocalPlayer

        espToggle = state

        if espToggle then
            local RunService = game:GetService("RunService")
            local Camera = game:GetService("Workspace").CurrentCamera
            local LocalPlayer = Players.LocalPlayer

            local function createBillboard(player)
                local character = player.Character
                if character and character:FindFirstChild("Head") then
                    local head = character.Head
                    local billboard = Instance.new("BillboardGui", head)
                    billboard.Name = "ESP"
                    billboard.Size = UDim2.new(4, 0, 1.5, 0)
                    billboard.Adornee = head
                    billboard.AlwaysOnTop = true
                    billboard.StudsOffset = Vector3.new(0, 2, 0)

                    local nameLabel = Instance.new("TextLabel", billboard)
                    nameLabel.Size = UDim2.new(1, 0, 1, 0)
                    nameLabel.BackgroundTransparency = 0
                    nameLabel.BackgroundColor3 = Color3.fromRGB(64, 64, 64)
                    nameLabel.Text = player.Name
                    nameLabel.TextScaled = true
                    nameLabel.Font = Enum.Font.SourceSans
                    nameLabel.TextColor3 = Color3.new(1, 1, 1)
                    nameLabel.BorderSizePixel = 0
                    nameLabel.TextStrokeTransparency = 0.5
                    nameLabel.TextStrokeColor3 = Color3.new(0, 0, 0)
                    nameLabel.ZIndex = 2

                    local border = Instance.new("Frame", nameLabel)
                    border.Size = UDim2.new(1, 4, 1, 4)
                    border.Position = UDim2.new(0, -2, 0, -2)
                    border.BackgroundColor3 = Color3.fromRGB(169, 169, 169)
                    border.BorderSizePixel = 0
                    border.ZIndex = 1

                    local corner = Instance.new("UICorner", nameLabel)
                    corner.CornerRadius = UDim.new(0, 20)

                    local borderCorner = Instance.new("UICorner", border)
                    borderCorner.CornerRadius = UDim.new(0, 20)
                end
            end

            local function createBox(character)
                local parts = {"Head", "Torso", "Left Arm", "Right Arm", "Left Leg", "Right Leg"}
                for _, partName in pairs(parts) do
                    local part = character:FindFirstChild(partName)
                    if part then
                        local box = Instance.new("BoxHandleAdornment")
                        box.Name = "ESPBox"
                        box.Adornee = part
                        box.Size = part.Size + Vector3.new(0.2, 0.2, 0.2)
                        box.Color3 = Color3.new(1, 1, 1)
                        box.Transparency = 1
                        box.AlwaysOnTop = true
                        box.ZIndex = 10
                        box.Parent = part
                    end
                end
            end

            local function isPartVisible(part)
                local camera = Camera
                local partPos = part.Position
                local _, onScreen = camera:WorldToViewportPoint(partPos)

                if onScreen then
                    local ray = Ray.new(camera.CFrame.Position, (partPos - camera.CFrame.Position).unit * 500)
                    local hitPart = workspace:FindPartOnRay(ray, workspace)
                    return hitPart and hitPart:IsDescendantOf(part.Parent)
                end
                return false
            end

            local function hasSpeedShake(player)
                local backpack = player.Backpack
                local character = player.Character
                return backpack:FindFirstChild("Speed Shake") or character:FindFirstChild("Speed Shake")
            end

            local function updateESP()
                for _, player in pairs(Players:GetPlayers()) do
                    if player ~= LocalPlayer and player.Character and player.Character:FindFirstChild("Humanoid") then
                        local humanoid = player.Character.Humanoid
                        local walkSpeed = humanoid.WalkSpeed
                        local head = player.Character:FindFirstChild("Head")
                        local esp = head and head:FindFirstChild("ESP")

                        if not esp then
                            createBillboard(player)
                            esp = head:FindFirstChild("ESP")
                        end

                        for _, partName in pairs({"Head", "Torso", "Left Arm", "Right Arm", "Left Leg", "Right Leg"}) do
                            local part = player.Character:FindFirstChild(partName)
                            if part and not part:FindFirstChild("ESPBox") then
                                createBox(player.Character)
                            end
                        end

                        local color
                        if hasSpeedShake(player) then
                            if walkSpeed ~= 5 and walkSpeed ~= 16 and walkSpeed ~= 24 and walkSpeed ~= 25 then
                                color = Color3.fromRGB(252, 76, 2) -- Orange
                            else
                                color = Color3.fromRGB(255, 165, 0) -- White
                            end
                        else
                            if walkSpeed == 16 then
                                color = Color3.fromRGB(50, 205, 50) -- Lime Green
                            elseif walkSpeed == 5 then
                                color = Color3.fromRGB(204, 204, 0) -- Darkish Yellow
                            elseif walkSpeed > 16 and walkSpeed ~= 24 and walkSpeed ~= 25 then
                                color = Color3.fromRGB(255, 0, 0) -- Red
                            elseif walkSpeed == 24 then
                                color = Color3.fromRGB(50, 205, 50) -- Orange
                            elseif walkSpeed == 25 then
                                color = Color3.fromRGB(172, 79, 198) -- Purple
                            elseif walkSpeed == 20 or walkSpeed == 22.5 then
                                color = Color3.fromRGB(255, 0, 0) -- Red (if not using Speed Shake)
                            else
                                color = Color3.new(255, 165, 0) -- Default to White
                            end
                        end

                        if color then
                            if esp then
                                esp.TextLabel.TextColor3 = color
                            end
                            for _, partName in pairs({"Head", "Torso", "Left Arm", "Right Arm", "Left Leg", "Right Leg"}) do
                                local part = player.Character:FindFirstChild(partName)
                                local box = part and part:FindFirstChild("ESPBox")
                                if box then
                                    if isPartVisible(part) then
                                        box.Transparency = 1
                                    else
                                        box.Transparency = 0.4
                                        box.Color3 = color
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

            -- Make sure all ESP elements are removed
            for _, player in pairs(Players:GetPlayers()) do
                if player.Character then
                    local head = player.Character:FindFirstChild("Head")
                    if head then
                        local esp = head:FindFirstChild("ESP")
                        if esp then
                            esp:Destroy()
                        end
                    end
                    for _, partName in pairs({"Head", "Torso", "Left Arm", "Right Arm", "Left Leg", "Right Leg"}) do
                        local part = player.Character:FindFirstChild(partName)
                        if part then
                            local box = part:FindFirstChild("ESPBox")
                            if box then
                                box:Destroy()
                            end
                        end
                    end
                end
            end
        end
    end
})

local seslreek = SurviTab:CreateButton({
    Name = "Spectate Slasher",
    Callback = function()
        local Players = game:GetService("Players")
        local player = Players.LocalPlayer
        local camera = workspace.CurrentCamera
        local tweenService = game:GetService("TweenService")

        -- Function to create and display a styled text label
        local function createStyledTextLabel(text, color)
            local screenGui = Instance.new("ScreenGui", player:WaitForChild("PlayerGui"))
            local textLabel = Instance.new("TextLabel", screenGui)
            textLabel.Size = UDim2.new(0, 600, 0, 100)
            textLabel.Position = UDim2.new(0.5, -300, -0.1, 0)  -- Start above the screen
            textLabel.Text = text
            textLabel.TextScaled = true
            textLabel.BackgroundTransparency = 1
            textLabel.TextColor3 = color
            textLabel.Font = Enum.Font.GothamBlack
            textLabel.TextStrokeTransparency = 0
            textLabel.TextStrokeColor3 = Color3.fromRGB(0, 0, 0) -- Black stroke

            -- Slide-in effect
            local targetPosition = UDim2.new(0.5, -300, 0.5, -50) -- Centered position
            local tweenInfo = TweenInfo.new(0.5, Enum.EasingStyle.Quad, Enum.EasingDirection.Out)
            local slideInTween = tweenService:Create(textLabel, tweenInfo, {Position = targetPosition})
            slideInTween:Play()

            -- Smooth fade-out after a delay
            slideInTween.Completed:Connect(function()
                task.delay(4, function()
                    local fadeOutTween = tweenService:Create(textLabel, TweenInfo.new(1, Enum.EasingStyle.Quad, Enum.EasingDirection.In), {TextTransparency = 1, TextStrokeTransparency = 1})
                    fadeOutTween:Play()
                    fadeOutTween.Completed:Connect(function()
                        textLabel:Destroy()
                    end)
                end)
            end)

            return textLabel
        end

        -- Function to check if there are players with specified walk speeds
        local function hasPlayersWithWalkSpeed(speeds)
            for _, p in ipairs(Players:GetPlayers()) do
                if p.Character and p.Character:FindFirstChild("Humanoid") then
                    local humanoid = p.Character:FindFirstChild("Humanoid")
                    if table.find(speeds, humanoid.WalkSpeed) then
                        return true
                    end
                end
            end
            return false
        end

        -- Function to move the camera to a player with specified walk speeds
        local function moveToPlayerWithWalkSpeed(speeds)
            for _, p in ipairs(Players:GetPlayers()) do
                if p.Character and p.Character:FindFirstChild("Humanoid") then
                    local humanoid = p.Character:FindFirstChild("Humanoid")
                    if table.find(speeds, humanoid.WalkSpeed) then
                        camera.CameraSubject = p.Character
                        camera.CameraType = Enum.CameraType.Attach
                        return
                    end
                end
            end
        end

        -- Function to create a GUI button with a background
        local function createButton()
            local screenGui = Instance.new("ScreenGui", player:WaitForChild("PlayerGui"))
            local button = Instance.new("TextButton", screenGui)
            local uiCorner = Instance.new("UICorner", button)
            
            button.Size = UDim2.new(0, 250, 0, 75) -- Increased size
            button.Position = UDim2.new(0.5, -125, 0.8, 0) -- Adjusted position for new size
            button.Text = "EXIT"
            button.BackgroundColor3 = Color3.fromRGB(255, 0, 0) -- Red background
            button.TextColor3 = Color3.fromRGB(255, 255, 255) -- White text
            button.Font = Enum.Font.GothamBlack
            button.TextScaled = true
            button.TextStrokeTransparency = 0
            button.TextStrokeColor3 = Color3.fromRGB(0, 0, 0) -- Black stroke

            -- Add rounded corners with a radius of 0.15
            uiCorner.CornerRadius = UDim.new(0, 15)

            -- Function to move the camera back to normal view
            button.MouseButton1Click:Connect(function()
                camera.CameraSubject = player.Character.HumanoidRootPart
                camera.CameraType = Enum.CameraType.Custom
                screenGui:Destroy()
            end)
        end

        -- Check for players with the specified walk speeds before proceeding
        if hasPlayersWithWalkSpeed({20, 22.5}) then
            -- Call the function to move the camera
            moveToPlayerWithWalkSpeed({20, 22.5})
            
            -- Call the function to create the button
            createButton()
        else
            -- Show warning message if no player with specified walk speed is found
            createStyledTextLabel("Warning: Please wait until Slasher is selected or spawns to use this.", Color3.fromRGB(255, 174, 66))
        end
    end,
})

SurviTab:CreateButton({
    Name = "Heal Me",
    Callback = function()
        local tweenService = game:GetService("TweenService")
        local player = game.Players.LocalPlayer
        local character = player.Character or player.CharacterAdded:Wait()
        local humanoid = character:WaitForChild("Humanoid")
        local humanoidRootPart = character:WaitForChild("HumanoidRootPart")
        local backpack = player:WaitForChild("Backpack")
        local originalPosition = humanoidRootPart.Position
        local targetPlayer = nil
        local Players = game:GetService("Players")
        local teleporting = true

        -- Create a styled text label
        local function createStyledTextLabel(text, color)
            local screenGui = Instance.new("ScreenGui", player:WaitForChild("PlayerGui"))
            local textLabel = Instance.new("TextLabel", screenGui)
            textLabel.Size = UDim2.new(0, 600, 0, 100)
            textLabel.Position = UDim2.new(0.5, -300, -0.1, 0)  -- Start above the screen
            textLabel.Text = text
            textLabel.TextScaled = true
            textLabel.BackgroundTransparency = 1
            textLabel.TextColor3 = color
            textLabel.Font = Enum.Font.GothamBlack
            textLabel.TextStrokeTransparency = 0
            textLabel.TextStrokeColor3 = Color3.fromRGB(0, 0, 0) -- Black stroke

            -- Slide-in effect
            local targetPosition = UDim2.new(0.5, -300, 0.5, -50) -- Centered position
            local tweenInfo = TweenInfo.new(0.5, Enum.EasingStyle.Quad, Enum.EasingDirection.Out)
            local slideInTween = tweenService:Create(textLabel, tweenInfo, {Position = targetPosition})
            slideInTween:Play()

            -- Smooth fade-out after a delay
            slideInTween.Completed:Connect(function()
                task.delay(4, function()
                    local fadeOutTween = tweenService:Create(textLabel, TweenInfo.new(1, Enum.EasingStyle.Quad, Enum.EasingDirection.In), {TextTransparency = 1, TextStrokeTransparency = 1})
                    fadeOutTween:Play()
                    fadeOutTween.Completed:Connect(function()
                        textLabel:Destroy()
                    end)
                end)
            end)

            return textLabel
        end

        -- Check for DisplayGun and Speed Shake
        local function checkConditions()
            if character:FindFirstChild("DisplayGun") then
                createStyledTextLabel("Warning: You can't use this in the lobby.", Color3.fromRGB(255, 174, 66))
                return false
            elseif (humanoid.WalkSpeed == 20 or humanoid.WalkSpeed == 22.5) and 
                (not backpack:FindFirstChild("Speed Shake") and not character:FindFirstChild("Speed Shake")) then
                createStyledTextLabel("Warning: You can't use this while you are slasher.", Color3.fromRGB(255, 174, 66))
                return false
            elseif humanoid.WalkSpeed == 16 then
                createStyledTextLabel("Warning: You must be injured by the slasher to use this.", Color3.fromRGB(255, 174, 66))
                return false
            end
            return true
        end

        -- Function to find a target player with WalkSpeed of 20 or 22.5
        local function findTargetPlayer()
            for _, otherPlayer in pairs(Players:GetPlayers()) do
                if otherPlayer ~= player and otherPlayer.Character then
                    local otherHumanoid = otherPlayer.Character:FindFirstChildOfClass("Humanoid")
                    if otherHumanoid and (otherHumanoid.WalkSpeed == 20 or otherHumanoid.WalkSpeed == 22.5) then
                        return otherPlayer
                    end
                end
            end
            return nil
        end

        -- Function to teleport to the target player
        local function teleportToTarget()
            if targetPlayer and targetPlayer.Character and targetPlayer.Character:FindFirstChild("HumanoidRootPart") then
                humanoidRootPart.CFrame = targetPlayer.Character.HumanoidRootPart.CFrame
            end
        end

        -- Function to spam teleport and handle ForceField check
        local function spamTeleport()
            targetPlayer = findTargetPlayer()

            -- Spam teleport to player with WalkSpeed 20 or 22.5
            while teleporting do
                if targetPlayer then
                    teleportToTarget()
                else
                    targetPlayer = findTargetPlayer()
                end
                
                -- Check for ForceField
                if character:FindFirstChild("ForceField") then
                    teleporting = false
                    humanoidRootPart.CFrame = CFrame.new(originalPosition) -- Teleport back to original position
                    break
                end
                wait(0.05)
            end
        end

        -- Check the conditions before teleporting
        if checkConditions() then
            spamTeleport()
        end
    end
})

local toggle = false
local connection
local PseudoAnchor
local CanInvis = true
local IsInvisible = false
local FakeCharacter
local Part
local Player = game.Players.LocalPlayer
local RealCharacter = Player.Character or Player.CharacterAdded:Wait()
local humanoid = RealCharacter:WaitForChild("Humanoid")
local Transparency = true
local NoClip = false
local LightPart -- Part for light to indicate player's position
local FaceDecal
local tweenService = game:GetService("TweenService")
local originalTransparency = {}

SurviTab:CreateToggle({
    Name = "Invisible (Disables Game Interaction)",
    CurrentValue = false,
    Flag = "SlasherToggle",
    Callback = function(Value)
        toggle = Value

        local function createStyledTextLabel(text, color)
            local screenGui = Instance.new("ScreenGui", Player:WaitForChild("PlayerGui"))
            local textLabel = Instance.new("TextLabel", screenGui)
            textLabel.Size = UDim2.new(0, 600, 0, 100)
            textLabel.Position = UDim2.new(0.5, -300, 0.1, 0)
            textLabel.Text = text
            textLabel.TextScaled = true
            textLabel.BackgroundTransparency = 1
            textLabel.TextColor3 = color
            textLabel.Font = Enum.Font.GothamBlack
            textLabel.TextStrokeTransparency = 0
            textLabel.TextStrokeColor3 = Color3.fromRGB(0, 0, 0)

            local targetPosition = UDim2.new(0.5, -300, 0.5, -50)
            local tweenInfo = TweenInfo.new(0.5, Enum.EasingStyle.Quad, Enum.EasingDirection.Out)
            local slideInTween = tweenService:Create(textLabel, tweenInfo, {Position = targetPosition})
            slideInTween:Play()

            slideInTween.Completed:Connect(function()
                task.delay(4, function()
                    local fadeOutTween = tweenService:Create(textLabel, TweenInfo.new(1, Enum.EasingStyle.Quad, Enum.EasingDirection.In), {TextTransparency = 1, TextStrokeTransparency = 1})
                    fadeOutTween:Play()
                    fadeOutTween.Completed:Connect(function()
                        textLabel:Destroy()
                    end)
                end)
            end)
        end

        -- Ensure invisibility can still proceed
        if RealCharacter:FindFirstChild("DisplayGun") then
            createStyledTextLabel("Warning: You can't become invisible in the lobby, wait until game starts.", Color3.fromRGB(255, 174, 66))
            return
        elseif humanoid.WalkSpeed == 20 or humanoid.WalkSpeed == 22.5 then
            createStyledTextLabel("Warning: You can't become invisible while being slasher, it will glitch.", Color3.fromRGB(255, 174, 66))
            return
        end

        -- Initialize the fake character
        local function setupFakeCharacter()
            RealCharacter.Archivable = true
            FakeCharacter = RealCharacter:Clone()

            Part = Instance.new("Part", workspace)
            Part.Anchored = true
            Part.Size = Vector3.new(200, 1, 200)
            Part.CFrame = CFrame.new(0, -500, 0)
            Part.CanCollide = true

            LightPart = Instance.new("Part", workspace)
            LightPart.Anchored = true
            LightPart.CanCollide = false
            LightPart.Shape = Enum.PartType.Ball
            LightPart.Size = Vector3.new(5, 5, 5)
            LightPart.CFrame = RealCharacter.HumanoidRootPart.CFrame
            local light = Instance.new("PointLight", LightPart)
            light.Brightness = 2
            light.Range = 10

            FakeCharacter.Parent = workspace
            FakeCharacter.HumanoidRootPart.CFrame = Part.CFrame * CFrame.new(0, 5, 0)

            if Transparency then
                for _, v in pairs(FakeCharacter:GetDescendants()) do
                    if v:IsA("BasePart") then
                        v.Transparency = 1
                    elseif v:IsA("Decal") and v.Name == "face" then
                        v.Transparency = 1 -- Hide face decal
                    end
                end
            end

            for _, v in pairs(RealCharacter:GetChildren()) do
                if v:IsA("LocalScript") then
                    local clone = v:Clone()
                    clone.Disabled = true
                    clone.Parent = FakeCharacter
                end
            end

            PseudoAnchor = FakeCharacter.HumanoidRootPart
        end

        -- Toggle invisibility
        local function Invisible(enable)
            if enable and not IsInvisible then
                local StoredCF = RealCharacter.HumanoidRootPart.CFrame
                RealCharacter.HumanoidRootPart.CFrame = FakeCharacter.HumanoidRootPart.CFrame
                FakeCharacter.HumanoidRootPart.CFrame = StoredCF
                RealCharacter.Humanoid:UnequipTools()
                Player.Character = FakeCharacter
                workspace.CurrentCamera.CameraSubject = FakeCharacter.Humanoid
                PseudoAnchor = RealCharacter.HumanoidRootPart

                for _, v in pairs(FakeCharacter:GetChildren()) do
                    if v:IsA("LocalScript") then
                        v.Disabled = false
                    end
                end

                -- Hide real character's face decal
                for _, v in pairs(RealCharacter:GetChildren()) do
                    if v:IsA("Decal") and v.Name == "face" then
                        FaceDecal = v
                        v.Transparency = 1
                    end
                end

                IsInvisible = true
            elseif not enable and IsInvisible then
                local StoredCF = FakeCharacter.HumanoidRootPart.CFrame
                FakeCharacter.HumanoidRootPart.CFrame = RealCharacter.HumanoidRootPart.CFrame
                RealCharacter.HumanoidRootPart.CFrame = StoredCF

                FakeCharacter.Humanoid:UnequipTools()
                Player.Character = RealCharacter
                workspace.CurrentCamera.CameraSubject = RealCharacter.Humanoid
                PseudoAnchor = FakeCharacter.HumanoidRootPart

                for _, v in pairs(FakeCharacter:GetChildren()) do
                    if v:IsA("LocalScript") then
                        v.Disabled = true
                    end
                end

                -- Restore real character's face decal
                if FaceDecal then
                    FaceDecal.Transparency = 0
                end

                IsInvisible = false
            end
        end

        if toggle then
            if not FakeCharacter then
                setupFakeCharacter()
            end
            Invisible(true) -- Enable invisibility

            connection = game:GetService("RunService").RenderStepped:Connect(function()
                if NoClip and FakeCharacter then
                    FakeCharacter.Humanoid:ChangeState(11) -- NoClip mode
                end
                if PseudoAnchor then
                    PseudoAnchor.CFrame = Part.CFrame * CFrame.new(0, 5, 0) -- Keep the fake character in place
                end

                if LightPart and RealCharacter and RealCharacter:FindFirstChild("HumanoidRootPart") then
                    LightPart.CFrame = RealCharacter.HumanoidRootPart.CFrame * CFrame.new(0, 5, 0)
                end
            end)
        else
            if IsInvisible then
                Invisible(false) -- Disable invisibility
            end
            if FakeCharacter then
                FakeCharacter:Destroy()
                FakeCharacter = nil
            end
            if Part then
                Part:Destroy()
                Part = nil
            end
            if LightPart then
                LightPart:Destroy()
                LightPart = nil
            end
            if connection then
                connection:Disconnect()
                connection = nil
            end
        end
    end
})

local tweenService = game:GetService("TweenService")
local Players = game:GetService("Players")
local player = Players.LocalPlayer

-- Beast Mode Button
local slrbstm = SlrTab:CreateToggle({
    Name = "Beast Mode (Fun)",
    CurrentValue = false,
    Flag = "RedVisionToggle",
    Callback = function(Value)
        local humanoid = player.Character and player.Character:FindFirstChildOfClass("Humanoid")
        
        if not humanoid then return end
        
        local function createStyledTextLabel(text, color)
            local playerGui = player:WaitForChild("PlayerGui")
            -- Only remove the custom ScreenGui created for the label, not all ScreenGuis
            local previousGui = playerGui:FindFirstChild("CustomTextLabelGui")
            if previousGui then
                previousGui:Destroy()
            end

            local screenGui = Instance.new("ScreenGui")
            screenGui.Name = "CustomTextLabelGui"
            screenGui.Parent = playerGui

            local textLabel = Instance.new("TextLabel", screenGui)
            textLabel.Size = UDim2.new(0, 600, 0, 100)
            textLabel.Position = UDim2.new(0.5, -300, 0.1, 0)
            textLabel.Text = text
            textLabel.TextScaled = true
            textLabel.BackgroundTransparency = 1
            textLabel.TextColor3 = color
            textLabel.Font = Enum.Font.GothamBlack
            textLabel.TextStrokeTransparency = 0
            textLabel.TextStrokeColor3 = Color3.fromRGB(0, 0, 0)

            local targetPosition = UDim2.new(0.5, -300, 0.5, -50)
            local tweenInfo = TweenInfo.new(0.5, Enum.EasingStyle.Quad, Enum.EasingDirection.Out)
            local slideInTween = tweenService:Create(textLabel, tweenInfo, {Position = targetPosition})
            slideInTween:Play()

            slideInTween.Completed:Connect(function()
                task.delay(4, function()
                    local fadeOutTween = tweenService:Create(textLabel, TweenInfo.new(1, Enum.EasingStyle.Quad, Enum.EasingDirection.In), {TextTransparency = 1, TextStrokeTransparency = 1})
                    fadeOutTween:Play()
                    fadeOutTween.Completed:Connect(function()
                        screenGui:Destroy()
                    end)
                end)
            end)
        end

        if player.Character:FindFirstChild("DisplayGun") then
            createStyledTextLabel("Warning: You can't use this while being in the lobby.", Color3.fromRGB(255, 174, 66))
            return
        elseif humanoid.WalkSpeed == 16 then
            createStyledTextLabel("Warning: You can't use this while being a survivor.", Color3.fromRGB(255, 174, 66))
            return
        end
        
        -- Beast Mode functionality
        if Value then
            humanoid.MaxHealth = math.huge
            humanoid.Health = math.huge
            humanoid.WalkSpeed = 30

            local redOverlay = Instance.new("ScreenGui", player.PlayerGui)
            redOverlay.Name = "RedOverlay"
            redOverlay.IgnoreGuiInset = true

            local redFrame = Instance.new("Frame", redOverlay)
            redFrame.Size = UDim2.new(1, 0, 1, 0)
            redFrame.BackgroundColor3 = Color3.fromRGB(255, 0, 0)
            redFrame.BackgroundTransparency = 0.7

        else
            humanoid.MaxHealth = 100
            humanoid.Health = 100

            if player.PlayerGui:FindFirstChild("RedOverlay") then
                player.PlayerGui.RedOverlay:Destroy()
            end
        end
    end
})

-- Kill All Button
SlrTab:CreateButton({
    Name = "Kill All",
    Callback = function()
        local player = game.Players.LocalPlayer
        local humanoid = player.Character and player.Character:FindFirstChildOfClass("Humanoid")

        -- Function to create a styled text label
        local function createStyledTextLabel(text, color)
            local playerGui = player:WaitForChild("PlayerGui")
            local previousGui = playerGui:FindFirstChild("CustomTextLabelGui")
            if previousGui then
                previousGui:Destroy()
            end

            local screenGui = Instance.new("ScreenGui")
            screenGui.Name = "CustomTextLabelGui"
            screenGui.Parent = playerGui

            local textLabel = Instance.new("TextLabel")
            textLabel.Size = UDim2.new(0, 600, 0, 100)
            textLabel.Position = UDim2.new(0.5, -300, 0.1, 0)
            textLabel.Text = text
            textLabel.TextScaled = true
            textLabel.BackgroundTransparency = 1
            textLabel.TextColor3 = color
            textLabel.Font = Enum.Font.GothamBlack
            textLabel.TextStrokeTransparency = 0
            textLabel.TextStrokeColor3 = Color3.fromRGB(0, 0, 0)
            textLabel.Parent = screenGui

            local tweenService = game:GetService("TweenService")
            local targetPosition = UDim2.new(0.5, -300, 0.5, -50)
            local tweenInfo = TweenInfo.new(0.5, Enum.EasingStyle.Quad, Enum.EasingDirection.Out)
            local slideInTween = tweenService:Create(textLabel, tweenInfo, {Position = targetPosition})
            slideInTween:Play()

            slideInTween.Completed:Connect(function()
                task.delay(4, function()
                    local fadeOutTween = tweenService:Create(textLabel, TweenInfo.new(1, Enum.EasingStyle.Quad, Enum.EasingDirection.In), {TextTransparency = 1, TextStrokeTransparency = 1})
                    fadeOutTween:Play()
                    fadeOutTween.Completed:Connect(function()
                        screenGui:Destroy()
                    end)
                end)
            end)
        end

        -- Function to teleport to a player but slightly behind
        local function teleportToPlayer(targetPlayer)
            if targetPlayer and targetPlayer.Character and targetPlayer.Character:FindFirstChild("HumanoidRootPart") then
                local targetHRP = targetPlayer.Character:FindFirstChild("HumanoidRootPart")
                if targetHRP then
                    -- Teleport slightly behind the target player (offset by -3 studs on the look vector)
                    local offsetCFrame = targetHRP.CFrame * CFrame.new(0, 0, 3)
                    player.Character.HumanoidRootPart.CFrame = offsetCFrame
                end
            end
        end

        -- Function to start teleporting to other players
        local function startTeleporting()
            local teleporting = true
            while teleporting do
                local validPlayers = {}
                for _, otherPlayer in pairs(game.Players:GetPlayers()) do
                    if otherPlayer ~= player and otherPlayer.Character and not otherPlayer.Character:FindFirstChild("DisplayGun") then
                        table.insert(validPlayers, otherPlayer)
                    end
                end

                for _, otherPlayer in pairs(validPlayers) do
                    teleportToPlayer(otherPlayer)
                    wait(1)  -- Slight delay between teleports
                end
            end
        end

        -- Check for warnings and start process
        if player.Character:FindFirstChild("DisplayGun") then
            createStyledTextLabel("Warning: You can't use this while being in the lobby.", Color3.fromRGB(255, 174, 66))
            return
        elseif humanoid and humanoid.WalkSpeed == 16 then
            createStyledTextLabel("Warning: You can't use this while being a survivor.", Color3.fromRGB(255, 174, 66))
            return
        end

        -- Toggle teleporting
        if _G.teleporting then
            _G.teleporting = false
        else
            createStyledTextLabel("Starting.. Try attacking with the knife by yourself if the autoclicker fails.", Color3.fromRGB(255, 0, 0))
            task.delay(2, function()
                _G.teleporting = true
                startTeleporting()
            end)
        end
    end
})

local players = game:GetService("Players")
local camera = workspace.CurrentCamera
local userInputService = game:GetService("UserInputService")

local controlsEnabled = true -- Global flag to manage camera controls

local seslreek = SlrTab:CreateButton({
    Name = "Seek Players",
    Callback = function()
        local Players = game:GetService("Players")
        local player = Players.LocalPlayer
        local camera = workspace.CurrentCamera
        local tweenService = game:GetService("TweenService")

        -- Function to create and display a styled text label
        local function createStyledTextLabel(text, color)
            local screenGui = Instance.new("ScreenGui", player:WaitForChild("PlayerGui"))
            local textLabel = Instance.new("TextLabel", screenGui)
            textLabel.Size = UDim2.new(0, 600, 0, 100)
            textLabel.Position = UDim2.new(0.5, -300, -0.1, 0)  -- Start above the screen
            textLabel.Text = text
            textLabel.TextScaled = true
            textLabel.BackgroundTransparency = 1
            textLabel.TextColor3 = color
            textLabel.Font = Enum.Font.GothamBlack
            textLabel.TextStrokeTransparency = 0
            textLabel.TextStrokeColor3 = Color3.fromRGB(0, 0, 0) -- Black stroke

            -- Slide-in effect
            local targetPosition = UDim2.new(0.5, -300, 0.5, -50) -- Centered position
            local tweenInfo = TweenInfo.new(0.5, Enum.EasingStyle.Quad, Enum.EasingDirection.Out)
            local slideInTween = tweenService:Create(textLabel, tweenInfo, {Position = targetPosition})
            slideInTween:Play()

            -- Smooth fade-out after a delay
            slideInTween.Completed:Connect(function()
                task.delay(4, function()
                    local fadeOutTween = tweenService:Create(textLabel, TweenInfo.new(1, Enum.EasingStyle.Quad, Enum.EasingDirection.In), {TextTransparency = 1, TextStrokeTransparency = 1})
                    fadeOutTween:Play()
                    fadeOutTween.Completed:Connect(function()
                        textLabel:Destroy()
                    end)
                end)
            end)

            return textLabel
        end

        -- Function to check for player warnings
        local function checkPlayerWarnings()
            for _, p in ipairs(Players:GetPlayers()) do
                if p.Character and p.Character:FindFirstChild("Humanoid") then
                    local humanoid = p.Character:FindFirstChild("Humanoid")

                    -- Warning if DisplayGun is found
                    if p.Character:FindFirstChild("DisplayGun") then
                        createStyledTextLabel("Warning: You can't use this in the lobby.", Color3.fromRGB(255, 174, 66))
                        return false
                    end

                    -- Warning if walk speed is 16
                    if humanoid.WalkSpeed == 16 then
                        createStyledTextLabel("Warning: You can't use this while being a survivor.", Color3.fromRGB(255, 174, 66))
                        return false
                    end
                end
            end
            return true
        end

        -- Function to check if there are players with specific walk speeds
        local function hasPlayersWithWalkSpeed(speeds)
            for _, p in ipairs(Players:GetPlayers()) do
                if p.Character and p.Character:FindFirstChild("Humanoid") then
                    local humanoid = p.Character:FindFirstChild("Humanoid")
                    if table.find(speeds, humanoid.WalkSpeed) then
                        return true
                    end
                end
            end
            return false
        end

        -- Function to move the camera to a player with specified walk speeds
        local function moveToPlayerWithWalkSpeed(speeds)
            for _, p in ipairs(Players:GetPlayers()) do
                if p.Character and p.Character:FindFirstChild("Humanoid") then
                    local humanoid = p.Character:FindFirstChild("Humanoid")
                    if table.find(speeds, humanoid.WalkSpeed) then
                        camera.CameraSubject = p.Character
                        camera.CameraType = Enum.CameraType.Attach
                        return
                    end
                end
            end
        end

        -- Function to create a GUI button with a background
        local function createButton()
            local screenGui = Instance.new("ScreenGui", player:WaitForChild("PlayerGui"))
            local button = Instance.new("TextButton", screenGui)
            local uiCorner = Instance.new("UICorner", button)
            
            button.Size = UDim2.new(0, 250, 0, 75) -- Increased size
            button.Position = UDim2.new(0.5, -125, 0.8, 0) -- Adjusted position for new size
            button.Text = "EXIT"
            button.BackgroundColor3 = Color3.fromRGB(255, 0, 0) -- Red background
            button.TextColor3 = Color3.fromRGB(255, 255, 255) -- White text
            button.Font = Enum.Font.GothamBlack
            button.TextScaled = true
            button.TextStrokeTransparency = 0
            button.TextStrokeColor3 = Color3.fromRGB(0, 0, 0) -- Black stroke

            -- Add rounded corners with a radius of 0.15
            uiCorner.CornerRadius = UDim.new(0, 15)

            -- Function to move the camera back to normal view
            button.MouseButton1Click:Connect(function()
                camera.CameraSubject = player.Character
                camera.CameraType = Enum.CameraType.Custom
                screenGui:Destroy()
            end)
        end

        -- Main Execution
        if checkPlayerWarnings() then
            if hasPlayersWithWalkSpeed({20, 22.5}) then
                moveToPlayerWithWalkSpeed({20, 22.5})
                createButton()
            else
                -- Show warning message if no player with specified walk speed is found
                createStyledTextLabel("Warning: Please wait until Slasher is selected or spawns to use this.", Color3.fromRGB(255, 174, 66))
            end
        end
    end,
})


CrdTab:CreateButton({
    Name = "Creator: Jor.",
    Callback = function()
    end
})

CrdTab:CreateButton({
    Name = "Scripter: WaveAI & Jor.",
    Callback = function()
    end
})

CrdTab:CreateButton({
    Name = "Tester: Jor.",
    Callback = function()
    end
})

CrdTab:CreateButton({
    Name = "GUI Creator/Contributor: shlexware.",
    Callback = function()
    end
})

CrdTab:CreateButton({
    Name = "GUI Type: Rayfield.",
    Callback = function()
    end
})

ExtrTab:CreateButton({
    Name = "Teleport To Lobby",
    Callback = function()
        local player = game.Players.LocalPlayer
        local character = player.Character
        local lobbySpawns = game.Workspace:FindFirstChild("Lobby"):FindFirstChild("LobbySpawns")

        if lobbySpawns and character then
            local spawnPoints = lobbySpawns:GetChildren()
            local randomSpawn = spawnPoints[math.random(1, #spawnPoints)]
            if randomSpawn:IsA("BasePart") then
                character:MoveTo(randomSpawn.Position)
            end
        end
    end
})

ExtrTab:CreateButton({
    Name = "Teleport To Current Map",
    Callback = function()
        local player = game.Players.LocalPlayer
        local character = player.Character
        local survivorSpawns = game.Workspace:FindFirstChild("CurrentMap"):FindFirstChild("SurvivorSpawns")

        if survivorSpawns and character then
            local spawnPoints = survivorSpawns:GetChildren()
            local randomSpawn = spawnPoints[math.random(1, #spawnPoints)]
            if randomSpawn:IsA("BasePart") then
                character:MoveTo(randomSpawn.Position)
            end
        end
    end
})

-- Collect All Coins Button
ExtrTab:CreateButton({
    Name = "Collect All Coins",
    Callback = function()
        local player = game.Players.LocalPlayer
        local character = player.Character
        local hrp = character:WaitForChild("HumanoidRootPart")
        local originalPosition = hrp.CFrame

        local function collectCoins()
            local coinsModel = workspace:FindFirstChild("CurrentMap"):FindFirstChild("Coins")
            if coinsModel then
                local coins = coinsModel:GetChildren()
                while #coins > 0 do
                    for _, coin in pairs(coins) do
                        if coin:IsA("BasePart") and coin.Name == "Coin" then
                            hrp.CFrame = coin.CFrame
                            wait(0.05) -- Adjust delay to control teleport speed
                            coin:Destroy()
                        end
                    end
                    coins = coinsModel:GetChildren()
                end
            end

            hrp.CFrame = originalPosition
        end

        collectCoins()
    end
})


ExtrTab:CreateButton({
    Name = "Teleport To Slasher",
    Callback = function()
        -- Find the player with 20 walk speed
        local targetPlayer = nil
        for _, player in ipairs(game.Players:GetPlayers()) do
            if player.Character and player.Character:FindFirstChild("Humanoid") then
                local humanoid = player.Character.Humanoid
                if humanoid.WalkSpeed == 20 then
                    targetPlayer = player
                    break
                end
            end
        end

        -- Teleport the player to the target player
        if targetPlayer then
            local localPlayer = game.Players.LocalPlayer
            if localPlayer.Character and localPlayer.Character:FindFirstChild("HumanoidRootPart") then
                localPlayer.Character.HumanoidRootPart.CFrame = targetPlayer.Character.HumanoidRootPart.CFrame
            end
        else
            warn("Slasher has not been picked yet!.")
        end
    end
})

ExtrTab:CreateButton({
    Name = "Teleport To Random Player",
    Callback = function()
        local players = game:GetService("Players")
        local localPlayer = players.LocalPlayer
        local blacklist = {} -- List to store players to avoid teleporting to
        
        -- Function to check if player is blacklisted
        local function isBlacklisted(player)
            for _, blacklistedPlayer in ipairs(blacklist) do
                if blacklistedPlayer == player then
                    return true
                end
            end
            return false
        end

        -- Function to teleport to a player
        local function teleportToPlayer(player)
            if player and player.Character and player.Character:FindFirstChild("HumanoidRootPart") then
                localPlayer.Character:SetPrimaryPartCFrame(player.Character.HumanoidRootPart.CFrame)
            end
        end

        -- Populate blacklist based on WalkSpeed condition
        for _, player in pairs(players:GetPlayers()) do
            if player ~= localPlayer and player.Character and player.Character:FindFirstChild("Humanoid") then
                local walkSpeed = player.Character.Humanoid.WalkSpeed
                if walkSpeed == 20 or walkSpeed == 22.5 then
                    table.insert(blacklist, player)
                end
            end
        end

        -- Get a list of valid players for teleporting
        local validPlayers = {}
        for _, player in pairs(players:GetPlayers()) do
            if player ~= localPlayer and player.Character and not player.Character:FindFirstChild("DisplayGun") and not isBlacklisted(player) then
                table.insert(validPlayers, player)
            end
        end

        -- Teleport to a random valid player
        if #validPlayers > 0 then
            local randomPlayer = validPlayers[math.random(1, #validPlayers)]
            teleportToPlayer(randomPlayer)
        else
            print("No valid players found.")
        end
    end
})

ExtrTab:CreateButton({
    Name = "Teleport To Random Player (Lobby)",
    Callback = function()
        local players = game:GetService("Players")
        local localPlayer = players.LocalPlayer

        -- Function to teleport to a player
        local function teleportToPlayer(player)
            if player and player.Character and player.Character:FindFirstChild("HumanoidRootPart") then
                localPlayer.Character:SetPrimaryPartCFrame(player.Character.HumanoidRootPart.CFrame)
            end
        end

        -- Get a list of players that have DisplayGun
        local validPlayers = {}
        for _, player in pairs(players:GetPlayers()) do
            if player ~= localPlayer and player.Character and player.Character:FindFirstChild("DisplayGun") then
                table.insert(validPlayers, player)
            end
        end

        -- Teleport to a random valid player
        if #validPlayers > 0 then
            local randomPlayer = validPlayers[math.random(1, #validPlayers)]
            teleportToPlayer(randomPlayer)
        else
            print("No players with DisplayGun found.")
        end
    end
})

local clickDetector = Instance.new("ClickDetector") -- Create a click detector
local indicatorActive = false
local teleportConnection

-- Toggle for activating the click detector and teleportation
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

ExtrTab:CreateButton({
    Name = "Remove Barriers (Map)",
    Callback = function()
        local workspace = game:GetService("Workspace")
        local currentMap = workspace:FindFirstChild("CurrentMap")

        if currentMap then
            local otherFolder = currentMap:FindFirstChild("Other")
            local extraModel = currentMap:FindFirstChild("Extra")
            local invisWalls = nil

            if otherFolder then
                invisWalls = otherFolder:FindFirstChild("InvisWalls")
            end

            if not invisWalls and extraModel and extraModel:IsA("Model") then
                invisWalls = extraModel:FindFirstChild("InvisWalls")
            end

            if invisWalls and invisWalls:IsA("Model") then
                invisWalls:Destroy()
            end
        end
    end
})

ExtrTab:CreateButton({
    Name = "Remove Barriers (Lobby)",
    Callback = function()
        local workspace = game:GetService("Workspace")
        local lobbyFolder = workspace:FindFirstChild("Lobby")

        if lobbyFolder then
            -- Remove the "Invis" model if it exists
            local invisModel = lobbyFolder:FindFirstChild("Invis")
            if invisModel and invisModel:IsA("Model") then
                invisModel:Destroy()
            end

            -- Check for and delete 5 parts named "Part"
            local partsRemoved = 0
            for _, item in ipairs(workspace:GetChildren()) do
                if item:IsA("BasePart") and item.Name == "Part" then
                    item:Destroy()
                    partsRemoved = partsRemoved + 1
                    if partsRemoved >= 5 then
                        break
                    end
                end
            end
        end
    end
})

-- Initial Variables
local SlrTab = {} -- Replace this with your actual SlrTab instance
local player = game.Players.LocalPlayer
local camera = workspace.CurrentCamera
local players = game.Players:GetPlayers()
local currentPlayerIndex = nil
local originalCameraCFrame = camera.CFrame

-- Create a button for spectating
SlrTab:CreateButton({
    Name = "Spectate Player",
    Callback = function()
        -- Function to spectate a player
        local function spectatePlayer(targetPlayer)
            if targetPlayer and targetPlayer.Character then
                camera.CameraSubject = targetPlayer.Character.Humanoid
                camera.CFrame = targetPlayer.Character.HumanoidRootPart.CFrame
                originalCameraCFrame = camera.CFrame
                currentPlayerIndex = table.find(players, targetPlayer)
            end
        end

        -- Find players without 'DisplayGun'
        local eligiblePlayers = {}
        for _, p in pairs(players) do
            if p.Character and p.Character:FindFirstChild("DisplayGun") == nil and p ~= player then
                table.insert(eligiblePlayers, p)
            end
        end

        -- Switch between eligible players
        if #eligiblePlayers > 0 then
            currentPlayerIndex = (currentPlayerIndex or 0) % #eligiblePlayers + 1
            local targetPlayer = eligiblePlayers[currentPlayerIndex]
            spectatePlayer(targetPlayer)
        else
            print("No eligible players found.")
        end
    end
})

-- Handle key press for switching between players
game:GetService("UserInputService").InputBegan:Connect(function(input)
    if input.KeyCode == Enum.KeyCode.E then
        -- Trigger the button callback
        local spectateButton = SlrTab:FindButton("Spectate Player")
        if spectateButton then
            spectateButton:Click()
        end
    end
end)

local players = game:GetService("Players")
local camera = workspace.CurrentCamera
local userInputService = game:GetService("UserInputService")

local currentSpectatedPlayer = nil
local spectateList = {}
local previousSpectatedPlayers = {}

-- Create GUI Elements
local screenGui = Instance.new("ScreenGui", game.Players.LocalPlayer:WaitForChild("PlayerGui"))
local exitButton = Instance.new("TextButton")
local leftArrowButton = Instance.new("TextButton")
local rightArrowButton = Instance.new("TextButton")
local playerLabel = Instance.new("TextLabel")
local spectateLabel = Instance.new("TextLabel")

-- UI Corners for rounded edges
local uiCorner = Instance.new("UICorner")

-- Customize GUI
screenGui.Name = "SpectateGui"
screenGui.ResetOnSpawn = false

-- Exit Button
exitButton.Size = UDim2.new(0, 150, 0, 60)
exitButton.Position = UDim2.new(0.5, -75, 0.85, 0)
exitButton.BackgroundColor3 = Color3.fromRGB(255, 0, 0) -- Red background
exitButton.BorderSizePixel = 0
exitButton.Text = "EXIT"
exitButton.TextColor3 = Color3.fromRGB(255, 255, 255) -- White text
exitButton.Font = Enum.Font.SourceSansBold
exitButton.TextSize = 60
exitButton.Parent = screenGui
local exitCorner = uiCorner:Clone()
exitCorner.CornerRadius = UDim.new(0, 8)
exitCorner.Parent = exitButton

-- Left Arrow Button
leftArrowButton.Size = UDim2.new(0, 50, 0, 50)
leftArrowButton.Position = UDim2.new(0.35, 0, 0.85, 0)
leftArrowButton.BackgroundColor3 = Color3.fromRGB(50, 50, 50) -- Dark gray background
leftArrowButton.BorderSizePixel = 0
leftArrowButton.Text = "< (Q)"
leftArrowButton.TextColor3 = Color3.fromRGB(255, 255, 255) -- White text
leftArrowButton.Font = Enum.Font.SourceSansBold
leftArrowButton.TextSize = 30
leftArrowButton.Parent = screenGui
local leftArrowCorner = uiCorner:Clone()
leftArrowCorner.CornerRadius = UDim.new(0, 8)
leftArrowCorner.Parent = leftArrowButton

-- Right Arrow Button
rightArrowButton.Size = UDim2.new(0, 50, 0, 50)
rightArrowButton.Position = UDim2.new(0.65, 0, 0.85, 0)
rightArrowButton.BackgroundColor3 = Color3.fromRGB(50, 50, 50) -- Dark gray background
rightArrowButton.BorderSizePixel = 0
rightArrowButton.Text = "> (E)"
rightArrowButton.TextColor3 = Color3.fromRGB(255, 255, 255) -- White text
rightArrowButton.Font = Enum.Font.SourceSansBold
rightArrowButton.TextSize = 30
rightArrowButton.Parent = screenGui
local rightArrowCorner = uiCorner:Clone()
rightArrowCorner.CornerRadius = UDim.new(0, 8)
rightArrowCorner.Parent = rightArrowButton

-- Player Name Label
playerLabel.Size = UDim2.new(0, 200, 0, 50)
playerLabel.Position = UDim2.new(0.5, -100, 0.68, 0)
playerLabel.BackgroundColor3 = Color3.fromRGB(50, 50, 50) -- Dark gray background
playerLabel.BorderSizePixel = 0
playerLabel.Text = ""
playerLabel.TextColor3 = Color3.fromRGB(255, 255, 255) -- White text
playerLabel.Font = Enum.Font.SourceSansBold
playerLabel.TextSize = 30
playerLabel.TextWrapped = true
playerLabel.TextYAlignment = Enum.TextYAlignment.Center
playerLabel.Parent = screenGui
local playerLabelCorner = uiCorner:Clone()
playerLabelCorner.CornerRadius = UDim.new(0, 8)
playerLabelCorner.Parent = playerLabel

-- Spectating Label
spectateLabel.Size = UDim2.new(0, 300, 0, 50)
spectateLabel.Position = UDim2.new(0.5, -150, 0.6, 0)
spectateLabel.BackgroundColor3 = Color3.fromRGB(50, 50, 50) -- Dark gray background
spectateLabel.BorderSizePixel = 0
spectateLabel.Text = "Currently seeking:"
spectateLabel.TextColor3 = Color3.fromRGB(255, 0, 0) -- Red text
spectateLabel.Font = Enum.Font.SourceSansBold
spectateLabel.TextSize = 30
spectateLabel.Parent = screenGui
local spectateLabelCorner = uiCorner:Clone()
spectateLabelCorner.CornerRadius = UDim.new(0, 8)
spectateLabelCorner.Parent = spectateLabel

-- Function to get a list of players who don't have "DisplayGun"
local function getAvailablePlayers()
    local availablePlayers = {}
    for _, player in pairs(players:GetPlayers()) do
        if player ~= players.LocalPlayer and player.Character and 
           not player.Character:FindFirstChild("DisplayGun") and
           not table.find(previousSpectatedPlayers, player) then
            table.insert(availablePlayers, player)
        end
    end
    return availablePlayers
end

-- Function to get a random player from the list
local function getRandomPlayer()
    local availablePlayers = getAvailablePlayers()
    if #availablePlayers > 0 then
        return availablePlayers[math.random(1, #availablePlayers)]
    end
    return nil
end

-- Function to spectate a player
local function spectatePlayer(player)
    if player and player.Character and player.Character:FindFirstChild("HumanoidRootPart") then
        camera.CameraSubject = player.Character.HumanoidRootPart
        playerLabel.Text = player.Name
        currentSpectatedPlayer = player

        -- Add to previously spectated players
        if not table.find(previousSpectatedPlayers, player) then
            table.insert(previousSpectatedPlayers, player)
        end
    end
end

-- Function to reset the camera
local function resetCamera()
    camera.CameraSubject = players.LocalPlayer.Character.HumanoidRootPart
    playerLabel.Text = ""
    currentSpectatedPlayer = nil
end

-- Function to handle text size adjustments
local function adjustTextSize()
    local playerName = playerLabel.Text
    local maxLength = 20 -- Adjust as needed
    local textSize = 30
    
    if #playerName > maxLength then
        textSize = math.max(14, 30 - (#playerName - maxLength)) -- Minimum text size of 14
    end

    playerLabel.TextSize = textSize
end

-- Button Actions
exitButton.MouseButton1Click:Connect(function()
    resetCamera()
    screenGui:Destroy() -- Destroy the entire GUI on exit
end)

leftArrowButton.MouseButton1Click:Connect(function()
    local prevPlayerIndex = table.find(previousSpectatedPlayers, currentSpectatedPlayer) - 1
    if prevPlayerIndex < 1 then
        prevPlayerIndex = #previousSpectatedPlayers
    end
    local prevPlayer = previousSpectatedPlayers[prevPlayerIndex]
    if prevPlayer then
        spectatePlayer(prevPlayer)
        adjustTextSize()
    end
end)

rightArrowButton.MouseButton1Click:Connect(function()
    local newPlayer = getRandomPlayer()
    if newPlayer then
        spectatePlayer(newPlayer)
        adjustTextSize()
    end
end)

-- Keyboard Actions (Q and E for arrows)
userInputService.InputBegan:Connect(function(input)
    if input.KeyCode == Enum.KeyCode.Q then
        local prevPlayerIndex = table.find(previousSpectatedPlayers, currentSpectatedPlayer) - 1
        if prevPlayerIndex < 1 then
            prevPlayerIndex = #previousSpectatedPlayers
        end
        local prevPlayer = previousSpectatedPlayers[prevPlayerIndex]
        if prevPlayer then
            spectatePlayer(prevPlayer)
            adjustTextSize()
        end
    elseif input.KeyCode == Enum.KeyCode.E then
        local newPlayer = getRandomPlayer()
        if newPlayer then
            spectatePlayer(newPlayer)
            adjustTextSize()
        end
    elseif input.KeyCode == Enum.KeyCode.Escape then
        resetCamera()
        screenGui:Destroy() -- Destroy the GUI on escape
    end
end)

-- Start by spectating a random player
local randomPlayer = getRandomPlayer()
if randomPlayer then
    spectatePlayer(randomPlayer)
    adjustTextSize()
end

