-- ================================================================
-- RUZHUB + TOOLBOXHUB - ПОЛНЫЙ МЕРЖ ВСЕХ ФУНКЦИЙ
-- ================================================================

local Players = game:GetService('Players')
local Workspace = game:GetService('Workspace')
local RunService = game:GetService('RunService')
local UserInputService = game:GetService('UserInputService')
local ReplicatedStorage = game:GetService('ReplicatedStorage')
local TweenService = game:GetService('TweenService')
local HttpService = game:GetService('HttpService')
local Lighting = game:GetService('Lighting')
local CoreGui = game:GetService('CoreGui')
local TextChatService = game:GetService('TextChatService')
local GuiService = game:GetService('GuiService')
local TeleportService = game:GetService('TeleportService')

local LocalPlayer = Players.LocalPlayer
local CurrentCamera = Workspace.CurrentCamera

-- ================================================================
-- ЗАГРУЗКА WINDUI
-- ================================================================
local WindUI = loadstring(game:HttpGet('https://raw.githubusercontent.com/Footagesus/WindUI/refs/heads/main/dist/main.lua'))()
WindUI:SetTheme('Crimson')

-- ================================================================
-- ВСЕ ПЕРЕМЕННЫЕ СОСТОЯНИЯ ИЗ TOOLBOXHUB
-- ================================================================
local Settings = {
    -- ESP
    ESPEnabled = false,
    ESPEveryone = false,
    ESPMurderer = false,
    ESPSheriff = false,
    ESPInnocent = false,
    ESPGun = false,
    ESPCoin = false,
    
    OutlineEnabled = false,
    OutlineEveryone = false,
    OutlineMurderer = false,
    OutlineSheriff = false,
    OutlineInnocent = false,
    OutlineGun = false,
    
    ChamsEnabled = false,
    ChamsEveryone = false,
    ChamsMurderer = false,
    ChamsSheriff = false,
    ChamsInnocent = false,
    ChamsGun = false,
    ChamsCoin = false,
    
    TracersEnabled = false,
    TracersEveryone = false,
    TracersMurderer = false,
    TracersSheriff = false,
    TracersInnocent = false,
    TracersGun = false,
    TracersCoin = false,
    
    BoxEnabled = false,
    BoxEveryone = false,
    BoxMurderer = false,
    BoxSheriff = false,
    BoxInnocent = false,
    BoxGun = false,
    BoxCoin = false,
    
    -- Combat
    SilentAim = false,
    TriggerBot = false,
    TriggerBotShiftLockOnly = false,
    AutoKillMurderer = false,
    AutoKillAll = false,
    AutoGrab = false,
    AutoThrow = false,
    GunDropNotify = false,
    
    -- Local Player
    Noclip = false,
    Fly = false,
    InfiniteJump = false,
    SpeedGlitch = false,
    OnlySideways = false,
    Invisible = false,
    AntiFling = false,
    AntiVoid = false,
    XRay = false,
    
    -- Movement
    WalkSpeed = 16,
    JumpPower = 50,
    normalWalkSpeed = 16,
    
    -- Prediction
    PredictionEnabled = true,
    PredictionMultiplier = 16.5,
    YClampMin = -2,
    YClampMax = 2.65,
    lastVel = {},
    
    -- Aimlock
    AimlockEnabled = false,
    AimlockMurderer = false,
    AimlockSheriff = false,
    AimlockSelected = false,
    AimlockSmoothness = 10,
    aimlockTarget = nil,
    
    -- Fling
    TouchFlingEnabled = false,
    isFlinging = false,
    flingTarget = nil,
    flingOldPos = nil,
    flingAngle = 0,
    flingHighVelCount = 0,
    flingQueue = {},
    flingQueueIndex = 1,
    isFlingingAll = false,
    trueOriginalPos = nil,
    
    -- Auto Farm
    AutoFarmEnabled = false,
    AutoFarmTweenSpeed = 25,
    AutoFarmStartCFrame = nil,
    AutoFarmCurrentTargetCoin = nil,
    AutoFarmCoinRegistry = {},
    AutoFarmSessionCoinsCollected = 0,
    AutoFarmSessionStartTime = 0,
    CoinsStarted = false,
    CoinsFull = false,
    CoinsCollected = 0,
    AutoFarmOriginalNoclipState = false,
    AutoFarmBodyMovers = {},
    AutoFarmAntigravBV = nil,
    AutoFarmActiveTween = nil,
    PostFarmKillMurd = false,
    PostFarmKillAll = false,
    PostFarmFlingMurd = false,
    postfarmresetin = false,
    postfarmresetsh = false,
    postfarmresetmurd = false,
    
    -- Bomb Jump
    BombJumpEnabled = false,
    BombJumpAutoGet = false,
    BombJumpOnCooldown = false,
    BombJumpDebounce = false,
    BombJumpJustRespawned = false,
    
    -- Webhook
    WebhookEnabled = false,
    WebhookURL = '',
    WebhookInterval = 10,
    WebhookLastSent = 0,
    WebhookOnFull = false,
    
    -- Rejoin
    AutoRejoinEnabled = false,
    AutoRejoinTarget = nil,
    
    -- Roles
    roleTable = {},
    currentMurderer = nil,
    currentSheriff = nil,
    currentHero = nil,
    prevRoles = {},
    killingPlayer = nil,
    silentAimCooldown = 0,
    notifiedGunDrops = {},
    
    -- Status Overlay
    StatusOverlayEnabled = false,
    
    -- FE Animations
    FEAnimEnabled = false,
    FEAnimState = { all = 'Default', idle = 'Default', walk = 'Default', run = 'Default', jump = 'Default', climb = 'Default', fall = 'Default' },
    FEAnimOriginals = {},
    FEAnimPresets = {},
    FEAnimMap = {},
    
    -- Misc
    grabConn = nil,
    touchFlingThread = nil,
    flickInProgress = false,
    autoThrowReady = true,
    autoKillMurdererReady = true,
    autoKillReady = true,
    CoinAura = false,
    Trickshot = false,
    DualEffectEnabled = false,
    DualEffectSelected = 'Electric',
    ShowRoundTimer = false,
    ExposeRoles = false,
    InstantRoleNotify = false,
    ShowMurdererChance = false,
    StatusOverlayEnabled = false,
}

-- ================================================================
-- ТАБЛИЦЫ ДЛЯ ESP И ОБЪЕКТОВ
-- ================================================================
local espObjects = {}
local highlightObjects = {}
local gunEspObjects = {}
local gunHighlightObjects = {}
local coinEspObjects = {}
local coinHighlightObjects = {}
local xrayParts = {}
local connections = {}
local floatingGui = nil
local buttonStyles = {}

local Colors = {
    Murderer = Color3.fromRGB(255, 40, 40),
    Sheriff = Color3.fromRGB(40, 130, 255),
    Hero = Color3.fromRGB(255, 215, 0),
    Innocent = Color3.fromRGB(0, 220, 0),
    Unknown = Color3.fromRGB(190, 190, 190),
    Gun = Color3.fromRGB(70, 130, 255),
    Coin = Color3.fromRGB(255, 215, 0),
    Name = Color3.fromRGB(255, 255, 255),
}

-- ================================================================
-- ВСПОМОГАТЕЛЬНЫЕ ФУНКЦИИ (ИЗ TOOLBOXHUB)
-- ================================================================
local function GetRole(player)
    local data = Settings.roleTable[player.Name]
    return data and data.Role or 'Unknown'
end

local function IsDead(player)
    local data = Settings.roleTable[player.Name]
    return data and data.Dead == true
end

local function IsHeroEligible(player)
    local char = player.Character
    local backpack = player:FindFirstChild('Backpack')
    if char and char:FindFirstChild('Gun') then return true end
    if backpack and backpack:FindFirstChild('Gun') then return true end
    return false
end

local function GetDisplayColor(role)
    if role == 'Murderer' then return Colors.Murderer end
    if role == 'Sheriff' then return Colors.Sheriff end
    if role == 'Hero' then return Colors.Hero end
    if role == 'Innocent' then return Colors.Innocent end
    return Colors.Unknown
end

local function ShouldShow(category, role)
    local settings = Settings[category]
    if not settings then return false end
    if settings.Everyone then return true end
    if role == 'Murderer' and settings.Murderer then return true end
    if (role == 'Sheriff' or role == 'Hero') and settings.Sheriff then return true end
    if role == 'Innocent' and settings.Innocent then return true end
    return false
end

local function ShouldShowGun(category)
    return Settings[category .. 'Gun'] or false
end

local function ShouldShowCoin(category)
    return Settings[category .. 'Coin'] or false
end

local function IsCollectibleCoin(part)
    return part and part:IsA('BasePart') and part.Parent and part.Parent.Name == 'CoinContainer'
end

local function GetColorString(color, text)
    local r = math.floor(color.R * 255)
    local g = math.floor(color.G * 255)
    local b = math.floor(color.B * 255)
    return string.format('<font color="rgb(%d,%d,%d)">%s</font>', r, g, b, text or '')
end

local function SecondsToMinutes(seconds)
    if not seconds or type(seconds) ~= 'number' then return '0:00' end
    local mins = math.floor(seconds / 60)
    local secs = math.floor(seconds % 60)
    return string.format('%d:%02d', mins, secs)
end

local function IsRoundOngoing()
    local timer = Workspace:FindFirstChild('RoundTimerPart')
    if not timer then return false end
    local time = timer:GetAttribute('Time')
    return type(time) == 'number' and time > 0
end

local function Trim(str)
    if type(str) ~= 'string' then return str end
    return str:match('^%s*(.-)%s*$') or str
end

-- ================================================================
-- ФУНКЦИИ ОБНОВЛЕНИЯ РОЛЕЙ (ИЗ TOOLBOXHUB)
-- ================================================================
local function UpdateCachedRoles()
    Settings.currentMurderer = nil
    Settings.currentSheriff = nil
    Settings.currentHero = nil
    
    for _, player in ipairs(Players:GetPlayers()) do
        local role = GetRole(player)
        if role == 'Murderer' and not IsDead(player) then
            Settings.currentMurderer = player
        end
        if role == 'Sheriff' and not IsDead(player) then
            Settings.currentSheriff = player
        end
        if role == 'Hero' and not IsDead(player) then
            Settings.currentHero = player
        end
    end
    
    if not Settings.currentSheriff and not Settings.currentHero and IsRoundOngoing() then
        for _, player in ipairs(Players:GetPlayers()) do
            local role = GetRole(player)
            if (role == 'Innocent' or role == 'Unknown') and not IsDead(player) and IsHeroEligible(player) then
                Settings.currentHero = player
                break
            end
        end
    end
end

local function RefreshRoles()
    local remote = ReplicatedStorage:FindFirstChild('GetPlayerData', true)
    if remote then
        local ok, result = pcall(function() return remote:InvokeServer() end)
        if ok and type(result) == 'table' then
            Settings.roleTable = result
        end
    end
    UpdateCachedRoles()
end

local function CheckRoleNotify()
    local role = GetRole(LocalPlayer)
    local dead = IsDead(LocalPlayer)
    local prevRole = Settings.prevRoles.__local__ or 'Unknown'
    
    if role ~= prevRole and not dead then
        if role == 'Murderer' and prevRole ~= 'Murderer' and Settings.DualEffectEnabled then
            local remotes = ReplicatedStorage:FindFirstChild('Remotes', true)
            if remotes then
                local equip = remotes:FindFirstChild('Inventory', true):FindFirstChild('Equip')
                if equip then
                    equip:FireServer('Dual', 'Effects')
                    task.delay(15, function()
                        if Settings.DualEffectEnabled then
                            equip:FireServer(Settings.DualEffectSelected, 'Effects')
                        end
                    end)
                end
            end
        end
        
        Settings.prevRoles.__local__ = role
        
        if role ~= 'Unknown' and Settings.InstantRoleNotify then
            pcall(function()
                WindUI:Notify({
                    Title = 'Role Assigned',
                    Content = GetColorString(GetDisplayColor(role), role),
                    Duration = 5,
                })
            end)
        end
        
        if prevRole == 'Unknown' and role ~= 'Unknown' and Settings.ShowMurdererChance then
            pcall(function()
                local remote = ReplicatedStorage:FindFirstChild('Remotes', true)
                if remote then
                    local chance = remote:FindFirstChild('Extras', true):FindFirstChild('GetChance')
                    if chance then
                        local result = chance:InvokeServer()
                        if type(result) == 'number' then
                            WindUI:Notify({
                                Title = 'Murderer Chance',
                                Content = GetColorString(Colors.Murderer, tostring(result) .. '%'),
                                Duration = 5,
                            })
                        end
                    end
                end
            end)
        end
    elseif dead then
        Settings.prevRoles.__local__ = 'Dead'
    elseif not dead and role == 'Unknown' then
        Settings.prevRoles.__local__ = 'Unknown'
    end
    
    for _, player in ipairs(Players:GetPlayers()) do
        if player ~= LocalPlayer then
            local role2 = GetRole(player)
            local dead2 = IsDead(player)
            local prev = Settings.prevRoles[player.Name]
            local prevRole2 = prev and prev.Role or 'Unknown'
            
            if role2 ~= prevRole2 and not dead2 then
                Settings.prevRoles[player.Name] = { Role = role2, Dead = dead2 }
                
                if role2 ~= 'Unknown' and Settings.ExposeRoles then
                    if role2 == 'Murderer' or role2 == 'Sheriff' or role2 == 'Hero' then
                        pcall(function()
                            WindUI:Notify({
                                Title = player.Name,
                                Content = GetColorString(GetDisplayColor(role2), role2),
                                Duration = 5,
                            })
                        end)
                    end
                end
            elseif dead2 then
                Settings.prevRoles[player.Name] = { Role = 'Dead', Dead = true }
            elseif not dead2 and role2 == 'Unknown' then
                Settings.prevRoles[player.Name] = { Role = 'Unknown', Dead = false }
            end
        end
    end
end

-- ================================================================
-- ФУНКЦИИ BLURT/EXPOSE (ИЗ TOOLBOXHUB)
-- ================================================================
local function BlurtSheriff()
    local sheriff = Settings.currentSheriff or Settings.currentHero
    if sheriff then
        pcall(function()
            TextChatService.TextChannels.RBXGeneral:SendAsync('The Sheriff is : ' .. sheriff.Name)
        end)
    end
end

local function BlurtMurderer()
    if Settings.currentMurderer then
        pcall(function()
            TextChatService.TextChannels.RBXGeneral:SendAsync('The Murderer is : ' .. Settings.currentMurderer.Name)
        end)
    end
end

local function BlurtBoth()
    BlurtSheriff()
    BlurtMurderer()
end

local function ExposeSheriff()
    local sheriff = Settings.currentSheriff or Settings.currentHero
    if sheriff then
        local role = GetRole(sheriff)
        local displayRole = role == 'Hero' and 'Hero' or 'Sheriff'
        local color = role == 'Hero' and Colors.Hero or Colors.Sheriff
        WindUI:Notify({
            Title = sheriff.Name,
            Content = GetColorString(color, displayRole),
            Duration = 5,
        })
    end
end

local function ExposeMurderer()
    if Settings.currentMurderer then
        WindUI:Notify({
            Title = Settings.currentMurderer.Name,
            Content = GetColorString(Colors.Murderer, 'Murderer'),
            Duration = 5,
        })
    end
end

local function ExposeBoth()
    ExposeSheriff()
    ExposeMurderer()
end

-- ================================================================
-- ФУНКЦИИ ПРЕДСКАЗАНИЯ (ИЗ TOOLBOXHUB)
-- ================================================================
local function GetPredictedPos(part)
    if not part or not part:IsA('BasePart') then
        return part and part.Position or Vector3.zero
    end
    
    if not Settings.PredictionEnabled then
        return part.Position
    end
    
    local char = LocalPlayer.Character
    if not char then return part.Position end
    
    local hrp = char:FindFirstChild('HumanoidRootPart')
    if not hrp then return part.Position end
    
    local parent = part.Parent
    local humanoid = parent and parent:FindFirstChildOfClass('Humanoid')
    local state = humanoid and humanoid:GetState() or Enum.HumanoidStateType.None
    
    local dist = (hrp.Position - part.Position).Magnitude
    local ping = LocalPlayer:GetNetworkPing() or 0
    local predTime = math.clamp(dist / math.max(part.AssemblyLinearVelocity.Magnitude + 800, 800) * 0.6 + ping, 0.005, 0.12)
    
    local vel = part.AssemblyLinearVelocity
    if not Settings.lastVel[part] then
        Settings.lastVel[part] = vel
    end
    
    local velDiff = (vel - Settings.lastVel[part]).Magnitude
    local lerpFactor = velDiff > 50 and 0.8 or 0.35
    local smoothedVel = Settings.lastVel[part]:Lerp(vel, lerpFactor)
    Settings.lastVel[part] = smoothedVel
    
    local predicted = part.Position + smoothedVel * predTime
    
    if state == Enum.HumanoidStateType.Jumping or state == Enum.HumanoidStateType.Freefall or state == Enum.HumanoidStateType.GettingUp then
        predicted = predicted + Vector3.new(0, workspace.Gravity * predTime * predTime * 0.12, 0)
        if smoothedVel.Y > 0 then
            predicted = predicted + Vector3.new(0, smoothedVel.Y * predTime * 0.08, 0)
        end
    end
    
    local yDiff = predicted.Y - part.Position.Y
    local yClamp = state == Enum.HumanoidStateType.Jumping or state == Enum.HumanoidStateType.Freefall and 
                   math.clamp(yDiff, -dist * 0.08, dist * 0.12) or 
                   math.clamp(yDiff, -dist * 0.03, dist * 0.05)
    
    local headOffset = 1
    if parent then
        local head = parent:FindFirstChild('Head')
        local root = parent:FindFirstChild('HumanoidRootPart')
        if head and root then
            headOffset = math.abs(head.Position.Y - root.Position.Y) + 1
        end
    end
    
    local result = Vector3.new(predicted.X, part.Position.Y + yClamp + headOffset * 0.25, predicted.Z)
    local maxDist = math.max(dist * 0.25, 8)
    local diff = result - part.Position
    if maxDist < diff.Magnitude then
        result = part.Position + diff.Unit * maxDist
    end
    
    return result
