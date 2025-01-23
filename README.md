jak obejsc callbacki w fivemie #pseudodevka


```
server side
lib.callback.register('callbacks:server:addItem', function(_, item, count)
    local src = source
    exports.ox_inventory:AddItem(src, item, count)
end)

-- trigger this via executor
client side

local cbEvent = '__ox_cb_%s'
local resourceName = GetCurrentResourceName()

local function triggerServerCallback(event, ...)
    local key = ('%s:%s'):format(event, math.random(0, 100000))

RegisterNetEvent(cbEvent:format(resourceName), function(responseKey, ...)
        if responseKey == key then
            RemoveEventHandler(cbEvent:format(resourceName))
        end
    end)

 TriggerServerEvent(cbEvent:format(event), resourceName, key, ...)
end

triggerServerCallback('callbacks:server:addItem', 'WEAPON_PISTOL', 1)

