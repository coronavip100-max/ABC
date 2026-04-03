-- [[ ABC SUPER GUI - FINAL MILITARY DELTA EDITION ]] --
local TweenService = game:GetService("TweenService")
local UserInputService = game:GetService("UserInputService")
local Players = game:GetService("Players")
local RunService = game:GetService("RunService")

local ScreenGui = Instance.new("ScreenGui", game.CoreGui)
ScreenGui.Name = "ABC_SUPER_GUI"

local key = "ABC123"

-- Rainbow Function
local function MakeRainbow(obj, property)
    if property == "Background" then
        if obj:FindFirstChild("RainbowGrad") then obj.RainbowGrad:Destroy() end
        local grad = Instance.new("UIGradient", obj)
        grad.Name = "RainbowGrad"
        grad.Color = ColorSequence.new{
            ColorSequenceKeypoint.new(0, Color3.fromRGB(255,0,0)),
            ColorSequenceKeypoint.new(0.2, Color3.fromRGB(255,255,0)),
            ColorSequenceKeypoint.new(0.4, Color3.fromRGB(0,255,0)),
            ColorSequenceKeypoint.new(0.6, Color3.fromRGB(0,255,255)),
            ColorSequenceKeypoint.new(0.8, Color3.fromRGB(0,0,255)),
            ColorSequenceKeypoint.new(1, Color3.fromRGB(255,0,255))
        }
        spawn(function()
            while obj.Parent and grad.Parent do
                grad.Rotation += 2
                task.wait(0.02)
            end
        end)
    elseif property == "Text" then
        spawn(function()
            while obj.Parent do
                for i=0,1,0.01 do
                    obj.TextColor3 = Color3.fromHSV(i,1,1)
                    task.wait(0.02)
                end
            end
        end)
    elseif property == "Stroke" then
        local stroke = obj:FindFirstChildOfClass("UIStroke") or Instance.new("UIStroke", obj)
        stroke.Thickness = 3
        local grad = Instance.new("UIGradient", stroke)
        grad.Color = ColorSequence.new{
            ColorSequenceKeypoint.new(0, Color3.fromRGB(255,0,0)),
            ColorSequenceKeypoint.new(0.2, Color3.fromRGB(255,255,0)),
            ColorSequenceKeypoint.new(0.4, Color3.fromRGB(0,255,0)),
            ColorSequenceKeypoint.new(0.6, Color3.fromRGB(0,255,255)),
            ColorSequenceKeypoint.new(0.8, Color3.fromRGB(0,0,255)),
            ColorSequenceKeypoint.new(1, Color3.fromRGB(255,0,255))
        }
        spawn(function()
            while obj.Parent and grad.Parent do
                grad.Rotation += 2
                task.wait(0.02)
            end
        end)
    end
end

-- Drag Function
local function Drag(frame)
    local dragging, startPos, startFramePos
    frame.InputBegan:Connect(function(input)
        if input.UserInputType == Enum.UserInputType.MouseButton1 then
            dragging = true
            startPos = input.Position
            startFramePos = frame.Position
        end
    end)
    frame.InputEnded:Connect(function(input)
        if input.UserInputType == Enum.UserInputType.MouseButton1 then dragging = false end
    end)
    UserInputService.InputChanged:Connect(function(input)
        if dragging and input.UserInputType == Enum.UserInputType.MouseMovement then
            local delta = input.Position - startPos
            frame.Position = UDim2.new(startFramePos.X.Scale, startFramePos.X.Offset + delta.X, startFramePos.Y.Scale, startFramePos.Y.Offset + delta.Y)
        end
    end)
end

-- LOGIN FRAME
local Login = Instance.new("Frame", ScreenGui)
Login.Size = UDim2.new(0,320,0,180)
Login.Position = UDim2.new(0.5,-160,0.5,-90)
MakeRainbow(Login,"Background")
Drag(Login)

local Title = Instance.new("TextLabel", Login)
Title.Size = UDim2.new(1,0,0,40)
Title.Text = "TOP ON ABC"
Title.BackgroundTransparency = 1
Title.TextScaled = true
MakeRainbow(Title,"Text")

local Box = Instance.new("TextBox", Login)
Box.Size = UDim2.new(0.8,0,0,40)
Box.Position = UDim2.new(0.1,0,0.4,0)
Box.PlaceholderText = "Enter Key"
MakeRainbow(Box,"Background")
MakeRainbow(Box,"Stroke")

