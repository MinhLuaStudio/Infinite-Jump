# 🚀 Roblox Infinite Jump Script (Modern GUI)

Một script Luau nhẹ và mượt mà dành cho Roblox, cung cấp khả năng **Nhảy vô hạn (Infinite Jump)** với giao diện người dùng (GUI) hiện đại, có nút bật/tắt (Toggle) và hiệu ứng chuyển động.

![Lua](https://img.shields.io/badge/Language-Lua-blue) ![Platform](https://img.shields.io/badge/Platform-Roblox-red)

## ✨ Tính năng (Features)

- [x] **Infinite Jump Logic:** Cho phép nhảy liên tục giữa không trung (Air Jump).
- [x] **Modern GUI:** Giao diện được thiết kế đẹp mắt với:
  - Bo tròn góc (Rounded Corners).
  - Hiệu ứng bóng đổ (Drop Shadow).
  - Viền tinh tế (Stroke).
- [x] **Smooth Animations:** Sử dụng `TweenService` để chuyển đổi màu sắc mượt mà khi Bật/Tắt.
- [x] **GUI Toggle:** Nút bấm On/Off nằm ở góc trái màn hình, dễ dàng thao tác.
- [x] **Safe Injection:** Tự động đưa GUI vào `CoreGui` (nếu hỗ trợ) hoặc `PlayerGui` để tránh bị reset khi chết.
- [x] **Anti-Duplicate:** Tự động xóa GUI cũ khi chạy lại script để tránh chồng chéo giao diện.

## 📸 Hình ảnh (Preview)

*(Bạn hãy chụp ảnh màn hình trong game khi script đang chạy và dán link ảnh vào đây, ví dụ: `<img src="link_anh_cua_ban.png" width="400">`)*

## 🛠️ Cách sử dụng (How to Use)

1. Mở trình thực thi script (Executor) của bạn (ví dụ: Synapse X, Krnl, Fluxus, Delta, v.v.).
2. Sao chép đoạn code bên dưới.
3. Dán vào Executor và nhấn **Execute**.
4. Một nút bấm **"INF JUMP: OFF"** sẽ hiện ở góc trên bên trái màn hình.
5. Nhấn vào nút để bật chế độ nhảy vô hạn (Nút chuyển sang màu xanh lá).

## 📜 Source Code

```lua
-- Dịch vụ cần thiết
local UserInputService = game:GetService("UserInputService")
local TweenService = game:GetService("TweenService")
local Players = game:GetService("Players")
local CoreGui = game:GetService("CoreGui")

-- Biến trạng thái
local infiniteJumpEnabled = false
local localPlayer = Players.LocalPlayer

-- --- PHẦN TẠO GUI (GIAO DIỆN) ---

-- Xóa GUI cũ nếu đã tồn tại
if CoreGui:FindFirstChild("InfJumpGUI") then
	CoreGui:FindFirstChild("InfJumpGUI"):Destroy()
end

-- Tạo ScreenGui
local screenGui = Instance.new("ScreenGui")
screenGui.Name = "InfJumpGUI"
pcall(function()
	screenGui.Parent = CoreGui
end)
if not screenGui.Parent then
	screenGui.Parent = localPlayer:WaitForChild("PlayerGui")
end

-- Tạo Nút bấm chính
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

-- Trang trí (Bo góc, Viền, Bóng)
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

-- --- LOGIC ---

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
```

## ⚠️ Lưu ý (Disclaimer)

Script này được tạo ra cho mục đích giáo dục và thử nghiệm. Vui lòng sử dụng có trách nhiệm.

---
Created by [MinhLuaStudio]
```
