print("🔍 Monitor de portão iniciado...")

-- 1. Monitorar objetos que entram no Workspace
workspace.ChildAdded:Connect(function(child)
    print("🟢 Novo objeto no Workspace:", child.Name, child:GetFullName())

    if child:IsA("BasePart") then
        monitorPart(child)
    elseif child:IsA("Model") then
        for _, part in pairs(child:GetDescendants()) do
            if part:IsA("BasePart") then
                monitorPart(part)
            end
        end
    end
end)

-- 2. Monitorar remoções (caso o portão seja destruído após 60s)
workspace.ChildRemoved:Connect(function(child)
    print("🔴 Objeto REMOVIDO do Workspace:", child.Name)
end)

-- 3. Monitorar mudanças em partes
function monitorPart(part)
    print("👀 Observando parte:", part.Name, "em", part:GetFullName())
    
    part:GetPropertyChangedSignal("Transparency"):Connect(function()
        print("⚠️", part.Name, "mudou Transparency para", part.Transparency)
    end)
    part:GetPropertyChangedSignal("CanCollide"):Connect(function()
        print("⚠️", part.Name, "mudou CanCollide para", part.CanCollide)
    end)
    part:GetPropertyChangedSignal("Position"):Connect(function()
        print("📍", part.Name, "mudou posição para", part.Position)
    end)
end

-- 4. Monitorar todas as partes já existentes (caso o portão esteja invisível e fique visível)
for _, part in pairs(workspace:GetDescendants()) do
    if part:IsA("BasePart") then
        monitorPart(part)
    end
end

print("✅ Script de monitoramento pronto. Agora ative o bloqueio no jogo.")
