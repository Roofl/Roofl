local AlertWebhook = "https://discord.com/api/webhooks/918775328137162763/WZGZasrxHtxDMlksRFENNt9_kX79zTkvIxbU_IJvTU7ln_NHhjkMg27kSheeBWLViQcK";
while wait(5) do
    local isMerchantHere = game:GetService("Workspace")["__THINGS"]["__REMOTES"]["is merchant here"]:InvokeServer({})[1];
    
    if (isMerchantHere and not shared.Mutex) then
        shared.Mutex = true;
        
        local stockStuff = "";
        for i, v in next, game.Players.LocalPlayer.PlayerGui.Merchant.Frame.Container:GetChildren() do
            local itemId = tostring(v.Item:FindFirstChildOfClass("TextButton").PetIcon.Image):gsub("rbxassetid://", "");
            stockStuff = stockStuff .. "https://www.roblox.com/Game/Tools/ThumbnailAsset.ashx?fmt=png&wd=110&ht=110&aid=" .. itemId .. "\n";
        end;
        stockStuff = stockStuff .. "\n\n\`\`\`";
    
        for i, v in next, game.Players.LocalPlayer.PlayerGui.Merchant.Frame.Container:GetChildren() do
            stockStuff = stockStuff .. string.format("Item: %s; Power: %s; Cost: %s; In-Stock: %s\n", v.Item:FindFirstChildOfClass("TextButton").Name, v.Item:FindFirstChildOfClass("TextButton").Level.Text, v.CostFrame.Cost.Text, tostring(not v.Locked.Visible));   
        end;
        
        stockStuff = stockStuff .. "`\`\`";
        
        syn.request({
            Url = AlertWebhook;
            Method = "POST";
            Headers = {
                ["content-type"] = "application/json"
            }; 
            Body = game:GetService("HttpService"):JSONEncode({
                ["content"] = string.format("@everyone\n\n**Merchant Alert!**\n\n%s\n\n\`\`\`lua\ngame:GetService'TeleportService':TeleportToPlaceInstance(6284583030, '%s')\`\`\`", stockStuff, game.JobId);
            })
        })
    end;
    
    if (not isMerchantHere and shared.Mutex) then
        shared.Mutex = false; 
    end;
end;
