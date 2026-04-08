# 🚀 Roblox Infinite Jump Script (Modern GUI)

A lightweight and smooth Luau script for Roblox that provides **Infinite Jump** capabilities with a modern User Interface (GUI), featuring a toggle button and smooth animations.

![Lua](https://img.shields.io/badge/Language-Lua-blue) ![Platform](https://img.shields.io/badge/Platform-Roblox-red)

## ✨ Features

- [x] **Infinite Jump Logic:** Allows continuous jumping in mid-air (Air Jump).
- [x] **Modern GUI:** Beautifully designed interface featuring:
  - Rounded Corners.
  - Drop Shadow effects.
  - Subtle Stroke/Borders.
- [x] **Smooth Animations:** Utilizes `TweenService` for seamless color transitions when toggling ON/OFF.
- [x] **GUI Toggle:** An On/Off button located at the top-left of the screen for easy access.
- [x] **Safe Injection:** Automatically inserts the GUI into `CoreGui` (if supported) or `PlayerGui` to prevent it from resetting upon character death.
- [x] **Anti-Duplicate:** Automatically removes previous GUIs when re-running the script to avoid interface overlapping.

## 📸 Preview

*(Please take an in-game screenshot while the script is running and paste the image link here, for example: `<img src="your_image_link.png" width="400">`)*

## 🛠️ How to Use

1. Open your script executor (e.g., Synapse X, Krnl, Fluxus, Delta, etc.).
2. Copy the source code provided below.
3. Paste it into the Executor and press **Execute**.
4. An **"INF JUMP: OFF"** button will appear at the top-left of your screen.
5. Click the button to enable infinite jump (the button will turn green).

## 📜 Source Code

```lua
local UserInputService = game:GetService("UserInputService")
local TweenService = game:GetService("TweenService")
local Players = game:GetService("Players")
local CoreGui = game:GetService("CoreGui")

local infiniteJumpEnabled = false
local localPlayer = Players.LocalPlayer

if CoreGui:FindFirstChild("InfJumpGUI") then
	CoreGui:FindFirstChild("InfJumpGUI"):Destroy()
end

local screenGui = Instance.new("ScreenGui")
screenGui.Name = "InfJumpGUI"
pcall(function()
	screenGui.Parent = CoreGui
end)
if not screenGui.Parent then
	screenGui.Parent = localPlayer:WaitForChild("PlayerGui")
end

local toggleButton = Instance.new("TextButton")
toggleButton.Name = "ToggleButton"
toggleButton.Parent = screenGui
toggleButton.BackgroundColor3 = Color3.fromRGB(35, 35, 35)
toggleButton.Position = UDim2.new(0, 20, 0, 50)
toggleButton.Size = UDim2.new(0, 150, 0, 50)
toggleButton.Font = Enum.Font.GothamBold
toggleButton.Text = "INF JUMP: OFF"
toggleButton.TextColor3 = Color3.fromRGB(255, 100, 100)
toggleButton.TextSize = 16
toggleButton.AutoButtonColor = false

local uiCorner = Instance.new("UICorner")
uiCorner.CornerRadius = UDim.new(0, 12)
uiCorner.Parent = toggleButton

local uiStroke = Instance.new("UIStroke")
uiStroke.Parent = toggleButton
uiStroke.Color = Color3.fromRGB(60, 60, 60)
uiStroke.Thickness = 2
uiStroke.ApplyStrokeMode = Enum.ApplyStrokeMode.Border

local shadow = Instance.new("ImageLabel")
shadow.Name = "Shadow"
shadow.Parent = toggleButton
shadow.BackgroundTransparency = 1
shadow.Position = UDim2.new(0, -15, 0, -15)
shadow.Size = UDim2.new(1, 30, 1, 30)
shadow.ZIndex = 0
shadow.Image = "rbxassetid://5554236805"
shadow.ImageColor3 = Color3.fromRGB(0, 0, 0)
shadow.ScaleType = Enum.ScaleType.Slice
shadow.SliceCenter = Rect.new(23, 23, 277, 277)
shadow.ImageTransparency = 0.4

local function updateButtonVisuals()
	local targetColor, targetTextColor, targetText, strokeColor

	if infiniteJumpEnabled then
		targetColor = Color3.fromRGB(46, 204, 113)
		targetTextColor = Color3.fromRGB(255, 255, 255)
		targetText = "INF JUMP: ON"
		strokeColor = Color3.fromRGB(39, 174, 96)
	else
		targetColor = Color3.fromRGB(35, 35, 35)
		targetTextColor = Color3.fromRGB(255, 100, 100)
		targetText = "INF JUMP: OFF"
		strokeColor = Color3.fromRGB(60, 60, 60)
	end

	TweenService:Create(toggleButton, TweenInfo.new(0.3), {BackgroundColor3 = targetColor}):Play()
	TweenService:Create(toggleButton, TweenInfo.new(0.3), {TextColor3 = targetTextColor}):Play()
	TweenService:Create(uiStroke, TweenInfo.new(0.3), {Color = strokeColor}):Play()
	toggleButton.Text = targetText
end

toggleButton.MouseButton1Click:Connect(function()
	infiniteJumpEnabled = not infiniteJumpEnabled
	updateButtonVisuals()
end)

UserInputService.JumpRequest:Connect(function()
	if infiniteJumpEnabled then
		if localPlayer.Character and localPlayer.Character:FindFirstChild("Humanoid") then
			localPlayer.Character.Humanoid:ChangeState(Enum.HumanoidStateType.Jumping)
		end
	end
end)
