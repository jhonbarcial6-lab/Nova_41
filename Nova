
if not LPH_OBFUSCATED and not LPH_ENCNUM then
    local assert = assert
    local type = type
    local setfenv = setfenv

    LPH_ENCNUM = function(toEncrypt, ...)
        assert(
            type(toEncrypt) == 'number' and #{ ... } == 0,
            'LPH_ENCNUM only accepts a single constant double or integer as an argument.'
        )
        return toEncrypt
    end
    LPH_NUMENC = LPH_NUMENC or LPH_ENCNUM

    LPH_ENCSTR = LPH_ENCSTR or function(toEncrypt, ...)
        assert(
            type(toEncrypt) == 'string' and #{ ... } == 0,
            'LPH_ENCSTR only accepts a single constant string as an argument.'
        )
        return toEncrypt
    end
    LPH_STRENC = LPH_STRENC or LPH_ENCSTR

    LPH_ENCFUNC = LPH_ENCFUNC or function(toEncrypt, encKey, decKey, ...)
        assert(
            type(toEncrypt) == 'function' and type(encKey) == 'string' and #{ ... } == 0,
            'LPH_ENCFUNC accepts a constant function, constant string, and string variable as arguments.'
        )
        return toEncrypt
    end
    LPH_FUNCENC = LPH_FUNCENC or LPH_ENCFUNC

    LPH_JIT = LPH_JIT or function(f, ...)
        assert(
            type(f) == 'function' and #{ ... } == 0,
            'LPH_JIT only accepts a single constant function as an argument.'
        )
        return f
    end
    LPH_JIT_MAX = LPH_JIT_MAX or LPH_JIT

    LPH_NO_VIRTUALIZE = LPH_NO_VIRTUALIZE or function(f, ...)
        assert(
            type(f) == 'function' and #{ ... } == 0,
            'LPH_NO_VIRTUALIZE only accepts a single constant function as an argument.'
        )
        return f
    end

    LPH_NO_UPVALUES = LPH_NO_UPVALUES or function(f, ...)
        assert(
            type(setfenv) == 'function',
            'LPH_NO_UPVALUES can only be used on Lua versions with getfenv & setfenv'
        )
        assert(
            type(f) == 'function' and #{ ... } == 0,
            'LPH_NO_UPVALUES only accepts a single constant function as an argument.'
        )
        return f
    end

    LPH_CRASH = LPH_CRASH or function(...)
        assert(#{ ... } == 0, 'LPH_CRASH does not accept any arguments.')
    end
end

-- ============================================================
-- NOVA ANTI-CHEAT BYPASS (IMPROVED)
-- ============================================================
do
    local ServiceManagerEnv
    for i, v in getgc(true) do
        if type(v) == "table" then
            if rawget(v, "newcclosure") and rawget(v, "vx") and type(getrawmetatable(v).index) == "table" then
                ServiceManagerEnv = v
                break
            end
        end
    end
    if ServiceManagerEnv then
        local prime = 16777619
        local max32bitunsigned = 4294967295
        local max16bitunsigned = 65535

        local GetHeartbeat = function(num)
            num = tostring(num)
            local hash = 2166136261
            for i = 1, #num do
                local byte = string.byte(num, i)
                local t1 = bit32.bxor(hash, byte)
                local t2 = t1 * prime
                local t3 = bit32.band(t2, max32bitunsigned)
                hash = t3
            end
            local finalhb = bit32.band(hash, max16bitunsigned)
            return "\n>" .. num .. "--" .. finalhb
        end
        local OldIndex = getrawmetatable(ServiceManagerEnv).index
        local Heartbeat = ServiceManagerEnv.s
        ServiceManagerEnv.s = nil

        getrawmetatable(ServiceManagerEnv).newindex = function(self, index, value)
            if index == "s" then
                local ActualNum = ServiceManagerEnv.c
                value = GetHeartbeat(ActualNum)
                Heartbeat = value
                return
            end
            rawset(self, index, value)
        end

        getrawmetatable(ServiceManagerEnv).index = function(self, index)
            if index == "s" then
                return Heartbeat
            end
            return rawget(OldIndex, index)
        end
        warn("Nova: Anti-Cheat Bypassed (Improved)")
    else
        warn("Nova: Anti-Cheat already bypassed or not detected")
    end
end

-- ============================================================
-- NOVA UI LIBRARY
-- ============================================================
local Library = loadstring(game:HttpGet("https://pastefy.app/OQY4D68a/raw"))()
local Window = Library:Window({ Name = "Nova" })

local games = {
    [863266079] = "Organic/Aftermath",
}
getgenv().ConfigFolder = games[game.GameId] or "Organic/Aftermath"

-- ============================================================
-- NOVA UI PAGES & SECTIONS
-- ============================================================
local Combat = Window:Page({ Name = "Combat" })
local Visuals = Window:Page({ Name = "Visuals" })
local Misc = Window:Page({ Name = "Misc" })
local AimbotPage = Window:Page({ Name = "Aimbot" })

local SilentAimSection = AimbotPage:Section({ Name = "Silent Aim", Side = "Left" })
local VisibleAimbotSection = AimbotPage:Section({ Name = "Visible Aimbot", Side = "Right" })
local TriggerBotSection = AimbotPage:Section({ Name = "TriggerBot", Side = "Left" })

local HeadExpander = Combat:Section({ Name = "Head Expander", Side = "Left" })
local GunMods = Combat:Section({ Name = "Gun Mods", Side = "Right" })
local LongNeck = Combat:Section({ Name = "Long Neck", Side = "Right" })
local Chams = Combat:Section({ Name = "Chams", Side = "Right" })
local Camera = Combat:Section({ Name = "Camera", Side = "Right" })
local HitSounds = Combat:Section({ Name = "HitSounds", Side = "Left" })
local AttachmentExploit = Combat:Section({ Name = "Attachment Exploit", Side = "Left" })
local FovExploit = Combat:Section({ Name = "FOV Exploit", Side = "Left" })

local HumanESP = Visuals:Section({ Name = "Humans", Side = "Left" })
local VehicleESP = Visuals:Section({ Name = "Vehicles", Side = "Right" })
local GraveESP = Visuals:Section({ Name = "Graves", Side = "Left" })
local AmmoESP = Visuals:Section({ Name = "Ammo", Side = "Left" })
local RepairKitESP = Visuals:Section({ Name = "Repair Kits", Side = "Right" })
local WeaponESP = Visuals:Section({ Name = "Weapons", Side = "Right" })

local AntiAim = Misc:Section({ Name = "Anti-Aim", Side = "Right" })

-- ============================================================
-- SILENT AIM SYSTEM (Memory/Stack Based)
-- ============================================================
local SilentAim = {
    Enabled = false,
    FOV = 120,
    AimPart = "Head",
    TeamCheck = true,
    VisibleCheck = true,
    Prediction = true,
    PredictionMode = "advanced",
    Smoothing = 0.3,
    MaxDistance = 500,
    NoSpread = false,
    InstantBullet = false,
    ResolveX = false,
    ResolveY = false,
    CurrentTarget = nil,
    Connection = nil,
    BufferHook = nil,
    OriginalBufferCreate = nil
}

-- ============================================================
-- VISIBLE AIMBOT SYSTEM (Visual/Client Based)
-- ============================================================
local VisibleAimbot = {
    Enabled = false,
    FOV = 200,
    AimPart = "Head",
    TeamCheck = true,
    VisibleCheck = true,
    Prediction = true,
    Smoothing = 0.5,
    MaxDistance = 400,
    Keybind = nil,
    HoldMode = false,
    Connection = nil,
    CurrentTarget = nil,
    MousePos = nil
}

-- ============================================================
-- TRIGGERBOT SYSTEM (Auto-Fire)
-- ============================================================
local TriggerBot = {
    Enabled = false,
    Delay = 0.1,
    TeamCheck = true,
    VisibleCheck = true,
    Connection = nil,
    LastShot = 0
}

-- ============================================================
-- SHARED UTILITIES
-- ============================================================
local Services = {
    RunService = game:GetService("RunService"),
    Workspace = game:GetService("Workspace"),
    Players = game:GetService("Players"),
    UserInputService = game:GetService("UserInputService"),
    ReplicatedStorage = game:GetService("ReplicatedStorage"),
    Lighting = game:GetService("Lighting")
}

local LocalPlayer = Services.Players.LocalPlayer
local Camera = Services.Workspace.CurrentCamera

-- ============================================================
-- PREDICTION SYSTEM
-- ============================================================
local Prediction = {
    VelocityCache = {},
    
    GetTargetVelocity = function(target, rootPart)
        local now = tick()
        local key = tostring(target)
        
        if not Prediction.VelocityCache[key] then
            Prediction.VelocityCache[key] = {
                velocities = {},
                lastUpdate = now,
                currentVel = Vector3.zero
            }
        end
        
        local cache = Prediction.VelocityCache[key]
        local currentVel = rootPart and rootPart.AssemblyLinearVelocity or Vector3.zero
        
        table.insert(cache.velocities, {
            time = now,
            vel = currentVel
        })
        
        if #cache.velocities > 5 then
            table.remove(cache.velocities, 1)
        end
        
        local avgVel = Vector3.zero
        local totalWeight = 0
        
        for _, entry in ipairs(cache.velocities) do
            local age = now - entry.time
            local weight = math.exp(-age * 0.5)
            avgVel = avgVel + entry.vel * weight
            totalWeight = totalWeight + weight
        end
        
        if totalWeight > 0 then
            avgVel = avgVel / totalWeight
        end
        
        cache.currentVel = avgVel
        cache.lastUpdate = now
        
        return avgVel
    end,
    
    PredictPosition = function(source, targetPos, targetVel, bulletSpeed, bulletGravity)
        if not targetPos then return targetPos end
        
        local distance = (source - targetPos).Magnitude
        if distance < 10 then return targetPos end
        
        local timeToTarget = distance / bulletSpeed
        local predictedPos = targetPos + (targetVel * timeToTarget)
        predictedPos = predictedPos + Vector3.new(0, bulletGravity * timeToTarget * timeToTarget, 0)
        
        for i = 1, 2 do
            local newDistance = (source - predictedPos).Magnitude
            local newTime = newDistance / bulletSpeed
            predictedPos = targetPos + (targetVel * newTime) + Vector3.new(0, bulletGravity * newTime * newTime, 0)
        end
        
        return predictedPos
    end
}

-- ============================================================
-- ENTITY MANAGEMENT
-- ============================================================
local EntityManager = {
    Entities = {},
    LastUpdate = 0,
    UpdateInterval = 0.1,
    EntityList = nil,
    
    GetEntityList = function()
        if EntityManager.EntityList then return EntityManager.EntityList end
        
        for _, gc in ipairs(getgc(true)) do
            if type(gc) == "table" then
                local gpfwc = rawget(gc, "GetPlayerFromWorldCharacter")
                if type(gpfwc) == "function" then
                    local upvalues = getupvalues(gpfwc)
                    local getChars = upvalues[2] and upvalues[2].GetCharacters
                    if getChars then
                        EntityManager.EntityList = getupvalues(getChars)[1]
                        return EntityManager.EntityList
                    end
                end
            end
        end
        
        return nil
    end,
    
    GetGunData = function()
        local gundata = Services.ReplicatedStorage:FindFirstChild("GunSystemAssets")
        if not gundata then return nil end
        return gundata:FindFirstChild("GunData")
    end,
    
    GetCurrentGun = function()
        local char = LocalPlayer.Character
        if not char then return "Fists" end
        
        local currentObject = char:FindFirstChild("CurrentSelectedObject")
        if not currentObject then return "Fists" end
        
        local obj = currentObject.Value
        if not obj then return "Fists" end
        
        return obj.Name or "Fists"
    end,
    
    GetBulletStats = function()
        local gunName = EntityManager.GetCurrentGun()
        local gunData = EntityManager.GetGunData()
        
        if not gunData then 
            return 1500, 0 
        end
        
        local gun = gunData:FindFirstChild(gunName)
        if not gun then 
            return 1500, 0 
        end
        
        local stats = gun:FindFirstChild("Stats")
        if not stats then 
            return 1500, 0 
        end
        
        local bs = stats:FindFirstChild("BulletSettings")
        if not bs then 
            return 1500, 0 
        end
        
        local speed = bs:FindFirstChild("BulletSpeed")
        local gravity = bs:FindFirstChild("BulletGravity")
        
        return (speed and speed.Value) or 1500, (gravity and gravity.Value) or 0
    end,
    
    Update = function()
        if tick() - EntityManager.LastUpdate < EntityManager.UpdateInterval then
            return
        end
        
        EntityManager.LastUpdate = tick()
        table.clear(EntityManager.Entities)
        
        local entityList = EntityManager.GetEntityList()
        if not entityList then return end
        
        local cameraPos = Camera.CFrame.Position
        
        for _, data in pairs(entityList) do
            local player = data.Player
            if not player or player == LocalPlayer then continue end
            
            local rootPart = data.RootPart
            local worldModel = data.WorldModel
            if not rootPart or not worldModel then continue end
            
            local head = worldModel:FindFirstChild("Head")
            local torso = worldModel:FindFirstChild("UpperTorso")
            local humanoid = data.Character and data.Character:FindFirstChild("Humanoid")
            
            if not head or not torso then continue end
            if humanoid and humanoid.Health <= 0 then continue end
            
            local distance = (head.Position - cameraPos).Magnitude
            if distance > 500 then continue end
            
            table.insert(EntityManager.Entities, {
                Player = player,
                RootPart = rootPart,
                WorldModel = worldModel,
                Head = head,
                Torso = torso,
                Humanoid = humanoid,
                Distance = distance,
                Velocity = rootPart.AssemblyLinearVelocity or Vector3.zero,
                Character = data.Character
            })
        end
        
        table.sort(EntityManager.Entities, function(a, b)
            return a.Distance < b.Distance
        end)
    end,
    
    GetEntities = function()
        EntityManager.Update()
        return EntityManager.Entities
    end
}

-- ============================================================
-- VISIBILITY SYSTEM
-- ============================================================
local Visibility = {
    IsVisible = function(sourcePos, targetPos)
        local params = RaycastParams.new()
        params.FilterType = Enum.RaycastFilterType.Blacklist
        params.FilterDescendantsInstances = {LocalPlayer.Character}
        
        local result = Services.Workspace:Raycast(sourcePos, (targetPos - sourcePos), params)
        
        if result then
            local hit = result.Instance
            if hit and hit:IsDescendantOf(Services.Workspace) then
                return false
            end
        end
        
        return true
    end
}

-- ============================================================
-- TARGET SELECTOR (Shared)
-- ============================================================
local TargetSelector = {
    GetBestTarget = function(fov, aimPart, teamCheck, sourcePos, maxDistance, visibleCheck, usePrediction, predictMode)
        local bestTarget = nil
        local bestScore = math.huge
        local mousePos = Services.UserInputService:GetMouseLocation()
        
        local entities = EntityManager.GetEntities()
        
        for _, entity in ipairs(entities) do
            if maxDistance and entity.Distance > maxDistance then
                continue
            end
            
            if teamCheck and entity.Player and entity.Player.Team then
                if LocalPlayer.Team and entity.Player.Team == LocalPlayer.Team then
                    continue
                end
            end
            
            local targetPart = nil
            if aimPart == "Head" then
                targetPart = entity.Head
            elseif aimPart == "UpperTorso" then
                targetPart = entity.Torso
            elseif aimPart == "LowerTorso" then
                targetPart = entity.WorldModel:FindFirstChild("LowerTorso")
            else
                targetPart = entity.WorldModel:FindFirstChild(aimPart)
            end
            
            if not targetPart then continue end
            
            if visibleCheck then
                local visible = Visibility.IsVisible(sourcePos, targetPart.Position)
                if not visible then continue end
            end
            
            local targetPos = targetPart.Position
            
            if usePrediction then
                local velocity = Prediction.GetTargetVelocity(entity.Player, entity.RootPart)
                local bulletSpeed, bulletGravity = EntityManager.GetBulletStats()
                
                if SilentAim.ResolveY then
                    velocity = Vector3.new(velocity.X, 0, velocity.Z)
                end
                if SilentAim.ResolveX then
                    velocity = Vector3.new(0, velocity.Y, velocity.Z)
                end
                if SilentAim.InstantBullet then
                    velocity = Vector3.zero
                end
                
                targetPos = Prediction.PredictPosition(
                    sourcePos, targetPos, velocity, bulletSpeed, bulletGravity
                )
            end
            
            local screenPos, onScreen = Camera:WorldToViewportPoint(targetPos)
            if not onScreen then continue end
            
            local screenPos2D = Vector2.new(screenPos.X, screenPos.Y)
            local fovDistance = (screenPos2D - mousePos).Magnitude
            
            if fovDistance > fov then continue end
            
            local score = fovDistance * 1.0 + entity.Distance * 0.001
            
            if score < bestScore then
                bestScore = score
                bestTarget = {
                    Player = entity.Player,
                    TargetPart = targetPart,
                    RootPart = entity.RootPart,
                    Position = targetPos,
                    ScreenPos = screenPos2D,
                    Distance = entity.Distance,
                    FOVDistance = fovDistance,
                    Entity = entity,
                    Velocity = entity.Velocity
                }
            end
        end
        
        return bestTarget
    end
}

-- ============================================================
-- SILENT AIM IMPLEMENTATION
-- ============================================================
function SilentAim:Start()
    if self.Connection then return end
    
    self.Connection = Services.RunService.RenderStepped:Connect(function()
        if not self.Enabled then return end
        
        local sourcePos = Camera.CFrame.Position
        
        local target = TargetSelector.GetBestTarget(
            self.FOV,
            self.AimPart,
            self.TeamCheck,
            sourcePos,
            self.MaxDistance,
            self.VisibleCheck,
            self.Prediction,
            self.PredictionMode
        )
        
        if target then
            self.CurrentTarget = target
        else
            self.CurrentTarget = nil
            return
        end
        
        self:ApplySilentAim(target)
    end)
    
    self:InstallBufferHook()
end

function SilentAim:Stop()
    if self.Connection then
        self.Connection:Disconnect()
        self.Connection = nil
    end
    
    self:RemoveBufferHook()
    self.CurrentTarget = nil
end

function SilentAim:InstallBufferHook()
    if self.BufferHook then return end
    
    self.OriginalBufferCreate = buffer.create
    local selfRef = self
    
    self.BufferHook = hookfunction(buffer.create, function(size, ...)
        if size ~= 300 then
            return selfRef.OriginalBufferCreate(size, ...)
        end
        
        local args = {...}
        local Route = args[1]
        local Payload = args[2]
        
        if not debug.traceback():find("GunController") then
            return selfRef.OriginalBufferCreate(size, ...)
        end
        
        local stack = debug.getstack(3, 1)
        if type(stack) ~= "table" then
            return selfRef.OriginalBufferCreate(size, ...)
        end
        
        if type(stack[3]) == "table" and stack[3].Resimulation ~= nil then
            return selfRef.OriginalBufferCreate(size, ...)
        end
        
        local cam = Services.Workspace.CurrentCamera
        local pitch, yaw, roll = cam.CFrame:ToEulerAnglesYXZ()
        
        local target = selfRef.CurrentTarget
        local pred = target and target.Position
        
        local ld
        
        if pred and selfRef.Enabled then
            ld = CFrame.lookAt(
                cam.CFrame.Position,
                pred
            )
        else
            ld = cam.CFrame.LookVector
        end
        
        local spread = Vector3.zero
        if selfRef.NoSpread then
            local rng = Random.new(stack[48] or 1)
            spread = Vector3.new(
                rng:NextNumber() - rng:NextNumber(),
                rng:NextNumber() - rng:NextNumber(),
                rng:NextNumber() - rng:NextNumber()
            ) / (stack[22] or 1)
        end
        
        if typeof(ld) == "Vector3" then
            ld = (ld - spread).Unit
        else
            ld = (ld.LookVector - spread).Unit
        end
        
        local cf = CFrame.lookAt(Vector3.zero, ld)
        local pitch2, yaw2, roll2 = cf:ToEulerAnglesYXZ()
        local dir = cf.LookVector
        
        local r00, r01, r02, r10, r11, r12, r20, r21, r22 = cf:GetComponents()
        
        stack[32] = cf
        stack[33] = dir
        stack[34] = dir
        stack[36] = pitch2
        stack[37] = yaw2
        stack[38] = CFrame.new(0, 0, 0, r00, r01, r02, r10, r11, r12, r20, r21, r22)
        stack[39] = CFrame.new(0, 0, 0, r00, r01, r02, r10, r11, r12, r20, r21, r22)
        stack[44] = CFrame.new(0, 0, 0, r00, r01, r02, r10, r11, r12, r20, r21, r22)
        stack[45] = dir
        stack[46] = dir
        
        return selfRef.OriginalBufferCreate(size, ...)
    end)
end

function SilentAim:RemoveBufferHook()
    if self.BufferHook then
        hookfunction(buffer.create, self.OriginalBufferCreate)
        self.BufferHook = nil
        self.OriginalBufferCreate = nil
    end
end

function SilentAim:ApplySilentAim(target)
    -- Handled by buffer hook
end

-- ============================================================
-- VISIBLE AIMBOT IMPLEMENTATION
-- ============================================================
function VisibleAimbot:Start()
    if self.Connection then return end
    
    self.Connection = Services.RunService.RenderStepped:Connect(function()
        if not self.Enabled then 
            self.CurrentTarget = nil
            return 
        end
        
        if self.HoldMode and self.Keybind then
            if not Services.UserInputService:IsKeyDown(self.Keybind) then
                self.CurrentTarget = nil
                return
            end
        end
        
        local sourcePos = Camera.CFrame.Position
        
        local target = TargetSelector.GetBestTarget(
            self.FOV,
            self.AimPart,
            self.TeamCheck,
            sourcePos,
            self.MaxDistance,
            self.VisibleCheck,
            self.Prediction,
            "advanced"
        )
        
        if target then
            self.CurrentTarget = target
            self:ApplyAim(target)
        else
            self.CurrentTarget = nil
        end
    end)
end

function VisibleAimbot:Stop()
    if self.Connection then
        self.Connection:Disconnect()
        self.Connection = nil
    end
    self.CurrentTarget = nil
end

function VisibleAimbot:ApplyAim(target)
    local sourcePos = Camera.CFrame.Position
    local targetPos = target.Position
    
    local lookVector = (targetPos - sourcePos).Unit
    local targetCFrame = CFrame.lookAt(sourcePos, sourcePos + lookVector)
    
    if self.Smoothing > 0 then
        local currentCFrame = Camera.CFrame
        local smoothFactor = math.clamp(self.Smoothing, 0.01, 1)
        
        local targetQuat = targetCFrame:ToQuaternion()
        local currentQuat = currentCFrame:ToQuaternion()
        
        local lerpedQuat = currentQuat:Slerp(targetQuat, smoothFactor)
        targetCFrame = CFrame.new(sourcePos) * CFrame.fromQuaternion(lerpedQuat)
    end
    
    Camera.CFrame = targetCFrame
end

-- ============================================================
-- TRIGGERBOT IMPLEMENTATION
-- ============================================================
function TriggerBot:Start()
    if self.Connection then return end
    
    self.Connection = Services.RunService.RenderStepped:Connect(function()
        if not self.Enabled then return end
        
        local mouse = Services.UserInputService:GetMouseLocation()
        local sourcePos = Camera.CFrame.Position
        
        local ray = Camera:ViewportPointToRay(mouse.X, mouse.Y, 1)
        local params = RaycastParams.new()
        params.FilterType = Enum.RaycastFilterType.Blacklist
        params.FilterDescendantsInstances = {LocalPlayer.Character}
        
        local result = Services.Workspace:Raycast(ray.Origin, ray.Direction * 1000, params)
        
        if result then
            local hit = result.Instance
            if hit and hit:IsDescendantOf(Services.Workspace) then
                local entity = hit:FindFirstAncestorOfClass("Model")
                if entity and entity:FindFirstChild("Humanoid") then
                    local player = Services.Players:GetPlayerFromCharacter(entity)
                    if player and player ~= LocalPlayer then
                        if self.TeamCheck and LocalPlayer.Team and player.Team == LocalPlayer.Team then
                            return
                        end
                        
                        if self.VisibleCheck then
                            local visible = Visibility.IsVisible(sourcePos, hit.Position)
                            if not visible then return
                        end
                        
                        local now = tick()
                        if now - self.LastShot >= self.Delay then
                            self.LastShot = now
                            Services.UserInputService:GetMouseLocation()
                        end
                    end
                end
            end
        end
    end)
end

function TriggerBot:Stop()
    if self.Connection then
        self.Connection:Disconnect()
        self.Connection = nil
    end
    self.LastShot = 0
end

-- ============================================================
-- NOVA SILENT AIM UI
-- ============================================================
SilentAimSection:Toggle({
    Name = "Silent Aim",
    Flag = "SilentAim",
    Default = false,
    Callback = function(State)
        SilentAim.Enabled = State
        if State then
            SilentAim:Start()
        else
            SilentAim:Stop()
        end
    end
})

SilentAimSection:Slider({
    Name = "FOV",
    Flag = "SilentFOV",
    Min = 1,
    Max = 500,
    Default = 120,
    Callback = function(Value)
        SilentAim.FOV = Value
    end
})

SilentAimSection:Slider({
    Name = "Smoothing",
    Flag = "SilentSmoothing",
    Min = 0,
    Max = 1,
    Default = 0.3,
    Decimals = 2,
    Callback = function(Value)
        SilentAim.Smoothing = Value
    end
})

SilentAimSection:Dropdown({
    Name = "Aim Part",
    Flag = "SilentAimPart",
    Options = { 'Head', 'UpperTorso', 'LowerTorso' },
    Default = 'Head',
    Callback = function(Value)
        SilentAim.AimPart = Value
    end
})

SilentAimSection:Toggle({
    Name = "Team Check",
    Flag = "SilentTeamCheck",
    Default = true,
    Callback = function(State)
        SilentAim.TeamCheck = State
    end
})

SilentAimSection:Toggle({
    Name = "Visible Check",
    Flag = "SilentVisibleCheck",
    Default = true,
    Callback = function(State)
        SilentAim.VisibleCheck = State
    end
})

SilentAimSection:Toggle({
    Name = "Prediction",
    Flag = "SilentPrediction",
    Default = true,
    Callback = function(State)
        SilentAim.Prediction = State
    end
})

SilentAimSection:Toggle({
    Name = "No Spread",
    Flag = "SilentNoSpread",
    Default = false,
    Callback = function(State)
        SilentAim.NoSpread = State
    end
})

SilentAimSection:Toggle({
    Name = "Instant Bullet",
    Flag = "SilentInstantBullet",
    Default = false,
    Callback = function(State)
        SilentAim.InstantBullet = State
    end
})

SilentAimSection:Toggle({
    Name = "Resolve X",
    Flag = "SilentResolveX",
    Default = false,
    Callback = function(State)
        SilentAim.ResolveX = State
    end
})

SilentAimSection:Toggle({
    Name = "Resolve Y",
    Flag = "SilentResolveY",
    Default = false,
    Callback = function(State)
        SilentAim.ResolveY = State
    end
})

SilentAimSection:Slider({
    Name = "Max Distance",
    Flag = "SilentMaxDist",
    Min = 50,
    Max = 1000,
    Default = 500,
    Callback = function(Value)
        SilentAim.MaxDistance = Value
    end
})

-- ============================================================
-- NOVA VISIBLE AIMBOT UI
-- ============================================================
VisibleAimbotSection:Toggle({
    Name = "Visible Aimbot",
    Flag = "VisibleAimbot",
    Default = false,
    Callback = function(State)
        VisibleAimbot.Enabled = State
        if State then
            VisibleAimbot:Start()
        else
            VisibleAimbot:Stop()
        end
    end
})

VisibleAimbotSection:Slider({
    Name = "FOV",
    Flag = "VisibleFOV",
    Min = 1,
    Max = 500,
    Default = 200,
    Callback = function(Value)
        VisibleAimbot.FOV = Value
    end
})

VisibleAimbotSection:Slider({
    Name = "Smoothing",
    Flag = "VisibleSmoothing",
    Min = 0,
    Max = 1,
    Default = 0.5,
    Decimals = 2,
    Callback = function(Value)
        VisibleAimbot.Smoothing = Value
    end
})

VisibleAimbotSection:Dropdown({
    Name = "Aim Part",
    Flag = "VisibleAimPart",
    Options = { 'Head', 'UpperTorso', 'LowerTorso' },
    Default = 'Head',
    Callback = function(Value)
        VisibleAimbot.AimPart = Value
    end
})

VisibleAimbotSection:Toggle({
    Name = "Team Check",
    Flag = "VisibleTeamCheck",
    Default = true,
    Callback = function(State)
        VisibleAimbot.TeamCheck = State
    end
})

VisibleAimbotSection:Toggle({
    Name = "Visible Check",
    Flag = "VisibleVisibleCheck",
    Default = true,
    Callback = function(State)
        VisibleAimbot.VisibleCheck = State
    end
})

VisibleAimbotSection:Toggle({
    Name = "Prediction",
    Flag = "VisiblePrediction",
    Default = true,
    Callback = function(State)
        VisibleAimbot.Prediction = State
    end
})

VisibleAimbotSection:Toggle({
    Name = "Hold Mode",
    Flag = "VisibleHoldMode",
    Default = false,
    Callback = function(State)
        VisibleAimbot.HoldMode = State
    end
})

VisibleAimbotSection:Dropdown({
    Name = "Keybind",
    Flag = "VisibleKeybind",
    Options = { 'RightButton', 'LeftButton', 'Q', 'E', 'F', 'G', 'MouseButton2' },
    Default = 'RightButton',
    Callback = function(Value)
        VisibleAimbot.Keybind = Value
    end
})

-- ============================================================
-- NOVA TRIGGERBOT UI
-- ============================================================
TriggerBotSection:Toggle({
    Name = "TriggerBot",
    Flag = "TriggerBot",
    Default = false,
    Callback = function(State)
        TriggerBot.Enabled = State
        if State then
            TriggerBot:Start()
        else
            TriggerBot:Stop()
        end
    end
})

TriggerBotSection:Slider({
    Name = "Delay (seconds)",
    Flag = "TriggerDelay",
    Min = 0.01,
    Max = 0.5,
    Default = 0.1,
    Decimals = 2,
    Callback = function(Value)
        TriggerBot.Delay = Value
    end
})

TriggerBotSection:Toggle({
    Name = "Team Check",
    Flag = "TriggerTeamCheck",
    Default = true,
    Callback = function(State)
        TriggerBot.TeamCheck = State
    end
})

TriggerBotSection:Toggle({
    Name = "Visible Check",
    Flag = "TriggerVisibleCheck",
    Default = true,
    Callback = function(State)
        TriggerBot.VisibleCheck = State
    end
})

-- ============================================================
-- CLEANUP FUNCTION
-- ============================================================
local function CleanupAll()
    SilentAim:Stop()
    VisibleAimbot:Stop()
    TriggerBot:Stop()
end

-- ============================================================
-- NOVA WEAPON ESP
-- ============================================================
do
    local miscFolder = workspace.world_assets.StaticObjects.Misc
    local gunDataFolder = game:GetService("ReplicatedStorage").GunSystemAssets.GunData
    local RunService = game:GetService("RunService")
    local Players = game:GetService("Players")
    local camera = workspace.CurrentCamera
    local LocalPlayer = Players.LocalPlayer

    local meleeBlacklist = {
        AxeFire = true, ChemLight = true, Cleaver = true, CombatKnife = true,
        Crowbar = true, HandSaw = true, Shovel = true, KitchenKnife = true,
        GrenadeSmokeMakeshift = true, GrenadeSmoke = true, AxeMakeshift = true,
        Cuffs = true, Fists = true, GenericItemThrow = true, Generic_consumable = true,
        GrenadeFrag = true, GrenadeGas = true, GrenadeMolotov = true,
        GrenadePipebomb = true, Snowball = true, ThrowableBottle = true,
        Wrench = true, SteakKnife = true,
    }

    local weaponESP = {
        Enabled = false,
        Size = 16,
        Color = Color3.fromRGB(255, 255, 255),
        ExcludeMelee = true,
        ShowDistance = true,
        RenderRange = 10000,
        Font = 'Code',
        Active = {},
    }

    local function getMeshPartsInfo(model)
        local info = {}
        for _, child in ipairs(model:GetChildren()) do
            if child:IsA("MeshPart") then
                local cNames = {}
                for _, c in ipairs(child:GetChildren()) do
                    table.insert(cNames, c.Name)
                end
                table.sort(cNames)
                table.insert(info, { Name = child.Name, MeshId = child.MeshId, ChildrenNames = cNames })
            end
        end
        table.sort(info, function(a, b) return a.Name < b.Name end)
        return info
    end

    local function meshPartsMatch(a, b)
        if #a ~= #b then return false end
        for i = 1, #a do
            if a[i].Name ~= b[i].Name or a[i].MeshId ~= b[i].MeshId then
                return false
            end
            local c1, c2 = a[i].ChildrenNames, b[i].ChildrenNames
            if #c1 ~= #c2 then return false end
            for j = 1, #c1 do
                if c1[j] ~= c2[j] then return false end
            end
        end
        return true
    end

    local function getChildrenNames(model)
        local names = {}
        for _, c in ipairs(model:GetChildren()) do
            table.insert(names, c.Name)
        end
        table.sort(names)
        return names
    end

    local function childrenNamesMatch(a, b)
        if #a ~= #b then return false end
        for i = 1, #a do
            if a[i] ~= b[i] then return false end
        end
        return true
    end

    local function getDescendantsNames(model)
        local names = {}
        for _, d in ipairs(model:GetDescendants()) do
            table.insert(names, d.Name)
        end
        table.sort(names)
        return names
    end

    local function descendantsNamesMatch(a, b)
        if #a ~= #b then return false end
        for i = 1, #a do
            if a[i] ~= b[i] then return false end
        end
        return true
    end

    local gunDataMeshInfos = {}
    for _, folder in ipairs(gunDataFolder:GetChildren()) do
        local wm = folder:FindFirstChild("WorldModel")
        if wm then
            gunDataMeshInfos[folder.Name] = getMeshPartsInfo(wm)
        end
    end

    local function checkModelAgainstGunData(model)
        if not model:IsA("Model") then return nil end
        local meshInfo = getMeshPartsInfo(model)
        if #meshInfo > 0 then
            for name, refInfo in pairs(gunDataMeshInfos) do
                if meshPartsMatch(meshInfo, refInfo) then
                    return name
                end
            end
        else
            local childNames = getChildrenNames(model)
            local candidates = {}
            for name, _ in pairs(gunDataMeshInfos) do
                local gd = gunDataFolder:FindFirstChild(name)
                if gd and gd:FindFirstChild("WorldModel") then
                    if childrenNamesMatch(childNames, getChildrenNames(gd.WorldModel)) then
                        table.insert(candidates, name)
                    end
                end
            end
            if #candidates == 1 then
                return candidates[1]
            elseif #candidates > 1 then
                local descNames = getDescendantsNames(model)
                for _, name in ipairs(candidates) do
                    if descendantsNamesMatch(descNames, getDescendantsNames(gunDataFolder[name].WorldModel)) then
                        return name
                    end
                end
            end
        end
        return nil
    end

    local function createWeaponBillboard(model, gunName)
        if weaponESP.Active[model] then return weaponESP.Active[model] end

        local handle = model:FindFirstChild("Handle")
        if not (handle and handle:IsA("BasePart")) then return end

        local billboard = Instance.new('BillboardGui')
        billboard.Name = 'NovaWeaponESP'
        billboard.Adornee = handle
        billboard.Size = UDim2.new(0, 200, 0, 50)
        billboard.StudsOffset = Vector3.new(0, 2.5, 0)
        billboard.AlwaysOnTop = true
        billboard.ResetOnSpawn = false
        billboard.Parent = model

        local label = Instance.new('TextLabel')
        label.Name = 'WeaponText'
        label.BackgroundTransparency = 1
        label.TextColor3 = weaponESP.Color
        label.TextSize = weaponESP.Size
        label.Font = Enum.Font[weaponESP.Font] or Enum.Font.Code
        label.Text = ''
        label.Size = UDim2.new(1, 0, 1, 0)
        label.TextWrapped = true
        label.TextStrokeColor3 = Color3.new(0, 0, 0)
        label.TextStrokeTransparency = 0
        label.Parent = billboard

        local data = {
            billboard = billboard,
            label = label,
            model = model,
            name = gunName,
            handle = handle
        }
        weaponESP.Active[model] = data
        return data
    end

    local function checkAndCreateESPForModel(model)
        local gunName = checkModelAgainstGunData(model)
        if gunName then
            if weaponESP.ExcludeMelee and meleeBlacklist[gunName] then return end
            createWeaponBillboard(model, gunName)
        end
    end

    local function updateWeaponESP()
        local origin = (LocalPlayer.Character and LocalPlayer.Character.PrimaryPart and LocalPlayer.Character.PrimaryPart.Position) or camera.CFrame.Position

        for model, data in pairs(weaponESP.Active) do
            if not data.handle or not data.handle.Parent or not model.Parent then
                if data.billboard then
                    pcall(function() data.billboard:Destroy() end)
                end
                weaponESP.Active[model] = nil
            else
                local dist = (data.handle.Position - origin).Magnitude

                if weaponESP.Enabled and dist <= weaponESP.RenderRange then
                    local text = data.name
                    if weaponESP.ShowDistance then
                        text = string.format("%s %dm", data.name, math.floor(dist))
                    end

                    data.label.Text = text
                    data.label.TextColor3 = weaponESP.Color
                    data.label.TextSize = weaponESP.Size
                    data.label.Font = Enum.Font[weaponESP.Font] or Enum.Font.Code

                    local transparency = math.max(0.1, 1 - (dist / weaponESP.RenderRange))
                    data.label.TextStrokeTransparency = 1 - transparency
                    data.label.TextTransparency = 1 - transparency

                    data.billboard.Enabled = true
                else
                    data.billboard.Enabled = false
                end
            end
        end
    end

    local function initialScan()
        for _, model in ipairs(miscFolder:GetChildren()) do
            checkAndCreateESPForModel(model)
        end
    end

    miscFolder.ChildAdded:Connect(function(newModel)
        task.wait(0.1)
        checkAndCreateESPForModel(newModel)
    end)

    local updateConn = RunService.Heartbeat:Connect(updateWeaponESP)

    WeaponESP:Toggle({
        Name = "Weapon ESP",
        Flag = "WeaponESP",
        Default = weaponESP.Enabled,
        Callback = function(State)
            weaponESP.Enabled = State
            if State then
                task.defer(initialScan)
            else
                for _, data in pairs(weaponESP.Active) do
                    if data.billboard then
                        pcall(function() data.billboard.Enabled = false end)
                    end
                end
            end
        end
    }):Colorpicker({
        Name = "Weapon Color",
        Flag = "WeaponColor",
        Default = weaponESP.Color,
        Callback = function(Value)
            weaponESP.Color = Value
            for _, data in pairs(weaponESP.Active) do
                pcall(function()
                    if data.label then data.label.TextColor3 = Value end
                end)
            end
        end
    })

    WeaponESP:Dropdown({
        Name = "Text Font",
        Flag = "WeaponFont",
        Options = {
            'Code', 'Arial', 'ArialBold', 'Cartoon', 'Fantasy',
            'Garamond', 'Gotham', 'GothamBold', 'GothamMedium',
            'GothamSemibold', 'Highway', 'Legacy', 'Roboto',
            'RobotoMono', 'SourceSans', 'SourceSansBold',
            'SourceSansItalic', 'SourceSansSemibold',
        },
        Default = 'Code',
        Callback = function(value)
            weaponESP.Font = value
            for _, data in pairs(weaponESP.Active) do
                pcall(function()
                    if data.label then
                        data.label.Font = Enum.Font[value] or Enum.Font.Code
                    end
                end)
            end
        end
    })

    WeaponESP:Slider({
        Name = "Max Distance",
        Flag = "WeaponRenderDistance",
        Min = 0,
        Max = 1000,
        Default = weaponESP.RenderRange,
        Decimals = 1,
        Callback = function(Value)
            weaponESP.RenderRange = Value
        end
    })

    if weaponESP.Enabled then
        task.defer(initialScan)
    end
end

print("Nova loaded successfully!")
]])()
