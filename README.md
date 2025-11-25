local Players = game:GetService("Players")
local player = Players.LocalPlayer

local char = player.Character or player.CharacterAdded:Wait()
local hrp = char:WaitForChild("HumanoidRootPart")

-- Criar hitbox do reach
local hitbox = Instance.new("Part")
hitbox.Name = "ReachHitbox"
hitbox.Size = Vector3.new(4, 4, 4) -- REACH 4
hitbox.Transparency = 1
hitbox.CanCollide = false
hitbox.Anchored = false
hitbox.Parent = char

-- Soldar no HRP
local weld = Instance.new("WeldConstraint")
weld.Part0 = hitbox
weld.Part1 = hrp
weld.Parent = hitbox
hitbox.CFrame = hrp.CFrame

-- Função para saber se o objeto tocado é uma bola redonda
local function isBall(part)
	if not part:IsA("BasePart") then
		return false
	end

	-- Se for esférica (igual nos comandos :ball, :soccerball etc)
	local size = part.Size
	local sphereShape = (size.X == size.Y) and (size.Y == size.Z)

	-- Se tiver Mesh de bola
	local hasSphereMesh = false
	for _, obj in ipairs(part:GetChildren()) do
		if obj:IsA("SpecialMesh") and obj.MeshType == Enum.MeshType.Sphere then
			hasSphereMesh = true
		end
	end

	return sphereShape or hasSphereMesh
end

-- Quando a hitbox encostar na bola
hitbox.Touched:Connect(function(hit)
	if isBall(hit) then
		local bv = Instance.new("BodyVelocity")
		bv.Velocity = hrp.CFrame.LookVector * 70 -- força (pode aumentar)
		bv.MaxForce = Vector3.new(1e5, 1e5, 1e5)
		bv.Parent = hit
		game.Debris:AddItem(bv, 0.15)
	end
end)
# Mr25
Mr25