local Enter = Instance.new("TextButton", Login)
Enter.Size = UDim2.new(0.6,0,0,35)
Enter.Position = UDim2.new(0.2,0,0.7,0)
Enter.Text = "ENTER"
MakeRainbow(Enter,"Background")

-- MAIN WINDOW
local Main = Instance.new("Frame", ScreenGui)
Main.Size = UDim2.new(0,500,0,400)
Main.Position = UDim2.new(0.5,-250,0.5,-200)
Main.Visible = false
MakeRainbow(Main,"Background")
MakeRainbow(Main,"Stroke")
Drag(Main)

-- Tabs System
local TabFrame = Instance.new("Frame", Main)
TabFrame.Size = UDim2.new(1,0,0,40)
TabFrame.BackgroundTransparency = 0.7

local Tabs = {"Main","Players","Settings","Fun","Security"}
local Pages = {}

for i,name in ipairs(Tabs) do
    local btn = Instance.new("TextButton", TabFrame)
    btn.Size = UDim2.new(0.2,0,1,0)
    btn.Position = UDim2.new((i-1)*0.2,0,0,0)
    btn.Text = name
    MakeRainbow(btn,"Background")
    MakeRainbow(btn,"Text")

    local page = Instance.new("ScrollingFrame", Main)
    page.Size = UDim2.new(1,0,1,-40)
    page.Position = UDim2.new(0,0,0,40)
    page.Visible = false
    page.BackgroundTransparency = 1
    page.CanvasSize = UDim2.new(0,0,2,0)
    page.ScrollBarThickness = 5
    Pages[name] = page

    btn.MouseButton1Click:Connect(function()
        for _,p in pairs(Pages) do p.Visible = false end
        page.Visible = true
    end)
end

-- 1. MAIN TAB (تم تحديث عبود ولورين بالأكواد المطلوبة)
local Scripts = {
    {name="Abood", url="https://raw.githubusercontent.com/jshshga/abood-au3/refs/heads/main/aboodau3"},
    {name="LORLN", url="https://raw.githubusercontent.com/LORLN9/LORLN-PRO/refs/heads/main/LORLN%20LOR"},
    {name="Youssef", url="https://pastebin.com/raw/aVgRfxKi"}
}

for i,s in ipairs(Scripts) do
    local b = Instance.new("TextButton", Pages["Main"])
    b.Size = UDim2.new(0.8,0,0,40)
    b.Position = UDim2.new(0.1,0,0, (i-1)*50 + 20)
    b.Text = s.name
    MakeRainbow(b,"Background")
    b.MouseButton1Click:Connect(function() 
        pcall(function()
            -- تنفيذ السكربتات البرمجية المطلوبة
            loadstring(game:HttpGet(s.url))() 
        end)
    end)
end

-- 2. PLAYERS TAB
local PlayersTab = Pages["Players"]
local UIListLayout = Instance.new("UIListLayout", PlayersTab)
UIListLayout.SortOrder = Enum.SortOrder.LayoutOrder
UIListLayout.Padding = UDim.new(0,5)

local function RefreshPlayers()
    for _,child in ipairs(PlayersTab:GetChildren()) do if child:IsA("Frame") then child:Destroy() end end
    for _,p in ipairs(Players:GetPlayers()) do
        if p == Players.LocalPlayer then continue end
        local frame = Instance.new("Frame", PlayersTab)
        frame.Size = UDim2.new(1,-10,0,50)
        frame.BackgroundColor3 = Color3.fromRGB(0,0,0)
        frame.BackgroundTransparency = 0.5
        
        local img = Instance.new("ImageLabel", frame)
        img.Size = UDim2.new(0,40,0,40)
        img.Position = UDim2.new(0,5,0,5)
        pcall(function() img.Image = Players:GetUserThumbnailAsync(p.UserId, Enum.ThumbnailType.AvatarBust, Enum.ThumbnailSize.Size48x48) end)

        local nameL = Instance.new("TextLabel", frame)
        nameL.Size = UDim2.new(0.4,0,1,0)
        nameL.Position = UDim2.new(0.2,0,0,0)
        nameL.Text = p.Name
        nameL.TextColor3 = Color3.fromRGB(255,255,255)
        nameL.BackgroundTransparency = 1
        nameL.TextScaled = true

        local TPBtn = Instance.new("TextButton", frame)
        TPBtn.Size = UDim2.new(0,100,0,30)
        TPBtn.Position = UDim2.new(1,-110,0.5,-15)
        TPBtn.Text = "Teleport"
        MakeRainbow(TPBtn,"Background")
        TPBtn.MouseButton1Click:Connect(function()
            if p.Character and p.Character:FindFirstChild("HumanoidRootPart") then
                Players.LocalPlayer.Character.HumanoidRootPart.CFrame = p.Character.HumanoidRootPart.CFrame
            end
        end)
    end