end

-- ================================================================
-- ФУНКЦИИ ESP (ИЗ TOOLBOXHUB)
-- ================================================================
local function CreateESP(player)
    if espObjects[player] then return end
    
    local box = {}
    for i = 1, 4 do
        local line = Drawing.new('Line')
        line.Visible = false
        line.Color = Color3.fromRGB(255, 255, 255)
        line.Thickness = 1
        line.Transparency = 1
        box[i] = line
    end
    
    local billboard = Instance.new('BillboardGui')
    billboard.Size = UDim2.new(0, 220, 0, 60)
    billboard.StudsOffset = Vector3.new(0, 2.2, 0)
    billboard.AlwaysOnTop = true
    billboard.LightInfluence = 0
    billboard.Enabled = false
    billboard.Parent = CoreGui
    
    local nameLabel = Instance.new('TextLabel', billboard)
    nameLabel.BackgroundTransparency = 1
    nameLabel.TextStrokeTransparency = 0
    nameLabel.TextStrokeColor3 = Color3.new(0, 0, 0)
    nameLabel.Font = Enum.Font.Code
    nameLabel.TextSize = 14
    nameLabel.TextColor3 = Colors.Name
    nameLabel.Size = UDim2.new(1, 0, 0.4, 0)
    
    local roleLabel = Instance.new('TextLabel', billboard)
    roleLabel.BackgroundTransparency = 1
    roleLabel.TextStrokeTransparency = 0
    roleLabel.TextStrokeColor3 = Color3.new(0, 0, 0)
    roleLabel.Font = Enum.Font.Code
    roleLabel.TextSize = 13
    roleLabel.Size = UDim2.new(1, 0, 0.3, 0)
    roleLabel.Position = UDim2.new(0, 0, 0.4, 0)
    
    local distLabel = Instance.new('TextLabel', billboard)
    distLabel.BackgroundTransparency = 1
    distLabel.TextStrokeTransparency = 0
    distLabel.TextStrokeColor3 = Color3.new(0, 0, 0)
    distLabel.Font = Enum.Font.Ubuntu
    distLabel.TextSize = 12
    distLabel.TextColor3 = Color3.fromRGB(200, 200, 200)
    distLabel.Size = UDim2.new(1, 0, 0.3, 0)
    distLabel.Position = UDim2.new(0, 0, 0.7, 0)
    
    local tracer = Drawing.new('Line')
    tracer.Visible = false
    tracer.Color = Color3.fromRGB(255, 255, 255)
    tracer.Thickness = 1
    tracer.Transparency = 1
    
    espObjects[player] = {
        box = box,
        billboard = billboard,
        nameLabel = nameLabel,
        roleLabel = roleLabel,
        distLabel = distLabel,
        tracer = tracer,
    }
end

local function RemoveESP(player)
    local data = espObjects[player]
    if not data then return end
    for _, line in pairs(data.box) do line:Remove() end
    data.tracer:Remove()
    if data.billboard then data.billboard:Destroy() end
    espObjects[player] = nil
end

local function ApplyHighlight(player, color, fill, outline)
    local char = player.Character
    if not char then return end
    
    local hl = highlightObjects[player]
    if not hl then
        hl = Instance.new('Highlight')
        hl.DepthMode = Enum.HighlightDepthMode.AlwaysOnTop
        hl.Parent = CoreGui
        highlightObjects[player] = hl
    end
    
    hl.Adornee = char
    
    if fill and outline then
        hl.FillColor = color
        hl.FillTransparency = 0.4
        hl.OutlineColor = color
        hl.OutlineTransparency = 0
    elseif fill then
        hl.FillColor = color
        hl.FillTransparency = 0.4
        hl.OutlineTransparency = 1
    elseif outline then
        hl.FillTransparency = 1
        hl.OutlineColor = color
        hl.OutlineTransparency = 0
    end
end

local function RemoveHighlight(player)
    local hl = highlightObjects[player]
    if hl then
        pcall(function() hl.Adornee = nil end)
        pcall(function() hl.Enabled = false end)
        hl:Destroy()
        highlightObjects[player] = nil
    end
end

local function CreateGunESP(part)
    if gunEspObjects[part] then return end
    
    local box = {}
    for i = 1, 4 do
        local line = Drawing.new('Line')
        line.Visible = false
        line.Color = Colors.Gun
        line.Thickness = 1
        line.Transparency = 1
        box[i] = line
    end
    
    local billboard = Instance.new('BillboardGui')
    billboard.Size = UDim2.new(0, 160, 0, 45)
    billboard.StudsOffset = Vector3.new(0, 2.8, 0)
    billboard.AlwaysOnTop = true
    billboard.LightInfluence = 0
    billboard.Parent = CoreGui
    
    local label = Instance.new('TextLabel', billboard)
    label.BackgroundTransparency = 1
    label.TextStrokeTransparency = 0
    label.TextStrokeColor3 = Color3.new(0, 0, 0)
    label.Font = Enum.Font.Code
    label.TextSize = 14
    label.TextColor3 = Colors.Gun
    label.Text = '[GUN]'
    label.Size = UDim2.new(1, 0, 0.5, 0)
    
    local distLabel = Instance.new('TextLabel', billboard)
    distLabel.BackgroundTransparency = 1
    distLabel.TextStrokeTransparency = 0
    distLabel.TextStrokeColor3 = Color3.new(0, 0, 0)
    distLabel.Font = Enum.Font.Code
    distLabel.TextSize = 12
    distLabel.TextColor3 = Color3.fromRGB(200, 200, 200)
    distLabel.Size = UDim2.new(1, 0, 0.5, 0)
    distLabel.Position = UDim2.new(0, 0, 0.5, 0)
    
    local tracer = Drawing.new('Line')
    tracer.Visible = false
    tracer.Color = Colors.Gun
    tracer.Thickness = 1
    tracer.Transparency = 1
    
    gunEspObjects[part] = {
        box = box,
        billboard = billboard,
        label = label,
        distLabel = distLabel,
        tracer = tracer,
    }
end

local function RemoveGunESP(part)
    local data = gunEspObjects[part]
    if not data then return end
    for _, line in pairs(data.box) do line:Remove() end
    data.tracer:Remove()
    if data.billboard then data.billboard:Destroy() end
    gunEspObjects[part] = nil
end

local function CreateCoinESP(part)
    if coinEspObjects[part] then return end
    
    local box = {}
    for i = 1, 4 do
        local line = Drawing.new('Line')
        line.Visible = false
        line.Color = Colors.Coin
        line.Thickness = 1
        line.Transparency = 1
        box[i] = line
    end
    
    local billboard = Instance.new('BillboardGui')
    billboard.Size = UDim2.new(0, 160, 0, 45)
    billboard.StudsOffset = Vector3.new(0, 2.8, 0)
    billboard.AlwaysOnTop = true
    billboard.LightInfluence = 0
    billboard.Parent = CoreGui
    
    local label = Instance.new('TextLabel', billboard)
    label.BackgroundTransparency = 1
    label.TextStrokeTransparency = 0
    label.TextStrokeColor3 = Color3.new(0, 0, 0)
    label.Font = Enum.Font.Code
    label.TextSize = 14
    label.TextColor3 = Colors.Coin
    label.Text = '[COIN]'
    label.Size = UDim2.new(1, 0, 0.5, 0)
    
    local distLabel = Instance.new('TextLabel', billboard)
    distLabel.BackgroundTransparency = 1
    distLabel.TextStrokeTransparency = 0
    distLabel.TextStrokeColor3 = Color3.new(0, 0, 0)
    distLabel.Font = Enum.Font.Code
    distLabel.TextSize = 12
    distLabel.TextColor3 = Color3.fromRGB(200, 200, 200)
    distLabel.Size = UDim2.new(1, 0, 0.5, 0)
    distLabel.Position = UDim2.new(0, 0, 0.5, 0)
    
    local tracer = Drawing.new('Line')
    tracer.Visible = false
    tracer.Color = Colors.Coin
    tracer.Thickness = 1
    tracer.Transparency = 1
    
    coinEspObjects[part] = {
        box = box,
        billboard = billboard,
        label = label,
        distLabel = distLabel,
        tracer = tracer,
    }
end

local function RemoveCoinESP(part)
    local data = coinEspObjects[part]
    if not data then return end
    for _, line in pairs(data.box) do line:Remove() end
    data.tracer:Remove()
    if data.billboard then data.billboard:Destroy() end
    coinEspObjects[part] = nil
end

local function UpdateESP()
    local char = LocalPlayer.Character
    if not char then return end
    local hrp = char:FindFirstChild('HumanoidRootPart')
    if not hrp then return end
    
    local viewport = CurrentCamera.ViewportSize
    
    for _, player in ipairs(Players:GetPlayers()) do
        if player ~= LocalPlayer then
            if not espObjects[player] then CreateESP(player) end
            
            local data = espObjects[player]
            local pChar = player.Character
            if not pChar then
                for _, line in pairs(data.box) do line.Visible = false end
                data.tracer.Visible = false
                data.billboard.Enabled = false
                RemoveHighlight(player)
                continue
            end
            
            local pHrp = pChar:FindFirstChild('HumanoidRootPart')
            local humanoid = pChar:FindFirstChildOfClass('Humanoid')
            if not pHrp or not humanoid then
                for _, line in pairs(data.box) do line.Visible = false end
                data.tracer.Visible = false
                data.billboard.Enabled = false
                RemoveHighlight(player)
                continue
            end
            
            if player.Name == Settings.killingPlayer then
                for _, line in pairs(data.box) do line.Visible = false end
                data.tracer.Visible = false
                data.billboard.Enabled = false
                RemoveHighlight(player)
                continue
            end
            
            local role = GetRole(player)
            local dead = IsDead(player)
            local color = dead and Colors.Unknown or GetDisplayColor(role)
            
            local pos, onScreen = CurrentCamera:WorldToViewportPoint(pHrp.Position)
            local dist = (hrp.Position - pHrp.Position).Magnitude
            if dist < 0.001 then dist = 0.001 end
            
            local showBox = onScreen and ShouldShow('Box', role)
            local showESP = onScreen and ShouldShow('ESP', role)
            local showTracers = onScreen and ShouldShow('Tracers', role)
            local showChams = ShouldShow('Chams', role)
            local showOutline = ShouldShow('Outline', role)
            
            local anyVisible = showBox or showESP or showTracers or showChams or showOutline
            
            if not anyVisible then
                for _, line in pairs(data.box) do line.Visible = false end
                data.tracer.Visible = false
                data.billboard.Enabled = false
                RemoveHighlight(player)
            else
                if showBox then
                    local head = pChar:FindFirstChild('Head')
                    local top = head and CurrentCamera:WorldToViewportPoint(head.Position + Vector3.new(0, 0.5, 0)) or pos
                    local bottom = CurrentCamera:WorldToViewportPoint(pHrp.Position - Vector3.new(0, 3, 0))
                    local size = 2000 / dist
                    
                    data.box[1].From = Vector2.new(pos.X - size/2, top.Y)
                    data.box[1].To = Vector2.new(pos.X + size/2, top.Y)
                    data.box[1].Color = color
                    data.box[1].Visible = true
                    
                    data.box[2].From = Vector2.new(pos.X - size/2, bottom.Y)
                    data.box[2].To = Vector2.new(pos.X + size/2, bottom.Y)
                    data.box[2].Color = color
                    data.box[2].Visible = true
                    
                    data.box[3].From = Vector2.new(pos.X - size/2, top.Y)
                    data.box[3].To = Vector2.new(pos.X - size/2, bottom.Y)
                    data.box[3].Color = color
                    data.box[3].Visible = true
                    
                    data.box[4].From = Vector2.new(pos.X + size/2, top.Y)
                    data.box[4].To = Vector2.new(pos.X + size/2, bottom.Y)
                    data.box[4].Color = color
                    data.box[4].Visible = true
                else
                    for _, line in pairs(data.box) do line.Visible = false end
                end
                
                if showESP then
                    data.billboard.Adornee = pChar:FindFirstChild('Head') or pChar
                    data.billboard.Enabled = true
                    data.nameLabel.Text = player.Name
                    data.roleLabel.Text = dead and '[DEAD]' or '[' .. role:upper() .. ']'
                    data.roleLabel.TextColor3 = color
                    data.distLabel.Text = string.format('[%d studs]', math.floor(dist))
                else
                    data.billboard.Enabled = false
                end
                
                if showTracers then
                    data.tracer.From = Vector2.new(viewport.X / 2, viewport.Y)
                    data.tracer.To = Vector2.new(pos.X, pos.Y)
                    data.tracer.Color = color
                    data.tracer.Visible = true
                else
                    data.tracer.Visible = false
                end
                
                if showChams or showOutline then
                    ApplyHighlight(player, color, showChams, showOutline)
                else
                    RemoveHighlight(player)
                end
            end
        end
    end
end

local function UpdateGunESP()
    local char = LocalPlayer.Character
    if not char then return end
    local hrp = char:FindFirstChild('HumanoidRootPart')
    if not hrp then return end
    
    local viewport = CurrentCamera.ViewportSize
    
    for part, data in pairs(gunEspObjects) do
        if not part or not part.Parent then
            RemoveGunESP(part)
            continue
        end
        
        local pos, onScreen = CurrentCamera:WorldToViewportPoint(part.Position)
        local dist = (hrp.Position - part.Position).Magnitude
        if dist < 0.001 then dist = 0.001 end
        
        local showBox = onScreen and ShouldShowGun('Box')
        local showESP = onScreen and ShouldShowGun('ESP')
        local showTracers = onScreen and ShouldShowGun('Tracers')
        local showChams = ShouldShowGun('Chams')
        local showOutline = ShouldShowGun('Outline')
        
        if not showBox and not showESP and not showTracers and not showChams and not showOutline then
            for _, line in pairs(data.box) do line.Visible = false end
            data.tracer.Visible = false
            data.billboard.Enabled = false
            continue
        end
        
        if showBox then
            local top = CurrentCamera:WorldToViewportPoint(part.Position + Vector3.new(0, 2, 0))
            local bottom = CurrentCamera:WorldToViewportPoint(part.Position - Vector3.new(0, 2, 0))
            local size = 2000 / dist
            
            data.box[1].From = Vector2.new(pos.X - size/2, top.Y)
            data.box[1].To = Vector2.new(pos.X + size/2, top.Y)
            data.box[1].Color = Colors.Gun
            data.box[1].Visible = true
            
            data.box[2].From = Vector2.new(pos.X - size/2, bottom.Y)
            data.box[2].To = Vector2.new(pos.X + size/2, bottom.Y)
            data.box[2].Color = Colors.Gun
            data.box[2].Visible = true
            
            data.box[3].From = Vector2.new(pos.X - size/2, top.Y)
            data.box[3].To = Vector2.new(pos.X - size/2, bottom.Y)
            data.box[3].Color = Colors.Gun
            data.box[3].Visible = true
            
            data.box[4].From = Vector2.new(pos.X + size/2, top.Y)
            data.box[4].To = Vector2.new(pos.X + size/2, bottom.Y)
            data.box[4].Color = Colors.Gun
            data.box[4].Visible = true
        else
            for _, line in pairs(data.box) do line.Visible = false end
        end
        
        if showESP then
            data.billboard.Adornee = part
            data.billboard.Enabled = true
            data.label.Text = '[GUN]'
            data.distLabel.Text = string.format('[%d studs]', math.floor(dist))
        else
            data.billboard.Enabled = false
        end
        
        if showTracers then
            data.tracer.From = Vector2.new(viewport.X / 2, viewport.Y)
            data.tracer.To = Vector2.new(pos.X, pos.Y)
            data.tracer.Color = Colors.Gun
            data.tracer.Visible = true
        else
            data.tracer.Visible = false
        end
    end
end

