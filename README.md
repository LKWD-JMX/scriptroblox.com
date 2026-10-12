local P = game:GetService("Players")
local R = game:GetService("RunService")
local LP = game:GetService("Players").LocalPlayer
local C = workspace.CurrentCamera
local A = false
local AW = false
local E = false
local EL = false
local S = false
local N = false
local Prt = "Head"
local SM = 0.25
local FOV = 130
local V = 100
local LW = 2
local isS = false
local min = false
local sAct = false
local sAct2 = false

local g = Instance.new("ScreenGui", LP:WaitForChild("PlayerGui"))
g.ResetOnSpawn = false

local F = Instance.new("Frame", g)
F.Size = UDim2.new(0, 160, 0, 240)
F.Position = UDim2.new(1, -180, 0.25, 0)
F.BackgroundColor3 = Color3.fromRGB(15, 15, 15)
F.BackgroundTransparency = 0.25
Instance.new("UICorner", F).CornerRadius = UDim.new(0, 12)

local SFr = Instance.new("ScrollingFrame", F)
SFr.Size = UDim2.new(1, -10, 1, -35)
SFr.Position = UDim2.new(0, 5, 0, 15)
SFr.BackgroundTransparency = 1
SFr.CanvasSize = UDim2.new(0, 0, 0, 0)
SFr.ScrollBarThickness = 4
SFr.ScrollingDirection = Enum.ScrollingDirection.Y

local L = Instance.new("UIListLayout", SFr)
L.FillDirection = Enum.FillDirection.Vertical
L.SortOrder = Enum.SortOrder.LayoutOrder
L.Padding = UDim.new(0, 8)

L:GetPropertyChangedSignal("AbsoluteContentSize"):Connect(function()
    SFr.CanvasSize = UDim2.new(0, 0, 0, L.AbsoluteContentSize.Y + 10)
end)

local cls = Instance.new("TextButton", g)
cls.Size = UDim2.new(0, 30, 0, 30)
cls.Text = "X"
cls.BackgroundColor3 = Color3.fromRGB(200, 40, 40)
cls.TextColor3 = Color3.fromRGB(255, 255, 255)
cls.Font = Enum.Font.GothamBold
cls.TextSize = 14
Instance.new("UICorner", cls).CornerRadius = UDim.new(0, 8)

local function upC()
    if not min then
        cls.Position = UDim2.new(F.Position.X.Scale, F.Position.X.Offset + F.Size.X.Offset - 35, F.Position.Y.Scale, F.Position.Y.Offset + 5)
    end
end

cls.MouseButton1Click:Connect(function()
    min = not min
    F.Visible = not min
    cls.Text = min and "☰" or "X"
    cls.BackgroundColor3 = min and Color3.fromRGB(40, 40, 40) or Color3.fromRGB(200, 40, 40)
end)

local drag, dSt, sPos
F.InputBegan:Connect(function(i)
    if i.UserInputType == Enum.UserInputType.MouseButton1 or i.UserInputType == Enum.UserInputType.Touch then
        drag, dSt, sPos = true, i.Position, F.Position
        i.Changed:Connect(function()
            if i.UserInputState == Enum.UserInputState.End then
                drag = false
            end
        end)
    end
end)

F.InputChanged:Connect(function(i)
    if drag and (i.UserInputType == Enum.UserInputType.MouseMovement or i.UserInputType == Enum.UserInputType.Touch) then
        local d = i.Position - dSt
        F.Position = UDim2.new(sPos.X.Scale, sPos.X.Offset + d.X, sPos.Y.Scale, sPos.Y.Offset + d.Y)
        upC()
    end
end)
upC()

local function cB(t, o)
    local b = Instance.new("TextButton", SFr)
    b.Size = UDim2.new(1, -10, 0, 35)
    b.Text = t
    b.BackgroundColor3 = Color3.fromRGB(25, 25, 25)
    b.TextColor3 = Color3.fromRGB(255, 255, 255)
    b.Font = Enum.Font.GothamSemibold
    b.TextSize = 14
    b.LayoutOrder = o
    Instance.new("UICorner", b).CornerRadius = UDim.new(0, 8)
    return b
