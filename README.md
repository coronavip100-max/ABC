-- ABC SCRIPT - FINAL MASTER SYSTEM (ALL LINKS INTEGRATED)
local TweenService = game:GetService("TweenService")
local RunService = game:GetService("RunService")

local ScreenGui = Instance.new("ScreenGui")
ScreenGui.Parent = game.CoreGui
ScreenGui.Name = "ABC_Final_Production"
ScreenGui.IgnoreGuiInset = true

-- [ 1. زر الدائرة العائم ]
local ToggleBtn = Instance.new("TextButton", ScreenGui)
ToggleBtn.BackgroundColor3 = Color3.fromRGB(5, 5, 15)
ToggleBtn.Position = UDim2.new(0.05, 0, 0.4, 0)
ToggleBtn.Size = UDim2.new(0, 60, 0, 60)
ToggleBtn.Text = "ABC"
ToggleBtn.TextColor3 = Color3.fromRGB(0, 255, 255)
ToggleBtn.Font = Enum.Font.GothamBold
ToggleBtn.Active = true
ToggleBtn.Draggable = true
Instance.new("UICorner", ToggleBtn).CornerRadius = UDim.new(1, 0)

local ToggleStroke = Instance.new("UIStroke", ToggleBtn)
ToggleStroke.Thickness = 2.5
ToggleStroke.Color = Color3.fromRGB(0, 200, 255)
local ToggleGradient = Instance.new("UIGradient", ToggleStroke)
ToggleGradient.Color = ColorSequence.new{ColorSequenceKeypoint.new(0, Color3.fromRGB(0, 150, 255)), ColorSequenceKeypoint.new(1, Color3.fromRGB(0, 255, 200))}

-- [ 2. القائمة الرئيسية ]
local MainFrame = Instance.new("Frame", ScreenGui)
MainFrame.BackgroundColor3 = Color3.fromRGB(4, 4, 10)
MainFrame.Position = UDim2.new(0.5, -175, 0.5, -250)
MainFrame.Size = UDim2.new(0, 350, 0, 500)
MainFrame.Visible = true
MainFrame.ClipsDescendants = true
MainFrame.Draggable = true
MainFrame.Active = true
Instance.new("UICorner", MainFrame).CornerRadius = UDim.new(0, 15)

local MainStroke = Instance.new("UIStroke", MainFrame)
MainStroke.Thickness = 3
MainStroke.Color = Color3.fromRGB(0, 200, 255)

ToggleBtn.MouseButton1Click:Connect(function() MainFrame.Visible = not MainFrame.Visible end)

-- [ 3. نظام النجوم ]
local StarsContainer = Instance.new("Frame", MainFrame)
StarsContainer.Size = UDim2.new(1, 0, 1, 0); StarsContainer.BackgroundTransparency = 1
local starsActive = true
local function spawnStar()
    if not starsActive then return end
    local star = Instance.new("Frame", StarsContainer)
    star.Size = UDim2.new(0, math.random(1,2), 0, math.random(1,2))
    star.Position = UDim2.new(math.random(), 0, -0.05, 0); star.BackgroundColor3 = Color3.fromRGB(0, 255, 255)
    Instance.new("UICorner", star); local speed = math.random(5, 10)
    TweenService:Create(star, TweenInfo.new(speed), {Position = UDim2.new(star.Position.X.Scale, 0, 1.1, 0), BackgroundTransparency = 1}):Play()
    task.delay(speed, function() star:Destroy() end)
end
task.spawn(function() while true do spawnStar() task.wait(0.18) end end)

-- [ 4. العنوان والبحث ]
local Title = Instance.new("TextLabel", MainFrame)
Title.Text = "ABC"; Title.Size = UDim2.new(1, 0, 0, 45); Title.Position = UDim2.new(0, 0, 0, 10); Title.BackgroundTransparency = 1; Title.TextColor3 = Color3.fromRGB(0, 255, 255); Title.Font = Enum.Font.GothamBold; Title.TextSize = 26

