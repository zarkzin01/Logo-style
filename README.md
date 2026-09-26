-- ═══════════════════════════════════════════════════════════════
-- Logo + Painel UI (versão PC)
-- Feito por LZINN
-- Delta / Xeno / LocalScript
-- ═══════════════════════════════════════════════════════════════

local Players = game:GetService("Players")
local UserInputService = game:GetService("UserInputService")
local ContentProvider = game:GetService("ContentProvider")

local player = Players.LocalPlayer
local ASSET_ID = 121036642893138

local function getUIParent()
	local ok, hui = pcall(function()
		return gethui()
	end)
	if ok and hui then return hui end
	local ok2, cg = pcall(function()
		return game:GetService("CoreGui")
	end)
	if ok2 and cg then return cg end
	return player:WaitForChild("PlayerGui")
end

local uiParent = getUIParent()

local Config = {
	Transparency = 0.1,
	SizePC = 330,
	OffsetX = 0,
	OffsetY = 28,
	Visible = true,
	MoveStep = 10,
}

local sizeMultiplier = 1

pcall(function()
	local old = uiParent:FindFirstChild("CityLogoWatermark")
	if old then old:Destroy() end
end)

local sg = Instance.new("ScreenGui")
sg.Name = "CityLogoWatermark"
sg.ResetOnSpawn = false
sg.IgnoreGuiInset = true
sg.DisplayOrder = 200
sg.ZIndexBehavior = Enum.ZIndexBehavior.Sibling
sg.Parent = uiParent

pcall(function()
	if syn and syn.protect_gui then syn.protect_gui(sg) end
end)

-- LOGO
local logo = Instance.new("ImageLabel")
logo.Name = "Logo"
logo.BackgroundTransparency = 1
logo.Image = "rbxassetid://" .. tostring(ASSET_ID)
logo.ImageTransparency = Config.Transparency
logo.ScaleType = Enum.ScaleType.Fit
logo.AnchorPoint = Vector2.new(0.5, 0)
logo.ZIndex = 5
logo.Parent = sg

local function applyLogo()
	local size = math.floor(Config.SizePC * sizeMultiplier)
	logo.Size = UDim2.fromOffset(size, size)
	logo.Position = UDim2.new(0.5, Config.OffsetX, 0, Config.OffsetY)
	logo.ImageTransparency = Config.Transparency
	logo.Visible = Config.Visible
end

applyLogo()

task.spawn(function()
	pcall(function()
		ContentProvider:PreloadAsync({ logo })
	end)
	task.wait(0.5)
	if not logo.IsLoaded then
		logo.Image = "rbxthumb://type=Asset&id=" .. tostring(ASSET_ID) .. "&w=420&h=420"
	end
end)

local function corner(o, r)
	local c = Instance.new("UICorner")
	c.CornerRadius = UDim.new(0, r or 10)
	c.Parent = o
end

-- BOTÃO ABRIR (esquerda no PC)
local openBtn = Instance.new("TextButton")
openBtn.Size = UDim2.fromOffset(50, 50)
openBtn.Position = UDim2.new(0, 14, 0.4, 0)
openBtn.BackgroundColor3 = Color3.fromRGB(255, 180, 40)
openBtn.Text = "LOGO"
openBtn.TextSize = 12
openBtn.Font = Enum.Font.GothamBold
openBtn.TextColor3 = Color3.fromRGB(40, 25, 0)
openBtn.AutoButtonColor = false
openBtn.ZIndex = 30
openBtn.Parent = sg
corner(openBtn, 12)

local openStroke = Instance.new("UIStroke")
openStroke.Color = Color3.fromRGB(255, 220, 100)
openStroke.Thickness = 2
openStroke.Parent = openBtn

-- PAINEL
local panel = Instance.new("Frame")
panel.Size = UDim2.fromOffset(300, 380)
panel.Position = UDim2.new(0, 74, 0.4, -40)
panel.BackgroundColor3 = Color3.fromRGB(250, 250, 253)
panel.Visible = false
panel.ClipsDescendants = true
panel.ZIndex = 25
panel.Parent = sg
corner(panel, 16)

local panelStroke = Instance.new("UIStroke")
panelStroke.Color = Color3.fromRGB(210, 210, 220)
panelStroke.Thickness = 1
panelStroke.Parent = panel

