
require "import"
import "android.widget."
import "android.view."

-- =======================================================
--  PRO LUA LEARNING SANDBOX
--  Features simulated: Fly, Noclip, Speed, Hitbox, Platform
--  All outputs go to console, safe for LuaDroid
-- =======================================================

-- ==========================
-- STATE VARIABLES
-- ==========================
local state = {
flying = false,
noclip = false,
speedEnabled = false,
speed = 16,
hitboxEnabled = false,
hitboxSize = 2,
platformEnabled = false,
platform = nil
}

-- ==========================
-- UTILITY FUNCTIONS
-- ==========================
local function logStatus()
print("\n=== Current Status ===")
print("Flying: " .. tostring(state.flying))
print("Noclip: " .. tostring(state.noclip))
print("Speed: " .. tostring(state.speed))
print("Speed Enabled: " .. tostring(state.speedEnabled))
print("Hitbox Size: " .. tostring(state.hitboxSize))
print("Hitbox Enabled: " .. tostring(state.hitboxEnabled))
print("Platform: " .. (state.platform and "Active, Size="..state.platform.size or "None"))
print("====================\n")
end

-- ==========================
-- FEATURE FUNCTIONS
-- ==========================

-- Fly Toggle
local function toggleFly()
state.flying = not state.flying
print(state.flying and "Flying ON" or "Flying OFF")
logStatus()
end

-- Noclip Toggle
local function toggleNoclip()
state.noclip = not state.noclip
print(state.noclip and "Noclip ON" or "Noclip OFF")
logStatus()
end

-- Speed Toggle (100B default)
local function toggleSpeed(custom)
state.speedEnabled = not state.speedEnabled
if state.speedEnabled then
state.speed = custom or 100000000000 -- 100B default
print("Speed Enabled: " .. state.speed)
else
state.speed = 16
print("Speed Disabled (reset to 16)")
end
logStatus()
end

-- Hitbox Toggle
local function toggleHitbox(custom)
state.hitboxEnabled = not state.hitboxEnabled
if state.hitboxEnabled then
state.hitboxSize = custom or state.hitboxSize
print("Hitbox Enabled: " .. state.hitboxSize)
else
state.hitboxSize = 2
print("Hitbox Disabled (reset to 2)")
end
logStatus()
end

-- Platform Toggle
local function togglePlatform(custom)
state.platformEnabled = not state.platformEnabled
if state.platformEnabled then
state.platform = {size = custom or 10}
print("Platform created at your position, size: " .. state.platform.size)
else
state.platform = nil
print("Platform removed")
end
logStatus()
end

-- ==========================
-- UI LAYOUT FUNCTION
-- ==========================
local function createUI()
local layout = {
ScrollView,
{
LinearLayout,
orientation = "vertical",
padding = "16dp",

-- Title  
        { TextView, text="Lua Learning Sandbox", textSize="20sp", textColor=0xFF00FF00, gravity=Gravity.CENTER },  

        -- Fly Toggle  
        { Button, text = "Toggle Fly", onClick = toggleFly },  

        -- Noclip Toggle  
        { Button, text = "Toggle Noclip", onClick = toggleNoclip },  

        -- Speed Section  
        { TextView, text="Speed Controls", textSize="16sp", textColor=0xFFFFFF00 },  
        { Button, text="Toggle Speed (100B Default)", onClick=function() toggleSpeed() end },  
        { EditText, hint="Enter Custom Speed", id="speedInput" },  
        { Button, text="Toggle Custom Speed", onClick=function()  
            local s = tonumber(speedInput.getText().toString())  
            if s then toggleSpeed(s) else print("Invalid speed input") end  
        end },  

        -- Hitbox Section  
        { TextView, text="Hitbox Controls", textSize="16sp", textColor=0xFFFFFF00 },  
        { Button, text="Toggle Hitbox", onClick=function() toggleHitbox() end },  
        { EditText, hint="Enter Hitbox Size", id="hitboxInput" },  
        { Button, text="Toggle Custom Hitbox", onClick=function()  
            local s = tonumber(hitboxInput.getText().toString())  
            if s then toggleHitbox(s) else print("Invalid hitbox size") end  
        end },  

        -- Platform Section  
        { TextView, text="Platform Controls", textSize="16sp", textColor=0xFFFFFF00 },  
        { EditText, hint="Enter Platform Size", id="platformInput" },  
        { Button, text="Toggle Platform", onClick=function()  
            local s = tonumber(platformInput.getText().toString())  
            if s then togglePlatform(s) else togglePlatform() end  
        end }  
    }  
}  

activity.setContentView(loadlayout(layout))

end

-- ==========================
-- MAIN
-- ==========================
createUI()
print("Lua Learning Sandbox Loaded ✅")
logStatus()