local function UpdateCoinESP()
    local char = LocalPlayer.Character
    if not char then return end
    local hrp = char:FindFirstChild('HumanoidRootPart')
    if not hrp then return end
    
    local viewport = CurrentCamera.ViewportSize
    
    for part, data in pairs(coinEspObjects) do
        if not part or not part.Parent then
            RemoveCoinESP(part)
            continue
        end
        
        local pos, onScreen = CurrentCamera:WorldToViewportPoint(part.Position)
        local dist = (hrp.Position - part.Position).Magnitude
        if dist < 0.001 then dist = 0.001 end
        
        local showBox = onScreen and ShouldShowCoin('Box')
        local showESP = onScreen and ShouldShowCoin('ESP')
        local showTracers = onScreen and ShouldShowCoin('Tracers')
        local showChams = ShouldShowCoin('Chams')
        
        if not showBox and not showESP and not showTracers and not showChams then
            for _, line in pairs(data.box) do line.Visible = false end
            data.tracer.Visible = false
            data.billboard.Enabled = false
            continue
        end
        
        if showBox then
            local top = CurrentCamera:WorldToViewportPoint(part.Position + Vector3.new(0, 1, 0))
            local bottom = CurrentCamera:WorldToViewportPoint(part.Position - Vector3.new(0, 1, 0))
            local size = 2000 / dist
            
            data.box[1].From = Vector2.new(pos.X - size/2, top.Y)
            data.box[1].To = Vector2.new(pos.X + size/2, top.Y)
            data.box[1].Color = Colors.Coin
            data.box[1].Visible = true
            
            data.box[2].From = Vector2.new(pos.X - size/2, bottom.Y)
            data.box[2].To = Vector2.new(pos.X + size/2, bottom.Y)
            data.box[2].Color = Colors.Coin
            data.box[2].Visible = true
            
            data.box[3].From = Vector2.new(pos.X - size/2, top.Y)
            data.box[3].To = Vector2.new(pos.X - size/2, bottom.Y)
            data.box[3].Color = Colors.Coin
            data.box[3].Visible = true
            
            data.box[4].From = Vector2.new(pos.X + size/2, top.Y)
            data.box[4].To = Vector2.new(pos.X + size/2, bottom.Y)
            data.box[4].Color = Colors.Coin
            data.box[4].Visible = true
        else
            for _, line in pairs(data.box) do line.Visible = false end
        end
        
        if showESP then
            data.billboard.Adornee = part
            data.billboard.Enabled = true
            data.label.Text = '[COIN]'
            data.distLabel.Text = string.format('[%d studs]', math.floor(dist))
        else
            data.billboard.Enabled = false
        end
        
        if showTracers then
            data.tracer.From = Vector2.new(viewport.X / 2, viewport.Y)
            data.tracer.To = Vector2.new(pos.X, pos.Y)
            data.tracer.Color = Colors.Coin
            data.tracer.Visible = true
        else
            data.tracer.Visible = false
        end
    end
end

local function RegisterCoinContainer(container)
    if not container or container.Name ~= 'CoinContainer' then return end
    if not container:IsA('Folder') and not container:IsA('Model') then return end
    
    for _, child in ipairs(container:GetChildren()) do
        if IsCollectibleCoin(child) then
            CreateCoinESP(child)
        end
    end
    
    container.ChildAdded:Connect(function(child)
        if IsCollectibleCoin(child) then
            CreateCoinESP(child)
        end
    end)
    
    container.ChildRemoved:Connect(function(child)
        if coinEspObjects[child] then
            RemoveCoinESP(child)
        end
    end)
end

local function RegisterGunDrop(part)
    if not part or part.Name ~= 'GunDrop' or not part:IsA('BasePart') then return end
    
    CreateGunESP(part)
    
    if Settings.GunDropNotify and not Settings.notifiedGunDrops[part] then
        Settings.notifiedGunDrops[part] = true
        local char = LocalPlayer.Character
        if char then
            local hrp = char:FindFirstChild('HumanoidRootPart')
            if hrp then
                local dist = math.floor((hrp.Position - part.Position).Magnitude)
                WindUI:Notify({
                    Title = 'Gun Drop',
                    Content = 'A Gun has Dropped ' .. dist .. ' studs away!',
                    Duration = 5,
                })
            end
        end
    end
    
    part.AncestryChanged:Connect(function(_, parent)
        if not parent then
            RemoveGunESP(part)
            Settings.notifiedGunDrops[part] = nil
        end
    end)
end

-- ================================================================
-- ФУНКЦИИ COMBAT (ИЗ TOOLBOXHUB)
-- ================================================================
local function ShootMurderer()
    task.spawn(function()
        local role = GetRole(LocalPlayer)
        if role ~= 'Sheriff' and role ~= 'Hero' then return end
        if IsDead(LocalPlayer) then return end
        
        if tick() - Settings.silentAimCooldown < 0.25 then return end
        Settings.silentAimCooldown = tick()
        
        local murderer = Settings.currentMurderer
        if not murderer or not murderer.Character then return end
        
        local targetPart = murderer.Character:FindFirstChild('UpperTorso') or 
                          murderer.Character:FindFirstChild('Torso') or 
                          murderer.Character:FindFirstChild('Head') or 
                          murderer.Character:FindFirstChild('HumanoidRootPart')
        if not targetPart then return end
        
        local char = LocalPlayer.Character
        if not char then return end
        
        local hrp = char:FindFirstChild('HumanoidRootPart')
        if not hrp then return end
        
        local gun = char:FindFirstChild('Gun')
        if not gun then
            gun = LocalPlayer.Backpack:FindFirstChild('Gun')
            if gun then
                gun.Parent = char
                task.wait(0.15)
                gun = char:FindFirstChild('Gun')
            end
        end
        if not gun then return end
        
        local predictedPos = GetPredictedPos(targetPart)
        local remote = gun:FindFirstChild('Shoot', true)
        if remote then
            remote:FireServer(CFrame.new(hrp.Position, predictedPos), CFrame.new(predictedPos))
        end
    end)
end

local function KillMurderer()
    task.spawn(function()
        local role = GetRole(LocalPlayer)
        if role ~= 'Sheriff' and role ~= 'Hero' then
            -- Попытка стать героем
            if IsHeroEligible(LocalPlayer) then
                if not Settings.roleTable[LocalPlayer.Name] then
                    Settings.roleTable[LocalPlayer.Name] = {}
                end
                Settings.roleTable[LocalPlayer.Name].Role = 'Hero'
                Settings.roleTable[LocalPlayer.Name].Dead = false
                UpdateCachedRoles()
            end
            return
        end
        if IsDead(LocalPlayer) then return end
        
        local murderer = Settings.currentMurderer
        if not murderer or not murderer.Character then return end
        
        local targetHrp = murderer.Character:FindFirstChild('HumanoidRootPart')
        if not targetHrp then return end
        
        local char = LocalPlayer.Character
        if not char then return end
        
        local hrp = char:FindFirstChild('HumanoidRootPart')
        if not hrp then return end
        
        local gun = char:FindFirstChild('Gun')
        if not gun then
            gun = LocalPlayer.Backpack:FindFirstChild('Gun')
            if gun then
                gun.Parent = char
                task.wait(0.15)
                gun = char:FindFirstChild('Gun')
            end
        end
        if not gun then return end
        
        -- Телепорт к убийце
        local originalCFrame = hrp.CFrame
        hrp.Anchored = true
        hrp.CFrame = CFrame.new(targetHrp.Position + Vector3.new(0, 3, 0))
        task.wait(0.05)
        
        -- Стреляем
        local predictedPos = GetPredictedPos(targetHrp)
        local remote = gun:FindFirstChild('Shoot', true)
        if remote then
            remote:FireServer(CFrame.new(hrp.Position, predictedPos), CFrame.new(predictedPos))
        end
        
        task.wait(0.05)
        hrp.CFrame = originalCFrame
        hrp.Anchored = false
    end)
end

local function ThrowKnife()
    task.spawn(function()
        if GetRole(LocalPlayer) ~= 'Murderer' then return end
        if IsDead(LocalPlayer) then return end
        
        local char = LocalPlayer.Character
        if not char then return end
        
        local hrp = char:FindFirstChild('HumanoidRootPart')
        if not hrp then return end
        
        local knife = char:FindFirstChild('Knife')
        if not knife then
            knife = LocalPlayer.Backpack:FindFirstChild('Knife')
            if knife then
                knife.Parent = char
                task.wait(0.1)
                knife = char:FindFirstChild('Knife')
            end
        end
        if not knife then return end
        
        local nearest, nearestDist
        for _, player in ipairs(Players:GetPlayers()) do
            if player ~= LocalPlayer and not IsDead(player) and player.Character then
                local targetHrp = player.Character:FindFirstChild('HumanoidRootPart')
                if targetHrp then
                    local dist = (hrp.Position - targetHrp.Position).Magnitude
                    if not nearestDist or dist < nearestDist then
                        nearestDist = dist
                        nearest = targetHrp
                    end
                end
            end
        end
        
        if not nearest then return end
        
        local predictedPos = GetPredictedPos(nearest)
        local remote = knife:FindFirstChild('Events', true)
        if remote then
            remote = remote:FindFirstChild('KnifeThrown')
            if remote then
                remote:FireServer(knife.Handle.CFrame, CFrame.new(predictedPos))
            end
        end
    end)
end

local function KillPlayer(player)
    task.spawn(function()
        local char = LocalPlayer.Character
        if not char then return end
        
        local targetChar = player.Character
        if not targetChar then return end
        
        local targetHrp = targetChar:FindFirstChild('HumanoidRootPart')
        if not targetHrp then return end
        
        Settings.killingPlayer = player.Name
        
        local knife = char:FindFirstChild('Knife')
        if not knife then
            knife = LocalPlayer.Backpack:FindFirstChild('Knife')
            if knife then
                knife.Parent = char
                task.wait(0.1)
                knife = char:FindFirstChild('Knife')
            end
        end
        if not knife then
            Settings.killingPlayer = nil
            return
        end
        
        local handle = knife:FindFirstChild('Handle')
        local events = knife:FindFirstChild('Events')
        local remote = events and events:FindFirstChild('HandleTouched')
        
        if handle and remote then
            local weld = handle:FindFirstChildWhichIsA('Weld') or handle:FindFirstChildWhichIsA('WeldConstraint')
            if weld then weld.Enabled = false end
            
            local handleParent = handle.Parent
            local handleCFrame = handle.CFrame
            
            handle.Parent = workspace
            handle.CFrame = targetHrp.CFrame
            task.wait()
            remote:FireServer(targetHrp)
            task.wait()
            
            handle.CFrame = handleCFrame
            handle.Parent = handleParent
            if weld then weld.Enabled = true end
        end
        
        Settings.killingPlayer = nil
    end)
end

local function KillAll()
    task.spawn(function()
        local char = LocalPlayer.Character
        if not char then return end
        
        local knife = char:FindFirstChild('Knife')
        if not knife then
            knife = LocalPlayer.Backpack:FindFirstChild('Knife')
            if knife then
                knife.Parent = char
                task.wait(0.1)
                knife = char:FindFirstChild('Knife')
            end
        end
        if not knife then return end
        
        local handle = knife:FindFirstChild('Handle')
        local events = knife:FindFirstChild('Events')
        local remote = events and events:FindFirstChild('HandleTouched')
        if not handle or not remote then return end
        
        local targets = {}
        for _, player in ipairs(Players:GetPlayers()) do
            if player ~= LocalPlayer and not IsDead(player) and player.Character then
                local hrp = player.Character:FindFirstChild('HumanoidRootPart')
                if hrp then
                    table.insert(targets, hrp)
                end
            end
        end
        
        if #targets == 0 then return end
        
        local weld = handle:FindFirstChildWhichIsA('Weld') or handle:FindFirstChildWhichIsA('WeldConstraint')
        if weld then weld.Enabled = false end
        
        local handleParent = handle.Parent
        local handleCFrame = handle.CFrame
        
        handle.Parent = workspace
        for _, target in ipairs(targets) do
            pcall(function()
                handle.CFrame = target.CFrame
                task.wait()
                remote:FireServer(target)
                task.wait()
            end)
        end
        
        handle.CFrame = handleCFrame
        handle.Parent = handleParent
        if weld then weld.Enabled = true end
    end)
end

local function KillSheriff()
    for _, player in ipairs(Players:GetPlayers()) do
        if player ~= LocalPlayer and not IsDead(player) then
            local role = GetRole(player)
            if role == 'Sheriff' or role == 'Hero' then
                task.spawn(function() KillPlayer(player) end)
                break
            end
        end
    end
end

local function GrabGun()
    task.spawn(function()
        if IsDead(LocalPlayer) then return end
        
        local role = GetRole(LocalPlayer)
        if role == 'Murderer' or role == 'Unknown' then return end
        
        local char = LocalPlayer.Character
        if not char then return end
        
        local hrp = char:FindFirstChild('HumanoidRootPart')
        if not hrp then return end
        
        local gunPart = nil
        for part in pairs(gunEspObjects) do
            if part and part.Parent then
                gunPart = part
                break
            end
        end
        
        if not gunPart then return end
        
        if Settings.grabConn then
            Settings.grabConn:Disconnect()
            Settings.grabConn = nil
        end
        
        Settings.grabConn = RunService.Heartbeat:Connect(function()
            if not gunPart or not gunPart.Parent then
                if Settings.grabConn then
                    Settings.grabConn:Disconnect()
                    Settings.grabConn = nil
                end
                return
            end
            gunPart.CFrame = hrp.CFrame
        end)
        
        task.delay(2, function()
            if Settings.grabConn then
                Settings.grabConn:Disconnect()
                Settings.grabConn = nil
            end
        end)
    end)
end

-- ================================================================
-- ФУНКЦИИ ЛОКАЛЬНОГО ИГРОКА (ИЗ TOOLBOXHUB)
-- ================================================================
local function ToggleFly(enabled)
    Settings.Fly = enabled
    local char = LocalPlayer.Character
    if not char then return end
    
    local hrp = char:FindFirstChild('HumanoidRootPart')
    local humanoid = char:FindFirstChildOfClass('Humanoid')
    if not hrp or not humanoid then return end
    
    if enabled then
        humanoid.PlatformStand = true
        humanoid:ChangeState(Enum.HumanoidStateType.Swimming)
        if char:FindFirstChild('Animate') then
            char.Animate.Disabled = true
        end
        for _, anim in pairs(humanoid:GetPlayingAnimationTracks()) do
            anim:Stop()
        end
        
        local gyro = Instance.new('BodyGyro')
        gyro.P = 90000
        gyro.MaxTorque = Vector3.new(1, 1, 1) * 1e999
        gyro.CFrame = CurrentCamera.CFrame
        gyro.Name = '_FlyGyro'
        gyro.Parent = hrp
        
        local velocity = Instance.new('BodyVelocity')
        velocity.MaxForce = Vector3.new(1, 1, 1) * 1e999
        velocity.Velocity = Vector3.zero
        velocity.Name = '_FlyVelocity'
        velocity.Parent = hrp
        
        if connections.flyRender then connections.flyRender:Disconnect() end
        connections.flyRender = RunService.RenderStepped:Connect(function()
            if not Settings.Fly then return end
            local cam = CurrentCamera.CFrame
            local moveDir = humanoid.MoveDirection
            local unit = (cam.RightVector * moveDir:Dot(cam.RightVector) + 
                         cam.LookVector * moveDir:Dot(Vector3.new(cam.LookVector.X, 0, cam.LookVector.Z).Unit)).Unit
            local vel = moveDir.Magnitude > 0 and unit * 90 or Vector3.zero
            
            local flyGyro = hrp:FindFirstChild('_FlyGyro')
            local flyVel = hrp:FindFirstChild('_FlyVelocity')
            if flyVel then flyVel.Velocity = vel end
            if flyGyro then flyGyro.CFrame = cam end
        end)
    else
        if connections.flyRender then
            connections.flyRender:Disconnect()
            connections.flyRender = nil
        end
        if hrp:FindFirstChild('_FlyGyro') then hrp._FlyGyro:Destroy() end
        if hrp:FindFirstChild('_FlyVelocity') then hrp._FlyVelocity:Destroy() end
        humanoid.PlatformStand = false
        if char:FindFirstChild('Animate') then
            char.Animate.Disabled = false
        end
    end
end

local function SetupSpeedGlitch(character)
    local humanoid = character:WaitForChild('Humanoid')
    Settings.normalWalkSpeed = humanoid.WalkSpeed
    
    humanoid.StateChanged:Connect(function(_, newState)
        if not Settings.SpeedGlitch then return end
        if newState == Enum.HumanoidStateType.Landed then
            humanoid.WalkSpeed = Settings.normalWalkSpeed
            return
        end
        if newState == Enum.HumanoidStateType.Jumping then
            if Settings.OnlySideways then
                local moveDir = humanoid.MoveDirection
                if math.abs(moveDir.X) > math.abs(moveDir.Z) and moveDir.Magnitude > 0.1 then
                    humanoid.WalkSpeed = 30
                end
            else
                humanoid.WalkSpeed = 30
            end
        end
    end)
end

local function ApplyMovement()
    local char = LocalPlayer.Character
    if not char then return end
    local humanoid = char:FindFirstChildOfClass('Humanoid')
    if humanoid then
        humanoid.WalkSpeed = Settings.WalkSpeed
        humanoid.JumpPower = Settings.JumpPower
        Settings.normalWalkSpeed = Settings.WalkSpeed
    end
end

