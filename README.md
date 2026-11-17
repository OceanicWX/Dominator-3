local Players = game:GetService("Players")
local TweenService = game:GetService("TweenService")
local ContentProvider = game:GetService("ContentProvider")
local UserInputService = game:GetService("UserInputService")
local RunService = game:GetService("RunService")
local CoreGui = game:GetService("CoreGui")

local player = Players.LocalPlayer
local char = player.Character or player.CharacterAdded:Wait()
local humanoid = char:WaitForChild("Humanoid")
local hrp = char:WaitForChild("HumanoidRootPart")
local backpack = player:WaitForChild("Backpack")
local playerGui = player:WaitForChild("PlayerGui")
local camera = workspace.CurrentCamera

local oldScreenGui = CoreGui:FindFirstChild("Dominator2DriveUI")
if oldScreenGui then oldScreenGui:Destroy() end

local screenGui = Instance.new("ScreenGui")
screenGui.Name = "Dominator2DriveUI"
screenGui.Enabled = false
screenGui.ResetOnSpawn = false
screenGui.Parent = CoreGui

local mainUIFrame = Instance.new("Frame", screenGui)
mainUIFrame.Size = UDim2.new(1, 0, 1, 0)
mainUIFrame.BackgroundTransparency = 1

local leftSteerPanel = Instance.new("Frame", mainUIFrame)
leftSteerPanel.Size = UDim2.new(0, 160, 0, 70)
leftSteerPanel.Position = UDim2.new(0, 40, 1, -110)
leftSteerPanel.BackgroundTransparency = 1

local rightPedalPanel = Instance.new("Frame", mainUIFrame)
rightPedalPanel.Size = UDim2.new(0, 180, 0, 110)
rightPedalPanel.Position = UDim2.new(1, -220, 1, -150)
rightPedalPanel.BackgroundTransparency = 1

local speedoFrame = Instance.new("Frame", mainUIFrame)
speedoFrame.Size = UDim2.new(0, 180, 0, 70)
speedoFrame.Position = UDim2.new(0.5, -90, 1, -120)
speedoFrame.BackgroundColor3 = Color3.fromRGB(15, 15, 15)
speedoFrame.BackgroundTransparency = 0.5

local speedoCorner = Instance.new("UICorner", speedoFrame)
speedoCorner.CornerRadius = UDim.new(0, 8)

local speedText = Instance.new("TextLabel", speedoFrame)
speedText.Size = UDim2.new(1, 0, 0.7, 0)
speedText.Position = UDim2.new(0, 0, 0, 2)
speedText.BackgroundTransparency = 1
speedText.Text = "000"
speedText.TextColor3 = Color3.fromRGB(100, 240, 255)
speedText.Font = Enum.Font.Code
speedText.TextSize = 34

local unitText = Instance.new("TextLabel", speedoFrame)
unitText.Size = UDim2.new(1, 0, 0.3, 0)
unitText.Position = UDim2.new(0, 0, 0.65, 0)
unitText.BackgroundTransparency = 1
unitText.Text = "MPH"
unitText.TextColor3 = Color3.fromRGB(160, 160, 160)
unitText.Font = Enum.Font.SourceSansBold
unitText.TextSize = 14

local controlBar = Instance.new("Frame", mainUIFrame)
controlBar.Size = UDim2.new(0, 740, 0, 50)
controlBar.Position = UDim2.new(0.5, -370, 1, -185)
controlBar.BackgroundTransparency = 1

local function createSleekButton(text, pos, size, parent)
	local btn = Instance.new("TextButton", parent)
	btn.Text = text
	btn.Size = size
	btn.Position = pos
	btn.BackgroundColor3 = Color3.fromRGB(20, 20, 20)
	btn.TextColor3 = Color3.fromRGB(245, 245, 245)
	btn.Font = Enum.Font.SourceSansBold
	btn.TextSize = 22
	btn.BackgroundTransparency = 0.4
	local corner = Instance.new("UICorner", btn)
	corner.CornerRadius = UDim.new(0, 6)
	return btn
end