end

local function cSl(t, o, pD, fU)
    local f = Instance.new("Frame", SFr)
    f.Size = UDim2.new(1, -10, 0, 45)
    f.BackgroundColor3 = Color3.fromRGB(25, 25, 25)
    f.LayoutOrder = o
    Instance.new("UICorner", f).CornerRadius = UDim.new(0, 8)
    
    local l = Instance.new("TextLabel", f)
    l.Size = UDim2.new(1, 0, 0, 20)
    l.BackgroundTransparency = 1
    l.Text = t
    l.TextColor3 = Color3.fromRGB(255, 255, 255)
    l.Font = Enum.Font.GothamSemibold
    l.TextSize = 11
    
    local tr = Instance.new("Frame", f)
    tr.Size = UDim2.new(0.85, 0, 0, 6)
    tr.Position = UDim2.new(0.075, 0, 0.6, 0)
    tr.BackgroundColor3 = Color3.fromRGB(50, 50, 50)
    
    local btn = Instance.new("TextButton", tr)
    btn.Size = UDim2.new(0, 16, 0, 16)
    btn.Position = UDim2.new(pD, 0, 0.5, -8)
    btn.BackgroundColor3 = Color3.fromRGB(200, 40, 40)
    btn.Text = ""
    Instance.new("UICorner", btn).CornerRadius = UDim.new(1, 0)
    
    btn.InputBegan:Connect(function(i)
        if i.UserInputType == Enum.UserInputType.MouseButton1 or i.UserInputType == Enum.UserInputType.Touch then
            fU(true)
        end
    end)
    return tr, btn, l
end

local bA = cB("AIM VISIBLE OFF", 1)
local bW = cB("AIM WALLS OFF", 2)
local bP = cB("HEAD", 4)
local bE = cB("ESP OFF", 5)
local bEL = cB("ESP LINE OFF", 6)
local sT2, sB2, sL2 = cSl("GROSOR LINEA: 2", 7, 0.11, function(v) sAct2 = v end)
local bN = cB("NOCLIP OFF", 8)
local bS = cB("SPD OFF", 9)
local sT, sB, sL = cSl("VELOCIDAD: 100", 10, 0.33, function(v) sAct = v end)

game:GetService("UserInputService").InputEnded:Connect(function(i)
    if i.UserInputType == Enum.UserInputType.MouseButton1 or i.UserInputType == Enum.UserInputType.Touch then
        sAct, sAct2, isS = false, false, false
    end
end)

game:GetService("UserInputService").InputChanged:Connect(function(i)
    if sAct and (i.UserInputType == Enum.UserInputType.MouseMovement or i.UserInputType == Enum.UserInputType.Touch) then
        local p = math.clamp((i.Position.X - sT.AbsolutePosition.X) / sT.AbsoluteSize.X, 0, 1)
        sB.Position = UDim2.new(p, 0, 0.5, -8)
        V = math.floor(p * 300)
        sL.Text = "VELOCIDAD: " .. tostring(V)
    elseif sAct2 and (i.UserInputType == Enum.UserInputType.MouseMovement or i.UserInputType == Enum.UserInputType.Touch) then
        local p = math.clamp((i.Position.X - sT2.AbsolutePosition.X) / sT2.AbsoluteSize.X, 0, 1)
        sB2.Position = UDim2.new(p, 0, 0.5, -8)
        LW = math.floor(p * 9) + 1
        sL2.Text = "GROSOR LINEA: " .. tostring(LW)
    end
end)

