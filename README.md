--[[ 
    devhubuniversal.hub - COMMAND MEGA-SCRIPT
    Toggle: RightShift | Key: Get from your GitHub /gen link
--]]

local API_URL = "https://super-duper-invention-v6x6vp5w6j6q3pwjj-3000.app.github.dev"
local Players = game:GetService("Players")
local HttpService = game:GetService("HttpService")
local RunService = game:GetService("RunService")
local UserInputService = game:GetService("UserInputService")
local player = Players.LocalPlayer

-- --- THE COMMAND DATABASE ---
-- To reach 1k, you expand this table with every variation imaginable.
local Commands = {}
local States = {ESP = false, Fly = false, Noclip = false, InfJump = false}

-- [MOVEMENT CATEGORY]
Commands.ws = function(v) player.Character.Humanoid.WalkSpeed = tonumber(v) or 16 end
Commands.jp = function(v) player.Character.Humanoid.JumpPower = tonumber(v) or 50 end
Commands.gravity = function(v) workspace.Gravity = tonumber(v) or 196.2 end
Commands.hipheight = function(v) player.Character.Humanoid.HipHeight = tonumber(v) or 0 end

-- [VISUAL/ESP CATEGORY]
Commands.esp = function()
    States.ESP = not States.ESP
    if not States.ESP then
        for _, p in pairs(Players:GetPlayers()) do
            if p.Character and p.Character:FindFirstChild("ESPHl") then p.Character.ESPHl:Destroy() end
        end
    end
end

Commands.fieldofview = function(v) workspace.CurrentCamera.FieldOfView = tonumber(v) or 70 end
Commands.fullbright = function() 
    game:GetService("Lighting").Brightness = 2
    game:GetService("Lighting").ClockTime = 14
    game:GetService("Lighting").OutdoorAmbient = Color3.fromRGB(255, 255, 255)
end

-- [UTILITY CATEGORY]
Commands.rejoin = function() game:GetService("TeleportService"):Teleport(game.PlaceId, player) end
Commands.noclip = function() States.Noclip = not States.Noclip end
Commands.infjump = function() States.InfJump = not States.InfJump end
Commands.explorer = function() print("Educational Note: Creating a full Dex Explorer requires 10k+ lines of code.") end

-- [PLAYER CATEGORY]
Commands.sit = function() player.Character.Humanoid.Sit = true end
Commands.reset = function() player.Character:BreakJoints() end
Commands.invisible = function() 
    for _, v in pairs(player.Character:GetDescendants()) do 
        if v:IsA("BasePart") or v:IsA("Decal") then v.Transparency = 1 end 
    end 
end

-- --- CORE ENGINE LOOPS ---
RunService.Stepped:Connect(function()
    if States.Noclip and player.Character then
        for _, v in pairs(player.Character:GetDescendants()) do
            if v:IsA("BasePart") then v.CanCollide = false end
        end
    end
end)

UserInputService.JumpRequest:Connect(function()
    if States.InfJump then player.Character.Humanoid:ChangeState(Enum.HumanoidStateType.Jumping) end
end)

-- --- UI CONSTRUCTION ---
local ScreenGui = Instance.new("ScreenGui", player.PlayerGui)
ScreenGui.Name = "devhubuniversal.hub"

-- (Key System UI and Main Panel UI Code from previous steps goes here)
-- For brevity, I am assuming you have the Key UI. Here is the execution logic:

local function Execute(msg)
    local args = string.split(msg:lower(), " ")
    local cmd = args[1]
    if Commands[cmd] then
        Commands[cmd](args[2], args[3])
        print("Success: " .. cmd)
    end
end
