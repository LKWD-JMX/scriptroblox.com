local P, R = game:GetService("Players"), game:GetService("RunService")
local LP, C = P.LocalPlayer, workspace.CurrentCamera

if not _G.MenuShared then
    _G.MenuShared = {
        A = false, AW = false, ASH = false, E = false, EL = false, S = false, N = false, Inv = false,
        Prt = "Head", SM = 0.25, FOV = 130, V = 100, LW = 2,
        isS = false, min = false, sAct = false, sAct2 = false
    }
end
local d = _G.MenuShared

local g = Instance.new("ScreenGui", LP:WaitForChild("PlayerGui"))
g.ResetOnSpawn = false
_G.MenuGui = g

local F = Instance.new("Frame", g)
F.Size, F.Position = UDim2.new(0, 160, 0, 240), UDim2.new(1, -180, 0.25, 0)
F.BackgroundColor3, F.BackgroundTransparency = Color3.fromRGB(15, 15, 15), 0.25
Instance.new("UICorner", F).CornerRadius = UDim.new(0, 12)

local SFr = Instance.new("ScrollingFrame", F)
SFr.Size, SFr.Position = UDim2.new(1, -10, 1, -35), UDim2.new(0, 5, 0, 15)
SFr.BackgroundTransparency, SFr.ScrollBarThickness, SFr.ScrollingDirection = 1, 4, Enum.ScrollingDirection.Y

local L = Instance.new("UIListLayout", SFr)
L.FillDirection, L.SortOrder, L.Padding = Enum.FillDirection.Vertical, Enum.SortOrder.LayoutOrder, UDim.new(0, 8)
L:GetPropertyChangedSignal("AbsoluteContentSize"):Connect(function()
    SFr.CanvasSize = UDim2.new(0, 0, 0, L.AbsoluteContentSize.Y + 10)
end)

local cls = Instance.new("TextButton", g)
cls.Size, cls.Text = UDim2.new(0, 30, 0, 30), "X"
cls.BackgroundColor3, cls.TextColor3 = Color3.fromRGB(200, 40, 40), Color3.fromRGB(255, 255, 255)
cls.Font, cls.TextSize = Enum.Font.GothamBold, 14
Instance.new("UICorner", cls).CornerRadius = UDim.new(0, 8)

local function upC()
    if not d.min then
        cls.Position = UDim2.new(F.Position.X.Scale, F.Position.X.Offset + F.Size.X.Offset - 35, F.Position.Y.Scale, F.Position.Y.Offset + 5)
    end
end

cls.MouseButton1Click:Connect(function()
    d.min = not d.min
    F.Visible = not d.min
    cls.Text = d.min and "☰" or "X"
    cls.BackgroundColor3 = d.min and Color3.fromRGB(40, 40, 40) or Color3.fromRGB(200, 40, 40)
end)

local drag, dSt, sPos
F.InputBegan:Connect(function(i)
    if i.UserInputType == Enum.UserInputType.MouseButton1 or i.UserInputType == Enum.UserInputType.Touch then
        drag, dSt, sPos = true, i.Position, F.Position
        i.Changed:Connect(function() if i.UserInputState == Enum.UserInputState.End then drag = false end end)
    end
end)

F.InputChanged:Connect(function(i)
    if drag and (i.UserInputType == Enum.UserInputType.MouseMovement or i.UserInputType == Enum.UserInputType.Touch) then
        local delta = i.Position - dSt
        F.Position = UDim2.new(sPos.X.Scale, sPos.X.Offset + delta.X, sPos.Y.Scale, sPos.Y.Offset + delta.Y)
        upC()
    end
end)
upC()

local function cB(t, o)
    local b = Instance.new("TextButton", SFr)
    b.Size, b.Text = UDim2.new(1, -10, 0, 35), t
    b.BackgroundColor3, b.TextColor3 = Color3.fromRGB(25, 25, 25), Color3.fromRGB(255, 255, 255)
    b.Font, b.TextSize, b.LayoutOrder = Enum.Font.GothamSemibold, 14, o
    Instance.new("UICorner", b).CornerRadius = UDim.new(0, 8)
    return b
