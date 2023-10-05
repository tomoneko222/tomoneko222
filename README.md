local Players = game:GetService("Players")
local teleportingPlayer = Players.LocalPlayer
local teleportEnabled = true
local targetPlayerName = ""

local screenGui = Instance.new("ScreenGui")
screenGui.Parent = teleportingPlayer:WaitForChild("PlayerGui")

local button = Instance.new("TextButton")
button.Parent = screenGui
button.Size = UDim2.new(0, 150, 0, 65)
button.Position = UDim2.new(0.5, -75, 0, 0)
button.Text = "悪魔所属友猫作"
button.TextSize = 24
button.BackgroundColor3 = Color3.fromRGB(0, 255, 0)

local textBox = Instance.new("TextBox")
textBox.Parent = screenGui
textBox.Size = UDim2.new(0, 150, 0, 65)
textBox.Position = UDim2.new(0.5, 75, 0, 0)
textBox.Text = ""
textBox.PlaceholderText = "プレイヤーIDを入力"

textBox.FocusLost:Connect(function()
    if teleportEnabled then
        targetPlayerName = textBox.Text
    end
end)

button.MouseButton1Click:Connect(function()
    teleportEnabled = not teleportEnabled
    if teleportEnabled then
        targetPlayerName = textBox.Text
    else
        targetPlayerName = ""
    end
    button.Text = teleportEnabled and "オン" or "オフ"
    button.BackgroundColor3 = teleportEnabled and Color3.fromRGB(0, 255, 0) or Color3.fromRGB(255, 0, 0)
end)

while wait(0.1) do
    local player
    for _, p in ipairs(Players:GetPlayers()) do
        if p.Name == targetPlayerName or p.DisplayName == targetPlayerName then
            player = p
            break
        end
    end

    if player and player.Character and player.Character:FindFirstChild("HumanoidRootPart") and teleportingPlayer.Character and teleportingPlayer.Character:FindFirstChild("HumanoidRootPart") then
        local teleportPosition = player.Character.HumanoidRootPart.Position - player.Character.HumanoidRootPart.CFrame.LookVector * 3 + player.Character.HumanoidRootPart.CFrame.RightVector * 2
        local teleportDirection = player.Character.HumanoidRootPart.CFrame.LookVector
        teleportingPlayer.Character.HumanoidRootPart.CFrame = CFrame.new(teleportPosition, teleportPosition + teleportDirection)
    end
end