game:GetService("UserInputService").InputBegan:Connect(function(i, gP)
    if not gP and (i.UserInputType == Enum.UserInputType.MouseButton1 or i.UserInputType == Enum.UserInputType.Touch) then
        isS = true
    end
end)local function IsV(t)
    if not LP.Character then return false end
    local rp = RaycastParams.new()
    local f = {LP.Character, C}
    if LP.Character:FindFirstChildOfClass("Tool") then
        table.insert(f, LP.Character:FindFirstChildOfClass("Tool"))
    end
    rp.FilterDescendantsInstances, rp.FilterType = f, Enum.RaycastFilterType.Exclude
    local res = workspace:Raycast(C.CFrame.Position, (t.Position - C.CFrame.Position).Unit * (t.Position - C.CFrame.Position).Magnitude, rp)
    return not res or res.Instance:IsDescendantOf(t.Parent)
end

local function GetC(iw)
    local cl, sh = nil, FOV
    local cen = Vector2.new(C.ViewportSize.X / 2, C.ViewportSize.Y / 2)
    for _, p in ipairs(P:GetPlayers()) do
        if p ~= LP and (not p.Team or p.Team ~= LP.Team) then
            local ch = p.Character
            if ch and ch:FindFirstChild(Prt) and ch:FindFirstChildOfClass("Humanoid") and ch:FindFirstChildOfClass("Humanoid").Health > 0 then
                local pt = ch[Prt]
                local pos, on = C:WorldToViewportPoint(pt.Position)
                if on and (iw or IsV(pt)) then
                    local d = (Vector2.new(pos.X, pos.Y) - cen).Magnitude
                    if d < sh then
                        cl, sh = pt, d
                    end
                end
            end
        end
    end
    return cl
end

bA.MouseButton1Click:Connect(function()
    A = not A
    bA.Text = A and "AIM VISIBLE ON" or "AIM VISIBLE OFF"
    bA.BackgroundColor3 = A and Color3.fromRGB(40, 120, 40) or Color3.fromRGB(25, 25, 25)
end)

bW.MouseButton1Click:Connect(function()
    AW = not AW
    bW.Text = AW and "AIM WALLS ON" or "AIM WALLS OFF"
    bW.BackgroundColor3 = AW and Color3.fromRGB(40, 120, 40) or Color3.fromRGB(25, 25, 25)
end)

bP.MouseButton1Click:Connect(function()
    Prt = Prt == "Head" and "HumanoidRootPart" or "Head"
    bP.Text = Prt == "Head" and "HEAD" or "TORSO"
end)

bE.MouseButton1Click:Connect(function()
    E = not E
    bE.Text = E and "ESP ON" or "ESP OFF"
    bE.BackgroundColor3 = E and Color3.fromRGB(40, 120, 40) or Color3.fromRGB(25, 25, 25)
    if not E then
        for _, p in ipairs(P:GetPlayers()) do
            if p.Character then
                if p.Character:FindFirstChildOfClass("Highlight") then
                    p.Character:FindFirstChildOfClass("Highlight"):Destroy()
                end
                if p.Character:FindFirstChild("E_Text") then
                    p.Character.E_Text:Destroy()
                end
            end
        end
    end
end)

bEL.MouseButton1Click:Connect(function()
    EL = not EL
    bEL.Text = EL and "ESP LINE ON" or "ESP LINE OFF"
    bEL.BackgroundColor3 = EL and Color3.fromRGB(40, 120, 40) or Color3.fromRGB(25, 25, 25)
    if not EL then
        for _, p in ipairs(P:GetPlayers()) do
            local old = g:FindFirstChild("Line_" .. p.Name)
            if old then
                old:Destroy()
            end
        end
    end
end)

bN.MouseButton1Click:Connect(function()
    N = not N
    bN.Text = N and "NOCLIP ON" or "NOCLIP OFF"
    bN.BackgroundColor3 = N and Color3.fromRGB(40, 120, 40) or Color3.fromRGB(25, 25, 25)
end)

bS.MouseButton1Click:Connect(function()
    S = not S
    bS.Text = S and "SPD ON" or "SPD OFF"
    bS.BackgroundColor3 = S and Color3.fromRGB(40, 120, 40) or Color3.fromRGB(25, 25, 25)
end)