end

local function cSl(t, o, pD, fU)
    local f = Instance.new("Frame", SFr)
    f.Size, f.BackgroundColor3, f.LayoutOrder = UDim2.new(1, -10, 0, 45), Color3.fromRGB(25, 25, 25), o
    Instance.new("UICorner", f).CornerRadius = UDim.new(0, 8)
    local l = Instance.new("TextLabel", f)
    l.Size, l.BackgroundTransparency, l.Text = UDim2.new(1, 0, 0, 20), 1, t
    l.TextColor3, l.Font, l.TextSize = Color3.fromRGB(255, 255, 255), Enum.Font.GothamSemibold, 11
    local tr = Instance.new("Frame", f)
    tr.Size, tr.Position, tr.BackgroundColor3 = UDim2.new(0.85, 0, 0, 6), UDim2.new(0.075, 0, 0.6, 0), Color3.fromRGB(50, 50, 50)
    local btn = Instance.new("TextButton", tr)
    btn.Size, btn.Position, btn.BackgroundColor3, btn.Text = UDim2.new(0, 16, 0, 16), UDim2.new(pD, 0, 0.5, -8), Color3.fromRGB(200, 40, 40), ""
    Instance.new("UICorner", btn).CornerRadius = UDim.new(1, 0)
    btn.InputBegan:Connect(function(i)
        if i.UserInputType == Enum.UserInputType.MouseButton1 or i.UserInputType == Enum.UserInputType.Touch then fU(true) end
    end)
    return tr, btn, l
end

_G.bA = cB("AIM VISIBLE OFF", 1)
_G.bW = cB("AIM WALLS OFF", 2)
_G.bSH = cB("AIM SHOOT OFF", 3)
_G.bP = cB("HEAD", 4)
_G.bE = cB("ESP OFF", 5)
_G.bEL = cB("ESP LINE OFF", 6)

local sT2, sB2, sL2 = cSl("GROSOR LINEA: 2", 7, 0.11, function(v) d.sAct2 = v end)
_G.bN = cB("NOCLIP OFF", 8)
_G.bInv = cB("INVISIBILIDAD OFF", 9)
_G.bS = cB("SPD OFF", 10)
local sT, sB, sL = cSl("VELOCIDAD: 100", 11, 0.33, function(v) d.sAct = v end)

game:GetService("UserInputService").InputEnded:Connect(function(i)
    if i.UserInputType == Enum.UserInputType.MouseButton1 or i.UserInputType == Enum.UserInputType.Touch then
        d.sAct, d.sAct2, d.isS = false, false, false
    end
end)

game:GetService("UserInputService").InputChanged:Connect(function(i)
    if d.sAct and (i.UserInputType == Enum.UserInputType.MouseMovement or i.UserInputType == Enum.UserInputType.Touch) then
        local p = math.clamp((i.Position.X - sT.AbsolutePosition.X) / sT.AbsoluteSize.X, 0, 1)
        sB.Position = UDim2.new(p, 0, 0.5, -8)
        d.V = math.floor(p * 300)
        sL.Text = "VELOCIDAD: " .. tostring(d.V)
    elseif d.sAct2 and (i.UserInputType == Enum.UserInputType.MouseMovement or i.UserInputType == Enum.UserInputType.Touch) then
        local p = math.clamp((i.Position.X - sT2.AbsolutePosition.X) / sT2.AbsoluteSize.X, 0, 1)
        sB2.Position = UDim2.new(p, 0, 0.5, -8)
        d.LW = math.floor(p * 9) + 1
        sL2.Text = "GROSOR LINEA: " .. tostring(d.LW)
    end
end)