local function ApplyInvisibility()
    local char = LocalPlayer.Character
    if not char then return end
    
    local hrp = char:FindFirstChild('HumanoidRootPart')
    if not hrp then return end
    
    local seat = Instance.new('Seat', Workspace)
    seat.Anchored = false
    seat.CanCollide = false
    seat.Name = 'invischair'
    seat.Transparency = 1
    seat.Position = hrp.Position
    
    local weld = Instance.new('Weld', seat)
    weld.Name = 'invisweld'
    weld.Part0 = seat
    
    local torso = char:FindFirstChild('Torso') or char:FindFirstChild('UpperTorso')
    if torso then
        weld.Part1 = torso
    end
    
    seat.CFrame = hrp.CFrame
    
    for _, part in ipairs(char:GetDescendants()) do
        if part:IsA('BasePart') then
            if part.Name == 'HumanoidRootPart' then
                part.LocalTransparencyModifier = 0.5
            else
                part.Transparency = 0.5
            end
        elseif part:IsA('Decal') then
            part.Transparency = 0.5
        end
    end
end

local function RemoveInvisibility()
    local char = LocalPlayer.Character
    if not char then return end
    
    local chair = Workspace:FindFirstChild('invischair')
    if chair then
        local weld = chair:FindFirstChild('invisweld')
        if weld then weld:Destroy() end
        chair:Destroy()
    end
    
    for _, part in ipairs(char:GetDescendants()) do
        if part:IsA('BasePart') then
            if part.Name == 'HumanoidRootPart' then
                part.LocalTransparencyModifier = 0
            else
                part.Transparency = 0
            end
        elseif part:IsA('Decal') then
            part.Transparency = 0
        end
    end
end

local function SetupAntiFling()
    if connections.antiFlingLoop then
        connections.antiFlingLoop:Disconnect()
        connections.antiFlingLoop = nil
    end
    
    connections.antiFlingLoop = RunService.Stepped:Connect(function()
        if not Settings.AntiFling then return end
        for _, player in ipairs(Players:GetPlayers()) do
            if player ~= LocalPlayer and player.Character then
                for _, part in ipairs(player.Character:GetDescendants()) do
                    if part:IsA('BasePart') then
                        part.CanCollide = false
                    end
                end
            end
        end
    end)
end

local function EnableXray()
    xrayParts = {}
    for _, desc in ipairs(Workspace:GetDescendants()) do
        if desc:IsA('BasePart') then
            local isPlayer = false
            for _, player in ipairs(Players:GetPlayers()) do
                if player.Character and desc:IsDescendantOf(player.Character) then
                    isPlayer = true
                    break
                end
            end
            if not isPlayer then
                desc.LocalTransparencyModifier = 0.7
                table.insert(xrayParts, desc)
            end
        end
    end
end

local function ClearXray()
    for _, part in ipairs(xrayParts) do
        if part and part.Parent then
            part.LocalTransparencyModifier = 0
        end
    end
    xrayParts = {}
end

-- ================================================================
-- ФУНКЦИИ ФЛИНГА (ИЗ TOOLBOXHUB)
-- ================================================================
local function CleanupFling()
    if connections.fling then
        connections.fling:Disconnect()
        connections.fling = nil
    end
    if connections.flingVel then
        connections.flingVel:Disconnect()
        connections.flingVel = nil
    end
    
    Settings.isFlinging = false
    Settings.flingTarget = nil
    Settings.flingAngle = 0
    Settings.flingHighVelCount = 0
    
    if Settings.flingOriginalFallenHeight then
        pcall(function() Workspace.FallenPartsDestroyHeight = Settings.flingOriginalFallenHeight end)
        Settings.flingOriginalFallenHeight = nil
    end
    
    if Settings.flingOldCameraSubject then
        pcall(function() CurrentCamera.CameraSubject = Settings.flingOldCameraSubject end)
        Settings.flingOldCameraSubject = nil
    end
    
    local char = LocalPlayer.Character
    if char then
        local hrp = char:FindFirstChild('HumanoidRootPart')
        local humanoid = char:FindFirstChildOfClass('Humanoid')
        
        if hrp then
            for _, child in pairs(hrp:GetChildren()) do
                if child:IsA('BodyVelocity') or child:IsA('BodyGyro') or child:IsA('LinearVelocity') or 
                   child:IsA('AngularVelocity') or child:IsA('AlignOrientation') or child:IsA('AlignPosition') then
                    child:Destroy()
                end
            end
            
            if Settings.trueOriginalPos then
                task.spawn(function()
                    hrp.Anchored = true
                    task.wait(0.1)
                    for _ = 1, 20 do
                        if not hrp.Parent then break end
                        pcall(function()
                            hrp.CFrame = Settings.trueOriginalPos
                            char:SetPrimaryPartCFrame(Settings.trueOriginalPos)
                            hrp.Velocity = Vector3.zero
                            hrp.AssemblyLinearVelocity = Vector3.zero
                            hrp.AssemblyAngularVelocity = Vector3.zero
                            hrp.RotVelocity = Vector3.zero
                        end)
                        task.wait(0.03)
                    end
                    pcall(function() hrp.Anchored = false end)
                end)
            end
        end
        
        if humanoid then
            pcall(function()
                humanoid:SetStateEnabled(Enum.HumanoidStateType.Seated, true)
                humanoid:ChangeState(Enum.HumanoidStateType.GettingUp)
                humanoid.PlatformStand = false
            end)
        end
    end
    
    Settings.trueOriginalPos = nil
    Settings.flingOldPos = nil
end

local function StartFling(target)
    if Settings.isFlinging then
        CleanupFling()
        return
    end
    
    if not target or not target.Character then return end
    
    local targetChar = target.Character
    local targetHrp = targetChar:FindFirstChild('HumanoidRootPart')
    local targetHead = targetChar:FindFirstChild('Head')
    if not targetHrp and not targetHead then return end
    
    local char = LocalPlayer.Character
    if not char then return end
    
    local hrp = char:FindFirstChild('HumanoidRootPart')
    local humanoid = char:FindFirstChildOfClass('Humanoid')
    if not hrp or not humanoid then return end
    
    Settings.trueOriginalPos = hrp.CFrame
    Settings.isFlinging = true
    Settings.flingTarget = target
    Settings.flingAngle = 0
    Settings.flingOldPos = hrp.CFrame
    Settings.flingHighVelCount = 0
    
    if CurrentCamera.CameraSubject then
        Settings.flingOldCameraSubject = CurrentCamera.CameraSubject
    end
    CurrentCamera.CameraSubject = targetHead or targetHrp or targetChar
    
    humanoid:SetStateEnabled(Enum.HumanoidStateType.Seated, false)
    Settings.flingOriginalFallenHeight = Workspace.FallenPartsDestroyHeight
    Workspace.FallenPartsDestroyHeight = 0/0
    
    for _, part in ipairs(char:GetChildren()) do
        if part:IsA('BasePart') then
            part.CanCollide = true
        end
    end
    
    local bv = Instance.new('BodyVelocity')
    bv.Name = '_FlingBV'
    bv.MaxForce = Vector3.new(1000000, 1000000, 1000000)
    bv.Velocity = Vector3.new(900000000, 900000000, 900000000)
    bv.Parent = hrp
    
    local bg = Instance.new('BodyGyro')
    bg.Name = '_FlingBG'
    bg.P = 90000
    bg.MaxTorque = Vector3.new(1000000, 1000000, 1000000)
    bg.CFrame = hrp.CFrame
    bg.Parent = hrp
    
    local function FlingMove(targetPart, offset, angle)
        local vel = targetPart.AssemblyLinearVelocity
        local pos = targetPart.Position + vel * 0.04
        local cf = CFrame.new(pos) * CFrame.Angles(0, math.atan2(vel.X, vel.Z), 0)
        
        hrp.CFrame = cf * CFrame.new(offset + Vector3.new(2.6 * math.sin(math.rad(Settings.flingAngle)), 0, 2.6 * math.cos(math.rad(Settings.flingAngle)))) * 
                     CFrame.Angles(math.rad(Settings.flingAngle * 1.8), math.rad(Settings.flingAngle * 3.2), math.rad(Settings.flingAngle * 0.7))
        pcall(function() char:SetPrimaryPartCFrame(hrp.CFrame) end)
        pcall(function()
            hrp.Velocity = Vector3.new(90000000, 1800000000, 90000000)
            hrp.RotVelocity = Vector3.new(900000000, 900000000, 900000000)
        end)
    end
    
    connections.fling = RunService.Stepped:Connect(function()
        if not Settings.isFlinging or not Settings.flingTarget or not Settings.flingTarget.Character then
            CleanupFling()
            return
        end
        
        local targetPart = Settings.flingTarget.Character:FindFirstChild('Head') or 
                          Settings.flingTarget.Character:FindFirstChild('HumanoidRootPart')
        if not targetPart then
            CleanupFling()
            return
        end
        
        local velMag = targetPart.AssemblyLinearVelocity.Magnitude
        if velMag > 420 then
            Settings.flingHighVelCount = Settings.flingHighVelCount + 1
        else
            Settings.flingHighVelCount = 0
        end
        
        if velMag > 780 or Settings.flingHighVelCount >= 9 then
            CleanupFling()
            return
        end
        
        Settings.flingAngle = Settings.flingAngle + 260
        
        local offsets = {
            Vector3.new(0, 0.15, 0),
            Vector3.new(0, -0.15, 0),
            Vector3.new(0.2, 0.25, 0),
            Vector3.new(-0.2, -0.25, 0),
        }
        
        for _, offset in ipairs(offsets) do
            FlingMove(targetPart, offset, 0)
            task.wait()
        end
    end)
    
    task.delay(12, function()
        if Settings.isFlinging then
            CleanupFling()
        end
    end)
end

local function FlingMurderer()
    if Settings.isFlinging then
        CleanupFling()
        return
    end
    
    UpdateCachedRoles()
    if Settings.currentMurderer and Settings.currentMurderer ~= LocalPlayer and not IsDead(Settings.currentMurderer) then
        StartFling(Settings.currentMurderer)
        return
    end
end

local function FlingSheriff()
    if Settings.isFlinging then
        CleanupFling()
        return
    end
    
    UpdateCachedRoles()
    local target = Settings.currentSheriff or Settings.currentHero
    if target and target ~= LocalPlayer and not IsDead(target) then
        StartFling(target)
        return
    end
end

local function FlingEveryone()
    if Settings.isFlinging then
        CleanupFling()
        return
    end
    
    local char = LocalPlayer.Character
    if not char then return end
    local hrp = char:FindFirstChild('HumanoidRootPart')
    if not hrp then return end
    
    Settings.flingQueue = {}
    for _, player in ipairs(Players:GetPlayers()) do
        if player ~= LocalPlayer and not IsDead(player) and player.Character then
            local pHrp = player.Character:FindFirstChild('HumanoidRootPart')
            if pHrp then
                table.insert(Settings.flingQueue, {
                    player = player,
                    distance = (hrp.Position - pHrp.Position).Magnitude
                })
            end
        end
    end
    
    if #Settings.flingQueue == 0 then return end
    
    table.sort(Settings.flingQueue, function(a, b) return a.distance < b.distance end)
    for i, v in ipairs(Settings.flingQueue) do
        Settings.flingQueue[i] = v.player
    end
    
    Settings.flingQueueIndex = 1
    Settings.isFlingingAll = true
    StartFling(Settings.flingQueue[1])
end

local function ToggleTouchFling(enabled)
    Settings.TouchFlingEnabled = enabled
    
    if enabled then
        Settings.touchFlingThread = task.spawn(function()
            local step = 0.1
            while Settings.TouchFlingEnabled do
                RunService.Heartbeat:Wait()
                local char = LocalPlayer.Character
                if char then
                    local hrp = char:FindFirstChild('HumanoidRootPart')
                    if hrp then
                        local vel = hrp.Velocity
                        hrp.Velocity = vel * 10000 + Vector3.new(0, 10000, 0)
                        RunService.RenderStepped:Wait()
                        hrp.Velocity = vel
                        RunService.Stepped:Wait()
                        hrp.Velocity = vel + Vector3.new(0, step, 0)
                        step = -step
                    end
                end
            end
        end)
    else
        Settings.touchFlingThread = nil
    end
end

-- ================================================================
-- ФУНКЦИИ FLICK И WALL HOP (ИЗ TOOLBOXHUB)
-- ================================================================
local function DoFlick()
    if Settings.flickInProgress then return end
    
    local role = GetRole(LocalPlayer)
    if role ~= 'Sheriff' and role ~= 'Hero' then return end
    
    if not Settings.currentMurderer or IsDead(Settings.currentMurderer) then return end
    
    local char = LocalPlayer.Character
    if not char then return end
    local hrp = char:FindFirstChild('HumanoidRootPart')
    if not hrp then return end
    
    local targetChar = Settings.currentMurderer.Character
    if not targetChar then return end
    local targetHead = targetChar:FindFirstChild('Head')
    if not targetHead then return end
    
    Settings.flickInProgress = true
    
    local cameraType = CurrentCamera.CameraType
    local cameraSubject = CurrentCamera.CameraSubject
    local camCFrame = CurrentCamera.CFrame
    local targetCFrame = CFrame.new(camCFrame.Position, targetHead.Position)
    
    CurrentCamera.CameraType = Enum.CameraType.Scriptable
    
    for i = 1, 3 do
        CurrentCamera.CFrame = camCFrame:Lerp(targetCFrame, i * 0.15 + 0.5)
        RunService.RenderStepped:Wait()
    end
    
    ShootMurderer()
    
    for i = 1, 3 do
        CurrentCamera.CFrame = targetCFrame:Lerp(camCFrame, i * 0.15 + 0.5)
        RunService.RenderStepped:Wait()
    end
    
    CurrentCamera.CameraType = cameraType
    if cameraSubject then
        pcall(function() CurrentCamera.CameraSubject = cameraSubject end)
    end
    
    Settings.flickInProgress = false
end

local function DoWallHop()
    if Settings.flickInProgress then return end
    
    local char = LocalPlayer.Character
    if not char then return end
    local hrp = char:FindFirstChild('HumanoidRootPart')
    local humanoid = char:FindFirstChildOfClass('Humanoid')
    if not hrp or not humanoid then return end
    
    Settings.flickInProgress = true
    
    local isShiftLock = UserInputService.MouseBehavior == Enum.MouseBehavior.LockCenter
    local originalYRot = hrp.CFrame:ToEulerAnglesYXZ()
    local camCFrame = CurrentCamera.CFrame
    
    if not isShiftLock then
        local targetRot = originalYRot - 1.5708
        for i = 1, 7 do
            local t = i / 7
            local newRot = originalYRot + (targetRot - originalYRot) * (t * t)
            hrp.CFrame = CFrame.new(hrp.Position) * CFrame.fromEulerAnglesYXZ(0, newRot, 0)
            RunService.RenderStepped:Wait()
        end
    else
        local lookVec = Vector3.new(camCFrame.LookVector.X, 0, camCFrame.LookVector.Z).Unit
        local rightVec = Vector3.new(camCFrame.RightVector.X, 0, camCFrame.RightVector.Z).Unit
        for i = 1, 7 do
            local t = i / 7
            local newPos = camCFrame.Position + lookVec:Lerp(rightVec, t * t).Unit
            CurrentCamera.CFrame = CFrame.lookAt(camCFrame.Position, newPos)
            RunService.RenderStepped:Wait()
        end
    end
    
    hrp.AssemblyLinearVelocity = Vector3.new(hrp.AssemblyLinearVelocity.X, 55, hrp.AssemblyLinearVelocity.Z)
    pcall(function() humanoid:ChangeState(Enum.HumanoidStateType.Jumping) end)
    task.wait(0.12)
    
    if not isShiftLock then
        local currentRot = hrp.CFrame:ToEulerAnglesYXZ()
        for i = 1, 5 do
            local t = i / 5
            local newRot = currentRot + (originalYRot - currentRot) * (t * t)
            hrp.CFrame = CFrame.new(hrp.Position) * CFrame.fromEulerAnglesYXZ(0, newRot, 0)
            RunService.RenderStepped:Wait()
        end
    else
        local lookVec = Vector3.new(CurrentCamera.CFrame.LookVector.X, 0, CurrentCamera.CFrame.LookVector.Z).Unit
        for i = 1, 5 do
            local t = i / 5
            local newPos = CurrentCamera.CFrame.Position + lookVec:Lerp(Vector3.new(camCFrame.LookVector.X, 0, camCFrame.LookVector.Z).Unit, t * t).Unit
            CurrentCamera.CFrame = CFrame.lookAt(CurrentCamera.CFrame.Position, newPos)
            RunService.RenderStepped:Wait()
        end
    end
    
    task.wait(0.1)
    Settings.flickInProgress = false
end

