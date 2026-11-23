local Players = game:GetService("Players")
local player = Players.LocalPlayer

local gui = Instance.new("ScreenGui")
gui.Name = "MoedaEventGUI"
gui.ResetOnSpawn = false

local frame = Instance.new("Frame")
frame.Size = UDim2.new(0, 220, 0, 110)
frame.Position = UDim2.new(0.5, -110, 0.5, -55)
frame.BackgroundColor3 = Color3.fromRGB(30, 30, 30)
frame.Active = true
frame.Draggable = true
frame.Parent = gui

local title = Instance.new("TextLabel")
title.Size = UDim2.new(1, 0, 0, 35)
title.BackgroundColor3 = Color3.fromRGB(20, 20, 20)
title.Text = "Mania Hub v1"
title.TextColor3 = Color3.new(1, 1, 1)
title.TextSize = 18
title.Parent = frame

local button = Instance.new("TextButton")
button.Size = UDim2.new(0.8, 0, 0, 40)
button.Position = UDim2.new(0.1, 0, 0, 55)
button.BackgroundColor3 = Color3.fromRGB(0, 170, 80)
button.Text = "ATIVAR"
button.TextColor3 = Color3.new(1, 1, 1)
button.TextSize = 16
button.Parent = frame

local ativo = false

button.MouseButton1Click:Connect(function()
    ativo = not ativo

    if ativo then
        button.Text = "DESATIVAR"
        button.BackgroundColor3 = Color3.fromRGB(200, 50, 50)
    else
        button.Text = "ATIVAR"
        button.BackgroundColor3 = Color3.fromRGB(0, 170, 80)
    end
end)

gui.Parent = player:WaitForChild("PlayerGui")

task.spawn(function()
    while true do
        if ativo then
            local character = player.Character
            local hrp = character and character:FindFirstChild("HumanoidRootPart")

            if hrp then
                for _, obj in ipairs(workspace:GetDescendants()) do
                    if not ativo then break end

                    if obj.Name == "Moeda Event" then
                        local destino

                        if obj:IsA("BasePart") then
                            destino = obj
                        elseif obj:IsA("Model") then
                            destino = obj.PrimaryPart or obj:FindFirstChildWhichIsA("BasePart", true)
                        end

                        if destino then
                            hrp.CFrame = destino.CFrame + Vector3.new(0, 3, 0)
                            task.wait(1)
                        end
                    end
                end
            end
        end

        task.wait(0.1)
    end
end)
