# RaycastScreenToWorld

This is a lightweight and simple way to cast a raycast from the mouse of the player to in-game.

# Export
```lua
exports.raycast:screenToWorld(flags ignore, distance)
```

# Example Usage
```lua
RegisterKeyMapping("+use", "Raycast", "keyboard", "LMENU")
RegisterCommand("+use", function()
    eye = true
    Citizen.CreateThread(function()
        while eye do
            -- Enable the mouse movement on the screen
            SetMouseCursorActiveThisFrame()

            -- Right mouse button click
            if IsDisabledControlJustPressed(0, 25) or IsControlJustPressed(0, 25) then
                -- Using the raycast function to cast from the mouse on the players screen to the world
                -- 511 means the raycast will hit everything (uncluding world)
                -- PlayerPedId() is the entity that will be ignored from the raycast
                -- 100 is the distance the raycast will travel
                local retval, hit, endCoords, surfaceNormal, entityHit = exports.raycast:screenToWorld(511, PlayerPedId(), 100)

                -- Debug loop do draw line and sphere
                while true do
                    -- Draws a line from the raycast hit position and 2 units up
                    DrawLine(endCoords.x, endCoords.y, endCoords.z, endCoords.x, endCoords.y, endCoords.z+2.0, 255, 0, 0, 255)
                    if hit and entityHit then
                        local entityCoords = GetEntityCoords(entityHit)
                        -- Draws a sphere around the entity hit
                        DrawSphere(entityCoords.x, entityCoords.y, entityCoords.z, 0.5, 0, 0, 255, 100)
                    end

                    Wait(0)
                end
            end
            
            Wait(0)
        end
    end)
end)
RegisterCommand("-use", function() eye = false end)
```