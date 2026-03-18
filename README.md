local player = game.Players.LocalPlayer
local UIS = game:GetService("UserInputService")

-- luôn lấy character mới
local function getChar()
    return player.Character or player.CharacterAdded:Wait()
end

-- GUI
local gui = Instance.new("ScreenGui", game.CoreGui)

local toggleBtn = Instance.new("TextButton", gui)
toggleBtn.Size = UDim2.new(0, 110, 0, 40)
toggleBtn.Position = UDim2.new(0, 10, 0, 10)
toggleBtn.Text = "NT Script"
toggleBtn.BackgroundColor3 = Color3.fromRGB(30,30,30)
toggleBtn.TextColor3 = Color3.new(1,1,1)

local frame = Instance.new("Frame", gui)
frame.Size = UDim2.new(0, 240, 0, 200)
frame.Position = UDim2.new(0.3, 0, 0.3, 0)
frame.BackgroundColor3 = Color3.fromRGB(20,20,20)
frame.Visible = false
frame.Active = true
frame.Draggable = true

toggleBtn.MouseButton1Click:Connect(function()
    frame.Visible = not frame.Visible
end)

local stroke = Instance.new("UIStroke", frame)
stroke.Color = Color3.fromRGB(0,170,255)

-- tạo nút
function createBtn(text, y, func)
    local b = Instance.new("TextButton", frame)
    b.Size = UDim2.new(1, -20, 0, 40)
    b.Position = UDim2.new(0, 10, 0, y)
    b.Text = text
    b.BackgroundColor3 = Color3.fromRGB(40,40,40)
    b.TextColor3 = Color3.new(1,1,1)
    b.MouseButton1Click:Connect(func)
end

-- 👁 ESP AUTO
function addESP(target)
    if target:FindFirstChild("ESP") then return end
    
    local head = target:FindFirstChild("Head")
    if head then
        local bill = Instance.new("BillboardGui", target)
        bill.Name = "ESP"
        bill.Size = UDim2.new(0,100,0,40)
        bill.Adornee = head
        bill.AlwaysOnTop = true
        
        local txt = Instance.new("TextLabel", bill)
        txt.Size = UDim2.new(1,0,1,0)
        txt.BackgroundTransparency = 1
        txt.TextColor3 = Color3.fromRGB(255,0,0)
        txt.Text = target.Name
    end
end

-- auto esp
task.spawn(function()
    while true do
        for _, p in pairs(game.Players:GetPlayers()) do
            if p ~= player and p.Character then
                addESP(p.Character)
            end
        end
        task.wait(1)
    end
end)

-- ⚡ SPEED
local speedOn = false

createBtn("Speed ON/OFF", 20, function()
    speedOn = not speedOn
    
    local char = getChar()
    local hum = char:FindFirstChildOfClass("Humanoid")
    
    if hum then
        hum.WalkSpeed = speedOn and 50 or 16
    end
end)

-- 🦘 JUMP
local jumpOn = false

createBtn("High Jump ON/OFF", 80, function()
    jumpOn = not jumpOn
    
    local char = getChar()
    local hum = char:FindFirstChildOfClass("Humanoid")
    
    if hum then
        hum.JumpPower = jumpOn and 120 or 50
    end
end)
