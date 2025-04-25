
-- LocalScript dentro de StarterPlayerScripts
local player = game.Players.LocalPlayer
local character = player.Character or player.CharacterAdded:Wait()
local humanoid = character:WaitForChild("Humanoid")
local rootPart = character:WaitForChild("HumanoidRootPart")

local flying = false
local speed = 50 -- Velocidade do voo

local function fly()
    if flying then
        rootPart.Velocity = Vector3.new(rootPart.Velocity.X, speed, rootPart.Velocity.Z)
    end
end

local function toggleFly()
    flying = not flying
    if flying then
        humanoid.PlatformStand = true
        while flying do
            fly()
            game:GetService("RunService").RenderStepped:Wait()
        end
        humanoid.PlatformStand = false
    end
end

game:GetService("UserInputService").InputBegan:Connect(function(input)
    if input.KeyCode == Enum.KeyCode.E then -- Pressione E para voar
        toggleFly()
    end
end)

game:GetService("UserInputService").InputChanged:Connect(function(input)
    if flying and input.KeyCode == Enum.KeyCode.W then
        rootPart.Velocity = rootPart.Velocity + Vector3.new(0, 0, speed)
    elseif flying and input.KeyCode == Enum.KeyCode.S then
        rootPart.Velocity = rootPart.Velocity + Vector3.new(0, 0, -speed)
    elseif flying and input.KeyCode == Enum.KeyCode.A then
        rootPart.Velocity = rootPart.Velocity + Vector3.new(-speed, 0, 0)
    elseif flying and input.KeyCode == Enum.KeyCode.D then
        rootPart.Velocity = rootPart.Velocity + Vector3.new(speed, 0, 0)
    end
end)