local btnLeft = createSleekButton("◀", UDim2.new(0, 0, 0, 10), UDim2.new(0, 70, 0, 50), leftSteerPanel)
local btnRight = createSleekButton("▶", UDim2.new(0, 85, 0, 10), UDim2.new(0, 70, 0, 50), leftSteerPanel)
local btnBrake = createSleekButton("┃┃┃", UDim2.new(0, 0, 0, 10), UDim2.new(0, 45, 0, 90), rightPedalPanel)
btnBrake.TextColor3 = Color3.fromRGB(220, 60, 60)
local btnReverse = createSleekButton("R", UDim2.new(0, 60, 0, 25), UDim2.new(0, 45, 0, 75), rightPedalPanel)
btnReverse.TextSize = 20
local btnForward = createSleekButton("┃┃┃", UDim2.new(0, 120, 0, 10), UDim2.new(0, 45, 0, 90), rightPedalPanel)
btnForward.TextColor3 = Color3.fromRGB(60, 180, 60)

local btnDeploy = createSleekButton("DEPLOY", UDim2.new(0, 0, 0, 0), UDim2.new(0, 140, 1, 0), controlBar)
btnDeploy.BackgroundColor3 = Color3.fromRGB(30, 60, 110)
local btnLights = createSleekButton("LIGHTS", UDim2.new(0, 150, 0, 0), UDim2.new(0, 140, 1, 0), controlBar)
btnLights.BackgroundColor3 = Color3.fromRGB(50, 50, 50)
local btnCabin = createSleekButton("CABIN", UDim2.new(0, 300, 0, 0), UDim2.new(0, 140, 1, 0), controlBar)
btnCabin.BackgroundColor3 = Color3.fromRGB(40, 40, 40)

local btnHorn = createSleekButton("HORN", UDim2.new(0, 450, 0, 0), UDim2.new(0, 140, 1, 0), controlBar)
btnHorn.BackgroundColor3 = Color3.fromRGB(70, 20, 20)

local btnCoolCamera = createSleekButton("COOL CAMERA", UDim2.new(0, 600, 0, 0), UDim2.new(0, 140, 1, 0), controlBar)
btnCoolCamera.BackgroundColor3 = Color3.fromRGB(35, 45, 65)
btnCoolCamera.TextSize = 18

local persistentPanel = Instance.new("Frame", screenGui)
persistentPanel.Size = UDim2.new(0, 200, 0, 40)
persistentPanel.Position = UDim2.new(0, 40, 0, 70)
persistentPanel.BackgroundTransparency = 1

local btnHideUI = createSleekButton("HIDE UI", UDim2.new(0, 0, 0, 0), UDim2.new(0, 90, 1, 0), persistentPanel)
btnHideUI.TextSize = 14
local btnEngineToggle = createSleekButton("ENG ON", UDim2.new(0, 100, 0, 0), UDim2.new(0, 90, 1, 0), persistentPanel)
btnEngineToggle.TextSize = 14
btnEngineToggle.BackgroundColor3 = Color3.fromRGB(20, 100, 20)

for _, name in ipairs({"Airbags", "Horn", "Lights", "Cool Camera"}) do
	local old = backpack:FindFirstChild(name) or char:FindFirstChild(name)
	if old then old:Destroy() end
end

local objects = game:GetObjects("rbxassetid://18855886737")
local asset = objects and objects
local target = asset and asset:FindFirstChild("Mobile Dominator3", true)