-- ================================================================
-- ФУНКЦИИ БОМБ (ИЗ TOOLBOXHUB)
-- ================================================================
local function ExecuteBombJump(bombType)
    if Settings.BombJumpOnCooldown or Settings.BombJumpDebounce then return end
    
    Settings.BombJumpDebounce = true
    
    local char = LocalPlayer.Character
    if not char then
        Settings.BombJumpDebounce = false
        return
    end
    
    local hrp = char:FindFirstChild('HumanoidRootPart')
    local humanoid = char:FindFirstChildOfClass('Humanoid')
    if not hrp or not humanoid then
        Settings.BombJumpDebounce = false
        return
    end
    
    local bombName = bombType == 'gold' and 'GoldBomb' or 'FakeBomb'
    local bomb = char:FindFirstChild(bombName)
    if not bomb then
        bomb = LocalPlayer.Backpack:FindFirstChild(bombName)
        if bomb then
            bomb.Parent = char
            task.wait(0.1)
            bomb = char:FindFirstChild(bombName)
        end
    end
    
    if not bomb then
        -- Попытка получить бомбу через ReplicateToy
        pcall(function()
            local remote = ReplicatedStorage:FindFirstChild('Remotes', true)
            if remote then
                remote = remote:FindFirstChild('Extras', true)
                if remote then
                    remote = remote:FindFirstChild('ReplicateToy')
                    if remote then
                        remote:InvokeServer(bombName)
                    end
                end
            end
        end)
        
        for _ = 1, 5 do
            task.wait(0.05)
            bomb = char:FindFirstChild(bombName)
            if not bomb then
                bomb = LocalPlayer.Backpack:FindFirstChild(bombName)
                if bomb then
                    bomb.Parent = char
                    task.wait(0.1)
                    bomb = char:FindFirstChild(bombName)
                end
            end
            if bomb then break end
        end
    end
    
    if not bomb then
        Settings.BombJumpDebounce = false
        return
    end
    
    local remote = bomb:FindFirstChild('Remote', true)
    if remote then
        local pos = hrp.Position + CurrentCamera.CFrame.LookVector * 5
        remote:FireServer(CFrame.new(pos), 50)
        humanoid:ChangeState(Enum.HumanoidStateType.Jumping)
        
        local cooldown = bombType == 'gold' and 4 or 21
        Settings.BombJumpOnCooldown = true
        
        task.spawn(function()
            task.wait(cooldown)
            Settings.BombJumpOnCooldown = false
        end)
        
        task.spawn(function()
            task.wait(0.5)
            local bomb2 = char:FindFirstChild(bombName)
            if bomb2 then
                bomb2.Parent = LocalPlayer:FindFirstChild('Backpack') or char
            end
        end)
    end
    
    Settings.BombJumpDebounce = false
end

-- ================================================================
-- ФУНКЦИИ FE ANIMATIONS (ИЗ TOOLBOXHUB)
-- ================================================================
local FEAnimPresets = {
    Vampire = {
        idle1 = 'http://www.roblox.com/asset/?id=1083445855',
        idle2 = 'http://www.roblox.com/asset/?id=1083450166',
        walk = 'http://www.roblox.com/asset/?id=1083473930',
        run = 'http://www.roblox.com/asset/?id=1083462077',
        jump = 'http://www.roblox.com/asset/?id=1083455352',
        climb = 'http://www.roblox.com/asset/?id=1083439238',
        fall = 'http://www.roblox.com/asset/?id=1083443587',
    },
    Hero = {
        idle1 = 'http://www.roblox.com/asset/?id=616111295',
        idle2 = 'http://www.roblox.com/asset/?id=616113536',
        walk = 'http://www.roblox.com/asset/?id=616122287',
        run = 'http://www.roblox.com/asset/?id=616117076',
        jump = 'http://www.roblox.com/asset/?id=616115533',
        climb = 'http://www.roblox.com/asset/?id=616104706',
        fall = 'http://www.roblox.com/asset/?id=616108001',
    },
    ['Zombie Classic'] = {
        idle1 = 'http://www.roblox.com/asset/?id=616158929',
        idle2 = 'http://www.roblox.com/asset/?id=616160636',
        walk = 'http://www.roblox.com/asset/?id=616168032',
        run = 'http://www.roblox.com/asset/?id=616163682',
        jump = 'http://www.roblox.com/asset/?id=616161997',
        climb = 'http://www.roblox.com/asset/?id=616156119',
        fall = 'http://www.roblox.com/asset/?id=616157476',
    },
    Mage = {
        idle1 = 'http://www.roblox.com/asset/?id=707742142',
        idle2 = 'http://www.roblox.com/asset/?id=707855907',
        walk = 'http://www.roblox.com/asset/?id=707897309',
        run = 'http://www.roblox.com/asset/?id=707861613',
        jump = 'http://www.roblox.com/asset/?id=707853694',
        climb = 'http://www.roblox.com/asset/?id=707826056',
        fall = 'http://www.roblox.com/asset/?id=707829716',
    },
}

local FEAnimMap = {
    idle = {
        folder = 'idle',
        slots = {
            { child = 'Animation1', origKey = 'idle1' },
            { child = 'Animation2', origKey = 'idle2' },
        }
    },
    walk = {
        folder = 'walk',
        slots = { { child = 'WalkAnim', origKey = 'walk' } }
    },
    run = {
        folder = 'run',
        slots = { { child = 'RunAnim', origKey = 'run' } }
    },
    jump = {
        folder = 'jump',
        slots = { { child = 'JumpAnim', origKey = 'jump' } }
    },
    climb = {
        folder = 'climb',
        slots = { { child = 'ClimbAnim', origKey = 'climb' } }
    },
    fall = {
        folder = 'fall',
        slots = { { child = 'FallAnim', origKey = 'fall' } }
    },
}

local function SaveFEAnimOriginals(animate)
    for _, v in pairs(FEAnimMap) do
        local folder = animate:FindFirstChild(v.folder)
        if folder then
            for _, slot in ipairs(v.slots) do
                local anim = folder:FindFirstChild(slot.child)
                if anim and anim:IsA('Animation') and anim.AnimationId and anim.AnimationId ~= '' then
                    Settings.FEAnimOriginals[slot.origKey] = anim.AnimationId
                end
            end
        end
    end
end

local function ApplyFEAnims(character)
    local animate = character:FindFirstChild('Animate')
    if not animate then return end
    
    local humanoid = character:FindFirstChildOfClass('Humanoid')
    if not humanoid then return end
    
    SaveFEAnimOriginals(animate)
    
    for _, track in pairs(humanoid:GetPlayingAnimationTracks()) do
        track:Stop(0)
    end
    
    animate.Disabled = true
    task.wait(0.15)
    
    for key, v in pairs(FEAnimMap) do
        local preset = Settings.FEAnimState[key] ~= 'Default' and Settings.FEAnimState[key] or Settings.FEAnimState.all
        local folder = animate:FindFirstChild(v.folder)
        
        if folder then
            for _, slot in ipairs(v.slots) do
                local anim = folder:FindFirstChild(slot.child)
                if anim and anim:IsA('Animation') then
                    if preset == 'Default' then
                        if Settings.FEAnimOriginals[slot.origKey] then
                            anim.AnimationId = Settings.FEAnimOriginals[slot.origKey]
                        end
                    else
                        if FEAnimPresets[preset] and FEAnimPresets[preset][slot.origKey] then
                            anim.AnimationId = FEAnimPresets[preset][slot.origKey]
                        end
                    end
                end
            end
        end
    end
    
    animate.Disabled = false
    local state = humanoid:GetState()
    humanoid:ChangeState(Enum.HumanoidStateType.GettingUp)
    task.delay(0.1, function()
        if humanoid and humanoid.Parent then
            humanoid:ChangeState(state)
        end
    end)
end

local function RestoreFEAnimOriginals(character)
    local animate = character:FindFirstChild('Animate')
    if not animate then return end
    
    local humanoid = character:FindFirstChildOfClass('Humanoid')
    if not humanoid then return end
    
    for _, track in pairs(humanoid:GetPlayingAnimationTracks()) do
        track:Stop(0)
    end
    
    animate.Disabled = true
    task.wait(0.15)
    
    for _, v in pairs(FEAnimMap) do
        local folder = animate:FindFirstChild(v.folder)
        if folder then
            for _, slot in ipairs(v.slots) do
                local anim = folder:FindFirstChild(slot.child)
                if anim and anim:IsA('Animation') and Settings.FEAnimOriginals[slot.origKey] then
                    anim.AnimationId = Settings.FEAnimOriginals[slot.origKey]
                end
            end
        end
    end
    
    animate.Disabled = false
    local state = humanoid:GetState()
    humanoid:ChangeState(Enum.HumanoidStateType.GettingUp)
    task.delay(0.1, function()
        if humanoid and humanoid.Parent then
            humanoid:ChangeState(state)
        end
    end)
end

-- ================================================================
-- ФУНКЦИИ AUTO FARM (ИЗ TOOLBOXHUB)
-- ================================================================
local function CancelFarmTween()
    if Settings.AutoFarmActiveTween then
        pcall(function() Settings.AutoFarmActiveTween:Cancel() end)
        Settings.AutoFarmActiveTween = nil
    end
end

local function CleanupAutoFarmPhysics()
    local char = LocalPlayer.Character
    if char then
        local humanoid = char:FindFirstChildOfClass('Humanoid')
        if humanoid then humanoid.PlatformStand = false end
    end
    
    for _, mover in ipairs(Settings.AutoFarmBodyMovers) do
        if mover and mover.Parent then mover:Destroy() end
    end
    Settings.AutoFarmBodyMovers = {}
    
    if Settings.AutoFarmAntigravBV then
        Settings.AutoFarmAntigravBV:Destroy()
        Settings.AutoFarmAntigravBV = nil
    end
    
    Settings.Noclip = Settings.AutoFarmOriginalNoclipState or false
end

local function GetNearestFarmCoin(position)
    local nearest, nearestDist
    for coin in pairs(Settings.AutoFarmCoinRegistry) do
        if coin and coin.Parent and coin:IsDescendantOf(Workspace) and IsCollectibleCoin(coin) then
            local dist = (position - coin.Position).Magnitude
            if not nearestDist or dist < nearestDist then
                nearestDist = dist
                nearest = coin
            end
        end
    end
    return nearest
end

local function StartAutoFarmCoinTracking()
    for _, desc in ipairs(Workspace:GetDescendants()) do
        if IsCollectibleCoin(desc) then
            Settings.AutoFarmCoinRegistry[desc] = true
        end
    end
    
    if connections.farmDescAdded then connections.farmDescAdded:Disconnect() end
    if connections.farmDescRemoved then connections.farmDescRemoved:Disconnect() end
    
    connections.farmDescAdded = Workspace.DescendantAdded:Connect(function(desc)
        if IsCollectibleCoin(desc) then
            Settings.AutoFarmCoinRegistry[desc] = true
        end
    end)
    
    connections.farmDescRemoved = Workspace.DescendantRemoving:Connect(function(desc)
        Settings.AutoFarmCoinRegistry[desc] = nil
    end)
end

local function StopAutoFarmCoinTracking()
    if connections.farmDescAdded then
        connections.farmDescAdded:Disconnect()
        connections.farmDescAdded = nil
    end
    if connections.farmDescRemoved then
        connections.farmDescRemoved:Disconnect()
        connections.farmDescRemoved = nil
    end
    Settings.AutoFarmCoinRegistry = {}
end

local function StartAutoFarm()
    if Settings.AutoFarmThread then
        Settings.AutoFarmThread = nil
    end
    
    Settings.AutoFarmOriginalNoclipState = Settings.Noclip
    Settings.AutoFarmSessionCoinsCollected = 0
    Settings.AutoFarmSessionStartTime = tick()
    Settings.AutoFarmActiveTween = nil
    Settings.AutoFarmCurrentTargetCoin = nil
    Settings.CoinsStarted = false
    Settings.CoinsFull = false
    Settings.CoinsCollected = 0
    
    StartAutoFarmCoinTracking()
    
    Settings.AutoFarmThread = task.spawn(function()
        local bodyVelocity = nil
        
        while Settings.AutoFarmEnabled do
            task.wait(0.01)
            
            local char = LocalPlayer.Character
            if not char then task.wait(0.25) continue end
            
            local hrp = char:FindFirstChild('HumanoidRootPart')
            local humanoid = char:FindFirstChildOfClass('Humanoid')
            if not hrp or not humanoid or humanoid.Health <= 0 then
                task.wait(0.25)
                continue
            end
            
            local role = GetRole(LocalPlayer)
            if role == 'Unknown' then
                task.wait(0.25)
                continue
            end
            
            if not Settings.CoinsStarted then
                -- Проверяем наличие монет на карте
                local hasCoins = false
                for coin in pairs(Settings.AutoFarmCoinRegistry) do
                    if coin and coin.Parent and coin:IsDescendantOf(Workspace) and IsCollectibleCoin(coin) then
                        hasCoins = true
                        break
                    end
                end
                if not hasCoins then
                    task.wait(0.25)
                    continue
                end
                
                Settings.CoinsStarted = true
                Settings.CoinsFull = false
                Settings.CoinsCollected = 0
                Settings.AutoFarmStartCFrame = hrp.CFrame
                if not Settings.Noclip then
                    Settings.Noclip = true
                end
            end
            
            if Settings.CoinsFull then
                CancelFarmTween()
                CleanupAutoFarmPhysics()
                Settings.CoinsStarted = false
                Settings.CoinsFull = false
                Settings.AutoFarmStartCFrame = nil
                continue
            end
            
            if not bodyVelocity or not bodyVelocity.Parent then
                bodyVelocity = Instance.new('BodyVelocity')
                bodyVelocity.MaxForce = Vector3.new(0, 1e999, 0)
                bodyVelocity.Velocity = Vector3.new(0, 0, 0)
                bodyVelocity.Parent = hrp
                Settings.AutoFarmAntigravBV = bodyVelocity
                table.insert(Settings.AutoFarmBodyMovers, bodyVelocity)
            end
            
            local target = Settings.AutoFarmCurrentTargetCoin
            if not target or not target.Parent or not target:IsDescendantOf(Workspace) or not IsCollectibleCoin(target) then
                target = GetNearestFarmCoin(hrp.Position)
                Settings.AutoFarmCurrentTargetCoin = target
            end
            
            if target then
                humanoid.PlatformStand = true
                
                local targetPos = target.Position + Vector3.new(0, -4.5, 0)
                local dist = (hrp.Position - target.Position).Magnitude
                
                if dist > 2 then
                    local duration = math.max(0.03, dist / Settings.AutoFarmTweenSpeed)
                    CancelFarmTween()
                    
                    Settings.AutoFarmActiveTween = TweenService:Create(hrp, TweenInfo.new(duration, Enum.EasingStyle.Linear), {
                        CFrame = CFrame.new(targetPos) * CFrame.Angles(1.5708, 0, 0)
                    })
                    Settings.AutoFarmActiveTween:Play()
                    task.wait(duration + 0.05)
                    Settings.AutoFarmActiveTween = nil
                end
                
                if IsCollectibleCoin(target) then
                    pcall(function()
                        firetouchinterest(hrp, target, 0)
                        firetouchinterest(hrp, target, 1)
                    end)
                    Settings.AutoFarmCoinRegistry[target] = nil
                    Settings.AutoFarmCurrentTargetCoin = nil
                    Settings.CoinsCollected = (Settings.CoinsCollected or 0) + 1
                    Settings.AutoFarmSessionCoinsCollected = Settings.AutoFarmSessionCoinsCollected + 1
                end
            end
        end
        
        if bodyVelocity then bodyVelocity:Destroy() end
        Settings.Noclip = Settings.AutoFarmOriginalNoclipState or false
        Settings.AutoFarmStartCFrame = nil
        CleanupAutoFarmPhysics()
        StopAutoFarmCoinTracking()
    end)
end

local function StopAutoFarm()
    Settings.AutoFarmEnabled = false
    CancelFarmTween()
    Settings.CoinsStarted = false
    Settings.CoinsFull = false
    Settings.AutoFarmStartCFrame = nil
    Settings.AutoFarmSessionCoinsCollected = 0
    Settings.AutoFarmSessionStartTime = 0
    Settings.AutoFarmCurrentTargetCoin = nil
    Settings.Noclip = Settings.AutoFarmOriginalNoclipState or false
    Settings.AutoFarmOriginalNoclipState = false
    
    if Settings.AutoFarmThread then
        Settings.AutoFarmThread = nil
    end
    
    CleanupAutoFarmPhysics()
    StopAutoFarmCoinTracking()
end

-- ================================================================
-- ФУНКЦИИ AIMLOCK (ИЗ TOOLBOXHUB)
-- ================================================================
local function GetAimlockTarget()
    if Settings.AimlockSelected then
        local target = Settings.aimlockTarget
        if target and target ~= LocalPlayer and not IsDead(target) and target.Character and target.Character:FindFirstChild('Head') then
            return target
        end
        return nil
    end
    
    if Settings.AimlockMurderer and Settings.currentMurderer and not IsDead(Settings.currentMurderer) then
        return Settings.currentMurderer
    end
    
    if Settings.AimlockSheriff then
        local target = Settings.currentSheriff or Settings.currentHero
        if target and not IsDead(target) then
            return target
        end
    end
    
    return nil
end