local SearchBar = Instance.new("TextBox", MainFrame)
SearchBar.Size = UDim2.new(0.85, 0, 0, 35); SearchBar.Position = UDim2.new(0.075, 0, 0, 60); SearchBar.BackgroundColor3 = Color3.fromRGB(15, 15, 25); SearchBar.PlaceholderText = "Search Scripts..."; SearchBar.TextColor3 = Color3.new(1, 1, 1); SearchBar.Font = Enum.Font.Gotham; Instance.new("UICorner", SearchBar)
local SearchStroke = Instance.new("UIStroke", SearchBar); SearchStroke.Color = Color3.fromRGB(0, 150, 255)

-- [ 5. القائمة والبطاقات ]
local Scroll = Instance.new("ScrollingFrame", MainFrame)
Scroll.Size = UDim2.new(0.9, 0, 0, 360); Scroll.Position = UDim2.new(0.05, 0, 0, 110); Scroll.BackgroundTransparency = 1; Scroll.ScrollBarThickness = 0; Scroll.CanvasSize = UDim2.new(0,0,2.5,0)
Instance.new("UIListLayout", Scroll).Padding = UDim.new(0, 12)

local allCardsStrokes = {}
local function createCard(name, url)
    local Card = Instance.new("Frame", Scroll)
    Card.Name = name; Card.Size = UDim2.new(1, 0, 0, 70); Card.BackgroundColor3 = Color3.fromRGB(10, 10, 15); Instance.new("UICorner", Card)
    local cStroke = Instance.new("UIStroke", Card); cStroke.Thickness = 2; cStroke.Color = MainStroke.Color; table.insert(allCardsStrokes, cStroke)
    
    local n = Instance.new("TextLabel", Card); n.Text = name; n.Size = UDim2.new(0.6,0,1,0); n.Position = UDim2.new(0.05,0,0,0); n.BackgroundTransparency = 1; n.TextColor3 = Color3.new(1,1,1); n.Font = Enum.Font.GothamBold; n.TextXAlignment = 0; n.TextSize = 17
    local b = Instance.new("TextButton", Card); b.Text = "EXECUTE"; b.Size = UDim2.new(0,95,0,35); b.Position = UDim2.new(0.65,0,0.25,0); b.BackgroundColor3 = Color3.fromRGB(220,30,30); b.TextColor3 = Color3.new(1,1,1); b.Font = Enum.Font.GothamBold; Instance.new("UICorner", b)
    
    b.MouseButton1Click:Connect(function() 
        loadstring(game:HttpGet(url))()
        print("Executed: " .. name)
    end)
end

-- إضافة الروابط التي طلبتها بالترتيب
createCard("ABOOD", "https://raw.githubusercontent.com/jshshga/abood-au3/refs/heads/main/aboodau3")
createCard("LORLN", "https://raw.githubusercontent.com/LORLN9/LORLN-PRO/refs/heads/main/LORLN%20LOR")
createCard("YOUSEF", "https://pastebin.com/raw/aVgRfxKi")
createCard("QUALITY", "https://raw.githubusercontent.com/randomstring0/pshade-ultimate/refs/heads/main/src/cd.lua")
createCard("DARK HUB", "https://rawscripts.net/raw/mwahb-rsm-adna-Dark-Hub-122515")                createCard("BLOOD", "https://rawscripts.net/raw/Universal-Script-Blood-scriptss-112548")              createCard("MOLYEN", "https://rawscripts.net/raw/Universal-Script-Molyn-better-than-VR7-193694")                                                                                                                                              createCard("MURDERERS VS SHERIFFS DUELS", "https://rawscripts.net/raw/Murderers-VS-Sheriffs-DUELS-CyberCoders-Menu-II-193913")                                                                                                                            createCard("BROOKHAVEN RP", "https://rawscripts.net/raw/Brookhaven-RP-ZAGNZ-X-207641")                             createCard("BRUTON", "https://rawscripts.net/raw/Universal-Script-Bruton-script-arab-195454") createCard("STEAL A BRAINROT", "https://rawscripts.net/raw/Universal-Script-Kurd-Hub-27356")                            createCard("THE SURVIVAL GAME", "https://rawscripts.net/raw/civilization-survival-game-LedZ-Civ-75902")                       
SearchBar:GetPropertyChangedSignal("Text"):Connect(function()
    local input = string.lower(SearchBar.Text)
    for _, card in pairs(Scroll:GetChildren()) do
        if card:IsA("Frame") then
            card.Visible = (input == "" or string.find(string.lower(card.Name), input))
        end
    end
end)

