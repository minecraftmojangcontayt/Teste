-- Hub Tester (Delta Friendly)

local UIS = game:GetService("UserInputService")
local Players = game:GetService("Players")
local LP = Players.LocalPlayer
local RS = game:GetService("RunService")

-- GUI simples
local gui = Instance.new("ScreenGui", game.CoreGui)
gui.Name = "HubTester"

local frame = Instance.new("Frame", gui)
frame.Size = UDim2.new(0, 260, 0, 320)
frame.Position = UDim2.new(0.5, -130, 0.5, -160)
frame.BackgroundColor3 = Color3.fromRGB(30,30,30)
frame.Active = true
frame.Draggable = true

local UICorner = Instance.new("UICorner", frame)
UICorner.CornerRadius = UDim.new(0, 12)

local title = Instance.new("TextLabel", frame)
title.Text = "Hub Tester (Delta Safe)"
title.Size = UDim2.new(1, 0, 0, 30)
title.TextColor3 = Color3.new(1,1,1)
title.Font = Enum.Font.GothamBold
title.TextSize = 16
title.BackgroundTransparency = 1

-- Botão padrão
local function createButton(text, yPos, callback)
	local btn = Instance.new("TextButton", frame)
	btn.Size = UDim2.new(0, 220, 0, 30)
	btn.Position = UDim2.new(0, 20, 0, yPos)
	btn.Text = text.." OFF"
	btn.BackgroundColor3 = Color3.fromRGB(80,80,80)
	btn.TextColor3 = Color3.new(1,1,1)
	btn.Font = Enum.Font.Gotham
	btn.TextSize = 14
	local on = false

	local corner = Instance.new("UICorner", btn)
	corner.CornerRadius = UDim.new(0, 8)

	btn.MouseButton1Click:Connect(function()
		on = not on
		btn.Text = text.." "..(on and "ON" or "OFF")
		btn.BackgroundColor3 = on and Color3.fromRGB(0, 200, 0) or Color3.fromRGB(80,80,80)
		callback(on)
	end)
end

-- Infinite Jump
_G.InfJump = false
createButton("Infinite Jump", 40, function(state)
	_G.InfJump = state
end)

UIS.JumpRequest:Connect(function()
	if _G.InfJump then
		LP.Character:FindFirstChildOfClass("Humanoid"):ChangeState("Jumping")
	end
end)

-- Speed Boost
createButton("Speed Boost", 80, function(state)
	local h = LP.Character:FindFirstChild("Humanoid")
	if h then
		h.WalkSpeed = state and 100 or 16
	end
end)

-- Jump Boost
createButton("Jump Boost", 120, function(state)
	local h = LP.Character:FindFirstChild("Humanoid")
	if h then
		h.JumpPower = state and 120 or 50
	end
end)

-- NoClip
local noclip = false
createButton("Noclip", 160, function(state)
	noclip = state
end)

RS.Stepped:Connect(function()
	if noclip and LP.Character then
		for _,v in pairs(LP.Character:GetDescendants()) do
			if v:IsA("BasePart") and v.CanCollide then
				v.CanCollide = false
			end
		end
	end
end)

-- Rainbow Body
local rainbow = false
createButton("Rainbow Body", 200, function(state)
	rainbow = state
	if state then
		spawn(function()
			while rainbow do
				if LP.Character then
					local hue = tick() % 5 / 5
					local color = Color3.fromHSV(hue, 1, 1)
					for _, part in ipairs(LP.Character:GetChildren()) do
						if part:IsA("BasePart") then
							part.Color = color
						end
					end
				end
				wait(0.2)
			end
		end)
	end
end)

-- Créditos
local credits = Instance.new("TextLabel", frame)
credits.Text = "Feito por Davi – ChatGPT"
credits.Size = UDim2.new(1, 0, 0, 30)
credits.Position = UDim2.new(0, 0, 1, -30)
credits.BackgroundTransparency = 1
credits.TextColor3 = Color3.fromRGB(200,200,200)
credits.Font = Enum.Font.Gotham
credits.TextSize = 12