local header = Instance.new("Frame")
header.Size = UDim2.new(1, 0, 0, 44)
header.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
header.BackgroundTransparency = 0.2
header.BorderSizePixel = 0
header.ZIndex = 26
header.Parent = panel
corner(header, 16)

local hFix = Instance.new("Frame")
hFix.Size = UDim2.new(1, 0, 0, 14)
hFix.Position = UDim2.new(0, 0, 1, -14)
hFix.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
hFix.BackgroundTransparency = 0.2
hFix.BorderSizePixel = 0
hFix.ZIndex = 26
hFix.Parent = header

local title = Instance.new("TextLabel")
title.Size = UDim2.new(1, -50, 1, 0)
title.Position = UDim2.fromOffset(14, 0)
title.BackgroundTransparency = 1
title.Text = "LOGO"
title.Font = Enum.Font.GothamBold
title.TextSize = 14
title.TextColor3 = Color3.fromRGB(35, 35, 45)
title.TextXAlignment = Enum.TextXAlignment.Left
title.ZIndex = 27
title.Parent = header

local closeBtn = Instance.new("TextButton")
closeBtn.Size = UDim2.fromOffset(28, 28)
closeBtn.Position = UDim2.new(1, -36, 0.5, -14)
closeBtn.BackgroundColor3 = Color3.fromRGB(240, 240, 245)
closeBtn.Text = "X"
closeBtn.TextSize = 13
closeBtn.Font = Enum.Font.GothamBold
closeBtn.TextColor3 = Color3.fromRGB(100, 100, 110)
closeBtn.AutoButtonColor = false
closeBtn.ZIndex = 27
closeBtn.Parent = header
corner(closeBtn, 8)

local content = Instance.new("ScrollingFrame")
content.Size = UDim2.new(1, -16, 1, -74)
content.Position = UDim2.fromOffset(8, 48)
content.BackgroundTransparency = 1
content.BorderSizePixel = 0
content.ScrollBarThickness = 4
content.ScrollBarImageColor3 = Color3.fromRGB(80, 120, 255)
content.AutomaticCanvasSize = Enum.AutomaticSize.Y
content.CanvasSize = UDim2.new(0, 0, 0, 0)
content.ZIndex = 26
content.Parent = panel

local lay = Instance.new("UIListLayout")
lay.Padding = UDim.new(0, 10)
lay.SortOrder = Enum.SortOrder.LayoutOrder
lay.Parent = content

local pad = Instance.new("UIPadding")
pad.PaddingTop = UDim.new(0, 4)
pad.PaddingBottom = UDim.new(0, 10)
pad.PaddingLeft = UDim.new(0, 4)
pad.PaddingRight = UDim.new(0, 4)
pad.Parent = content