local function ToggleAimlock(enabled)
    Settings.AimlockEnabled = enabled
    
    if enabled then
        if connections.aimlockRender then return end
        
        connections.aimlockRender = RunService.RenderStepped:Connect(function()
            if not Settings.AimlockEnabled then return end
            
            local target = GetAimlockTarget()
            if not target or not target.Character or IsDead(target) then return end
            
            local head = target.Character:FindFirstChild('Head')
            if not head then return end
            
            local smoothness = math.clamp((Settings.AimlockSmoothness or 1) / 50, 0.01, 1)
            CurrentCamera.CFrame = CurrentCamera.CFrame:Lerp(
                CFrame.new(CurrentCamera.CFrame.Position, head.Position),
                smoothness
            )
        end)
    else
        if connections.aimlockRender then
            connections.aimlockRender:Disconnect()
            connections.aimlockRender = nil
        end
    end
end

-- ================================================================
-- ФУНКЦИИ TICKET/SKYBOX (ИЗ TOOLBOXHUB)
-- ================================================================
local skyboxOriginal = nil

local function RestoreSkybox()
    if skyboxOriginal then
        for _, child in pairs(Lighting:GetChildren()) do
            if child:IsA('Sky') or child:IsA('Atmosphere') or child:IsA('Clouds') then
                child:Destroy()
            end
        end
        
        local sky = Instance.new('Sky', Lighting)
        for k, v in pairs(skyboxOriginal) do
            sky[k] = v
        end
        skyboxOriginal = nil
    end
end

local function ApplySkybox(id)
    for _, child in pairs(Lighting:GetChildren()) do
        if child:IsA('Sky') or child:IsA('Atmosphere') or child:IsA('Clouds') then
            child:Destroy()
        end
    end
    
    if not skyboxOriginal then
        local existing = Lighting:FindFirstChildOfClass('Sky')
        if existing then
            skyboxOriginal = {
                SkyboxBk = existing.SkyboxBk,
                SkyboxDn = existing.SkyboxDn,
                SkyboxFt = existing.SkyboxFt,
                SkyboxLf = existing.SkyboxLf,
                SkyboxRt = existing.SkyboxRt,
                SkyboxUp = existing.SkyboxUp,
            }
        end
    end
    
    local sky = Instance.new('Sky', Lighting)
    sky.Name = 'RuzHub_CustomSky'
    local url = 'rbxassetid://' .. tostring(id)
    sky.SkyboxBk = url
    sky.SkyboxDn = url
    sky.SkyboxFt = url
    sky.SkyboxLf = url
    sky.SkyboxRt = url
    sky.SkyboxUp = url
    sky.SunTextureId = ''
    sky.MoonTextureId = ''
    sky.SunAngularSize = 0
    sky.StarCount = 0
    Lighting.ClockTime = 14
    Lighting.Brightness = 2
    Lighting.GlobalShadows = false
    Lighting.FogEnd = 999999
end

-- ================================================================
-- СОЗДАНИЕ GUI
-- ================================================================
local Window = WindUI:CreateWindow({
    Title = 'RuzHub + Toolbox',
    Icon = 'sparkles',
    Author = 'Full Merge',
    Folder = 'RuzHub',
    Size = UDim2.fromOffset(850, 650),
    Theme = 'Crimson',
    Acrylic = false,
    HideSearchBar = false,
    OpenButton = {
        Title = 'RuzHub',
        CornerRadius = UDim.new(1, 0),
        StrokeThickness = 2,
        Enabled = true,
        OnlyMobile = false,
        Color = ColorSequence.new(Color3.fromHex('#dc2626'), Color3.fromHex('#991b1b')),
    },
})

-- Вкладки
local MainTab = Window:Tab({ Title = 'Main', Icon = 'zap' })
local VisualTab = Window:Tab({ Title = 'Visuals', Icon = 'eye' })
local CombatTab = Window:Tab({ Title = 'Combat', Icon = 'sword' })
local LocalTab = Window:Tab({ Title = 'Local', Icon = 'user' })
local FarmTab = Window:Tab({ Title = 'Farm', Icon = 'coins' })
local GlobalTab = Window:Tab({ Title = 'Global', Icon = 'globe' })
local SettingsTab = Window:Tab({ Title = 'Settings', Icon = 'settings' })

-- ================================================================
-- MAIN TAB
-- ================================================================
local MainSection = MainTab:Section({ Title = 'Quick Actions', Opened = true })

MainSection:Paragraph({
    Title = 'Status',
    Content = 'Use the buttons below for quick actions',
})

MainSection:Button({
    Title = 'Shoot Murderer',
    Description = 'Auto-aim and shoot the murderer',
    Callback = ShootMurderer,
})

MainSection:Button({
    Title = 'Kill Murderer',
    Description = 'Teleport and kill the murderer',
    Callback = KillMurderer,
})

MainSection:Button({
    Title = 'Throw Knife',
    Description = 'Throw knife at nearest player',
    Callback = ThrowKnife,
})

MainSection:Button({
    Title = 'Kill All',
    Description = 'Kill all players with knife',
    Callback = KillAll,
})

MainSection:Button({
    Title = 'Kill Sheriff',
    Description = 'Kill the sheriff/hero',
    Callback = KillSheriff,
})

MainSection:Divider()

MainSection:Button({
    Title = 'Blurt Murderer',
    Description = 'Say murderer in chat',
    Callback = BlurtMurderer,
})

MainSection:Button({
    Title = 'Blurt Sheriff',
    Description = 'Say sheriff in chat',
    Callback = BlurtSheriff,
})

MainSection:Button({
    Title = 'Blurt Both',
    Description = 'Say both roles in chat',
    Callback = BlurtBoth,
})

MainSection:Divider()

MainSection:Button({
    Title = 'Expose Murderer',
    Description = 'Notify murderer role',
    Callback = ExposeMurderer,
})

MainSection:Button({
    Title = 'Expose Sheriff',
    Description = 'Notify sheriff/hero role',
    Callback = ExposeSheriff,
})

MainSection:Button({
    Title = 'Expose Both',
    Description = 'Notify both roles',
    Callback = ExposeBoth,
})

-- ================================================================
-- VISUALS TAB
-- ================================================================
local ESPGroup = VisualTab:LeftGroupbox('ESP')
local OutlineGroup = VisualTab:RightGroupbox('Outline')
local ChamsGroup = VisualTab:LeftGroupbox('Chams')
local TracersGroup = VisualTab:RightGroupbox('Tracers')
local BoxGroup = VisualTab:LeftGroupbox('Box')
local ColorsGroup = VisualTab:RightGroupbox('Colors')
local MiscVisualGroup = VisualTab:LeftGroupbox('Misc Visuals')

-- ESP Toggles
ESPGroup:Toggle({
    Title = 'Enable ESP',
    Default = false,
    Callback = function(v) Settings.ESPEnabled = v end,
})

ESPGroup:Toggle({
    Title = 'Everyone',
    Default = false,
    Callback = function(v) Settings.ESPEveryone = v end,
})

ESPGroup:Toggle({
    Title = 'Murderer',
    Default = false,
    Callback = function(v) Settings.ESPMurderer = v end,
})

ESPGroup:Toggle({
    Title = 'Sheriff/Hero',
    Default = false,
    Callback = function(v) Settings.ESPSheriff = v end,
})

ESPGroup:Toggle({
    Title = 'Innocent',
    Default = false,
    Callback = function(v) Settings.ESPInnocent = v end,
})

ESPGroup:Toggle({
    Title = 'Gun ESP',
    Default = false,
    Callback = function(v) Settings.ESPGun = v end,
})

ESPGroup:Toggle({
    Title = 'Coin ESP',
    Default = false,
    Callback = function(v) Settings.ESPCoin = v end,
})

-- Outline Toggles
OutlineGroup:Toggle({
    Title = 'Enable Outline',
    Default = false,
    Callback = function(v) Settings.OutlineEnabled = v end,
})

OutlineGroup:Toggle({
    Title = 'Everyone',
    Default = false,
    Callback = function(v) Settings.OutlineEveryone = v end,
})

OutlineGroup:Toggle({
    Title = 'Murderer',
    Default = false,
    Callback = function(v) Settings.OutlineMurderer = v end,
})

OutlineGroup:Toggle({
    Title = 'Sheriff/Hero',
    Default = false,
    Callback = function(v) Settings.OutlineSheriff = v end,
})

OutlineGroup:Toggle({
    Title = 'Innocent',
    Default = false,
    Callback = function(v) Settings.OutlineInnocent = v end,
})

OutlineGroup:Toggle({
    Title = 'Gun',
    Default = false,
    Callback = function(v) Settings.OutlineGun = v end,
})

-- Chams Toggles
ChamsGroup:Toggle({
    Title = 'Enable Chams',
    Default = false,
    Callback = function(v) Settings.ChamsEnabled = v end,
})

ChamsGroup:Toggle({
    Title = 'Everyone',
    Default = false,
    Callback = function(v) Settings.ChamsEveryone = v end,
})

ChamsGroup:Toggle({
    Title = 'Murderer',
    Default = false,
    Callback = function(v) Settings.ChamsMurderer = v end,
})

ChamsGroup:Toggle({
    Title = 'Sheriff/Hero',
    Default = false,
    Callback = function(v) Settings.ChamsSheriff = v end,
})

ChamsGroup:Toggle({
    Title = 'Innocent',
    Default = false,
    Callback = function(v) Settings.ChamsInnocent = v end,
})

ChamsGroup:Toggle({
    Title = 'Gun',
    Default = false,
    Callback = function(v) Settings.ChamsGun = v end,
})

ChamsGroup:Toggle({
    Title = 'Coin',
    Default = false,
    Callback = function(v) Settings.ChamsCoin = v end,
})

-- Tracers Toggles
TracersGroup:Toggle({
    Title = 'Enable Tracers',
    Default = false,
    Callback = function(v) Settings.TracersEnabled = v end,
})

TracersGroup:Toggle({
    Title = 'Everyone',
    Default = false,
    Callback = function(v) Settings.TracersEveryone = v end,
})

TracersGroup:Toggle({
    Title = 'Murderer',
    Default = false,
    Callback = function(v) Settings.TracersMurderer = v end,
})

TracersGroup:Toggle({
    Title = 'Sheriff/Hero',
    Default = false,
    Callback = function(v) Settings.TracersSheriff = v end,
})

TracersGroup:Toggle({
    Title = 'Innocent',
    Default = false,
    Callback = function(v) Settings.TracersInnocent = v end,
})

TracersGroup:Toggle({
    Title = 'Gun',
    Default = false,
    Callback = function(v) Settings.TracersGun = v end,
})

TracersGroup:Toggle({
    Title = 'Coin',
    Default = false,
    Callback = function(v) Settings.TracersCoin = v end,
})

-- Box Toggles
BoxGroup:Toggle({
    Title = 'Enable Box',
    Default = false,
    Callback = function(v) Settings.BoxEnabled = v end,
})

BoxGroup:Toggle({
    Title = 'Everyone',
    Default = false,
    Callback = function(v) Settings.BoxEveryone = v end,
})

BoxGroup:Toggle({
    Title = 'Murderer',
    Default = false,
    Callback = function(v) Settings.BoxMurderer = v end,
})

BoxGroup:Toggle({
    Title = 'Sheriff/Hero',
    Default = false,
    Callback = function(v) Settings.BoxSheriff = v end,
})

BoxGroup:Toggle({
    Title = 'Innocent',
    Default = false,
    Callback = function(v) Settings.BoxInnocent = v end,
})

BoxGroup:Toggle({
    Title = 'Gun',
    Default = false,
    Callback = function(v) Settings.BoxGun = v end,
})

BoxGroup:Toggle({
    Title = 'Coin',
    Default = false,
    Callback = function(v) Settings.BoxCoin = v end,
})

-- Colors
ColorsGroup:ColorPicker({
    Title = 'Murderer Color',
    Default = Colors.Murderer,
    Callback = function(c) Colors.Murderer = c end,
})

ColorsGroup:ColorPicker({
    Title = 'Sheriff Color',
    Default = Colors.Sheriff,
    Callback = function(c) Colors.Sheriff = c end,
})

ColorsGroup:ColorPicker({
    Title = 'Hero Color',
    Default = Colors.Hero,
    Callback = function(c) Colors.Hero = c end,
})

ColorsGroup:ColorPicker({
    Title = 'Innocent Color',
    Default = Colors.Innocent,
    Callback = function(c) Colors.Innocent = c end,
})

ColorsGroup:ColorPicker({
    Title = 'Gun Color',
    Default = Colors.Gun,
    Callback = function(c) Colors.Gun = c end,
})

ColorsGroup:ColorPicker({
    Title = 'Coin Color',
    Default = Colors.Coin,
    Callback = function(c) Colors.Coin = c end,
})

-- Misc Visuals
MiscVisualGroup:Toggle({
    Title = 'Gun Drop Notification',
    Default = false,
    Callback = function(v) Settings.GunDropNotify = v end,
})

MiscVisualGroup:Toggle({
    Title = 'Status Overlay',
    Default = false,
    Callback = function(v) Settings.StatusOverlayEnabled = v end,
})

MiscVisualGroup:Toggle({
    Title = 'X-Ray',
    Default = false,
    Callback = function(v)
        Settings.XRay = v
        if v then EnableXray() else ClearXray() end
    end,
})

-- ================================================================
-- COMBAT TAB
-- ================================================================
local CombatSection = CombatTab:Section({ Title = 'Combat Features', Opened = true })

CombatSection:Toggle({
    Title = 'Silent Aim',
    Description = 'Auto-aim when shooting',
    Default = false,
    Callback = function(v) Settings.SilentAim = v end,
})

CombatSection:Toggle({
    Title = 'Trigger Bot',
    Description = 'Auto-shoot when crosshair is on enemy',
    Default = false,
    Callback = function(v) Settings.TriggerBot = v end,
})

CombatSection:Toggle({
    Title = 'Trigger Bot ShiftLock Only',
    Description = 'Trigger bot only when ShiftLock is active',
    Default = false,
    Callback = function(v) Settings.TriggerBotShiftLockOnly = v end,
})

CombatSection:Divider()

CombatSection:Toggle({
    Title = 'Auto Kill Murderer',
    Description = 'Automatically kill murderer when possible',
    Default = false,
    Callback = function(v) Settings.AutoKillMurderer = v end,
})

CombatSection:Toggle({
    Title = 'Auto Kill All',
    Description = 'Kill all players automatically',
    Default = false,
    Callback = function(v) Settings.AutoKillAll = v end,
})

CombatSection:Divider()

CombatSection:Toggle({
    Title = 'Auto Grab Gun',
    Description = 'Automatically grab gun when dropped',
    Default = false,
    Callback = function(v) Settings.AutoGrab = v end,
})

CombatSection:Toggle({
    Title = 'Auto Throw Knife',
    Description = 'Automatically throw knife at nearest player',
    Default = false,
    Callback = function(v) Settings.AutoThrow = v end,
})

CombatSection:Divider()

CombatSection:Button({
    Title = 'Grab Gun',
    Description = 'Teleport to dropped gun',
    Callback = GrabGun,
})

CombatSection:Button({
    Title = 'Grab Gun (Teleport)',
    Description = 'Instant teleport to gun',
    Callback = function()
        local gun = Workspace:FindFirstChild('GunDrop', true)
        if gun then
            local char = LocalPlayer.Character
            if char then
                local hrp = char:FindFirstChild('HumanoidRootPart')
                if hrp then
                    local pos = gun:IsA('BasePart') and gun.Position or gun:GetModelCFrame().Position
                    hrp.CFrame = CFrame.new(pos + Vector3.new(0, 2, 0))
                end
            end
        end
    end,
})

-- ================================================================
-- LOCAL TAB
-- ================================================================
local LocalSection = LocalTab:Section({ Title = 'Local Player', Opened = true })

LocalSection:Toggle({
    Title = 'Noclip',
    Description = 'Walk through walls',
    Default = false,
    Callback = function(v) Settings.Noclip = v end,
})

LocalSection:Toggle({
    Title = 'Fly',
    Description = 'Fly in any direction',
    Default = false,
    Callback = function(v) ToggleFly(v) end,
})

LocalSection:Toggle({
    Title = 'Infinite Jump',
    Description = 'Jump infinitely',
    Default = false,
    Callback = function(v) Settings.InfiniteJump = v end,
})

LocalSection:Toggle({
    Title = 'Speed Glitch',
    Description = 'Increase movement speed',
    Default = false,
    Callback = function(v) Settings.SpeedGlitch = v end,
})

LocalSection:Toggle({
    Title = 'Only Sideways',
    Description = 'Speed glitch only when moving sideways',
    Default = false,
    Callback = function(v) Settings.OnlySideways = v end,
})

LocalSection:Toggle({
    Title = 'Invisible',
    Description = 'Become invisible to others',
    Default = false,
    Callback = function(v)
        Settings.Invisible = v
        if v then ApplyInvisibility() else RemoveInvisibility() end
    end,
})

LocalSection:Divider()

LocalSection:Slider({
    Title = 'Walk Speed',
    Default = 16,
    Min = 0,
    Max = 500,
    Callback = function(v)
        Settings.WalkSpeed = v
        ApplyMovement()
    end,
})

LocalSection:Slider({
    Title = 'Jump Power',
    Default = 50,
    Min = 0,
    Max = 500,
    Callback = function(v)
        Settings.JumpPower = v
        ApplyMovement()
    end,
})

LocalSection:Divider()

