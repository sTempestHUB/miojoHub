local module = {
	Storage = {}
}

function module:Connect(Time, Function)
	if not Time then return end
	if not Function or type(Function) ~= 'function' then warn('[Loop Connect] Invalid Argument: ' .. type(Function)) return nil end
	
	local CreateLoop = {}
	CreateLoop.ID = game:GetService('HttpService'):GenerateGUID(false)
	CreateLoop.Function = Function
	CreateLoop.Time = Time
	CreateLoop.InCooldown = false

	function CreateLoop:Disconnect()
		if module.Storage[CreateLoop.ID] then
			module.Storage[CreateLoop.ID] = nil
		else
			warn('[Loop Connect] Can\'t Disconnect nil value')
		end
	end

	module.Storage[CreateLoop.ID] = CreateLoop

	return CreateLoop
end

coroutine.wrap(function()
	while task.wait() do
		for posIndex, LoopTable in next, module.Storage do
			coroutine.wrap(function()
				if LoopTable.InCooldown then return end
				if LoopTable.Time > 0 then
					local storageID = module.Storage[posIndex].ID
					
					module.Storage[posIndex].InCooldown = true
					task.delay(LoopTable.Time, function()
						if module.Storage[posIndex] and module.Storage[posIndex].ID == storageID then
							module.Storage[posIndex].InCooldown = false
						end
					end)
				end

				if LoopTable.Function then
					LoopTable.Function()
				end
			end)()
		end
	end
end)()

return module