-- [ 6. الإعدادات ]
local SettingsFrame = Instance.new("Frame", ScreenGui)
SettingsFrame.BackgroundColor3 = Color3.fromRGB(5, 5, 15); SettingsFrame.Position = UDim2.new(0.5, -140, 0.5, -160); SettingsFrame.Size = UDim2.new(0, 0, 0, 0); SettingsFrame.Visible = false; SettingsFrame.ClipsDescendants = true; Instance.new("UICorner", SettingsFrame)
local SetStroke = Instance.new("UIStroke", SettingsFrame); SetStroke.Thickness = 2.5; SetStroke.Color = MainStroke.Color

local function updateAllColors(newColor)
    MainStroke.Color = newColor; SetStroke.Color = newColor; SearchStroke.Color = newColor; ToggleStroke.Color = newColor; Title.TextColor3 = newColor
    for _, s in pairs(allCardsStrokes) do s.Color = newColor end
end

local function createSetBtn(text, pos, callback)
    local btn = Instance.new("TextButton", SettingsFrame)
    btn.Text = text; btn.Size = UDim2.new(0.85, 0, 0, 45); btn.Position = UDim2.new(0.075, 0, 0, pos); btn.BackgroundColor3 = Color3.fromRGB(25, 25, 45); btn.TextColor3 = Color3.new(1, 1, 1); btn.Font = Enum.Font.Gotham; Instance.new("UICorner", btn); btn.MouseButton1Click:Connect(callback)
end

createSetBtn("Change Theme Color", 70, function() updateAllColors(Color3.fromHSV(math.random(), 0.8, 1)) end)
createSetBtn("Toggle Stars", 125, function() starsActive = not starsActive; StarsContainer.Visible = starsActive end)
createSetBtn("Reset UI Position", 180, function() MainFrame.Position = UDim2.new(0.5, -175, 0.5, -250) end)

-- [ 7. أزرار التحكم ]
local CloseBtn = Instance.new("TextButton", MainFrame)
CloseBtn.Text = "×"; CloseBtn.Position = UDim2.new(1,-45,0,5); CloseBtn.Size = UDim2.new(0,45,0,45); CloseBtn.BackgroundTransparency = 1; CloseBtn.TextColor3 = Color3.fromRGB(255,70,70); CloseBtn.TextSize = 35
CloseBtn.MouseButton1Click:Connect(function() MainFrame.Visible = false end)

local SettingsBtn = Instance.new("TextButton", MainFrame)
SettingsBtn.Text = "⚙"; SettingsBtn.Position = UDim2.new(1,-85,0,5); SettingsBtn.Size = UDim2.new(0,45,0,45); SettingsBtn.BackgroundTransparency = 1; SettingsBtn.TextColor3 = Color3.fromRGB(0,255,255); SettingsBtn.TextSize = 28
SettingsBtn.MouseButton1Click:Connect(function() SettingsFrame.Visible = true; SettingsFrame:TweenSize(UDim2.new(0, 280, 0, 320), "Out", "Back", 0.4) end)

local SetClose = Instance.new("TextButton", SettingsFrame)
SetClose.Text = "×"; SetClose.Position = UDim2.new(1,-35,0,5); SetClose.Size = UDim2.new(0,35,0,35); SetClose.BackgroundTransparency = 1; SetClose.TextColor3 = Color3.fromRGB(255,70,70); SetClose.MouseButton1Click:Connect(function() SettingsFrame:TweenSize(UDim2.new(0,0,0,0), "In", "Quad", 0.3, true, function() SettingsFrame.Visible = false end) end)

RunService.RenderStepped:Connect(function() ToggleGradient.Rotation = tick() * 120 end)
print("ABC ULTIMATE HUB - ALL LINKS ACTIVE")
