-- 🛡️ ABC Protection UI (Full Version)

local Players = game:GetService("Players")
local lp = Players.LocalPlayer
local pgui = lp:WaitForChild("PlayerGui")
local Rep = game:GetService("ReplicatedStorage")
local TweenService = game:GetService("TweenService")
local UserInputService = game:GetService("UserInputService")

local enabled = false
-- القائمة الكاملة التي طلبتها
local blockedWords = {"log","clog","nv","alert","cmd","cmdbar","admin","bchat"}

-- إنشاء الواجهة
local sg = Instance.new("ScreenGui")
sg.Name = "ABC_System_Final"
sg.ResetOnSpawn = false
sg.Parent = pgui

--------------------------------
-- 🛠️ وظيفة السحب (Draggable)
--------------------------------
local function makeDraggable(gui)
    local dragging, dragInput, dragStart, startPos
    gui.InputBegan:Connect(function(input)
        if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then
            dragging = true
            dragStart = input.Position
            startPos = gui.Position
            input.Changed:Connect(function()
                if input.UserInputState == Enum.UserInputState.End then dragging = false end
            end)
        end
    end)
    gui.InputChanged:Connect(function(input)
        if input.UserInputType == Enum.UserInputType.MouseMovement or input.UserInputType == Enum.UserInputType.Touch then dragInput = input end
    end)
    UserInputService.InputChanged:Connect(function(input)
        if input == dragInput and dragging then
            local delta = input.Position - dragStart
            gui.Position = UDim2.new(startPos.X.Scale, startPos.X.Offset + delta.X, startPos.Y.Scale, startPos.Y.Offset + delta.Y)
        end
    end)
end

--------------------------------
-- 🟢 الدائرة القابلة للتحريك (ABC Button)
--------------------------------
local OpenCircle = Instance.new("TextButton")
OpenCircle.Size = UDim2.new(0, 60, 0, 60)
OpenCircle.Position = UDim2.new(0, 50, 0.5, -30)
OpenCircle.BackgroundColor3 = Color3.fromRGB(10, 10, 10)
OpenCircle.Text = "ABC"
OpenCircle.TextColor3 = Color3.fromRGB(255, 255, 255)
OpenCircle.Font = Enum.Font.GothamBold
OpenCircle.TextSize = 14
OpenCircle.Visible = false
OpenCircle.Parent = sg

Instance.new("UICorner", OpenCircle).CornerRadius = UDim.new(1, 0)
local CircleStroke = Instance.new("UIStroke", OpenCircle)
CircleStroke.Thickness = 3
CircleStroke.Color = Color3.fromRGB(0, 150, 255)

makeDraggable(OpenCircle)

--------------------------------
-- ⬛ القائمة الرئيسية (Main Frame)
--------------------------------
local MainFrame = Instance.new("Frame")
MainFrame.Size = UDim2.new(0, 300, 0, 180)
MainFrame.Position = UDim2.new(0.5, -150, 0.5, -90)
MainFrame.BackgroundColor3 = Color3.fromRGB(15, 15, 15)
MainFrame.Parent = sg

Instance.new("UICorner", MainFrame).CornerRadius = UDim.new(0, 10)
local MainStroke = Instance.new("UIStroke", MainFrame)
MainStroke.Thickness = 3
MainStroke.Color = Color3.fromRGB(0, 150, 255)

makeDraggable(MainFrame)

-- زر الإغلاق X
local CloseBtn = Instance.new("TextButton")
CloseBtn.Size = UDim2.new(0, 30, 0, 30)
CloseBtn.Position = UDim2.new(1, -35, 0, 5)
CloseBtn.BackgroundTransparency = 1
CloseBtn.Text = "X"
CloseBtn.TextColor3 = Color3.fromRGB(255, 50, 50)
CloseBtn.Font = Enum.Font.GothamBold
CloseBtn.TextSize = 20
CloseBtn.Parent = MainFrame

-- زر التشغيل (التصميم المطلوب)
local ToggleBtn = Instance.new("TextButton")
ToggleBtn.Size = UDim2.new(0, 200, 0, 50)
ToggleBtn.Position = UDim2.new(0.5, -100, 0.5, -10)
ToggleBtn.BackgroundColor3 = Color3.fromRGB(25, 25, 25)
ToggleBtn.Text = "OFF"
ToggleBtn.TextColor3 = Color3.fromRGB(255, 255, 255)
ToggleBtn.Font = Enum.Font.GothamBold
ToggleBtn.TextSize = 18
ToggleBtn.Parent = MainFrame
Instance.new("UICorner", ToggleBtn)

--------------------------------
-- 🔥 منطق الحماية (Core Logic)
--------------------------------

local function checkAndDestroy(gui)
    if enabled then
        local name = gui.Name:lower()
        for _, word in ipairs(blockedWords) do
            if name:find(word, 1, true) and gui.Name ~= sg.Name then
                gui:Destroy()
                break
            end
        end
    end
end

-- تنظيف الـ Remotes عند التشغيل
local function nukeRemotes()
    for _, obj in ipairs(Rep:GetDescendants()) do
        if obj:IsA("RemoteEvent") and obj.Name:lower():find("executeclientcommand") then
            obj:Destroy()
        end
    end
end

pgui.ChildAdded:Connect(function(child)
    task.wait(0.05)
    checkAndDestroy(child)
end)

--------------------------------
-- ✨ تأثيرات الأطراف والوظائف
--------------------------------
local function animate(s)
    task.spawn(function()
        while true do
            local t = TweenService:Create(s, TweenInfo.new(1.5, Enum.EasingStyle.Sine, Enum.EasingDirection.InOut, -1, true), {Color = Color3.fromRGB(0, 255, 255)})
            t:Play()
            break
        end
    end)
end

animate(MainStroke)
animate(CircleStroke)

CloseBtn.MouseButton1Click:Connect(function()
    MainFrame.Visible = false
    OpenCircle.Visible = true
end)

OpenCircle.MouseButton1Click:Connect(function()
    MainFrame.Visible = true
    OpenCircle.Visible = false
end)

ToggleBtn.MouseButton1Click:Connect(function()
    enabled = not enabled
    if enabled then
        ToggleBtn.Text = "ON"
        ToggleBtn.TextColor3 = Color3.fromRGB(0, 255, 150)
        nukeRemotes()
        -- تنظيف القوائم الموجودة حالياً
        for _, g in ipairs(pgui:GetChildren()) do checkAndDestroy(g) end
    else
        ToggleBtn.Text = "OFF"
        ToggleBtn.TextColor3 = Color3.fromRGB(255, 255, 255)
    end
end)