game:GetService("UserInputService").InputBegan:Connect(function(i, gP)
    if not gP and (i.UserInputType == Enum.UserInputType.MouseButton1 or i.UserInputType == Enum.UserInputType.Touch) then d.isS = true end
end)local P, R = game:GetService("Players"), game:GetService("RunService")
local LP, C = P.LocalPlayer, workspace.CurrentCamera
local d = _G.MenuShared
local g = _G.MenuGui
local bA, bW, bSH, bP, bE, bEL, bN, bInv, bS = _G.bA, _G.bW, _G.bSH, _G.bP, _G.bE, _G.bEL, _G.bN, _G.bInv, _G.bS

local function IsV(t)
    if not LP.Character then return false end
    local rp = RaycastParams.new()
    local f = {LP.Character, C}
    if LP.Character:FindFirstChildOfClass("Tool") then table.insert(f, LP.Character:FindFirstChildOfClass("Tool")) end
    rp.FilterDescendantsInstances, rp.FilterType = f, Enum.RaycastFilterType.Exclude
    local res = workspace:Raycast(C.CFrame.Position, (t.Position - C.CFrame.Position).Unit * (t.Position - C.CFrame.Position).Magnitude, rp)
    return not res or res.Instance:IsDescendantOf(t.Parent)
end

local function GetC(iw)
    local cl, sh = nil, d.FOV
    local cen = Vector2.new(C.ViewportSize.X / 2, C.ViewportSize.Y / 2)
    for _, p in ipairs(P:GetPlayers()) do
        if p ~= LP and (not p.Team or p.Team ~= LP.Team) then
            local ch = p.Character
            if ch and ch:FindFirstChild(d.Prt) and ch:FindFirstChildOfClass("Humanoid") and ch:FindFirstChildOfClass("Humanoid").Health > 0 then
                local pt = ch[d.Prt]
                local pos, on = C:WorldToViewportPoint(pt.Position)
                if on and (iw or IsV(pt)) then
                    local dist = (Vector2.new(pos.X, pos.Y) - cen).Magnitude
                    if dist < sh then cl, sh = pt, dist end
                end
            end
        end
    end
    return cl
end

bA.MouseButton1Click:Connect(function()
    d.A = not d.A
    bA.Text = d.A and "AIM VISIBLE ON" or "AIM VISIBLE OFF"
    bA.BackgroundColor3 = d.A and Color3.fromRGB(40, 120, 40) or Color3.fromRGB(25, 25, 25)
end)

bW.MouseButton1Click:Connect(function()
    d.AW = not d.AW
    bW.Text = d.AW and "AIM WALLS ON" or "AIM WALLS OFF"
    bW.BackgroundColor3 = d.AW and Color3.fromRGB(40, 120, 40) or Color3.fromRGB(25, 25, 25)
end)

bSH.MouseButton1Click:Connect(function()
    d.ASH = not d.ASH
    bSH.Text = d.ASH and "AIM SHOOT ON" or "AIM SHOOT OFF"
    bSH.BackgroundColor3 = d.ASH and Color3.fromRGB(40, 120, 40) or Color3.fromRGB(25, 25, 25)
end)

bP.MouseButton1Click:Connect(function()
    d.Prt = d.Prt == "Head" and "HumanoidRootPart" or "Head"
    bP.Text = d.Prt == "Head" and "HEAD" or "TORSO"
end)

bE.MouseButton1Click:Connect(function()
    d.E = not d.E
    bE.Text = d.E and "ESP ON" or "ESP OFF"
    bE.BackgroundColor3 = d.E and Color3.fromRGB(40, 120, 40) or Color3.fromRGB(25, 25, 25)
    if not d.E then
        for _, p in ipairs(P:GetPlayers()) do
            if p.Character then
                if p.Character:FindFirstChildOfClass("Highlight") then p.Character:FindFirstChildOfClass("Highlight"):Destroy() end
                if p.Character:FindFirstChild("E_Text") then p.Character.E_Text:Destroy() end
            end
        end
    end
end)