end
RefreshPlayers()
Players.PlayerAdded:Connect(RefreshPlayers)
Players.PlayerRemoving:Connect(RefreshPlayers)

-- 3. SETTINGS TAB
local SetTab = Pages["Settings"]
local function AddSetBtn(txt, pos, callback)
    local b = Instance.new("TextButton", SetTab)
    b.Size = UDim2.new(0.8,0,0,40)
    b.Position = UDim2.new(0.1,0,0,pos)
    b.Text = txt
    MakeRainbow(b,"Background")
    b.MouseButton1Click:Connect(callback)
    return b
end
local namesOn = true
AddSetBtn("Hide/Show Names", 20, function()
    namesOn = not namesOn
    for _,v in pairs(Players:GetPlayers()) do 
        pcall(function() 
            if v.Character:FindFirstChild("Head") and v.Character.Head:FindFirstChildOfClass("BillboardGui") then
                v.Character.Head:FindFirstChildOfClass("BillboardGui").Enabled = namesOn 
            end
        end) 
    end
end)
AddSetBtn("Red Theme", 70, function() if Main:FindFirstChild("RainbowGrad") then Main.RainbowGrad:Destroy() end Main.BackgroundColor3 = Color3.fromRGB(150,0,0) end)
AddSetBtn("White Theme", 120, function() if Main:FindFirstChild("RainbowGrad") then Main.RainbowGrad:Destroy() end Main.BackgroundColor3 = Color3.fromRGB(255,255,255) end)
AddSetBtn("Reset Rainbow", 170, function() MakeRainbow(Main,"Background") end)

-- 4. FUN TAB
local FunTab = Pages["Fun"]
AddSetBtn("Low Gravity (Moon)", 20, function() workspace.Gravity = (workspace.Gravity == 196.2) and 50 or 196.2 end).Parent = FunTab
AddSetBtn("Super Speed", 70, function() local h = Players.LocalPlayer.Character.Humanoid h.WalkSpeed = (h.WalkSpeed == 16) and 100 or 16 end).Parent = FunTab
local spin = false
AddSetBtn("Spin Me!", 120, function() 
    spin = not spin 
    spawn(function() 
        while spin do 
            Players.LocalPlayer.Character.HumanoidRootPart.CFrame *= CFrame.Angles(0,math.rad(20),0) 
            task.wait(0.01) 
        end 
    end) 
end).Parent = FunTab

-- 5. SECURITY TAB
local SecTab = Pages["Security"]
AddSetBtn("Anti-AFK", 20, function()
    Players.LocalPlayer.Idled:Connect(function() 
        game:GetService("VirtualUser"):Button2Down(Vector2.new(0,0),workspace.CurrentCamera.CFrame) 
        wait(1) 
        game:GetService("VirtualUser"):Button2Up(Vector2.new(0,0),workspace.CurrentCamera.CFrame) 
    end)
end).Parent = SecTab
AddSetBtn("Server Hop", 70, function() game:GetService("TeleportService"):Teleport(game.PlaceId) end).Parent = SecTab
AddSetBtn("Self Destruct", 120, function() ScreenGui:Destroy() end).Parent = SecTab

-- Final Logic
Enter.MouseButton1Click:Connect(function() if Box.Text == key then Login.Visible = false Main.Visible = true Pages["Main"].Visible = true end end)
local Toggle = Instance.new("TextButton", ScreenGui)
Toggle.Size = UDim2.new(0,50,0,50)
Toggle.Position = UDim2.new(0,20,0,50)
Toggle.Text = "ABC"
Toggle.TextScaled = true
Instance.new("UICorner", Toggle).CornerRadius = UDim.new(1,0)
MakeRainbow(Toggle,"Background")
Drag(Toggle)
local open = false
Toggle.MouseButton1Click:Connect(function() open = not open Main.Visible = open end)
