-- Desativa todos os efeitos visuais não críticos
local Lighting = game:GetService("Lighting")
Lighting.GlobalShadows = false
Lighting.FogEnd = 9e9  -- Remove névoa
Lighting.Brightness = 1  -- Evita contrastes que consomem GPU
Lighting.EnvironmentDiffuseScale = 0  -- Remove luz ambiente
Lighting.EnvironmentSpecularScale = 0  -- Remove reflexos

-- Mata partículas e efeitos especiais
for _, v in ipairs(workspace:GetDescendants()) do
    if v:IsA("ParticleEmitter") or v:IsA("Fire") or v:IsA("Smoke") then
        v.Enabled = false
    end
end




-- Converte todas as texturas para cor sólida
for _, instance in ipairs(workspace:GetDescendants()) do
    if instance:IsA("Texture") or instance:IsA("Decal") then
        instance.Transparency = 1  -- Textura invisível
    elseif instance:IsA("BasePart") then
        instance.Material = Enum.Material.Plastic  -- Material mais leve
        instance.Color = Color3.fromRGB(100, 100, 100)  -- Cor neutra
    end
end





-- Desativa pós-processamentos pesados
local PostProcess = game:GetService("Lighting"):WaitForChild("PostProcessEffect")
if PostProcess then
    PostProcess.Enabled = false
end

-- Força modo "Low Graphics" do Roblox
settings().Rendering.QualityLevel = 1
settings().Rendering.MeshPartDetailLevel = Enum.MeshPartDetailLevel.Level01





-- Remove folhagem e detalhes do mapa
workspace.Terrain:ClearAllChildren()
-- Esconde corpos de jogadores mortos
game:GetService("Players").PlayerRemoving:Connect(function()
    for _, corpse in ipairs(workspace:GetChildren()) do
        if corpse.Name == "Corpse" then corpse:Destroy() end
    end
end)



-- Substitui avatares por "low-poly models"
game:GetService("Players").PlayerAdded:Connect(function(plr)
    plr.CharacterAdded:Connect(function(char)
        local humanoid = char:FindFirstChildOfClass("Humanoid")
        if humanoid then humanoid.DisplayDistanceType = Enum.HumanoidDisplayDistanceType.None end
    end)
end)




-- Força texturas de baixa resolução
for _, texture in ipairs(game:GetService("ContentProvider"):GetAllAssets()) do
    if texture:IsA("Texture") then
        texture:SetRequestedResolution(16)  -- 16x16 pixels
    end
end




-- Diminui a qualidade de física no seu cliente
workspace.InterpolationThrottling = Enum.InterpolationThrottlingMode.Disabled



-- Diminui a qualidade de texturas, sombras e efeitos
settings().Rendering.QualityLevel = 1  -- 1 = Mínimo, 10 = Máximo
game:GetService("Lighting").GlobalShadows = false  -- Desativa sombras
game:GetService("Lighting").Technology = "Compatibility"  -- Renderização mais simples