LocalSection:Toggle({
    Title = 'Anti-Fling',
    Description = 'Protect against being flung',
    Default = false,
    Callback = function(v)
        Settings.AntiFling = v
        if v then SetupAntiFling() end
    end,
})

LocalSection:Toggle({
    Title = 'Anti-Void',
    Description = 'Protect against falling into void',
    Default = false,
    Callback = function(v) Settings.AntiVoid = v end,
})

-- ================================================================
-- FARM TAB
-- ================================================================
local FarmSection = FarmTab:Section({ Title = 'Auto Farm', Opened = true })

FarmSection:Toggle({
    Title = 'Auto Farm',
    Description = 'Automatically collect coins',
    Default = false,
    Callback = function(v)
        Settings.AutoFarmEnabled = v
        if v then StartAutoFarm() else StopAutoFarm() end
    end,
})

FarmSection:Slider({
    Title = 'Tween Speed',
    Default = 25,
    Min = 5,
    Max = 100,
    Callback = function(v) Settings.AutoFarmTweenSpeed = v end,
})

FarmSection:Divider()

FarmSection:Paragraph({
    Title = 'Post Farm Actions',
    Content = 'Actions to perform when coins are full',
})

FarmSection:Toggle({
    Title = 'Kill Murderer (Sheriff/Hero)',
    Default = false,
    Callback = function(v) Settings.PostFarmKillMurd = v end,
})

FarmSection:Toggle({
    Title = 'Kill All (Murderer)',
    Default = false,
    Callback = function(v) Settings.PostFarmKillAll = v end,
})

FarmSection:Toggle({
    Title = 'Fling Murderer (Innocent)',
    Default = false,
    Callback = function(v) Settings.PostFarmFlingMurd = v end,
})

FarmSection:Toggle({
    Title = 'Reset (Innocent)',
    Default = false,
    Callback = function(v) Settings.postfarmresetin = v end,
})

FarmSection:Toggle({
    Title = 'Reset (Sheriff/Hero)',
    Default = false,
    Callback = function(v) Settings.postfarmresetsh = v end,
})

FarmSection:Toggle({
    Title = 'Reset (Murderer)',
    Default = false,
    Callback = function(v) Settings.postfarmresetmurd = v end,
})

-- ================================================================
-- GLOBAL TAB
-- ================================================================
local GlobalSection = GlobalTab:Section({ Title = 'Global Features', Opened = true })

GlobalSection:Toggle({
    Title = 'Aimlock',
    Description = 'Lock aim on target',
    Default = false,
    Callback = function(v) ToggleAimlock(v) end,
})

GlobalSection:Toggle({
    Title = 'Aimlock Murderer',
    Description = 'Lock aim on murderer',
    Default = false,
    Callback = function(v) Settings.AimlockMurderer = v end,
})

GlobalSection:Toggle({
    Title = 'Aimlock Sheriff',
    Description = 'Lock aim on sheriff/hero',
    Default = false,
    Callback = function(v) Settings.AimlockSheriff = v end,
})

GlobalSection:Toggle({
    Title = 'Aimlock Selected',
    Description = 'Lock aim on selected player',
    Default = false,
    Callback = function(v) Settings.AimlockSelected = v end,
})

GlobalSection:Slider({
    Title = 'Aimlock Smoothness',
    Default = 10,
    Min = 1,
    Max = 50,
    Callback = function(v) Settings.AimlockSmoothness = v end,
})

GlobalSection:Divider()

GlobalSection:Toggle({
    Title = 'Bomb Jump',
    Description = 'Jump using bomb',
    Default = false,
    Callback = function(v) Settings.BombJumpEnabled = v end,
})

GlobalSection:Toggle({
    Title = 'Auto Get Bomb',
    Description = 'Automatically get bomb when available',
    Default = false,
    Callback = function(v) Settings.BombJumpAutoGet = v end,
})

GlobalSection:Divider()

GlobalSection:Toggle({
    Title = 'Coin Aura',
    Description = 'Automatically collect nearby coins',
    Default = false,
    Callback = function(v) Settings.CoinAura = v end,
})

GlobalSection:Toggle({
    Title = 'Touch Fling',
    Description = 'Fling players on touch',
    Default = false,
    Callback = function(v) ToggleTouchFling(v) end,
})

GlobalSection:Divider()

GlobalSection:Toggle({
    Title = 'Prediction',
    Description = 'Predict enemy movement',
    Default = true,
    Callback = function(v) Settings.PredictionEnabled = v end,
})

GlobalSection:Slider({
    Title = 'Prediction Multiplier',
    Default = 16.5,
    Min = 1,
    Max = 50,
    Callback = function(v) Settings.PredictionMultiplier = v end,
})

GlobalSection:Slider({
    Title = 'Y Clamp Min',
    Default = -2,
    Min = -10,
    Max = 0,
    Callback = function(v) Settings.YClampMin = v end,
})

GlobalSection:Slider({
    Title = 'Y Clamp Max',
    Default = 2.65,
    Min = 0,
    Max = 10,
    Callback = function(v) Settings.YClampMax = v end,
})

GlobalSection:Divider()

GlobalSection:Toggle({
    Title = 'Trickshot',
    Description = 'Enable trickshot mode',
    Default = false,
    Callback = function(v) Settings.Trickshot = v end,
})

GlobalSection:Button({
    Title = 'Flick',
    Description = 'Quick flick to murderer',
    Callback = DoFlick,
})

GlobalSection:Button({
    Title = 'Wall Hop',
    Description = 'Wall hop jump',
    Callback = DoWallHop,
})

GlobalSection:Divider()

GlobalSection:Toggle({
    Title = 'Dual Effect',
    Description = 'Auto equip dual effect',
    Default = false,
    Callback = function(v) Settings.DualEffectEnabled = v end,
})

GlobalSection:Dropdown({
    Title = 'Dual Effect Select',
    Options = {
        'Vampiric2024', 'SynthEffect2025', 'Sunbeams2024', 'Snowstorm2024',
        'Retro2025', 'Radioactive', 'Musical', 'Heatwave2025',
        'Heartify', 'Gifts2024', 'Ghosts2024', 'Ghostify',
        'FlamingoEffect2025', 'Burn', 'Cursed2024', 'Coal2025',
        'Starry2024', 'Bats2024', 'Aquatic2025', 'Treats2025',
        'Confetti2025', 'Bokeh2025', 'Lights2025', 'Jellyfish2024',
        'Hearts26', 'XmasGlow2025', 'Cats2025', 'Carrots2025',
        'BlueFire', 'Rainbows2025', 'Nightsky2025', 'Frost2025',
        'Elitify', 'Electric', 'Dual', 'Abduction2025',
        'SweetEffect26', 'UFOs2025', 'Strawberries26', 'Snowballs2025',
        'Leaves2025'
    },
    Default = 'Electric',
    Callback = function(v) Settings.DualEffectSelected = v end,
})

GlobalSection:Divider()

GlobalSection:Toggle({
    Title = 'FE Animations',
    Description = 'Enable custom animations',
    Default = false,
    Callback = function(v)
        Settings.FEAnimEnabled = v
        if v and LocalPlayer.Character then
            task.spawn(function() ApplyFEAnims(LocalPlayer.Character) end)
        elseif not v and LocalPlayer.Character then
            task.spawn(function() RestoreFEAnimOriginals(LocalPlayer.Character) end)
        end
    end,
})

GlobalSection:Dropdown({
    Title = 'FE Anim All',
    Options = {
        'Default', 'Vampire', 'Hero', 'Zombie Classic', 'Mage',
        'Ghost', 'Elder', 'Levitation', 'Astronaut', 'Ninja',
        'Werewolf', 'Cartoon', 'Pirate', 'Sneaky', 'Toy',
        'Knight', 'Confident', 'Popstar', 'Princess', 'Cowboy',
        'Patrol', 'Zombie FE', 'Catwalk Glam', 'Amazon Unboxed',
        'Glow Motion', 'Bubbly', 'Adidas Comm', 'KATSEYE', 'Wicked Popular'
    },
    Default = 'Default',
    Callback = function(v)
        Settings.FEAnimState.all = v
        if Settings.FEAnimEnabled and LocalPlayer.Character then
            ApplyFEAnims(LocalPlayer.Character)
        end
    end,
})

GlobalSection:Dropdown({
    Title = 'FE Anim Idle',
    Options = {
        'Default', 'Vampire', 'Hero', 'Zombie Classic', 'Mage',
        'Ghost', 'Elder', 'Levitation', 'Astronaut', 'Ninja',
        'Werewolf', 'Cartoon', 'Pirate', 'Sneaky', 'Toy',
        'Knight', 'Confident', 'Popstar', 'Princess', 'Cowboy',
        'Patrol', 'Zombie FE', 'Catwalk Glam', 'Amazon Unboxed',
        'Glow Motion', 'Bubbly', 'Adidas Comm', 'KATSEYE', 'Wicked Popular'
    },
    Default = 'Default',
    Callback = function(v)
        Settings.FEAnimState.idle = v
        if Settings.FEAnimEnabled and LocalPlayer.Character then
            ApplyFEAnims(LocalPlayer.Character)
        end
    end,
})

GlobalSection:Dropdown({
    Title = 'FE Anim Walk',
    Options = {
        'Default', 'Vampire', 'Hero', 'Zombie Classic', 'Mage',
        'Ghost', 'Elder', 'Levitation', 'Astronaut', 'Ninja',
        'Werewolf', 'Cartoon', 'Pirate', 'Sneaky', 'Toy',
        'Knight', 'Confident', 'Popstar', 'Princess', 'Cowboy',
        'Patrol', 'Zombie FE', 'Catwalk Glam', 'Amazon Unboxed',
        'Glow Motion', 'Bubbly', 'Adidas Comm', 'KATSEYE', 'Wicked Popular'
    },
    Default = 'Default',
    Callback = function(v)
        Settings.FEAnimState.walk = v
        if Settings.FEAnimEnabled and LocalPlayer.Character then
            ApplyFEAnims(LocalPlayer.Character)
        end
    end,
})

GlobalSection:Dropdown({
    Title = 'FE Anim Run',
    Options = {
        'Default', 'OG Rthro Run', 'Vampire', 'Hero', 'Zombie Classic',
        'Mage', 'Ghost', 'Elder', 'Levitation', 'Astronaut',
        'Ninja', 'Werewolf', 'Cartoon', 'Pirate', 'Sneaky',
        'Toy', 'Knight', 'Confident', 'Popstar', 'Princess',
        'Cowboy', 'Patrol', 'Zombie FE', 'Catwalk Glam', 'Amazon Unboxed',
        'Glow Motion', 'Bubbly', 'Adidas Comm', 'KATSEYE', 'Wicked Popular'
    },
    Default = 'Default',
    Callback = function(v)
        Settings.FEAnimState.run = v
        if Settings.FEAnimEnabled and LocalPlayer.Character then
            ApplyFEAnims(LocalPlayer.Character)
        end
    end,
})

GlobalSection:Dropdown({
    Title = 'FE Anim Jump',
    Options = {
        'Default', 'Vampire', 'Hero', 'Zombie Classic', 'Mage',
        'Ghost', 'Elder', 'Levitation', 'Astronaut', 'Ninja',
        'Werewolf', 'Cartoon', 'Pirate', 'Sneaky', 'Toy',
        'Knight', 'Confident', 'Popstar', 'Princess', 'Cowboy',
        'Patrol', 'Zombie FE', 'Catwalk Glam', 'Amazon Unboxed',
        'Glow Motion', 'Bubbly', 'Adidas Comm', 'KATSEYE', 'Wicked Popular'
    },
    Default = 'Default',
    Callback = function(v)
        Settings.FEAnimState.jump = v
        if Settings.FEAnimEnabled and LocalPlayer.Character then
            ApplyFEAnims(LocalPlayer.Character)
        end
    end,
})

GlobalSection:Dropdown({
    Title = 'FE Anim Climb',
    Options = {
        'Default', 'Vampire', 'Hero', 'Zombie Classic', 'Mage',
        'Ghost', 'Elder', 'Levitation', 'Astronaut', 'Ninja',
        'Werewolf', 'Cartoon', 'Pirate', 'Sneaky', 'Toy',
        'Knight', 'Confident', 'Popstar', 'Princess', 'Cowboy',
        'Patrol', 'Zombie FE', 'Catwalk Glam', 'Amazon Unboxed',
        'Glow Motion', 'Bubbly', 'Adidas Comm', 'KATSEYE', 'Wicked Popular'
    },
    Default = 'Default',
    Callback = function(v)
        Settings.FEAnimState.climb = v
        if Settings.FEAnimEnabled and LocalPlayer.Character then
            ApplyFEAnims(LocalPlayer.Character)
        end
    end,
})

GlobalSection:Dropdown({
    Title = 'FE Anim Fall',
    Options = {
        'Default', 'Vampire', 'Hero', 'Zombie Classic', 'Mage',
        'Ghost', 'Elder', 'Levitation', 'Astronaut', 'Ninja',
        'Werewolf', 'Cartoon', 'Pirate', 'Sneaky', 'Toy',
        'Knight', 'Confident', 'Popstar', 'Princess', 'Cowboy',
        'Patrol', 'Zombie FE', 'Catwalk Glam', 'Amazon Unboxed',
        'Glow Motion', 'Bubbly', 'Adidas Comm', 'KATSEYE', 'Wicked Popular'
    },
    Default = 'Default',
    Callback = function(v)
        Settings.FEAnimState.fall = v
        if Settings.FEAnimEnabled and LocalPlayer.Character then
            ApplyFEAnims(LocalPlayer.Character)
        end
    end,
})

-- ================================================================
-- SETTINGS TAB
-- ================================================================
local SettingsSection = SettingsTab:Section({ Title = 'Settings', Opened = true })

SettingsSection:Toggle({
    Title = 'Auto Rejoin',
    Description = 'Auto rejoin when kicked',
    Default = false,
    Callback = function(v) Settings.AutoRejoinEnabled = v end,
})

SettingsSection:Divider()

SettingsSection:Toggle({
    Title = 'Webhook',
    Description = 'Send farm stats to Discord',
    Default = false,
    Callback = function(v) Settings.WebhookEnabled = v end,
})

SettingsSection:Input({
    Title = 'Webhook URL',
    Default = '',
    Numeric = false,
    Placeholder = 'https://discord.com/api/webhooks/...',
    Callback = function(v) Settings.WebhookURL = v end,
})

SettingsSection:Slider({
    Title = 'Webhook Interval',
    Default = 10,
    Min = 5,
    Max = 60,
    Callback = function(v) Settings.WebhookInterval = v end,
})

SettingsSection:Toggle({
    Title = 'Webhook on Full',
    Description = 'Send when coins are full',
    Default = false,
    Callback = function(v) Settings.WebhookOnFull = v end,
})

SettingsSection:Divider()

SettingsSection:Toggle({
    Title = 'Show Round Timer',
    Default = false,
    Callback = function(v) Settings.ShowRoundTimer = v end,
})

SettingsSection:Toggle({
    Title = 'Instant Role Notify',
    Default = false,
    Callback = function(v) Settings.InstantRoleNotify = v end,
})

SettingsSection:Toggle({
    Title = 'Show Murderer Chance',
    Default = false,
    Callback = function(v) Settings.ShowMurdererChance = v end,
})

SettingsSection:Toggle({
    Title = 'Expose Roles',
    Default = false,
    Callback = function(v) Settings.ExposeRoles = v end,
})

-- ================================================================
-- СОЗДАНИЕ ЭКРАННЫХ КНОПОК (ИЗ RUZHUB)
-- ================================================================
local function CreateScreenButton(name, text, pos, color, callback)
    local btn = Instance.new('TextButton')
    btn.Name = 'RuzBtn_' .. name
    btn.Size = UDim2.new(0, 60, 0, 60)
    btn.Position = pos
    btn.BackgroundColor3 = Color3.fromRGB(0, 0, 0)
    btn.BackgroundTransparency = 0.6
    btn.Text = ''
    btn.AutoButtonColor = false
    btn.BorderSizePixel = 0
    btn.Parent = CoreGui
    
    Instance.new('UICorner', btn).CornerRadius = UDim.new(0, 12)
    
    local stroke = Instance.new('UIStroke', btn)
    stroke.Color = color
    stroke.Thickness = 1.5
    stroke.Transparency = 0.5
    stroke.ApplyStrokeMode = Enum.ApplyStrokeMode.Border
    
    local label = Instance.new('TextLabel', btn)
    label.Name = 'Lbl'
    label.Size = UDim2.new(1, 0, 1, 0)
    label.BackgroundTransparency = 1
    label.Text = text
    label.TextColor3 = color
    label.Font = Enum.Font.GothamBold
    label.TextSize = 10
    label.TextWrapped = true
    
    btn.MouseButton1Click:Connect(callback)
    
    -- Drag functionality
    local dragging = false
    local dragStart, startPos
    
    btn.InputBegan:Connect(function(input)
        if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then
            dragging = true
            dragStart = input.Position
            startPos = btn.Position
        end
    end)
    
    btn.InputChanged:Connect(function(input)
        if dragging and (input.UserInputType == Enum.UserInputType.MouseMovement or input.UserInputType == Enum.UserInputType.Touch) then
            local delta = input.Position - dragStart
            btn.Position = UDim2.new(startPos.X.Scale, startPos.X.Offset + delta.X, startPos.Y.Scale, startPos.Y.Offset + delta.Y)
        end
    end)
    
    UserInputService.InputEnded:Connect(function(input)
        if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then
            dragging = false
        end
    end)
    
    return btn
