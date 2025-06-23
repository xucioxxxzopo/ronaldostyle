local player = game.Players.LocalPlayer

local function cleanup()
    -- Remove any existing cloned ability buttons
    for _, buttonName in ipairs({"FakeHeelFlickButton", "JinpachiTurnButton", "MetavisionButton", "SatoruShotButton"}) do
        local button = player.PlayerGui.InGameUI.Bottom.Abilities:FindFirstChild(buttonName)
        if button then
            button:Destroy()
        end
    end
    
    -- Remove any existing GUI text
    if player.PlayerGui:FindFirstChild("AbilityTextGui") then
        player.PlayerGui.AbilityTextGui:Destroy()
    end
end

local function initializeAbility()
    local character = player.Character or player.CharacterAdded:Wait()
    local humanoid = character:WaitForChild("Humanoid")
    local humanoidRootPart = character:WaitForChild("HumanoidRootPart")

    -- The animation for the dribble/drive move
    local animation1 = Instance.new("Animation")
    animation1.AnimationId = "rbxassetid://111257275604578"  -- Replace with your desired animation ID
    local animationTrack1 = humanoid:LoadAnimation(animation1)

    local animation2 = Instance.new("Animation")
    animation2.AnimationId = "rbxassetid://111257275604578"  -- Replace with your desired animation ID
    local animationTrack2 = humanoid:LoadAnimation(animation2)

    local animation3 = Instance.new("Animation")
    animation3.AnimationId = "rbxassetid://111257275604578"  -- Replace with your desired animation ID
    local animationTrack3 = humanoid:LoadAnimation(animation3)

    -- Sound for the move
    local sound1 = Instance.new("Sound")
    sound1.SoundId = "rbxassetid://17704386866"  -- Change the sound ID
    sound1.Parent = humanoidRootPart  -- You can attach the sound to the character's root part

    local sound2 = Instance.new("Sound")
    sound2.SoundId = "rbxassetid://5835032207"  -- Change the sound ID
    sound2.Parent = humanoidRootPart  -- You can attach the sound to the character's root part

    local sound3 = Instance.new("Sound")
    sound3.SoundId = "rbxassetid://7681560017"  -- Change the sound ID
    sound3.Parent = humanoidRootPart  -- You can attach the sound to the character's root part

    local sound4 = Instance.new("Sound")
    sound4.SoundId = "rbxassetid://112558145937517"  -- Change the sound ID
    sound4.Parent = humanoidRootPart  -- Attach the sound to the character's root part

    -- Visual effect for the move
    local visualEffect = Instance.new("ParticleEmitter")
    visualEffect.Texture = "rbxassetid://107877955127835"  -- New visual effect texture ID
    visualEffect.Color = ColorSequence.new(Color3.new(1, 0, 0))  -- Red color for better visibility
    visualEffect.Lifetime = NumberRange.new(0.5, 1)
    visualEffect.Rate = 100
    visualEffect.Speed = NumberRange.new(20, 30)
    visualEffect.Size = NumberSequence.new(2, 3)
    visualEffect.Transparency = NumberSequence.new(0, 0.5)
    visualEffect.Enabled = false  -- Ensure it starts as disabled
    visualEffect.Parent = humanoidRootPart

    -- Additional cool visual effect
    local coolEffect = Instance.new("ParticleEmitter")
    coolEffect.Texture = "rbxassetid://107877955127835"  -- New cool effect texture ID
    coolEffect.Color = ColorSequence.new(Color3.new(0, 1, 1))  -- Cyan color for contrast
    coolEffect.Lifetime = NumberRange.new(0.5, 1.5)
    coolEffect.Rate = 50
    coolEffect.Speed = NumberRange.new(15, 25)
    coolEffect.Size = NumberSequence.new(3, 4)
    coolEffect.Transparency = NumberSequence.new(0, 0.7)
    coolEffect.Enabled = false  -- Ensure it starts as disabled
    coolEffect.Parent = humanoidRootPart

    -- Black foggy aura
    local foggyAura = Instance.new("ParticleEmitter")
    foggyAura.Texture = "rbxassetid://243660364"  -- Black fog texture ID
    foggyAura.Color = ColorSequence.new(Color3.new(0, 0, 0))  -- Black color
    foggyAura.Lifetime = NumberRange.new(1, 2)
    foggyAura.Rate = 200
    foggyAura.Speed = NumberRange.new(5, 10)
    foggyAura.Size = NumberSequence.new(5, 10)
    foggyAura.Transparency = NumberSequence.new(0.5, 0.8)
    foggyAura.Enabled = false  -- Ensure it starts as disabled
    foggyAura.Parent = humanoidRootPart

    -- TweenService for smooth zigzag movement
    local TweenService = game:GetService("TweenService")

    -- Function to create a visual red effect
    local function createRedEffect()
        for _, part in pairs(character:GetChildren()) do
            if part:IsA("BasePart") then
                local originalColor = part.Color
                part.Color = Color3.fromRGB(255, 0, 0)

                -- Revert the color back after a short duration
                delay(0.5, function()
                    part.Color = originalColor
                end)
            end
        end
    end

    -- Function to create and display the ability text
    local function displayAbilityText(text, duration)
        local screenGui = Instance.new("ScreenGui")
        screenGui.Name = "AbilityTextGui"
        screenGui.Parent = player.PlayerGui

        local textLabel = Instance.new("TextLabel")
        textLabel.Name = "AbilityTextLabel"
        textLabel.Text = text
        textLabel.Size = UDim2.new(0.5, 0, 0.1, 0)
        textLabel.Position = UDim2.new(0.25, 0, 0, 0)
        textLabel.TextScaled = true
        textLabel.BackgroundTransparency = 1
        textLabel.TextColor3 = Color3.new(1, 1, 1)
        textLabel.Font = Enum.Font.SourceSansBold  -- Choose a good font
        textLabel.Parent = screenGui

        -- Remove the text after the specified duration
        game:GetService("Debris"):AddItem(screenGui, duration)
    end

    -- Function to set the screen color
    local function setScreenColor(color)
        local lighting = game:GetService("Lighting")
        lighting.Ambient = color
        lighting.OutdoorAmbient = color
    end

    -- Function to trigger the Fake Heel Flick ability
    local function triggerAbility1()
        -- Play the sound at the start of the move
        sound1:Play()

        -- Start the animation immediately (dribble animation)
        animationTrack1:Play()
        animationTrack1:AdjustSpeed(1.5)  -- Increase animation speed
        
        -- Zigzag movement variables
        local zigzagDuration = 0.25  -- Duration for faster movement
        local zigzagDistance = 10  -- Distance for more noticeable zigzag
        local forwardForce = 48  -- Forward force

        -- Create tweens for zigzag movement
        local function createZigzagTween(direction, index)
            local forwardVector = humanoidRootPart.CFrame.LookVector
            local rightVector = humanoidRootPart.CFrame.RightVector
            local targetPosition = humanoidRootPart.Position + (forwardVector * (zigzagDistance * index) + direction * rightVector * zigzagDistance)
            local targetCFrame = CFrame.new(targetPosition, targetPosition + forwardVector)  -- Ensure the player faces forward
            local tweenInfo = TweenInfo.new(zigzagDuration, Enum.EasingStyle.Linear, Enum.EasingDirection.InOut)
            local tween = TweenService:Create(humanoidRootPart, tweenInfo, {CFrame = targetCFrame})
            return tween
        end

        -- Enable visual effects
        visualEffect.Enabled = true
        coolEffect.Enabled = true
        foggyAura.Enabled = true

        -- Start zigzag movement
        local zigzagTweens = {}
        for i = 1, 10 do  -- Adjust the number of zigzags as needed
            local direction = (i % 2 == 1) and 1 or -1
            local tween = createZigzagTween(direction, i)
            table.insert(zigzagTweens, tween)
        end

        -- Play the zigzag tweens sequentially
        local function playZigzagTweens(index)
            if index > #zigzagTweens then
                -- Zigzag movement finished
                visualEffect.Enabled = false
                coolEffect.Enabled = false
                foggyAura.Enabled = false
                return
            end
            local currentTween = zigzagTweens[index]
            currentTween:Play()
            currentTween.Completed:Connect(function()
                playZigzagTweens(index + 1)
            end)
        end

        playZigzagTweens(1)
    end

    -- Function to trigger the Jinpachi Turn ability
    local function triggerAbility2()
        -- Apply a forward force to the player
        local bodyVelocity = Instance.new("BodyVelocity")
        bodyVelocity.Velocity = humanoidRootPart.CFrame.LookVector * 62
        bodyVelocity.MaxForce = Vector3.new(1000000, 0, 1000000)
        bodyVelocity.Parent = humanoidRootPart

        -- Remove the force after a short duration
        game.Debris:AddItem(bodyVelocity, 0.5)

        -- Play the sound at the start of the move
        sound2:Play()

        -- Start the animation immediately (dribble animation)
        animationTrack2:Play()
        animationTrack2:AdjustSpeed(0.9)

        -- Create the visual red effect
        createRedEffect()
    end

    -- Function to trigger the Metavision ability
    local function triggerAbility3()
        -- Play the sound at the start of the move
        sound4:Play()

        -- Display ability text
        displayAbilityText("Get On Your Knees, Blue Lock.", 5)

        -- Set the screen color to blue
        setScreenColor(Color3.new(0, 0, 1))

        -- After 5 seconds, display the "METAVISION ACTIVATED" text
        delay(5, function()
            displayAbilityText("METAVISION ACTIVATED", 5)
        end)

        -- Revert the screen color back to normal after 10 seconds
        delay(10, function()
            setScreenColor(Color3.new(0.5, 0.5, 0.5))  -- Adjust to default lighting settings
        end)
    end

    -- Function to trigger the Satoru Shot ability
    local function triggerAbility4()
        -- Play the sound at the start of the move
        sound3:Play()

        -- Start the animation immediately
        animationTrack3:Play()
        animationTrack3:AdjustSpeed(1)

        -- Fire the ball after the animation finishes
        animationTrack3.Stopped:Connect(function()
            local args = {
                [1] = 132,  -- Adjust force as necessary
            }
            game:GetService("ReplicatedStorage").Packages.Knit.Services.BallService.RE.Shoot:FireServer(unpack(args))
        end)
    end

    -- Clone the ability buttons (for mobile or GUI use)
    local Clone1 = player.PlayerGui.InGameUI.Bottom.Abilities["1"]:Clone()
    Clone1.Name = "FakeHeelFlickButton"
    Clone1.Parent = player.PlayerGui.InGameUI.Bottom.Abilities
    Clone1.LayoutOrder = -4
    Clone1.Keybind.Text = "Y"
    Clone1.Timer.Text = "Egoistic Style"
    Clone1.ActualTimer.Text = ""
    Clone1.Cooldown:Destroy()

    local Clone2 = player.PlayerGui.InGameUI.Bottom.Abilities["1"]:Clone()
    Clone2.Name = "JinpachiTurnButton"
    Clone2.Parent = player.PlayerGui.InGameUI.Bottom.Abilities
    Clone2.LayoutOrder = -2
    Clone2.Keybind.Text = "N"
    Clone2.Timer.Text = "Jinpachi Turn"
    Clone2.ActualTimer.Text = ""
    Clone2.Cooldown:Destroy()

    local Clone3 = player.PlayerGui.InGameUI.Bottom.Abilities["1"]:Clone()
    Clone3.Name = "MetavisionButton"
    Clone3.Parent = player.PlayerGui.InGameUI.Bottom.Abilities
    Clone3.LayoutOrder = -3
    Clone3.Keybind.Text = "V"
    Clone3.Timer.Text = "Metavision"
    Clone3.ActualTimer.Text = ""
    Clone3.Cooldown:Destroy()

    local Clone4 = player.PlayerGui.InGameUI.Bottom.Abilities["1"]:Clone()
    Clone4.Name = "SatoruShotButton"
    Clone4.Parent = player.PlayerGui.InGameUI.Bottom.Abilities
    Clone4.LayoutOrder = -1
    Clone4.Keybind.Text = "T"
    Clone4.Timer.Text = "Ronaldo"
    Clone4.ActualTimer.Text = ""
    Clone4.Cooldown:Destroy()

    -- Listen for the button activations (when clicked)
    Clone1.Activated:Connect(function()
        -- Trigger the Fake Heel Flick ability when the button is clicked
        triggerAbility1()
    end)

    Clone2.Activated:Connect(function()
        -- Trigger the Jinpachi Turn ability when the button is clicked
        triggerAbility2()
    end)

    Clone3.Activated:Connect(function()
        -- Trigger the Metavision ability when the button is clicked
        triggerAbility3()
    end)

    Clone4.Activated:Connect(function()
        -- Trigger the Satoru Shot ability when the button is clicked
        triggerAbility4()
    end)

    -- Listen for the key presses (PC input)
    game:GetService("UserInputService").InputBegan:Connect(function(input, gameProcessed)
        if gameProcessed then return end  -- Ignore if the game has already processed this input (e.g., typing in chat)

        if input.UserInputType == Enum.UserInputType.Keyboard and input.KeyCode == Enum.KeyCode.Y then
            -- Trigger the Fake Heel Flick ability when the "Y" key is pressed
            triggerAbility1()
        end

        if input.UserInputType == Enum.UserInputType.Keyboard and input.KeyCode == Enum.KeyCode.N then
            -- Trigger the Jinpachi Turn ability when the "N" key is pressed
            triggerAbility2()
        end

        if input.UserInputType == Enum.UserInputType.Keyboard and input.KeyCode == Enum.KeyCode.V then
            -- Trigger the Metavision ability when the "V" key is pressed
            triggerAbility3()
        end

        if input.UserInputType == Enum.UserInputType.Keyboard and input.KeyCode == Enum.KeyCode.T then
            -- Trigger the Satoru Shot ability when the "T" key is pressed
            triggerAbility4()
        end
    end)
end

-- Reinitialize the ability each time the character is added (e.g., when joining a match)
player.CharacterAdded:Connect(function()
    cleanup()
    initializeAbility()
end)

-- Initial cleanup and ability initialization
cleanup()
if player.Character then
    initializeAbility()
end