bEL.MouseButton1Click:Connect(function()
    d.EL = not d.EL
    bEL.Text = d.EL and "ESP LINE ON" or "ESP LINE OFF"
    bEL.BackgroundColor3 = d.EL and Color3.fromRGB(40, 120, 40) or Color3.fromRGB(25, 25, 25)
    if not d.EL then
        for _, p in ipairs(P:GetPlayers()) do
            local old = g:FindFirstChild("Line_" .. p.Name)
            if old then old:Destroy() end
        end
    end
end)

bN.MouseButton1Click:Connect(function()
    d.N = not d.N
    bN.Text = d.N and "NOCLIP ON" or "NOCLIP OFF"
    bN.BackgroundColor3 = d.N and Color3.fromRGB(40, 120, 40) or Color3.fromRGB(25, 25, 25)
end)

bInv.MouseButton1Click:Connect(function()
    d.Inv = not d.Inv
    bInv.Text = d.Inv and "INVISIBILIDAD ON" or "INVISIBILIDAD OFF"
    bInv.BackgroundColor3 = d.Inv and Color3.fromRGB(40, 120, 40) or Color3.fromRGB(25, 25, 25)
end)

bS.MouseButton1Click:Connect(function()
    d.S = not d.S
    bS.Text = d.S and "SPD ON" or "SPD OFF"
    bS.BackgroundColor3 = d.S and Color3.fromRGB(40, 120, 40) or Color3.fromRGB(25, 25, 25)
end)

R.RenderStepped:Connect(function()
    if LP.Character and LP.Character:FindFirstChildOfClass("Humanoid") then
        local hum = LP.Character:FindFirstChildOfClass("Humanoid")
        if d.S then
            hum.WalkSpeed = d.V
        else
            hum.WalkSpeed = 16
        end
        if d.N then
            for _, p in ipairs(LP.Character:GetDescendants()) do
                if p:IsA("BasePart") and p.CanCollide then p.CanCollide = false end
            end
        end
    end
    
    if d.A or d.AW or (d.ASH and d.isS) then
        local t = GetC(d.AW)
        if t then C.CFrame = C.CFrame:Lerp(CFrame.new(C.CFrame.Position, t.Position), d.SM) end
    end
    
    if d.E then
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
                    if p.Character:FindFirstChild("E_Text") then p.Character.E_Text:Destroy() end
                end
            end
        end
    end
    
    for _, p in ipairs(P:GetPlayers()) do
        local old = g:FindFirstChild("Line_" .. p.Name)
        if old then old:Destroy() end
        if d.EL and p ~= LP and p.Character and p.Character:FindFirstChild("HumanoidRootPart") and p.Character:FindFirstChildOfClass("Humanoid") and p.Character:FindFirstChildOfClass("Humanoid").Health > 0 then
            local pos, on = C:WorldToViewportPoint(p.Character.HumanoidRootPart.Position + Vector3.new(0, -0.5, 0))
            if on then
                local fr = Instance.new("Frame", g)
                fr.Name, fr.AnchorPoint, fr.BorderSizePixel, fr.BackgroundColor3 = "Line_" .. p.Name, Vector2.new(0.5, 0.5), 0, ((p.Team and p.Team == LP.Team) and Color3.fromRGB(0, 255, 0) or Color3.fromRGB(255, 0, 0))
                local sX, sY = C.ViewportSize.X / 2, C.ViewportSize.Y
                fr.Size = UDim2.new(0, d.LW, 0, (Vector2.new(pos.X, pos.Y) - Vector2.new(sX, sY)).Magnitude)
                fr.Position = UDim2.new(0, (pos.X + sX) / 2, 0, (pos.Y + sY) / 2)
                fr.Rotation = math.deg(math.atan2(pos.Y - sY, pos.X - sX)) - 90
            end
        end
    end
end)
