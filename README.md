-- [[ ИНТЕРФЕЙС COMBO UNIVERSAL ]]
local ScreenGui = Instance.new("ScreenGui")
ScreenGui.Parent = game.CoreGui
ScreenGui.Name = "BY 9BACATEOFC"

local MainFrame = Instance.new("Frame")
MainFrame.Parent = ScreenGui
MainFrame.BackgroundColor3 = Color3.fromRGB(20, 20, 20)
MainFrame.BorderSizePixel = 0
MainFrame.Position = UDim2.new(0, 50, 0, 150)
MainFrame.Size = UDim2.new(0, 160, 0, 180)
MainFrame.Draggable = true
MainFrame.Active = true

-- Скругление углов
local UICorner = Instance.new("UICorner")
UICorner.CornerRadius = UDim.new(0, 10)
UICorner.Parent = MainFrame

-- Заголовок
local Title = Instance.new("TextLabel")
Title.Parent = MainFrame
Title.Size = UDim2.new(1, 0, 0, 35)
Title.BackgroundColor3 = Color3.fromRGB(35, 35, 35)
Title.Text = "Combo Universal"
Title.TextColor3 = Color3.fromRGB(0, 255, 255) -- Циановый цвет
Title.TextSize = 16
Title.Font = Enum.Font.GothamBold

local SubTitle = Instance.new("TextLabel")
SubTitle.Parent = MainFrame
SubTitle.Position = UDim2.new(0, 0, 0, 25)
SubTitle.Size = UDim2.new(1, 0, 0, 15)
SubTitle.BackgroundTransparency = 1
SubTitle.Text = "by BZ1P"
SubTitle.TextColor3 = Color3.fromRGB(255, 255, 255)
SubTitle.TextSize = 10
SubTitle.Font = Enum.Font.SourceSansItalic

-- Кнопки (Функция для создания стильных кнопок)
local function createBtn(text, pos, color)
    local btn = Instance.new("TextButton")
    btn.Parent = MainFrame
    btn.Size = UDim2.new(0, 140, 0, 30)
    btn.Position = pos
    btn.BackgroundColor3 = color
    btn.Text = text
    btn.TextColor3 = Color3.fromRGB(255, 255, 255)
    btn.Font = Enum.Font.Gotham
    btn.TextSize = 12
    local corner = Instance.new("UICorner")
    corner.CornerRadius = UDim.new(0, 6)
    corner.Parent = btn
    return btn
end

local FlyBtn = createBtn("FLY/GOD: OFF", UDim2.new(0, 10, 0, 50), Color3.fromRGB(150, 0, 0))
local SpeedBtn = createBtn("SPEED: OFF", UDim2.new(0, 10, 0, 90), Color3.fromRGB(0, 100, 200))
local JumpBtn = createBtn("INF JUMP: OFF", UDim2.new(0, 10, 0, 130), Color3.fromRGB(100, 0, 150))

-- [[ ЛОГИКА ]]
local lp = game.Players.LocalPlayer
local active = false
local speedActive = false
local infJumpActive = false

-- Fly/God/Noclip Logic
game:GetService("RunService").Stepped:Connect(function()
    if active and lp.Character then
        for _, p in pairs(lp.Character:GetDescendants()) do
            if p:IsA("BasePart") then p.CanCollide = false end
        end
        if lp.Character:FindFirstChild("Humanoid") then
            lp.Character.Humanoid:SetStateEnabled(Enum.HumanoidStateType.Dead, false)
        end
    end
end)

local function toggleFly()
    local char = lp.Character or lp.CharacterAdded:Wait()
    local hrp = char:WaitForChild("HumanoidRootPart")
    local bg = Instance.new("BodyGyro", hrp)
    local bv = Instance.new("BodyVelocity", hrp)
    bg.P = 9e4
    bg.maxTorque = Vector3.new(9e9, 9e9, 9e9)
    bv.maxForce = Vector3.new(9e9, 9e9, 9e9)
    
    task.spawn(function()
        while active do
            task.wait()
            if char:FindFirstChild("Humanoid") then char.Humanoid.PlatformStand = true end
            bv.velocity = (workspace.CurrentCamera.CFrame.LookVector * 60)
            bg.cframe = workspace.CurrentCamera.CFrame
        end
        if char:FindFirstChild("Humanoid") then char.Humanoid.PlatformStand = false end
        bg:Destroy()
        bv:Destroy()
    end)
end

-- Обработка кликов
FlyBtn.MouseButton1Click:Connect(function()
    active = not active
    FlyBtn.Text = active and "FLY/GOD: ON" or "FLY/GOD: OFF"
    FlyBtn.BackgroundColor3 = active and Color3.fromRGB(0, 150, 0) or Color3.fromRGB(150, 0, 0)
    if active then toggleFly() end
end)

SpeedBtn.MouseButton1Click:Connect(function()
    speedActive = not speedActive
    SpeedBtn.Text = speedActive and "SPEED: ON" or "SPEED: OFF"
    SpeedBtn.BackgroundColor3 = speedActive and Color3.fromRGB(0, 150, 0) or Color3.fromRGB(0, 100, 200)
    task.spawn(function()
        while speedActive do
            if lp.Character and lp.Character:FindFirstChild("Humanoid") then lp.Character.Humanoid.WalkSpeed = 100 end
            task.wait(0.1)
        end
        if lp.Character and lp.Character:FindFirstChild("Humanoid") then lp.Character.Humanoid.WalkSpeed = 16 end
    end)
end)

JumpBtn.MouseButton1Click:Connect(function()
    infJumpActive = not infJumpActive
    JumpBtn.Text = infJumpActive and "INF JUMP: ON" or "INF JUMP: OFF"
    JumpBtn.BackgroundColor3 = infJumpActive and Color3.fromRGB(0, 150, 0) or Color3.fromRGB(100, 0, 150)
end)

game:GetService("UserInputService").JumpRequest:Connect(function()
    if infJumpActive and lp.Character and lp.Character:FindFirstChild("Humanoid") then
        lp.Character.Humanoid:ChangeState("Jumping")
    end
end)