local function makeSlider(name, min, max, default, order, callback)
	local frame = Instance.new("Frame")
	frame.Size = UDim2.new(1, 0, 0, 50)
	frame.BackgroundColor3 = Color3.fromRGB(245, 245, 250)
	frame.LayoutOrder = order
	frame.ZIndex = 26
	frame.Parent = content
	corner(frame, 10)

	local label = Instance.new("TextLabel")
	label.Size = UDim2.new(1, -70, 0, 16)
	label.Position = UDim2.fromOffset(12, 6)
	label.BackgroundTransparency = 1
	label.Text = name
	label.Font = Enum.Font.Gotham
	label.TextSize = 11
	label.TextColor3 = Color3.fromRGB(100, 100, 115)
	label.TextXAlignment = Enum.TextXAlignment.Left
	label.ZIndex = 27
	label.Parent = frame

	local valueLbl = Instance.new("TextLabel")
	valueLbl.Size = UDim2.fromOffset(50, 16)
	valueLbl.Position = UDim2.new(1, -58, 0, 6)
	valueLbl.BackgroundTransparency = 1
	valueLbl.Text = string.format("%.2f", default)
	valueLbl.Font = Enum.Font.GothamBold
	valueLbl.TextSize = 11
	valueLbl.TextColor3 = Color3.fromRGB(50, 50, 60)
	valueLbl.TextXAlignment = Enum.TextXAlignment.Right
	valueLbl.ZIndex = 27
	valueLbl.Parent = frame

	local bar = Instance.new("Frame")
	bar.Size = UDim2.new(1, -24, 0, 8)
	bar.Position = UDim2.fromOffset(12, 30)
	bar.BackgroundColor3 = Color3.fromRGB(220, 220, 230)
	bar.ZIndex = 27
	bar.Parent = frame
	corner(bar, 4)

	local fill = Instance.new("Frame")
	fill.BackgroundColor3 = Color3.fromRGB(80, 120, 255)
	fill.Size = UDim2.new((default - min) / math.max(max - min, 0.001), 0, 1, 0)
	fill.ZIndex = 28
	fill.Parent = bar
	corner(fill, 4)

	local drag = false
	local function set(v)
		v = math.clamp(v, min, max)
		local p = (v - min) / math.max(max - min, 0.001)
		fill.Size = UDim2.new(p, 0, 1, 0)
		valueLbl.Text = string.format("%.2f", v)
		callback(v)
	end
	set(default)

	bar.InputBegan:Connect(function(input)
		if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then
			drag = true
			local r = math.clamp((input.Position.X - bar.AbsolutePosition.X) / math.max(bar.AbsoluteSize.X, 1), 0, 1)
			set(min + r * (max - min))
		end
	end)
	UserInputService.InputChanged:Connect(function(input)
		if drag and (input.UserInputType == Enum.UserInputType.MouseMovement or input.UserInputType == Enum.UserInputType.Touch) then
			local r = math.clamp((input.Position.X - bar.AbsolutePosition.X) / math.max(bar.AbsoluteSize.X, 1), 0, 1)
			set(min + r * (max - min))
		end
	end)
	UserInputService.InputEnded:Connect(function(input)
		if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then
			drag = false
		end
	end)
end

makeSlider("Tamanho", 0.4, 1.8, 1, 1, function(v)
	sizeMultiplier = v
	applyLogo()
end)

makeSlider("Transparencia", 0, 0.9, Config.Transparency, 2, function(v)
	Config.Transparency = v
	applyLogo()
end)

-- POSIÇÃO
local posCard = Instance.new("Frame")
posCard.Size = UDim2.new(1, 0, 0, 130)
posCard.BackgroundColor3 = Color3.fromRGB(245, 245, 250)
posCard.LayoutOrder = 3
posCard.ZIndex = 26
posCard.Parent = content
corner(posCard, 10)

local posTitle = Instance.new("TextLabel")
posTitle.Size = UDim2.new(1, -16, 0, 18)
posTitle.Position = UDim2.fromOffset(12, 6)
posTitle.BackgroundTransparency = 1
posTitle.Text = "Posicao"
posTitle.Font = Enum.Font.GothamBold
posTitle.TextSize = 11
posTitle.TextColor3 = Color3.fromRGB(100, 100, 115)
posTitle.TextXAlignment = Enum.TextXAlignment.Left
posTitle.ZIndex = 27
posTitle.Parent = posCard

local posLbl = Instance.new("TextLabel")
posLbl.Size = UDim2.new(1, -16, 0, 14)
posLbl.Position = UDim2.fromOffset(12, 24)
posLbl.BackgroundTransparency = 1
posLbl.Text = "X: 0  |  Y: 28"
posLbl.Font = Enum.Font.Gotham
posLbl.TextSize = 10
posLbl.TextColor3 = Color3.fromRGB(130, 130, 145)
posLbl.TextXAlignment = Enum.TextXAlignment.Left
posLbl.ZIndex = 27
posLbl.Parent = posCard

local function updatePosLabel()
	posLbl.Text = string.format("X: %d  |  Y: %d", Config.OffsetX, Config.OffsetY)
end

local function makeArrow(text, x, y, dx, dy)
	local b = Instance.new("TextButton")
	b.Size = UDim2.fromOffset(42, 34)
	b.Position = UDim2.new(0.5, x, 0, y)
	b.BackgroundColor3 = Color3.fromRGB(80, 120, 255)
	b.Text = text
	b.TextSize = 16
	b.Font = Enum.Font.GothamBold
	b.TextColor3 = Color3.new(1, 1, 1)
	b.AutoButtonColor = false
	b.ZIndex = 28
	b.Parent = posCard
	corner(b, 8)
	b.MouseButton1Click:Connect(function()
		Config.OffsetX = Config.OffsetX + dx
		Config.OffsetY = math.max(0, Config.OffsetY + dy)
		updatePosLabel()
		applyLogo()
	end)
