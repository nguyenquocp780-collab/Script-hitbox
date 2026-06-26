# Script-hitbox
Hitbox For slap battles
copy bên dưới

-- Khởi tạo Dịch vụ
local Players = game:GetService("Players")
local RunService = game:GetService("RunService")
local LocalPlayer = Players.LocalPlayer

-- Cấu hình Hitbox
local HitboxSize = Vector3.new(10, 10, 10)
local HitboxColor = Color3.fromRGB(255, 0, 0)
_G.HitboxEnabled = false

-- Tạo Giao diện UI
local ScreenGui = Instance.new("ScreenGui")
ScreenGui.Name = "HitboxGUI"
ScreenGui.Parent = game.CoreGui
ScreenGui.DisplayOrder = 999 -- Luôn nằm trên cùng

local ToggleButton = Instance.new("TextButton")
ToggleButton.Parent = ScreenGui
ToggleButton.BackgroundColor3 = Color3.fromRGB(200, 50, 50)
ToggleButton.Position = UDim2.new(0.1, 0, 0.1, 0)
ToggleButton.Size = UDim2.new(0, 150, 0, 50)
ToggleButton.Text = "Hitbox: OFF"
ToggleButton.TextColor3 = Color3.new(1, 1, 1)
ToggleButton.Font = Enum.Font.GothamBold
ToggleButton.TextSize = 18

-- Làm cho nút có thể di chuyển (Draggable)
ToggleButton.Draggable = true 

-- Hàm reset hitbox
local function ResetAllHitboxes()
    for _, player in pairs(Players:GetPlayers()) do
        if player ~= LocalPlayer and player.Character and player.Character:FindFirstChild("HumanoidRootPart") then
            local hrp = player.Character.HumanoidRootPart
            hrp.Size = Vector3.new(2, 2, 1) -- Kích thước mặc định Roblox
            hrp.Transparency = 1
        end
    end
end

-- Sự kiện nhấn nút (Dùng MouseButton1Down để nhạy hơn trên Executor)
ToggleButton.MouseButton1Down:Connect(function()
    _G.HitboxEnabled = not _G.HitboxEnabled
    
    if _G.HitboxEnabled then
        ToggleButton.Text = "Hitbox: ON"
        ToggleButton.BackgroundColor3 = Color3.fromRGB(50, 200, 50)
    else
        ToggleButton.Text = "Hitbox: OFF"
        ToggleButton.BackgroundColor3 = Color3.fromRGB(200, 50, 50)
        ResetAllHitboxes()
    end
end)

-- Vòng lặp cập nhật hitbox
RunService.RenderStepped:Connect(function()
    if _G.HitboxEnabled then
        for _, player in pairs(Players:GetPlayers()) do
            if player ~= LocalPlayer and player.Character and player.Character:FindFirstChild("HumanoidRootPart") then
                local hrp = player.Character.HumanoidRootPart
                hrp.Size = HitboxSize
                hrp.Transparency = 0.5
                hrp.Color = HitboxColor
                hrp.Material = Enum.Material.Neon
                hrp.CanCollide = false
            end
        end
    end
end)