if target then
	for _, v in ipairs(target:GetDescendants()) do
		if v.Name == "Anchor Button" then
			v:Destroy()
		end
	end

	if target:IsA("BasePart") then target.Anchored = true end
	for _, v in ipairs(target:GetDescendants()) do
		if v:IsA("BasePart") then v.Anchored = true end
	end

	local spawnCFrame = hrp.CFrame * CFrame.new(0, -1.1, -15) * CFrame.Angles(0, math.rad(90), 0)
	if target:IsA("Model") then target:PivotTo(spawnCFrame) else target.CFrame = spawnCFrame end

	local wheels, wheelLookup = {}, {}
	for _, v in ipairs(target:GetDescendants()) do
		if v.Name:lower():find("wheel") and (v:IsA("Model") or v:IsA("BasePart")) then
			local parentIsWheel = v.Parent and v.Parent ~= target and v.Parent.Name:lower():find("wheel")
			if not parentIsWheel then table.insert(wheels, v) end
		end
	end

	if #wheels >= 4 then
		local w2 = wheels
		local targetY = w2:IsA("Model") and w2:GetPivot().Position.Y or w2.Position.Y
		for _, i in ipairs({1, 3, 4}) do
			local w = wheels[i]
			if w:IsA("Model") then
				local cf = w:GetPivot()
				w:PivotTo(cf + Vector3.new(0, targetY - cf.Position.Y, 0))
			else
				w.Position = Vector3.new(w.Position.X, targetY, w.Position.Z)
			end
		end
	end

	for _, w in ipairs(wheels) do
		wheelLookup[w] = true
		for _, sub in ipairs(w:GetDescendants()) do wheelLookup[sub] = true end
	end

	local bodyParts = {}
	if target:IsA("BasePart") and not wheelLookup[target] then table.insert(bodyParts, target) end
	for _, v in ipairs(target:GetDescendants()) do
		if v:IsA("BasePart") and not wheelLookup[v] then table.insert(bodyParts, v) end
	end

	local wheelParts = {}
	for _, w in ipairs(wheels) do
		if w:IsA("BasePart") then
			table.insert(wheelParts, w)
		end
		for _, sub in ipairs(w:GetDescendants()) do
			if sub:IsA("BasePart") then
				table.insert(wheelParts, sub)
			end
		end
	end

	local allParts = {}
	for _, p in ipairs(bodyParts) do table.insert(allParts, p) end
	for _, p in ipairs(wheelParts) do table.insert(allParts, p) end

	target.Parent = workspace

	local modelCFrame, modelSize = target:IsA("Model") and target:GetBoundingBox() or target.CFrame, target:IsA("Model") and select(2, target:GetBoundingBox()) or target.Size

	local chassis = Instance.new("Part")
	chassis.Name = "Dominator3ChassisRoot"
	chassis.Size = Vector3.new(10, 2, 20)
	chassis.Transparency = 1
	chassis.CanCollide = false
	chassis.Anchored = true
	chassis.CFrame = CFrame.new(modelCFrame.Position) * (spawnCFrame - spawnCFrame.Position) * CFrame.Angles(0, math.rad(-90), 0)
	chassis.CustomPhysicalProperties = PhysicalProperties.new(2.4, 0.3, 0.0, 1.0, 1.0)
	chassis.Parent = target

	local bodyMount = Instance.new("Part")
	bodyMount.Name = "Dominator3BodyMount"
	bodyMount.Size = Vector3.new(2, 2, 2)
	bodyMount.Transparency = 1
	bodyMount.CanCollide = false
	bodyMount.Anchored = true
	bodyMount.CFrame = chassis.CFrame
	bodyMount.CustomPhysicalProperties = PhysicalProperties.new(0.05, 0, 0, 0, 0)
	bodyMount.Parent = target

	local suspensionAtt0 = Instance.new("Attachment", chassis)
	local suspensionAtt1 = Instance.new("Attachment", bodyMount)

	local bodyAlignPos = Instance.new("AlignPosition")
	bodyAlignPos.Attachment0 = suspensionAtt1
	bodyAlignPos.Attachment1 = suspensionAtt0
	bodyAlignPos.MaxForce = 2000000
	bodyAlignPos.MaxVelocity = 500
	bodyAlignPos.Responsiveness = 40
	bodyAlignPos.Parent = bodyMount

	local bodyAlignOri = Instance.new("AlignOrientation")
	bodyAlignOri.Attachment0 = suspensionAtt1
	bodyAlignOri.Attachment1 = suspensionAtt0
	bodyAlignOri.MaxTorque = 2000000
	bodyAlignOri.MaxAngularVelocity = 500
	bodyAlignOri.Responsiveness = 50
	bodyAlignOri.Parent = bodyMount

	for _, p in ipairs(wheelParts) do
		p.CustomPhysicalProperties = PhysicalProperties.new(0.6, 0.05, 0.0, 1.0, 1.0)
		local w = Instance.new("WeldConstraint")
		w.Part0 = chassis
		w.Part1 = p
		w.Parent = chassis
	end

	for _, p in ipairs(bodyParts) do
		p.CustomPhysicalProperties = PhysicalProperties.new(0.25, 0.2, 0.0, 1.0, 1.0)
		local w = Instance.new("WeldConstraint")
		w.Part0 = bodyMount
		w.Part1 = p
		w.Parent = bodyMount
	end

	local driveAtt = Instance.new("Attachment", chassis)
	driveAtt.Name = "Dominator3DriveAttachment"

	local vehicleEngine = Instance.new("LinearVelocity")
	vehicleEngine.Name = "VehicleEngine"
	vehicleEngine.Attachment0 = driveAtt
	vehicleEngine.VelocityConstraintMode = Enum.VelocityConstraintMode.Vector
	vehicleEngine.RelativeTo = Enum.ActuatorRelativeTo.World
	vehicleEngine.MaxForce = 280000
  vehicleEngine.VectorVelocity = Vector3.zero