end

-- Создаем кнопки (из RuzHub + toolboxhub)
local screenButtons = {}

-- Shoot/Throw
screenButtons.shoot = CreateScreenButton(
    'Shoot',
    'SHOOT',
    UDim2.new(0.5, -10, 0.78, 0),
    Color3.fromRGB(255, 255, 255),
    function()
        if LocalPlayer.Backpack:FindFirstChild('Knife') or (LocalPlayer.Character and LocalPlayer.Character:FindFirstChild('Knife')) then
            ThrowKnife()
        else
            ShootMurderer()
        end
    end
)

-- ESP
screenButtons.esp = CreateScreenButton(
    'ESP',
    'ESP\nOFF',
    UDim2.new(0.5, 90, 0.78, 16),
    Color3.fromRGB(10, 140, 30),
    function()
        Settings.ESPEnabled = not Settings.ESPEnabled
        local color = Settings.ESPEnabled and Color3.fromRGB(50, 220, 80) or Color3.fromRGB(10, 140, 30)
        screenButtons.esp.Lbl.Text = Settings.ESPEnabled and 'ESP\nON' or 'ESP\nOFF'
        screenButtons.esp.Lbl.TextColor3 = color
        screenButtons.esp.UIStroke.Color = color
    end
)

-- Fling Murderer
screenButtons.flingMurd = CreateScreenButton(
    'FlingMurd',
    'FLING\nMURD',
    UDim2.new(0.5, -278, 0.68, 16),
    Color3.fromRGB(255, 50, 50),
    FlingMurderer
)

-- Fling Sheriff
screenButtons.flingSher = CreateScreenButton(
    'FlingSher',
    'FLING\nSHERIF',
    UDim2.new(0.5, -214, 0.68, 16),
    Color3.fromRGB(40, 130, 255),
    FlingSheriff
)

-- Fling Everyone
screenButtons.flingAll = CreateScreenButton(
    'FlingAll',
    'FLING\nALL',
    UDim2.new(0.5, -150, 0.68, 16),
    Color3.fromRGB(255, 215, 0),
    FlingEveryone
)

-- Grab Gun
screenButtons.grab = CreateScreenButton(
    'Grab',
    'GRAB\nGUN',
    UDim2.new(0.5, 154, 0.78, 16),
    Color3.fromRGB(200, 120, 0),
    GrabGun
)

-- Kill Murderer
screenButtons.kill = CreateScreenButton(
    'Kill',
    'KILL\nMURD',
    UDim2.new(0.5, 218, 0.78, 16),
    Color3.fromRGB(255, 0, 0),
    KillMurderer
)

-- Kill All
screenButtons.killAll = CreateScreenButton(
    'KillAll',
    'KILL\nALL',
    UDim2.new(0.5, 282, 0.78, 16),
    Color3.fromRGB(200, 0, 200),
    KillAll
)

-- Flick
screenButtons.flick = CreateScreenButton(
    'Flick',
    'FLICK',
    UDim2.new(0.5, -342, 0.78, 16),
    Color3.fromRGB(180, 50, 255),
    DoFlick
)

-- Wall Hop
screenButtons.wallhop = CreateScreenButton(
    'WallHop',
    'WALL\nHOP',
    UDim2.new(0.5, -278, 0.58, 16),
    Color3.fromRGB(0, 210, 210),
    DoWallHop
)

-- ================================================================
-- ОБНОВЛЕНИЕ СОСТОЯНИЯ КНОПОК
-- ================================================================
task.spawn(function()
    while true do
        task.wait(0.1)
        
        -- Shoot/Throw
        local hasKnife = LocalPlayer.Backpack:FindFirstChild('Knife') or 
                        (LocalPlayer.Character and LocalPlayer.Character:FindFirstChild('Knife'))
        screenButtons.shoot.Lbl.Text = hasKnife and 'THROW' or 'SHOOT'
        
        -- ESP
        local espColor = Settings.ESPEnabled and Color3.fromRGB(50, 220, 80) or Color3.fromRGB(10, 140, 30)
        screenButtons.esp.Lbl.Text = Settings.ESPEnabled and 'ESP\nON' or 'ESP\nOFF'
        screenButtons.esp.Lbl.TextColor3 = espColor
        screenButtons.esp.UIStroke.Color = espColor
        
        -- Fling Murderer
        local hasMurderer = false
        for _, player in ipairs(Players:GetPlayers()) do
            if player ~= LocalPlayer and (player.Backpack:FindFirstChild('Knife') or 
               (player.Character and player.Character:FindFirstChild('Knife'))) then
                hasMurderer = true
                break
            end
        end
        local flingColor = Settings.isFlinging and Color3.fromRGB(255, 180, 0) or 
                          (hasMurderer and Color3.fromRGB(255, 50, 50) or Color3.fromRGB(200, 20, 20))
        screenButtons.flingMurd.Lbl.Text = Settings.isFlinging and 'FLING...' or (hasMurderer and 'FLING\nMURD' or 'NO\nMURD')
        screenButtons.flingMurd.Lbl.TextColor3 = flingColor
        screenButtons.flingMurd.UIStroke.Color = flingColor
        
        -- Fling Sheriff
        local hasSheriff = false
        for _, player in ipairs(Players:GetPlayers()) do
            if player ~= LocalPlayer and (player.Backpack:FindFirstChild('Gun') or 
               (player.Character and player.Character:FindFirstChild('Gun'))) then
                hasSheriff = true
                break
            end
        end
        local flingColor2 = Settings.isFlinging and Color3.fromRGB(255, 180, 0) or 
                           (hasSheriff and Color3.fromRGB(40, 130, 255) or Color3.fromRGB(10, 80, 200))
        screenButtons.flingSher.Lbl.Text = Settings.isFlinging and 'FLING...' or (hasSheriff and 'FLING\nSHERIF' or 'NO\nSHERIF')
        screenButtons.flingSher.Lbl.TextColor3 = flingColor2
        screenButtons.flingSher.UIStroke.Color = flingColor2
        
        -- Grab Gun
        local hasGun = Workspace:FindFirstChild('GunDrop', true)
        local grabColor = hasGun and Color3.fromRGB(255, 215, 0) or Color3.fromRGB(200, 100, 0)
        screenButtons.grab.Lbl.Text = hasGun and 'GRAB\nGUN' or 'NO\nGUN'
        screenButtons.grab.Lbl.TextColor3 = grabColor
        screenButtons.grab.UIStroke.Color = grabColor
    end
end)

-- ================================================================
-- ЗАПУСК ВСЕХ АВТОМАТИЧЕСКИХ ЦИКЛОВ
-- ================================================================

-- ESP Loop
task.spawn(function()
    while true do
        task.wait(0.05)
        if Settings.ESPEnabled then
            UpdateESP()
            UpdateGunESP()
            UpdateCoinESP()
        end
    end
end)

-- Role Updates
task.spawn(function()
    while true do
        task.wait(0.5)
        RefreshRoles()
        CheckRoleNotify()
    end
end)

-- Auto Kill Murderer
task.spawn(function()
    while true do
        task.wait(0.1)
        if Settings.AutoKillMurderer and Settings.autoKillMurdererReady then
            Settings.autoKillMurdererReady = false
            KillMurderer()
            task.delay(0.5, function() Settings.autoKillMurdererReady = true end)
        end
    end
end)

-- Auto Throw
task.spawn(function()
    while true do
        task.wait(0.25)
        if Settings.AutoThrow and Settings.autoThrowReady then
            Settings.autoThrowReady = false
            ThrowKnife()
            task.delay(0.25, function() Settings.autoThrowReady = true end)
        end
    end
end)

-- Auto Kill All
task.spawn(function()
    while true do
        task.wait(0.1)
        if Settings.AutoKillAll and Settings.autoKillReady then
            Settings.autoKillReady = false
            KillAll()
            task.delay(0.1, function() Settings.autoKillReady = true end)
        end
    end
end)

-- Auto Grab Gun
task.spawn(function()
    while true do
        task.wait(0.05)
        if Settings.AutoGrab then
            GrabGun()
        end
    end
end)

-- Noclip
task.spawn(function()
    while true do
        task.wait(0.05)
        if Settings.Noclip and LocalPlayer.Character then
            for _, part in ipairs(LocalPlayer.Character:GetDescendants()) do
                if part:IsA('BasePart') then
                    part.CanCollide = false
                end
            end
        end
    end
end)

-- Infinite Jump
UserInputService.JumpRequest:Connect(function()
    if Settings.InfiniteJump then
        local char = LocalPlayer.Character
        if char then
            local humanoid = char:FindFirstChildOfClass('Humanoid')
            if humanoid then
                humanoid:ChangeState(Enum.HumanoidStateType.Jumping)
            end
        end
    end
end)

-- Anti-Fling
task.spawn(function()
    while true do
        task.wait(0.05)
        if Settings.AntiFling and LocalPlayer.Character then
            local hrp = LocalPlayer.Character:FindFirstChild('HumanoidRootPart')
            if hrp and hrp.AssemblyLinearVelocity.Magnitude > 200 then
                hrp.AssemblyLinearVelocity = hrp.AssemblyLinearVelocity.Unit * 200
            end
        end
    end
end)

-- Anti-Void
task.spawn(function()
    while true do
        task.wait(0.05)
        if Settings.AntiVoid and LocalPlayer.Character then
            local hrp = LocalPlayer.Character:FindFirstChild('HumanoidRootPart')
            if hrp and hrp.Position.Y < -50 then
                hrp.Velocity = Vector3.new(0, 250, 0)
            end
        end
    end
end)

-- Speed Glitch
task.spawn(function()
    while true do
        task.wait(0.1)
        if Settings.SpeedGlitch and LocalPlayer.Character then
            local humanoid = LocalPlayer.Character:FindFirstChildOfClass('Humanoid')
            if humanoid then
                local state = humanoid:GetState()
                if state == Enum.HumanoidStateType.Jumping or state == Enum.HumanoidStateType.Freefall then
                    if humanoid.MoveDirection.Magnitude > 0 then
                        humanoid.WalkSpeed = 200
                    end
                else
                    humanoid.WalkSpeed = Settings.normalWalkSpeed or 16
                end
            end
        end
    end
end)

-- Coin Aura
task.spawn(function()
    while true do
        task.wait(0.1)
        if Settings.CoinAura and LocalPlayer.Character then
            local hrp = LocalPlayer.Character:FindFirstChild('HumanoidRootPart')
            if hrp then
                for _, child in ipairs(Workspace:GetChildren()) do
                    local container = child:FindFirstChild('CoinContainer')
                    if container then
                        for _, coin in ipairs(container:GetChildren()) do
                            if coin:IsA('BasePart') and (hrp.Position - coin.Position).Magnitude <= 10 then
                                pcall(function()
                                    firetouchinterest(hrp, coin, 0)
                                    firetouchinterest(hrp, coin, 1)
                                end)
                            end
                        end
                    end
                end
            end
        end
    end
end)

-- Bomb Jump Auto Get
task.spawn(function()
    while true do
        task.wait(5)
        if Settings.BombJumpAutoGet then
            pcall(function()
                local remote = ReplicatedStorage:FindFirstChild('Remotes', true)
                if remote then
                    remote = remote:FindFirstChild('Extras', true)
                    if remote then
                        remote = remote:FindFirstChild('ReplicateToy')
                        if remote then
                            remote:InvokeServer('FakeBomb')
                            remote:InvokeServer('GoldBomb')
                        end
                    end
                end
            end)
        end
    end
end)

-- Bomb Jump (B key)
UserInputService.InputBegan:Connect(function(input, gameProcessed)
    if gameProcessed then return end
    if input.KeyCode == Enum.KeyCode.B and Settings.BombJumpEnabled then
        local char = LocalPlayer.Character
        if char then
            local hasBomb = char:FindFirstChild('FakeBomb') or 
                           (LocalPlayer.Backpack and LocalPlayer.Backpack:FindFirstChild('FakeBomb'))
            local hasGold = char:FindFirstChild('GoldBomb') or 
                           (LocalPlayer.Backpack and LocalPlayer.Backpack:FindFirstChild('GoldBomb'))
            if hasGold then
                ExecuteBombJump('gold')
            elseif hasBomb then
                ExecuteBombJump('normal')
            end
        end
    end
end)

-- Trigger Bot
task.spawn(function()
    while true do
        task.wait(0.05)
        if Settings.TriggerBot then
            local role = GetRole(LocalPlayer)
            if role ~= 'Sheriff' and role ~= 'Hero' then continue end
            if IsDead(LocalPlayer) then continue end
            
            if Settings.TriggerBotShiftLockOnly then
                if UserInputService.MouseBehavior ~= Enum.MouseBehavior.LockCenter then
                    continue
                end
            end
            
            local target = LocalPlayer:GetMouse().Target
            if not target then continue end
            
            local model = target:FindFirstAncestorWhichIsA('Model')
            if not model then continue end
            
            local player = Players:GetPlayerFromCharacter(model)
            if not player or player == LocalPlayer then continue end
            
            if player == Settings.currentMurderer then
                ShootMurderer()
            end
        end
    end
end)

-- Silent Aim (mouse click)
UserInputService.InputBegan:Connect(function(input, gameProcessed)
    if gameProcessed then return end
    if not Settings.SilentAim then return end
    if input.UserInputType ~= Enum.UserInputType.MouseButton1 then return end
    
    if tick() - Settings.silentAimCooldown < 0.25 then return end
    
    local char = LocalPlayer.Character
    if not char then return end
    
    local gun = char:FindFirstChild('Gun')
    if not gun or not gun:IsA('Tool') then return end
    
    local role = GetRole(LocalPlayer)
    if role ~= 'Sheriff' and role ~= 'Hero' then return end
    if IsDead(LocalPlayer) then return end
    
    gun:Activate()
end)

-- Round Timer
task.spawn(function()
    while true do
        task.wait(1)
        if Settings.ShowRoundTimer then
            local timer = Workspace:FindFirstChild('RoundTimerPart')
            if timer then
                local time = timer:GetAttribute('Time')
                if type(time) == 'number' then
                    print('Round Time:', SecondsToMinutes(time))
                end
            end
        end
    end
end)

-- Webhook
task.spawn(function()
    while true do
        task.wait(1)
        if Settings.WebhookEnabled and Settings.WebhookURL ~= '' then
            if tick() - Settings.WebhookLastSent >= Settings.WebhookInterval then
                Settings.WebhookLastSent = tick()
                -- Отправка вебхука (упрощенная версия)
                pcall(function()
                    local data = {
                        username = 'RuzHub Farm',
                        content = string.format('Coins collected: %d | Time: %s', 
                            Settings.AutoFarmSessionCoinsCollected or 0,
                            SecondsToMinutes(tick() - Settings.AutoFarmSessionStartTime)
                        )
                    }
                    request({
                        Url = Settings.WebhookURL,
                        Method = 'POST',
                        Headers = { ['Content-Type'] = 'application/json' },
                        Body = HttpService:JSONEncode(data)
                    })
                end)
            end
        end
    end
end)

-- Auto Rejoin
game:BindToClose(function()
    if Settings.AutoRejoinEnabled then
        pcall(function()
            TeleportService:TeleportToPlaceInstance(game.PlaceId, game.JobId, LocalPlayer)
        end)
    end
end)

-- Gun Drop Detection
Workspace.DescendantAdded:Connect(function(desc)
    if desc.Name == 'GunDrop' then
        task.wait(0.1)
        RegisterGunDrop(desc)
    end
    if desc.Name == 'CoinContainer' and (desc:IsA('Folder') or desc:IsA('Model')) then
        RegisterCoinContainer(desc)
    end
end)

-- Character Added
LocalPlayer.CharacterAdded:Connect(function(character)
    if Settings.isFlinging then CleanupFling() end
    task.wait(0.5)
    if character then
        local humanoid = character:FindFirstChildOfClass('Humanoid')
        if humanoid then
            CurrentCamera.CameraSubject = humanoid
            SetupSpeedGlitch(character)
            ApplyMovement()
        end
        if Settings.FEAnimEnabled then
            task.spawn(function() ApplyFEAnims(character) end)
        end
    end
    if Settings.Invisible then
        Settings.Invisible = false
    end
end)

-- Player Added
Players.PlayerAdded:Connect(function(player)
    CreateESP(player)
end)

-- Player Removed
Players.PlayerRemoving:Connect(function(player)
    RemoveESP(player)
    RemoveHighlight(player)
    if player == Settings.flingTarget then
        CleanupFling()
    end
end)

-- ================================================================
-- УВЕДОМЛЕНИЕ О ЗАГРУЗКЕ
-- ================================================================
WindUI:Notify({
    Title = 'RuzHub + Toolbox',
    Content = 'Full merge v1.0 loaded successfully!',
    Duration = 3,
    Icon = 'sparkles',
})

print('[RuzHub + Toolbox] Full merge v1.0 loaded.')