end

makeArrow("^", -21, 46, 0, -Config.MoveStep)
makeArrow("<", -68, 84, -Config.MoveStep, 0)
makeArrow(">", 26, 84, Config.MoveStep, 0)
makeArrow("v", -21, 84, 0, Config.MoveStep)

local resetPos = Instance.new("TextButton")
resetPos.Size = UDim2.fromOffset(48, 34)
resetPos.Position = UDim2.new(0.5, -24, 0, 46)
resetPos.BackgroundColor3 = Color3.fromRGB(200, 200, 210)
resetPos.Text = "RST"
resetPos.TextSize = 11
resetPos.Font = Enum.Font.GothamBold
resetPos.TextColor3 = Color3.fromRGB(50, 50, 60)
resetPos.AutoButtonColor = false
resetPos.ZIndex = 28
resetPos.Parent = posCard
corner(resetPos, 8)
resetPos.MouseButton1Click:Connect(function()
	Config.OffsetX = 0
	Config.OffsetY = 28
	updatePosLabel()
	applyLogo()
end)

local toggleRow = Instance.new("TextButton")
toggleRow.Size = UDim2.new(1, 0, 0, 36)
toggleRow.BackgroundColor3 = Color3.fromRGB(80, 120, 255)
toggleRow.Text = "OCULTAR LOGO"
toggleRow.Font = Enum.Font.GothamBold
toggleRow.TextSize = 12
toggleRow.TextColor3 = Color3.new(1, 1, 1)
toggleRow.AutoButtonColor = false
toggleRow.LayoutOrder = 4
toggleRow.ZIndex = 26
toggleRow.Parent = content
corner(toggleRow, 9)

toggleRow.MouseButton1Click:Connect(function()
	Config.Visible = not Config.Visible
	applyLogo()
	toggleRow.Text = Config.Visible and "OCULTAR LOGO" or "MOSTRAR LOGO"
	toggleRow.BackgroundColor3 = Config.Visible and Color3.fromRGB(80, 120, 255) or Color3.fromRGB(100, 100, 110)
end)

local creditsBar = Instance.new("Frame")
creditsBar.Size = UDim2.new(1, 0, 0, 22)
creditsBar.Position = UDim2.new(0, 0, 1, -22)
creditsBar.BackgroundColor3 = Color3.fromRGB(240, 240, 245)
creditsBar.BorderSizePixel = 0
creditsBar.ZIndex = 30
creditsBar.Parent = panel

local creditsText = Instance.new("TextLabel")
creditsText.Size = UDim2.new(1, 0, 1, 0)
creditsText.BackgroundTransparency = 1
creditsText.Text = "Feito por LZINN"
creditsText.Font = Enum.Font.Gotham
creditsText.TextSize = 11
creditsText.TextColor3 = Color3.fromRGB(130, 130, 145)
creditsText.ZIndex = 31
creditsText.Parent = creditsBar

-- arrastar
local function makeDraggable(frame, handle)
	local dragging, dragStart, startPos = false, nil, nil
	handle = handle or frame
	handle.InputBegan:Connect(function(input)
		if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then
			dragging = true
			dragStart = input.Position
			startPos = frame.Position
		end
	end)
	UserInputService.InputChanged:Connect(function(input)
		if dragging and (input.UserInputType == Enum.UserInputType.MouseMovement or input.UserInputType == Enum.UserInputType.Touch) then
			local d = input.Position - dragStart
			frame.Position = UDim2.new(startPos.X.Scale, startPos.X.Offset + d.X, startPos.Y.Scale, startPos.Y.Offset + d.Y)
		end
	end)
	UserInputService.InputEnded:Connect(function(input)
		if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then
			dragging = false
		end
	end)
end

makeDraggable(openBtn)
makeDraggable(panel, header)

local clickStart = nil
openBtn.InputBegan:Connect(function(input)
	if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then
		clickStart = input.Position
	end
end)
openBtn.InputEnded:Connect(function(input)
	if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then
		if clickStart and (input.Position - clickStart).Magnitude < 12 then
			panel.Visible = not panel.Visible
		end
		clickStart = nil
	end
end)

closeBtn.MouseButton1Click:Connect(function()
	panel.Visible = false
end)

print("[City Logo] Versao PC | Feito por LZINN")