R.RenderStepped:Connect(function()
    if A or AW then
        local t = GetC(AW)
        if t then
            C.CFrame = C.CFrame:Lerp(CFrame.new(C.CFrame.Position, t.Position), SM)
        end
    end
    if E then
        for _, p in ipairs(P:GetPlayers()) do
            if p ~= LP and p.Character and p.Character:FindFirstChild("Head") then
                local isEn = (not p.Team or p.Team ~= LP.Team)
                local hl = p.Character:FindFirstChildOfClass("Highlight")
                if not hl then
                    hl = Instance.new("Highlight", p.Character)
                    hl.OutlineColor, hl.FillTransparency, hl.DepthMode = Color3.fromRGB(255, 255, 255), 0.5, Enum.HighlightDepthMode.AlwaysOnTop
                end
                hl.FillColor = isEn and Color3.fromRGB(255, 0, 0) or Color3.fromRGB(0, 255, 0)
                if isEn and p.Character:FindFirstChildOfClass("Humanoid") then
                    local txt = p.Character:FindFirstChild("E_Text")
                    if not txt then
                        txt = Instance.new("BillboardGui", p.Character)
                        txt.Name, txt.Size, txt.AlwaysOnTop, txt.Adornee = "E_Text", UDim2.new(0, 100, 0, 20), true, p.Character.Head
                        txt.StudsOffset = Vector3.new(0, 2, 0)
                        local lbl = Instance.new("TextLabel", txt)
                        lbl.Size, lbl.BackgroundTransparency, lbl.TextColor3, lbl.TextSize, lbl.Font = UDim2.new(1, 0, 1, 0), 1, Color3.fromRGB(255, 40, 40), 8, Enum.Font.GothamBold
                    end
                    local dist = math.floor((LP.Character and LP.Character:FindFirstChild("HumanoidRootPart") and (LP.Character.HumanoidRootPart.Position - p.Character.HumanoidRootPart.Position).Magnitude) or 0)
                    txt.TextLabel.Text = p.Name .. " [" .. math.floor(p.Character:FindFirstChildOfClass("Humanoid").Health) .. " HP] (" .. dist .. "m)"
                else
                    if p.Character:FindFirstChild("E_Text") then
                        p.Character.E_Text:Destroy()
                    end
                end
            end
        end
    end
    for _, p in ipairs(P:GetPlayers()) do
        local old = g:FindFirstChild("Line_" .. p.Name)
        if old then
            old:Destroy()
        end
        if EL and p ~= LP and p.Character and p.Character:FindFirstChild("HumanoidRootPart") and p.Character:FindFirstChildOfClass("Humanoid") and p.Character:FindFirstChildOfClass("Humanoid").Health > 0 then
            local pos, on = C:WorldToViewportPoint(p.Character.HumanoidRootPart.Position + Vector3.new(0, -0.5, 0))
            if on then
                local fr = Instance.new("Frame", g)
                fr.Name, fr.AnchorPoint, fr.BorderSizePixel, fr.BackgroundColor3 = "Line_" .. p.Name, Vector2.new(0.5, 0.5), 0, ((p.Team and p.Team == LP.Team) and Color3.fromRGB(0, 255, 0) or Color3.fromRGB(255, 0, 0))
                local sX, sY = C.ViewportSize.X / 2, C.ViewportSize.Y
                local mC = LP.Character
                if mC and mC:FindFirstChild("HumanoidRootPart") then
                    fr.Size = UDim2.new(0, (Vector2.new(pos.X, pos.Y) - Vector2.new(sX, sY)).Magnitude, 0, LW)
                    fr.Position = UDim2.new(0, (pos.X + sX) / 2, 0, (pos.Y + sY) / 2)
                    fr.Rotation = math.deg(math.atan2(pos.Y - sY, pos.X - sX))
                end
            end
        end
    end
end)