vehicleEngine.Parent = chassis
local vehicleSteer = Instance.new("AngularVelocity")
vehicleSteer.Name = "VehicleSteer"
vehicleSteer.Attachment0 = driveAtt
vehicleSteer.RelativeTo = Enum.ActuatorRelativeTo.World
vehicleSteer.MaxTorque = 360000
vehicleSteer.AngularVelocity = Vector3.zero
vehicleSteer.Parent = chassis
local uprightGyro = Instance.new("BodyGyro")
uprightGyro.MaxTorque = Vector3.new(200000, 0, 200000)
uprightGyro.P = 12000
uprightGyro.D = 1200
uprightGyro.CFrame = chassis.CFrame
uprightGyro.Parent = chassis
local soundParent = chassis
local airbagSound = Instance.new("Sound")
airbagSound.SoundId = "rbxassetid://86592408723413"
airbagSound.Volume = 3
airbagSound.RollOffMaxDistance = 300
airbagSound.Parent = soundParent
local hornSound = Instance.new("Sound")
hornSound.SoundId = "rbxassetid://110678372100100"
hornSound.Volume = 4
hornSound.Looped = true
hornSound.PlaybackSpeed = 1.3
hornSound.RollOffMaxDistance = 500
hornSound.Parent = soundParent
local echo = Instance.new("EchoSoundEffect")
echo.Delay = 0.28
echo.Feedback = 0.38
echo.WetLevel = -8
echo.Parent = hornSound
local reverb = Instance.new("ReverbSoundEffect")
reverb.DecayTime = 2.4
reverb.Density = 0.7
reverb.Diffusion = 0.85
reverb.WetLevel = -10
reverb.Parent = hornSound
local startupSound = Instance.new("Sound")
startupSound.SoundId = "rbxassetid://18912671577"
startupSound.Volume = 3.5
startupSound.Looped = false
startupSound.RollOffMaxDistance = 350
startupSound.Parent = soundParent
local idleSound = Instance.new("Sound")
idleSound.SoundId = "rbxassetid://102289457821409"
idleSound.Volume = 3
idleSound.PlaybackSpeed = 1.0
idleSound.Looped = true
idleSound.RollOffMaxDistance = 350
idleSound.Parent = soundParent
pcall(function() ContentProvider:PreloadAsync({airbagSound, hornSound, startupSound, idleSound}) end)
_G.D1DriveStateRef = _G.D1DriveStateRef or {
forward = false,
reverse = false,
left = false,
right = false,
brake = false,
currentSpeed = 0
}
local driveState = {
engineKillOverride = false,
canDrive = true,
deployed = false,
currentSpeed = 0
}
_G.Dominator3DriveState = driveState
local isSeated, activeSeat, playerWeld, jumpConn = false, nil, nil, nil
local soundDriverSeat = nil
local engineSession = 0
local function stopEngine()
engineSession += 1
if startupSound.IsPlaying then startupSound:Stop() end
if idleSound.IsPlaying then idleSound:Stop() end
idleSound.PlaybackSpeed = 1.0
end
local function startEngine()
if driveState.engineKillOverride then return end
engineSession += 1
local currentSession = engineSession
if startupSound.IsPlaying then startupSound:Stop() end
if idleSound.IsPlaying then idleSound:Stop() end
idleSound.PlaybackSpeed = 1.0
startupSound:Play()
startupSound.Ended:Once(function()
if engineSession == currentSession and isSeated and not driveState.engineKillOverride and target.Parent then
idleSound.PlaybackSpeed = 1.0
idleSound:Play()
end
end)
end
local uiHidden = false
local hiddenExternalGuis = {}
local function setGlobalUIVisibility(hide)
uiHidden = hide
mainUIFrame.Visible = not hide
btnEngineToggle.Visible = not hide
btnHideUI.Text = hide and "SHOW UI" or "HIDE UI"
if hide then
table.clear(hiddenExternalGuis)
for _, gui in ipairs(CoreGui:GetChildren()) do
if gui:IsA("ScreenGui") and gui ~= screenGui and gui.Enabled then
gui.Enabled = false
hiddenExternalGuis[gui] = true
end
end
for _, gui in ipairs(playerGui:GetChildren()) do
if gui:IsA("ScreenGui") and gui.Enabled then
gui.Enabled = false
hiddenExternalGuis[gui] = true
end
end
else
for gui in pairs(hiddenExternalGuis) do
if gui and gui.Parent then
gui.Enabled = true
end
end
table.clear(hiddenExternalGuis)
end
end
btnHideUI.MouseButton1Click:Connect(function()
setGlobalUIVisibility(not uiHidden)
end)
btnEngineToggle.MouseButton1Click:Connect(function()
driveState.engineKillOverride = not driveState.engineKillOverride
if driveState.engineKillOverride then
driveState.canDrive = false
btnEngineToggle.Text = "ENG OFF"
btnEngineToggle.BackgroundColor3 = Color3.fromRGB(100, 20, 20)
stopEngine()
if _G.D1DriveStateRef then
_G.D1DriveStateRef.forward = false
_G.D1DriveStateRef.reverse = false
_G.D1DriveStateRef.left = false
_G.D1DriveStateRef.right = false
_G.D1DriveStateRef.brake = false
_G.D1DriveStateRef.currentSpeed = 0
end
else
driveState.canDrive = true
btnEngineToggle.Text = "ENG ON"
btnEngineToggle.BackgroundColor3 = Color3.fromRGB(20, 100, 20)
if isSeated and activeSeat == soundDriverSeat then
startEngine()
end
end
end)
local camActive, camStage, camConn, camAnchor = false, 0, nil, nil
local camZoom = 0
local camOffsets = {
Vector3.new(20, 3, 0),
Vector3.new(0, 3, -22),
Vector3.new(-20, 3, 0),
Vector3.new(0, 3, 22),
Vector3.new(0, 5, 26),
}
local function resetCam()
camActive, camZoom = false, 0
if camConn then camConn:Disconnect() camConn = nil end
if camAnchor then camAnchor:Destroy() camAnchor = nil end
camera.CameraType = Enum.CameraType.Custom
camera.CameraSubject = humanoid
btnCoolCamera.BackgroundColor3 = Color3.fromRGB(35, 45, 65)
btnCoolCamera.TextColor3 = Color3.fromRGB(245, 245, 245)
end
local function setCamStage(idx)
camZoom = 0
local basis = chassis.CFrame
local pos = (basis * CFrame.new(camOffsets[idx])).Position
if not camAnchor then
camAnchor = Instance.new("Part")
camAnchor.Size = Vector3.new(0.1, 0.1, 0.1)
camAnchor.Transparency = 1
camAnchor.Anchored = true
camAnchor.CanCollide = false
camAnchor.Parent = workspace
end
camAnchor.Position = pos
camera.CameraType = Enum.CameraType.Custom
camera.CameraSubject = camAnchor
camera.CFrame = CFrame.lookAt(pos, chassis.Position)
if not camConn then
camConn = RunService.RenderStepped:Connect(function()
if not camActive or not camAnchor or not target.Parent then resetCam() return end
local currentPos = (chassis.CFrame * CFrame.new(camOffsets[camStage])).Position
camAnchor.Position = currentPos
local rot = camera.CFrame - camera.CFrame.Position
camera.CFrame = (CFrame.new(camAnchor.Position) * rot) * CFrame.new(0, 0, camZoom)
end)
end
end
UserInputService.InputChanged:Connect(function(input, gp)
if gp or not camActive then return end
if input.UserInputType == Enum.UserInputType.MouseWheel then
camZoom = math.clamp(camZoom - (input.Position.Z * 2), -18, 30)
end
end)
UserInputService.TouchPinch:Connect(function(_, scale, _, state, gp)
if gp or not camActive then return end
if state == Enum.UserInputState.Change then
camZoom = math.clamp(camZoom + ((1 - scale) * 1.5), -18, 30)
end
end)
btnCoolCamera.MouseButton1Click:Connect(function()
if not target.Parent then return end
if camActive then
resetCam()
else
camActive = true
camStage = (camStage % #camOffsets) + 1
setCamStage(camStage)
btnCoolCamera.BackgroundColor3 = Color3.fromRGB(80, 150, 235)
btnCoolCamera.TextColor3 = Color3.new(0, 0, 0)
end
end)
local function exitSeat()
if not isSeated then return end
isSeated = false
if uiHidden then
setGlobalUIVisibility(false)
end
screenGui.Enabled = false
if camActive then
resetCam()
end
stopEngine()
if playerWeld then playerWeld:Destroy() playerWeld = nil end
if jumpConn then jumpConn:Disconnect() jumpConn = nil end
if hrp and activeSeat then hrp.CFrame = activeSeat.CFrame * CFrame.new(-6, 2, 0) end
if humanoid then humanoid.Sit = false end
activeSeat = nil
end
local function enterSeat(seatPart, isDriverSeat)
if isSeated or not char or not hrp or not humanoid or humanoid.Health <= 0 then return end
isSeated = true
activeSeat = seatPart
hrp.CFrame = seatPart.CFrame * CFrame.new(0, 1.5, 0)
humanoid.Sit = true
playerWeld = Instance.new("WeldConstraint", hrp)
playerWeld.Name = "ForcedLocalSeatLock"
playerWeld.Part0 = seatPart
playerWeld.Part1 = hrp
if isDriverSeat and seatPart == soundDriverSeat then
screenGui.Enabled = true
if not driveState.engineKillOverride then
startEngine()
end
end
jumpConn = humanoid:GetPropertyChangedSignal("Jump"):Connect(function()
if humanoid.Jump then exitSeat() end
end)
end
local baseSeat = Instance.new("Part")
baseSeat.Name = "CustomIsolatedSeat"
baseSeat.BrickColor = BrickColor.new("Dark stone grey")
baseSeat.Material = Enum.Material.SmoothPlastic
baseSeat.Anchored = true
baseSeat.CanCollide = true
local vSeats, rSeats = {}, {}
for _, v in ipairs(target:GetDescendants()) do
if v:IsA("BasePart") then
if v.Name == "VehicleSeat" then table.insert(vSeats, v)
elseif v.Name == "Seat" then table.insert(rSeats, v) end
end
end
local function bindSeat(custom, orig, isDriverSeat)
if orig:IsA("Seat") or orig:IsA("VehicleSeat") then orig.Disabled = true end
custom.Size = orig.Size
custom.CFrame = orig.CFrame * CFrame.new(0, 0.8, 0)
custom.Transparency = orig.Transparency
custom.Parent = orig
if isDriverSeat and not soundDriverSeat then
soundDriverSeat = custom
end
custom.Touched:Connect(function(hit)
if hit.Parent == char then enterSeat(custom, isDriverSeat) end
end)
table.insert(bodyParts, custom)
table.insert(allParts, custom)
end
for i, s in ipairs(vSeats) do bindSeat(i == 1 and baseSeat or baseSeat:Clone(), s, i == 1) end
for _, s in ipairs(rSeats) do bindSeat(baseSeat:Clone(), s, false) end
if #vSeats == 0 then baseSeat:Destroy() end
local lowered, isAnimating = false, false
btnDeploy.MouseButton1Click:Connect(function()
if isAnimating or not target.Parent then return end
isAnimating = true
local t = 0
while airbagSound.TimeLength <= 0 and t < 2 do t += task.wait(0.05) end
local animTime = airbagSound.TimeLength > 0 and airbagSound.TimeLength or 0.6
airbagSound:Play()
lowered = not lowered
driveState.deployed = lowered
btnDeploy.Text = lowered and "DEPLOYING..." or "RETRACTING..."
btnDeploy.BackgroundColor3 = Color3.fromRGB(180, 100, 30)
local targetOffset = lowered and Vector3.new(0, -0.6, 0) or Vector3.zero
local info = TweenInfo.new(animTime, Enum.EasingStyle.Quad, Enum.EasingDirection.Out)
local tween = TweenService:Create(suspensionAtt1, info, {Position = -targetOffset})
tween:Play()
tween.Completed:Wait()
btnDeploy.Text = lowered and "DEPLOYED" or "DEPLOY"
btnDeploy.BackgroundColor3 = lowered and Color3.fromRGB(180, 30, 30) or Color3.fromRGB(30, 60, 110)
isAnimating = false
end)
local holdingHorn, hornSession = false, 0
local function stopHorn()
if not holdingHorn then return end
holdingHorn = false
hornSession += 1
local s = hornSession
btnHorn.BackgroundColor3 = Color3.fromRGB(70, 20, 20)
btnHorn.TextColor3 = Color3.fromRGB(245, 245, 245)
if hornSound.IsPlaying then
TweenService:Create(hornSound, TweenInfo.new(0.12), {Volume = 0}):Play()
task.delay(1.8, function()
if hornSession == s and not holdingHorn and hornSound.IsPlaying then
hornSound:Stop()
hornSound.Volume = 4
end
end)
end
end
local function startHorn()
if holdingHorn or not target.Parent then return end
holdingHorn = true
hornSession += 1
btnHorn.BackgroundColor3 = Color3.fromRGB(220, 40, 40)
btnHorn.TextColor3 = Color3.new(0, 0, 0)
hornSound.Volume = 4
if not hornSound.IsPlaying then hornSound:Play() end
end
btnHorn.InputBegan:Connect(function(input)
if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then
startHorn()
end
end)
btnHorn.InputEnded:Connect(function(input)
if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then
stopHorn()
end
end)
local lightsOn, spawnedLights = false, {}
local function createHeadlight(sideOffset)
local p = Instance.new("Part")
p.Size = Vector3.new(0.1, 0.1, 0.1)
p.Transparency = 1
p.Anchored = false
p.CanCollide = false
p.CastShadow = false
local heightOffset = (-modelSize.Y * 0.1)
p.CFrame = bodyMount.CFrame * CFrame.new(sideOffset, heightOffset, -(modelSize.X / 2) - 0.15)
p.Parent = target
local w = Instance.new("WeldConstraint")
w.Part0 = bodyMount
w.Part1 = p
w.Parent = p
local spot = Instance.new("SpotLight")
spot.Face = Enum.NormalId.Front
spot.Brightness = 15
spot.Range = 60
spot.Angle = 75
spot.Color = Color3.fromRGB(235, 245, 255)
spot.Shadows = true
spot.Parent = p
local point = Instance.new("PointLight")
point.Brightness = 3
point.Range = 10
point.Color = Color3.fromRGB(235, 245, 255)
point.Parent = p
table.insert(spawnedLights, p)
end
btnLights.MouseButton1Click:Connect(function()
if not target.Parent then return end
lightsOn = not lightsOn
btnLights.BackgroundColor3 = lightsOn and Color3.fromRGB(230, 200, 50) or Color3.fromRGB(50, 50, 50)
btnLights.TextColor3 = lightsOn and Color3.new(0, 0, 0) or Color3.fromRGB(245, 245, 245)
if lightsOn then
local spread = math.max(modelSize.Z * 0.3, 2.2)
createHeadlight(-spread)
createHeadlight(spread)
else
for _, p in ipairs(spawnedLights) do
if p and p.Parent then p:Destroy() end
end
table.clear(spawnedLights)
end
end)
local function bindDriveBtn(btn, key, activeColor, defaultColor)
btn.InputBegan:Connect(function(input)
if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then
if driveState.engineKillOverride or not driveState.canDrive then return end
if key == "forward" then _G.D1DriveStateRef.reverse = false
elseif key == "reverse" then _G.D1DriveStateRef.forward = false end
_G.D1DriveStateRef[key] = true
btn.BackgroundTransparency = 0.1
if activeColor then btn.TextColor3 = activeColor end
end
end)
btn.InputEnded:Connect(function(input)
if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then
_G.D1DriveStateRef[key] = false
btn.BackgroundTransparency = 0.4
btn.TextColor3 = defaultColor
end
end)
end
bindDriveBtn(btnForward, "forward", Color3.fromRGB(100, 255, 100), Color3.fromRGB(60, 180, 60))
bindDriveBtn(btnReverse, "reverse", Color3.fromRGB(255, 255, 100), Color3.fromRGB(245, 245, 245))
bindDriveBtn(btnLeft, "left", nil, Color3.fromRGB(245, 245, 245))
bindDriveBtn(btnRight, "right", nil, Color3.fromRGB(245, 245, 245))
bindDriveBtn(btnBrake, "brake", Color3.fromRGB(255, 100, 100), Color3.fromRGB(220, 60, 60))
for _, p in ipairs(allParts) do
if p and p.Parent then
p.Anchored = false
end
end
bodyMount.Anchored = false
chassis.Anchored = false
local MAX_FORWARD_STUDS = 128
local MAX_REVERSE_STUDS = 38
local STUDS_TO_MPH = 95 / MAX_FORWARD_STUDS
local currentPitch = 1.0
RunService.Heartbeat:Connect(function(dt)
if not target or not target.Parent or not chassis or not chassis.Parent then return end
local stateRef = _G.D1DriveStateRef
local locked = driveState.deployed or driveState.engineKillOverride or not driveState.canDrive or not isSeated or activeSeat ~= soundDriverSeat
local rayOrigin = chassis.Position + Vector3.new(0, 2, 0)
local rayParams = RaycastParams.new()
rayParams.FilterDescendantsInstances = {target, char}
rayParams.FilterType = Enum.RaycastFilterType.Exclude
local groundHit = workspace:Raycast(rayOrigin, Vector3.new(0, -10, 0), rayParams)
local isOffroad = groundHit and (groundHit.Material == Enum.Material.Grass or groundHit.Material == Enum.Material.Mud or groundHit.Material == Enum.Material.Sand or groundHit.Material == Enum.Material.Ground)
local maxFwd = isOffroad and (MAX_FORWARD_STUDS * 0.72) or MAX_FORWARD_STUDS
local maxRev = isOffroad and (MAX_REVERSE_STUDS * 0.75) or MAX_REVERSE_STUDS
if locked then
driveState.currentSpeed = 0
stateRef.currentSpeed = 0
vehicleEngine.VectorVelocity = Vector3.new(0, chassis.AssemblyLinearVelocity.Y, 0)
vehicleSteer.AngularVelocity = Vector3.zero
else
local absSp = math.abs(driveState.currentSpeed)
if stateRef.brake then
local brakeDecel = 145 * dt
if driveState.currentSpeed > 0 then
driveState.currentSpeed = math.max(0, driveState.currentSpeed - brakeDecel)
elseif driveState.currentSpeed < 0 then
driveState.currentSpeed = math.min(0, driveState.currentSpeed + brakeDecel)
end
elseif stateRef.forward then
local torqueCurve = math.clamp(1.25 - (absSp / maxFwd) * 0.85, 0.28, 1.25)
local accelRate = 46 * torqueCurve * dt
driveState.currentSpeed = math.min(maxFwd, driveState.currentSpeed + accelRate)
elseif stateRef.reverse then
local revTorqueCurve = math.clamp(1.2 - (absSp / maxRev) * 0.75, 0.35, 1.2)
local revAccelRate = 34 * revTorqueCurve * dt
driveState.currentSpeed = math.max(-maxRev, driveState.currentSpeed - revAccelRate)
else
local coastDrag = 18 * dt
if driveState.currentSpeed > maxFwd then
driveState.currentSpeed = math.max(maxFwd, driveState.currentSpeed - (40 * dt))
elseif driveState.currentSpeed > 0 then
driveState.currentSpeed = math.max(0, driveState.currentSpeed - coastDrag)
elseif driveState.currentSpeed < -maxRev then
driveState.currentSpeed = math.min(-maxRev, driveState.currentSpeed + (40 * dt))
elseif driveState.currentSpeed < 0 then
driveState.currentSpeed = math.min(0, driveState.currentSpeed + coastDrag)
end
end
stateRef.currentSpeed = driveState.currentSpeed
local forwardVec = chassis.CFrame.LookVector
local verticalVel = chassis.AssemblyLinearVelocity.Y
vehicleEngine.VectorVelocity = Vector3.new(
forwardVec.X * driveState.currentSpeed,
verticalVel,
forwardVec.Z * driveState.currentSpeed
)
local turnAuthority = 0
if absSp > 0.5 then
local speedRatio = math.clamp(absSp / maxFwd, 0, 1)
local heavyTurnRate = math.clamp(1.32 - (speedRatio * 0.72), 0.52, 1.32)
if isOffroad then
heavyTurnRate = heavyTurnRate * 0.85
end
local reverseDir = driveState.currentSpeed < 0 and -1 or 1
if stateRef.left then
turnAuthority = heavyTurnRate * reverseDir
elseif stateRef.right then
turnAuthority = -heavyTurnRate * reverseDir
end
end
vehicleSteer.AngularVelocity = Vector3.new(0, turnAuthority, 0)
end
local displayMph = math.abs(math.floor(driveState.currentSpeed * STUDS_TO_MPH + 0.5))
speedText.Text = string.format("%03d", displayMph)
if idleSound.IsPlaying and not driveState.engineKillOverride then
local speedNorm = math.clamp(math.abs(driveState.currentSpeed) / MAX_FORWARD_STUDS, 0, 1)
local gearProgress = (speedNorm * 5) % 1
local gearRpmFactor = (speedNorm > 0.02) and (0.25 + gearProgress * 0.75) or 0
local throttleLoad = (stateRef.forward or stateRef.reverse) and 0.14 or 0
local targetPitch = 1.0 + (speedNorm * 0.65) + (gearRpmFactor * 0.28) + throttleLoad
currentPitch = currentPitch + (targetPitch - currentPitch) * math.clamp(8 * dt, 0, 1)
idleSound.PlaybackSpeed = math.clamp(currentPitch, 0.95, 2.05)
else
currentPitch = 1.0
idleSound.PlaybackSpeed = 1.0
end
end)
end
if asset then asset:Destroy() end
